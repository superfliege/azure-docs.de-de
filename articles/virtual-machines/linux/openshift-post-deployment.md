---
title: OpenShift in Azure-Aufgaben nach der Bereitstellung | Microsoft-Dokumentation
description: Zusätzliche Aufgaben nach der Bereitstellung eines OpenShift-Clusters.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 7b129eea513b7856ca99b02842b3b9c33c6ec19b
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50084984"
---
# <a name="post-deployment-tasks"></a>Aufgaben nach der Bereitstellung

Nach der Bereitstellung eines OpenShift-Clusters können Sie zusätzliche Elemente konfigurieren. In diesem Artikel wird Folgendes behandelt:

- Konfigurieren des einmaligen Anmeldens mit Azure Active Directory (Azure AD)
- Konfigurieren von Log Analytics zum Überwachen von OpenShift
- Konfigurieren von Metriken und Protokollierung
- Installieren von Open Service Broker für Azure (OSBA)

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Konfigurieren des einmaligen Anmeldens mit Azure Active Directory

Um Azure Active Directory für die Authentifizierung zu verwenden, müssen Sie zuerst eine Azure AD-App-Registrierung erstellen. Dieser Prozess umfasst zwei Schritte: das Erstellen der App-Registrierung und das Konfigurieren von Berechtigungen.

### <a name="create-an-app-registration"></a>Erstellen einer App-Registrierung

Für diese Schritte wird die Azure-Befehlszeilenschnittstelle verwendet, um die App-Registrierung zu erstellen, und die grafische Benutzeroberfläche (das Portal), um die Berechtigungen festzulegen. Für die Erstellung der App-Registrierung werden die folgenden fünf Angaben benötigt:

- Anzeigename: Der Name der App-Registrierung (z. B. OCPAzureAD)
- Startseite: OpenShift-Konsolen-URL (z.B. https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Bezeichner-URI: OpenShift-Konsolen-URL (z.B. https://masterdns343khhde.westus.cloudapp.azure.com/console)
- Antwort-URL: Öffentliche Master-URL und Name der App-Registrierung (z.B. https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Kennwort: Ein sicheres Kennwort

Im folgenden Beispiel wird eine App-Registrierung mit den obigen Informationen erstellt:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com/console --password {Strong Password}
```

Nach erfolgreicher Befehlsausführung erhalten Sie eine JSON-Ausgabe wie die folgende:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Notieren Sie sich die zurückgegebene appId-Eigenschaft zur späteren Verwendung.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1.  Klicken Sie auf **Azure Active Directory** > **App-Registrierung**.
2.  Suchen Sie nach Ihrer App-Registrierung. (z. B. OCPAzureAD).
3.  Klicken Sie in den Ergebnissen auf die App-Registrierung.
4.  Klicken Sie unter **Einstellungen** auf **Erforderliche Berechtigungen**.
5.  Wählen Sie unter **Erforderliche Berechtigungen** die Option **Hinzufügen** aus.

  ![App-Registrierung](media/openshift-post-deployment/app-registration.png)

6.  Klicken Sie auf „Schritt 1: API auswählen“ und dann auf **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Klicken Sie im unteren Bereich auf **Auswählen**.

  ![App-Registrierung: API auswählen](media/openshift-post-deployment/app-registration-select-api.png)

7.  Wählen Sie in „Schritt 2 Berechtigungen auswählen“ unter **Delegierte Berechtigungen** die Berechtigung **Anmelden und Benutzerprofil lesen** aus, und klicken Sie anschließend auf **Auswählen**.

  ![App-Registrierung: Zugriff](media/openshift-post-deployment/app-registration-access.png)

8.  Wählen Sie **Fertig**aus.

### <a name="configure-openshift-for-azure-ad-authentication"></a>Konfigurieren von OpenShift für die Azure AD-Authentifizierung

Wenn Sie OpenShift für die Verwendung von Azure AD als Authentifizierungsanbieter konfigurieren möchten, muss die Datei „/etc/origin/master/master-config.yaml“ auf allen Masterknoten bearbeitet werden.

Die Mandanten-ID können Sie mit dem folgenden CLI-Befehl ermitteln:

```azurecli
az account show
```

Suchen Sie in der YAML-Datei nach folgenden Zeilen:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Fügen Sie unmittelbar nach den obigen Zeilen die folgenden Zeilen ein:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Stellen Sie sicher, dass der Text unter „identityProviders“ korrekt ausgerichtet ist. Die Mandanten-ID können Sie mit dem folgenden CLI-Befehl ermitteln: ```az account show```

Starten Sie die OpenShift-Masterdienste auf allen Masterknoten neu:

**OpenShift Container Platform (OCP) mit mehreren Mastern**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform mit einzelnem Master**

```bash
sudo systemctl restart atomic-openshift-master
```

**OKD mit mehreren Mastern**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OKD mit einzelnem Master**

```bash
sudo systemctl restart origin-master
```

In der OpenShift-Konsole werden nun zwei Authentifizierungsoptionen angezeigt: „htpasswd_auth“ und [App-Registrierung].

## <a name="monitor-openshift-with-log-analytics"></a>Überwachen von OpenShift mit Log Analytics

Zum Hinzufügen des Log Analytics-Agents zu OpenShift stehen drei Möglichkeiten zur Verfügung.
- Installieren des Log Analytics-Agents für Linux direkt auf jedem OpenShift-Knoten
- Aktivieren der Log Analytics-VM-Erweiterung auf jedem OpenShift-Knoten
- Installieren des Log Analytics-Agents als OpenShift-Daemongruppe

Die vollständigen Anweisungen finden Sie hier: https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-containers#configure-a-log-analytics-agent-for-red-hat-openshift.

## <a name="configure-metrics-and-logging"></a>Konfigurieren von Metriken und Protokollierung

Basierend auf dem Branch können die Azure Resource Manager-Vorlagen für OpenShift Container Platform und OKD als Teil der Installation Eingabeparameter für die Aktivierung von Metriken und die Protokollierung enthalten.

Das Marketplace-Angebot für OpenShift Container Platform umfasst zudem eine Option für die Aktivierung von Metriken und der Protokollierung während der Clusterinstallation.

Wenn Metriken und die Protokollierung während der Installation des Clusters nicht aktiviert wurden, können sie später auf einfache Weise aktiviert werden.

### <a name="ansible-inventory-pre-work"></a>Vorbereitungsschritte für die Ansible-Inventurdatei

Überprüfen Sie, ob die Ansible-Inventurdatei („/etc/ansible/hosts“) die entsprechenden Variablen für Metriken und Protokollierung enthält. Die Inventurdatei befindet sich je nach verwendeter Vorlage auf unterschiedlichen Hosts.

Für die OpenShift Container-Vorlage und das Marketplace-Angebot befindet sich die Inventurdatei auf dem Bastion-Host. Für die OKD-Vorlage befindet sich die Inventurdatei abhängig vom verwendeten Branch auf dem Master-0-Host oder dem Bastion-Host.

1. Bearbeiten Sie die Datei „/etc/ansible/hosts“, und fügen Sie nach dem Identitätsanbieterabschnitt („# Enable HTPasswdPasswordIdentityProvider“) die folgenden Zeilen hinzu. Wenn diese Zeilen bereits vorhanden sind, fügen Sie sie nicht erneut hinzu.

   OpenShift- und OKD-Versionen 3.9 und früher

   ```yaml
   # Setup metrics
   openshift_hosted_metrics_deploy=false
   openshift_metrics_cassandra_storage_type=dynamic
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"type":"infra"}
   openshift_metrics_cassandra_nodeselector={"type":"infra"}
   openshift_metrics_heapster_nodeselector={"type":"infra"}
   openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

   # Setup logging
   openshift_hosted_logging_deploy=false
   openshift_hosted_logging_storage_kind=dynamic
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"type":"infra"}
   openshift_logging_kibana_nodeselector={"type":"infra"}
   openshift_logging_curator_nodeselector={"type":"infra"}
   openshift_master_logging_public_url=https://kibana.$ROUTING
   ```

   OpenShift- und OKD-Versionen 3.10 und höher

   ```yaml
   # Setup metrics
   openshift_metrics_install_metrics=false
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_cassandra_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_metrics_heapster_nodeselector={"node-role.kubernetes.io/infra":"true"}

   # Setup logging
   openshift_logging_install_logging=false
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_kibana_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_curator_nodeselector={"node-role.kubernetes.io/infra":"true"}
   openshift_logging_master_public_url=https://kibana.$ROUTING
   ```

3. Ersetzen Sie „$ROUTING“ in der gleichen Datei („/etc/ansible/hosts“) durch die Zeichenfolge für die Option „openshift_master_default_subdomain“.

### <a name="azure-cloud-provider-in-use"></a>Mit Azure-Cloudanbieter

Stellen Sie eine SSH-Verbindung mit dem Bastion-Knoten oder dem ersten Masterknoten her (basierend auf der verwendeten Vorlage und dem Branch), und verwenden Sie dazu die während der Bereitstellung angegebenen Anmeldeinformationen. Geben Sie den folgenden Befehl ein:

**OpenShift Container Platform 3.7 und früher**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OpenShift Container Platform 3.9 und höher**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

**OKD 3.7 und früher**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

**OKD 3.9 und höher**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

### <a name="azure-cloud-provider-not-in-use"></a>Ohne Azure-Cloudanbieter

Stellen Sie eine SSH-Verbindung mit dem Bastion-Knoten oder dem ersten Masterknoten her (basierend auf der verwendeten Vorlage und dem Branch), und verwenden Sie dazu die während der Bereitstellung angegebenen Anmeldeinformationen. Geben Sie den folgenden Befehl ein:


**OpenShift Container Platform 3.7 und früher**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OpenShift Container Platform 3.9 und höher**

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

**OKD 3.7 und früher**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True
```

**OKD 3.9 und höher**

```bash
ansible-playbook ~/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True
ansible-playbook ~/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

## <a name="install-open-service-broker-for-azure-osba"></a>Installieren von Open Service Broker für Azure (OSBA)

Mit Open Service Broker für Azure (OSBA) können Sie Azure Cloud Services direkt über OpenShift bereitstellen. OSBA ist eine Open Service Broker-API-Implementierung für Azure. Die Open Service Broker-API ist eine Spezifikation einer gemeinsamen Sprache für Cloudanbieter, die cloudbasierte Anwendungen zur Verwaltung von Clouddiensten unabhängig verwenden können.

Anweisungen zum Installieren von OSBA auf OpenShift finden Sie hier: https://github.com/Azure/open-service-broker-azure#openshift-project-template. 

## <a name="next-steps"></a>Nächste Schritte

- [Getting Started with OpenShift Container Platform](https://docs.openshift.com/container-platform) (Erste Schritte mit OpenShift Container Platform)
- [Erste Schritte mit OKD](https://docs.okd.io/latest)

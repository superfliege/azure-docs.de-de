---
title: OpenShift in Azure-Aufgaben nach der Bereitstellung | Microsoft-Dokumentation
description: "Zusätzliche Aufgaben nach der Bereitstellung eines OpenShift-Clusters."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 77c4719b5cee7f5736d73ee10cf6abf12229ea11
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="post-deployment-tasks"></a>Aufgaben nach der Bereitstellung

Nach der Bereitstellung eines OpenShift-Clusters können Sie zusätzliche Elemente konfigurieren. In diesem Artikel werden die folgenden Aufgaben behandelt:

- Konfigurieren des einmaligen Anmeldens mit Azure Active Directory (Azure AD)
- Konfigurieren von Operations Management Suite zum Überwachen von OpenShift
- Konfigurieren von Metriken und Protokollierung

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Konfigurieren des einmaligen Anmeldens mit Azure Active Directory

Um Azure Active Directory für die Authentifizierung zu verwenden, müssen Sie zuerst eine Azure AD-App-Registrierung erstellen. Dieser Prozess umfasst zwei Schritte: das Erstellen der App-Registrierung und das Konfigurieren von Berechtigungen.

### <a name="create-an-app-registration"></a>Erstellen einer App-Registrierung

Für diese Schritte wird die Azure-Befehlszeilenschnittstelle verwendet, um die App-Registrierung zu erstellen, und die grafische Benutzeroberfläche (das Portal), um die Berechtigungen festzulegen. Für die Erstellung der App-Registrierung werden die folgenden fünf Angaben benötigt:

- Anzeigename: Der Name der App-Registrierung (z. B. OCPAzureAD)
- Homepage: Die URL der OpenShift-Konsole (z. B. „https://masterdns343khhde.westus.cloudapp.azure.com:8443/console“)
- Bezeichner-URI: Die URL der OpenShift-Konsole (z. B. „https://masterdns343khhde.westus.cloudapp.azure.com:8443/console“)
- Antwort-URL: Die öffentliche Master-URL und der Name der App-Registrierung (z. B. „https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD“)
- Kennwort: Ein sicheres Kennwort

Im folgenden Beispiel wird eine App-Registrierung mit den obigen Informationen erstellt:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Nach erfolgreicher Befehlsausführung erhalten Sie eine JSON-Ausgabe wie die folgende:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD"
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
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
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

Die Mandanten-ID können Sie mit dem folgenden CLI-Befehl ermitteln: ```az account show```

Starten Sie die OpenShift-Masterdienste auf allen Masterknoten neu:

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OpenShift Container Platform (OCP) mit mehreren Mastern**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform mit einzelnem Master**

```bash
sudo systemctl restart atomic-openshift-master
```

In der OpenShift-Konsole werden nun zwei Authentifizierungsoptionen angezeigt: „htpasswd_auth“ und [App-Registrierung].

## <a name="monitor-openshift-with-operations-management-suite"></a>Überwachen von OpenShift mit Operations Management Suite

Wenn Sie OpenShift mit Operations Management Suite überwachen möchten, stehen Sie zwei Optionen zur Verfügung: OMS-Agent-Installation auf dem VM-Host oder OMS-Container. In diesem Artikel erfahren Sie, wie Sie den OMS-Container bereitstellen.

## <a name="create-an-openshift-project-for-operations-management-suite-and-set-user-access"></a>Erstellen eines OpenShift-Projekts für Operations Management Suite und Festlegen des Benutzerzugriffs

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-a-daemon-set-yaml-file"></a>Erstellen einer YAML-Datei für den Daemon-Satz

Erstellen Sie eine Datei namens „ocp-omsagent.yml“:

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-a-secret-yaml-file"></a>Erstellen einer YAML-Geheimnisdatei

Für die Erstellung der YAML-Geheimnisdatei benötigen Sie zwei Informationen: die ID des OMS-Arbeitsbereichs und den gemeinsam verwendeten Schlüssel des OMS-Arbeitsbereichs. 

Beispiel für die Datei „ocp-secret.yml“: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Ersetzen Sie „wsid_data“ durch die Base64-codierte OMS-Arbeitsbereichs-ID. Ersetzen Sie dann „key_data“ durch den gemeinsam verwendeten Schlüssel des Base64-codierten OMS-Arbeitsbereichs.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-the-secret-and-daemon-set"></a>Erstellen von Geheimnis und Daemon-Satz

Stellen Sie die Geheimnisdatei bereit:

```bash
oc create -f ocp-secret.yml
```

Stellen Sie den OMS-Agent-Daemon-Satz bereit:

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Konfigurieren von Metriken und Protokollierung

Die Azure Resource Manager-Vorlage für OpenShift Container Platform stellt Eingabeparameter zur Aktivierung von Metriken und Protokollierung bereit. Dies ist beim Marketplace-Angebot „OpenShift Container Platform“ und bei der Ressourcen-Manager-Vorlage „OpenShift Origin“ nicht der Fall.

Falls Sie die OCP-Ressourcen-Manager-Vorlage verwendet haben und Metriken und Protokollierung zum Installationszeitpunkt nicht aktiviert waren oder wenn Sie das OCP-Marketplace-Angebot verwendet haben, kann die Aktivierung problemlos nachgeholt werden. Bei Verwendung der Ressourcen-Manager-Vorlage „OpenShift Origin“ sind gewisse Vorbereitungsschritte erforderlich.

### <a name="openshift-origin-template-pre-work"></a>Vorbereitungsschritte für die Vorlage „OpenShift Origin“

1. Stellen Sie über Port 2200 eine SSH-Verbindung mit dem ersten Masterknoten her.

   Beispiel:

   ```bash
   ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
   ```

2. Bearbeiten Sie die Datei „/etc/ansible/hosts“, und fügen Sie nach dem Identitätsanbieterabschnitt (# Enable HTPasswdPasswordIdentityProvider) die folgenden Zeilen hinzu:

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

3. Ersetzen Sie „$ROUTING“ in der gleichen Datei („/etc/ansible/hosts“) durch die Zeichenfolge für die Option „openshift_master_default_subdomain“.

### <a name="azure-cloud-provider-in-use"></a>Mit Azure-Cloudanbieter

Stellen Sie auf dem ersten Masterknoten (Origin) oder Bastion-Knoten (OCP) eine SSH-Verbindung her, und verwenden Sie dabei die im Zuge der Bereitstellung angegebenen Anmeldeinformationen. Geben Sie den folgenden Befehl ein:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Ohne Azure-Cloudanbieter

Stellen Sie auf dem ersten Masterknoten (Origin) oder Bastion-Knoten (OCP) eine SSH-Verbindung her, und verwenden Sie dabei die im Zuge der Bereitstellung angegebenen Anmeldeinformationen. Geben Sie den folgenden Befehl ein:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Nächste Schritte

- [Getting Started with OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html) (Erste Schritte mit OpenShift Container Platform)
- [Getting started with OpenShift Origin (Erste Schritte mit OpenShift Origin)](https://docs.openshift.org/latest/getting_started/index.html)

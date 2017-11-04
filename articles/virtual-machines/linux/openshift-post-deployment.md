---
title: OpenShift in Azure-Aufgaben nach der Bereitstellung | Microsoft-Dokumentation
description: "OpenShift – Aufgaben nach der Bereitstellung"
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
ms.openlocfilehash: 12e6785358f5f412326418b0c64eeaeabdaa3b5f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="post-deployment-tasks"></a>Aufgaben nach der Bereitstellung

Nach dem Bereitstellen des OpenShift-Clusters können weitere Elemente konfiguriert werden. In diesem Artikel wird Folgendes behandelt:

- Konfigurieren des einmaligen Anmeldens mit Azure Active Directory (AAD)
- Konfigurieren der OMS zur Überwachung von OpenShift
- Konfigurieren von Metriken und Protokollierung

## <a name="single-sign-on-using-aad"></a>Einmaliges Anmelden mit AAD

Wenn Sie AAD für die Authentifizierung verwenden möchten, müssen Sie zunächst eine Azure AD-App-Registrierung erstellen. Dieser Prozess umfasst zwei Schritte: das Erstellen der App-Registrierung und das anschließende Konfigurieren der Berechtigungen.

### <a name="create-app-registration"></a>Erstellen der App-Registrierung

Wir verwenden die Azure-Befehlszeilenschnittstelle, um die App-Registrierung zu erstellen, und die grafische Benutzeroberfläche (das Portal), um die Berechtigungen festzulegen. Für die Erstellung der App-Registrierung werden fünf Informationen benötigt.

- Anzeigename: Der Name der App-Registrierung (Beispiel: OCPAzureAD)
- Homepage: Die URL der OpenShift-Konsole (Beispiel: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Bezeichner-URI: Die URL der OpenShift-Konsole (Beispiel: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- Antwort-URL: Die öffentliche Master-URL und der Name der App-Registrierung (Beispiel: https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD)
- Kennwort: Ein sicheres Kennwort

Im folgenden Beispiel wird eine App-Registrierung mit den obigen Informationen erstellt.

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

Gehen Sie im **Azure-Portal** wie folgt vor:

1.  Klicken Sie auf **Azure Active Directory** --> **App-Registrierung**.
2.  Suchen Sie nach Ihrer App-Registrierung. (Beispiel: OCPAzureAD)
3.  Klicken Sie in den Ergebnissen auf die App-Registrierung.
4.  Klicken Sie auf dem Blatt „Einstellungen“ auf **Erforderliche Berechtigungen**.
5.  Klicken Sie auf dem Blatt „Erforderliche Berechtigungen“ auf **Hinzufügen**.

  ![App-Registrierung](media/openshift-post-deployment/app-registration.png)

6.  Klicken Sie auf Schritt 1 (API auswählen), auf **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)** und anschließend im unteren Bereich auf **Auswählen**.

  ![App-Registrierung: API auswählen](media/openshift-post-deployment/app-registration-select-api.png)

7.  Wählen Sie in Schritt 2 (Berechtigungen auswählen) unter **Delegierte Berechtigungen** die Berechtigung **Anmelden und Benutzerprofil lesen** aus, und klicken Sie anschließend auf **Auswählen**.

  ![App-Registrierung: Zugriff](media/openshift-post-deployment/app-registration-access.png)

8.  Klicken Sie auf **Fertig**.

### <a name="configure-openshift-for-azure-ad-authentication"></a>Konfigurieren von OpenShift für die Azure AD-Authentifizierung

Wenn Sie OpenShift für die Verwendung von Azure AD als Authentifizierungsanbieter konfigurieren möchten, muss die Datei **/etc/origin/master/master-config.yaml** auf allen Masterknoten bearbeitet werden.

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

Neustarten der OpenShift-Masterdienste auf allen Masterknoten

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OpenShift Container Platform mit mehreren Mastern**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform mit einzelnem Master**

```bash
sudo systemctl restart atomic-openshift-master
```

In der OpenShift-Konsole werden nun zwei Authentifizierungsoptionen angezeigt: „htpasswd_auth“ und **[App-Registrierung]**.

## <a name="monitor-openshift-with-oms"></a>Überwachen von OpenShift mit der OMS

Wenn Sie OpenShift mit der OMS überwachen möchten, haben Sie zwei Möglichkeiten: OMS-Agent-Installation auf dem VM-Host oder OMS-Container. In diesem Artikel erfahren Sie, wie Sie den OMS-Container bereitstellen.

## <a name="create-an-openshift-project-for-oms-and-set-user-access"></a>Erstellen eines OpenShift-Projekts für OMS und Festlegen des Benutzerzugriffs

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-daemon-set-yaml-file"></a>Erstellen einer YAML-Datei für den Daemon-Satz

Erstellen Sie eine Datei namens „ocp-omsagent.yml“.

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

## <a name="create-secret-yaml-file"></a>Erstellen einer YAML-Geheimnisdatei

Für die Erstellung der YAML-Geheimnisdatei benötigen Sie zwei Informationen: die ID des OMS-Arbeitsbereichs und den gemeinsam verwendeten Schlüssel des OMS-Arbeitsbereichs. 

Beispiel für die Datei „ocp-secret.yml“ 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Ersetzen Sie „wsid_data“ durch die Base64-codierte ID des OMS-Arbeitsbereichs und „key_data“ durch den Base64-codierten, gemeinsam verwendeten Schlüssel des OMS-Arbeitsbereichs.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-secret-and-daemon-set"></a>Erstellen von Geheimnis und Daemon-Satz

Stellen Sie die Geheimnisdatei bereit.

```bash
oc create -f ocp-secret.yml
```

Stellen Sie den OMS-Agent-Daemon-Satz bereit.

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Konfigurieren von Metriken und Protokollierung

Die Resource Manager-Vorlage „OpenShift Container Plattform (OCP)“ stellt Eingabeparameter zur Aktivierung von Metriken und Protokollierung bereit. Dies ist beim Marketplace-Angebot „OpenShift Container Platform“ und bei der Resource Manager-Vorlage „OpenShift Origin“ nicht der Fall.

Falls die OCP-Resource Manager-Vorlage verwendet wurde und Metriken und Protokollierung zum Installationszeitpunkt nicht aktiviert waren oder das OCP-Marketplace-Angebot verwendet wurde, kann die Aktivierung problemlos nachgeholt werden. Bei Verwendung der Resource Manager-Vorlage „OpenShift Origin“ sind gewisse Vorbereitungsschritte erforderlich.

### <a name="openshift-origin-template-pre-work"></a>Vorbereitungsschritte für die Vorlage „OpenShift Origin“

Stellen Sie über Port 2200 eine SSH-Verbindung mit dem ersten Masterknoten her.

Beispiel

```bash
ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
```

Bearbeiten Sie die Datei **/etc/ansible/hosts**, und fügen Sie nach dem Identitätsanbieterabschnitt (# Enable HTPasswdPasswordIdentityProvider) die folgenden Zeilen hinzu:

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

Ersetzen Sie „$ROUTING“ in der gleichen Datei (**/etc/ansible/hosts**) durch die Zeichenfolge für die Option **openshift_master_default_subdomain**.

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
---
title: "Authentifizieren per Azure Container Registry über den Azure Container Service"
description: "Es wird beschrieben, wie Sie Zugriff auf Images in Ihrer privaten Containerregistrierung über den Azure Container Service gewähren, indem Sie einen Azure Active Directory-Dienstprinzipal verwenden."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/12/2018
ms.author: nepeters
ms.openlocfilehash: d6f6688011ddebe2b486bb6ae00f1f3e095a931d
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-service"></a>Authentifizieren per Azure Container Registry über den Azure Container Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Container Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. In diesem Dokument werden die empfohlenen Konfigurationen für die Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben.

## <a name="grant-aks-access-to-acr"></a>Gewähren von AKS-Zugriff auf ACR

Beim Erstellen eines AKS-Clusters wird auch ein Dienstprinzipal zum Verwalten der Clusteroperabilität mit Azure-Ressourcen erstellt. Dieser Dienstprinzipal kann auch für die Authentifizierung per ACR verwendet werden. Hierfür muss eine Rollenzuweisung erstellt werden, um für den Dienstprinzipal Zugriff auf die ACR-Ressource zu gewähren. 

Sie können das folgende Beispiel für diesen Vorgang nutzen.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Zugreifen per Kubernetes-Geheimnis

In einigen Fällen kann der von AKS verwendete Dienstprinzipal nicht an ACR angepasst werden. Zur Lösung dieses Problems können Sie einen eindeutigen Dienstprinzipal erstellen und speziell an ACR anpassen.

Das folgende Skript kann verwendet werden, um den Dienstprinzipal zu erstellen. 

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id. 
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a contributor role assignment with a scope of the ACR resource. 
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principle client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Die Anmeldeinformationen des Dienstprinzipals können jetzt in einem [Kubernetes-Geheimnis für einen Image-Pullvorgang][image-pull-secret] gespeichert werden, und beim Ausführen von Containern in einem AKS-Cluster kann darauf verwiesen werden. 

Mit dem folgenden Befehl wird das Kubernetes-Geheimnis erstellt. Ersetzen Sie den Servernamen durch den ACR-Anmeldeserver, den Benutzernamen durch die Dienstprinzipal-ID und das Kennwort durch das Dienstprinzipal-Kennwort.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> 
```

Das Kubernetes-Geheimnis kann in einer Podbereitstellung mit dem Parameter `ImagePullSecrets` verwendet werden. 

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets

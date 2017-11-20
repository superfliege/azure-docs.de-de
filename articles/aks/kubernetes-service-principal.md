---
title: "Dienstprinzipal für Azure Kubernetes-Cluster | Microsoft-Dokumentation"
description: "Erstellen und Verwalten eines Azure Active Directory-Dienstprinzipals für einen Kubernetes-Cluster in AKS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6c61d99f1d023ac643455faae10ef284f1f5bb14
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Dienstprinzipale mit Azure Container Service (AKS)

Für die Interaktion mit Azure-APIs benötigt ein AKS-Cluster einen [Azure Active Directory-Dienstprinzipal](../active-directory/develop/active-directory-application-objects.md). Der Dienstprinzipal wird für die dynamische Verwaltung von Ressourcen wie etwa [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md) und [Azure Load Balancer (Layer 4)](../load-balancer/load-balancer-overview.md) benötigt.

Dieser Artikel informiert über verschiedene Optionen zum Einrichten eines Dienstprinzipals für Ihren Kubernetes-Cluster in AKS.

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung](./kubernetes-walkthrough.md).

Für die Erstellung eines Azure AD-Dienstprinzipals müssen Sie dazu berechtigt sein, eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Abonnement zuzuweisen. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, können Sie ggf. Ihren Azure AD- oder Abonnementadministrator bitten, Ihnen die erforderlichen Berechtigungen zuzuweisen oder vorab einen Dienstprinzipal für den Kubernetes-Cluster zu erstellen.

Außerdem muss mindestens Version 2.0.21 der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie „az --version“ aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-sp-with-aks-cluster"></a>Erstellen eines Dienstprinzipals mit dem AKS-Cluster

Wenn Sie einen AKS-Cluster mit dem Befehl `az aks create` bereitstellen, haben Sie die Möglichkeit, automatisch einen Dienstprinzipal zu generieren.

Im folgenden Beispiel wird ein AKS-Cluster erstellt, und da kein vorhandener Dienstprinzipal angegeben ist, wird ein Dienstprinzipal für den Cluster erstellt. Für diesen Vorgang muss Ihr Konto über die erforderlichen Rechte zum Erstellen eines Dienstprinzipals verfügen.

```azurecli
az aks create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Verwenden eines vorhandenen Dienstprinzipals

Ein vorhandener Azure AD-Dienstprinzipal kann verwendet oder vorab für die Verwendung mit einem AKS-Cluster erstellt werden. Dies ist hilfreich, wenn Sie einen Cluster über das Azure-Portal bereitstellen, wo Sie die Dienstprinzipalinformationen angeben müssen.

Ein bereits vorhandener Dienstprinzipal muss folgende Anforderungen erfüllen:

- Umfang: Das für die Clusterbereitstellung verwendete Abonnement
- Rolle: Mitwirkender
- Clientgeheimnis: Muss ein Kennwort sein

## <a name="pre-create-a-new-sp"></a>Erstellen eines Dienstprinzipals im Voraus

Verwenden Sie den Befehl [az ad sp create-for-rbac](), um den Dienstprinzipal mit der Azure-Befehlszeilenschnittstelle zu erstellen.

```azurecli
id=$(az account show --query id --output tsv)
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$id"
```

Die Ausgabe sieht in etwa wie folgt aus. Notieren Sie sich die Werte für `appId` und `password`. Diese Werte werden beim Erstellen eines AKS-Clusters verwendet.

```
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Verwenden eines vorhandenen Dienstprinzipals

Geben Sie bei Verwendung eines vorab erstellten Dienstprinzipals die Werte für `appId` und `password` als Argumentwerte für den Befehl `az aks create` an.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-princal <appId> ----client-secret <password>
```

Wenn Sie einen AKS-Cluster über das Azure-Portal bereitstellen, geben Sie diese Werte im Formular für die AKS-Clusterkonfiguration ein.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Zusätzliche Überlegungen

Beachten Sie bei Verwendung von AKS und Azure AD-Dienstprinzipalen Folgendes:

* Der Dienstprinzipal für Kubernetes ist Bestandteil der Clusterkonfiguration. Verwenden Sie diese Identität jedoch nicht zum Bereitstellen des Clusters.
* Jeder Dienstprinzipal ist einer Azure AD-Anwendung zugeordnet. Der Dienstprinzipal für einen Kubernetes-Cluster kann einem beliebigen gültigen Azure AD-Anwendungsnamen zugeordnet sein (z.B. `https://www.contoso.org/example`). Bei der URL für die Anwendung muss es sich nicht um einen realen Endpunkt handeln.
* Wenn Sie die **Client-ID** des Dienstprinzipals angeben, können Sie den Wert von `appId` (wie in diesem Artikel gezeigt) oder den entsprechenden `name`-Wert für den Dienstprinzipal (beispielsweise `https://www.contoso.org/example`) verwenden.
* Auf dem virtuellen Mastercomputer und den virtuellen Knotencomputern des Kubernetes-Clusters werden die Dienstprinzipal-Anmeldeinformationen in der Datei „/etc/kubernetes/azure.json“ gespeichert.
* Wenn Sie den Dienstprinzipal mithilfe des Befehls `az aks create` automatisch generieren, werden die Dienstprinzipal-Anmeldeinformationen auf dem Computer, auf dem der Befehl ausgeführt wird, in die Datei „~/.azure/acsServicePrincipal.json“ geschrieben.
* Wenn Sie den Dienstprinzipal mithilfe des Befehls `az aks create` automatisch generieren, kann sich der Dienstprinzipal auch bei einer [Azure-Containerregistrierung](../container-registry/container-registry-intro.md) registrieren, die im gleichen Abonnement erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Active Directory-Dienstprinzipalen finden Sie in der Dokumentation zu Azure AD-Anwendungen.

> [!div class="nextstepaction"]
> [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](../active-directory/develop/active-directory-application-objects.md)

---
title: Dienstprinzipale für Azure Kubernetes Service (AKS)
description: Erstellen und Verwalten eines Azure Active Directory-Dienstprinzipals für einen Cluster im Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: ef3139c4b3f06644b219e177fad0c094ed600fb6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394589"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Dienstprinzipale mit Azure Kubernetes Service (AKS)

Für die Interaktion mit Azure-APIs benötigt ein AKS-Cluster einen [Azure Active Directory-Dienstprinzipal (AD)][aad-service-principal]. Der Dienstprinzipal wird für die dynamische Erstellung und Verwaltung anderer Azure-Ressourcen wie Azure Load Balancer oder Azure Container Registry (ACR) benötigt.

Dieser Artikel veranschaulicht das Erstellen und Verwenden eines Dienstprinzipals für Ihre AKS-Cluster.

## <a name="before-you-begin"></a>Voraussetzungen

Für die Erstellung eines Azure AD-Dienstprinzipals müssen Sie dazu berechtigt sein, eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Abonnement zuzuweisen. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, können Sie ggf. Ihren Azure AD- oder Abonnementadministrator bitten, Ihnen die erforderlichen Berechtigungen zuzuweisen oder vorab einen Dienstprinzipal für die Verwendung mit dem AKS-Cluster für Sie zu erstellen.

Außerdem muss mindestens Version 2.0.46 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][install-azure-cli] Informationen dazu.

## <a name="automatically-create-and-use-a-service-principal"></a>Automatisches Erstellen und Verwenden eines Dienstprinzipals

Beim Erstellen eines AKS-Clusters im Azure-Portal oder mithilfe des Befehls [az aks create][az-aks-create] kann Azure automatisch einen Dienstprinzipal generieren.

Im folgenden Azure CLI-Beispiel wird kein Dienstprinzipal angegeben. In diesem Szenario erstellt die Azure-Befehlszeilenschnittstelle einen Dienstprinzipal für den AKS-Cluster. Für den erfolgreichen Abschluss dieses Vorgangs muss Ihr Azure-Konto über die erforderlichen Rechte zum Erstellen eines Dienstprinzipals verfügen.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Manuelles Erstellen eines Dienstprinzipals

Verwenden Sie den Befehl [az ad sp create-for-rbac][az-ad-sp-create], um den Dienstprinzipal mit der Azure-Befehlszeilenschnittstelle manuell zu erstellen. Im folgenden Beispiel verhindert der `--skip-assignment`-Parameter, dass zusätzliche Standardzuweisungen durchgeführt werden:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus: Notieren Sie sich Ihre persönlichen Angaben für `appId` und `password`. Diese Werte werden verwendet, wenn Sie im nächsten Abschnitt einen AKS-Cluster erstellen.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Angeben eines Dienstprinzipals für einen AKS-Cluster

Um einen vorhandenen Dienstprinzipal bei der Erstellung eines AKS-Clusters mithilfe des [az aks create][az-aks-create]-Befehls zu verwenden, geben Sie mit den Parametern `--service-principal` und `--client-secret` `appId` und `password` aus der Ausgabe des [az ad sp create-for-rbac][az-ad-sp-create]-Befehls an:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Wenn Sie einen AKS-Cluster über das Azure-Portal bereitstellen, wählen Sie auf der Seite *Authentication (Authentifizierung)* des **Create Kubernetes cluster (Kubernetes-Cluster erstellen)**-Dialogfelds **Configure service principal (Dienstprinzipal konfigurieren)** aus. Wählen Sie **Use existing (Vorhandene verwenden)** aus, und geben Sie die folgenden Werte an:

- **Service principal client ID (Client-ID des Dienstprinzipals)** ist Ihre *appId*.
- **Service principal client secret (Clientgeheimnis des Dienstprinzipals)** ist der Wert *password*.

![Abbildung der Navigation zu Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="additional-considerations"></a>Zusätzliche Überlegungen

Beachten Sie bei Verwendung von AKS und Azure AD-Dienstprinzipalen die folgenden Kriterien.

- Der Dienstprinzipal für Kubernetes ist Bestandteil der Clusterkonfiguration. Verwenden Sie diese Identität jedoch nicht zum Bereitstellen des Clusters.
- Jeder Dienstprinzipal ist einer Azure AD-Anwendung zugeordnet. Der Dienstprinzipal für einen Kubernetes-Cluster kann einem beliebigen gültigen Azure AD-Anwendungsnamen zugeordnet sein (z.B. *https://www.contoso.org/example*). Bei der URL für die Anwendung muss es sich nicht um einen realen Endpunkt handeln.
- Geben Sie als **Client-ID** des Dienstprinzipals den `appId`-Wert an.
- Auf dem virtuellen Mastercomputer und den virtuellen Knotencomputern des Kubernetes-Clusters werden die Dienstprinzipal-Anmeldeinformationen in der Datei `/etc/kubernetes/azure.json` gespeichert.
- Wenn Sie den Dienstprinzipal mithilfe des Befehls [az aks create][az-aks-create] automatisch generieren, werden die Dienstprinzipal-Anmeldeinformationen auf dem Computer, auf dem der Befehl ausgeführt wird, in die Datei `~/.azure/aksServicePrincipal.json` geschrieben.
- Beim Löschen eines AKS-Clusters, der mit [az aks create][az-aks-create] erstellt wurde, wird der automatisch erstellte Dienstprinzipal nicht gelöscht.
    - Zum Löschen des Dienstprinzipals rufen Sie zuerst mit [az ad app list][az-ad-app-list] die ID für den Dienstprinzipal ab. Das folgende Beispiel fragt den Cluster mit dem Namen *myAKSCluster* ab und löscht dann die App-ID mit [az ad app delete][az-ad-app-delete]. Ersetzen Sie diese Namen durch Ihre eigenen Werte:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Active Directory-Dienstprinzipalen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory][service-principal].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create

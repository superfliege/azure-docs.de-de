---
title: Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
description: Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c250ef3520079f58eea2362212d861fdb134e1af
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-a-static-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)

In einigen Fällen kann sich die öffentliche IP-Adresse des Kubernetes-Diensts ändern, z.B. wenn der Lastenausgleich von Azure Kubernetes Service (AKS) neu erstellt wird, oder Kubernetes-Dienste mit dem Typ „LoadBalancer“ neu erstellt werden. In diesem Dokument wird das Konfigurieren einer statischen IP-Adresse für Ihre Kubernetes-Dienste erläutert.

## <a name="create-static-ip-address"></a>Erstellen einer statischen IP-Adresse

Erstellen Sie eine statische öffentliche IP-Adresse für den Kubernetes-Dienst. Die IP-Adresse muss in der Ressourcengruppe erstellt werden, die während der Clusterbereitstellung automatisch erstellt wurde. Informationen zu den verschiedenen AKS-Ressourcengruppen und zur Identifizierung der automatisch erstellten Ressourcengruppe finden Sie in den [Häufig gestellten Fragen zu AKS][aks-faq-resource-group].

Erstellen Sie mit dem Befehl [az network public ip create][az-network-public-ip-create] die IP-Adresse.

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGRoup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

Notieren Sie sich die IP-Adresse.

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    "ipConfiguration": null,
    "ipTags": [],
    "location": "eastus",
    "name": "myAKSPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Static",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "56ec8760-a3b8-4aeb-a89d-42e68d2cbc8c",
    "sku": {
      "name": "Basic"
    },
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses",
    "zones": null
  }
````

 Bei Bedarf kann die Adresse mit dem Befehl [az network public-ip list][az-network-public-ip-list] abgerufen werden.

```azurecli-interactive
az network public-ip list --resource-group MC_myResourceGRoup_myAKSCluster_eastus --query [0].ipAddress --output tsv
```

```console
40.121.183.52
```

## <a name="create-service-with-ip-address"></a>Erstellen eines Diensts mit IP-Adresse

Sobald die statische IP-Adresse bereitgestellt wurde, kann ein Kubernetes-Dienst mit der `loadBalancerIP`-Eigenschaft und der statischen IP-Adresse als Wert erstellt werden.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="troubleshooting"></a>Problembehandlung

Wenn die statische IP-Adresse nicht oder in der falschen Ressourcengruppe erstellt wurde, schlägt die Diensterstellung fehl. Geben Sie zur Problembehandlung mit dem Befehl [kubectl describe][kubectl-describe] Ereignisse der Diensterstellung zurück.

```azurecli-interactive
kubectl describe service azure-vote-front
```

```console
Name:                     azure-vote-front
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-vote-front
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-vote-front: user supplied IP Address 40.121.183.52 was not found
```

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az_network_public_ip_create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az_network_public_ip_list

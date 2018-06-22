---
title: Whitelist für ausgehende Daten aus Azure Kubernetes Service-Clustern (AKS)
description: Whitelist für ausgehende Daten aus einem Azure Kubernetes Service-Cluster (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: nepeters
ms.openlocfilehash: 0bafb62fcdc8a24084cf7170a0e0f72bfd7824b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659301"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Ausgehende Daten aus Azure Kubernetes Service (AKS)

Standardmäßig wird die Ausgangsadresse eines Azure Kubernetes Service-Clusters zufällig zugewiesen. Diese Konfiguration ist nicht ideal, wenn eine IP-Adresse für den Zugriff auf externe Dienste bestimmt werden muss. In diesem Dokument erfahren sie, wie Sie eine statisch zugewiesene IP-Adresse für ausgehenden Datenverkehr in einem AKS-Cluster erstellen und verwalten.

## <a name="egress-overview"></a>Übersicht über ausgehenden Datenverkehr

Der ausgehende Datenverkehr aus einem AKS-Cluster befolgt die Azure Load Balancer-Konventionen, die [hier dokumentiert werden][outbound-connections]. Bevor der erste Kubernetes-Dienst des Typs `LoadBalancer` erstellt wird, gehören die Knoten des Agent nicht zum Azure Load Balancer-Pool. In dieser Konfiguration haben die Knoten keine öffentliche IP-Adresse auf Instanzebene. Azure verschiebt den ausgehenden Datenfluss zu einer öffentlichen Quell-IP-Adresse, die nicht konfigurierbar oder deterministisch ist.

Wenn ein Kubernetes-Dienst des Typs `LoadBalancer` erstellt wurde, werden Knoten des Agent einem Azure Load Balancer-Pool hinzugefügt. Azure verschiebt den ausgehenden Datenfluss zu der ersten öffentlichen IP-Adresse, die vom Load Balancer konfiguriert wurde.

## <a name="create-a-static-public-ip"></a>Erstellen einer statischen öffentlichen IP-Adresse

Erstellen Sie eine statische IP-Adresse, und stellen Sie sicher, dass der Load Balancer diese nutzt, damit keine zufälligen IP-Adressen verwendet werden. Die IP-Adresse muss in der AKS-Ressourcengruppe **Knoten** erstellt werden.

Rufen Sie den Namen der Ressourcengruppe mit dem Befehl [az resource show][az-resource-show] ab. Aktualisieren Sie den Namen der Ressourcengruppe und des Clusters gemäß Ihrer Umgebung.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Erstellen Sie dann mit dem Befehl [az network public-ip create][public-ip-create] eine statische öffentliche IP-Adresse. Aktualisieren Sie den Namen der Ressourcengruppe, damit er dem Namen aus dem vorherigen Schritt entspricht.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Erstellen eines Dienstes mit der statischen IP-Adresse

Da Sie nun eine IP-Adresse haben, erstellen Sie einen Kubernetes-Dienst des Typs `LoadBalancer`, und weisen Sie die IP-Adresse diesem Dienst zu.

Erstellen Sie eine Datei namens „`egress-service.yaml`“, und fügen Sie den folgenden YAML-Code ein. Aktualisieren Sie die Bezeichnung der IP-Adresse entsprechend Ihrer Umgebung.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-egress
spec:
  loadBalancerIP: 23.101.128.81
  type: LoadBalancer
  ports:
  - port: 8080
```

Erstellen Sie den Dienst und die Bereitstellung mit dem Befehl `kubectl apply`.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Beim Erstellen dieses Dienstes wird eine neue Front-End-IP im Azure Load Balancer konfiguriert. Wenn keine anderen IP-Adressen konfiguriert wurden, wird ab sofort **sämtlicher ausgehender Datenverkehr** diese Adresse verwenden. Wenn mehrere IP-Adressen im Azure Load Balancer konfiguriert wurden, nutzt der ausgehende Datenverkehr die erste IP-Adresse im Load Balancer.

## <a name="verify-egress-address"></a>Überprüfen der Adresse für ausgehende Daten

Überprüfen Sie mithilfe eines Dienstes wie `checkip.dyndns.org`, ob die öffentliche IP-Adresse verwendet wird.

Starten Sie den Dienst und fügen Sie ihn an einen Pod an:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Installieren Sie bei Bedarf Curl im Container:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org` gibt folgende Ausgangs-IP-Adresse zurück:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

Die IP-Adresse muss der statischen IP-Adresse entsprechen, die Azure Load Balancer angefügt ist.

## <a name="ingress-controller"></a>Eingangscontroller

Verwenden Sie einen Eingangscontroller, um die Verwaltung mehrerer öffentlicher IP-Adresse im Azure Load Balancer zu vermeiden. Eingangscontroller bieten Vorteile wie Lastenausgleich, SSL-/TLS-Terminierung, Unterstützung der URI-Neugenerierung und Upstream der SSL-/TLS-Verschlüsselung. Weitere Informationen zu Eingangscontrollern in AKS finden Sie unter [Konfigurieren eines NGINX-Eingangscontrollers in einem AKS-Cluster][ingress-aks-cluster].

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die in diesem Dokument demonstrierte Software.

- [Helm CLI][helm-cli-install]
- [NGINX-Eingangscontroller][nginx-ingress]
- [Azure Load Balancer – ausgehende Verbindungen][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

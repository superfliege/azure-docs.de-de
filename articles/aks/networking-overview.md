---
title: Netzwerkkonfiguration in Azure Kubernetes Service (AKS)
description: Enthält Informationen zur grundlegenden und erweiterten Netzwerkkonfiguration in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/23/2018
ms.author: marsma
ms.openlocfilehash: cfe034d6dcac48d7c9e4b2ce17e4926a81a27886
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216103"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Netzwerkkonfiguration in Azure Kubernetes Service (AKS)

Beim Erstellen eines AKS-Clusters (Azure Kubernetes Service) können Sie zwischen zwei Netzwerkoptionen wählen: **Basic** (Grundlegend) und **Advanced** (Erweitert).

## <a name="basic-networking"></a>Grundlegende Netzwerke

Die Netzwerkoption **Basic** (Grundlegend) ist die Standardkonfiguration für die AKS-Clustererstellung. Die Netzwerkkonfiguration des Clusters und der dazugehörigen Pods wird vollständig über Azure verwaltet und eignet sich für Bereitstellungen, für die keine benutzerdefinierte VNET-Konfiguration erforderlich ist. Bei Auswahl von „Basic“ (Grundlegend) haben Sie keine Kontrolle über die Netzwerkkonfiguration, z.B. Subnetze oder die dem Cluster zugewiesenen IP-Adressbereiche.

Für Knoten in einem AKS-Cluster, für den die Netzwerkoption „Basic“ (Grundlegend) konfiguriert ist, wird das Kubernetes-Plug-In [kubenet][kubenet] verwendet.

## <a name="advanced-networking"></a>Erweiterte Netzwerke

Bei der Netzwerkoption **Advanced** (Erweitert) werden Ihre Pods in einem virtuellen Azure-Netzwerk (VNET) angeordnet. Sie konfigurieren dieses Netzwerk, um für die Pods die automatische Konnektivität mit VNET-Ressourcen herzustellen und die Integration in die umfassenden Funktionen von VNETs zu ermöglichen. Die Netzwerkoption „Advanced“ (Erweitert) ist verfügbar, wenn AKS-Cluster mit dem [Azure-Portal][portal], mit Azure CLI oder mit einer Resource Manager-Vorlage bereitgestellt werden.

Für Knoten in einem AKS-Cluster, für das die Netzwerkoption „Advanced“ (Erweitert) konfiguriert ist, wird das Kubernetes-Plug-In [Azure Container Networking Interface (CNI)][cni-networking] verwendet.

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Features der Netzwerkoption „Advanced“ (Erweitert)

Die Netzwerkoption „Advanced“ (Erweitert) bietet die folgenden Vorteile:

* Stellen Sie Ihren AKS-Cluster in einem vorhandenen VNET bereit, oder erstellen Sie ein neues VNET und Subnetz für Ihren Cluster.
* Jedem Pod im Cluster wird eine IP-Adresse im VNET zugewiesen, und es ist jeweils eine direkte Kommunikation mit anderen Pods im Cluster und anderen Knoten im VNET möglich.
* Ein Pod kann eine Verbindung mit anderen Diensten eines mittels Peering verbundenen VNET und mit lokalen Netzwerken über ExpressRoute- und Site-to-Site-VPN-Verbindungen (S2S) herstellen. Pods sind auch über die lokale Umgebung erreichbar.
* Machen Sie einen Kubernetes-Dienst extern oder intern über den Azure Load Balancer verfügbar. Dies ist auch ein Feature der Netzwerkoption „Basic“ (Grundlegend).
* Pods in einem Subnetz mit aktivierten Dienstendpunkten können eine sichere Verbindung mit Azure-Diensten herstellen, z.B. Azure Storage und SQL-Datenbank.
* Verwenden Sie benutzerdefinierte Routen (UDRs) zum Weiterleiten von Datenverkehr von Pods an eine virtuelle Netzwerkappliance.
* Pods können auf Ressourcen im öffentlichen Internet zugreifen. Dies ist auch ein Feature der Netzwerkoption „Basic“ (Grundlegend).

## <a name="advanced-networking-prerequisites"></a>Voraussetzungen für erweiterte Netzwerke

* Das VNET des AKS-Clusters muss eine ausgehende Internetkonnektivität zulassen.
* In einem Subnetz sollte nicht mehr als ein AKS-Cluster erstellt werden.
* AKS-Cluster verwenden möglicherweise nicht `169.254.0.0/16`, `172.30.0.0/16` oder `172.31.0.0/16` für den Kubernetes-Dienstadressbereich.
* Der vom AKS-Cluster verwendete Dienstprinzipal muss zumindest über Berechtigungen als [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) im Subnetz in Ihrem VNET verfügen. Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planen der IP-Adressierung für Ihren Cluster

Für Cluster, für die die Netzwerkoption „Advanced“ (Erweitert) konfiguriert wird, ist eine zusätzliche Planung erforderlich. Die Größe Ihres VNET und des dazugehörigen Subnetzes muss die Anzahl von Pods, die ausgeführt werden sollen, und die Anzahl von Knoten für den Cluster abdecken.

IP-Adressen für die Pods und die Knoten des Clusters werden über das angegebene Subnetz im VNET zugewiesen. Für jeden Knoten werden eine primäre IP-Adresse (die eigentliche IP-Adresse des Knotens) und 30 zusätzliche IP-Adressen konfiguriert (per Azure CNI vorkonfiguriert), die den für den Knoten bestimmten Pods zugewiesen werden. Wenn Sie Ihren Cluster horizontal hochskalieren, wird jeder Knoten auf ähnliche Weise mit IP-Adressen aus dem Subnetz konfiguriert.

Der IP-Addressplan eines AKS-Clusters beinhaltet neben einem VNET mindestens ein Subnetz für Knoten und Pods sowie einen Kubernetes-Dienstadressbereich.

| Adressbereich/Azure-Ressourcen | Grenzen und Kontingente |
| --------- | ------------- |
| Virtuelles Netzwerk | Ein Azure-VNET kann eine Größe von bis zu /8 haben, dabei aber nur 16.000 konfigurierte IP-Adressen aufweisen. |
| Subnetz | Muss groß genug für die Knoten, Pods und alle Kubernetes- und Azure-Ressourcen sein, die in Ihrem Cluster bereitgestellt werden können. Wenn Sie beispielsweise einen internen Azure Load Balancer bereitstellen, werden dessen Front-End-IP-Adressen aus dem Clusternetzwerk zugeordnet, nicht die öffentlichen IP-Adressen. <p/>So wird die *mindestens erforderliche* Subnetzgröße berechnet: `(number of nodes) + (number of nodes * pods per node)` <p/>Beispiel für einen Cluster mit 50 Knoten: `(50) + (50 * 30) = 1,550` (/ 21 oder mehr) |
| Kubernetes-Dienstadressbereich | Dieser Bereich sollte nicht von Netzwerk-Elementen verwendet werden, die sich in diesem VNET befinden oder damit verbunden sind. Das Dienstadress-CIDR darf höchstens eine Größe von /12 aufweisen. |
| Kubernetes-DNS-Dienst – IP-Adresse | Die IP-Adresse im Kubernetes-Dienstadressbereich wird bei der Clusterdienstermittlung (kube-dns) verwendet. |
| Docker-Bridge-Adresse | Die IP-Adresse (in CIDR-Schreibweise) wird auf Knoten als Docker-Bridge-Adresse verwendet. Standard 172.17.0.1/16. |

Jedes VNET, das für die Verwendung mit dem Azure CNI-Plug-In bereitgestellt wird, ist auf **16.000 konfigurierte IP-Adressen** begrenzt.

## <a name="maximum-pods-per-node"></a>Maximale Pods pro Knoten

Standardmäßig variiert die maximale Anzahl von Pods pro Knoten in einem AKS-Cluster zwischen grundlegenden und erweiterten Netzwerken und je nach Methode für die Clusterbereitstellung.

### <a name="default-maximum"></a>Höchstwert (Standard)

* Grundlegende Netzwerke: **110 Pods pro Knoten**
* Erweiterte Netzwerke: **30 Pods pro Knoten**

### <a name="configure-maximum"></a>Konfigurieren des Höchstwerts

Je nach Ihrer Bereitstellungsmethode können Sie die maximale Anzahl von Pods pro Knoten in einem AKS-Cluster anpassen.

* **Azure CLI:** Geben Sie das `--max-pods`-Argument an, wenn Sie einen Cluster mit dem Befehl [az aks create][az-aks-create] bereitstellen.
* **Resource Manager-Vorlage:** Geben Sie die `maxPods`-Eigenschaft im Objekt [ManagedClusterAgentPoolProfile] an, wenn Sie einen Cluster mit einer Resource Manager-Vorlage bereitstellen.
* **Azure-Portal:** Sie können die maximale Anzahl von Pods pro Knoten nicht ändern, wenn Sie einen Cluster über das Azure-Portal bereitstellen. Die Cluster in erweiterten Netzwerken sind auf 30 Pods pro Knoten beschränkt, wenn diese im Azure-Portal bereitgestellt werden.

## <a name="deployment-parameters"></a>Bereitstellungsparameter

Beim Erstellen eines AKS-Clusters können folgende Parameter für erweiterte Netzwerke konfiguriert werden:

**Virtuelles Netzwerk**: Das VNET, in dem Sie den Kubernetes-Cluster bereitstellen möchten. Wenn Sie ein neues VNET für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines virtuellen Netzwerks* ausführen. Das VNET ist auf 16.000 konfigurierte IP-Adressen beschränkt.

**Subnetz**: Das Subnetz im VNET, in dem Sie den Cluster bereitstellen möchten. Wenn Sie ein neues Subnetz im VNET für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines Subnetzes* ausführen.

**Kubernetes-Dienstadressbereich**: Der *Kubernetes-Dienstadressbereich* ist der IP-Bereich, aus dem die Adressen den Kubernetes-Diensten in Ihrem Cluster zugewiesen werden (weitere Informationen zu den Kubernetes-Diensten finden Sie in der Kubernetes-Dokumentation unter [Dienste][services]).

Für den Kubernetes-Dienst-IP-Adressbereich gilt Folgendes:

* Er darf nicht innerhalb des VNET-IP-Adressbereichs Ihres Clusters liegen.
* Er darf sich nicht mit anderen VNETs überlappen, die Peers des Cluster-VNETs sind.
* Er darf sich nicht mit lokalen IP-Adressen überlappen.

Bei Verwendung sich überlappender IP-Adressbereiche kann es zu unvorhersehbarem Verhalten kommen. Wenn beispielsweise ein Pod versucht, auf eine IP-Adresse außerhalb des Clusters zuzugreifen, und diese IP-Adresse auch eine Dienst-IP-Adresse ist, können unvorhersehbares Verhalten und Fehler auftreten.

**Kubernetes DNS service IP address** (Kubernetes-DNS-Dienst – IP-Adresse): Die IP-Adresse für den DNS-Dienst des Clusters. Diese Adresse muss innerhalb des *Kubernetes-Dienstadressbereichs* liegen.

**Docker Bridge address** (Docker-Bridge-Adresse): Die IP-Adresse und Netzmaske für die Zuweisung zur Docker-Bridge. Diese IP-Adresse darf nicht innerhalb des VNET-IP-Adressbereichs Ihres Clusters liegen.

## <a name="configure-networking---cli"></a>Konfigurieren der Netzwerkeinstellungen – CLI

Wenn Sie einen AKS-Cluster mit Azure CLI erstellen, können Sie auch erweiterte Netzwerkeinstellungen konfigurieren. Verwenden Sie die folgenden Befehle, um einen neuen AKS-Cluster mit aktivierten erweiterten Netzwerkfunktionen zu erstellen.

Ermitteln Sie zunächst die Subnetzressourcen-ID für das vorhandene Subnetz, mit dem der AKS-Cluster verknüpft werden soll:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Verwenden Sie den Befehl [az aks create][az-aks-create] mit dem Argument `--network-plugin azure`, um einen Cluster mit erweiterten Netzwerkeinstellungen zu erstellen. Aktualisieren Sie den `--vnet-subnet-id`-Wert mit der im vorherigen Schritt erfassten Subnetz-ID:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Konfigurieren der Netzwerkeinstellungen – Portal

Im folgenden Screenshot des Azure-Portals ist ein Beispiel für die Konfiguration dieser Einstellungen während der AKS-Clustererstellung dargestellt:

![Erweiterte Netzwerkkonfiguration im Azure-Portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Die folgenden Fragen und Antworten gelten für die Netzwerkkonfiguration vom Typ **Advanced** (Erweitert).

* *Kann ich VMs in meinem Clustersubnetz bereitstellen?*

  Nein. Die Bereitstellung von VMs in dem Subnetz, das von Ihrem Kubernetes-Cluster verwendet wird, wird nicht unterstützt. VMs können in demselben VNET bereitgestellt werden, aber es muss ein anderes Subnetz verwendet werden.

* *Kann ich Netzwerkrichtlinien pro Pod konfigurieren?*

  Nein. Netzwerkrichtlinien pro Pod werden derzeit nicht unterstützt.

* *Ist die maximale Anzahl von Pods, die auf einem Knoten bereitgestellt werden können, konfigurierbar?*

  Ja, wenn Sie einen Cluster mit der Azure CLI oder einer Resource Manager-Vorlage bereitstellen. Weitere Informationen finden Sie unter [Maximale Pods pro Knoten](#maximum-pods-per-node).

* *Wie kann ich zusätzliche Eigenschaften für das Subnetz konfigurieren, das ich während der Erstellung des AKS-Clusters erstellt habe? Beispiel: Dienstendpunkte.*

  Die vollständige Liste mit Eigenschaften für das VNET und Subnetze, die Sie während der Erstellung des AKS-Clusters erstellen, kann im Azure-Portal auf der Standardseite für die VNET-Konfiguration konfiguriert werden.

## <a name="next-steps"></a>Nächste Schritte

### <a name="networking-in-aks"></a>Netzwerke in AKS

Weitere Informationen zu Netzwerken in AKS finden Sie in den folgenden Artikeln:

[Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)](static-ip.md)

[Eingehende HTTPS-Daten in Azure Container Service (AKS)](ingress.md)

[Verwenden eines internen Lastenausgleichs mit Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>ACS Engine

[Azure Container Service Engine (ACS Engine)][acs-engine] ist ein Open-Source-Projekt, über das Azure Resource Manager-Vorlagen generiert werden, die Sie zum Bereitstellen von Docker-fähigen Clustern in Azure verwenden können. Kubernetes-, DC/OS-, Swarm-Modus- und Swarm-Orchestratoren können mit der ACS Engine bereitgestellt werden.

Für per ACS Engine erstellte Kubernetes-Cluster werden sowohl [kubenet][kubenet]- als auch [Azure CNI][cni-networking]-Plug-Ins unterstützt. Für die ACS Engine werden also die Netzwerkszenarien „Basic“ (Grundlegend) und „Advanced“ (Erweitert) unterstützt.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object

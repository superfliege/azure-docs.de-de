---
title: Netzwerkkonfiguration in Azure Kubernetes Service (AKS)
description: Enthält Informationen zur grundlegenden und erweiterten Netzwerkkonfiguration in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/31/2018
ms.author: marsma
ms.openlocfilehash: e78be76d68cf75cf9d59f5b5dff86c65524275a9
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697240"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Netzwerkkonfiguration in Azure Kubernetes Service (AKS)

Beim Erstellen eines AKS-Clusters (Azure Kubernetes Service) können Sie zwischen zwei Netzwerkoptionen wählen: **Basic** (Grundlegend) und **Advanced** (Erweitert).

## <a name="basic-networking"></a>Grundlegende Netzwerke

Die Netzwerkoption **Basic** (Grundlegend) ist die Standardkonfiguration für die AKS-Clustererstellung. Die Netzwerkkonfiguration des Clusters und der dazugehörigen Pods wird vollständig über Azure verwaltet und eignet sich für Bereitstellungen, die keine benutzerdefinierte VNET-Konfiguration erfordern. Bei Auswahl von „Basic“ (Grundlegend) haben Sie keine Kontrolle über die Netzwerkkonfiguration, z.B. Subnetze oder die dem Cluster zugewiesenen IP-Adressbereiche.

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

* Das virtuelle Netzwerk des AKS-Clusters muss ausgehende Internetkonnektivität zulassen.
* In einem Subnetz sollte nicht mehr als ein AKS-Cluster erstellt werden.
* AKS-Cluster verwenden möglicherweise nicht `169.254.0.0/16`, `172.30.0.0/16` oder `172.31.0.0/16` für den Kubernetes-Dienstadressbereich.
* Der vom AKS-Cluster verwendete Dienstprinzipal muss zumindest über Berechtigungen [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) für das Subnetz in Ihrem virtuellen Netzwerk verfügen. Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planen der IP-Adressierung für Ihren Cluster

Für Cluster, für die die Netzwerkoption „Advanced“ (Erweitert) konfiguriert wird, ist eine zusätzliche Planung erforderlich. Die Größe Ihres virtuellen Netzwerks und des dazugehörigen Subnetzes muss die Anzahl von Pods, die ausgeführt werden sollen, und die Anzahl von Knoten für den Cluster abdecken.

IP-Adressen für die Pods und die Knoten des Clusters werden über das angegebene Subnetz im virtuellen Netzwerk zugewiesen. Für jeden Knoten werden eine primäre IP-Adresse (die eigentliche IP-Adresse des Knotens) und 30 zusätzliche IP-Adressen konfiguriert (per Azure CNI vorkonfiguriert), die den für den Knoten bestimmten Pods zugewiesen werden. Wenn Sie Ihren Cluster horizontal hochskalieren, wird jeder Knoten auf ähnliche Weise mit IP-Adressen aus dem Subnetz konfiguriert.

Der IP-Adressplan eines AKS-Clusters enthält neben einem virtuellen Netzwerk mindestens ein Subnetz für Knoten und Pods sowie einen Kubernetes-Dienstadressbereich.

| Adressbereich/Azure-Ressourcen | Grenzen und Kontingente |
| --------- | ------------- |
| Virtuelles Netzwerk | Das virtuelle Azure-Netzwerk kann eine Größe von /8 haben, ist aber auf 65.536 konfigurierte IP-Adressen beschränkt. |
| Subnetz | Muss groß genug für die Knoten, Pods und alle Kubernetes- und Azure-Ressourcen sein, die in Ihrem Cluster bereitgestellt werden können. Wenn Sie beispielsweise einen internen Azure Load Balancer bereitstellen, werden dessen Front-End-IP-Adressen aus dem Clusternetzwerk zugeordnet, nicht die öffentlichen IP-Adressen. <p/>So wird die *mindestens erforderliche* Subnetzgröße berechnet: `(number of nodes) + (number of nodes * pods per node)` <p/>Beispiel für einen Cluster mit 50 Knoten: `(50) + (50 * 30) = 1,550` (/ 21 oder mehr) |
| Kubernetes-Dienstadressbereich | Dieser Bereich darf nicht von Netzwerkelementen verwendet werden, die sich in diesem virtuellen Netzwerk befinden oder damit verbunden sind. Das Dienstadress-CIDR darf höchstens eine Größe von /12 aufweisen. |
| Kubernetes-DNS-Dienst – IP-Adresse | Die IP-Adresse im Kubernetes-Dienstadressbereich wird bei der Clusterdienstermittlung (kube-dns) verwendet. |
| Docker-Bridge-Adresse | Die IP-Adresse (in CIDR-Schreibweise) wird auf Knoten als Docker-Bridge-Adresse verwendet. Standard 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Maximale Pods pro Knoten

Standardmäßig variiert die maximale Anzahl von Pods pro Knoten in einem AKS-Cluster zwischen einfachen und erweiterten Netzwerken und je nach Methode für die Clusterbereitstellung.

### <a name="default-maximum"></a>Höchstwert (Standard)

Dies sind die *standardmäßigen* Höchstwerte, wenn Sie einen AKS-Cluster bereitstellen, ohne die maximale Anzahl von Pods zum Zeitpunkt der Bereitstellung anzugeben:

| Bereitstellungsmethode | Basic | Erweitert | Bei der Bereitstellung konfigurierbar |
| -- | :--: | :--: | -- |
| Azure-Befehlszeilenschnittstelle | 110 | 30 | JA |
| Resource Manager-Vorlage | 110 | 30 | JA |
| Portal | 110 | 30 | Nein  |

### <a name="configure-maximum---new-clusters"></a>Konfigurieren des Höchstwerts: Neue Cluster

So geben Sie eine andere maximale Anzahl von Pods pro Knoten an, wenn Sie einen AKS-Cluster bereitstellen

* **Azure CLI:** Geben Sie das `--max-pods`-Argument an, wenn Sie einen Cluster mit dem Befehl [az aks create][az-aks-create] bereitstellen.
* **Resource Manager-Vorlage:** Geben Sie die `maxPods`-Eigenschaft im Objekt [ManagedClusterAgentPoolProfile] an, wenn Sie einen Cluster mit einer Resource Manager-Vorlage bereitstellen.
* **Azure-Portal:** Sie können die maximale Anzahl von Pods pro Knoten nicht ändern, wenn Sie einen Cluster über das Azure-Portal bereitstellen. Die Cluster in erweiterten Netzwerken sind auf 30 Pods pro Knoten beschränkt, wenn diese im Azure-Portal bereitgestellt werden.

### <a name="configure-maximum---existing-clusters"></a>Konfigurieren des Höchstwerts: Vorhandene Cluster

Sie können die maximalen Anzahl von Pods pro Knoten für einen vorhandenen AKS-Cluster nicht ändern. Sie können die Anzahl nur bei der ersten Bereitstellung des Clusters anpassen.

## <a name="deployment-parameters"></a>Bereitstellungsparameter

Beim Erstellen eines AKS-Clusters können folgende Parameter für erweiterte Netzwerke konfiguriert werden:

**Virtuelles Netzwerk**: Das virtuelle Netzwerk, in dem Sie den Kubernetes-Cluster bereitstellen möchten. Wenn Sie ein neues virtuelles Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines virtuellen Netzwerks* ausführen. Weitere Informationen zu Grenzwerten und Kontingenten für virtuelle Azure-Netzwerke finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subnetz**: Das Subnetz im virtuellen Netzwerk, in dem Sie den Cluster bereitstellen möchten. Wenn Sie ein neues Subnetz im virtuellen Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines Subnetzes* ausführen.

**Kubernetes-Dienstadressbereich**: Dies ist der Satz von virtuellen IP-Adressen, die Kubernetes [Diensten][services] in Ihrem Cluster zuweist. Sie können jeden privaten Adressbereich verwenden, der die folgenden Anforderungen erfüllen:

* Darf nicht innerhalb des IP-Adressbereichs des virtuellen Netzwerk Ihres Clusters liegen
* Darf sich nicht mit anderen virtuellen Netzwerken überlappen, die Peers des virtuellen Netzwerks des Clusters sind
* Darf sich nicht mit lokalen IP-Adressen überlappen.
* Darf sich nicht in den Bereichen `169.254.0.0/16`, `172.30.0.0/16` oder `172.31.0.0/16` befinden.

Obwohl es technisch möglich ist, einen Dienstadressbereich im gleichen virtuellen Netzwerk wie Ihr Cluster anzugeben, wird dies nicht empfohlen. Bei Verwendung sich überlappender IP-Adressbereiche kann es zu unvorhersehbarem Verhalten kommen. Weitere Informationen finden Sie im Abschnitt [Häufig gestellte Fragen](#frequently-asked-questions) in diesem Artikel. Weitere Informationen zu Kubernetes-Diensten finden Sie in der Kubernetes-Dokumentation unter [Dienste][services].

**Kubernetes DNS service IP address** (Kubernetes-DNS-Dienst – IP-Adresse): Die IP-Adresse für den DNS-Dienst des Clusters. Diese Adresse muss innerhalb des *Kubernetes-Dienstadressbereichs* liegen.

**Docker Bridge address** (Docker-Bridge-Adresse): Die IP-Adresse und Netzmaske für die Zuweisung zur Docker-Bridge. Diese IP-Adresse darf nicht innerhalb des IP-Adressbereichs des virtuellen Netzwerk Ihres Clusters liegen.

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

  Nein. Die Bereitstellung von VMs in dem Subnetz, das von Ihrem Kubernetes-Cluster verwendet wird, wird nicht unterstützt. VMs können in demselben virtuellen Netzwerk bereitgestellt werden, aber nur in einem anderen Subnetz.

* *Kann ich Netzwerkrichtlinien pro Pod konfigurieren?*

  Nein. Netzwerkrichtlinien pro Pod werden derzeit nicht unterstützt.

* *Ist die maximale Anzahl von Pods, die auf einem Knoten bereitgestellt werden können, konfigurierbar?*

  Ja, wenn Sie einen Cluster mit der Azure CLI oder einer Resource Manager-Vorlage bereitstellen. Weitere Informationen finden Sie unter [Maximale Pods pro Knoten](#maximum-pods-per-node).

  Sie können die maximalen Anzahl von Pods pro Knoten für einen vorhandenen Cluster nicht ändern.

* *Wie kann ich zusätzliche Eigenschaften für das Subnetz konfigurieren, das ich während der Erstellung des AKS-Clusters erstellt habe? Beispiel: Dienstendpunkte.*

  Die vollständige Liste mit Eigenschaften für das virtuelle Netzwerk und Subnetze, die Sie während der Erstellung des AKS-Clusters erstellen, kann im Azure-Portal auf der Standardseite für die Konfiguration des virtuellen Netzwerks konfiguriert werden.

* *Kann ich im virtuellen Netzwerk meines Clusters ein anderes Subnetz für den*  **Kubernetes-Dienstadressbereich** verwenden?

  Es wird zwar nicht empfohlen, diese Konfiguration ist jedoch möglich. Der Dienstadressbereich ist ein Satz von virtuellen IP-Adressen (VIPs), die Kubernetes Diensten in Ihrem Cluster zuweist. Das Azure-Netzwerk hat keinen Einblick in den Dienst-IP-Adressbereich des Kubernetes-Clusters. Aufgrund fehlender Einblicke in den Dienstadressbereich des Clusters ist es möglich, später ein neues Subnetz im virtuellen Netzwerk des Clusters zu erstellen, das mit dem Dienstadressbereich überlappt. Im Falle einer solchen Überlappung weist Kubernetes einem Dienst ggf. eine IP zu, die bereits von einer anderen Ressource im Subnetz verwendet wird. Dies führt zu unvorhersehbarem Verhalten oder Fehlern. Wenn Sie einen Adressbereich außerhalb des virtuellen Netzwerk des Clusters verwenden, können Sie dieses Überlappungsrisiko umgehen.

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
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object

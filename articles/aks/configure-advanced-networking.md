---
title: Konfigurieren von erweiterten Netzwerkoptionen in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie erweiterte Netzwerke in Azure Kubernetes Service (AKS) konfigurieren, einschließlich der Bereitstellung eines AKS-Clusters in einem vorhandenen virtuellen Netzwerk und Subnetz.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 943c0d4eb25fad1282b3329b945ded45581aeba3
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994556"
---
# <a name="configure-advanced-networking-in-azure-kubernetes-service-aks"></a>Konfigurieren von erweiterten Netzwerken in Azure Kubernetes Service (AKS)

Standardmäßig nutzen AKS-Cluster *grundlegende* Netzwerkfunktionen, indem ein virtuelles Netzwerk und ein Subnetz für die Verwendung mit dem Cluster erstellt und konfiguriert werden. Für zusätzliche Kontrolle über diese Netzwerkoptionen, z.B. IP-Adressbereiche, können Sie stattdessen *erweiterte* Netzwerke verwenden. Mit erweiterten Netzwerken können Sie einen AKS-Cluster auch in einem vorhandenen virtuellen Netzwerk und Subnetz erstellen. Dieses vorhandene virtuelle Netzwerk bietet häufig Verbindungen mit einem lokalen Netzwerk über Azure ExpressRoute oder Site-to-Site-VPN.

Dieser Artikel veranschaulicht die Verwendung von erweiterten Netzwerken zum Erstellen und Verwenden eines virtuellen Netzwerks mit einem AKS-Cluster. Weitere allgemeine Informationen zu Netzwerken finden Sie unter [Netzwerkkonzepte für Kubernetes und AKS][aks-network-concepts].

## <a name="prerequisites"></a>Voraussetzungen

* Das virtuelle Netzwerk des AKS-Clusters muss ausgehende Internetkonnektivität zulassen.
* In einem Subnetz sollte nicht mehr als ein AKS-Cluster erstellt werden.
* AKS-Cluster verwenden möglicherweise nicht `169.254.0.0/16`, `172.30.0.0/16` oder `172.31.0.0/16` für den Kubernetes-Dienstadressbereich.
* Der vom AKS-Cluster verwendete Dienstprinzipal muss zumindest über Berechtigungen [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) für das Subnetz in Ihrem virtuellen Netzwerk verfügen. Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planen der IP-Adressierung für Ihren Cluster

Für Cluster, für die die Netzwerkoption „Advanced“ (Erweitert) konfiguriert wird, ist eine zusätzliche Planung erforderlich. Die Größe Ihres virtuellen Netzwerks und des zugehörigen Subnetzes muss die Anzahl von Pods, die ausgeführt werden sollen, und die Anzahl von Knoten für den Cluster abdecken.

IP-Adressen für die Pods und die Knoten des Clusters werden über das angegebene Subnetz im virtuellen Netzwerk zugewiesen. Jeder Knoten wird mit einer primären IP-Adresse konfiguriert. Standardmäßig werden 30 zusätzliche IP-Adressen durch Azure CNI vorkonfiguriert, die den im Knoten geplanten Pods zugewiesen werden. Wenn Sie Ihren Cluster horizontal hochskalieren, wird jeder Knoten auf ähnliche Weise mit IP-Adressen aus dem Subnetz konfiguriert. Sie können auch die [maximale Anzahl von Pods pro Knoten](#maximum-pods-per-node) anzeigen.

> [!IMPORTANT]
> Die Anzahl der erforderlichen IP-Adressen sollte Überlegungen für Upgrade- und Skalierungsvorgänge beinhalten. Wenn Sie den IP-Adressbereich so einstellen, dass er nur eine feste Anzahl von Knoten unterstützt, können Sie Ihren Cluster nicht aktualisieren oder skalieren.
>
> - Wenn Sie Ihren AKS-Cluster **aktualisieren**, wird im Cluster ein neuer Knoten bereitgestellt. Dienste und Workloads werden auf dem neuen Knoten ausgeführt, und ein älterer Knoten wird aus dem Cluster entfernt. Dieser rollende Aktualisierungsprozess erfordert, dass mindestens ein zusätzlicher Block von IP-Adressen verfügbar ist. Die Knotenanzahl ist dann `n + 1`.
>
> - Wenn Sie Ihren AKS-Cluster **skalieren**, wird im Cluster ein neuer Knoten bereitgestellt. Dienste und Workloads werden auf dem neuen Knoten ausgeführt. Bei Ihrem IP-Adressbereich muss berücksichtigt werden, wie Sie die Anzahl der Knoten und Pods, die Ihr Cluster unterstützen kann, vergrößern können. Es sollte auch ein zusätzlicher Knoten für Aktualisierungsvorgänge integriert werden. Die Knotenanzahl ist dann `n + number-of-additional-scaled-nodes-you-anticipate + 1`.

Wenn Sie erwarten, dass Ihre Knoten die maximale Anzahl von Pods ausführen und regelmäßig Pods zerstören und bereitstellen, sollten Sie auch einige zusätzliche IP-Adressen pro Knoten berücksichtigen. Diese zusätzlichen IP-Adressen berücksichtigen, dass es einige Sekunden dauern kann, bis ein Dienst gelöscht und die IP-Adresse für einen neuen Dienst freigegeben wird und die Adresse erworben wird.

Der IP-Adressplan eines AKS-Clusters enthält neben einem virtuellen Netzwerk mindestens ein Subnetz für Knoten und Pods sowie einen Kubernetes-Dienstadressbereich.

| Adressbereich/Azure-Ressourcen | Grenzen und Kontingente |
| --------- | ------------- |
| Virtuelles Netzwerk | Das virtuelle Azure-Netzwerk kann eine Größe von /8 haben, ist aber auf 65.536 konfigurierte IP-Adressen beschränkt. |
| Subnetz | Muss groß genug für die Knoten, Pods und alle Kubernetes- und Azure-Ressourcen sein, die in Ihrem Cluster bereitgestellt werden können. Wenn Sie beispielsweise einen internen Azure Load Balancer bereitstellen, werden dessen Front-End-IP-Adressen aus dem Clusternetzwerk zugeordnet, nicht die öffentlichen IP-Adressen. Die Größe des Subnetzes sollte auch Aktualisierungsvorgänge oder zukünftige Skalierungsanforderungen berücksichtigen.<p />So berechnen Sie die *Mindestsubnetzgröße* mit einem zusätzlichen Knoten für Aktualisierungsvorgänge: `(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>Beispiel für einen Cluster mit 50 Knoten: `(51) + (51  * 30 (default)) = 1,581` (/ 21 oder mehr)<p/>Beispiel für einen 50-Knoten-Cluster, der auch die Möglichkeit beinhaltet, ihn um weitere 10 Knoten hochzuskalieren: `(61) + (61 * 30 (default)) = 1,891` (/21 oder größer)<p>Wenn Sie beim Erstellen des Clusters keine maximale Anzahl von Pods pro Knoten angeben, wird diese auf *30* festgelegt. Die mindestens erforderliche Anzahl von IP-Adressen basiert auf diesem Wert. Wenn Sie Ihre mindestens erforderliche Anzahl von IP-Adressen anhand eines anderen Maximalwerts berechnen, finden Sie Informationen zum Festlegen dieses Werts beim Bereitstellen Ihres Clusters unter [Konfigurieren der maximalen Anzahl von Pods pro Knoten](#configure-maximum---new-clusters). |
| Kubernetes-Dienstadressbereich | Dieser Bereich darf nicht von Netzwerkelementen verwendet werden, die sich in diesem virtuellen Netzwerk befinden oder damit verbunden sind. Das Dienstadress-CIDR darf höchstens eine Größe von /12 aufweisen. |
| Kubernetes-DNS-Dienst – IP-Adresse | Die IP-Adresse im Kubernetes-Dienstadressbereich wird bei der Clusterdienstermittlung (kube-dns) verwendet. Verwenden Sie nicht die erste IP-Adresse Ihres Adressbereichs, z.B. „.1“. Die erste Adresse Ihres Subnetzbereichs wird für die Adresse *kubernetes.default.svc.cluster.local* genutzt. |
| Docker-Bridge-Adresse | Die IP-Adresse (in CIDR-Schreibweise) wird auf Knoten als Docker-Bridge-Adresse verwendet. Standard 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Maximale Pods pro Knoten

Die maximale Anzahl von Pods pro Knoten in einem AKS-Cluster beträgt 110. *Standardmäßig* variiert die maximale Anzahl von Pods pro Knoten zwischen einfachen und erweiterten Netzwerken sowie je nach Methode für die Clusterbereitstellung.

| Bereitstellungsmethode | Standard für grundlegende Netzwerke | Standard für erweiterte Netzwerke | Bei der Bereitstellung konfigurierbar |
| -- | :--: | :--: | -- |
| Azure-Befehlszeilenschnittstelle | 110 | 30 | Ja (bis zu 110) |
| Resource Manager-Vorlage | 110 | 30 | Ja (bis zu 110) |
| Portal | 110 | 30 | Nein  |

### <a name="configure-maximum---new-clusters"></a>Konfigurieren des Höchstwerts: Neue Cluster

Sie können die maximale Anzahl von Pods pro Knoten *nur zum Zeitpunkt der Bereitstellung* konfigurieren. Wenn Sie die Bereitstellung mit der Azure-Befehlszeilenschnittstelle oder mit einer Resource Manager-Vorlage durchführen, können Sie die maximale Anzahl von Pods pro Knoten auf bis zu 110 festlegen.

* **Azure CLI**: Geben Sie das `--max-pods`-Argument an, wenn Sie einen Cluster mit dem Befehl [az aks create][az-aks-create] bereitstellen. Der Maximalwert ist 110.
* **Resource Manager-Vorlage**: Geben Sie die `maxPods`-Eigenschaft im Objekt [ManagedClusterAgentPoolProfile] an, wenn Sie einen Cluster mit einer Resource Manager-Vorlage bereitstellen. Der Maximalwert ist 110.
* **Azure-Portal**: Sie können die maximale Anzahl von Pods pro Knoten nicht ändern, wenn Sie einen Cluster über das Azure-Portal bereitstellen. Die Cluster in erweiterten Netzwerken sind, wenn die Bereitstellung über das Azure-Portal erfolgt, auf 30 Pods pro Knoten beschränkt.

### <a name="configure-maximum---existing-clusters"></a>Konfigurieren des Höchstwerts: Vorhandene Cluster

Sie können die maximalen Anzahl von Pods pro Knoten für einen vorhandenen AKS-Cluster nicht ändern. Sie können die Anzahl nur bei der ersten Bereitstellung des Clusters anpassen.

## <a name="deployment-parameters"></a>Bereitstellungsparameter

Beim Erstellen eines AKS-Clusters können folgende Parameter für erweiterte Netzwerke konfiguriert werden:

**Virtuelles Netzwerk**: Das virtuelle Netzwerk, in dem Sie den Kubernetes-Cluster bereitstellen möchten. Wenn Sie ein neues virtuelles Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines virtuellen Netzwerks* ausführen. Weitere Informationen zu Grenzwerten und Kontingenten für virtuelle Azure-Netzwerke finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subnetz**: Das Subnetz im virtuellen Netzwerk, in dem Sie den Cluster bereitstellen möchten. Wenn Sie ein neues Subnetz im virtuellen Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines Subnetzes* ausführen. Bei Hybridkonnektivität sollte sich der Adressbereich nicht mit anderen virtuellen Netzwerken in Ihrer Umgebung überschneiden.

**Kubernetes-Dienstadressbereich**: Dies ist der Satz mit den virtuellen IP-Adressen, die von Kubernetes den [Diensten][services] in Ihrem Cluster zugewiesen werden. Sie können jeden privaten Adressbereich verwenden, der die folgenden Anforderungen erfüllen:

* Darf nicht innerhalb des IP-Adressbereichs des virtuellen Netzwerk Ihres Clusters liegen
* Darf sich nicht mit anderen virtuellen Netzwerken überlappen, die Peers des virtuellen Netzwerks des Clusters sind
* Er darf sich nicht mit lokalen IP-Adressen überlappen.
* Er darf sich nicht in den Bereichen `169.254.0.0/16`, `172.30.0.0/16` oder `172.31.0.0/16` befinden.

Obwohl es technisch möglich ist, einen Dienstadressbereich im gleichen virtuellen Netzwerk wie Ihr Cluster anzugeben, wird dies nicht empfohlen. Bei Verwendung sich überlappender IP-Adressbereiche kann es zu unvorhersehbarem Verhalten kommen. Weitere Informationen finden Sie im Abschnitt [Häufig gestellte Fragen](#frequently-asked-questions) in diesem Artikel. Weitere Informationen zu Kubernetes-Diensten finden Sie in der Kubernetes-Dokumentation unter [Dienste][services].

**Kubernetes-DNS-Dienst – IP-Adresse**:  Die IP-Adresse für den DNS-Dienst des Clusters. Diese Adresse muss innerhalb des *Kubernetes-Dienstadressbereichs* liegen. Verwenden Sie nicht die erste IP-Adresse Ihres Adressbereichs, z.B. „.1“. Die erste Adresse Ihres Subnetzbereichs wird für die Adresse *kubernetes.default.svc.cluster.local* genutzt.

**Docker-Bridge-Adresse**: Die IP-Adresse und Netzmaske, die der Docker-Bridge zugewiesen werden. Diese IP-Adresse darf nicht innerhalb des IP-Adressbereichs des virtuellen Netzwerk Ihres Clusters liegen.

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

- [Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)](static-ip.md)
- [Verwenden eines internen Lastenausgleichs mit Azure Container Service (AKS)](internal-lb.md)

- [Erstellen eines einfachen Eingangscontrollers mit Verbindung mit einem externen Netzwerk][aks-ingress-basic]
- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
- [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
- [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]

### <a name="aks-engine"></a>AKS Engine

[Azure Kubernetes Service Engine (AKS Engine)][aks-engine] ist ein Open-Source-Projekt, über das Azure Resource Manager-Vorlagen generiert werden, die Sie zum Bereitstellen von Docker-fähigen Clustern in Azure verwenden können. Kubernetes-, DC/OS-, Swarm-Modus- und Swarm-Orchestratoren können mit der AKS Engine bereitgestellt werden.

Für per AKS Engine erstellte Kubernetes-Cluster werden sowohl [kubenet][kubenet]- als auch [Azure CNI][cni-networking]-Plug-Ins unterstützt. Für die AKS Engine werden also die Netzwerkszenarien „Basic“ (Grundlegend) und „Advanced“ (Erweitert) unterstützt.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md

---
title: Netzwerkkonfiguration in Azure Kubernetes Service (AKS)
description: Enthält Informationen zur grundlegenden und erweiterten Netzwerkkonfiguration in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/07/2018
ms.author: marsma
ms.openlocfilehash: 80d12d1f5d6b388c46ed90eb84b7bc00250e17ff
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Netzwerkkonfiguration in Azure Kubernetes Service (AKS)

Beim Erstellen eines AKS-Clusters (Azure Kubernetes Service) können Sie zwischen zwei Netzwerkoptionen wählen: **Basic** (Grundlegend) und **Advanced** (Erweitert).

## <a name="basic-networking"></a>Grundlegende Netzwerke

Die Netzwerkoption **Basic** (Grundlegend) ist die Standardkonfiguration für die AKS-Clustererstellung. Die Netzwerkkonfiguration des Clusters und der dazugehörigen Pods wird vollständig über Azure verwaltet und eignet sich für Bereitstellungen, für die keine benutzerdefinierte VNET-Konfiguration erforderlich ist. Bei Auswahl von „Basic“ (Grundlegend) haben Sie keine Kontrolle über die Netzwerkkonfiguration, z.B. Subnetze oder die dem Cluster zugewiesenen IP-Adressbereiche.

Für Knoten in einem AKS-Cluster, für den die Netzwerkoption „Basic“ (Grundlegend) konfiguriert ist, wird das Kubernetes-Plug-In [kubenet][kubenet] verwendet.

## <a name="advanced-networking"></a>Erweiterte Netzwerke

Bei der Netzwerkoption **Advanced** (Erweitert) werden Ihre Pods in einem virtuellen Azure-Netzwerk (VNET) angeordnet. Sie konfigurieren dieses Netzwerk, um für die Pods die automatische Konnektivität mit VNET-Ressourcen herzustellen und die Integration in die umfassenden Funktionen von VNETs zu ermöglichen.
Die Netzwerkoption „Advanced“ (Erweitert) ist derzeit nur verfügbar, wenn AKS-Cluster im [Azure-Portal][portal] oder mit einer Resource Manager-Vorlage bereitgestellt werden.

Für Knoten in einem AKS-Cluster, für das die Netzwerkoption „Advanced“ (Erweitert) konfiguriert ist, wird das Kubernetes-Plug-In [Azure Container Networking Interface (CNI)][cni-networking] verwendet.

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Features der Netzwerkoption „Advanced“ (Erweitert)

Die Netzwerkoption „Advanced“ (Erweitert) bietet die folgenden Vorteile:

* Stellen Sie Ihren AKS-Cluster in einem vorhandenen VNET bereit, oder erstellen Sie ein neues VNET und Subnetz für Ihren Cluster.
* Jedem Pod im Cluster wird eine IP-Adresse im VNET zugewiesen, und es ist jeweils eine direkte Kommunikation mit anderen Pods im Cluster und anderen VMs im VNET möglich.
* Ein Pod kann eine Verbindung mit anderen Diensten eines mittels Peering verbundenen VNET und mit lokalen Netzwerken über ExpressRoute- und Site-to-Site-VPN-Verbindungen (S2S) herstellen. Pods sind auch über die lokale Umgebung erreichbar.
* Machen Sie einen Kubernetes-Dienst extern oder intern über den Azure Load Balancer verfügbar. Dies ist auch ein Feature der Netzwerkoption „Basic“ (Grundlegend).
* Pods in einem Subnetz mit aktivierten Dienstendpunkten können eine sichere Verbindung mit Azure-Diensten herstellen, z.B. Azure Storage und SQL-Datenbank.
* Verwenden Sie benutzerdefinierte Routen (UDRs) zum Weiterleiten von Datenverkehr von Pods an eine virtuelle Netzwerkappliance.
* Pods können auf Ressourcen im öffentlichen Internet zugreifen. Dies ist auch ein Feature der Netzwerkoption „Basic“ (Grundlegend).

> [!IMPORTANT]
> Jeder Knoten in einem AKS-Cluster, für den die Netzwerkoption „Advanced“ (Erweitert) konfiguriert ist, kann maximal **30 Pods** hosten. Jedes VNET, das für die Verwendung mit dem Azure CNI-Plug-In bereitgestellt wird, ist auf **4096 IP-Adressen** (/20) begrenzt.

## <a name="configure-advanced-networking"></a>Konfigurieren der Netzwerkoption „Advanced“ (Erweitert)

Beim [Erstellen eines AKS-Clusters](kubernetes-walkthrough-portal.md) im Azure-Portal können für die Netzwerkoption „Advanced“ (Erweitert) die folgenden Parameter konfiguriert werden:

**Virtuelles Netzwerk**: Das VNET, in dem Sie den Kubernetes-Cluster bereitstellen möchten. Wenn Sie ein neues VNET für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines virtuellen Netzwerks* ausführen.

**Subnetz**: Das Subnetz im VNET, in dem Sie den Cluster bereitstellen möchten. Wenn Sie ein neues Subnetz im VNET für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines Subnetzes* ausführen.

**Kubernetes service address range** (Kubernetes-Dienstadressbereich): Der IP-Adressbereich für die Dienst-IPs des Kubernetes-Clusters. Dieser Bereich darf nicht innerhalb des VNET-IP-Adressbereichs Ihres Clusters liegen.

**Kubernetes DNS service IP address** (Kubernetes-DNS-Dienst – IP-Adresse): Die IP-Adresse für den DNS-Dienst des Clusters. Diese Adresse muss innerhalb des *Kubernetes-Dienstadressbereichs* liegen.

**Docker Bridge address** (Docker-Bridge-Adresse): Die IP-Adresse und Netzmaske für die Zuweisung zur Docker-Bridge. Diese IP-Adresse darf nicht innerhalb des VNET-IP-Adressbereichs Ihres Clusters liegen.

Im folgenden Screenshot des Azure-Portals ist ein Beispiel für die Konfiguration dieser Einstellungen während der AKS-Clustererstellung dargestellt:

![Erweiterte Netzwerkkonfiguration im Azure-Portal][portal-01-networking-advanced]

## <a name="plan-ip-addressing-for-your-cluster"></a>Planen der IP-Adressierung für Ihren Cluster

Für Cluster, für die die Netzwerkoption „Advanced“ (Erweitert) konfiguriert wird, ist eine zusätzliche Planung erforderlich. Die Größe Ihres VNET und des dazugehörigen Subnetzes muss die Anzahl von Pods, die im Cluster gleichzeitig ausgeführt werden sollen, und Ihre Skalierungsanforderungen abdecken.

IP-Adressen für die Pods und die Knoten des Clusters werden über das angegebene Subnetz im VNET zugewiesen. Für jeden Knoten werden eine primäre IP-Adresse (die eigentliche IP-Adresse des Knotens) und 30 zusätzliche IP-Adressen konfiguriert (per Azure CNI vorkonfiguriert), die den für den Knoten bestimmten Pods zugewiesen werden. Wenn Sie Ihren Cluster horizontal hochskalieren, wird jeder Knoten auf ähnliche Weise mit IP-Adressen aus dem Subnetz konfiguriert.

Wie bereits erwähnt, ist jedes VNET, das für die Verwendung mit dem Azure CNI-Plug-In bereitgestellt wird, auf **4096 IP-Adressen** (/20) begrenzt. Jeder Knoten in einem Cluster, für den die Netzwerkoption „Advanced“ (Erweitert) konfiguriert ist, kann maximal **30 Pods** hosten.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Die folgenden Fragen und Antworten gelten für die Netzwerkkonfiguration vom Typ **Advanced** (Erweitert).

* *Kann ich die Netzwerkoption „Advanced“ (Erweitert) mit der Azure CLI konfigurieren?*

  Nein. Die Netzwerkoption „Advanced“ (Erweitert) ist derzeit nur verfügbar, wenn AKS-Cluster im Azure-Portal oder mit einer Resource Manager-Vorlage bereitgestellt werden.

* *Kann ich VMs in meinem Clustersubnetz bereitstellen?*

  Nein. Die Bereitstellung von VMs in dem Subnetz, das von Ihrem Kubernetes-Cluster verwendet wird, wird nicht unterstützt. VMs können in demselben VNET bereitgestellt werden, aber es muss ein anderes Subnetz verwendet werden.

* *Kann ich Netzwerkrichtlinien pro Pod konfigurieren?

  Nein. Netzwerkrichtlinien pro Pod werden derzeit nicht unterstützt.

* *Ist die maximale Anzahl von Pods, die auf einem Knoten bereitgestellt werden können, konfigurierbar?*

  Standardmäßig können auf jedem Knoten maximal 30 Pods gehostet werden. Sie können den maximalen Wert derzeit nur ändern, indem Sie beim Bereitstellen eines Clusters mit einer Resource Manager-Vorlage die `maxPods`-Eigenschaft ändern.

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
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[aks-ssh]: aks-ssh.md

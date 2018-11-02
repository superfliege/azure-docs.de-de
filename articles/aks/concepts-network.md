---
title: Konzepte – Netzwerke in Azure Kubernetes Service (AKS)
description: Lernen Sie Netzwerke in Azure Kubernetes Service (AKS) kennen, einschließlich grundlegende und erweiterte Netzwerke, Eingangscontroller, Lastenausgleichsmodule und statische IP-Adressen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 62ba98f221041d5bbf9bb095a02d052218eb0fd0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49380655"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)

In einem containergestützten Microservice-Ansatz für die Anwendungsentwicklung müssen die Anwendungskomponenten zusammenarbeiten, um ihre Aufgaben zu verarbeiten. Kubernetes stellt verschiedene Ressourcen bereit, die diese Anwendungskommunikation ermöglichen. Sie können eine Verbindung mit Anwendungen herstellen und diese intern oder extern verfügbar machen. Zum Erstellen hochverfügbarer Anwendungen können Sie für Ihre Anwendungen einen Lastenausgleich vornehmen. Bei komplexeren Anwendungen ist möglicherweise die Konfiguration des eingehenden Datenverkehrs für die SSL/TLS-Terminierung oder ein Weiterleiten mehrerer Komponenten erforderlich. Aus Sicherheitsgründen müssen Sie möglicherweise auch den Netzwerkdatenverkehrsfluss in oder zwischen Pods und Knoten beschränken.

In diesem Artikel werden die wichtigsten Konzepte vorgestellt, mit denen Sie Netzwerke für Ihre Anwendungen in AKS bereitstellen:

- [Dienste](#services)
- [Virtuelle Azure-Netzwerke](#azure-virtual-networks)
- [Eingangscontroller](#ingress-controllers)
- [Netzwerkrichtlinien](#network-policies)

## <a name="kubernetes-basics"></a>Kubernetes-Grundlagen

Um den Zugriff auf Ihre Anwendungen oder die gegenseitige Kommunikation von Anwendungskomponenten zu erlauben, stellt Kubernetes eine Abstraktionsschicht für virtuelle Netzwerke bereit. Kubernetes-Knoten sind mit einem virtuellen Netzwerk verbunden und können eingehende und ausgehende Konnektivität für Pods bereitstellen. Die Komponente *kube-proxy*, die auf jedem Knoten ausgeführt wird, stellt diese Netzwerkfunktionen bereit.

In Kubernetes werden Pods von *Diensten* logisch gruppiert, um den direkten Zugriff über eine IP-Adresse oder einen DNS-Namen und an einem bestimmten Port zu erlauben. Sie können Datenverkehr auch über einen *Lastenausgleich* verteilen. Ein komplexeres Routing von Anwendungsdatenverkehr kann auch mit *Eingangscontrollern* erzielt werden. Die Sicherheit und das Filtern des Netzwerkdatenverkehrs für Pods kann mit Kubernetes-*Netzwerkrichtlinien* ermöglicht werden.

Außerdem trägt die Azure-Plattform zur Vereinfachung der virtuellen Netzwerke für AKS-Cluster bei. Wenn Sie einen Kubernetes-Lastenausgleich erstellen, wird die zugrunde liegende Azure Load Balancer-Ressource erstellt und konfiguriert. Wenn Sie Netzwerkports für Pods öffnen, werden die entsprechenden Regeln für Azure-Netzwerksicherheitsgruppen konfiguriert. Für das HTTP-Anwendungsrouting kann Azure auch *externes DNS* konfigurieren, wenn neue Eingangsrouten konfiguriert werden.

## <a name="services"></a>Dienste

Kubernetes verwendet *Dienste*, um die Netzwerkkonfiguration für Anwendungsworkloads zu vereinfachen. Dabei werden eine Reihe von Pods logisch miteinander gruppiert und Netzwerkkonnektivität bereitgestellt. Folgende Arten von Diensten sind verfügbar:

- **Cluster IP**: Erstellt eine interne IP-Adresse zur Verwendung innerhalb des AKS-Clusters. Ideal für rein interne Anwendungen, die andere Workloads im Cluster unterstützen.

    ![Diagramm mit Cluster IP-Datenverkehrsfluss in einem AKS-Cluster][aks-clusterip]

- **NodePort**: Erstellt eine Port-Zuordnung auf dem zugrunde liegenden Knoten, über die mithilfe von Knoten-IP-Adresse und Port der direkte Zugriff auf die Anwendung erfolgen kann.

    ![Diagramm mit NodePort-Datenverkehrsfluss in einem AKS-Cluster][aks-nodeport]

- **LoadBalancer**: Erstellt eine Azure Load Balancer-Ressource, konfiguriert eine externe IP-Adresse und verbindet die angeforderten Pods mit dem Back-End-Pool des Load Balancer. An den gewünschten Ports werden Regeln für den Lastenausgleich erstellt, damit der Datenverkehr der Kunden die Anwendung erreichen kann. 

    ![Diagramm mit Load Balancer-Datenverkehrsfluss in einem AKS-Cluster][aks-loadbalancer]

    Als zusätzliche Steuerung und für das Routing des eingehenden Datenverkehrs können Sie auch einen [Eingangscontroller](#ingress-controllers) verwenden.

- **ExternalName**: Erstellt einen bestimmten DNS-Eintrag für einen einfacheren Anwendungszugriff.

Die IP-Adresse für Lastenausgleichsmodule und Dienste kann dynamisch zugewiesen werden. Sie können aber auch eine vorhandene statische IP-Adresse angeben. Zugewiesen werden können sowohl interne als auch externe statische IP-Adressen. Diese vorhandene statische IP-Adresse ist häufig an einen DNS-Eintrag gebunden.

Es können sowohl *interne* als auch *externe* Lastenausgleichsmodule erstellt werden. Internen Lastenausgleichsmodulen wird nur eine private IP-Adresse zugeordnet. Daher kann über das Internet nicht darauf zugegriffen werden.

## <a name="azure-virtual-networks"></a>Virtuelle Azure-Netzwerke

In AKS können Sie einen Cluster bereitstellen, der eines der beiden folgenden Netzwerkmodelle verwendet:

- *Grundlegende* Netzwerke: Die Netzwerkressourcen werden bei der Bereitstellung des AKS-Clusters erstellt und konfiguriert.
- *Erweiterte* Netzwerke: Der AKS-Cluster wird mit vorhandenen virtuellen Netzwerkressourcen und -konfigurationen verbunden.

### <a name="basic-networking"></a>Grundlegende Netzwerke

Die Netzwerkoption *Basic* (Grundlegend) ist die Standardkonfiguration für die AKS-Clustererstellung. Die Azure-Plattform verwaltet die Netzwerkkonfiguration von Cluster und Pods. Grundlegende Netzwerke eignen sich für Bereitstellungen, die keine benutzerdefinierte virtuelle Netzwerkkonfiguration erfordern. Bei grundlegenden Netzwerken können Sie keine Netzwerkkonfiguration (z. B. Subnetznamen oder dem AKS-Cluster zugewiesene IP-Adressbereiche) definieren.

Für Knoten in einem AKS-Cluster, für das die Netzwerkoption „Basic“ (Grundlegend) konfiguriert ist, wird das Kubernetes-Plug-In [kubenet][kubenet] verwendet.

Grundlegende Netzwerke verfügen über folgende Funktionen:

- Machen Sie einen Kubernetes-Dienst extern oder intern über den Azure Load Balancer verfügbar.
- Pods können auf Ressourcen im öffentlichen Internet zugreifen.

### <a name="advanced-networking"></a>Erweiterte Netzwerke

Bei Auswahl der Netzwerkoption *Advanced* (Erweitert) werden Ihre Pods in einem virtuellen Azure-Netzwerk angeordnet, das Sie konfigurieren. Dieses virtuelle Netzwerk stellt eine automatische Verbindung mit anderen Azure-Ressourcen und die Integration in einen umfangreichen Funktionssatz bereit. Erweiterte Netzwerke eignen sich für Bereitstellungen, die bestimmte virtuelle Netzwerkkonfigurationen erfordern (z. B. Verwendung eines bestehenden Subnetzes und vorhandener Konnektivität). Bei erweiterten Netzwerken können Sie Subnetznamen und IP-Adressbereiche definieren.

Für Knoten in einem AKS-Cluster, für das die Netzwerkoption „Advanced“ (Erweitert) konfiguriert ist, wird das Kubernetes-Plug-In [Azure Container Networking Interface (CNI)][cni-networking] verwendet.

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET][advanced-networking-diagram]

Erweiterte Netzwerke verfügen gegenüber grundlegenden Netzwerken über folgende zusätzliche Funktionen:

- Sie können Ihren AKS-Cluster in einem vorhandenen virtuellen Azure-Netzwerk bereitstellen oder ein neues virtuelles Netzwerk und Subnetz für Ihren Cluster erstellen.
- Jedem Pod im Cluster wird eine IP-Adresse im virtuellen Netzwerk zugewiesen. Die Pods können direkt mit anderen Pods im Cluster und mit anderen Knoten im virtuellen Netzwerk kommunizieren.
- Ein Pod kann eine Verbindung mit anderen Diensten in einem mittels Peering verknüpften virtuellen Netzwerk herstellen. Dazu zählen auch Verbindungen mit lokalen Netzwerken über ExpressRoute- und Site-to-Site-VPN-Verbindungen (S2S). Pods sind auch über die lokale Umgebung erreichbar.
- Pods in einem Subnetz mit aktivierten Dienstendpunkten können eine sichere Verbindung mit Azure-Diensten (z. B. Azure Storage und SQL-Datenbank) herstellen.
- Sie können benutzerdefinierte Routen (UDRs) zum Weiterleiten von Datenverkehr von Pods an virtuelle Netzwerkgeräte erstellen.

Weitere Informationen finden Sie unter [Konfigurieren erweiterter Netzwerke für AKS-Cluster][aks-configure-advanced-networking].

## <a name="ingress-controllers"></a>Eingangscontroller

Wenn Sie einen Dienst des Typs "LoadBalancer" erstellen, wird eine zugrunde liegende Azure Load Balancer-Ressource erstellt. Der Load Balancer wird zum Verteilen von Datenverkehr an die Pods in Ihrem Dienst an einem bestimmten Port konfiguriert. Der Load Balancer arbeitet nur auf Schicht 4. Der Dienst hat keine Kenntnis von den tatsächlichen Anwendungen und kann keine zusätzlichen Überlegungen zum Netzwerkrouting vornehmen.

*Eingangscontroller* fungieren auf Schicht 7 und können intelligentere Regeln zum Verteilen des Anwendungsdatenverkehrs verwenden. Die übliche Aufgabe eines Eingangscontroller ist das Routing von HTTP-Datenverkehr an verschiedene Anwendungen anhand der eingehenden URL.

![Diagramm mit Eingangsdatenverkehrsfluss in einem AKS-Cluster][aks-ingress]

In AKS können Sie mit NGINX (oder ähnlich) eine Dateneingangsressource erstellen oder die AKS-Funktion für das HTTP-Anwendungsrouting verwenden. Wenn Sie das HTTP-Anwendungsrouting für einen AKS-Cluster aktivieren, erstellt die Azure-Plattform den Eingangscontroller und einen *externen DNS-Controller*. Wenn in Kubernetes neue Eingangsressourcen erstellt werden, werden in einer clusterspezifischen DNS-Zone die erforderlichen DNS-A-Einträge erstellt. Weitere Informationen finden Sie unter [HTTP-Anwendungsrouting][aks-http-routing].

Ein weiteres allgemeines Feature des Dateneingangs ist die SSL/TLS-Terminierung. Bei großen Webanwendungen, auf die über HTTPS zugegriffen wird, kann die TLS-Terminierung durch die Eingangsressource erfolgen und braucht nicht innerhalb der Anwendung verarbeitet zu werden. Um die automatische Generierung und Konfiguration der TLS-Zertifizierung bereitzustellen, können Sie die Eingangsressource für die Verwendung von Anbietern wie Let's Encrypt konfigurieren. Weitere Informationen zum Konfigurieren eines NGINX-Eingangscontrollers mit Let's Encrypt finden Sie unter [Eingang und TLS][aks-ingress-tls].

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Eine Netzwerksicherheitsgruppe filtert Datenverkehr für virtuelle Computer wie beispielsweise die AKS-Knoten. Beim Erstellen von Diensten (z. B. LoadBalancer) konfiguriert die Azure-Plattform automatisch alle erforderlichen Netzwerksicherheitsgruppen-Regeln. Sie sollten keine Netzwerksicherheitsgruppen-Regeln manuell konfigurieren, um Datenverkehr für Pods in einem AKS-Cluster zu filtern. Definieren Sie alle erforderlichen Ports und die Weiterleitung im Rahmen Ihrer Kubernetes-Dienstmanifeste, und überlassen Sie das Erstellen oder Aktualisieren der entsprechenden Regeln der Azure-Plattform.

Es gibt standardmäßige Netzwerksicherheitsgruppen-Regeln für Datenverkehr wie beispielsweise SSH. Diese Standardregeln sind für den Zugriff auf die Clusterverwaltung und Problembehandlung bestimmt. Das Löschen dieser Standardregeln kann Probleme mit der AKS-Verwaltung und eine Nichteinhaltung des Servicelevelziels (SLO) verursachen.

## <a name="next-steps"></a>Nächste Schritte

Die ersten Schritte mit AKS-Netzwerken sind unter [Erstellen und Konfigurieren erweiterter Netzwerke für einen AKS-Cluster][aks-configure-advanced-networking] beschrieben.

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Zugriff und Identität][aks-concepts-identity]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-advanced-networking]: configure-advanced-networking.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
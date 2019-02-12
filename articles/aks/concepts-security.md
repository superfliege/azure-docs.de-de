---
title: Konzepte – Sicherheit in Azure Kubernetes Service (AKS)
description: Erfahren Sie mehr über die Sicherheit in Azure Kubernetes Service (AKS), einschließlich Master- und Knoten-Kommunikation, Netzwerkrichtlinien und Kubernetes-Geheimnisse.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 2c6569d92913a3cff9ee51529dd381386ed2a792
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55818990"
---
# <a name="security-concepts-for-applications-and-clusters-in-azure-kubernetes-service-aks"></a>Sicherheitskonzepte für Anwendungen und Cluster in Azure Kubernetes Service (AKS)

Damit Ihre Kundendaten während der Ausführung von Anwendungsworkloads in Azure Kubernetes Service (AKS) geschützt werden, kommt der Sicherheit des Clusters eine besondere Bedeutung zu. Kubernetes enthält Sicherheitskomponenten wie *Netzwerkrichtlinien* und *Geheimnisse*. Durch Azure kommen Komponenten wie Netzwerksicherheitsgruppen und orchestrierte Clusterupgrades hinzu. Diese Sicherheitskomponenten werden dann so miteinander kombiniert, dass auf dem AKS-Cluster jederzeit die aktuellen Betriebssystem-Sicherheitsupdates und Kubernetes-Versionen ausgeführt werden, der Datenverkehr zwischen den Pods geschützt und der Zugriff auf sensible Anmeldeinformationen gesichert ist.

In diesem Artikel werden die wichtigsten Konzepte vorgestellt, mit denen Sie Anwendungen in AKS schützen können:

- [Sicherheit der Masterkomponenten](#master-security)
- [Knotensicherheit](#node-security)
- [Clusterupgrades](#cluster-upgrades)
- [Netzwerksicherheit](#network-security)
- Kubernetes-Geheimnisse

## <a name="master-security"></a>Sicherheit der Masterkomponenten

In AKS sind die Kubernetes-Masterkomponenten Bestandteil des verwalteten Diensts, der von Microsoft bereitgestellt wird. Jeder AKS-Cluster verfügt über seinen eigenen dedizierten Kubernetes-Master mit einem einzelnen Mandanten, um API-Server, Scheduler usw. bereitzustellen. Dieser Master wird von Microsoft verwaltet und gepflegt.

Standardmäßig verwendet der Kubernetes-API-Server eine öffentliche IP-Adresse mit vollqualifiziertem Domänennamen (FQDN). Mithilfe der rollenbasierten Zugriffssteuerung von Kubernetes und Azure Active Directory können Sie den Zugriff auf den API-Server steuern. Weitere Informationen finden Sie unter [Azure AD-Integration mit AKS][aks-aad].

## <a name="node-security"></a>Knotensicherheit

AKS-Knoten sind virtuelle Azure-Computer, die von Ihnen verwaltet und gepflegt werden. Auf den Knoten wird eine optimierte Ubuntu-Linux-Distribution mit der Docker Container Runtime ausgeführt. Wenn ein AKS-Cluster erstellt oder zentral hochskaliert wird, werden die Knoten automatisch mit den aktuellen Betriebssystem-Sicherheitsupdates und -konfigurationen bereitgestellt.

Die Azure-Plattform wendet über Nacht automatisch Betriebssystem-Sicherheitspatches auf die Knoten an. Wenn ein Betriebssystem-Sicherheitsupdate einen Neustart des Hosts erfordert, wird dieser Neustart nicht automatisch vorgenommen. Sie können die Knoten manuell neu starten. Eine andere gängige Methode ist die Verwendung von [Kured][kured], einem Open-Source-Neustartdaemon für Kubernetes. Kured wird als [DaemonSet][aks-daemonset] ausgeführt und überwacht jeden Knoten auf das Vorhandensein einer Datei, die angibt, dass ein Neustart erforderlich ist. Neustarts werden clusterübergreifend verwaltet, wobei derselbe [Vorgang des Absperrens und Ausgleichens](#cordon-and-drain) wie bei einem Clusterupgrade angewendet wird.

Knoten werden in einem Subnetz des privaten virtuellen Netzwerks ohne öffentliche IP-Adresse bereitgestellt. Zur Problembehandlung und Verwaltung ist SSH standardmäßig aktiviert. Dieser SSH-Zugriff ist nur über die interne IP-Adresse verfügbar. Mit den Netzwerksicherheitsgruppen-Regeln von Azure können Sie den Zugriff im IP-Adressbereich weiter auf die AKS-Knoten einschränken. Wenn Sie die standardmäßige SSH-Netzwerksicherheitsgruppen-Regel löschen und den SSH-Dienst auf den Knoten deaktivieren, kann die Azure-Plattform keine Wartungsaufgaben mehr ausführen.

Die Knoten verwenden Azure Managed Disks, um Speicher bereitzustellen. Bei den meisten VM-Knotengrößen handelt es sich um Premium-Datenträger, die von Hochleistungs-SSDs unterstützt werden. Die auf verwalteten Datenträgern gespeicherten Daten werden im Ruhezustand auf der Azure-Plattform automatisch verschlüsselt. Zur Verbesserung der Redundanz werden diese Datenträger außerdem sicher im Azure-Rechenzentrum repliziert.

Kubernetes-Umgebungen, ob in AKS oder an anderer Stelle, sind derzeit nicht völlig sicher vor feindlicher Verwendung mit mehreren Mandanten. Zusätzliche Sicherheitsfunktionen wie *Pod Security Policies* oder differenziertere rollenbasierte Zugriffssteuerung (RBAC) für Knoten erschweren Angriffe. Für echte Sicherheit bei der Ausführung feindlicher Workloads mit mehreren Mandanten ist jedoch ein Hypervisor die einzige Sicherheitsstufe, der Sie vertrauen sollten. Die Sicherheitsdomäne für Kubernetes wird zum gesamten Cluster und nicht zu einem einzelnen Knoten. Für diese Art von feindlichen Workloads mit mehreren Mandanten sollten Sie physisch isolierte Cluster verwenden. Weitere Informationen zu Möglichkeiten zur Isolierung von Workloads finden Sie unter [Bewährte Methoden für die Isolierung der Cluster in AKS][cluster-isolation],

## <a name="cluster-upgrades"></a>Clusterupgrades

Aus Sicherheits- und Compliancegründen (oder zur Verwendung der neuesten Features) stellt Azure Tools bereit, um das Upgrade eines AKS-Clusters und seiner Komponenten zu orchestrieren. Diese Upgradeorchestrierung umfasst sowohl die Kubernetes-Master- als auch die Agent-Komponenten. Sie können eine Liste der verfügbaren Kubernetes-Versionen für Ihren AKS-Cluster anzeigen. Um den Upgradevorgang zu starten, geben Sie eine dieser verfügbaren Versionen an. Azure verwendet dann für jeden AKS-Knoten den sicheren Vorgang des Absperrens und Ausgleichens und führt das Upgrade aus.

### <a name="cordon-and-drain"></a>Absperren und Ausgleichen

Während des Upgrades werden AKS-Knoten einzeln vom Cluster abgesperrt, damit auf ihnen keine neuen Pods geplant werden. Die Knoten werden dann wie folgt ausgeglichenund aktualisiert:

- Vorhandene Pods werden ordnungsgemäß beendet und auf den verbleibenden Knoten geplant.
- Der Knoten wird neu gestartet, und der Upgradevorgang wird abgeschlossen. Anschließend wird der Knoten wieder mit dem AKS-Cluster verknüpft.
- Pods werden wieder für die Ausführung auf dem Knoten geplant.
- Der nächste Knoten im Cluster wird mit der gleichen Methode abgesperrt und ausgeglichen usw., bis alle Knoten erfolgreich aktualisiert wurden.

Weitere Informationen finden Sie unter [Aktualisieren eines AKS-Clusters][aks-upgrade-cluster].

## <a name="network-security"></a>Netzwerksicherheit

Für Konnektivität und Sicherheit bei lokalen Netzwerken können Sie Ihren AKS-Cluster in Subnetzen eines vorhandenen virtuellen Azure-Netzwerks bereitstellen. Diese virtuellen Netzwerke können über ein Azure-Site-to-Site-VPN oder eine Expressroute-Verbindung zurück zu Ihrem lokalen Netzwerk verfügen. Kubernetes-Eingangscontroller können mit privaten, internen IP-Adressen definiert werden, damit die Dienste nur über diese interne Netzwerkverbindung zugänglich sind.

### <a name="azure-network-security-groups"></a>Azure-Netzwerksicherheitsgruppen

Zum Filtern des Datenverkehrsflusses in virtuellen Netzwerken verwendet Azure Netzwerksicherheitsgruppen-Regeln. Diese Regeln bestimmen die Quell- und Ziel-IP-Adressbereiche, Ports und Protokolle, die für den Zugriff auf Ressourcen zugelassen oder abgelehnt werden. Standardregeln werden erstellt, um den TLS-Datenverkehr zum Kubernetes-API-Server und den SSH-Zugriff auf die Knoten zu ermöglichen. Wenn Sie Dienste mit Lastenausgleich, Portzuordnungen oder Eingangsrouten erstellen, ändert AKS automatisch die Netzwerksicherheitsgruppe, damit der Datenverkehr entsprechend fließen kann.

## <a name="kubernetes-secrets"></a>Kubernetes-Geheimnisse

Ein Kubernetes-*Geheimnis* wird verwendet, um sensible Daten wie Anmeldeinformationen oder Schlüssel in Pods einzufügen. Zuerst erstellen Sie ein Geheimnis mit der Kubernetes-API. Wenn Sie Ihren Pod oder die Bereitstellung definieren, kann ein bestimmtes Geheimnis angefordert werden. Geheimnisse werden nur für Knoten bereitgestellt, die über einen eingeplanten Pod verfügen, der es benötigt, und das Geheimnis wird in *tmpfs* gespeichert, nicht auf den Datenträger geschrieben. Wenn der letzte Pod auf einem Knoten gelöscht wird, der ein Geheimnis benötigt, wird das Geheimnis aus dem Verzeichnis „tmpfs“ des Knotens gelöscht. Geheimnisse werden in einem bestimmten Namespace gespeichert, und nur Pods im gleichen Namespace können darauf zugreifen.

Die Verwendung von Geheimnissen reduziert die vertraulichen Informationen, die im YAML-Manifest für den Pod oder den Dienst definiert werden. Stattdessen fordern Sie im Rahmen Ihres YAML-Manifests das im Kubernetes-API-Server gespeicherte Geheimnis an. Mit diesem Ansatz erhält nur der spezielle Pod Zugriff auf das Geheimnis.

## <a name="next-steps"></a>Nächste Schritte

Die ersten Schritte zum Sichern Ihrer AKS-Cluster sind unter [Aktualisieren eines AKS-Clusters][aks-upgrade-cluster] beschrieben.

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Identität][aks-concepts-identity]
- [Kubernetes/AKS: virtuelle Netzwerke][aks-concepts-network]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- LINKS - External -->
[kured]: https://github.com/weaveworks/kured
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/

<!-- LINKS - Internal -->
[aks-daemonsets]: concepts-clusters-workloads.md#daemonsets
[aks-upgrade-cluster]: upgrade-cluster.md
[aks-aad]: aad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[cluster-isolation]: operator-best-practices-cluster-isolation.md

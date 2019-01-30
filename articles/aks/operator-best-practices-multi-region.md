---
title: 'Best Practices für Clustervorgänge: Hochverfügbarkeit und Notfallwiederherstellung in Azure Kubernetes Service (AKS)'
description: Lernen Sie die Best Practices für Clustervorgänge kennen, mit denen Sie die Betriebsbereitschaft Ihrer Anwendungen maximieren und so für Hochverfügbarkeit sorgen und sich auf Situationen vorbereiten können, in denen eine Notfallwiederherstellung in Azure Kubernetes Service (AKS) erforderlich ist.
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 36e87550c90c1a5d4b05aa62f2fbe79b04b25445
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382446"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Best Practices für Geschäftskontinuität und Notfallwiederherstellung in Azure Kubernetes Service (AKS)

Bei der Verwaltung von Clustern in Azure Kubernetes Service (AKS) ist die Betriebsbereitschaft Ihrer Anwendungen von größter Bedeutung. AKS sorgt mithilfe von mehreren Knoten in einer Verfügbarkeitsgruppe für Hochverfügbarkeit. Diese Knoten bieten allerdings keinen Schutz beim Ausfall einer ganzen Region. Um die Betriebsbereitschaft zu maximieren, implementieren Sie Features für Geschäftskontinuität und Notfallwiederherstellung.

Dieser Artikel zu Best Practices in diesem Bereich stellt Überlegungen vor, die Sie bei der Planung der Geschäftskontinuität und Notfallwiederherstellung in AKS berücksichtigen sollten. Folgendes wird vermittelt:

> [!div class="checklist"]
* [Planen von AKS-Clustern in mehreren Regionen](#region-planning)
* [Weiterleiten des Datenverkehrs über mehrere Cluster hinweg mithilfe von Azure Traffic Manager](#ingress-traffic)
* [Verwenden der Georeplikation für Ihre Containerimageregistrierungen](#container-registry)
* [Planen des Anwendungszustands über mehrere Cluster hinweg](#managing-application-state)
* [Replizieren des Speichers über mehrere Regionen hinweg](#storage)

## <a name="plan-for-multi-region-deployment"></a>Planen der Bereitstellung in mehreren Regionen

**Best Practices-Leitfaden**: Wenn Sie mehrere AKS-Cluster bereitstellen, wählen Sie Regionen aus, in denen AKS verfügbar ist, und verwenden Sie Regionspaare.

Ein AKS-Cluster wird in einer einzelnen Region bereitgestellt. Um für Schutz beim Ausfall dieser Region zu sorgen, stellen Sie Ihre Anwendung in mehreren AKS-Clustern in verschiedenen Regionen bereit. Berücksichtigen Sie bei der Planung der Regionen, in denen Sie Ihre AKS-Cluster bereitstellen, die folgenden Überlegungen:

* [Verfügbarkeit der AKS-Regionen](https://docs.microsoft.com/azure/aks/container-service-quotas#region-availability)
  * Wählen Sie Regionen in der Nähe Ihrer Benutzer aus. AKS wird fortlaufend auf neue Regionen ausgeweitet.
* [Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
  * Wählen Sie zwei Regionen in Ihrem geografischen Bereich aus, die miteinander gekoppelt sind. Diese Regionen koordinieren Plattformupdates und priorisieren bei Bedarf Wiederherstellungsvorgänge.
* Ebenen der Dienstverfügbarkeit (heiß/heiß, heiß/warm, heiß/kalt)
  * Entscheiden Sie, ob Sie beide Regionen gleichzeitig betreiben möchten, sodass eine Region sofort zur Weiterleitung des Datenverkehrs *bereit* ist, oder ob eine Region eine gewisse Vorlaufzeit zur Verarbeitung von Datenverkehr benötigt.

AKS-Regionsverfügbarkeit und Regionspaare sollten zusammen geplant werden. Stellen Sie Ihre AKS-Cluster in Regionspaaren bereit, die für die gemeinsame Verwaltung der Notfallwiederherstellung konzipiert sind. Ein Beispiel: AKS ist in den Regionen *USA, Osten* und *USA, Westen* verfügbar. Diese Regionen sind auch gekoppelt. Daher empfiehlt sich die Nutzung dieser beiden Regionen, wenn Sie Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung von AKS entwickeln.

Wenn Sie Ihre Anwendung bereitstellen, müssen Sie auch Ihrer CI/CD-Pipeline einen zusätzlichen Schritt hinzufügen, um die Bereitstellung in diesen AKS-Clustern zu ermöglichen. Wenn Sie Ihre Bereitstellungspipelines nicht aktualisieren, können Anwendungsbereitstellungen nur in einer Ihrer Regionen und einem Ihrer AKS-Cluster erfolgen. Datenverkehr von Kunden, der an eine sekundäre Region weitergeleitet wird, erhält nicht die neusten Codeupdates.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Weiterleiten von Datenverkehr mit Azure Traffic Manager

**Best Practices-Leitfaden**: Azure Traffic Manager kann Kundendatenverkehr an den nächstgelegenen AKS-Cluster und die nächstgelegene Anwendungsinstanz weiterleiten. Um optimale Leistungs- und Redundanzwerte zu erzielen, leiten Sie den gesamten Anwendungsdatenverkehr über Traffic Manager an Ihren AKS-Cluster.

Wenn Sie mehrere AKS-Cluster in verschiedenen Regionen bereitgestellt haben, müssen Sie die Weiterleitung des Datenverkehrs an die in jedem Cluster ausgeführten Anwendungen steuern. [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) ist ein DNS-basierter Lastenausgleichsdienst, der Datenverkehr auf mehrere Regionen verteilen kann. Sie können Benutzerdatenverkehr basierend auf der Antwortzeit des Clusters oder der Geografie weiterleiten.

![AKS mit Azure Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Wenn nur ein einzelner AKS-Cluster vorhanden ist, stellen Kunden in der Regel eine Verbindung mit der *Dienst-IP-Adresse* oder dem DNS-Namen einer bestimmten Anwendung her. In einer Bereitstellung mit mehreren Clustern sollten Kunden eine Verbindung mit dem DNS-Namen der Traffic Manager-Instanz herstellen, die auf die Dienste in den einzelnen AKS-Clustern verweist. Diese Dienste werden mithilfe von Traffic Manager-Endpunkten definiert. Jeder Endpunkt stellt die *IP-Adresse des Lastenausgleichsdiensts* dar. Mit dieser Konfiguration können Sie Netzwerkdatenverkehr vom Traffic Manager-Endpunkt in einer Region zu einem Endpunkt in einer anderen Region leiten.

![Geografisches Routing mit Azure Traffic Manager](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Mithilfe von Traffic Manager werden DNS-Lookupvorgänge ausgeführt und der am besten geeignete Endpunkt für einen Benutzer zurückgegeben. Es können geschachtelte Profile verwendet werden, wobei einem primären Standort Priorität eingeräumt wird. Ein Benutzer sollte beispielsweise primär eine Verbindung mit der ihm am nächsten gelegenen geografischen Region herstellen. Wenn in dieser Region ein Problem vorliegt, leitet Traffic Manager den Benutzer an eine sekundäre Region weiter. Diese Vorgehensweise stellt sicher, dass Kunden immer eine Verbindung mit einer Anwendungsinstanz herstellen können, auch wenn die nächstgelegene geografische Region nicht verfügbar ist.

Die Schritte zum Einrichten der Endpunkte und der Weiterleitung finden Sie unter [Konfigurieren der geografischen Routingmethode für Datenverkehr mithilfe von Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door"></a>Layer-7-Anwendungsrouting mit Azure Front Door

Azure Traffic Manager verwendet DNS (Layer 3) zum Formen des Datenverkehrs. [Azure Front Door (Vorschau)](https://docs.microsoft.com/azure/frontdoor/front-door-overview) stellt eine Routingoption über HTTP/HTTPS (Layer 7) bereit. Zu den weiteren Features von Front Door gehören SSL-Terminierung, benutzerdefinierte Domänen, Web Application Firewall, URL-Rewrite und Sitzungsaffinität.

Überprüfen Sie die Anforderungen des Anwendungsdatenverkehrs, um zu ermitteln, welche Lösung sich am besten eignet.

## <a name="enable-geo-replication-for-container-images"></a>Aktivieren der Georeplikation für Containerimages

**Best Practices-Leitfaden**: Speichern Sie Ihre Containerimages in Azure Container Registry (ACR), und erstellen Sie per Georeplikation in jeder AKS-Region eine Instanz der Registry.

Um Ihre Anwendungen in AKS bereitzustellen und zu registrieren, benötigen Sie eine Möglichkeit, die Containerimages zu speichern und abzurufen. Azure Container Registry lässt sich in AKS integrieren, um Containerimages oder Helm-Charts sicher zu speichern. ACR unterstützt die Multimaster-Georeplikation, mit der Sie Ihre Images automatisch weltweit in Azure-Regionen replizieren können. Um die Leistung und Verfügbarkeit zu verbessern, können Sie mithilfe der ACR-Georeplikation in jeder Region, in der Sie einen AKS-Cluster eingerichtet haben, eine Registry-Instanz erstellen. Jeder AKS-Cluster ruft Containerimages aus der lokalen ACR-Instanz in der gleichen Region ab:

![Georeplikation der Azure Container Registry für Containerimages](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Die ACR-Georeplikation bietet u.a. folgende Vorteile:

* **Images lassen sich schneller aus derselben Region abrufen.** Sie können Images über schnelle Netzwerkverbindungen mit niedriger Latenz innerhalb derselben Azure-Region abrufen.
* **Images lassen sich zuverlässiger aus derselben Region abrufen.** Wenn eine Region nicht verfügbar ist, ruft Ihr AKS-Cluster ein Image aus einer anderen ACR-Instanz ab, die verfügbar ist.
* **Images lassen sich kostengünstiger aus derselben Region abrufen.** Für den ausgehenden Netzwerkdatenverkehr zwischen Rechenzentren fallen keine Gebühren an.

Die Georeplikation ist ein Azure Container Registry-Feature in *Premium-SKUs*. Schritte zum Konfigurieren der Replikation finden Sie unter [Georeplikation in Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Entfernen des Dienstzustands aus Containern

**Best Practices-Leitfaden**: Speichern Sie den Dienstzustand nach Möglichkeit nicht im Container. Verwenden Sie stattdessen Azure-PaaS-Dienste (Platform-as-a-Service), die eine regionsübergreifende Replikation unterstützen.

Der Dienstzustand bezieht sich auf die Daten im Arbeitsspeicher oder auf einem Datenträger, die ein Dienst für seine Ausführung benötigt. Hierzu gehören auch die Datenstrukturen und Membervariablen, die vom Dienst gelesen und geschrieben werden. Je nach Architektur des Diensts kann der Zustand auch Dateien oder andere Ressourcen umfassen, die auf einem Datenträger gespeichert sind. Ein Beispiel wären etwa Dateien, die eine Datenbank zum Speichern von Daten und Transaktionsprotokollen verwendet.

Der Zustand kann ausgelagert oder mit dem Code gespeichert werden, der den Zustand ändert. Die Auslagerung des Zustands erfolgt in der Regel mithilfe einer Datenbank oder einem anderen Datenspeicher, der über das Netzwerk auf verschiedenen Computern oder prozessextern auf dem gleichen Computer ausgeführt wird.

Container und Microservices sind dann am stabilsten, wenn die darin ausgeführten Prozesse ihren Zustand nicht beibehalten. Da Anwendungen fast immer einen Zustand beinhalten, verwenden Sie eine PaaS-Lösung wie Azure Database for MySQL/PostgreSQL oder Azure SQL.

Informationen zum Erstellen von Anwendungen, die sich besser portieren lassen, finden Sie in den folgenden Leitfäden:

* [The Twelve-Factor App Methodology](https://12factor.net/) (Die Zwölf-Faktoren-Methodik für Apps)
* [Ausführen einer Webanwendung in mehreren Azure-Regionen](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Erstellen eines Plans für die Speichermigration

**Best Practices-Leitfaden**: Wenn Sie Azure Storage verwenden, bereiten Sie die Migration Ihrer Speicher von der primären zur Sicherungsregion vor, und testen Sie sie.

Ihre Anwendungen nutzen möglicherweise Azure Storage für ihre Daten. Wenn Ihre Anwendungen auf mehrere AKS-Cluster in verschiedenen Regionen verteilt sind, müssen Sie dafür sorgen, dass die Speicherinstanzen synchron bleiben. Die folgenden beiden Vorgehensweisen sind zwei gängige Methoden zum Replizieren von Speicher:

* Anwendungsbasierte asynchrone Replikation
* Infrastrukturbasierte asynchrone Replikation

### <a name="infrastructure-based-asynchronous-replication"></a>Infrastrukturbasierte asynchrone Replikation

Ihre Anwendungen erfordern möglicherweise auch dann noch einen beständigen Speicher, nachdem ein Pod gelöscht wurde. In Kubernetes können Sie persistente Volumes verwenden, um Daten dauerhaft zu speichern. Diese persistenten Volumes werden auf einem virtuellen Computerknoten bereitgestellt und dann für die Pods verfügbar gemacht. Persistente Volumes folgen Pods, auch dann, wenn ein Pod auf einen anderen Knoten im gleichen Cluster verschoben wird.

Je nach Nutzung der Speicherlösung können verschiedene Replikationsstrategien eingesetzt werden. Für gängige Speicherlösungen wie [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [CEPH](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [Rook](https://rook.io/docs/rook/master/disaster-recovery.html) und [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) gibt es jeweils eigene Leitfäden.

Der zentrale Ansatz besteht darin, einen gemeinsamen Speicher für Anwendungen zu erstellen, in den die Anwendungsdaten geschrieben werden. Diese Daten werden regionsübergreifend gespeichert, der Zugriff auf die Daten erfolgt lokal.

![Infrastrukturbasierte asynchrone Replikation](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Wenn Sie Azure Managed Disks verwenden, stehen folgende Methoden für Replikation und Notfallwiederherstellung zur Verfügung:

* [Ark auf Azure](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Anwendungsbasierte asynchrone Replikation

Zurzeit gibt es keine native Kubernetes-Implementierung für die anwendungsbasierte asynchrone Replikation. Aufgrund der lockeren Kopplung von Containern und Kubernetes sollte jeder herkömmliche Ansatz hinsichtlich Anwendung oder Sprache funktionieren. Der zentrale Ansatz besteht hier darin, dass die Anwendungen selbst die Speicheranforderungen replizieren, die dann in den zugrunde liegenden Datenspeicher jedes Clusters geschrieben werden.

![Anwendungsbasierte asynchrone Replikation](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden Überlegungen zur Geschäftskontinuität und Notfallwiederherstellung in AKS-Cluster erläutert. Weitere Informationen zu Clustervorgängen in AKS finden Sie in den folgenden Best Practices:

* [Mehrinstanzenfähigkeit und Clusterisolation][aks-best-practices-cluster-isolation]
* [Grundlegende Funktionen des Kubernetes-Schedulers][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md

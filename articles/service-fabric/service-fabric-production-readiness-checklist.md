---
title: Prüfliste für die Produktionsbereitschaft von Azure Service Fabric | Microsoft-Dokumentation
description: Bereiten Sie Ihre Service Fabric-Anwendung und den Cluster anhand der folgenden bewährten Methoden auf die Produktion vor.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: subramar
ms.openlocfilehash: 5092bf377f6ab213a2db52e01fb7b89ff0c218ce
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39127473"
---
# <a name="production-readiness-checklist"></a>Prüfliste für die Produktionsbereitschaft

Sind Ihre Anwendung und Ihr Cluster bereit für den Produktionsdatenverkehr? Das Ausführen und Testen Ihrer Anwendung und Ihres Clusters bedeutet nicht notwendigerweise, dass Produktionsbereitschaft vorliegt. Halten Sie die Ausführung Ihrer Anwendung und Ihres Clusters reibungslos in Gang, indem Sie die folgende Prüfliste durchgehen. Wir empfehlen dringend, alle diese Punkte abzuhaken. Selbstverständlich können Sie für einen bestimmten Punkt alternative Lösungen verwenden (z.B. eigene Diagnoseframeworks).


## <a name="pre-requisites-for-production"></a>Voraussetzungen für die Produktion

1. Erstellen Sie für Cluster mit mehr als 20 Kernen oder 10 Knoten einen dedizierten primären Knotentyp für Systemdienste. Fügen Sie [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) hinzu, um den primären Knotentyp für Systemdienste zu reservieren. 
2. Verwenden Sie für den primären Knotentyp eine SKU des Typs D2v2 oder höher. Es wird empfohlen, eine SKU mit einer Festplattenkapazität von mindestens 50 GB auszuwählen.
2. Produktionscluster müssen [sicher](service-fabric-cluster-security.md) sein. Ein Beispiel für das Einrichten eines sicheren Clusters finden Sie in dieser [Clustervorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Verwenden Sie allgemeine Namen für die Zertifikate, und vermeiden Sie selbstsignierte Zertifikate.
4. Fügen Sie [Ressourceneinschränkungen für Container und Dienste](service-fabric-resource-governance.md) hinzu, damit diese nicht mehr als 75 % der Knotenressourcen belegen. 
5. Machen Sie sich mit [Dauerhaftigkeitsstufen](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) vertraut, und legen Sie eine fest. Für Knotentypen, auf denen statusbehaftete Workloads ausgeführt werden, wird die Dauerhaftigkeitsstufe Silber oder höher empfohlen. Für den primären Knotentyp sollte die Dauerhaftigkeitsstufe auf Silber oder höher festgelegt sein.
6. Machen Sie sich mit [Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) vertraut, und legen Sie eine für den Knotentyp fest. Die Zuverlässigkeitsstufe Silver oder höher wird empfohlen.
7. Laden Sie Ihre Workloads, und unterziehen Sie sie Skalierungstests, um die [Kapazitätsanforderungen](service-fabric-cluster-capacity.md) für Ihren Cluster zu ermitteln. 
8. Ihre Dienste und Anwendungen werden überwacht und Anwendungsprotokolle mit Warnhinweisen erstellt und gespeichert. Beispiele finden Sie unter [Hinzufügen von Protokollierung zur Service Fabric-Anwendung](service-fabric-how-to-diagnostics-log.md) und [Überwachen von Containern mit Log Analytics](service-fabric-diagnostics-oms-containers.md).
9. Der Cluster wird mit Warnhinweisen (z.B. durch [OMS](service-fabric-diagnostics-event-analysis-oms.md)) überwacht. 
10. Die zugrunde liegende Infrastruktur von VM-Skalierungsgruppen wird mit Warnhinweisen überwacht (z.B. mit [Log Analytics](service-fabric-diagnostics-oms-agent.md)).
11. Der Cluster verfügt stets über [primäre und sekundäre Zertifikate](service-fabric-cluster-security-update-certs-azure.md) (damit Sie nicht ausgesperrt werden).
12. Für Entwicklung, Staging und Produktion werden getrennte Cluster verwaltet. 
13. [Anwendungsupgrades](service-fabric-application-upgrade.md) und [Clusterupgrades](service-fabric-tutorial-upgrade-cluster.md) werden zuerst in Entwicklungs- und Stagingclustern getestet und bereitgestellt. 
14. Deaktivieren Sie automatische Upgrades in Produktionsclustern, und aktivieren Sie sie für Entwicklungs- und Staging-Cluster (Rollback nach Bedarf). 
15. Richten Sie ein Recovery Point Objective (RPO) für Ihren Dienst und einen [Notfallwiederherstellungsprozess](service-fabric-disaster-recovery.md) ein, und testen Sie ihn.
16. Planen Sie die manuelle oder programmgesteuerte [Skalierung](service-fabric-cluster-scaling.md) Ihres Clusters.
17. Planen Sie das [Patchen](service-fabric-patch-orchestration-application.md) der Clusterknoten. 
18. Richten Sie eine CI/CD-Pipeline ein, sodass die letzten Änderungen fortlaufend getestet werden. Verwenden Sie dazu beispielsweise [VSTS](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) oder [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
19. Testen Sie Ihre Entwicklungs- und Stagingcluster unter Last mit dem [Fault Analysis Service](service-fabric-testability-overview.md), und induzieren Sie kontrolliertes [Chaos](service-fabric-controlled-chaos.md). 
20. Planen Sie die [Skalierung](service-fabric-concepts-scalability.md) Ihrer Anwendungen. 


Wenn Sie das Programmiermodell Service Fabric Reliable Services oder Reliable Actors verwenden, müssen die folgenden Punkte abgehakt werden:
21. Aktualisieren Sie Anwendungen während der lokalen Entwicklung, um zu überprüfen, ob Ihr Dienstcode das Annullierungstoken in der `RunAsync`-Methode beachtet und benutzerdefinierte Kommunikationslistener schließt.
22. Vermeiden Sie [häufige Fehlerquellen](service-fabric-work-with-reliable-collections.md) bei Verwendung zuverlässiger Sammlungen.
23. Überwachen Sie die .NET CLR-Speicherleistungszähler, wenn Sie Lasttests durchführen, und prüfen Sie auf hohe Raten von Garbage Collection oder Ausreißer bei der Zunahme der Heapgröße.
24. Halten Sie Offlinesicherungen von [Reliable Services und Reliable Actors](service-fabric-reliable-services-backup-restore.md) vor, und testen Sie den Wiederherstellungsprozess. 


## <a name="optional-best-practices"></a>Optionale bewährte Methoden

Die obigen Listen sind Voraussetzungen für die Inbetriebnahme in der Produktion, aber auch die folgenden Punkte sollten berücksichtigt werden:
25. Stellen Sie eine Verbindung mit dem [Service Fabric-Integritätsmodell](service-fabric-health-introduction.md) her, um die integrierte Integritätsbewertung und Berichterstellung zu erweitern.
26. Stellen Sie einen benutzerdefinierten Watchdog bereit, der Ihre Anwendung überwacht und [Last](service-fabric-cluster-resource-manager-metrics.md) für [Ressourcenausgleich](service-fabric-cluster-resource-manager-balancing.md) meldet. 


## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen eines Service Fabric-Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Bereitstellen eines Service Fabric-Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric

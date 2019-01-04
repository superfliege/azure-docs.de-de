---
title: Prüfliste für die Produktionsbereitschaft von Azure Service Fabric | Microsoft-Dokumentation
description: Bereiten Sie Ihre Service Fabric-Anwendung und den Cluster anhand der folgenden bewährten Methoden auf die Produktion vor.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: aljo-microsoft
ms.openlocfilehash: 4e6d5cb3191be7188c1a7c4753200cf049800f04
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436006"
---
# <a name="production-readiness-checklist"></a>Prüfliste für die Produktionsbereitschaft

Sind Ihre Anwendung und Ihr Cluster bereit für den Produktionsdatenverkehr? Das Ausführen und Testen Ihrer Anwendung und Ihres Clusters bedeutet nicht notwendigerweise, dass Produktionsbereitschaft vorliegt. Halten Sie die Ausführung Ihrer Anwendung und Ihres Clusters reibungslos in Gang, indem Sie die folgende Prüfliste durchgehen. Wir empfehlen dringend, alle diese Punkte abzuhaken. Selbstverständlich können Sie für einen bestimmten Punkt alternative Lösungen verwenden (z.B. eigene Diagnoseframeworks).


## <a name="pre-requisites-for-production"></a>Voraussetzungen für die Produktion
1. [Bewährte Methoden für Service Fabric](https://docs.microsoft.com/azure/security/azure-service-fabric-security-best-practices): 
* Verwenden von X.509-Zertifikaten
* Konfigurieren von Sicherheitsrichtlinien
* Konfigurieren von SSL für Azure Service Fabric
* Verwenden von Netzwerkisolation und -sicherheit mit Azure Service Fabric
* Einrichten einer Azure Key Vault-Instanz für die Sicherheit
* Zuweisen von Benutzern zu Rollen
* Implementieren der Reliable Actors-Sicherheitskonfiguration, wenn das Actors-Programmiermodell verwendet wird
2. Erstellen Sie für Cluster mit mehr als 20 Kernen oder 10 Knoten einen dedizierten primären Knotentyp für Systemdienste. Fügen Sie [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) hinzu, um den primären Knotentyp für Systemdienste zu reservieren. 
3. Verwenden Sie für den primären Knotentyp eine SKU des Typs D2v2 oder höher. Es wird empfohlen, eine SKU mit einer Festplattenkapazität von mindestens 50 GB auszuwählen.
4. Produktionscluster müssen [sicher](service-fabric-cluster-security.md) sein. Ein Beispiel für das Einrichten eines sicheren Clusters finden Sie in dieser [Clustervorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Verwenden Sie allgemeine Namen für die Zertifikate, und vermeiden Sie selbstsignierte Zertifikate.
5. Fügen Sie [Ressourceneinschränkungen für Container und Dienste](service-fabric-resource-governance.md) hinzu, damit diese nicht mehr als 75 % der Knotenressourcen belegen. 
6. Machen Sie sich mit [Dauerhaftigkeitsstufen](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) vertraut, und legen Sie eine fest. Für Knotentypen, auf denen statusbehaftete Workloads ausgeführt werden, wird die Dauerhaftigkeitsstufe Silber oder höher empfohlen. Für den primären Knotentyp sollte die Dauerhaftigkeitsstufe auf Silber oder höher festgelegt sein.
7. Machen Sie sich mit [Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) vertraut, und legen Sie eine für den Knotentyp fest. Die Zuverlässigkeitsstufe Silver oder höher wird empfohlen.
8. Laden Sie Ihre Workloads, und unterziehen Sie sie Skalierungstests, um die [Kapazitätsanforderungen](service-fabric-cluster-capacity.md) für Ihren Cluster zu ermitteln. 
9. Ihre Dienste und Anwendungen werden überwacht und Anwendungsprotokolle mit Warnhinweisen erstellt und gespeichert. Beispiele finden Sie unter [Hinzufügen von Protokollierung zur Service Fabric-Anwendung](service-fabric-how-to-diagnostics-log.md) und [Überwachen von Containern mit Log Analytics](service-fabric-diagnostics-oms-containers.md).
10. Der Cluster wird mit Warnungen (z.B. durch [Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)) überwacht. 
11. Die zugrunde liegende Infrastruktur von VM-Skalierungsgruppen wird mit Warnhinweisen überwacht (z.B. mit [Log Analytics](service-fabric-diagnostics-oms-agent.md)).
12. Der Cluster verfügt stets über [primäre und sekundäre Zertifikate](service-fabric-cluster-security-update-certs-azure.md) (damit Sie nicht ausgesperrt werden).
13. Für Entwicklung, Staging und Produktion werden getrennte Cluster verwaltet. 
14. [Anwendungsupgrades](service-fabric-application-upgrade.md) und [Clusterupgrades](service-fabric-tutorial-upgrade-cluster.md) werden zuerst in Entwicklungs- und Stagingclustern getestet und bereitgestellt. 
15. Deaktivieren Sie automatische Upgrades in Produktionsclustern, und aktivieren Sie sie für Entwicklungs- und Staging-Cluster (Rollback nach Bedarf). 
16. Richten Sie ein Recovery Point Objective (RPO) für Ihren Dienst und einen [Notfallwiederherstellungsprozess](service-fabric-disaster-recovery.md) ein, und testen Sie ihn.
17. Planen Sie die manuelle oder programmgesteuerte [Skalierung](service-fabric-cluster-scaling.md) Ihres Clusters.
18. Planen Sie das [Patchen](service-fabric-patch-orchestration-application.md) der Clusterknoten. 
19. Richten Sie eine CI/CD-Pipeline ein, sodass die letzten Änderungen fortlaufend getestet werden. Beispiel für die Verwendung von [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) oder [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
20. Testen Sie Ihre Entwicklungs- und Stagingcluster unter Last mit dem [Fault Analysis Service](service-fabric-testability-overview.md), und induzieren Sie kontrolliertes [Chaos](service-fabric-controlled-chaos.md). 
21. Planen Sie die [Skalierung](service-fabric-concepts-scalability.md) Ihrer Anwendungen. 


Wenn Sie das Programmiermodell Service Fabric Reliable Services oder Reliable Actors verwenden, müssen die folgenden Punkte abgehakt werden:
22. Aktualisieren Sie Anwendungen während der lokalen Entwicklung, um zu überprüfen, ob Ihr Dienstcode das Annullierungstoken in der `RunAsync`-Methode beachtet und benutzerdefinierte Kommunikationslistener schließt.
23. Vermeiden Sie [häufige Fehlerquellen](service-fabric-work-with-reliable-collections.md) bei Verwendung zuverlässiger Sammlungen.
24. Überwachen Sie die .NET CLR-Speicherleistungszähler, wenn Sie Lasttests durchführen, und prüfen Sie auf hohe Raten von Garbage Collection oder Ausreißer bei der Zunahme der Heapgröße.
25. Halten Sie Offlinesicherungen von [Reliable Services und Reliable Actors](service-fabric-reliable-services-backup-restore.md) vor, und testen Sie den Wiederherstellungsprozess. 


## <a name="optional-best-practices"></a>Optionale bewährte Methoden

Die obigen Listen sind Voraussetzungen für die Inbetriebnahme in der Produktion, aber auch die folgenden Punkte sollten berücksichtigt werden:
26. Stellen Sie eine Verbindung mit dem [Service Fabric-Integritätsmodell](service-fabric-health-introduction.md) her, um die integrierte Integritätsbewertung und Berichterstellung zu erweitern.
27. Stellen Sie einen benutzerdefinierten Watchdog bereit, der Ihre Anwendung überwacht und [Last](service-fabric-cluster-resource-manager-metrics.md) für [Ressourcenausgleich](service-fabric-cluster-resource-manager-balancing.md) meldet. 


## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen eines Service Fabric-Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Bereitstellen eines Service Fabric-Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric

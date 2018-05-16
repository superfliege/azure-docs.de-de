---
title: Azure Service Fabric-Ereignisliste | Microsoft-Dokumentation
description: Eine umfassende Liste mit Ereignissen, die von Azure Service Fabric bereitgestellt werden, um Sie bei der Clusterüberwachung zu unterstützen.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: d397dcb1ecdc25dbd66ab3d6f2f010bc29f87c6c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="list-of-service-fabric-events"></a>Liste mit Service Fabric-Ereignissen 

Service Fabric macht einen primären Satz von Clusterereignissen in Form von [Service Fabric-Ereignissen](service-fabric-diagnostics-events.md) verfügbar, um Sie über den Status Ihres Clusters zu informieren. Diese basieren auf Aktionen, die von Service Fabric für Ihre Knoten und Cluster ausgeführt werden, oder auf Verwaltungsentscheidungen eines Clusterbesitzers/-betreibers. Für den Zugriff auf diese Ereignisse können Sie den [EventStore](service-fabric-diagnostics-eventstore.md) in Ihrem Cluster abfragen oder den Betriebskanal verwenden. Auf Windows-Computern ist der Betriebskanal auch in „EventLog“ eingebunden, sodass Service Fabric-Ereignisse in der Ereignisanzeige angezeigt werden. 

>[!NOTE]
>Eine Liste mit Service Fabric-Ereignissen für Cluster in Versionen vor 6.2 finden Sie im folgenden Abschnitt. 

Die folgende Liste enthält alle verfügbaren Ereignisse der Plattform – sortiert nach der Entität, der sie zuordnet sind.

## <a name="cluster-events"></a>Clusterereignisse

**Ereignisse für Clusterupgrades**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStartOperational | CM | Information | 1 |
| 29628 | ClusterUpgradeCompleteOperational | CM | Information | 1 |
| 29629 | ClusterUpgradeRollbackStartOperational | CM | Information | 1 |
| 29630 | ClusterUpgradeRollbackCompleteOperational | CM | Information | 1 |
| 29631 | ClusterUpgradeDomainCompleteOperational | CM | Information | 1 |

**Ereignisse für Clusterintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54428 | ProcessClusterReportOperational | HM | Information | 1 |
| 54437 | ExpiredClusterEventOperational | HM | Information | 1 |

**Chaosdienstereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 50021 | ChaosStartedEvent | Prüfbarkeit | Information | 1 |
| 50023 | ChaosStoppedEvent | Prüfbarkeit | Information | 1 |

## <a name="node-events"></a>Knotenereignisse

**Ereignisse für den Knotenlebenszyklus** 

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 18602 | DeactivateNodeCompletedOperational | FM | Information | 1 |
| 18603 | NodeUpOperational | FM | Information | 1 |
| 18604 | NodeDownOperational | FM | Information | 1 |
| 18605 | NodeAddedOperational | FM | Information | 1 |
| 18606 | NodeRemovedOperational | FM | Information | 1 |
| 18607 | DeactivateNodeStartOperational | FM | Information | 1 |
| 25620 | NodeOpening | FabricNode | Information | 1 |
| 25621 | NodeOpenedSuccess | FabricNode | Information | 1 |
| 25622 | NodeOpenedFailed | FabricNode | Information | 1 |
| 25623 | NodeClosing | FabricNode | Information | 1 |
| 25624 | NodeClosed | FabricNode | Information | 1 |
| 25625 | NodeAborting | FabricNode | Information | 1 |
| 25626 | NodeAborted | FabricNode | Information | 1 |

**Ereignisse für Knotenintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54423 | ProcessNodeReportOperational | HM | Information | 1 |
| 54432 | ExpiredNodeEventOperational | HM | Information | 1 |

**Chaosknotenereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 50033 | ChaosRestartNodeFaultScheduledEvent | Prüfbarkeit | Information | 1 |
| 50087 | ChaosRestartNodeFaultCompletedEvent | Prüfbarkeit | Information | 1 |

## <a name="application-events"></a>Anwendungsereignisse

**Ereignisse für den Anwendungslebenszyklus**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 29620 | ApplicationCreatedOperational | CM | Information | 1 |
| 29625 | ApplicationDeletedOperational | CM | Information | 1 |
| 23083 | ProcessExitedOperational | Hosting | Information | 1 |

**Ereignisse für Anwendungsupgrades**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 29621 | ApplicationUpgradeStartOperational | CM | Information | 1 |
| 29622 | ApplicationUpgradeCompleteOperational | CM | Information | 1 |
| 29623 | ApplicationUpgradeRollbackStartOperational | CM | Information | 1 |
| 29624 | ApplicationUpgradeRollbackCompleteOperational | CM | Information | 1 |
| 29626 | ApplicationUpgradeDomainCompleteOperational | CM | Information | 1 |

**Ereignisse für Anwendungsintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54425 | ProcessApplicationReportOperational | HM | Information | 1 |
| 54426 | ProcessDeployedApplicationReportOperational | HM | Information | 1 |
| 54427 | ProcessDeployedServicePackageReportOperational | HM | Information | 1 |
| 54434 | ExpiredApplicationEventOperational | HM | Information | 1 |
| 54435 | ExpiredDeployedApplicationEventOperational | HM | Information | 1 |
| 54436 | ExpiredDeployedServicePackageEventOperational | HM | Information | 1 |

**Chaosanwendungsereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 50053 | ChaosRestartCodePackageFaultScheduledEvent | Prüfbarkeit | Information | 1 |
| 50101 | ChaosRestartCodePackageFaultCompletedEvent | Prüfbarkeit | Information | 1 |

## <a name="service-events"></a>Dienstereignisse

**Ereignisse für den Dienstlebenszyklus**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 18602 | ServiceCreatedOperational | FM | Information | 1 |
| 18658 | ServiceDeletedOperational | FM | Information | 1 |

**Ereignisse für Dienstintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54424 | ProcessServiceReportOperational | HM | Information | 1 |
| 54433 | ExpiredServiceEventOperational | HM | Information | 1 |

## <a name="partition-events"></a>Partitionsereignisse

**Ereignisse für Partitionsverschiebungen**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 18940 | ReconfigurationCompleted | RA | Information | 1 |

**Ereignisse für Partitionsintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54422 | ProcessPartitionReportOperational | HM | Information | 1 |
| 54431 | ExpiredPartitionEventOperational | HM | Information | 1 |

**Chaospartitionsereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 50069 | ChaosMovePrimaryFaultScheduledEvent | Prüfbarkeit | Information | 1 |
| 50077 | ChaosMoveSecondaryFaultScheduledEvent | Prüfbarkeit | Information | 1 |

**Ereignisse für die Partitionsanalyse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 65003 | PrimaryMoveAnalysisEvent | Prüfbarkeit | Information | 1 |

## <a name="replica-events"></a>Replikatereignisse

**Ereignisse für Replikatintegritätsberichte**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 54429 | ProcessStatefulReplicaReportOperational | HM | Information | 1 |
| 54430 | ProcessStatelessInstanceReportOperational | HM | Information | 1 |
| 54438 | ExpiredStatefulReplicaEventOperational | HM | Information | 1 |
| 54439 | ExpiredStatelessInstanceEventOperational | HM | Information | 1 |

**Chaosreplikatereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 50047 | ChaosRestartReplicaFaultScheduledEvent | Prüfbarkeit | Information | 1 |
| 50051 | ChaosRemoveReplicaFaultScheduledEvent | Prüfbarkeit | Information | 1 |
| 50093 | ChaosRemoveReplicaFaultCompletedEvent | Prüfbarkeit | Information | 1 |

## <a name="container-events"></a>Containerereignisse

**Ereignisse für den Containerlebenszyklus** 

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 23074 | ContainerActivatedOperational | Hosting | Information | 1 |
| 23075 | ContainerDeactivatedOperational | Hosting | Information | 1 |
| 23082 | ContainerExitedOperational | Hosting | Information | 1 |

## <a name="other-events"></a>Sonstige Ereignisse

**Korrelationsereignisse**

| EventId | NAME | Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- |
| 65011 | CorrelationOperational | Prüfbarkeit | Information | 1 |

## <a name="events-prior-to-version-62"></a>Ereignisse vor Version 6.2

Im Anschluss finden Sie eine umfassende Liste mit Ereignissen, die von Service Fabric vor Version 6.2 bereitgestellt wurden:

| EventId | NAME | Quelle (Aufgabe) | Ebene |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Information |
| 25621 | NodeOpenedSuccess | FabricNode | Information |
| 25622 | NodeOpenedFailed | FabricNode | Information |
| 25623 | NodeClosing | FabricNode | Information |
| 25624 | NodeClosed | FabricNode | Information |
| 25625 | NodeAborting | FabricNode | Information |
| 25626 | NodeAborted | FabricNode | Information |
| 29627 | ClusterUpgradeStart | CM | Information |
| 29628 | ClusterUpgradeComplete | CM | Information |
| 29629 | ClusterUpgradeRollback | CM | Information |
| 29630 | ClusterUpgradeRollbackComplete | CM | Information |
| 29631 | ClusterUpgradeDomainComplete | CM | Information |
| 23074 | ContainerActivated | Hosting | Information |
| 23075 | ContainerDeactivated | Hosting | Information |
| 29620 | ApplicationCreated | CM | Information |
| 29621 | ApplicationUpgradeStart | CM | Information |
| 29622 | ApplicationUpgradeComplete | CM | Information |
| 29623 | ApplicationUpgradeRollback | CM | Information |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Information |
| 29625 | ApplicationDeleted | CM | Information |
| 29626 | ApplicationUpgradeDomainComplete | CM | Information |
| 18566 | ServiceCreated | FM | Information |
| 18567 | ServiceDeleted | FM | Information |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die gesamte [Ereignisgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric.
* Ändern Ihrer [Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md)-Konfiguration zum Sammeln weiterer Protokolle
* [Einrichten von Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) zum Anzeigen der Betriebskanalprotokolle

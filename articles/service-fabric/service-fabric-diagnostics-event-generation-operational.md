---
title: Azure Service Fabric-Ereignisliste | Microsoft-Dokumentation
description: Eine umfassende Liste mit Ereignissen, die von Azure Service Fabric bereitgestellt werden, um Sie bei der Clusterüberwachung zu unterstützen.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 03dac03405588ba00a0f8ca5b127956c40853e36
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868512"
---
# <a name="list-of-service-fabric-events"></a>Liste mit Service Fabric-Ereignissen 

Service Fabric macht einen primären Satz von Clusterereignissen in Form von [Service Fabric-Ereignissen](service-fabric-diagnostics-events.md) verfügbar, um Sie über den Status Ihres Clusters zu informieren. Diese basieren auf Aktionen, die von Service Fabric für Ihre Knoten und Cluster ausgeführt werden, oder auf Verwaltungsentscheidungen eines Clusterbesitzers/-betreibers. Für den Zugriff auf diese Ereignisse können Sie den [EventStore](service-fabric-diagnostics-eventstore.md) in Ihrem Cluster abfragen oder den Betriebskanal verwenden. Auf Windows-Computern ist der Betriebskanal auch in „EventLog“ eingebunden, sodass Service Fabric-Ereignisse in der Ereignisanzeige angezeigt werden. 

>[!NOTE]
>Eine Liste mit Service Fabric-Ereignissen für Cluster in Versionen vor 6.2 finden Sie im folgenden Abschnitt. 

Die folgende Liste enthält alle verfügbaren Ereignisse der Plattform – sortiert nach der Entität, der sie zuordnet sind.

## <a name="cluster-events"></a>Clusterereignisse

**Ereignisse für Clusterupgrades**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStarted | Ein Clusterupgrade wurde gestartet. | CM | Information | 1 |
| 29628 | ClusterUpgradeCompleted | Ein Clusterupgrade wurde abgeschlossen.| CM | Information | 1 |
| 29629 | ClusterUpgradeRollbackStarted | Das Rollback eines Clusterupgrades wurde gestartet. | CM | Information | 1 |
| 29630 | ClusterUpgradeRollbackCompleted | Das Rollback eines Clusterupgrades wurde abgeschlossen. | CM | Information | 1 |
| 29631 | ClusterUpgradeDomainCompleted | Ein Domänenupgrade wurde während eines Clusterupgrades abgeschlossen. | CM | Information | 1 |

## <a name="node-events"></a>Knotenereignisse

**Ereignisse für den Knotenlebenszyklus** 

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 18602 | NodeDeactivateCompleted | Die Deaktivierung eines Knotens wurde abgeschlossen. | FM | Information | 1 |
| 18603 | NodeUp | Der Cluster hat erkannt, dass ein Knoten gestartet wurde. | FM | Information | 1 |
| 18604 | NodeDown | Der Cluster hat erkannt, dass ein Knoten heruntergefahren wurde. |  FM | Information | 1 |
| 18605 | NodeAddedToCluster | Ein neuer Knoten wurde dem Cluster hinzugefügt. | FM | Information | 1 |
| 18606 | NodeRemovedFromCluster | Ein Knoten wurde aus dem Cluster entfernt. | FM | Information | 1 |
| 18607 | NodeDeactivateStarted | Die Deaktivierung eines Knotens wurde gestartet. | FM | Information | 1 |
| 25620 | NodeOpening | Der Knoten wird gestartet. Erste Phase des Knotenlebenszyklus | FabricNode | Information | 1 |
| 25621 | NodeOpenSucceeded | Ein Knoten wurde erfolgreich gestartet. | FabricNode | Information | 1 |
| 25622 | NodeOpenFailed | Ein Knoten konnte nicht gestartet werden. | FabricNode | Information | 1 |
| 25623 | NodeClosing | Ein Knoten wird heruntergefahren. Beginn der Endphase des Knotenlebenszyklus | FabricNode | Information | 1 |
| 25624 | NodeClosed | Ein Knoten wurde erfolgreich heruntergefahren. | FabricNode | Information | 1 |
| 25625 | NodeAborting | Ein Knoten wird nicht ordnungsgemäß heruntergefahren. | FabricNode | Information | 1 |
| 25626 | NodeAborted | Ein Knoten wurde nicht ordnungsgemäß heruntergefahren. | FabricNode | Information | 1 |

## <a name="application-events"></a>Anwendungsereignisse

**Ereignisse für den Anwendungslebenszyklus**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 29620 | ApplicationCreated | Eine neue Anwendung wurde erstellt. | CM | Information | 1 |
| 29625 | ApplicationDeleted | Eine vorhandene Anwendung wurde gelöscht. | CM | Information | 1 |
| 23083 | ApplicationProcessExited | Ein Prozess in einer Anwendung wurde beendet. | Hosting | Information | 1 |

**Ereignisse für Anwendungsupgrades**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 29621 | ApplicationUpgradeStarted | Ein Anwendungsupgrade wurde gestartet. | CM | Information | 1 |
| 29622 | ApplicationUpgradeStarted | Ein Anwendungsupgrade wurde abgeschlossen. | CM | Information | 1 |
| 29623 | ApplicationUpgradeRollbackStarted | Das Rollback eines Anwendungsupgrades wurde gestartet. |CM | Information | 1 |
| 29624 | ApplicationUpgradeRollbackCompleted | Das Rollback eines Anwendungsupgrades wurde abgeschlossen. | CM | Information | 1 |
| 29626 | ApplicationUpgradeDomainCompleted | Ein Domänenupgrade wurde während eines Anwendungsupgrades abgeschlossen. | CM | Information | 1 |

## <a name="service-events"></a>Dienstereignisse

**Ereignisse für den Dienstlebenszyklus**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Ein neuer Dienst wurde erstellt. | FM | Information | 1 |
| 18658 | ServiceDeleted | Ein vorhandener Dienst wurde gelöscht. | FM | Information | 1 |

## <a name="partition-events"></a>Partitionsereignisse

**Ereignisse für Partitionsverschiebungen**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Die Neukonfiguration einer Partition wurde abgeschlossen. | RA | Information | 1 |

## <a name="container-events"></a>Containerereignisse

**Ereignisse für den Containerlebenszyklus** 

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Ein Container wurde gestartet. | Hosting | Information | 1 |
| 23075 | ContainerDeactivated | Ein Container wurde beendet. | Hosting | Information | 1 |
| 23082 | ContainerExited | Ein Container wurde beendet. Überprüfen Sie das UnexpectedTermination-Flag. | Hosting | Information | 1 |

## <a name="health-reports"></a>Integritätsberichte

**Ereignisse für Clusterintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Ein Integritätsbericht für einen neuen Cluster ist verfügbar. | HM | Information | 1 |
| 54437 | ClusterHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Cluster ist abgelaufen. | HM | Information | 1 |

**Ereignisse für Knotenintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Ein Integritätsbericht für einen neuen Knoten ist verfügbar. | HM | Information | 1 |
| 54432 | NodeHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Knoten ist abgelaufen. | HM | Information | 1 |

**Ereignisse für Anwendungsintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Ein Integritätsbericht für eine neue Anwendung wurde erstellt. Dies gilt für nicht bereitgestellte Anwendungen. | HM | Information | 1 |
| 54426 | DeployedApplicationNewHealthReport | Ein Integritätsbericht für eine neue bereitgestellte Anwendung wurde erstellt. | HM | Information | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Ein Integritätsbericht für einen neuen bereitgestellten Dienst wurde erstellt. | HM | Information | 1 |
| 54434 | ApplicationHealthReportExpired | Ein Integritätsbericht für eine vorhandene Anwendung ist abgelaufen. | HM | Information | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Ein Integritätsbericht für eine vorhandene bereitgestellte Anwendung ist abgelaufen. | HM | Information | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Ein Integritätsbericht für einen vorhandenen bereitgestellten Dienst ist abgelaufen. | HM | Information | 1 |

**Ereignisse für Dienstintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Ein Integritätsbericht für einen neuen Dienst wurde erstellt. | HM | Information | 1 |
| 54433 | ServiceHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Dienst ist abgelaufen. | HM | Information | 1 |

**Ereignisse für Partitionsintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Ein Integritätsbericht für eine neue Anwendung wurde erstellt. | HM | Information | 1 |
| 54431 | PartitionHealthReportExpired | Ein Integritätsbericht für eine vorhandene Partition ist abgelaufen. | HM | Information | 1 |

**Ereignisse für Replikatintegritätsberichte**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Ein Integritätsbericht für ein zustandsbehaftetes Replikat wurde erstellt. | HM | Information | 1 |
| 54430 | StatelessInstanceNewHealthReport | Ein Integritätsbericht für ein neues zustandsloses Replikat wurde erstellt. | HM | Information | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Ein Integritätsbericht für ein vorhandenes zustandsbehaftetes Replikat ist abgelaufen. | HM | Information | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Ein Integritätsbericht für eine vorhandene zustandslose Instanz ist abgelaufen. | HM | Information | 1 |

## <a name="chaos-testing-events"></a>Chaostestereignisse 

**Chaossitzungsereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Eine Chaostestsitzung wurde gestartet. | Prüfbarkeit | Information | 1 |
| 50023 | ChaosStopped | Eine Chaostestsitzung wurde beendet. | Prüfbarkeit | Information | 1 |

**Chaosknotenereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Der Neustart eines Knoten wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 50087 | ChaosNodeRestartCompleted | Der Neustart eines Knoten wurde als Teil einer Chaostestsitzung beendet. | Prüfbarkeit | Information | 1 |

**Chaosanwendungsereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Ein Neustart eines Codepakets wurde während einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Ein Neustart eines Codepakets wurde während einer Chaostestsitzung abgeschlossen. | Prüfbarkeit | Information | 1 |

**Chaospartitionsereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Die Verschiebung einer primären Partition wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Die Verschiebung einer sekundären Partition wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | Die Tiefenanalyse der Verschiebung der primären Partition ist verfügbar. | Prüfbarkeit | Information | 1 |

**Chaosreplikatereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Ein Neustart eines Replikats wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Die Entfernung eines Replikats wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Information | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Die Entfernung eines Replikats wurde als Teil einer Chaostestsitzung abgeschlossen. | Prüfbarkeit | Information | 1 |

## <a name="other-events"></a>Sonstige Ereignisse

**Korrelationsereignisse**

| EventId | NAME | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Eine Korrelation wurde getrennt. | Prüfbarkeit | Information | 1 |

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

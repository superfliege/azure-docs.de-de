---
title: Azure Service Fabric EventStore | Microsoft-Dokumentation
description: Informationen zu EventStore von Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: 520961fb4bd126ef878a779c10fb5689b8692c73
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683731"
---
# <a name="eventstore-service-overview"></a>Übersicht über den EventStore-Dienst

>[!NOTE]
>Ab Service Fabric-Version 6.4 sind die EventStore-APIs nur für in Azure ausgeführte Windows-Cluster verfügbar. Wir arbeiten daran, diese Funktionalität sowohl für Linux als auch für unseren eigenständigen Cluster zu portieren.

## <a name="overview"></a>Übersicht

Der EventStore-Dienst wurde in Version 6.2 eingeführt und ist eine Überwachungsoption in Service Fabric. EventStore bietet eine Möglichkeit, den Zustand Ihres Clusters oder Ihrer Workloads zu einem bestimmten Zeitpunkt zu verstehen. EventStore ist ein zustandsbehafteter Service Fabric-Dienst, der Ereignisse vom Cluster verwaltet. Die Ereignisse werden über Service Fabric-Explorer, REST und APIs bereitgestellt. EventStore fragt den Cluster direkt ab, um Diagnosedaten zu einer beliebigen Entität im Cluster abzurufen. Diese Daten können Sie bei den folgenden Aufgaben unterstützen:

* Diagnostizieren von Problemen bei der Entwicklung oder bei Tests oder an den Punkten, an denen Sie möglicherweise eine Überwachungspipeline verwenden.
* Sicherstellen, dass am Cluster vorgenommene Verwaltungsaktionen ordnungsgemäß verarbeitet werden.
* Abrufen einer „Momentaufnahme“ der Interaktion von Service Fabric mit der betreffenden Entität.

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

Unter [Service Fabric-Ereignisse](service-fabric-diagnostics-event-generation-operational.md) finden Sie eine vollständige Auflistung der in EventStore verfügbaren Ereignisse.

>[!NOTE]
>Ab Service Fabric-Version 6.2 sind die EventStore-APIs derzeit nur für unter Azure ausgeführte Windows-Cluster als Vorschauversion verfügbar. Wir arbeiten daran, diese Funktionalität sowohl für Linux als auch für unseren eigenständigen Cluster zu portieren.

Der EventStore-Dienst kann nach Ereignissen abgefragt werden, die in Ihrem Cluster für jede Entität und jeden Entitätstyp verfügbar sind. Dies bedeutet, dass Sie Ereignisse auf den folgenden Ebenen abfragen können:
* Cluster: Ereignisse in Bezug auf den Cluster selbst (z.B. Clusterupgrade).
* Knoten: Knotenereignisse auf allen Ebenen
* Knoten: Ereignisse, die spezifisch für einen bestimmten Knoten sind, der durch `nodeName` identifiziert wird.
* Anwendungen: Anwendungsereignisse auf allen Ebenen
* Anwendung: Ereignisse, die für eine durch `applicationId` identifizierte Anwendung spezifisch sind.
* Dienste: Ereignisse von allen Diensten im Cluster
* Dienst: Ereignisse von einem durch `serviceId` identifizierten bestimmten Dienst.
* Partitionen: Ereignisse von allen Partitionen
* Partition: Ereignisse von einer durch `partitionId` identifizierten bestimmten Partition.
* Partitionsreplikate: Ereignisse von allen Replikaten/Instanzen in einer bestimmten, durch `partitionId` identifizierten Partition.
* Partitionsreplikat: Ereignisse von einem bestimmten Replikat/einer bestimmten Instanz, das bzw. die durch `replicaId` und `partitionId` identifiziert wird.

Um mehr über die API zu erfahren, lesen Sie die [API-Referenz zu EventStore](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore).

Der EventStore-Dienst hat auch die Möglichkeit, Ereignisse in Ihrem Cluster zu korrelieren. Durch den Blick auf Ereignisse, die gleichzeitig von unterschiedlichen Entitäten geschrieben wurden und sich möglicherweise gegenseitig beeinträchtigt haben, kann der EventStore-Dienst diese Ereignisse verknüpfen und beim Identifizieren von Ursachen für Aktivitäten in Ihrem Cluster helfen. Wenn beispielsweise eine Anwendung ohne veranlasste Änderung fehlerhaft wird, sucht EventStore auch nach anderen Ereignissen, die von der Plattform bereitgestellt werden, und kann diese mit einem `Error`- oder `Warning`-Ereignis korrelieren. Das beschleunigt die Fehlererkennung und Analyse der Hauptursachen.

## <a name="enable-eventstore-on-your-cluster"></a>Aktivieren von EventStore für einen Cluster

### <a name="local-cluster"></a>Lokaler Cluster

Fügen Sie in der Datei [„fabricSettings.json“ in Ihrem Cluster](service-fabric-cluster-fabric-settings.md) EventStoreService als Add-On-Funktion hinzu, und führen Sie ein Clusterupgrade aus.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster"></a>Azure-Cluster

In der Azure Resource Manager-Vorlage Ihres Clusters können Sie den EventStore-Dienst aktivieren, indem Sie ein [Cluster-Konfigurations-Upgrade](service-fabric-cluster-config-upgrade-azure.md) durchführen und den folgenden Code hinzufügen. Sie können PlacementConstraints verwenden, um die Replikate des EventStore-Dienstes auf einen bestimmten NodeType zu festzulegen, z.B. einen NodeType, der für die Systemdienste bestimmt ist. Die Abschnitt `upgradeDescription` konfiguriert das Konfigurationsupgrade, um einen Neustart auf den Knoten auszulösen. Sie können den Abschnitt in einem weiteren Update entfernen.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              },
              {
                "name": "PlacementConstraints",
                "value": "(NodeType==<node_type_name_here>)"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit der EventStore-API: [Verwenden der EventStore-APIs in Azure Service Fabric-Clustern](service-fabric-diagnostics-eventstore-query.md)
* Weitere Informationen zur Liste der von EventStore bereitgestellten Ereignisse: [Service Fabric-Ereignisse](service-fabric-diagnostics-event-generation-operational.md)
* Eine Übersicht über Überwachung und Diagnose in Service Fabric finden Sie unter [Überwachungs- und Diagnosefunktionen für Service Fabric](service-fabric-diagnostics-overview.md).
* Anzeigen der vollständigen Liste der API-Aufrufe: [REST-API-Referenz zu EventStore](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Weitere Informationen zum Überwachen Ihres Clusters finden Sie unter [Überwachen des Clusters und der Plattform](service-fabric-diagnostics-event-generation-infra.md).

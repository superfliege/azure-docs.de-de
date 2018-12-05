---
title: Azure Service Fabric EventStore | Microsoft-Dokumentation
description: Informationen zu EventStore von Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: cd58e24a51b153d6bf217f7d32a82e882183ed73
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290670"
---
# <a name="eventstore-service-overview"></a>Übersicht über den EventStore-Dienst

## <a name="overview"></a>Übersicht

Der EventStore-Dienst, eingeführt in Version 6.2, ist eine Überwachungsoption in Service Fabric und bietet eine Möglichkeit, den Zustand des Clusters oder der Workloads zu einem bestimmten Zeitpunkt zu verstehen. Der EventStore-Dienst macht Service Fabric-Ereignisse über einen Satz von APIs (Zugriff über REST-Endpunkte oder über die Clientbibliothek) verfügbar. Diese EventStore-APIs ermöglichen es Ihnen, den Cluster direkt abzufragen, um Diagnosedaten zu einer Entität im Cluster abzurufen, Sie können bei folgenden Fragestellungen hilfreich sein:

* Diagnostizieren von Problemen bei der Entwicklung oder bei Tests oder an den Punkten, an denen Sie möglicherweise eine Überwachungspipeline verwenden.
* Sicherstellen, dass am Cluster vorgenommene Verwaltungsaktionen ordnungsgemäß vom Cluster ausgeführt werden.

Unter [Service Fabric-Ereignisse](service-fabric-diagnostics-event-generation-operational.md) finden Sie eine vollständige Auflistung der in EventStore verfügbaren Ereignisse.

>[!NOTE]
>Ab Service Fabric-Version 6.2 sind die EventStore-APIs derzeit nur für unter Azure ausgeführte Windows-Cluster als Vorschauversion verfügbar. Wir arbeiten daran, diese Funktionalität sowohl für Linux als auch für unseren eigenständigen Cluster zu portieren.

Der EventStore-Dienst kann nach Ereignissen abgefragt werden, die in Ihrem Cluster für jede Entität und jeden Entitätstyp verfügbar sind. Dies bedeutet, dass Sie Ereignisse auf den folgenden Ebenen abfragen können:
* Cluster: Clusterereignisse auf allen Ebenen
* Knoten: Knotenereignisse auf allen Ebenen
* Knoten: Ereignisse für einen bestimmten Knoten, anhand von `nodeName`
* Anwendungen: Anwendungsereignisse auf allen Ebenen
* Anwendung: Ereignisse für eine bestimmte Anwendung
* Dienste: Ereignisse von allen Diensten im Cluster
* Dienst: Ereignisse von einem bestimmten Dienst
* Partitionen: Ereignisse von allen Partitionen
* Partition: Ereignisse von einer bestimmten Partition
* Replikate: Ereignisse von allen Replikaten/Instanzen
* Replikat: Ereignisse von einem bestimmten Replikat/einer bestimmten Instanz


Der EventStore-Dienst hat auch die Möglichkeit, Ereignisse in Ihrem Cluster zu korrelieren. Durch den Blick auf Ereignisse, die gleichzeitig von unterschiedlichen Entitäten geschrieben wurden und sich möglicherweise gegenseitig beeinträchtigt haben, kann der EventStore-Dienst diese Ereignisse verknüpfen und beim Identifizieren von Ursachen für Aktivitäten in Ihrem Cluster helfen. Wenn beispielsweise eine Anwendung ohne veranlasste Änderung fehlerhaft wird, sucht EventStore auch nach anderen Ereignissen, die von der Plattform verfügbar gemacht werden, und kann diese mit einem `NodeDown`-Ereignis korrelieren. Das beschleunigt die Fehlererkennung und Analyse der Hauptursachen.

Wir empfehlen die Verwendung von EventStore für eine schnelle Analyse und eine Momentaufnahme der Arbeitsweise des Clusters. Anhand dieser Einblicke können Sie feststellen, ob alles erwartungsgemäß funktioniert.

Informationen zu ersten Schritten mit dem EventStore-Dienst finden Sie unter [Abfragen von EventStore-APIs nach Clusterereignissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Überwachung und Diagnose in Service Fabric finden Sie unter [Überwachungs- und Diagnosefunktionen für Service Fabric](service-fabric-diagnostics-overview.md).
* Weitere Informationen zum Überwachen Ihres Clusters finden Sie unter [Überwachen des Clusters und der Plattform](service-fabric-diagnostics-event-generation-infra.md).
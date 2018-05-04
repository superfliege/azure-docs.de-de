---
title: Migration von Azure Storage-Metriken | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Migration von Legacymetriken zu den neuen von Azure Monitor verwalteten Metriken.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 03/30/2018
ms.author: fryu
ms.openlocfilehash: c4dc9b231668315af16c625314c737fee99d672d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="azure-storage-metrics-migration"></a>Migration von Azure Storage-Metriken

In Übereinstimmung mit der Strategie zur Vereinheitlichung der Monitor-Oberfläche in Azure migriert Azure Storage Metriken zur Azure Monitor-Plattform. Der Dienst mit Legacymetriken wird in Zukunft mit vorheriger Benachrichtigung gemäß der Azure-Richtlinie eingestellt. Wenn Sie Legacyspeichermetriken benötigen, müssen Sie vor dem Enddatum des Diensts eine Migration durchführen, um Ihre Metrikdaten beizubehalten.

In diesem Artikel werden die Schritte zur Migration von Legacymetriken zu den neuen Metriken beschrieben.

## <a name="understand-legacy-metrics-managed-by-azure-storage"></a>Informationen zu den von Azure Storage verwalteten Legacymetriken

Azure Storage sammelt Legacymetrikwerte, aggregiert diese und speichert sie im Speicherkonto in $Metric-Tabellen. Über das Azure-Portal können Sie Überwachungsdiagramme einrichten, während Sie mit Azure Storage-SDKs auch die Daten aus $Metric-Tabellen basierend auf dem Schema lesen können. Weitere Einzelheiten finden Sie in diesem Artikel zu [Storage Analytics](./storage-analytics.md).

Legacymetriken bieten Kapazitätsmetriken nur für Blobs und Transaktionsmetriken für Blobs, Tabellen, Dateien und Warteschlangen.

Legacymetriken werden im Flatfile-Schema entworfen. Wenn die Metrik nicht durch die Datenverkehrsmuster ausgelöst wird, weist der Entwurf einen Metrikwert von Null auf. **ServerTimeoutError** ist in $Metric-Tabellen beispielsweise auf 0 festgelegt, auch wenn Sie keine Servertimeoutfehler beim Livedatenverkehr zum Speicherkonto erhalten.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Informationen zu den von Azure Monitor verwalteten neuen Metriken

Für neue Speichermetriken gibt Azure Storage die Metrikdaten an das Azure Monitor-Back-End aus. Azure Monitor bietet eine einheitliche Überwachungsoberfläche, auch für Daten aus dem Portal sowie für die Datenerfassung. Weitere Einzelheiten finden Sie in [diesem Artikel](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Neue Metriken bieten Kapazitäts- und Transaktionsmetriken für Blobs, Tabellen, Dateien, Warteschlangen und Storage Premium.

Das Feature für mehrere Dimensionen ist eines der von Azure Monitor bereitgestellten Features. Azure Storage übernimmt den Entwurf bei der Definition des Schemas für neue Metriken. Informationen zu unterstützten Dimensionen für Metriken finden Sie in diesem Artikel zu [Azure Storage-Metriken in Azure Monitor](./storage-metrics-in-azure-monitor.md). Ein mehrdimensionaler Entwurf bietet Kosteneffizienz sowohl im Hinblick auf den Bandbreitenverbrauch bei der Erfassung als auch im Hinblick auf die Kapazität bei der Speicherung von Metriken. Wenn Ihr Datenverkehr keine entsprechenden Metriken ausgelöst hat, werden die zugehörigen Metrikdaten infolgedessen nicht generiert. Beispiel: Wenn Ihr Datenverkehr keine Servertimeoutfehler ausgelöst hat, gibt Azure Monitor keine Daten zurück, wenn Sie den Wert der Metrik **Transactions** mit der Dimension **ResponseType** gleich **ServerTimeoutError** abfragen.

## <a name="metrics-mapping-between-legacy-metrics-and-new-metrics"></a>Zuordnung von Metriken zwischen Legacymetriken und neuen Metriken

Wenn Sie Metrikdaten programmgesteuert lesen, müssen Sie das Schema für neue Metriken in Ihren Programmen verwenden. Zur Verdeutlichung der Änderungen wird in der folgenden Tabelle die entsprechende Zuordnung aufgeführt:

**Kapazitätsmetriken**

| Legacymetrik | Neue Metrik |
| ------------------- | ----------------- |
| Capacity            | BlobCapacity mit der Dimension „BlobType“ gleich BlockBlob oder PageBlob |
| ObjectCount         | BlobCount mit der Dimension „BlobType“ gleich BlockBlob oder PageBlob |
| ContainerCount      | ContainerCount |

Die folgenden Metriken sind neue Angebote, die Legacymetriken nicht unterstützen:
* TableCapacity
* TableCount
* TableEntityCount
* QueueCapacity
* QueueCount
* QueueMessageCount
* FileCapacity
* FileCount
* FileShareCount
* UsedCapacity

**Transaktionsmetriken**

| Legacymetrik | Neue Metrik |
| ------------------- | ----------------- |
| AnonymousAuthorizationError | Transactions mit der Dimension „ResponseType“ gleich AuthorizationError |
| AnonymousClientOtherError | Transactions mit der Dimension „ResponseType“ gleich ClientOtherError |
| AnonymousClientTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ClientTimeoutError |
| AnonymousNetworkError | Transactions mit der Dimension „ResponseType“ gleich NetworkError |
| AnonymousServerOtherError | Transactions mit der Dimension „ResponseType“ gleich ServerOtherError |
| AnonymousServerTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ServerTimeoutError |
| AnonymousSuccess | Transactions mit der Dimension „ResponseType“ gleich Success |
| AnonymousThrottlingError | Transactions mit der Dimension „ResponseType“ gleich ClientThrottlingError oder ServerBusyError |
| AuthorizationError | Transactions mit der Dimension „ResponseType“ gleich AuthorizationError |
| Verfügbarkeit | Verfügbarkeit |
| AverageE2ELatency | SuccessE2ELatency |
| AverageServerLatency | SuccessServerLatency |
| ClientOtherError | Transactions mit der Dimension „ResponseType“ gleich ClientOtherError |
| ClientTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ClientTimeoutError |
| NetworkError | Transactions mit der Dimension „ResponseType“ gleich NetworkError |
| PercentAuthorizationError | Transactions mit der Dimension „ResponseType“ gleich AuthorizationError |
| PercentClientOtherError | Transactions mit der Dimension „ResponseType“ gleich ClientOtherError |
| PercentNetworkError | Transactions mit der Dimension „ResponseType“ gleich NetworkError |
| PercentServerOtherError | Transactions mit der Dimension „ResponseType“ gleich ServerOtherError |
| PercentSuccess | Transactions mit der Dimension „ResponseType“ gleich Success |
| PercentThrottlingError | Transactions mit der Dimension „ResponseType“ gleich ClientThrottlingError oder ServerBusyError |
| PercentTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ServerTimeoutError oder ClientTimeoutError|
| SASAuthorizationError | Transactions mit der Dimension „ResponseType“ gleich AuthorizationError |
| SASClientOtherError | Transactions mit der Dimension „ResponseType“ gleich ClientOtherError |
| SASClientTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ClientTimeoutError |
| SASNetworkError | Transactions mit der Dimension „ResponseType“ gleich NetworkError |
| SASServerOtherError | Transactions mit der Dimension „ResponseType“ gleich ServerOtherError |
| SASServerTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ServerTimeoutError |
| SASSuccess | Transactions mit der Dimension „ResponseType“ gleich Success |
| SASThrottlingError | Transactions mit der Dimension „ResponseType“ gleich ClientThrottlingError oder ServerBusyError |
| ServerOtherError | Transactions mit der Dimension „ResponseType“ gleich ServerOtherError |
| ServerTimeoutError | Transactions mit der Dimension „ResponseType“ gleich ServerTimeoutError |
| Erfolgreich | Transactions mit der Dimension „ResponseType“ gleich Success |
| ThrottlingError | Transactions mit der Dimension „ResponseType“ gleich ClientThrottlingError oder ServerBusyError |
| TotalBillableRequests | Transaktionen |
| TotalEgress | Ausgehende Daten |
| TotalIngress | Eingehende Daten |
| TotalRequests | Transaktionen |

## <a name="faq"></a>Häufig gestellte Fragen

* Wie sollte ich vorhandene Warnungsregeln migrieren?

A: Wenn Sie klassische Warnungsregeln basierend auf Legacyspeichermetriken erstellt haben, müssen Sie neue Warnungsregeln basierend auf dem Schema für neue Metriken erstellen.

* Werden neue Metrikdaten standardmäßig im selben Speicherkonto gespeichert?

A: Nein. Wenn Sie die Metrikdaten im Speicherkonto archivieren müssen, können Sie die [Diagnoseeinstellung in Azure Monitor](https://azure.microsoft.com/blog/azure-monitor-multiple-diagnostic-settings/) verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Storage-Metriken in Azure Monitor](./storage-metrics-in-azure-monitor.md)

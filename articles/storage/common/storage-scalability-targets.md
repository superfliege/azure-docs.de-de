---
title: Skalierbarkeits- und Leistungsziele für Azure Storage – Speicherkonten
description: Informationen zu den Skalierbarkeits- und Leistungszielen, einschließlich Kapazität, Anforderungsrate sowie eingehende und ausgehende Bandbreite für Azure-Speicherkonten.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: e3e0e9ae4a1939aad9ab2ae42a1b51b1b00e2462
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521715"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Skalierbarkeits- und Leistungsziele für Speicherkonten in Azure Storage

In diesem Artikel werden die Skalierbarkeits- und Leistungsziele für Azure-Speicherkonten erläutert. Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab.

Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.

Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. Wenn 503-Fehler auftreten, sollte die Anwendung so geändert werden, dass sie eine exponentielle Backoffrichtlinie für Wiederholungen verwendet. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.

## <a name="storage-account-scale-limits"></a>Skalierungslimits für Speicherkonten

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Skalierungslimits für Speicherkonten der Leistungsstufe „Premium“

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Skalierungslimits für Speicherressourcenanbieter

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Blob Storage

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files

Weitere Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files und die Azure-Dateisynchronisierung finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="premium-files-scale-targets"></a>Skalierbarkeitsziele für Premiumdateien

Es gibt drei Kategorien von Einschränkungen, die für Premium-Dateien zu berücksichtigen sind: Speicherkonten, Freigaben und Dateien.

Beispiel:  Eine einzelne Freigabe kann einen IOPS von 100.000 erreichen, und eine einzelne Datei kann bis zu einem IOPS von 5.000 zentral hochskalieren. Wenn Sie beispielsweise drei Dateien in einer Freigabe haben, entspricht der maximale IOPS aus dieser Freigabe 15.000.

#### <a name="premium-file-share-limits"></a>Freigabelimits für Premiumdateien

> [!IMPORTANT]
> Die Einschränkungen für Speicherkonten gelten für alle Freigaben. Zentrales Hochskalieren bis zum Maximalwert für Speicherkonten ist nur möglich, wenn es nur eine Freigabe pro Speicherkonto gibt.

|Bereich  |Ziel  |
|---------|---------|
|Bereitgestellte minimale Größe                        |100 GB      |
|Bereitgestellte maximale Größe                        |ca. 100 TiB      |
|Mindestgröße vergrößern/verkleinern    |1 GiB      |
|IOPS-Grundwert    |1 IOPS pro GiB bis zu 100.000|
|IOPS-Bursting    |3 x IOPS pro GiB bis zu 100.000|
|Ausgangsrate         |60 MiB/s + 0,06 * bereitgestelltes GiB        |
|Eingangsrate| 40 MiB/s + 0,04 * bereitgestelltes GiB |
|Maximale Anzahl von Momentaufnahmen        |200       |

#### <a name="premium-file-limits"></a>Grenzwerte für Premiumdateien

|Bereich  |Ziel  |
|---------|---------|
|Größe                  |1 TiB         |
|Max. IOPS pro Datei     |5.000         |
|Gleichzeitige Handles    |2.000         |

### <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für die Azure-Dateisynchronisierung

Azure File Sync wurde mit dem Ziel der unbegrenzten Nutzung entwickelt, aber unbegrenzte Nutzung ist nicht immer möglich. Die folgende Tabelle gibt an, welche Grenzen für Tests von Microsoft gelten und welche Ziele feste Grenzwerte sind:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Queue Storage

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Skalierbarkeitsziele für Azure-Tabellen

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Weitere Informationen

- [Speicher – Preisdetails](https://azure.microsoft.com/pricing/details/storage/)
- [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md)
- [Azure Storage-Replikation](../storage-redundancy.md)
- [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage-performance-checklist.md)
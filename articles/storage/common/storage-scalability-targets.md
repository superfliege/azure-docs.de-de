---
title: Skalierbarkeits- und Leistungsziele für Azure Storage
description: Informationen zu den Skalierbarkeits- und Leistungszielen, einschließlich Kapazität, Anforderungsrate sowie eingehende und ausgehende Bandbreite für standardmäßige Azure-Speicherkonten.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 11/08/2018
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 2f00b01bb07aafca847897f0c31d24d4add7cdbf
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328876"
---
# <a name="azure-storage-scalability-and-performance-targets-for-standard-storage-accounts"></a>Skalierbarkeits- und Leistungsziele für standardmäßige Speicherkonten

In diesem Artikel werden die Skalierbarkeits- und Leistungsziele für Azure-Speicherkonten erläutert. Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab. 

Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.

Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. Wenn 503-Fehler auftreten, sollte die Anwendung so geändert werden, dass sie eine exponentielle Backoffrichtlinie für Wiederholungen verwendet. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.

## <a name="standard-storage-account-scale-limits"></a>Skalierungslimits für Standard-Speicherkonto
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-storage-account-scale-limits"></a>Skalierungslimits für Storage Premium-Konto
[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Skalierungslimits für Speicherressourcenanbieter

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Blob Storage
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files
Weitere Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files und die Azure-Dateisynchronisierung finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für die Azure-Dateisynchronisierung
Azure File Sync wurde mit dem Ziel der unbegrenzten Nutzung entwickelt, aber unbegrenzte Nutzung ist nicht immer möglich. Die folgende Tabelle gibt an, welche Grenzen für Tests von Microsoft gelten und welche Ziele feste Grenzwerte sind:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Queue Storage
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Skalierbarkeitsziele für Azure-Tabellen
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Siehe auch
* [Speicher – Preisdetails](https://azure.microsoft.com/pricing/details/storage/)
* [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md)
* [Azure Storage-Replikation](../storage-redundancy.md)
* [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage-performance-checklist.md)


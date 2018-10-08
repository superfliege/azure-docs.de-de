---
title: Skalierbarkeits- und Leistungsziele für Azure Storage | Microsoft Docs
description: Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Storage, einschließlich Kapazität Anforderungsrate sowie eingehende und ausgehende Bandbreite für Standard- und Premium-Speicherkonten. Machen Sie sich mit den Leistungszielen für Partitionen in den einzelnen Azure Storage-Diensten vertraut.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: a49ad2546babe8bef6b52be1d8a2dbaceec94003
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391801"
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Storage
## <a name="overview"></a>Übersicht
In diesem Artikel werden die Skalierbarkeits- und Leistungsaspekte von Azure Storage beschrieben. Eine Zusammenfassung anderer Einschränkungen von Azure, finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md).

> [!NOTE]
> Alle Speicherkonten werden in der neuen, flachen Netzwerktopologie ausgeführt und unterstützen die in diesem Artikel beschriebenen Skalierbarkeits- und Leistungsziele, unabhängig vom Zeitpunkt ihrer Erstellung. Weitere Informationen zur flachen Netzwerkarchitektur von Azure Storage sowie zur Skalierbarkeit finden Sie unter [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)(in englischer Sprache).
> 

> [!IMPORTANT]
> Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab. Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.
> 
> Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. Wenn diese Fehler auftreten, sollte die Anwendung eine exponentielle Backoffrichtlinie für Wiederholungen verwenden. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.
> 
> 

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, können Sie die Anwendung so erstellen, dass mehrere Speicherkonten verwendet werden. Sie können Ihre Datenobjekte dann basierend auf diesen Speicherkonten partitionieren. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

## <a name="scalability-targets-for-a-storage-account"></a>Skalierbarkeitsziele für ein Speicherkonto
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

### <a name="storage-resource-provider-limits"></a>Grenzwerte für Speicherressourcenanbieter 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Blob Storage
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files
Weitere Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files und die Azure-Dateisynchronisierung finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für die Azure-Dateisynchronisierung
Bei der Azure-Dateisynchronisierung haben wir als Entwicklungsziel eine unbegrenzte Nutzung vorgesehen, was jedoch nicht immer möglich ist. Die folgende Tabelle zeigt die bei unseren Tests ermittelten Grenzwerte und für welche Ziele tatsächlich feste Grenzen gelten:

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
* [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (in englischer Sprache)](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)


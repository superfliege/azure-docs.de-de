---
title: Datenreplikation in Azure Storage | Microsoft-Dokumentation
description: Die Daten in Ihrem Microsoft Azure Storage-Konto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Die Redundanzoptionen umfassen den lokal redundanten Speicher (LRS), den zonenredundanten Speicher (ZRS), den georedundanten Speicher (GRS) und den georedundanten Speicher mit Lesezugriff (RA-GRS).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: bdb9bfaa85f526af0c5e42294a75664fa7137849
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="azure-storage-replication"></a>Azure Storage-Replikation

Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Die Azure Storage-Replikation kopiert Ihre Daten so, dass sie vor geplanten und ungeplanten Ereignissen geschützt sind – von vorübergehend auftretenden Hardwarefehlern und Netzwerk- oder Stromausfällen bis zu schweren Naturkatastrophen usw. Sie können Ihre Daten wahlweise im selben Rechenzentrum, Rechenzentren in derselben Zone und sogar Regionen übergreifend replizieren.

Mit der Replikation wird sichergestellt, dass Ihr Speicherkonto auch bei Ausfällen die [Servicelevelvereinbarung (SLA) für Storage](https://azure.microsoft.com/support/legal/sla/storage/) erfüllt. Die Servicelevelvereinbarung enthält Informationen zu Azure Storage-Garantien in Bezug auf Dauerhaftigkeit und Verfügbarkeit.

## <a name="choosing-a-replication-option"></a>Auswählen einer Replikationsoption

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Replikationsoptionen auswählen:

* [Lokal redundanter Speicher (LRS)](storage-redundancy-lrs.md)
* [Zonenredundanter Speicher (ZRS)](storage-redundancy-zrs.md)
* [Georedundanter Speicher (GRS)](storage-redundancy-grs.md)
* [Georedundanter Speicher mit Lesezugriff (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

Die folgende Tabelle zeigt in einer kurzen Übersicht, in welchem Umfang die jeweilige Replikationsstrategie bei einem bestimmten Ereignistyp (oder einem Ereignis ähnlicher Auswirkung) Dauerhaftigkeit und Verfügbarkeit bietet.

| Szenario | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Nichtverfügbarkeit von Knoten innerhalb eines Rechenzentrums |Ja |Ja |Ja |Ja
| Ein gesamtes Rechenzentrum (zonal oder nicht zonal) ist nicht mehr verfügbar. |Nein  |Ja |Ja |Ja |
| Ein regionsweiter Ausfall |Nein  |Nein  |Ja |Ja |
| Lesezugriff auf Ihre Daten (in einer georeplizierten Remoteregion) bei regionsweiter Nichtverfügbarkeit |Nein  |Nein  |Nein  |Ja |
| Konzipiert, um ___ Dauerhaftigkeit von Objekten über ein bestimmtes Jahr zu bieten. |mindestens 99,999999999 % (11 mal die 9)|mindestens 99,9999999999 % (12 mal die 9)|mindestens 99,99999999999999 % (16 mal die 9)|mindestens 99,99999999999999 % (16 mal die 9)|
| Verfügbar in ___ Speicherkontotypen |GPv1, GPv2, Blob |GPv2 |GPv1, GPv2, Blob |GPv1, GPv2, Blob

Informationen zu den Preisen für die verschiedenen Redundanzoptionen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

> [!NOTE]
> Storage Premium unterstützt nur lokal redundanten Speicher (Locally Redundant Storage, LRS). Informationen zu Storage Premium finden Sie unter [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer](../../virtual-machines/windows/premium-storage.md).

## <a name="changing-replication-strategy"></a>Ändern der Replikationsstrategie
Wir erlauben Ihnen, die Replikationsstrategie für Ihr Speicherkonto mithilfe von [Azure-Portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) oder einer der vielen [ Azure-Clientbibliotheken](https://docs.microsoft.com/en-us/azure/index?view=azure-dotnet#pivot=sdkstools) zu ändern. Bei einer Änderung des Replikationstyps Ihres Speicherkontos treten keine Ausfallzeiten auf.

   > [!NOTE]
   > Derzeit können Sie weder das Portal noch die API verwenden, um Ihr Konto in ZRS zu konvertieren. Wir planen jedoch die Unterstützung des Migrierens zu ZRS von LRS, GRS und RA-GRS, sobald das Migrieren zu ZRS allgemein verfügbar ist. Weitere Informationen siehe [Zonenredundanter Speicher (ZRS)](storage-redundancy-zrs.md).
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Entstehen mir Kosten für die Änderung der Replikationsstrategie meines Kontos?
Es hängt von Ihrem Konvertierungspfad ab. Vom billigsten bis zum teuersten Redundanzangebot bieten wir LRS, ZRS, GRS und RA-GRS an. Beispielsweise fallen beim Wechsel *von* LRS zu einem beliebigen anderen Angebot zusätzliche Kosten an, da Sie in eine komplexere Redundanzebene wechseln. Beim Wechsel *zu* GRS oder RA-GRS fallen Kosten für eine ausgehende Bandbreite an, da Ihre Daten (in Ihrer primären Region) in Ihre sekundäre Remoteregion repliziert werden. Dies sind einmalige Kosten bei der anfänglichen Einrichtung. Nachdem die Daten kopiert sind, fallen keine weiteren Kosten für die Konvertierung an. Ihnen werden nur ggf. die Replikation neuer oder die Aktualisierung vorhandener Daten in Rechnung gestellt. Ausführliche Informationen zu Bandbreitengebühren finden Sie auf der [Seite mit Informationen zu Azure Storage-Preisen](https://azure.microsoft.com/pricing/details/storage/blobs/).

Wenn Sie von GRS zu LRS wechseln, entstehen keine zusätzlichen Kosten, aber Ihre replizierten Daten werden am sekundären Standort gelöscht.

## <a name="see-also"></a>Weitere Informationen

- [Lokal redundanter Speicher (LRS): Datenredundanz für Azure Storage zu geringen Kosten](storage-redundancy-lrs.md)
- [Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen](storage-redundancy-zrs.md)
- [Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md)
- [Skalierbarkeits- und Leistungsziele für Azure-Speicher](storage-scalability-targets.md)
- [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS-Speicher](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Storage Redundancy Options and Read Access Geo Redundant Storage (in englischer Sprache) ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (SOSP-Dokument – Azure Storage: ein hochverfügbarer Cloudspeicherdienst mit starker Konsistenz)](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

---
title: Georedundanter Speicher (GRS) für regionsübergreifende Dauerhaftigkeit in Azure Storage | Microsoft-Dokumentation
description: Georedundante Speicher (GRS) replizieren Ihre Daten zwischen zwei Regionen, die Hunderte von Kilometern voneinander entfernt sind. GRS schützen vor Hardwareausfällen im Rechenzentrum sowie regionalen Notfällen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 85d69db2f94e4bddf1258233c34c64dcf78a3eeb
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219221"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Georedundanter Speicher mit Lesezugriff
Georedundante Speicher mit Lesezugriff (RA-GRS) maximieren die Verfügbarkeit Ihres Speicherkontos. RA-GRS bieten neben der Georeplikation zwischen zwei Regionen schreibgeschützten Zugriff auf die Daten am sekundären Standort.

Wenn Sie den schreibgeschützten Zugriff auf Ihre Daten in der sekundären Region aktivieren, sind Ihre Daten zusätzlich zum primären Endpunkt für Ihr Speicherkonto auf einem sekundären Endpunkt verfügbar. Der sekundäre Endpunkt ähnelt dem primären Endpunkt, wobei das Suffix `–secondary` an den Kontonamen angefügt wird. Wenn Ihr primärer Endpunkt für den Blob-Dienst z. B. `myaccount.blob.core.windows.net` ist, dann ist Ihr sekundärer Endpunkt `myaccount-secondary.blob.core.windows.net`. Die Zugriffsschlüssel für das Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Bei der Nutzung von RA-GRS sollten Sie einige Aspekte berücksichtigen:

* Von Ihrer Anwendung muss verwaltet werden, mit welchem Endpunkt sie bei Verwendung von RA-GRS interagieren muss.
* Da die asynchrone Replikation eine Verzögerung beinhaltet, gehen Änderungen, die noch nicht in der sekundären Region repliziert wurden, möglicherweise verloren, wenn Daten nicht aus der primären Region wiederhergestellt werden können.
* Sie können für Ihr Speicherkonto den Zeitpunkt der letzten Synchronisierung überprüfen. Bei dem Zeitpunkt der letzten Synchronisierung handelt es sich um einen Datums-/Uhrzeitwert in GMT. Alle primären Schreibvorgänge vor dem Zeitpunkt der letzten Synchronisierung wurden erfolgreich an den sekundären Standort geschrieben. Dies bedeutet, dass sie am sekundären Standort zur Verfügung stehen. Primäre Schreibvorgänge, die nach dem Zeitpunkt der letzten Synchronisierung stattgefunden haben, stehen unter Umständen noch nicht zum Lesen zur Verfügung. Sie können diesen Wert mit dem [Azure-Portal](https://portal.azure.com/), mit [Azure PowerShell](storage-powershell-guide-full.md) oder mit einer der Azure Storage-Clientbibliotheken abfragen.
* Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](storage-disaster-recovery-guidance.md).
* Weitere Informationen dazu, wie Sie in die sekundäre Region wechseln, finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](storage-disaster-recovery-guidance.md).
* RA-GRS ist für hohe Verfügbarkeit ausgelegt. Einen Leitfaden zur Skalierbarkeit finden Sie in der [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](storage-performance-checklist.md).
* Empfehlungen dazu, wie Sie mit RA-GRS Hochverfügbarkeit erreichen können, finden Sie unter [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Was ist die RPO und RTO in Bezug auf GRS?
**Recovery Point Objective (RPO)**: Bei GRS und RA-GRS führt der Speicherdienst eine asynchrone Georeplikation der Daten vom primären zum sekundären Standort durch. Bei einem schwerwiegenden regionalen Notfall in der primären Region führt Microsoft ein Failover zur sekundären Region durch. Bei einem Failover können kürzlich vorgenommene Änderungen, die noch nicht georepliziert wurden, verloren gehen. Die Anzahl von Minuten, in denen Daten potenziell verloren gehen können, wird als RPO bezeichnet. Die RPO gibt den Zeitpunkt an, auf den Daten wiederhergestellt werden können. Azure Storage weist normalerweise einen RPO-Wert von weniger als 15 Minuten auf, aber es gibt derzeit keine SLA zur Dauer der Georeplikation.

**Recovery Time Objective (RTO)**: Die RTO ist eine Kennzahl dafür, wie lange es dauert, bis das Failover durchgeführt wurde und das Speicherkonto wieder online ist. Die Zeit für die Durchführung des Failovers umfasst folgende Aktionen:

   * Feststellung seitens Microsoft, ob die Daten am primären Speicherort wiederhergestellt werden können oder ob ein Failover erforderlich ist
   * Durchführung eines Failovers des Speicherkontos, indem die primären DNS-Einträge so geändert werden, dass sie auf den sekundären Standort verweisen

Microsoft nimmt seine Verantwortung bezüglich der Speicherung Ihrer Daten sehr ernst. Falls eine Möglichkeit zur Wiederherstellung der Daten in der primären Region besteht, verschiebt Microsoft das Failover und konzentriert sich zunächst auf die Wiederherstellung Ihrer Daten. 

## <a name="paired-regions"></a>Regionspaare 
Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto aus. Die gekoppelte sekundäre Region wird basierend auf der primären Region bestimmt und kann nicht geändert werden. Weitere aktuelle Informationen zu unterstützten Azure-Regionen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Weitere Informationen
- [Azure Storage-Replikation](storage-redundancy.md)
- [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](storage-redundancy-lrs.md)
- [Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen](storage-redundancy-zrs.md)
---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f5b6e0e74bbab33b9ae6fbacca5c55ea434d3e41
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399956"
---
Georedundanter Speicher (GRS) ist so konzipiert, dass er eine Dauerhaftigkeit von mindestens 99,99999999999999999999 % (16 mal die 9) von Objekten über ein gegebenes Jahr bereitstellt, indem Ihre Daten in eine sekundäre Region repliziert werden, die Hunderte Kilometer von der primären Region entfernt ist. Wenn für Ihr Speicherkonto GRS aktiviert ist, sind Ihre Daten beständig gespeichert, selbst bei einem regionalen Komplettausfall oder einem Notfall, nach dem die primäre Region nicht mehr wiederherstellbar ist.

Wenn Sie sich für GRS entscheiden, haben Sie zwei Optionen zur Auswahl:

* GRS repliziert Ihre Daten in ein anderes Rechenzentrum in einer sekundären Region, aber diese Daten sind nur schreibgeschützt verfügbar, wenn Microsoft ein Failover von der primären in die sekundäre Region initiiert. 
* Georedundanter Speicher mit Lesezugriff (RA-GRS) basiert auf GRS. RA-GRS repliziert Ihre Daten in ein anderes Rechenzentrum in einer sekundären Region und bietet Ihnen die Möglichkeit, aus der sekundären Region zu lesen. Mit RA-GRS können Sie aus der sekundären Region lesen, unabhängig davon, ob Microsoft ein Failover von der primären Region in die sekundäre initiiert. 

Bei einem Speicherkonto mit aktiviertem GRS oder RA-GRS werden alle Daten zunächst mit dem lokal redundanten Speicher (LRS) repliziert. Ein Update wird zunächst an den primären Speicherort übertragen und mit LRS repliziert. Anschließend wird das Update asynchron mit GRS in die sekundäre Region repliziert. Wenn Daten in den sekundären Speicherort geschrieben werden, werden sie dort auch mit LRS repliziert. 

Sowohl in der primären als auch in der sekundären Region werden Replikate über separate Fehlerdomänen und Upgradedomänen hinweg in einer Speicherskalierungseinheit verwaltet. Die Speicherskalierungseinheit ist die grundlegende Replikationseinheit im Datencenter. Die Replikation auf dieser Ebene wird von LRS bereitgestellt. Weitere Informationen finden Sie unter [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Beachten Sie diese Punkte, wenn Sie sich für eine Replikationsoption entscheiden:

* Zonenredundanter Speicher (ZRS) bietet eine hohe Verfügbarkeit mit synchroner Replikation und ist für einige Szenarien ggf. besser geeignet als GRS oder RA-GRS. Weitere Informationen zu ZRS finden Sie unter [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen bei einem regionalen Notfall Änderungen, die noch nicht in der sekundären Region repliziert wurden, unter Umständen verloren, wenn die Daten nicht aus der primären Region wiederhergestellt werden können.
* Mit GRS ist das Replikat nicht verfügbar, sofern von Microsoft kein Failover in die sekundäre Region initiiert wird. Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie im [Leitfaden zur Notfallwiederherstellung](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Wenn Ihre Anwendung aus der sekundären Region lesen soll, aktivieren Sie RA-GRS.

## <a name="read-access-geo-redundant-storage"></a>Georedundanter Speicher mit Lesezugriff

Georedundante Speicher mit Lesezugriff (RA-GRS) maximieren die Verfügbarkeit Ihres Speicherkontos. RA-GRS bieten neben der Georeplikation zwischen zwei Regionen schreibgeschützten Zugriff auf die Daten am sekundären Standort.

Wenn Sie den schreibgeschützten Zugriff auf Ihre Daten in der sekundären Region aktivieren, sind Ihre Daten zusätzlich zum primären Endpunkt für Ihr Speicherkonto auf einem sekundären Endpunkt verfügbar. Der sekundäre Endpunkt ähnelt dem primären Endpunkt, wobei das Suffix `–secondary` an den Kontonamen angefügt wird. Wenn Ihr primärer Endpunkt für den Blob-Dienst z. B. `myaccount.blob.core.windows.net` ist, dann ist Ihr sekundärer Endpunkt `myaccount-secondary.blob.core.windows.net`. Die Zugriffsschlüssel für das Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Bei der Nutzung von RA-GRS sollten Sie einige Aspekte berücksichtigen:

* Von Ihrer Anwendung muss verwaltet werden, mit welchem Endpunkt sie bei Verwendung von RA-GRS interagieren muss.
* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen Änderungen, die noch nicht in der sekundären Region repliziert wurden, möglicherweise verloren, wenn Daten beispielsweise bei einem regionalen Notfall nicht aus der primären Region wiederhergestellt werden können.
* Sie können für Ihr Speicherkonto den Zeitpunkt der letzten Synchronisierung überprüfen. Bei dem Zeitpunkt der letzten Synchronisierung handelt es sich um einen Datums-/Uhrzeitwert in GMT. Alle primären Schreibvorgänge vor dem Zeitpunkt der letzten Synchronisierung wurden erfolgreich an den sekundären Standort geschrieben. Dies bedeutet, dass sie am sekundären Standort zur Verfügung stehen. Primäre Schreibvorgänge, die nach dem Zeitpunkt der letzten Synchronisierung stattgefunden haben, stehen unter Umständen noch nicht zum Lesen zur Verfügung. Sie können diesen Wert mit dem [Azure-Portal](https://portal.azure.com/), mit [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md) oder mit einer der Azure Storage-Clientbibliotheken abfragen.
* Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Weitere Informationen dazu, wie Sie in die sekundäre Region wechseln, finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS ist für hohe Verfügbarkeit ausgelegt. Einen Leitfaden zur Skalierbarkeit finden Sie in der [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../articles/storage/common/storage-performance-checklist.md).
* Empfehlungen dazu, wie Sie mit RA-GRS Hochverfügbarkeit erreichen können, finden Sie unter [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Was ist die RPO und RTO in Bezug auf GRS?
**Recovery Point Objective (RPO)**: Bei GRS und RA-GRS führt der Speicherdienst eine asynchrone Georeplikation der Daten vom primären zum sekundären Standort durch. Bei einem schwerwiegenden regionalen Notfall in der primären Region führt Microsoft ein Failover zur sekundären Region durch. Bei einem Failover können kürzlich vorgenommene Änderungen, die noch nicht georepliziert wurden, verloren gehen. Die Anzahl von Minuten des Zeitraums, für den ein potenzieller Datenverlust auftritt, wird als RPO bezeichnet und gibt den Zeitpunkt an, bis zu dem Daten wiederhergestellt werden können. Azure Storage weist normalerweise einen RPO-Wert von weniger als 15 Minuten auf, aber es gibt derzeit keine SLA zur Dauer der Georeplikation.

**Recovery Time Objective (RTO)**: Die RTO ist eine Kennzahl dafür, wie lange es dauert, bis das Failover durchgeführt wurde und das Speicherkonto wieder online ist. Die Zeit für die Durchführung des Failovers umfasst folgende Aktionen:

   * Feststellung seitens Microsoft, ob die Daten am primären Speicherort wiederhergestellt werden können oder ob ein Failover erforderlich ist
   * Durchführung eines Failovers des Speicherkontos, indem die primären DNS-Einträge so geändert werden, dass sie auf den sekundären Standort verweisen

   Microsoft nimmt seine Verantwortung bezüglich der Speicherung Ihrer Daten sehr ernst. Falls eine Möglichkeit zur Wiederherstellung der Daten in der primären Region besteht, verschiebt Microsoft das Failover und konzentriert sich zunächst auf die Wiederherstellung Ihrer Daten. In zukünftigen Versionen des Diensts wird die Auslösung eines Failovers auf Kontoebene ermöglicht, sodass Sie die RTO selbst steuern können.

## <a name="paired-regions"></a>Regionspaare 

Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto aus. Die gekoppelte sekundäre Region wird basierend auf der primären Region bestimmt und kann nicht geändert werden. Weitere aktuelle Informationen zu unterstützten Azure-Regionen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](../articles/best-practices-availability-paired-regions.md).

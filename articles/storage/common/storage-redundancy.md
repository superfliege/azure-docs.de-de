---
title: Datenreplikation in Azure Storage | Microsoft-Dokumentation
description: Die Daten in Ihrem Microsoft Azure Storage-Konto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Die Redundanzoptionen umfassen den lokal redundanten Speicher (LRS), den zonenredundanten Speicher (ZRS), den georedundanten Speicher (GRS) und den georedundanten Speicher mit Lesezugriff (RA-GRS).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 01/21/2018
ms.author: tamram
ms.openlocfilehash: 600b66af3b7da24c5a40d09d5cdf76f2d5be67ac
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-storage-replication"></a>Azure Storage-Replikation

Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Bei der Replikation werden Ihre Daten zum Schutz vor vorübergehenden Hardwarefehlern kopiert, wodurch die Betriebszeit Ihre Anwendung verlängert wird. 

Sie können Daten im selben Rechenzentrum, zwischen verschiedenen Rechenzentren in derselben Region oder über verschiedene Regionen hinweg replizieren. Wenn Ihre Daten über verschiedene Rechenzentren oder Regionen hinweg repliziert werden, sind diese auch vor schwerwiegenden Fehlern an einem einzelnen Standort geschützt.

Mit der Replikation wird sichergestellt, dass Ihr Speicherkonto auch bei Ausfällen die [Servicelevelvereinbarung (SLA) für Storage](https://azure.microsoft.com/support/legal/sla/storage/) erfüllt. Die Servicelevelvereinbarung enthält Informationen zu Azure Storage-Garantien in Bezug auf Dauerhaftigkeit und Verfügbarkeit.

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Replikationsoptionen auswählen:

* [Lokal redundanter Speicher (LRS)](#locally-redundant-storage)
* [Zonenredundanter Speicher (ZRS)](#zone-redundant-storage)
* [Georedundanter Speicher (GRS)](#geo-redundant-storage)
* [Georedundanter Speicher mit Lesezugriff (RA-GRS)](#read-access-geo-redundant-storage)

Lokaler redundanter Speicher (LRS) ist die Standardoption bei der Erstellung eines Speicherkontos.

Die folgende Tabelle bietet einen schnellen Überblick über die Unterschiede zwischen LRS, ZRS, GRS und RA-GRS. In den nachfolgenden Abschnitten dieses Artikels werden die einzelnen Replikationstypen ausführlicher behandelt.

| Replikationsstrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Daten werden in mehreren Datencentern repliziert. |Nein  |Ja |Ja |Ja |
| Daten können von einem sekundären Standort sowie vom primären Standort gelesen werden. |Nein  |Nein  |Nein  |Ja |
| Konzipiert, um ___ Dauerhaftigkeit von Objekten über ein bestimmtes Jahr zu bieten. |mindestens 99,999999999 % (11 mal die 9)|mindestens 99,9999999999 % (12 mal die 9)|mindestens 99,99999999999999 % (16 mal die 9)|mindestens 99,99999999999999 % (16 mal die 9)|

Informationen zu den Preisen für die verschiedenen Redundanzoptionen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

> [!NOTE]
> Storage Premium unterstützt nur lokal redundanten Speicher (Locally Redundant Storage, LRS). Informationen zu Storage Premium finden Sie unter [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Lokal redundanter Speicher
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Zonenredundanter Speicher
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="zrs-classic-accounts"></a>ZRS Classic-Konten

Die vorhandene ZRS-Funktion wird nun als „ZRS Classic“ bezeichnet. ZRS Classic-Konten sind nur für Blockblobs in allgemeinen V1-Speicherkonten verfügbar. 

ZRS Classic repliziert Daten asynchron zwischen Rechenzentren in einer Region oder in zwei Regionen. Ein Replikat ist ggf. wieder verfügbar, wenn von Microsoft das Failover in der sekundären Region initiiert wird. 

ZRS Classic-Konten können nicht in oder aus LRS, GRS oder RA-GRS konvertiert werden. ZRS Classic-Konten unterstützen zudem weder Metriken noch Protokollierungen.   

Sobald der ZRS in einer Region allgemein verfügbar ist, kann ein ZRS Classic-Konto nicht über das Portal in dieser Region erstellt werden. Dies ist nur durch andere Methoden möglich.  
In Zukunft wird ein automatisierter Migrationsprozess von ZRS Classic zum ZRS bereitgestellt.

Sie können manuell ZRS-Kontodaten zu oder von einem LRS-, ZRS Classic-, GRS- oder RAGRS-Konto migrieren. Derartige manuelle Migrationen können mit AzCopy, dem Azure Storage-Explorer, mit Azure PowerShell, der Azure CLI oder einer der Azure-Storage-Clientbibliotheken ausgeführt werden.

> [!NOTE]
> ZRS Classic-Konten werden am 31. März 2021 als veraltet gekennzeichnet, und für diese ist eine Migration erforderlich. Bevor die Komponenten als veraltet gekennzeichnet werden, sendet Microsoft weitere Informationen an ZRS Classic-Kunden.

Weitere Fragen werden im Abschnitt [Häufig gestellte Fragen](#frequently-asked-questions) behandelt. 

## <a name="geo-redundant-storage"></a>Georedundanter Speicher
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Georedundanter Speicher mit Lesezugriff
Georedundante Speicher mit Lesezugriff (RA-GRS) maximieren die Verfügbarkeit Ihres Speicherkontos. RA-GRS bieten neben der Georeplikation zwischen zwei Regionen schreibgeschützten Zugriff auf die Daten am sekundären Standort.

Wenn Sie den schreibgeschützten Zugriff auf Ihre Daten in der sekundären Region aktivieren, sind Ihre Daten zusätzlich zum primären Endpunkt für Ihr Speicherkonto auf einem sekundären Endpunkt verfügbar. Der sekundäre Endpunkt ähnelt dem primären Endpunkt, wobei das Suffix `–secondary` an den Kontonamen angefügt wird. Wenn Ihr primärer Endpunkt für den Blob-Dienst z. B. `myaccount.blob.core.windows.net` ist, dann ist Ihr sekundärer Endpunkt `myaccount-secondary.blob.core.windows.net`. Die Zugriffsschlüssel für das Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Bei der Nutzung von RA-GRS sollten Sie einige Aspekte berücksichtigen:

* Von Ihrer Anwendung muss verwaltet werden, mit welchem Endpunkt sie bei Verwendung von RA-GRS interagieren muss.
* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen Änderungen, die noch nicht in der sekundären Region repliziert wurden, möglicherweise verloren, wenn Daten beispielsweise bei einem regionalen Notfall nicht aus der primären Region wiederhergestellt werden können.
* Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](../storage-disaster-recovery-guidance.md).
* RA-GRS ist für hohe Verfügbarkeit ausgelegt. Einen Leitfaden zur Skalierbarkeit finden Sie in der [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Wie kann ich den Georeplikationstyp meines Speicherkontos ändern?

   Sie können den Georeplikationstyp Ihres Speicherkontos ändern, indem Sie das [Azure-Portal](https://portal.azure.com/) oder [Azure PowerShell](storage-powershell-guide-full.md) verwenden, oder Sie können eine der Azure Storage-Clientbibliotheken verwenden.

   > [!NOTE]
   > ZRS-Konten können nicht in LRS oder GRS konvertiert werden. Ebenso kann ein vorhandenes LRS- oder GRS-Konto nicht in ein ZRS-Konto konvertiert werden.
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2. Treten Ausfallzeiten bei der Änderung des Replikationstyps meines Speicherkontos auf?

   Nein. Bei einer Änderung des Replikationstyps Ihres Speicherkontos treten keine Ausfallzeiten auf.

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3. Fallen zusätzliche Kosten an, wenn ich den Replikationstyp meines Speicherkontos ändere?

   Ja. Wenn Sie für Ihr Speicherkonto eine Umstellung von LRS auf GRS (oder RA-GRS) durchführen, fallen Zusatzgebühren für den Datenverkehr in ausgehender Richtung an, der mit dem Kopieren von vorhandenen Daten vom primären Standort an den sekundären Standort verbunden ist. Nachdem die ersten Daten kopiert wurden, fallen keine zusätzlichen Ausgangsgebühren für die Georeplikation vom primären zum sekundären Standort an. Ausführliche Informationen zu Bandbreitengebühren finden Sie auf der [Seite mit Informationen zu Azure Storage-Preisen](https://azure.microsoft.com/pricing/details/storage/blobs/).

   Wenn Sie von GRS zu LRS wechseln, entstehen keine zusätzlichen Kosten, aber Ihre Daten werden am sekundären Standort gelöscht.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Welche Vorteile bietet RA-GRS?

   Georedundanter Speicher (GRS) ermöglicht die Replikation Ihrer Daten aus einer primären in einer sekundären Region, die Hunderte Kilometer weit von der primären Region entfernt ist. Bei GRS sind Ihre Daten auch bei einem regionalen Komplettausfall oder einem Notfall, nach dem die primäre Region nicht mehr wiederherstellbar ist, beständig gespeichert. RA-GRS unterstützt die Replikation von GRS und bietet zusätzlich die Möglichkeit, Daten am sekundären Standort zu lesen. Eine Empfehlung für den auf Hochverfügbarkeit ausgerichteten Entwurf finden Sie unter [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](../storage-designing-ha-apps-with-ragrs.md).

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Lässt sich herausfinden, wie lange es dauert, meine Daten aus der primären Region in der sekundären Region zu replizieren?

   Wenn Sie RA-GRS nutzen, können Sie für Ihr Speicherkonto den Zeitpunkt der letzten Synchronisierung überprüfen. Bei dem Zeitpunkt der letzten Synchronisierung handelt es sich um einen Datums-/Uhrzeitwert in GMT. Alle primären Schreibvorgänge vor dem Zeitpunkt der letzten Synchronisierung wurden erfolgreich an den sekundären Standort geschrieben. Dies bedeutet, dass sie am sekundären Standort zur Verfügung stehen. Primäre Schreibvorgänge, die nach dem Zeitpunkt der letzten Synchronisierung stattgefunden haben, stehen unter Umständen noch nicht zum Lesen zur Verfügung. Sie können diesen Wert mit dem [Azure-Portal](https://portal.azure.com/), mit [Azure PowerShell](storage-powershell-guide-full.md) oder mit einer der Azure Storage-Clientbibliotheken abfragen.

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6. Wie kann ich bei einem Ausfall in der primären Region zur sekundären Region wechseln?

   Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](../storage-disaster-recovery-guidance.md).

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Was ist die RPO und RTO in Bezug auf GRS?

   **Recovery Point Objective (RPO)**: Bei GRS und RA-GRS führt der Speicherdienst eine asynchrone Georeplikation der Daten vom primären zum sekundären Standort durch. Bei einem schwerwiegenden regionalen Notfall in der primären Region führt Microsoft ein Failover zur sekundären Region durch. Bei einem Failover können kürzlich vorgenommene Änderungen, die noch nicht georepliziert wurden, verloren gehen. Die Anzahl von Minuten des Zeitraums, für den ein potenzieller Datenverlust auftritt, wird als RPO bezeichnet und gibt den Zeitpunkt an, bis zu dem Daten wiederhergestellt werden können. Azure Storage weist normalerweise einen RPO-Wert von weniger als 15 Minuten auf, aber es gibt derzeit keine SLA zur Dauer der Georeplikation.

   **Recovery Time Objective (RTO)**: Die RTO ist eine Kennzahl dafür, wie lange es dauert, bis das Failover durchgeführt wurde und das Speicherkonto wieder online ist. Die Zeit für die Durchführung des Failovers umfasst folgende Aktionen:

   * Feststellung seitens Microsoft, ob die Daten am primären Speicherort wiederhergestellt werden können oder ob ein Failover erforderlich ist
   * Durchführung eines Failovers des Speicherkontos, indem die primären DNS-Einträge so geändert werden, dass sie auf den sekundären Standort verweisen

   Microsoft nimmt seine Verantwortung bezüglich der Speicherung Ihrer Daten sehr ernst. Falls eine Möglichkeit zur Wiederherstellung der Daten in der primären Region besteht, verschiebt Microsoft das Failover und konzentriert sich zunächst auf die Wiederherstellung Ihrer Daten. In zukünftigen Versionen des Diensts wird die Auslösung eines Failovers auf Kontoebene ermöglicht, sodass Sie die RTO selbst steuern können.

#### <a name="8-what-azure-storage-objects-does-zrs-support"></a>8. Welche Azure Storage-Objekte unterstützt der ZRS? 
Blockblobs, Seitenblobs (ausgenommen Blobs zur Sicherung von VM-Datenträgern), Tabellen, Dateien und Warteschlangen. 

#### <a name="9-does-zrs-also-include-geo-replication"></a>9. Unterstützt der ZRS auch Georeplikationen? 
Der ZRS unterstützt derzeit keine Georeplikation. Wenn Sie für Ihr Szenario eine regionsübergreifende Replikation zur Notfallwiederherstellung durchführen müssen, verwenden Sie stattdessen ein GRS- oder RA-GRS-Speicherkonto.   

#### <a name="10-what-happens-when-one-or-more-zrs-zones-go-down"></a>10. Was geschieht, wenn eine oder mehrere ZRS-Zonen ausfallen? 
Wenn die erste Zone ausfällt, schreibt der ZRS weiterhin Replikate von Ihren Daten zwischen den beiden verbleibenden Zonen in der Region. Wenn eine zweite Zone ausfällt, sind der Lese- und Schreibzugriff erst wieder verfügbar, wenn mindestens zwei Zonen verfügbar sind. 

## <a name="next-steps"></a>Nächste Schritte
* [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS-Speicher](../storage-designing-ha-apps-with-ragrs.md)
* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informationen zu Azure-Speicherkonten](../storage-create-storage-account.md)
* [Skalierbarkeits- und Leistungsziele für Azure-Speicher](storage-scalability-targets.md)
* [Microsoft Azure Storage Redundancy Options and Read Access Geo Redundant Storage (in englischer Sprache) ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (SOSP-Dokument – Azure Storage: ein hochverfügbarer Cloudspeicherdienst mit starker Konsistenz)](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

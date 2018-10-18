---
title: Erstellen von hochverfügbaren Azure Storage-Anwendungen für zonenredundante Speicher (ZRS) | Microsoft-Dokumentation
description: Zonenredundante Speicher (ZRS) bieten eine einfache Möglichkeit zum Erstellen von hochverfügbaren Anwendungen. ZRS schützen vor Hardwareausfällen im Rechenzentrum und vor regionalen Notfällen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 0a4ebefd89414fa62ca69f2f9cc726f471694bee
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733615"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Supportabdeckung und regionale Verfügbarkeit
ZRS unterstützt derzeit Standardkonten vom Typ „Allgemein v2“. Weitere Informationen zu Speicherkontotypen finden Sie unter [Azure-Speicherkonten – Übersicht](storage-account-overview.md).

ZRS ist für Blockblobs, Seitenblobs ohne Datenträger, Dateien, Tabellen und Warteschlangen verfügbar

ZRS ist in den folgenden Regionen verfügbar:

- USA, Osten
- USA (Ost 2)
- USA, Westen 2
- USA, Mitte
- Nordeuropa
- Europa, Westen
- Frankreich, Mitte
- Asien, Südosten

Microsoft arbeitet daran, ZRS in weiteren Azure-Regionen zu aktivieren. Sehen Sie regelmäßig auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/) nach, um Informationen zu neuen Regionen zu erhalten.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>Was geschieht, wenn eine Zone nicht mehr verfügbar ist?
Ihre Daten bleiben erhalten, wenn eine Zone ausfällt. Microsoft empfiehlt, die Vorgehensweisen für vorübergehende Fehler einzuhalten. Dazu gehört z.B. die Implementierung von Wiederholungsrichtlinien mit exponentiellem Backoff. Wenn eine Zone nicht verfügbar ist, führt Azure Netzwerkupdates durch, z.B. durch die Festlegung neuer DNS-Ziele. Diese Updates können sich auf Ihre Anwendung auswirken, wenn Sie auf Ihre Daten zugreifen, bevor sie abgeschlossen wurden.

ZRS kann Ihre Daten nicht vor einem regionalen Notfall schützen, bei dem mehrere Zonen dauerhaft betroffen sind. ZRS bietet stattdessen Resilienz für Ihre Daten bei einem temporären Ausfall. Für den Schutz vor regionalen Ausfällen empfiehlt Microsoft die Verwendung von georedundantem Speicher (GRS). Weitere Informationen zu GRS finden Sie unter [Georedundanter Speicher (GRS): regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Wechseln zur ZRS-Replikation
Sie können derzeit das Azure-Portal oder die Storage-Ressourcenanbieter-API verwenden, um den Redundanztyp Ihres Kontos zu ändern, sofern Sie eine Migration zu oder von LRS, GRS und RA-GRS durchführen. Mit ZRS ist die Migration jedoch nicht so einfach, da die Daten physisch aus einem einzelnen Speicherstempel in mehrere Stempel innerhalb einer Region verschoben werden müssen. 

Ihnen stehen zwei primäre Optionen für die Migration zu oder von ZRS zur Verfügung. Sie können Daten manuell aus Ihrem vorhandenen Konto in ein neues ZRS-Konto kopieren oder verschieben. Sie können auch eine Livemigration anfordern. Microsoft empfiehlt dringend, Sie eine manuelle Migration auszuführen, da es keine Garantie gibt, wann eine Livemigration abgeschlossen sein wird. Eine manuelle Migrationsroute bietet mehr Flexibilität als eine Livemigration, und Sie haben volle Kontrolle über den zeitlichen Ablauf der Migration.

Für eine manuelle Migration steht Ihnen eine Vielzahl von Optionen zur Verfügung:
- Verwenden Sie vorhandene Tools wie AzCopy, das Storage SDK, Tools von zuverlässigen Drittanbietern usw.
- Wenn Sie mit Hadoop oder HDInsight vertraut sind, können Sie das Quell- und Zielkonto (ZRS) an Ihren Cluster anfügen und beispielsweise DistCp verwenden, um den Kopiervorgang der Daten hochgradig zu parallelisieren.
- Erstellen Sie eigene Tools, die das Storage SDK nutzen.

Wenn eine manuelle Migration allerdings zu Anwendungsausfallzeiten führt und Sie dies auf Ihrer Seite nicht auffangen können, bietet Microsoft als Option eine Livemigration an. Eine Livemigration ist eine direkte Migration, bei der Sie Ihr vorhandenes Speicherkonto weiterverwenden können, während die Daten zwischen Quell- und Zielspeicherstempel migriert werden. Während der Migration verfügen Sie über das gleiche Maß an Dauerhaftigkeit und Verfügbarkeit gemäß SLA, wie Sie es gewohnt sind.

Trotzdem gelten für Livemigrationen gewisse Einschränkungen. Diese werden unten aufgeführt.

- Auch wenn Microsoft auf Ihre Anforderung zur Livemigration sofort reagiert, gibt es keine Garantie, wann die Migration abgeschlossen sein wird. Wenn Sie Ihre Daten zu einem bestimmten Zeitpunkt im ZRS benötigen, sollten Sie eine manuelle Migration durchführen. Grundsätzlich gilt: Je mehr Daten Sie in Ihrem Konto haben, desto länger dauert die Migration dieser Daten. 
- Sie können eine Livemigration nur über ein Konto mit LRS- oder GRS-Replikation ausführen. Wenn Ihr Konto RA-GRS verwendet, müssen Sie zunächst zu einem dieser Replikationstypen migrieren, bevor Sie fortfahren können. Dieser Zwischenschritt stellt sicher, dass der sekundäre schreibgeschützte-Endpunkt, den RA-GRS bereitstellt, vor der Migration entfernt wird.
- Ihr Konto muss Daten enthalten.
- Es werden nur Migrationen innerhalb einer Region unterstützt. Wenn Sie Ihre Daten in ein ZRS-Konto zu einer anderen Region als das Quellkonto migrieren möchten, müssen Sie eine manuelle Migration ausführen.
- Es werden nur Storage Standard-Konten unterstützt. Sie können nicht von einem Storage Premium-Konto migrieren.

Anforderungen zur Livemigration erfolgen über das Azure-Support-Portal. Sie wählen über das Portal das Speicherkonto aus, das Sie in ZRS konvertieren möchten.
1. Klicken Sie auf **Neue Supportanfrage**.
2. Überprüfen Sie die Grundlagen. Klicken Sie auf **Weiter**. 
3. Gehen Sie im Abschnitt **Problem** folgendermaßen vor: 
    - Lassen Sie den Schweregrad unverändert.
    - Problemtyp = **Datenmigration**
    - Kategorie = **Migrieren zu ZRS innerhalb einer Region**
    - Titel = **ZRS-Kontomigration** (oder eine Beschreibung)
    - Details = Ich möchte in der Region ______ von [LRS, GRS] zu ZRS migrieren. 
4. Klicken Sie auf **Weiter**.
5. Überprüfen Sie, ob die Kontaktinformationen auf dem Blatt „Kontaktinformationen“ korrekt sind.
6. Klicken Sie auf **Submit**.

Ein Supportmitarbeiter wird mit Ihnen in Kontakt treten. Diese Person bietet Ihnen ggf. erforderliche Unterstützung. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS Classic: eine ältere Option für die Redundanz von Blockblobs
> [!NOTE]
> Microsoft wird ZRS Classic-Konten am 31. März 2021 als veraltet markieren und migrieren. Bevor die Komponenten als veraltet gekennzeichnet werden, erhalten ZRS Classic-Kunden weitere Informationen. 
>
> Sobald der ZRS in einer Region [allgemein verfügbar](#support-coverage-and-regional-availability) wird, können Kunden ein ZRS Classic-Konto nicht mehr über das Portal in dieser Region erstellen. Die Verwendung von Microsoft PowerShell und der Azure CLI zum Erstellen von ZRS Classic-Konten wird unterstützt, bis ZRS Classic als veraltet gekennzeichnet wird.

ZRS Classic repliziert Daten asynchron zwischen Rechenzentren in einer oder zwei Regionen. Ein Replikat ist ggf. erst wieder verfügbar, wenn von Microsoft das Failover in der sekundären Region initiiert wird. Ein ZRS Classic-Konto kann nicht in bzw. aus LRS oder GRS konvertiert werden und verfügt nicht über Metriken oder Protokollierungsfunktionen.

ZRS Classic ist nur für **Blockblobs** in Speicherkonten vom Typ „Allgemein v1 (GPv1)“ verfügbar. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](storage-account-overview.md).

ZRS Classic-Konten können nicht in oder aus LRS, GRS oder RA-GRS konvertiert werden. ZRS Classic-Konten unterstützen zudem weder Metriken noch Protokollierungen.

Um manuell ZRS-Kontodaten zu oder von einem LRS-, ZRS Classic-, GRS- oder RA-GRS-Konto zu migrieren, verwenden Sie AzCopy, Azure Storage-Explorer, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle. Sie können auch Ihre eigene Migrationslösung mit einer der Azure Storage-Clientbibliotheken erstellen.

## <a name="see-also"></a>Weitere Informationen
- [Azure Storage-Replikation](storage-redundancy.md)
- [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](storage-redundancy-lrs.md)
- [Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md)
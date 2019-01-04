---
title: Speicherebenen „Premium“, „Heiß“, „Kalt“ und „Archiv“ für Blobs – Azure Storage
description: Speicherebenen „Premium“, „Heiß“, „Kalt“ und „Archiv“ für Azure-Speicherkonten.
services: storage
author: kuhussai
ms.service: storage
ms.topic: article
ms.date: 10/18/2018
ms.author: kuhussai
ms.component: blobs
ms.openlocfilehash: e12e29a5a627110ce845cd44be6dd97b717f9b26
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014496"
---
# <a name="azure-blob-storage-premium-preview-hot-cool-and-archive-storage-tiers"></a>Azure Blob Storage: Speicherebenen „Premium“ (Vorschauversion), „Heiß“, „Kalt“ und „Archiv“

## <a name="overview"></a>Übersicht

Azure Storage bietet unterschiedliche Speicherebenen, die Ihnen das Speichern von Blob-Objektdaten auf die kostengünstigste Art ermöglichen. Die verfügbaren Ebenen umfassen:

- **Storage Premium (Preview)** bietet Hochleistungshardware für Daten, auf die häufig zugegriffen wird.
 
- Die **Speicherebene „Heiß“ (Hot)** ist für die Speicherung von Daten optimiert, auf die häufig zugegriffen wird. 

- Die **Speicherebene „Kalt“ (Cool)** ist für die Speicherung von Daten optimiert, auf die selten zugegriffen wird und die mindestens 30 Tage lang gespeichert werden.
 
- Die **Speicherebene „Archiv“** ist für die Speicherung von Daten optimiert, auf die äußerst selten zugegriffen wird und die mindestens 180 Tage lang mit flexiblen Latenzanforderungen (in der Größenordnung von Stunden) gespeichert werden.

Die folgenden Überlegungen gehen mit den unterschiedlichen Speicherebenen einher:

- Die Archivspeicherebene ist nur auf Blobebene und nicht auf der Speicherkontoebene verfügbar.
 
- Bei den Daten der kalten Speicherebene können geringfügige Abstriche bei der Verfügbarkeit gemacht werden, aber Dauerhaftigkeit, Zugriffszeit und Durchsatz müssen sich auf einem ähnlich hohen Niveau wie bei Daten der heißen Ebene befinden. Daher kann bei Daten der kalten Ebene eine Kombination aus einer Vereinbarung zum Servicelevel mit etwas niedrigerer Verfügbarkeit und höheren Zugriffskosten im Vergleich zu Daten der heißen Ebene in Kauf genommen werden, um im Gegenzug die Speicherkosten zu verringern.

- Der Archivspeicher ist offline und ermöglicht die niedrigsten Speicherkosten, ist gleichzeitig aber mit den höchsten Zugriffskosten verbunden.
 
- Nur die heißen und kalten Speicherebenen können auf Kontoebene festgelegt werden. Derzeit kann die Archivebene nicht auf Kontoebene festgelegt werden.
 
- Heiße, kalte und Archivebene können auf Objektebene festgelegt werden.

Die Speicherung von Daten in der Cloud nimmt immer mehr zu. Um die Kosten für die zunehmenden Speicheranforderungen im Blick zu behalten, ist es hilfreich, die Daten anhand von Attributen wie der Zugriffshäufigkeit und geplanten Aufbewahrungsdauer zu organisieren. So können die Kosten optimiert werden. In der Cloud gespeicherte Daten können sich darin unterscheiden, wie sie über ihre Lebensdauer hinweg generiert, verarbeitet und genutzt werden. Auf einen Teil der Daten wird aktiv zugegriffen, und sie werden während ihrer Lebensdauer geändert. Auf andere Daten wird zu Beginn ihrer Lebensdauer häufig zugegriffen, und die Zugriffe fallen dann deutlich ab, je älter die Daten werden. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird.

Jedes dieser Datenzugriffsszenarien profitiert von einer differenzierten Speicherebene, die für ein bestimmtes Zugriffsmuster optimiert ist. Mit den heißen, kalten und Archivspeicherebenen bietet Azure Blob Storage unterschiedliche Speicherebenen mit separaten Preismodellen.

## <a name="storage-accounts-that-support-tiering"></a>Speicherkonten mit Tiering-Unterstützung

Sie können das Tiering für „Heiß“, „Kalt“ und „Archiv“ für Ihre Objektspeicherdaten nur in Blob Storage-Konten oder GPv2-Konten (universelle Konten der Version 2) durchführen. Für GPv1-Konten (General Purpose v1) wird das Tiering nicht unterstützt. Kunden können ihre vorhandenen GPv1- oder Blobspeicherkonten aber leicht in GPv2-Konten konvertieren, indem sie im Azure-Portal einen Schritt ausführen, für den nur ein Klick erforderlich ist. Für GPv2 gilt eine neue Preisstruktur für Blobs, Dateien und Warteschlangen, und außerdem besteht Zugriff auf viele andere neue Speicherfeatures. Zudem werden in Zukunft einige neue Features und Preisreduzierungen nur für GPv2-Konten angeboten. Aus diesem Grund ist es für Kunden ratsam, die Nutzung von GPv2-Konten zu evaluieren. Sie sollten die Konten aber erst verwenden, nachdem sie die Preise für alle Dienste überprüft haben, da einige Workloads unter GPv2 teurer als unter GPv1 sein können. Weitere Informationen finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).

Bei Blobspeicher- und GPv2-Konten wird das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene verfügbar gemacht. Hiermit können Sie die Standardspeicherebene für alle Blobs des Speicherkontos, für die auf Objektebene keine explizite Ebene festgelegt ist, als „Heiß“ oder „Kalt“ angeben. Für Objekte, für die die Ebene auf Objektebene festgelegt ist, gilt die Kontoebene nicht. Die Archivebene kann nur auf Objektebene angewendet werden. Sie können jederzeit zwischen diesen Speicherebenen wechseln.

## <a name="premium-access-tier"></a>Premium-Zugriffsebene

In der Preview ist eine Premium-Zugriffsebene verfügbar, die Daten, auf die häufig zugegriffen wird, über Hochleistungshardware verfügbar macht. Auf dieser Ebene gespeicherte Daten werden auf SSDs (Solid State Drives) gespeichert, die hinsichtlich niedriger Latenz und höherer Transaktionsraten im Vergleich zu herkömmlichen Festplatten optimiert sind. Die Premium-Zugriffsebene ist nur über den Blockblob-Speicherkontotyp verfügbar.

Diese Ebene eignet sich ideal für Workloads, die schnelle und konsistente Antwortzeiten benötigen. Daten, an denen Endbenutzer beteiligt sind, wie interaktive Videobearbeitung, statische Webinhalte, Onlinetransaktionen und ähnliches sind gute Kandidaten für die Premium-Zugriffsebene. Diese Ebene ist auf Workloads ausgelegt, bei denen viele kleine Transaktionen durchgeführt werden, z. B. die Erfassung von Telemetriedaten, die Nachrichtenübermittlung und die Datentransformation.

Um diese Ebene zu nutzen, stellen Sie ein neues Blockblob-Speicherkonto bereit, und beginnen mit der Erstellung von Containern und Blobs mithilfe der [REST-API des Blobdiensts](/rest/api/storageservices/blob-service-rest-api), von [AzCopy](/azure/storage/common/storage-use-azcopy) oder [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

Während der Preview-Phase gilt für die Premium-Zugriffsebene:

- Verfügbarkeit als lokal redundanter Speicher (LRS)
- Nur verfügbar in folgenden Regionen: „USA, Osten 2“, „USA, Mitte“ und „USA, Westen“
- Keine Unterstützung von automatischem Tiering und der Verwaltung des Datenlebenszyklus

Um zu erfahren, wie Sie sich für die Preview von Premium-Zugriffsebene registrieren, lesen Sie [Einführung in Azure Blob Storage Premium](https://aka.ms/premiumblob).

## <a name="hot-access-tier"></a>Zugriffsebene „Hot“

Für Speicher der heißen Ebene fallen höhere Speicherkosten als für Speicher der kalten und der Archivebene an, aber gleichzeitig auch die niedrigsten Zugriffskosten. Beispielszenarien für die Verwendung der heißen Speicherebene:

* Daten, die aktiv verwendet werden oder bei denen eine hohe Zugriffshäufigkeit (Lese- und Schreibvorgänge) zu erwarten ist
* Daten, die zur Verarbeitung und späteren Migration zur kalten Speicherebene bereitgestellt werden

## <a name="cool-access-tier"></a>Zugriffsebene „Cool“

Für die kalte Speicherebene fallen im Vergleich mit der heißen Speicherebene niedrigere Speicherkosten und höhere Zugriffskosten an. Diese Ebene ist für Daten bestimmt, die mindestens 30 Tage lang auf der kalten Ebene verbleiben. Beispielszenarien für die Verwendung der kalten Speicherebene:

* Kurzfristige Sicherung und Datasets für die Notfallwiederherstellung
* Ältere Medieninhalte, die nur noch selten angezeigt werden, dann aber umgehend verfügbar sein müssen
* Große Datasets, die kostengünstig gespeichert werden müssen, während weitere Daten für die zukünftige Verarbeitung gesammelt werden (*Beispiel:* langfristige Speicherung von wissenschaftlichen Daten oder Telemetrie-Rohdaten einer Produktionsstätte)

## <a name="archive-access-tier"></a>Zugriffsebene „Archiv“

Archivspeicher zeichnet sich im Vergleich zu den heißen und kalten Speicherebenen durch die niedrigsten Speicherkosten und höhere Datenabrufkosten aus. Diese Ebene ist für Daten bestimmt, die mehrere Stunden Abrufwartezeit tolerieren und mindestens 180 Tage lang auf der Archivebene verbleiben.

Während sich ein Blob im Archivspeicher befindet, ist es offline und kann nicht gelesen (mit Ausnahme der Metadaten, die online und verfügbar sind), kopiert, überschrieben oder geändert werden. Auch können Sie keine Momentaufnahmen von einem Blob im Archivspeicher erstellen. Sie können jedoch vorhandene Vorgänge verwenden, um das Blob zu löschen, aufzulisten, Eigenschaften/Metadaten des Blobs abzurufen oder die Ebene des Blobs zu ändern.

Beispielszenarien für die Verwendung der Archivspeicherebene:

* Langfristige Sicherung, sekundäre Sicherung und Archivierungsdatasets
* Originaldaten (Rohdaten), die auch nach der Umwandlung in ein endgültiges verwendbares Format erhalten bleiben müssen (*Beispiel:* Medien-Rohdateien nach der Transcodierung in andere Formate)
* Compliance- und Archivdaten, die über einen langen Zeitraum gespeichert werden müssen und selten verwendet werden (*Beispiele:* Überwachungsvideos, alte Röntgenbilder/MRT-Scans für Unternehmen aus dem Gesundheitswesen, Audioaufnahmen und Protokolle von Kundenanrufen bei Finanzdienstleistern)

### <a name="blob-rehydration"></a>Blobaktivierung
Wenn Sie Daten aus dem Archivspeicher lesen möchten, müssen Sie zuerst die Ebene des Blobs in „Heiß“ oder „Kalt“ ändern. Dieser Prozess wird als Aktivierung bezeichnet und kann bis zu 15 Stunden dauern. Es wird empfohlen, höhere Blobgrößen zu verwenden, um eine optimale Leistung zu erzielen. Die Dauer kann sich ggf. erhöhen, wenn mehrere kleine Blobs gleichzeitig aktiviert werden.

Während der Aktivierung können Sie anhand der Eigenschaft **Archive Status** (Archivstatus) des Blobs ermitteln, ob die Ebene geändert wurde. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Eigenschaft „Archive Status“ (Archivstatus) entfernt, und die Eigenschaft **Access Tier** (Zugriffsebene) des Blobs spiegelt die neue heiße oder kalte Ebene wider.  

## <a name="blob-level-tiering"></a>Blobebenentiering

Mit dem Blobebenentiering können Sie mithilfe eines einzelnen Vorgangs namens [Set Blob Tier](/rest/api/storageservices/set-blob-tier) (Blobebene festlegen) die Ebene Ihrer Daten auf der Objektebene ändern. So können Sie flexibel auf Nutzungsänderungen reagieren und problemlos zwischen den Blobzugriffsebenen „Heiß“, „Kalt“ und „Archiv“ wechseln, ohne Daten zwischen Konten zu verschieben. Alle Ebenenänderungen erfolgen sofort. Die Aktivierung eines Blobs aus dem Archiv kann jedoch mehrere Stunden dauern. 

Der Zeitpunkt der letzten Änderung der Blobebene wird über die Blobeigenschaft **Access Tier Change Time** (Änderungszeitpunkt der Zugriffsebene) verfügbar gemacht. Wenn sich ein Blob auf der Archivebene befindet, kann es nicht überschrieben werden. Daher ist das Hochladen desselben Blobs in diesem Szenario nicht zulässig. Sie können ein Blob auf der heißen und kalten Ebene überschreiben. In diesem Fall erbt das neue Blob die Ebene des Blobs, das überschrieben wurde.

Ein Konto kann Blobs aus allen drei Speicherebenen enthalten. Blobs, denen keine explizite Ebene zugewiesen ist, leiten die Ebene von der Zugriffsebeneneinstellung des Kontos ab. Wenn die Zugriffsebene vom Konto abgeleitet wird, sehen Sie, dass die Blobeigenschaft **Access Tier Inferred** (Abgeleitete Zugriffsebene) auf TRUE festgelegt ist und die Blobeigenschaft **Access Tier** (Zugriffsebene) des Blobs mit der Kontoebene übereinstimmt. Im Azure-Portal wird die „Access Tier Inferred“-Eigenschaft (Abgeleitete Zugriffsebene) mit der Blobzugriffsebene angezeigt (z.B. „Hot (inferred)“ oder „Cool (inferred)“).

> [!NOTE]
> Für die Archivspeicherebene und das Blobebenentiering werden nur Blockblobs unterstützt. Außerdem ist es nicht möglich, die Ebene eines Blockblobs zu ändern, das über Momentaufnahmen verfügt.

Auf der Premium-Zugriffsebene gespeicherte Daten können weder mithilfe von [Set Blob Tier](/rest/api/storageservices/set-blob-tier) noch unter Verwendung der Azure Blob Storage-Lebenszyklusverwaltung auf die Ebene „Heiß“, „Kalt“ oder „Archiv“ verschoben werden. Um Daten zu verschieben, müssen Sie Blobs synchron vom Premium-Zugriff auf die Ebene „Heiß“ kopieren und hierzu die [Put Block From URL-API](/rest/api/storageservices/put-block-from-url) oder eine AzCopy-Version, die diese API unterstützt, verwenden. Die *Put Block From URL*-API kopiert Daten synchron auf den Server, was heißt, dass der Aufruf erst abgeschlossen wird, nachdem alle Daten vom ursprünglichen Serverspeicherort an den Zielspeicherort verschoben wurden.

### <a name="blob-lifecycle-management"></a>Lebenszyklusverwaltung für Blobs
Die Blob Storage-Lebenszyklusverwaltung (Preview) bietet eine umfassende, regelbasierte Richtlinie, mit deren Hilfe Sie den Übergang Ihrer Daten in die optimale Zugriffsebene und den Ablauf der Daten am Ende ihres Lebenszyklus umsetzen können. Weitere Informationen finden Sie unter [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md).  

### <a name="blob-level-tiering-billing"></a>Abrechnung für das Blobebenentiering

Wenn ein Blob in eine „kältere“ Ebene verschoben wird (von „Heiß“ zu „Kalt“, von „Heiß“ zu „Archiv“ oder von „Kalt“ zu „Archiv“), wird der Vorgang als Schreibvorgang auf die Zielebene berechnet, und es gelten die Zielebenengebühren für Schreibvorgänge (pro 10.000) und das Schreiben von Daten (pro GB). Wenn ein Blob in eine „wärmere“ Ebene verschoben wird (von „Archiv“ zu „Kalt“, von „Archiv“ zu „Heiß“ oder von „Kalt“ zu „Heiß“), wird der Vorgang als Lesevorgang aus der Quellebene berechnet, und es gelten die Quellebenengebühren für Lesevorgänge (pro 10.000) und Datenabruf (pro GB).

Wenn Sie für die Kontoebene von „Heiß“ zu „Kalt“ wechseln, werden Ihnen Schreibvorgänge (pro 10.000) für alle Blobs ohne festgelegte Ebene nur für GPv2-Konten berechnet. Für diese Änderung bei Blobspeicherkonten fallen keine Gebühren an. Ihnen werden sowohl Gebühren für Lesevorgänge (pro 10.000) als auch für den Datenabruf (pro GB) berechnet, wenn Sie für Ihr Blob Storage- oder GPv2-Konto von „Kalt“ zu „Heiß“ wechseln. Für alle Blobs, die aus der Ebene „Kalt“ oder „Archiv“ verschoben werden, können auch Gebühren für das frühe Löschen anfallen.

### <a name="cool-and-archive-early-deletion"></a>Frühzeitiges Löschen für kalte Speicher und Archivspeicher

Zusätzlich zur Gebühr, die pro GB und pro Monat berechnet wird, gilt Folgendes: Alle Blobs, die in die kalte Ebene (nur GPv2-Konten) verschoben werden, unterliegen einem Zeitraum von 30 Tagen für das frühe Löschen für „Kalt“, und alle Blobs, die in die Archivebene verschoben werden, unterliegen einem Zeitraum von 180 Tagen für das frühe Löschen für „Archiv“. Diese Gebühr fällt anteilig an. Wenn ein Blob beispielsweise in das Archiv verschoben und dann nach 45 Tagen gelöscht oder in die heiße Ebene verschoben wird, wird Ihnen eine Gebühr für das frühe Löschen berechnet, die 135 Speichertagen (180 minus 45) dieses Blobs im Archiv entspricht.

## <a name="comparison-of-the-storage-tiers"></a>Vergleich der Speicherebenen

Die folgende Tabelle enthält eine Gegenüberstellung der Speicherebenen „Heiß“, „Kalt“ und „Archiv“.

| | **Speicherebene „Hot“** | **Speicherebene „Cool“** | **Speicherebene „Archiv“**
| ---- | ----- | ----- | ----- |
| **Verfügbarkeit** | 99,9 % | 99 % | N/V |
| **Verfügbarkeit** <br> **(RA-GRS-Lesevorgänge)**| 99,99 % | 99,9 % | N/V |
| **Nutzungsgebühren** | Höhere Speicherkosten, geringere Zugriffs- und Transaktionskosten | Geringere Speicherkosten, höhere Zugriffs- und Transaktionskosten | Niedrigste Speicherkosten, höchste Zugriffs- und Transaktionskosten |
| **Mindestobjektgröße** | N/V | N/V | N/V |
| **Mindestspeicherdauer** | N/V | 30 Tage (nur GPv2) | 180 Tage
| **Latenz** <br> **(Zeit bis zum ersten Byte)** | Millisekunden | Millisekunden | < 15 Stunden
| **Skalierbarkeits- und Leistungsziele** | Identisch mit allgemeinen Speicherkonten | Identisch mit allgemeinen Speicherkonten | Identisch mit allgemeinen Speicherkonten |

> [!NOTE]
> Blob-Speicherkonten unterstützen die gleichen Leistungs- und Skalierbarkeitsziele wie allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).

## <a name="quickstart-scenarios"></a>Schnellstartszenarien

In diesem Abschnitt werden unter Verwendung des Azure-Portals die folgenden Szenarien veranschaulicht:

* Ändern der Standard-Kontozugriffsebene für ein GPv2- oder Blobspeicherkonto
* Ändern der Ebene eines Blobs in einem GPv2- oder Blobspeicherkonto

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>Ändern der Standard-Kontozugriffsebene für ein GPv2- oder Blob Storage-Konto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Speicherkonto, wählen Sie „Alle Ressourcen“, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Konfiguration**, um die Kontokonfiguration anzuzeigen bzw. zu ändern.

4. Wählen Sie die passende Speicherebene für Ihre Anforderungen aus: Legen Sie die **Zugriffsebene** entweder auf **kalte Ebene** oder auf **heiße Ebene** fest.

5. Klicken Sie oben im Blatt auf „Speichern“.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>Ändern der Ebene eines Blobs in einem GPv2- oder Blobspeicherkonto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie zum Navigieren zu Ihrem Blob in Ihrem Speicherkonto die Option „Alle Ressourcen“, und wählen Sie Ihr Speicherkonto, Ihren Container und dann Ihr Blob aus.

3. Klicken Sie auf dem Blatt „Blob-Eigenschaften“ auf das Dropdownmenü **Zugriffsebene**, um die Speicherebene **Hot**, **Cool** oder **Archiv** auszuwählen.

5. Klicken Sie oben im Blatt auf „Speichern“.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Für alle Speicherkonten wird ein Blobspeicher-Preismodell verwendet, das auf der Ebene der einzelnen Blobs basiert. Beachten Sie dabei die folgenden Abrechnungsaspekte:

* **Speicherkosten:** Die Kosten für die Datenspeicherung hängen nicht nur von der gespeicherten Datenmenge ab, sondern auch von der Speicherebene. Je „cooler“ die Ebene, desto geringer die Kosten pro GB.
* **Kosten für den Datenzugriff:** Je „kälter“ die Ebene, desto höher die Gebühren für den Datenzugriff. Bei den Speicherebenen „Kalt“ und „Archiv“ fallen Zugriffsgebühren für Lesevorgänge pro Gigabyte an.
* **Transaktionskosten:** Für alle Ebenen fällt eine Gebühr pro Transaktion an, die sich erhöht, je „kälter“ die Ebene ist.
* **Datenübertragungskosten bei Georeplikation:** Diese Gebühr gilt nur für Konten mit konfigurierter Georeplikation (einschließlich GRS und RA-GRS). Die Datenübertragung für die Georeplikation wird pro Gigabyte abgerechnet.
* **Kosten für ausgehende Datenübertragungen:** Ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen werden) werden genau wie bei allgemeinen Speicherkonten nach Bandbreitennutzung pro Gigabyte abgerechnet.
* **Änderung der Speicherebene:** Bei einem Wechsel der Kontospeicherebene von „Kalt“ zu „Heiß“ fällt eine Gebühr an, die den Kosten entspricht, die durch das Lesen aller im Speicherkonto vorhandenen Daten entstehen. Beim Ändern der Kontospeicherebene von „Heiß“ in „Kalt“ fällt aber eine Gebühr an, die dem Schreiben aller Daten auf die Ebene „Kalt“ entspricht (nur GPv2-Konten).

> [!NOTE]
> Weitere Informationen zu den Preisen für Blob-Speicherkonten finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Weitere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie auf der Seite [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="faq"></a>Häufig gestellte Fragen

**Sollte ich Blobspeicher- oder GPv2-Konten verwenden, wenn ich für meine Daten das Tiering durchführen möchte?**

Wir empfehlen Ihnen, für das Tiering anstelle von Blobspeicherkonten GPv2-Konten zu nutzen. Für GPv2 werden alle Features von Blobspeicherkonten sowie noch viele weitere Features unterstützt. Die Preise für Blobspeicher und GPv2 sind nahezu identisch, aber einige neue Features und Preisreduzierungen gelten nur für GPv2-Konten. Für GPv1-Konten wird das Tiering nicht unterstützt.

Die Preisstruktur von GPv1 und GPv2 unterscheidet sich, und Kunden sollten dies jeweils sorgfältig evaluieren, bevor sie sich für die Nutzung von GPv2-Konten entscheiden. Sie können ein vorhandenes Blobspeicher- oder GPv1-Konto leicht in ein GPv2-Konto konvertieren, indem Sie im Azure-Portal einen Schritt ausführen, für den nur ein Klick erforderlich ist. Weitere Informationen finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).

**Kann ich Objekte unter demselben Konto auf allen drei Speicherebenen („Heiß“, „Kalt“ und „Archiv“) speichern?**

Ja. Das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene ist die Standardebene, die für alle Objekte des Kontos gilt, für die keine explizite Ebene festgelegt ist. Mit dem Blobebenentiering können Sie die Zugriffsebene auf dieser Objektebene unabhängig davon festlegen, welche Zugriffsebene für das Konto eingestellt ist. In einem Konto können Blobs in allen drei Speicherebenen („Heiß“, „Kalt“ oder „Archiv“) vorhanden sein.

**Kann ich die Standardspeicherebene meines Blob Storage- oder GPv2-Kontos ändern?**

Ja. Sie können die Standardspeicherebene ändern, indem Sie für das Speicherkonto das Attribut **Access tier** (Zugriffsebene) festlegen. Die Änderung der Speicherebene gilt für alle gespeicherten Objekte des Kontos, für die keine explizite Ebene festgelegt ist. Wenn Sie für die Speicherebene von „Heiß“ zu „Kalt“ wechseln, fallen Gebühren für Schreibvorgänge (pro 10.000) für alle Blobs ohne festgelegte Ebene nur für GPv2-Konten an. Wenn Sie von „Kalt“ zu „Heiß“ wechseln, fallen Gebühren für Lesevorgänge (pro 10.000) und den Datenabruf (pro GB) für alle Blobs in Blobspeicher- und GPv2-Konten an.

**Kann ich meine Standard-Kontozugriffsebene auf „Archiv“ festlegen?**

 Nein. Nur die Speicherebenen „Heiß“ und „Kalt“ können als Standard-Kontozugriffsebene festgelegt werden. „Archiv“ kann nur auf Objektebene festgelegt werden.

**In welchen Regionen sind die Speicherebenen „Heiß“, „Kalt“ und „Archiv“ verfügbar?**

Die Speicherebenen „Heißt“ und „Kalt“ sind zusammen mit dem Blobebenentiering in allen Regionen verfügbar. Die Speicherebene „Archiv“ ist anfänglich nur in ausgewählten Regionen verfügbar. Eine vollständige Liste finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

**Verhalten sich die Blobs der Speicherebene „Kalt“ anders als die Blobs der Speicherebene „Heiß“?**

Blobs der Speicherebene „Heiß“ weisen die gleiche Latenz wie Blobs in GPv1-, GPv2- und Blobspeicherkonten auf. Blobs der Speicherebene „Kalt“ verfügen über eine ähnliche Latenz (in Millisekunden) wie Blobs in GPv1-, GPv2- und Blobspeicherkonten. In GPv1-, GPv2- und Blobspeicherkonten gilt für Blobs auf der Speicherebene „Archiv“ eine Latenzzeit von mehreren Stunden.

Für Blobs der Speicherebene „Kalt“ gilt in Bezug auf die Verfügbarkeit ein etwas geringerer Servicelevel (SLA) als für Blobs, die auf der Speicherebene „Heiß“ gespeichert sind. Weitere Informationen finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**Sind die Vorgänge für die Ebenen „Heiß“, „Kalt“ und „Archiv“ identisch?**

Ja. Alle Vorgänge zwischen „Heiß“ und „Kalt“ sind zu 100 % konsistent. Alle gültigen Archivvorgänge, z.B. Löschen, Auflisten, Abrufen von Blobeigenschaften/Metadaten und Festlegen von Blobebenen, sind zu 100 % mit „Heiß“ und „Kalt“ konsistent. Ein Blob kann nicht gelesen oder geändert werden, während es sich auf der Ebene „Archiv“ befindet.

**Wie kann ich beim Aktivieren eines Blobs von der Ebene „Archiv“ auf die Ebene „Heiß“ oder „Kalt“ erkennen, wann die Aktivierung abgeschlossen ist?**

Während der Aktivierung können Sie den Vorgang „Get Blob Properties“ verwenden, um das Attribut **Archive Status** (Archivstatus) abzufragen und so zu ermitteln, wann die Ebenenänderung abgeschlossen ist. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Eigenschaft „Archive Status“ (Archivstatus) entfernt, und die Eigenschaft **Access Tier** (Zugriffsebene) des Blobs spiegelt die neue heiße oder kalte Ebene wider.  

**Ab wann werden mir nach dem Festlegen der Ebene eines Blobs die entsprechenden Gebühren berechnet?**

Die Berechnung für ein Blob basiert immer auf der Ebene, die in der Eigenschaft **Access Tier** (Zugriffsebene) des Blobs angegeben ist. Wenn Sie eine neue Ebene für ein Blob festlegen, übernimmt die Eigenschaft **Access Tier** (Zugriffsebene) sofort die neue Ebene für alle Übergänge. Die Aktivierung eines Blobs von der Archivebene in eine heiße oder kalte Ebene kann jedoch mehrere Stunden dauern. In diesem Fall werden Ihnen die Kosten für „Archiv“ berechnet, bis die Aktivierung abgeschlossen ist, wonach dann die neue Ebene in der Eigenschaft **Access Tier** (Zugriffsebene) angezeigt wird. Ab diesem Zeitpunkt erfolgt die Abrechnung für dieses Blob mit der Rate für die Ebene „Heiß“ oder „Kalt“.

**Wie kann ich ermitteln, ob eine Gebühr für das frühe Löschen anfällt, wenn ich ein Blob lösche oder aus der Ebene „Kalt“ oder „Archiv“ verschiebe?**

Für alle Blobs, die vor Ablauf von 30 bzw. 180 Tagen gelöscht oder aus der Ebene „Kalt“ (nur GPv2-Konten) oder „Archiv“ verschoben werden, fällt eine anteilige Gebühr für frühes Löschen an. Sie können ermitteln, wie lange sich ein Blob auf der Ebene „Kalt“ oder „Archiv“ befunden hat, indem Sie in der Blobeigenschaft **Access Tier Change Time** (Änderungszeitpunkt der Zugriffsebene) den Zeitstempel der letzten Ebenenänderung überprüfen. Weitere Informationen finden Sie im Abschnitt [Frühzeitiges Löschen für kalte Speicher und Archivspeicher](#cool-and-archive-early-deletion).

**Welche Azure-Tools und SDKs unterstützen das Blobebenentiering und die Speicherebene „Archiv“?**

Für das Azure-Portal, PowerShell und CLI-Tools sowie .NET-, Java-, Python- und Node.js-Clientbibliotheken werden das Blobebenentiering und die Speicherebene „Archiv“ unterstützt.  

**Wie viele Daten kann ich auf den Ebenen „Heiß“, „Kalt“ und „Archiv“ speichern?**

Die Datenspeicherung wird zusammen mit anderen Beschränkungen auf Kontoebene und nicht einzeln pro Speicherebene festgelegt. Daher können Sie wählen, ob Sie Ihr gesamtes Kontingent auf einer Ebene nutzen oder auf alle drei Ebenen verteilen möchten. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungszielen für Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Evaluieren von „Heiß“, „Kalt“ und „Archiv“ in GPv2- und Blobspeicherkonten

[Prüfen der Verfügbarkeit von „Heiß“, „Kalt“ und „Archiv“ nach Region](https://azure.microsoft.com/regions/#services)

[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)

[Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](../common/storage-enable-and-view-metrics.md)

[Prüfen der Preise für „Heiß“, „Kalt“ und „Archiv“ in Blobspeicher- und GPv2-Konten nach Region](https://azure.microsoft.com/pricing/details/storage/)

[Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

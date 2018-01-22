---
title: "Azure-Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“ für Blobs | Microsoft-Dokumentation"
description: "Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“ für Azure-Speicherkonten."
services: storage
documentationcenter: 
author: kuhussai
manager: jwillis
editor: 
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: kuhussai
ms.openlocfilehash: be84f68a044a73673e991f04c7fe36a7787b9c3c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="azure-blob-storage-hot-cool-and-archive-storage-tiers"></a>Azure Blob Storage: Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“

## <a name="overview"></a>Übersicht

Azure-Speicher bietet drei Speicherebenen für Blobobjektspeicher, damit Sie Ihre Daten je nach Nutzung möglichst kostengünstig speichern können. Die **Speicherebene „Heiß“ (Hot)** von Azure ist für die Speicherung von Daten optimiert, auf die häufig zugegriffen wird. Die **Speicherebene „Kalt“ (Cool)** von Azure ist für die Speicherung von Daten optimiert, auf die selten zugegriffen wird und die mindestens 30 Tage lang gespeichert werden. Die **Speicherebene „Archiv“** von Azure ist für die Speicherung von Daten optimiert, auf die äußerst selten zugegriffen wird und die mindestens 180 Tage lang mit flexiblen Latenzanforderungen (in der Größenordnung von Stunden) gespeichert werden. Die Archivspeicherebene ist nur auf Blobebene und nicht auf der Speicherkontoebene verfügbar. Bei den Daten der Speicherebene „Cool“ können geringfügige Abstriche bei der Verfügbarkeit gemacht werden, aber Haltbarkeit, Zugriffszeit und Durchsatz müssen sich auf einem ähnlich hohen Niveau wie bei häufig genutzten Daten befinden. Daher kann bei selten genutzten Daten eine Kombination aus einer Vereinbarung zum Servicelevel mit etwas niedrigerer Verfügbarkeit und höheren Zugriffskosten im Vergleich zu häufig genutzten Daten in Kauf genommen werden, um im Gegenzug die Speicherkosten zu verringern. Der Archivspeicher ist offline und ermöglicht die niedrigsten Speicherkosten, ist gleichzeitig aber mit den höchsten Zugriffskosten verbunden.

Die Speicherung von Daten in der Cloud nimmt immer mehr zu. Um die Kosten für die zunehmenden Speicheranforderungen im Blick zu behalten, ist es hilfreich, die Daten anhand von Attributen wie der Zugriffshäufigkeit und geplanten Aufbewahrungsdauer zu organisieren. So können die Kosten optimiert werden. In der Cloud gespeicherte Daten können sich darin unterscheiden, wie sie über ihre Lebensdauer hinweg generiert, verarbeitet und genutzt werden. Auf einen Teil der Daten wird aktiv zugegriffen, und sie werden während ihrer Lebensdauer geändert. Auf andere Daten wird zu Beginn ihrer Lebensdauer häufig zugegriffen, und die Zugriffe fallen dann deutlich ab, je älter die Daten werden. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird.

Jedes dieser Datenzugriffsszenarien profitiert von einer differenzierten Speicherebene, die für ein bestimmtes Zugriffsmuster optimiert ist. Mit den Speicherebenen „Hot“, „Cool“ und „Archiv“ bietet Azure Blob Storage unterschiedliche Speicherebenen mit separaten Preismodellen.

## <a name="storage-accounts-that-support-tiering"></a>Speicherkonten mit Tiering-Unterstützung

Sie können das Tiering für „Hot“, „Cool“ und „Archiv“ für Ihre Objektspeicherdaten nur in Blob Storage-Konten oder GPv2-Konten (General Purpose v2) durchführen. Für GPv1-Konten (General Purpose v1) wird das Tiering nicht unterstützt. Kunden können ihre vorhandenen GPv1- oder Blob Storage-Konten aber leicht in GPv2-Konten konvertieren, indem sie im Azure-Portal einen Schritt ausführen, für den nur ein Klick erforderlich ist. Für GPv2 gilt eine neue Preisstruktur für Blobs, Dateien und Warteschlangen, und außerdem besteht Zugriff auf viele andere neue Speicherfeatures. Zudem werden in Zukunft einige neue Features und Preisreduzierungen nur für GPv2-Konten angeboten. Aus diesem Grund ist es für Kunden ratsam, die Nutzung von GPv2-Konten zu evaluieren. Sie sollten die Konten aber erst verwenden, nachdem sie die Preise für alle Dienste überprüft haben, da einige Workloads unter GPv2 teurer als unter GPv1 sein können. Weitere Informationen finden Sie unter [Optionen für Azure Storage-Konten](../common/storage-account-options.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Bei Blob Storage- und GPv2-Konten wird das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene verfügbar gemacht. Hiermit können Sie die Standardspeicherebene für alle Blobs des Speicherkontos, für die auf Objektebene keine Ebene festgelegt ist, als „Hot“ oder „Cool“ angeben. Für Objekte, für die die Ebene auf Objektebene festgelegt ist, gilt die Kontoebene nicht. Die Archivebene kann nur auf Objektebene angewendet werden. Sie können jederzeit zwischen diesen Speicherebenen wechseln.

## <a name="hot-access-tier"></a>Zugriffsebene „Hot“

Für Speicher vom Typ „Hot“ fallen höhere Speicherkosten als für Speicher vom Typ „Cool“ und „Archiv“ an, aber gleichzeitig auch die niedrigsten Zugriffskosten. Beispielszenarien für die Verwendung der Speicherebene „Hot“:

* Daten, die aktiv verwendet werden oder bei denen eine hohe Zugriffshäufigkeit (Lese- und Schreibvorgänge) zu erwarten ist
* Daten, die zur Verarbeitung und späteren Migration zur Speicherebene „Cool“ bereitgestellt werden

## <a name="cool-access-tier"></a>Zugriffsebene „Cool“

Für die Speicherebene „Cool“ fallen im Vergleich mit der Speicherebene „Hot“ niedrigere Speicherkosten und höhere Zugriffskosten an. Diese Ebene ist für Daten bestimmt, die mindestens 30 Tage lang auf der Ebene „Cool“ verbleiben. Beispielszenarien für die Verwendung der Speicherebene „Cool“:

* Kurzfristige Sicherung und Datasets für die Notfallwiederherstellung
* Ältere Medieninhalte, die nur noch selten angezeigt werden, dann aber umgehend verfügbar sein müssen
* Große Datasets, die kostengünstig gespeichert werden müssen, während weitere Daten für die zukünftige Verarbeitung gesammelt werden (*Beispiel:* langfristige Speicherung von wissenschaftlichen Daten oder Telemetrie-Rohdaten einer Produktionsstätte)

## <a name="archive-access-tier"></a>Zugriffsebene „Archiv“

Archivspeicher zeichnet sich im Vergleich zu den Speicherebenen „Hot“ und „Cool“ durch die niedrigsten Speicherkosten und höhere Datenabrufkosten aus. Diese Ebene ist für Daten bestimmt, die mehrere Stunden Abrufwartezeit tolerieren und mindestens 180 Tage lang auf der Archivebene verbleiben.

Während sich ein Blob im Archivspeicher befindet, ist er offline und kann nicht gelesen (mit Ausnahme der Metadaten, die online und verfügbar sind), kopiert, überschrieben oder geändert werden. Auch können Sie keine Momentaufnahmen von einem Blob im Archivspeicher erstellen. Sie können jedoch vorhandene Vorgänge verwenden, um das Blob zu löschen, aufzulisten, Eigenschaften/Metadaten des Blobs abzurufen oder die Ebene des Blobs zu ändern.

Beispielszenarien für die Verwendung der Speicherebene „Archiv“:

* Langfristige Sicherung, Archivierung und Datasets für die Notfallwiederherstellung
* Originaldaten (Rohdaten), die auch nach der Umwandlung in ein endgültiges verwendbares Format erhalten bleiben müssen (*Beispiel:* Medien-Rohdateien nach der Transcodierung in andere Formate)
* Compliance- und Archivdaten, die über einen langen Zeitraum gespeichert werden müssen und selten verwendet werden (*Beispiele:* Überwachungsvideos, alte Röntgenbilder/MRT-Scans für Unternehmen aus dem Gesundheitswesen, Audioaufnahmen und Protokolle von Kundenanrufen bei Finanzdienstleistern)

### <a name="blob-rehydration"></a>Blobaktivierung
Wenn Sie Daten aus dem Archivspeicher lesen möchten, müssen Sie zuerst die Ebene des Blobs in „Hot“ oder „Cool“ ändern. Dieser Prozess wird als Aktivierung bezeichnet und kann bis zu 15 Stunden dauern. Es wird dringend empfohlen, höhere Blobgrößen zu verwenden, um eine optimale Leistung zu erzielen. Die Dauer kann sich ggf. erhöhen, wenn mehrere kleine Blobs gleichzeitig aktiviert werden.

Während der Aktivierung können Sie anhand der Eigenschaft **Archive Status** (Archivstatus) des Blobs ermitteln, ob die Ebene geändert wurde. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Eigenschaft „Archive Status“ (Archivstatus) entfernt, und die Eigenschaft **Access Tier** (Zugriffsebene) des Blobs spiegelt die neue Ebene vom Typ „Hot“ oder „Cool“ wider.  

## <a name="blob-level-tiering"></a>Blobebenentiering

Mit dem Blobebenentiering können Sie mithilfe eines einzelnen Vorgangs namens [Set Blob Tier](/rest/api/storageservices/set-blob-tier) (Blobebene festlegen) die Ebene Ihrer Daten auf der Objektebene ändern. So können Sie flexibel auf Nutzungsänderungen reagieren und problemlos zwischen den Blobzugriffsebenen „Hot“, „Cool“ und „Archiv“ wechseln, ohne Daten zwischen Konten zu verschieben. Alle Ebenenänderungen werden umgehend umgesetzt (mit Ausnahme der Aktivierung eines Blobs aus dem Archiv), und dieser Vorgang kann mehrere Stunden dauern. Der Zeitpunkt der letzten Änderung der Blobebene wird über die Blobeigenschaft **Access Tier Change Time** (Änderungszeitpunkt der Zugriffsebene) verfügbar gemacht. Wenn sich ein Blob auf der Archivebene befindet, darf er nicht überschrieben werden. Daher ist das Hochladen desselben Blobs in diesem Szenario nicht zulässig. Sie können ein Blob in „Hot“ und „Cool“ überschreiben. In diesem Fall erbt das neue Blob die Ebene des alten Blobs, das überschrieben wurde.

Ein Konto kann Blobs aus allen drei Speicherebenen enthalten. Blobs, denen keine explizite Ebene zugewiesen ist, leiten die Ebene von der Zugriffsebeneneinstellung des Kontos ab. Wenn die Zugriffsebene vom Konto abgeleitet wird, sehen Sie, dass die Blobeigenschaft **Access Tier Inferred** (Abgeleitete Zugriffsebene) auf „true“ festgelegt ist und die Blobeigenschaft **Access Tier** (Zugriffsebene) des Blobs mit der Kontoebene übereinstimmt. Im Azure-Portal wird die „Access Tier Inferred“-Eigenschaft (Abgeleitete Zugriffsebene) mit der Blobzugriffsebene angezeigt (z.B. „Hot (inferred)“ oder „Cool (inferred)“).

> [!NOTE]
> Für die Archivspeicherebene und das Blobebenentiering werden nur Blockblobs unterstützt. Außerdem ist es nicht möglich, die Ebene eines Blockblobs zu ändern, das über Momentaufnahmen verfügt.

### <a name="blob-level-tiering-billing"></a>Abrechnung für das Blobebenentiering

Wenn ein Blob in „Cool“-Richtung verschoben wird (Hot->Cool, Hot->Archiv oder Cool->Archiv), wird der Vorgang als Schreibvorgang auf die Zielebene berechnet, und es gelten die Gebühren der Zielebene für Schreibvorgänge (pro 10.000) und das Schreiben von Daten (pro GB). Wenn ein Blob in „Hot“-Richtung verschoben wird (Archiv->Cool, Archiv->Hot oder Cool->Hot), wird der Vorgang als Lesevorgang aus der Quellebene berechnet, und es gelten die Gebühren der Quellebene für Lesevorgänge (pro 10.000) und den Datenabruf (pro GB).

Wenn Sie für die Kontoebene von „Hot“ zu „Cool“ wechseln, werden Ihnen Schreibvorgänge (pro 10.000) für alle Blobs ohne festgelegte Ebene nur für GPv2-Konten berechnet. Für Blob Storage-Konten fallen hierfür keine Gebühren an. Ihnen werden sowohl Gebühren für Lesevorgänge (pro 10.000) als auch für den Datenabruf (pro GB) berechnet, wenn Sie für Ihr Blob Storage- oder GPv2-Konto von „Cool“ zu „Hot“ wechseln. Für alle Blobs, die aus der Ebene „Cool“ oder „Archiv“ verschoben werden, können auch Gebühren für das frühe Löschen anfallen.

### <a name="cool-and-archive-early-deletion-effective-february-1-2018"></a>Frühes Löschen für „Cool“ und „Archiv“ (ab 1. Februar 2018)

Zusätzlich zur Gebühr, die pro GB und pro Monat berechnet wird, gilt Folgendes: Alle Blobs, die in die Ebene „Cool“ (nur GPv2-Konten) verschoben werden, unterliegen einem Zeitraum von 30 Tagen für das frühe Löschen für „Cool“, und alle Blobs, die in die Ebene „Archiv“ verschoben werden, unterliegen einem Zeitraum von 180 Tagen für das frühe Löschen für „Archiv“. Diese Gebühr fällt anteilig an. Wenn ein Blob beispielsweise in das Archiv verschoben und dann nach 45 Tagen gelöscht oder in die Ebene „Hot“ verschoben wird, wird Ihnen eine Gebühr für das frühe Löschen berechnet, die 135 Speichertagen (180 minus 45) dieses Blobs im Archiv entspricht.

## <a name="comparison-of-the-storage-tiers"></a>Vergleich der Speicherebenen

Die folgende Tabelle enthält eine Gegenüberstellung der Speicherebenen „Hot“, „Cool“ und „Archiv“.

| | **Speicherebene „Hot“** | **Speicherebene „Cool“** | **Speicherebene „Archiv“**
| ---- | ----- | ----- | ----- |
| **Availability** | 99,9 % | 99 % | N/V |
| **Availability** <br> **(RA-GRS-Lesevorgänge)**| 99,99 % | 99,9 % | N/V |
| **Nutzungsgebühren** | Höhere Speicherkosten, geringere Zugriffs- und Transaktionskosten | Geringere Speicherkosten, höhere Zugriffs- und Transaktionskosten | Niedrigste Speicherkosten, höchste Zugriffs- und Transaktionskosten |
| **Mindestobjektgröße** | N/V | N/V | N/V |
| **Mindestspeicherdauer** | N/V | 30 Tage (nur GPv2) | 180 Tage
| **Latenz** <br> **(Zeit bis zum ersten Byte)** | Millisekunden | Millisekunden | < 15 Stunden
| **Skalierbarkeits- und Leistungsziele** | Identisch mit allgemeinen Speicherkonten | Identisch mit allgemeinen Speicherkonten | Identisch mit allgemeinen Speicherkonten |

> [!NOTE]
> Blob Storage-Konten unterstützen die gleichen Leistungs- und Skalierbarkeitsziele wie allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).

## <a name="quickstart-scenarios"></a>Schnellstartszenarien

In diesem Abschnitt werden unter Verwendung des Azure-Portals die folgenden Szenarien veranschaulicht:

* Ändern der Standard-Kontozugriffsebene für ein GPv2- oder Blob Storage-Konto
* Ändern der Ebene eines Blobs in einem GPv2- oder Blob Storage-Konto

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>Ändern der Standard-Kontozugriffsebene für ein GPv2- oder Blob Storage-Konto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Speicherkonto, wählen Sie „Alle Ressourcen“, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Konfiguration**, um die Kontokonfiguration anzuzeigen bzw. zu ändern.

4. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest.

5. Klicken Sie oben im Blatt auf „Speichern“.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>Ändern der Ebene eines Blobs in einem GPv2- oder Blob Storage-Konto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie zum Navigieren zu Ihrem Blob in Ihrem Speicherkonto die Option „Alle Ressourcen“, und wählen Sie Ihr Speicherkonto, Ihren Container und dann Ihr Blob aus.

3. Klicken Sie auf dem Blatt „Blob-Eigenschaften“ auf das Dropdownmenü **Zugriffsebene**, um die Speicherebene **Hot**, **Cool** oder **Archiv** auszuwählen.

5. Klicken Sie oben im Blatt auf „Speichern“.

## <a name="faq"></a>Häufig gestellte Fragen

**Sollte ich Blob Storage- oder GPv2-Konten verwenden, wenn ich für meine Daten das Tiering durchführen möchte?**

Wir empfehlen Ihnen, für das Tiering anstelle von Blob Storage-Konten GPv2-Konten zu nutzen. Für GPv2 werden alle Features von Blob Storage-Konten sowie noch viele weitere Features unterstützt. Die Preise für Blob Storage und GPv2 sind nahezu identisch, aber einige neue Features und Preisreduzierungen gelten nur für GPv2-Konten. Für GPv1-Konten wird das Tiering nicht unterstützt.

Die Preisstruktur von GPv1 und GPv2 unterscheidet sich, und Kunden sollten dies jeweils sorgfältig evaluieren, bevor sie sich für die Nutzung von GPv2-Konten entscheiden. Sie können ein vorhandenes Blob Storage- oder GPv1-Konto leicht in ein GPv2-Konto konvertieren, indem Sie im Azure-Portal einen Schritt ausführen, für den nur ein Klick erforderlich ist. Weitere Informationen finden Sie unter [Optionen für Azure Storage-Konten](../common/storage-account-options.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

**Kann ich Objekte unter demselben Konto auf allen drei Speicherebenen („Hot“, „Cool“ und „Archiv“) speichern?**

Ja. Das Attribut **Access Tier** (Zugriffsebene) auf Kontoebene ist die Standardebene, die für alle Objekte des Kontos gilt, für die keine explizite Ebene festgelegt ist. Mit dem Blobebenentiering können Sie die Zugriffsebene auf dieser Objektebene unabhängig davon festlegen, welche Zugriffsebene für das Konto eingestellt ist. In einem Konto können Blobs in allen drei Speicherebenen („Hot“, „Cool“ oder „Archiv“) vorhanden sein.

**Kann ich die Standardspeicherebene meines Blob Storage- oder GPv2-Kontos ändern?**

Ja. Sie können die Standardspeicherebene ändern, indem Sie für das Speicherkonto das Attribut **Access tier** (Zugriffsebene) festlegen. Die Änderung der Speicherebene gilt für alle gespeicherten Objekte des Kontos, für die keine explizite Ebene festgelegt ist. Wenn Sie für die Speicherebene von „Hot“ zu „Cool“ wechseln, fallen Gebühren für Schreibvorgänge (pro 10.000) für alle Blobs ohne festgelegte Ebene nur für GPv2-Konten an, und wenn Sie von „Cool“ zu „Hot“ wechseln, fallen Gebühren für Lesevorgänge (pro 10.000) und den Datenabruf (pro GB) für alle Blobs in Blob Storage- und GPv2-Konten an.

**Kann ich meine Standard-Kontozugriffsebene auf „Archiv“ festlegen?**

Nein. Nur die Speicherebenen „Hot“ und „Cool“ können als Standard-Kontozugriffsebene festgelegt werden. „Archiv“ kann nur auf Objektebene festgelegt werden.

**In welchen Regionen sind die Speicherebenen „Hot“, „Cool“ und „Archiv“ verfügbar?**

Die Speicherebenen „Hot“ und „Cool“ sind zusammen mit dem Blobebenentiering in allen Regionen verfügbar. Die Speicherebene „Archiv“ ist anfänglich nur in ausgewählten Regionen verfügbar. Eine vollständige Liste finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

**Verhalten sich die Blobs der Speicherebene „Cool“ anders als die Blobs der Speicherebene „Hot“?**

Blobs der Speicherebene „Hot“ weisen die gleiche Latenz wie Blobs in GPv1-, GPv2- und Blob Storage-Konten auf. Blobs der Speicherebene „Cool“ verfügen über eine ähnliche Latenz (in Millisekunden) wie Blobs in GPv1-, GPv2- und Blob Storage-Konten. In GPv1-, GPv2- und Blob Storage-Konten gilt für Blobs auf der Speicherebene „Archiv“ eine Latenzzeit von mehreren Stunden.

Für Blobs der Speicherebene „Cool“ gilt in Bezug auf die Verfügbarkeit ein etwas geringerer Servicelevel (SLA) als für Blobs, die auf der Speicherebene „Hot“ gespeichert sind. Weitere Details finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**Sind die Vorgänge für die Ebenen „Hot“, „Cool“ und „Archiv“ identisch?**

Ja. Alle Vorgänge zwischen „Hot“ und „Cool“ sind zu 100% konsistent. Alle gültigen Archivvorgänge, z.B. Delete, List, Get Blob Properties/Metadata und Set Blob Tier, sind zu 100% mit „Hot“ und „Cool“ konsistent. Ein Blob kann nicht gelesen oder geändert werden, während es sich auf der Ebene „Archiv“ befindet.

**Wie kann ich beim Aktivieren eines Blobs von der Ebene „Archiv“ auf die Ebene „Hot“ oder „Cool“ erkennen, wann die Aktivierung abgeschlossen ist?**

Während der Aktivierung können Sie den Vorgang „Get Blob Properties“ verwenden, um das Attribut **Archive Status** (Archivstatus) abzufragen und so zu ermitteln, wann die Ebenenänderung abgeschlossen ist. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Eigenschaft „Archive Status“ (Archivstatus) entfernt, und die Eigenschaft **Access Tier** (Zugriffsebene) des Blobs spiegelt die neue Ebene vom Typ „Hot“ oder „Cool“ wider.  

**Ab wann werden mir nach dem Festlegen der Ebene eines Blobs die entsprechenden Gebühren berechnet?**

Die Berechnung für ein Blob basiert immer auf der Ebene, die in der Blobeigenschaft **Access Tier** (Zugriffsebene) angegeben ist. Beim Festlegen einer neuen Ebene für ein Blob spiegelt die Eigenschaft **Access Tier** (Zugriffsebene) für alle Übergänge sofort die neue Ebene wider. Eine Ausnahme ist das Aktivieren eines Blobs aus dem Archiv für die Ebene „Hot“ oder „Cool“, da der Vorgang mehrere Stunden dauern kann. In diesem Fall werden Ihnen weiterhin die Kosten für „Archiv“ berechnet, bis die Aktivierung abgeschlossen ist und unter **Access Tier** (Zugriffsebene) die neue Ebene angezeigt wird. Erst ab diesem Zeitpunkt wird Ihnen der neue Preis für „Hot“ bzw. „Cool“ berechnet.

**Wie kann ich ermitteln, ob eine Gebühr für das frühe Löschen anfällt, wenn ich ein Blob lösche oder aus der Ebene „Cool“ oder „Archiv“ verschiebe?**

Für alle Blobs, die vor Ablauf von 30 bzw. 180 Tagen gelöscht oder aus der Ebene „Cool“ (nur GPv2-Konten) oder „Archiv“ verschoben werden, fällt eine anteilige Gebühr für frühes Löschen an (ab 1. Februar 2018). Sie können ermitteln, wie lange sich ein Blob auf der Ebene „Cool“ oder „Archiv“ befunden hat, indem Sie in der Blobeigenschaft **Access Tier Change Time** (Änderungszeitpunkt der Zugriffsebene) den Zeitstempel der letzten Ebenenänderung überprüfen. Weitere Informationen finden Sie im Abschnitt [Frühes Löschen für „Cool“ und „Archiv“](#cool-and-archive-early-deletion).

**Welche Azure-Tools und SDKs unterstützen das Blobebenentiering und die Speicherebene „Archiv“?**

Für das Azure-Portal, PowerShell und CLI-Tools sowie .NET-, Java-, Python- und Node.js-Clientbibliotheken werden das Blobebenentiering und die Speicherebene „Archiv“ unterstützt.  

**Wie viele Daten kann ich auf den Ebenen „Hot“, „Cool“ und „Archiv“ speichern?**

Die Datenspeicherung wird zusammen mit anderen Beschränkungen auf Kontoebene und nicht einzeln pro Speicherebene festgelegt. Daher können Sie wählen, ob Sie Ihr gesamtes Kontingent auf einer Ebene nutzen oder auf alle drei Ebenen verteilen möchten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Evaluieren von „Hot“, „Cool“ und „Archiv“ in GPv2- und Blob Storage-Konten

Prüfen der Verfügbarkeit von „Hot“, „Cool“ und „Archiv“ nach Region unter [Azure-Regionen](https://azure.microsoft.com/regions/#services)

[Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Prüfen der Preise für „Hot“, „Cool“ und „Archiv“ in Blob Storage- und GPv2-Konten nach Region unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)

[Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

---
title: Erstellen eines Sprachmodells mit dem Spracherkennungsdienst – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit dem Spracherkennungsdienst in Microsoft Cognitive Services ein Sprachmodell erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 85e67be406b3d9723476821adfb09fc4db8dc1d1
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068572"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Erstellen eines benutzerdefinierten Sprachmodells

In diesem Dokument erstellen Sie ein benutzerdefiniertes Sprachmodell, das Sie dann mit modernsten Sprachmodellen von Microsoft kombinieren können, um für Ihre App die Interaktion per Spracheingabe zu ermöglichen.

Im Dokument wird Folgendes beschrieben:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Sprachdatasets
> * Erstellen des benutzerdefinierten Sprachmodells

Sollten Sie über kein Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich auf der Seite [Cognitive Services Subscriptions](https://customspeech.ai/Subscriptions) (Cognitive Services-Abonnements), dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist.

Sie können auf die Schaltfläche **Verbindung mit vorhandenem Abonnement herstellen** klicken, um eine Verbindung mit einem Speech Service-Abonnement herzustellen, das im Azure-Portal erstellt wurde.

Informationen zur Erstellung eines Speech Service-Abonnements im Azure-Portal finden Sie auf der Seite zu den [ersten Schritten](get-started.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Wenn Sie ein benutzerdefiniertes Sprachmodell für Ihre Anwendung erstellen möchten, müssen Sie eine Liste mit Beispieläußerungen für das System bereitstellen. Beispielsweise:

*   „Der Patient litt letzte Woche an Urtikaria.“
*   „Die Herniorrhaphie-Narbe des Patienten ist gut verheilt.“

Die Sätze müssen nicht vollständig oder grammatisch korrekt sein, sondern sollen vielmehr Spracheingaben entsprechen, mit denen das System nach der Bereitstellung voraussichtlich konfrontiert wird. Diese Beispiele sollten Art und Inhalt der Aufgabe widerspiegeln, die Benutzer mit Ihrer Anwendung ausführen.

Die Sprachmodelldaten sollten im Format „UTF-8 BOM“ (UTF-8-Bytereihenfolge-Marke) verfasst werden. Die Textdatei muss pro Zeile jeweils ein Beispiel (Satz, Äußerung oder Abfrage) enthalten.

Wenn Sie möchten, dass einige Begriffe eine höhere Gewichtung (Wichtigkeit) erhalten, können Sie mehrere Äußerungen hinzufügen, die den Begriff in Ihren Daten enthalten. 

In der folgenden Tabelle sind die wichtigsten Anforderungen für die Sprachdaten zusammengefasst:

| Eigenschaft | Wert |
|----------|-------|
| Textcodierung | UTF-8 BOM|
| Anzahl von Äußerungen pro Zeile | 1 |
| Maximale Dateigröße | 1,5 GB |
| Anmerkungen | Vermeiden Sie Zeichenwiederholungen mit mehr als vier Zeichen (z.B. „aaaaa“).|
| Anmerkungen | Verwenden Sie keine Sonderzeichen wie „\t“ oder andere UTF-8-Zeichen über U+00A1 in der [Unicode-Zeichentabelle](http://www.utf8-chartable.de/).|
| Anmerkungen | URIs werden ebenfalls abgelehnt, da es keine eindeutige Aussprache für URIs gibt.|

Beim Importieren des Texts wird dieser normalisiert, damit er vom System verarbeitet werden kann. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen durch den Benutzer vorgenommen werden. Informationen zu geeigneter Sprache bei der Vorbereitung Ihrer Sprachdaten finden Sie in den [Richtlinien für die Transkription](prepare-transcription.md).

## <a name="language-support"></a>Sprachunterstützung

Die folgenden Sprachen werden für benutzerdefinierte **Sprache-zu-Text**-Sprachmodelle unterstützt.

Klicken Sie hier, um eine vollständige Liste der [unterstützten Sprachen](supported-languages.md) anzuzeigen

## <a name="import-the-language-data-set"></a>Importieren des Sprachdatasets

Klicken Sie in der Zeile „Language Datasets“ (Sprachdatasets) auf „Importieren“. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

Melden Sie sich am [Spracherkennungsdienst-Portal](https://customspeech.ai) an, wenn Sie Ihr Sprachdataset für den Import vorbereitet haben.  Klicken Sie anschließend auf dem oberen Menüband auf das Dropdownmenü „Custom Speech“, und wählen Sie „Adaptation Data“ (Anpassungsdaten) aus. Beim ersten Versuch, Daten für den Spracherkennungsdienst hochzuladen, wird eine leere Tabelle mit dem Namen „Datasets“ angezeigt.

Klicken Sie in der Zeile „Language Datasets“ (Sprachdatasets) auf „Importieren“, um ein neues Dataset zu importieren. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt. Geben Sie einen Namen und eine Beschreibung ein, um das Dataset später identifizieren zu können, und wählen Sie das Gebietsschema aus. Klicken Sie anschließend auf die Schaltfläche „Datei auswählen“, und navigieren Sie zu der Textdatei mit den Sprachdaten. Klicken Sie auf „Importieren“, um das Dataset hochzuladen. Der Importvorgang kann je nach Größe des Datasets mehrere Minuten dauern.

![Versuch](media/stt/speech-language-datasets-import.png)

Nach Abschluss des Importvorgangs wird wieder die Tabelle mit den Sprachdaten angezeigt, die nun einen Eintrag für Ihr Sprachdataset enthält. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Außerdem haben die Daten einen Status, der ihrem aktuellen Zustand entspricht: „Waiting“ (Warten) bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. „Processing“ (Verarbeitung) bedeutet, dass die Daten gerade überprüft werden. „Complete“ (Abgeschlossen) bedeutet, dass die Daten verwendungsbereit sind. Bei der Datenüberprüfung werden verschiedene Aspekte des Texts in der Datei geprüft und einige Textnormalisierungen für die Daten durchgeführt.

Wenn der Status „Complete“ (Abgeschlossen) lautet, können Sie auf „Bericht anzeigen“ klicken, um den Überprüfungsbericht für die Sprachdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im folgenden Beispiel haben zwei Beispiele die Überprüfung aufgrund von unzulässigen Zeichen nicht bestanden. (In diesem Dataset enthielt die erste Zeile zwei Tabulatorzeichen, die zweite enthielt mehrere Zeichen außerhalb des druckbaren ASCII-Zeichensatzes, und die dritte Zeile war leer.)

![Versuch](media/stt/speech-language-datasets-report.png)

Wenn der Status des Sprachdatasets „Complete“ (Abgeschlossen) lautet, kann es zur Erstellung eines benutzerdefinierten Sprachmodells verwendet werden.

![Versuch](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Erstellen eines benutzerdefinierten Sprachmodells

Wenn Ihre Sprachdaten bereit sind, klicken Sie im Dropdownmenü „Menü“ auf „Language Models“ (Sprachmodelle), um mit der Erstellung eines benutzerdefinierten Sprachmodells zu beginnen. Auf dieser Seite befindet sich eine Tabelle namens „Language Models“ (Sprachmodelle) mit Ihren aktuellen benutzerdefinierten Sprachmodellen. Falls Sie noch keine benutzerdefinierten Sprachmodelle erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema wird in der Tabelle neben dem relevanten Dateneintrag angezeigt.

Das entsprechende Gebietsschema muss ausgewählt werden, bevor Aktionen durchgeführt werden. Das aktuelle Gebietsschema ist im Tabellentitel auf allen Daten-, Modell- und Bereitstellungsseiten angegeben. Klicken Sie zum Ändern des Gebietsschemas auf die Schaltfläche „Change Locale“ (Gebietsschema ändern) unterhalb des Tabellentitels. Sie gelangen auf eine Seite zum Bestätigen des Gebietsschemas. Klicken Sie auf „OK“, um zur Tabelle zurückzukehren.

Geben Sie auf der Seite zum Erstellen des Sprachmodells einen Namen und eine Beschreibung ein, um wichtige Informationen zu diesem Modell (etwa das verwendete Dataset) zu erfassen. Wählen Sie als Nächstes im Dropdownmenü das Basissprachmodell aus. Dieses Modell bildet den Ausgangspunkt für Ihre Anpassung. Sie können zwischen zwei Basissprachmodellen wählen: Das Modell für Suche und Diktat eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das Konversationsmodell eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet. Ein neues Modell mit dem Namen „Universell“ ist ebenfalls öffentlich verfügbar. „Universell“ soll für alle Szenarien gelten und die Modelle vom Typ „Suche und Diktieren“ und „Konversation“ letztendlich ersetzen.

5.  Wählen Sie im Beispiel unten nach dem Angeben des Basissprachmodells über das Dropdownmenü „Language Data“ (Sprachdaten) das Sprachdataset aus, das Sie für die Anpassung verwenden möchten.

![Versuch](media/stt/speech-language-models-create2.png)

Genau wie bei der Erstellung des Akustikmodells können Sie sich optional dafür entscheiden, Ihr neues Modell nach der Verarbeitung offline zu testen. Für Modellevaluierungen ist ein akustisches Dataset erforderlich.

Wenn Sie Ihr Sprachmodell offline testen möchten, aktivieren Sie das Kontrollkästchen „Offlinetests“. Wählen Sie anschließend über das Dropdownmenü ein Akustikmodell aus. Falls Sie noch keine benutzerdefinierten Akustikmodelle erstellt haben, enthält das Menü nur die grundlegenden Akustikmodelle von Microsoft. Bei Verwendung eines Konversationssprachmodells muss hier ein Konversationsakustikmodell verwendet werden. Bei Verwendung eines Sprachmodells für Suche und Diktat muss auch ein Akustikmodell für Suche und Diktat gewählt werden.

Wählen Sie abschließend das Akustikdataset aus, das für die Auswertung verwendet werden soll.

Wenn Sie mit der Verarbeitung beginnen möchten, können Sie „Create“ (Erstellen) wählen, um zur Tabelle mit den Sprachmodellen zu gelangen. Die Tabelle enthält einen neuen Eintrag für dieses Modell. Der Status gibt den Zustand des Modells an – von „Waiting“ (Warten) über „Processing“ (Verarbeitung) bis hin zu „Complete“ (Abgeschlossen).

Wenn das Modell den Zustand „Complete“ (Abgeschlossen) erreicht hat, kann es an einem Endpunkt bereitgestellt werden. Wenn Sie auf „Ergebnis anzeigen“ klicken, werden die Ergebnisse der Offlinetests angezeigt (sofern Offlinetests durchgeführt wurden).

Wenn Sie nachträglich den Namen oder die Beschreibung des Modells ändern möchten, klicken Sie in der entsprechenden Zeile der Sprachmodelltabelle auf den Link „Bearbeiten“.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für die Spracherkennung](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
- [Git-Beispieldaten](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)

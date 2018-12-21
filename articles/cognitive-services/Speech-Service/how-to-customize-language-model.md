---
title: 'Tutorial: Erstellen eines Sprachmodells mit dem Speech-Dienst'
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie ein Sprachmodell mit dem Speech-Dienst erstellen. Dieses benutzerdefinierte Sprachmodell kann mit modernsten Sprachmodellen von Microsoft kombiniert werden, um für Ihre App die Interaktion per Spracheingabe zu ermöglichen.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 0eb946babaa3a01ca933a1290122755978fa017b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093455"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Erstellen eines benutzerdefinierten Sprachmodells

In diesem Dokument erstellen Sie ein benutzerdefiniertes Sprachmodell. Dieses benutzerdefinierte Sprachmodell kann mit modernsten Sprachmodellen von Microsoft kombiniert werden, um für Ihre App die Interaktion per Spracheingabe zu ermöglichen.

Im Dokument wird Folgendes beschrieben:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Sprachdatasets
> * Erstellen des benutzerdefinierten Sprachmodells

Sollten Sie über kein Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um sicherzustellen, dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist, öffnen Sie die Seite [Cognitive Services-Abonnements](https://customspeech.ai/Subscriptions).

Sie können die Schaltfläche **Verbindung mit vorhandenem Abonnement herstellen** auswählen, um eine Verbindung mit einem Speech Services-Abonnement herzustellen, das im Azure-Portal erstellt wurde.

Informationen zur Erstellung eines Speech Services-Abonnements im Azure-Portal finden Sie auf der Seite [Erste Schritte](get-started.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Um ein benutzerdefiniertes Sprachmodell für Ihre Anwendung zu erstellen, müssen Sie eine Liste mit Beispieläußerungen für das System bereitstellen, beispielsweise:

*   "Der Patient litt in der letzten Woche an Nesselsucht."
*   „Die Herniorrhaphie-Narbe des Patienten ist gut verheilt.“

Die Sätze müssen nicht vollständig oder grammatikalisch richtig sein, aber sie sollten die gesprochenen Eingaben, auf die das System bei der Bereitstellung voraussichtlich treffen wird, genau wiedergeben. Diese Beispiele sollten Art und Inhalt der Aufgabe widerspiegeln, die Benutzer mit Ihrer Anwendung ausführen.

Die Sprachmodelldaten sollten im Format „UTF-8 BOM“ (UTF-8-Bytereihenfolge-Marke) verfasst werden. Die Textdatei muss pro Zeile jeweils ein Beispiel (Satz, Äußerung oder Abfrage) enthalten.

Wenn Sie möchten, dass einige Begriffe eine höhere Gewichtung (Wichtigkeit) erhalten, können Sie Ihren Daten mehrere Äußerungen hinzufügen, die den Begriff enthalten.

In der folgenden Tabelle sind die wichtigsten Anforderungen für die Sprachdaten zusammengefasst:

| Eigenschaft | Wert |
|----------|-------|
| Textcodierung | UTF-8 BOM|
| Anzahl von Äußerungen pro Zeile | 1 |
| Maximale Dateigröße | 1,5 GB |
| Anmerkungen | Vermeiden Sie Zeichenwiederholungen mit mehr als vier Zeichen (z.B. „aaaaa“).|
| Anmerkungen | Keine Sonderzeichen wie „\t“ oder andere UTF-8-Zeichen über U+00A1 in der [Unicode-Zeichentabelle](http://www.utf8-chartable.de/)|
| Anmerkungen | URIs werden ebenfalls abgelehnt, da es keine eindeutige Aussprache für URIs gibt.|

Beim Importieren des Texts wird dieser normalisiert, damit er vom System verarbeitet werden kann. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen durch den Benutzer vorgenommen werden. Informationen zu geeigneter Sprache bei der Vorbereitung Ihrer Sprachdaten finden Sie in den [Richtlinien für die Transkription](prepare-transcription.md).

## <a name="language-support"></a>Sprachunterstützung

Weitere Informationen zu benutzerdefinierten **Speech-to-Text**-Sprachmodellen finden Sie in der vollständigen Liste der [ unterstützten Sprachen](language-support.md#text-to-speech).



## <a name="import-the-language-data-set"></a>Importieren des Sprachdatasets

Wählen Sie in der Zeile **Sprachdatasets** die Schaltfläche **Importieren** aus. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

Melden Sie sich am [Speech Services-Portal](https://customspeech.ai) an, wenn Sie bereit sind, Ihr Sprachdataset zu importieren. Wählen Sie zunächst auf dem oberen Menüband das Dropdownmenü **Custom Speech** aus. Wählen Sie dann **Anpassungsdaten** aus. Beim ersten Versuch, Daten für Speech Services hochzuladen, wird eine leere Tabelle mit dem Namen **Datasets** angezeigt.

Um ein neues Dataset zu importieren, wählen Sie die Schaltfläche **Importieren** in der Zeile **Sprachdatasets** aus. Danach zeigt die Website eine Seite zum Hochladen eines neuen Datasets an. Geben Sie einen **Namen** und eine **Beschreibung** ein, um das Dataset später identifizieren zu können, und wählen Sie dann das Gebietsschema aus.

Klicken Sie anschließend auf die Schaltfläche **Datei auswählen**, und navigieren Sie zu der Textdatei mit den Sprachdaten. Wählen Sie anschließend **Importieren** aus. Das Dataset wird nun hochgeladen. Der Importvorgang kann je nach Größe des Datasets mehrere Minuten dauern.

![Testen](media/stt/speech-language-datasets-import.png)

Nach Abschluss des Imports weisen die Sprachdaten einen Eintrag auf, der Ihrem Sprachdataset entspricht. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Darüber hinaus weisen die Daten einen Status auf, der ihrem aktuellen Zustand entspricht. **Warten** bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. **Verarbeitung** bedeutet, dass die Daten aktuell überprüft werden. **Abgeschlossen** bedeutet, dass die Daten verwendungsbereit sind. Bei der Datenüberprüfung werden verschiedene Aspekte des Texts in der Datei geprüft. Außerdem werden einige Textnormalisierungen für die Daten ausgeführt.

Wenn der Status **Abgeschlossen** lautet, können Sie **Bericht anzeigen** auswählen, um den Überprüfungsbericht für die Sprachdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im folgenden Beispiel haben zwei Beispiele die Überprüfung wegen falscher Zeichen nicht bestanden. (In diesem Dataset wies die erste Zeile zwei Tabulatorzeichen auf, die zweite mehrere Zeichen, die nicht Teil des druckbaren ASCII-Zeichensatzes sind, und die dritte Zeile war leer).

![Testen](media/stt/speech-language-datasets-report.png)

Wenn der Status des Sprachdatasets **Abgeschlossen** lautet, kann es zur Erstellung eines benutzerdefinierten Sprachmodells verwendet werden.

![Testen](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Erstellen eines benutzerdefinierten Sprachmodells

Wenn Ihre Sprachdaten bereit sind, wählen Sie im Dropdownmenü **Menü** die Option **Sprachmodelle** aus, um mit der Erstellung eines benutzerdefinierten Sprachmodells zu beginnen. Auf dieser Seite befindet sich eine Tabelle namens **Sprachmodelle** mit Ihren aktuellen benutzerdefinierten Sprachmodellen. Falls Sie noch keine benutzerdefinierten Sprachmodelle erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema wird in der Tabelle neben dem relevanten Dateneintrag angezeigt.

Das entsprechende Gebietsschema muss ausgewählt werden, bevor Aktionen durchgeführt werden. Das aktuelle Gebietsschema ist im Tabellentitel auf allen Daten-, Modell- und Bereitstellungsseiten angegeben. Wählen Sie zum Ändern des Gebietsschemas die Schaltfläche **Gebietsschema ändern** unterhalb des Tabellentitels aus.  Sie gelangen auf eine Seite zum Bestätigen des Gebietsschemas. Klicken Sie auf **OK**, um zur Tabelle zurückzukehren.

Geben Sie auf der Seite „Sprachmodell erstellen“ einen **Namen** und eine **Beschreibung** ein, um wichtige Informationen zu diesem Modell (etwa das verwendete Dataset) zu erfassen. Wählen Sie im nächsten Schritt das **Basissprachmodell** im Dropdownmenü aus. Dieses Modell bildet den Ausgangspunkt für Ihre Anpassung.

Sie können zwischen zwei Basissprachmodellen wählen: Das Modell für Suche und Diktat eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das Konversationsmodell eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet.

Das Modell für Suche und Diktat eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das Konversationsmodell eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet. Ein neues Modell mit dem Namen „Universell“ ist ebenfalls öffentlich verfügbar. „Universell“ soll für alle Szenarien gelten und die Modelle vom Typ „Suche und Diktieren“ und „Konversation“ letztendlich ersetzen.

Verwenden Sie (wie im folgenden Beispiel gezeigt) nach dem Angeben des Basissprachmodells das Dropdownmenü **Sprachdaten**, um das Sprachdataset auszuwählen, das Sie für die Anpassung verwenden möchten.

![Testen](media/stt/speech-language-models-create2.png)

Genau wie bei der Erstellung des Akustikmodells können Sie sich optional dafür entscheiden, Ihr neues Modell nach der Verarbeitung offline zu testen. Für Modellevaluierungen ist ein akustisches Dataset erforderlich.

Wenn Sie Ihr Sprachmodell offline testen möchten, aktivieren Sie das Kontrollkästchen **Offlinetests**. Wählen Sie anschließend über das Dropdownmenü ein Akustikmodell aus. Falls Sie noch keine benutzerdefinierten Akustikmodelle erstellt haben, enthält das Menü nur die grundlegenden Akustikmodelle von Microsoft. Bei Verwendung eines Konversationssprachmodells muss hier ein Konversationsakustikmodell verwendet werden. Bei Verwendung eines Sprachmodells für Suche und Diktat muss auch ein Akustikmodell für Suche und Diktat ausgewählt werden.

Wählen Sie abschließend das Akustikdataset aus, das für die Auswertung verwendet werden soll.

Wählen Sie **Erstellen** aus, wenn Sie zum Starten der Verarbeitung bereit sind. Nun wird die Tabelle der Sprachmodelle angezeigt. Die Tabelle enthält einen neuen Eintrag für dieses Modell. Der Status gibt den Zustand des Modells an: von **Warten** über **Verarbeitung** bis hin zu **Abgeschlossen**.

Wenn das Modell den Zustand **Abgeschlossen** erreicht hat, kann es an einem Endpunkt bereitgestellt werden. Wenn Sie **Ergebnis anzeigen** auswählen, werden die Ergebnisse der Offlinetests angezeigt (sofern Offlinetests ausgeführt wurden).

Wenn Sie nachträglich den **Namen** oder die **Beschreibung** des Modells ändern möchten, klicken Sie in der entsprechenden Zeile der Sprachmodelltabelle auf den Link **Bearbeiten**.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Speech Services-Testabonnements](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
- [Git-Beispieldaten](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)

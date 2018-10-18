---
title: 'Tutorial: Erstellen eines Sprachmodells mit Custom Speech Service – Microsoft Cognitives Services | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mit Custom Speech Service ein Sprachmodell in Microsoft Cognitive Services erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ms.openlocfilehash: 6af2da9ffc7678a58fcf1c647ba89c586066d2ad
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339095"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Erstellen eines benutzerdefinierten Sprachmodells

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

In diesem Tutorial erstellen Sie ein benutzerdefiniertes Sprachmodell für Textabfragen oder Äußerungen, die Benutzer voraussichtlich in einer Anwendung verwenden (entweder per Sprache oder per Eingabe). Dieses benutzerdefinierte Sprachmodell kann mit modernsten Sprachmodellen von Microsoft kombiniert werden, um für Ihre App die Interaktion per Spracheingabe zu ermöglichen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Sprachdatasets
> * Erstellen des benutzerdefinierten Sprachmodells

Sollten Sie über kein Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://cris.ai) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich auf der Seite [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Cognitive Services-Abonnements), dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist.

Sollten keine Abonnements aufgelistet werden, haben Sie zwei Möglichkeiten: Sie können auf die Schaltfläche **Get free subscription** (Kostenloses Abonnement erhalten) klicken und von Cognitive Services ein Konto erstellen lassen. Alternativ können Sie auf die Schaltfläche **Connect existing subscription** (Verbindung mit vorhandenem Abonnement herstellen) klicken, um eine Verbindung mit einem Abonnement für einen benutzerdefinierten Suchdienst herzustellen, das im Azure-Portal erstellt wurde.

Informationen zum Erstellen eines Abonnements für einen benutzerdefinierten Suchdienst über das Azure-Portal finden Sie unter [Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Wenn Sie ein benutzerdefiniertes Sprachmodell für Ihre Anwendung erstellen möchten, müssen Sie eine Liste mit Beispieläußerungen für das System bereitstellen. Beispielsweise:

*   „Er litt letzte Woche an Urtikaria.“
*   „Die Herniorrhaphie-Narbe des Patienten ist gut verheilt.“

Die Sätze müssen nicht vollständig oder grammatisch korrekt sein, sondern sollen vielmehr Spracheingaben entsprechen, mit denen das System nach der Bereitstellung voraussichtlich konfrontiert wird. Diese Beispiele sollten Art und Inhalt der Aufgabe widerspiegeln, die Benutzer mit Ihrer Anwendung ausführen.

Die Sprachmodelldaten müssen in einer Nur-Text-Datei mit US-ASCII- oder UTF-8-Codierung (je nach Gebietsschema) geschrieben werden. Für „en-US“ werden beide Codierungen unterstützt. Für „zh-CN“ wird nur UTF-8 unterstützt. (BOM ist optional.) Die Textdatei muss pro Zeile jeweils ein Beispiel (Satz, Äußerung oder Abfrage) enthalten.

Wenn bestimmte Sätze eine höhere Gewichtung (Wichtigkeit) erhalten sollen, können Sie sie mehrmals hinzufügen. Als Wiederholungsanzahl empfiehlt sich ein Wert zwischen 10 und 100. Bei einer Normalisierung auf 100 lassen sich Sätze sehr einfach in Relation zu diesem Wert gewichten.

In der folgenden Tabelle sind die wichtigsten Anforderungen für die Sprachdaten zusammengefasst:

| Eigenschaft | Wert |
|----------|-------|
| Textcodierung | en-US: US-ACSII oder UTF-8; zh-CN: UTF-8|
| Anzahl von Äußerungen pro Zeile | 1 |
| Maximale Dateigröße | 200 MB |
| Anmerkungen | Vermeiden Sie Zeichenwiederholungen mit mehr als vier Zeichen (beispielweise „aaaaa“).|
| Anmerkungen | Verwenden Sie keine Sonderzeichen wie „\t“ oder andere UTF-8-Zeichen über U+00A1 in der [Unicode-Zeichentabelle](http://www.utf8-chartable.de/).|
| Anmerkungen | URIs werden ebenfalls abgelehnt, da es keine eindeutige Aussprache für URIs gibt.|

Beim Importieren des Texts wird dieser normalisiert, damit er vom System verarbeitet werden kann. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen durch den Benutzer vorgenommen werden. Informationen zu geeigneter Sprache bei der Vorbereitung Ihrer Sprachdaten finden Sie in den [Richtlinien für die Transkription](cognitive-services-custom-speech-transcription-guidelines.md).

## <a name="import-the-language-data-set"></a>Importieren des Sprachdatasets

Klicken Sie in der Zeile „Acoustic Datasets“ (Akustikdatasets) auf „Importieren“. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

Wenn Sie Ihr Sprachdataset für den Import vorbereitet haben, melden Sie sich beim [Custom Speech Service-Portal](https://cris.ai) an.  Klicken Sie anschließend auf dem oberen Menüband auf das Dropdownmenü „Custom Speech“, und wählen Sie „Adaptation Data“ (Anpassungsdaten) aus. Wenn Sie zum ersten Mal Daten in Custom Speech Service hochladen, wird eine leere Tabelle namens „Datasets“ angezeigt.

Klicken Sie in der Zeile „Language Datasets“ (Sprachdatasets) auf „Importieren“, um ein neues Dataset zu importieren. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt. Geben Sie einen Namen und eine Beschreibung ein, um das Dataset später identifizieren zu können. Klicken Sie anschließend auf die Schaltfläche „Datei auswählen“, und navigieren Sie zu der Textdatei mit den Sprachdaten. Klicken Sie auf „Importieren“, um das Dataset hochzuladen. Dies kann je nach Größe des Datasets mehrere Minuten dauern.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-import.png)

Nach Abschluss des Importvorgangs wird wieder die Tabelle mit den Sprachdaten angezeigt, die nun einen Eintrag für Ihr Sprachdataset enthält. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Außerdem haben die Daten einen Status, der ihrem aktuellen Zustand entspricht: „Waiting“ (Warten) bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. „Processing“ (Verarbeitung) bedeutet, dass die Daten gerade überprüft werden. „Complete“ (Abgeschlossen) bedeutet, dass die Daten verwendungsbereit sind. Bei der Datenüberprüfung werden verschiedene Aspekte des Texts in der Datei geprüft und einige Textnormalisierungen für die Daten durchgeführt.

Wenn der Status „Complete“ (Abgeschlossen) lautet, können Sie auf „Bericht anzeigen“ klicken, um den Überprüfungsbericht für die Sprachdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im folgenden Beispiel haben zwei Beispiele die Überprüfung aufgrund von unzulässigen Zeichen nicht bestanden. (In diesem Dataset enthielt das erste Beispiel zwei Emoticons, das zweite enthielt mehrere Zeichen außerhalb des druckbaren ASCII-Zeichensatzes.)

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-report.png)

Wenn der Status des Sprachdatasets „Complete“ (Abgeschlossen) lautet, kann es zur Erstellung eines benutzerdefinierten Sprachmodells verwendet werden.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Erstellen eines benutzerdefinierten Sprachmodells

Wenn Ihre Sprachdaten bereit sind, klicken Sie im Dropdownmenü „Menü“ auf „Language Models“ (Sprachmodelle), um mit der Erstellung eines benutzerdefinierten Sprachmodells zu beginnen. Auf dieser Seite befindet sich eine Tabelle namens „Language Models“ (Sprachmodelle) mit Ihren aktuellen benutzerdefinierten Sprachmodellen. Falls Sie noch keine benutzerdefinierten Sprachmodelle erstellt haben, ist die Tabelle leer. Das aktuelle Gebietsschema ist im Tabellentitel angegeben. Wenn Sie ein Sprachmodell für eine andere Sprache erstellen möchten, klicken Sie auf „Gebietsschema ändern“. Weitere Informationen zu unterstützten Sprachen finden Sie im Abschnitt zum [Ändern des Gebietsschemas](cognitive-services-custom-speech-change-locale.md). Klicken Sie zum Erstellen eines neuen Modells unterhalb des Tabellentitels auf den Link „Neu erstellen“.

Geben Sie auf der Seite zum Erstellen des Sprachmodells einen Namen und eine Beschreibung ein, um wichtige Informationen zu diesem Modell (etwa das verwendete Dataset) zu erfassen. Wählen Sie als Nächstes im Dropdownmenü das Basissprachmodell aus. Dieses Modell bildet den Ausgangspunkt für Ihre Anpassung. Sie können zwischen zwei Basissprachmodellen wählen: Das _Microsoft-Sprachmodell für Suche und Diktat_ eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das _Konversationssprachmodell von Microsoft_ eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet.

Wählen Sie nach Angabe des Basissprachmodells über das Dropdownmenü „Language Data“ (Sprachdaten) das Sprachdataset aus, das Sie für die Anpassung verwenden möchten.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-language-models-create2.png)

Genau wie bei der Erstellung des Akustikmodells können Sie sich optional dafür entscheiden, Ihr neues Modell nach der Verarbeitung offline zu testen. Da es sich hierbei um eine Bewertung der Spracherkennung handelt, wird für Offlinetests ein Akustikdataset benötigt.

Wenn Sie Ihr Sprachmodell offline testen möchten, aktivieren Sie das Kontrollkästchen „Offlinetests“. Wählen Sie anschließend über das Dropdownmenü ein Akustikmodell aus. Falls Sie noch keine benutzerdefinierten Akustikmodelle erstellt haben, enthält das Menü nur die grundlegenden Akustikmodelle von Microsoft. Bei Verwendung eines Konversationssprachmodells muss hier ein Konversationsakustikmodell verwendet werden. Bei Verwendung eines Sprachmodells für Suche und Diktat muss auch ein Akustikmodell für Suche und Diktat gewählt werden.

Wählen Sie abschließend das Akustikdataset aus, das für die Auswertung verwendet werden soll.

Klicken Sie auf „Erstellen“, wenn Sie zum Starten der Verarbeitung bereit sind. Daraufhin wird wieder die Tabelle mit den Sprachmodellen angezeigt. Die Tabelle enthält einen neuen Eintrag für dieses Modell. Der Status gibt den Zustand des Modells an – von „Waiting“ (Warten) über „Processing“ (Verarbeitung) bis hin zu „Complete“ (Abgeschlossen).

Wenn das Modell den Zustand „Complete“ (Abgeschlossen) erreicht hat, kann es an einem Endpunkt bereitgestellt werden. Wenn Sie auf „Ergebnis anzeigen“ klicken, werden die Ergebnisse der Offlinetests angezeigt (sofern Offlinetests durchgeführt wurden).

Wenn Sie nachträglich den Namen oder die Beschreibung des Modells ändern möchten, klicken Sie in der entsprechenden Zeile der Sprachmodelltabelle auf den Link „Bearbeiten“.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Sprachmodell für die Verwendung mit Text entwickelt. Im nächsten Tutorial erfahren Sie, wie Sie ein benutzerdefiniertes Akustikmodell für die Verwendung mit Audiodateien und Transkriptionen verwenden.

> [!div class="nextstepaction"]
> [Erstellen eines benutzerdefinierten Akustikmodells](cognitive-services-custom-speech-create-acoustic-model.md)

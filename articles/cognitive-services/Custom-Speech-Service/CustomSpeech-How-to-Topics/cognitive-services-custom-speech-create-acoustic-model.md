---
title: 'Tutorial: Erstellen eines Akustikmodells mit Custom Speech Service – Microsoft Cognitive Services | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie mit Custom Speech Service ein Akustikmodell in Microsoft Cognitive Services erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 60fea175e8dffeefeb9cb3ecaadad5d8fc7caeb9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971540"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Erstellen eines benutzerdefinierten Akustikmodells

In diesem Tutorial erstellen Sie ein benutzerdefiniertes Akustikmodell für Sprachdaten, die von Ihrer Anwendung erkannt werden sollen. Die Erstellung eines benutzerdefinierten Akustikmodells ist hilfreich, wenn Ihre Anwendung für die Verwendung in einer bestimmten Umgebung, etwa einer lauten Werksumgebung, oder für die Verwendung durch bestimmte Benutzer entwickelt wurde.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Akustikdatasets
> * Erstellen des benutzerdefinierten Akustikmodells

Sollten Sie über kein Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://cris.ai) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich auf der Seite [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Cognitive Services-Abonnements), dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist.

Sollten keine Abonnements aufgelistet werden, haben Sie zwei Möglichkeiten: Sie können auf die Schaltfläche **Get free subscription** (Kostenloses Abonnement erhalten) klicken und von Cognitive Services ein Konto erstellen lassen. Alternativ können Sie auf die Schaltfläche **Connect existing subscription** (Verbindung mit vorhandenem Abonnement herstellen) klicken, um eine Verbindung mit einem Abonnement für einen benutzerdefinierten Suchdienst herzustellen, das im Azure-Portal erstellt wurde.

Informationen zum Erstellen eines Abonnements für einen benutzerdefinierten Suchdienst über das Azure-Portal finden Sie unter [Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Für die Anpassung des Akustikmodells an eine bestimmte Domäne ist eine Sammlung von Sprachdaten erforderlich. Sie besteht aus einer Reihe von Audiodateien mit Sprachdaten sowie einer Textdatei mit Transkriptionen von jeder Audiodatei. Die Audiodaten sollten repräsentativ für das Szenario sein, in dem Sie das Erkennungsmodul einsetzen möchten.

Beispiel:

*   Wenn Sie die Spracherkennung in einer lauten Werksumgebung verbessern möchten, sollten in den Audiodateien Personen zu hören sein, die in einer lauten Werksumgebung sprechen.
<a name="Preparing data to customize the acoustic model"></a>
*   Wenn Sie dagegen die Leistung eines einzelnen Lautsprechers optimieren möchten, um beispielsweise alle informellen Gespräche zu transkribieren, sollten die Audiodateien dementsprechend viele Beispiele nur für diesen Lautsprecher enthalten.

Ein akustisches Dataset zur Anpassung des Akustikmodells besteht aus zwei Teilen: (1) einer Reihe von Audiodateien mit den Sprachdaten und (2) einer Datei mit den Transkriptionen sämtlicher Audiodateien.

### <a name="audio-data-recommendations"></a>Empfehlungen für Audiodaten

*   Alle Audiodateien im Dataset sollten im Audioformat WAV (RIFF) gespeichert werden.
*   Das Audio muss eine Samplingrate von 8 kHz oder 16 kHz haben. Die Beispielwerte sollten als dekomprimierte, signierte PCM-16-Bit-Ganzzahlen (Shorts) gespeichert werden.
*   Es werden nur einkanalige Audiodateien (Mono) unterstützt.
*   Die Audiodateien müssen eine Länge zwischen 100 ms und 1 Minute haben. Jede Audiodatei sollte idealerweise mit mindestens 100 ms ohne Ton beginnen und enden. Ein Wert zwischen 500 ms und 1 Sekunde ist üblich.
*   Wenn in Ihren Daten Hintergrundgeräusche zu hören sind, empfiehlt es sich, in Ihren Daten vor und/oder nach dem Sprachinhalt auch über einige Beispiele mit längeren Segmenten ohne Ton zu verfügen, beispielsweise einige Sekunden.
*   Jede Audiodatei sollte genau eine Äußerung enthalten. Dies kann beispielsweise ein Satz zum Diktieren, eine Abfrage oder ein Gesprächsbeitrag eines Sprachdialogsystems sein.
*   Jede Audiodatei im Dataset sollte einen eindeutigen Dateinamen und die Erweiterung „.wav“ aufweisen.
*   Alle Dateien sollten in einem Ordner ohne Unterverzeichnisse zur Verfügung gestellt werden, und daraus sollte ein Paket mit einem ZIP-Datei-Archiv erstellt werden.

> [!NOTE]
> Datenimporte über das Webportal sind derzeit auf 2 GB beschränkt. Folglich ist dies die maximale Größe eines Akustikdatasets. Dies entspricht ca. 17 Stunden Audioaufnahmen bei 16 kHz oder 34 Stunden Audioaufnahmen bei 8 kHz. In der folgenden Tabelle sind die wichtigsten Anforderungen für die Audiodaten zusammengefasst.
>

| Eigenschaft | Wert |
|---------- |----------|
| Dateiformat | RIFF (WAV) |
| Samplingrate | 8.000 Hz oder 16.000 Hz |
| Kanäle | 1 (Mono) |
| Beispielformat | PCM, 16-Bit-Ganzzahl |
| Dateidauer | 0,1 Sekunden < Dauer < 60 Sekunden |
| Ruhe | > 0,1 Sekunden |
| Archivformat | ZIP |
| Maximale Archivgröße | 2 GB |

### <a name="transcriptions"></a>Transkriptionen

Die Transkriptionen für sämtliche WAV-Dateien sollten in einer einzelnen Textdatei enthalten sein. Jede Zeile der Transkriptionsdatei sollte den Namen einer der Audiodateien gefolgt von der entsprechenden Transkription enthalten. Der Dateiname und die Transkription sollten durch ein Tabulatorzeichen (\t) getrennt werden.

  Beispiel: 
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

Der Text der Transkriptionen wird normalisiert, damit diese vom System verarbeitet werden können. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen der Daten für den Custom Speech Service durch den Benutzer vorgenommen werden. Lesen Sie bei der Vorbereitung Ihrer Transkriptionen den Abschnitt zu [Richtlinien für die Transkription](cognitive-services-custom-speech-transcription-guidelines.md) für die entsprechende Sprache.

Die folgenden Schritte werden über das [Custom Speech Service-Portal](https://cris.ai) ausgeführt. 

## <a name="import-the-acoustic-data-set"></a>Importieren des Akustikdatasets

Nachdem die Audiodateien und Transkriptionen vorbereitet wurden, können sie in das Webportal des Diensts importiert werden.

Stellen Sie hierzu zunächst sicher, dass Sie sich beim [Custom Speech Service-Portal](https://cris.ai) angemeldet haben. Klicken Sie anschließend auf dem oberen Menüband auf das Dropdownmenü „Custom Speech“, und wählen Sie „Adaptation Data“ (Anpassungsdaten) aus. Wenn Sie zum ersten Mal Daten in Custom Speech Service hochladen, wird eine leere Tabelle namens „Datasets“ angezeigt. 

Klicken Sie in der Zeile „Acoustic Datasets“ (Akustikdatasets) auf „Importieren“. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

Geben Sie einen _Namen_ und eine _Beschreibung_ in die entsprechenden Textfelder ein. Sie eignen sich zum Nachverfolgen der verschiedenen, von Ihnen hochgeladenen Datasets. Klicken Sie anschließend bei „Transcription File“ (Transkriptionsdatei) und „WAV-Dateien“ auf „Datei auswählen“, und wählen Sie Ihre Text-Transkriptionsdatei bzw. das ZIP-Archiv der WAV-Dateien aus. Klicken Sie dann auf „Importieren“, um Ihre Daten hochzuladen. Anschließend werden Ihre Daten hochgeladen. Bei größeren Datasets kann dieser Vorgang mehrere Minuten dauern.

Nach Abschluss des Uploads wird wieder die Tabelle „Acoustic Datasets“ (Akustikdatasets) angezeigt, die nun einen Eintrag für Ihr Akustikdataset enthält. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Außerdem haben die Daten einen Status, der ihrem aktuellen Zustand entspricht: „Waiting“ (Warten) bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. „Processing“ (Verarbeitung) bedeutet, dass die Daten gerade überprüft werden. „Complete“ (Abgeschlossen) bedeutet, dass die Daten verwendungsbereit sind.

Die Datenüberprüfung beinhaltet einige Überprüfungen in den Audiodateien in Bezug auf das Dateiformat, die Länge und die Samplingrate und in den Transkriptionsdateien in Bezug auf das Dateiformat und die Textnormalisierung.

Wenn der Status „Abgeschlossen“ lautet, können Sie auf „Details“ klicken, um den Überprüfungsbericht zu den Akustikdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im folgenden Beispiel sind in zwei WAV-Dateien aufgrund eines falschen Audioformats Fehler bei der Überprüfung aufgetreten (in diesem Dataset enthielt eine Datei eine falsche Samplingrate, und die andere Datei wies das falsche Dateiformat auf).

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

Wenn Sie den Wert in „Name“ oder „Beschreibung“ des Datasets irgendwann ändern möchten, können Sie auf den Link „Bearbeiten“ klicken und diese Einträge ändern. Beachten Sie, dass die Audiodateien oder Transkriptionen nicht geändert werden können.

## <a name="create-a-custom-acoustic-model"></a>Erstellen eines benutzerdefinierten Akustikmodells

Wenn der Status Ihres Akustikdatasets „Complete“ (Abgeschlossen) lautet, kann es zur Erstellung eines benutzerdefinierten Akustikmodells verwendet werden. Klicken Sie hierzu im Dropdownmenü „Custom Speech“ auf „Acoustic Models“ (Akustikmodelle). Ihnen wird eine Tabelle mit dem Namen „Your models“ (Ihre Modelle) angezeigt, in der alle Ihre benutzerdefinierten Akustikmodelle aufgeführt werden. Bei der ersten Verwendung ist diese Tabelle leer. Das aktuelle Gebietsschema wird im Tabellentitel angezeigt. Akustikmodelle können derzeit nur für Englisch (USA) erstellt werden.

Klicken Sie zum Erstellen eines neuen Modells unter dem Tabellentitel auf den Link „Neu erstellen“. Geben Sie wie zuvor einen Namen und eine Beschreibung ein, um dieses Modell identifizieren zu können. Das Feld „Beschreibung“ kann beispielsweise verwendet werden, um aufzuzeichnen, welches Startmodell und Akustikdataset für die Erstellung des Modells verwendet wurden. Wählen Sie anschließend über das Dropdownmenü ein „Basisakustikmodell“ aus. Das Basismodell ist der Ausgangspunkt für Ihre Anpassung. Sie können zwischen zwei Basisakustikmodellen wählen. Das _Microsoft-Akustikmodell für Suche und Diktat_ eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das _Microsoft-Konversationsmodell_ eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet. Beachten Sie, dass die Wartezeit für die Teilergebnisse in Konversationsmodellen höher als bei Modellen für Suche und Diktat ist.

Wählen Sie anschließend die Akustikdaten aus dem Dropdownmenü aus, die Sie für die Anpassung verwenden möchten.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

Sie können sich optional dafür entscheiden, Ihr neues Modell nach der Verarbeitung einem Offlinetest zu unterziehen. Hierbei erfolgen eine Sprache-zu-Text-Auswertung für ein bestimmtes Akustikdataset mithilfe des angepassten Akustikmodells sowie eine Berichterstattung zu den Ergebnissen. Aktivieren Sie das Kontrollkästchen „Genauigkeitsprüfung“, um diese Tests auszuführen. Wählen Sie anschließend ein Akustikmodell aus dem Dropdownmenü aus. Wenn Sie keine benutzerdefinierten Sprachmodelle erstellt haben, werden nur die Basissprachmodelle in der Dropdownliste angezeigt. Schauen Sie sich die [Beschreibung](cognitive-services-custom-speech-create-language-model.md) der Basissprachmodelle im Leitfaden an, und wählen Sie das geeignetste Modell aus.

Wählen Sie abschließend das Akustikdataset aus, das für die Auswertung des benutzerdefinierten Modells verwendet werden soll. Wenn Sie eine Genauigkeitsprüfung durchführen, ist es wichtig, Akustikdaten auszuwählen, die sich von den für die Modellerstellung verwendeten Daten unterscheiden, um eine realistische Vorstellung von der Leistung des Modells zu bekommen. Beachten Sie zudem, dass Offlinetests auf 1.000 Äußerungen beschränkt sind. Wenn das zu testende Akustikdataset umfangreicher ist, werden nur die ersten 1.000 Äußerungen ausgewertet.

Klicken Sie auf „Erstellen“, wenn mit der Durchführung des Anpassungsprozesses gestartet werden kann.

In der Tabelle mit den Akustikmodellen wird Ihnen jetzt ein neuer Eintrag angezeigt, der diesem neuen Modell entspricht. Der Status des Prozesses wird in der Tabelle wiedergegeben. Die Status lauten „Warten“, „Wird verarbeitet“ und „Abgeschlossen“.

![Versuch](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Akustikmodell für die Verwendung mit Audiodateien und Transkripts entwickelt. Wenn Sie eine benutzerdefinierte Sprachdatei für die Verwendung mit Textdateien erstellen möchten, fahren Sie mit dem Tutorial zum Erstellen eines benutzerdefinierten Sprachmodells fort.

> [!div class="nextstepaction"]
> [Erstellen eines benutzerdefinierten Akustikmodells](cognitive-services-custom-speech-create-language-model.md)

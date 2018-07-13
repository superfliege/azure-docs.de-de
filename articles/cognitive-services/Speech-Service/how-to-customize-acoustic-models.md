---
title: Erstellen eines Akustikmodells mit dem Speech Service – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit dem Speech Service in Microsoft Cognitive Services ein Akustikmodell erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 3e247b6f81a9306cbad630d42c3a41d8ce3045a5
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345123"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Erstellen eines benutzerdefinierten Akustikmodells

Die Erstellung eines benutzerdefinierten Akustikmodells ist hilfreich, wenn Ihre Anwendung für die Verwendung in einer bestimmten Umgebung, wie z.B einem Auto, mit bestimmten Aufzeichnungsgeräten oder unter bestimmten Bedingungen oder für die Verwendung durch bestimmte Benutzer entwickelt wurde. Zu Beispielen zählen Sprechen mit Akzent, bestimmte Hintergrundgeräusche oder die Verwendung eines bestimmten Mikrofons für Aufzeichnungen.

Auf dieser Seite lernen Sie Folgendes:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Akustikdatasets
> * Erstellen des benutzerdefinierten Akustikmodells

Sollten Sie über kein Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich auf der Seite [Cognitive Services Subscriptions](https://customspeech.ai/Subscriptions) (Cognitive Services-Abonnements), dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist.

Sie können auf die Schaltfläche **Verbindung mit vorhandenem Abonnement herstellen** klicken, um eine Verbindung mit einem Speech Service-Abonnement herzustellen, das im Azure-Portal erstellt wurde.

Informationen zur Erstellung eines Speech Service-Abonnements im Azure-Portal finden Sie auf der Seite zu den [ersten Schritten](get-started.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Für die Anpassung eines Akustikmodells an eine bestimmte Domäne ist eine Sammlung von Sprachdaten erforderlich. Diese Sammlung kann ein paar Äußerungen oder auch mehrere hundert Stunden Spracheingabe umfassen. Sie besteht aus einer Reihe von Audiodateien mit Sprachdaten sowie einer Textdatei mit Transkriptionen von jeder Audiodatei. Die Audiodaten sollten repräsentativ für das Szenario sein, in dem Sie das Erkennungsmodul einsetzen möchten.

Beispiel:

*   Wenn Sie die Spracherkennung in einer lauten Werksumgebung verbessern möchten, sollten in den Audiodateien Personen zu hören sein, die in einer lauten Werksumgebung sprechen.
*   Wenn Sie dagegen die Leistung eines einzelnen Lautsprechers optimieren möchten, um beispielsweise alle informellen Gespräche zu transkribieren, sollten die Audiodateien dementsprechend viele Beispiele nur für diesen Lautsprecher enthalten.

Ein akustisches Dataset zur Anpassung des Akustikmodells besteht aus zwei Teilen: (1) einer Reihe von Audiodateien mit den Sprachdaten und (2) einer Datei mit den Transkriptionen sämtlicher Audiodateien.

### <a name="audio-data-recommendations"></a>Empfehlungen für Audiodaten

*   Alle Audiodateien im Dataset sollten im Audioformat WAV (RIFF) gespeichert werden.
*   Das Audio muss eine Samplingrate von 8 kHz oder 16 kHz haben. Die Beispielwerte sollten als dekomprimierte, signierte PCM-16-Bit-Ganzzahlen (Shorts) gespeichert werden.
*   Es werden nur einkanalige Audiodateien (Mono) unterstützt.
*   Die Audiodateien müssen eine Länge zwischen 100 ms und 1 Minute haben. Jede Audiodatei sollte idealerweise mit 100 ms ohne Ton beginnen und enden. Ein Wert zwischen 500 ms und 1 Sekunde ist üblich.
*   Wenn in Ihren Daten Hintergrundgeräusche zu hören sind, empfiehlt es sich, in Ihren Daten vor und/oder nach dem Sprachinhalt auch über einige Beispiele mit längeren Segmenten ohne Ton zu verfügen, beispielsweise einige Sekunden.
*   Jede Audiodatei sollte genau eine Äußerung enthalten. Dies kann beispielsweise ein Satz zum Diktieren, eine Abfrage oder ein Gesprächsbeitrag eines Sprachdialogsystems sein.
*   Jede Audiodatei im Dataset sollte einen eindeutigen Dateinamen und die Erweiterung „WAV“ aufweisen.
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
| Ruhe | >0,1 Sekunden |
| Archivformat | ZIP |
| Maximale Archivgröße | 2 GB |

## <a name="language-support"></a>Sprachunterstützung

Die folgenden Sprachen werden für benutzerdefinierte **Sprache-zu-Text**-Sprachmodelle unterstützt.

Klicken Sie hier, um eine vollständige Liste der [unterstützten Sprachen](supported-languages.md) anzuzeigen

### <a name="transcriptions-for-the-audio-dataset"></a>Transkriptionen für das Audio-Dataset

Die Transkriptionen für sämtliche WAV-Dateien sollten in einer einzelnen Textdatei enthalten sein. Jede Zeile der Transkriptionsdatei sollte den Namen einer der Audiodateien gefolgt von der entsprechenden Transkription enthalten. Der Dateiname und die Transkription sollten durch ein Tabulatorzeichen (\t) getrennt werden.

  Beispiel:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Die Transkription sollte als UTF-8 BOM codiert werden

Der Text der Transkriptionen wird normalisiert, damit diese vom System verarbeitet werden können. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen der Daten für den Custom Speech Service durch den Benutzer vorgenommen werden. Lesen Sie bei der Vorbereitung Ihrer Transkriptionen den Abschnitt zu [Richtlinien für die Transkription](prepare-transcription.md) für die entsprechende Sprache.

Die folgenden Schritte werden über das [Speech Service-Portal](https://customspeech.ai) ausgeführt.

## <a name="import-the-acoustic-data-set"></a>Importieren des Akustikdatasets

Nachdem die Audiodateien und Transkriptionen vorbereitet wurden, können sie in das Webportal des Diensts importiert werden.

Stellen Sie hierzu zunächst sicher, dass Sie sich beim [Speech Service-Portal](https://customspeech.ai) angemeldet haben. Klicken Sie anschließend auf dem oberen Menüband auf das Dropdownmenü „Custom Speech“, und wählen Sie „Adaptation Data“ (Anpassungsdaten) aus. Wenn Sie zum ersten Mal Daten in Custom Speech Service hochladen, wird eine leere Tabelle namens „Datasets“ angezeigt. 

Klicken Sie in der Zeile „Acoustic Datasets“ (Akustikdatasets) auf „Importieren“. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

![Versuch](media/stt/speech-acoustic-datasets-import.png)

Geben Sie einen _Namen_ und eine _Beschreibung_ in die entsprechenden Textfelder ein. Beschreibungen eignen sich zum Nachverfolgen der verschiedenen, von Ihnen hochgeladenen Datasets. Klicken Sie anschließend bei „Transcription File“ (Transkriptionsdatei) und „WAV-Dateien“ auf „Datei auswählen“, und wählen Sie Ihre Text-Transkriptionsdatei bzw. das ZIP-Archiv der WAV-Dateien aus. Klicken Sie nach Abschluss der Vorbereitungen auf „Importieren“, um Ihre Daten hochzuladen. Anschließend werden Ihre Daten hochgeladen. Bei größeren Datasets kann dieser Vorgang mehrere Minuten dauern.

Nach Abschluss des Uploads wird wieder die Tabelle „Acoustic Datasets“ (Akustikdatasets) angezeigt, die nun einen Eintrag für Ihr Akustikdataset enthält. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Darüber hinaus haben die Daten einen Status, der ihrem aktuellen Zustand entspricht. „NotStarted“ (Nicht gestartet) bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. „Running“ (Wird ausgeführt) bedeutet, dass die Daten gerade überprüft werden. „Complete“ (Abgeschlossen) bedeutet, dass die Daten verwendungsbereit sind.

Die Datenüberprüfung beinhaltet einige Überprüfungen in den Audiodateien in Bezug auf das Dateiformat, die Länge und die Samplingrate und in den Transkriptionsdateien in Bezug auf das Dateiformat und die Textnormalisierung.

Wenn der Status „Erfolgreich“ lautet, können Sie auf „Details“ klicken, um den Überprüfungsbericht zu den Akustikdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im folgenden Beispiel sind in zwei WAV-Dateien aufgrund eines falschen Audioformats Fehler bei der Überprüfung aufgetreten (in diesem Dataset enthielt eine Datei eine falsche Samplingrate, und die andere Datei wies das falsche Dateiformat auf).

![Versuch](media/stt/speech-acoustic-datasets-report.png)

Wenn Sie den Wert in „Name“ oder „Beschreibung“ des Datasets irgendwann ändern möchten, können Sie auf den Link „Bearbeiten“ klicken und diese Einträge ändern. Die Audiodateien oder Transkriptionen können nicht geändert werden.

## <a name="create-a-custom-acoustic-model"></a>Erstellen eines benutzerdefinierten Akustikmodells

Wenn der Status Ihres Akustikdatasets „Complete“ (Abgeschlossen) lautet, kann es zur Erstellung eines benutzerdefinierten Akustikmodells verwendet werden. Klicken Sie hierzu im Dropdownmenü „Custom Speech“ auf „Acoustic Models“ (Akustikmodelle). Ihnen wird eine Tabelle mit dem Namen „Your models“ (Ihre Modelle) angezeigt, in der alle Ihre benutzerdefinierten Akustikmodelle aufgeführt werden. Bei der ersten Verwendung ist diese Tabelle leer. Das aktuelle Gebietsschema wird im Tabellentitel angezeigt. Akustikmodelle können derzeit nur für Englisch (USA) erstellt werden.

Klicken Sie zum Erstellen eines neuen Modells unter dem Tabellentitel auf den Link „Neu erstellen“. Geben Sie wie zuvor einen Namen und eine Beschreibung ein, um dieses Modell identifizieren zu können. Das Feld „Beschreibung“ kann beispielsweise verwendet werden, um aufzuzeichnen, welches Startmodell und Akustikdataset für die Erstellung des Modells verwendet wurde. Wählen Sie anschließend über das Dropdownmenü ein „Basisakustikmodell“ aus. Das Basismodell ist der Ausgangspunkt für Ihre Anpassung. Sie können zwischen zwei Basisakustikmodellen wählen. Das _Microsoft-Akustikmodell für Suche und Diktat_ eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. Das _Microsoft-Konversationsmodell_ eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet. Die Wartezeit für die Teilergebnisse in Konversationsmodellen ist höher als bei Modellen für Suche und Diktat.

> [!NOTE]
> Derzeit führen wir das neue universelle Modell ein, das sich für alle Szenarios eignen soll. Die oben genannten Modelle bleiben weiterhin öffentlich verfügbar.

Wählen Sie anschließend die Akustikdaten aus dem Dropdownmenü aus, die Sie für die Anpassung verwenden möchten.

![Versuch](media/stt/speech-acoustic-models-create2.png)

Sie können sich optional dafür entscheiden, Ihr neues Modell nach der Verarbeitung einer Genauigkeitsprüfung zu unterziehen. Hierbei erfolgen eine Sprache-zu-Text-Auswertung für ein bestimmtes Akustikdataset mithilfe des angepassten Akustikmodells sowie eine Berichterstattung zu den Ergebnissen. Aktivieren Sie das Kontrollkästchen „Genauigkeitsprüfung“, um diese Tests auszuführen. Wählen Sie anschließend ein Akustikmodell aus dem Dropdownmenü aus. Wenn Sie keine benutzerdefinierten Sprachmodelle erstellt haben, werden nur die Basissprachmodelle in der Dropdownliste angezeigt. Schauen Sie sich die [Beschreibung](how-to-customize-language-model.md) der Basissprachmodelle im Leitfaden an, und wählen Sie das geeignetste Modell aus.

Wählen Sie abschließend das Akustikdataset aus, das für die Auswertung des benutzerdefinierten Modells verwendet werden soll. Wenn Sie eine Genauigkeitsprüfung durchführen, ist es wichtig, Akustikdaten auszuwählen, die sich von den für die Modellerstellung verwendeten Daten unterscheiden, um eine realistische Vorstellung von der Leistung des Modells zu bekommen. Bei der Genauigkeitsprüfung von Trainingsdaten dürfen Sie die Leistung des angepassten Modells unter realen Bedingungen nicht auswerten. Das Ergebnis wäre zu optimistisch. Beachten Sie zudem, dass die Genauigkeitsprüfung auf 1.000 Äußerungen beschränkt ist. Wenn das zu testende Akustikdataset umfangreicher ist, werden nur die ersten 1.000 Äußerungen ausgewertet.

Klicken Sie auf „Erstellen“, wenn mit der Durchführung des Anpassungsprozesses gestartet werden kann.

In der Tabelle mit den Akustikmodellen wird Ihnen jetzt ein neuer Eintrag angezeigt, der diesem neuen Modell entspricht. Der Status des Prozesses wird in der Tabelle wiedergegeben. Die Status lauten „Warten“, „Wird verarbeitet“ und „Abgeschlossen“.

![Versuch](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für die Spracherkennung](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-windows.md)
- [Git-Beispieldaten](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)

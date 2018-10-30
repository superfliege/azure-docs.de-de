---
title: 'Tutorial: Erstellen eines Akustikmodells mit dem Speech-Dienst'
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech-Dienst in Azure Cognitive Services ein Akustikmodell erstellen.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 81449889ae9218f2b59ea48f10c676dcee9aa8b1
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466085"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Erstellen eines benutzerdefinierten Akustikmodells

Die Erstellung eines benutzerdefinierten Akustikmodells ist hilfreich, wenn Ihre Anwendung für die Verwendung in einer bestimmten Umgebung (etwa in einem Auto), mit bestimmten Aufzeichnungsgeräten oder Bedingungen oder für die Verwendung durch bestimmte Benutzer entwickelt wurde. Zu Beispielen zählen Sprechen mit Akzent, bestimmte Hintergrundgeräusche oder die Verwendung eines bestimmten Mikrofons für Aufzeichnungen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Vorbereiten der Daten
> * Importieren des Akustikdatasets
> * Erstellen des benutzerdefinierten Akustikmodells

Sollten Sie über kein Azure Cognitive Services-Konto verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich auf der Seite [Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Cognitive Services-Abonnements), dass Ihr Cognitive Services-Konto mit einem Abonnement verbunden ist.

Durch Klicken auf **Verbindung mit vorhandenem Abonnement herstellen** können Sie eine Verbindung mit einem Speech-Dienstabonnement herstellen, das im Azure-Portal erstellt wurde.

Informationen zum Erstellen eines Speech-Dienstabonnements über das Azure-Portal finden Sie unter [Kostenloses Testen des Speech-Diensts](get-started.md).

## <a name="prepare-the-data"></a>Vorbereiten der Daten

Für die Anpassung eines Akustikmodells an eine bestimmte Domäne ist eine Sammlung von Sprachdaten erforderlich. Diese Sammlung kann einige wenige Äußerungen oder auch mehrere hundert Stunden an Spracheingaben umfassen. Sie besteht aus einer Reihe von Audiodateien mit Sprachdaten sowie einer Textdatei mit Transkriptionen von jeder Audiodatei. Die Audiodaten sollten repräsentativ für das Szenario sein, in dem Sie das Erkennungsmodul einsetzen möchten.

Beispiel: 

* Wenn Sie die Spracherkennung in einer lauten Produktionsumgebung verbessern möchten, sollten in den Audiodateien Personen zu hören sein, die in einer lauten Produktionsumgebung sprechen.
* Wenn Sie dagegen die Leistung für einen einzelnen Sprecher optimieren möchten, um beispielsweise alle informellen Gespräche eines Flugdatenschreibers zu transkribieren, sollten die Audiodateien viele Beispiele dieses einen Sprechers enthalten.

Ein Akustikdataset zur Anpassung des Akustikmodells besteht aus zwei Teilen: einer Reihe von Audiodateien mit den Sprachdaten und einer Datei mit den Transkriptionen sämtlicher Audiodateien.

### <a name="audio-data-recommendations"></a>Empfehlungen für Audiodaten

* Alle Audiodateien im Dataset sollten im Audioformat WAV (RIFF) gespeichert werden.
* Die Audiodaten müssen mit einer Samplingrate von 8 oder 16 Kilohertz (kHz) vorliegen, und die Beispielwerte müssen als unkomprimierte 16-Bit-PCM-Ganzzahlen mit Vorzeichen (Shorts) gespeichert sein.
* Es werden nur Audiodateien mit einem Kanal (Mono) unterstützt.
* Die Audiodateien müssen eine Länge zwischen 100 Mikrosekunden und einer Minute haben. Jede Audiodatei sollte idealerweise mit 100 Mikrosekunden ohne Ton beginnen und enden. Ein Wert zwischen 500 Mikrosekunden und einer Sekunde ist üblich.
* Falls in Ihren Daten Hintergrundgeräusche enthalten sind, sollten die Daten auch einige Beispiele mit längeren stillen Segmenten (beispielsweise einige Sekunden) vor und/oder nach dem Sprachinhalt enthalten.
* Jede Audiodatei muss eine einzelne Äußerung darstellen. Dies kann beispielsweise ein Satz zum Diktieren, eine Abfrage oder ein Gesprächsbeitrag eines Sprachdialogsystems sein.
* Jede Audiodatei im Dataset muss einen eindeutigen Dateinamen und die Erweiterung „.wav“ besitzen.
* Die Audiodateien müssen sich in einem einzelnen Ordner ohne Unterverzeichnisse befinden, und die gesamten Audiodateien müssen als einzelnes ZIP-Dateiarchiv gepackt werden.

> [!NOTE]
> Datenimporte über das Webportal sind derzeit auf 2 GB beschränkt. Folglich ist dies die maximale Größe eines Akustikdatasets. Diese Größe entspricht etwa 17 Stunden an Audio bei 16 kHz oder 34 Stunden an Audio bei 8 kHz. In der folgenden Tabelle sind die wichtigsten Anforderungen für die Audiodaten zusammengefasst:
>

| Eigenschaft | Wert |
|---------- |----------|
| Dateiformat | RIFF (WAV) |
| Samplingrate | 8.000 Hertz (Hz) oder 16.000 Hz |
| Kanäle | 1 (Mono) |
| Beispielformat | PCM, 16-Bit-Ganzzahl |
| Dateidauer | 0,1 Sekunden < Dauer < 60 Sekunden |
| Ruhe | > 0,1 Sekunden |
| Archivformat | .zip |
| Maximale Archivgröße | 2 GB |

> [!NOTE]
> Dateinamen dürfen nur lateinische Zeichen enthalten und müssen das Format „<Dateiname>.<Erweiterung>“ aufweisen.

## <a name="language-support"></a>Sprachunterstützung

Eine vollständige Liste mit den Sprachen, die für benutzerdefinierte Sprachmodelle vom Typ **Spracherkennung** unterstützt werden, finden Sie unter [Spracherkennung](language-support.md#speech-to-text).

### <a name="transcriptions-for-the-audio-dataset"></a>Transkriptionen für das Audio-Dataset

Die Transkriptionen für sämtliche WAV-Dateien sollten in einer einzelnen Textdatei enthalten sein. Jede Zeile der Transkriptionsdatei muss den Namen einer der Audiodateien enthalten – gefolgt von der jeweiligen Transkription. Der Dateiname und die Transkription sollten durch ein Tabulatorzeichen (\t) getrennt werden.

  Beispiel: 
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Die Transkription muss als UTF-8-Bytereihenfolge-Marke (byte order mark, BOM) codiert sein.

Der Text der Transkriptionen wird normalisiert, damit diese vom System verarbeitet werden können. Einige wichtige Normalisierungen müssen jedoch noch _vor_ dem Hochladen der Daten für den Custom Speech Service durch den Benutzer vorgenommen werden. Informationen dazu, welche Sprache bei der Vorbereitung Ihrer Transkripte verwendet werden muss, finden Sie unter [Transkriptionsrichtlinien zur Verwendung des Speech-Diensts](prepare-transcription.md).

Führen Sie die Schritte in den nächsten Abschnitten über das [Portal des Speech-Diensts](https://cris.ai) aus.

## <a name="import-the-acoustic-dataset"></a>Importieren des Akustikdatasets

Die vorbereiteten Audiodateien und Transkriptionen können in das Webportal des Diensts importiert werden.

Vergewissern Sie sich hierbei zunächst, dass Sie beim [Portal des Speech-Portals](https://cris.ai) angemeldet sind. Wählen Sie anschließend auf dem Menüband in der Dropdownliste **Custom Speech** die Option **Adaptation Data** (Anpassungsdaten) aus. Wenn Sie zum ersten Mal Daten in Custom Speech Service hochladen, wird eine leere Tabelle namens **Datasets** angezeigt. 

Klicken Sie in der Zeile **Acoustic Datasets** (Akustikdatasets) auf die Schaltfläche **Importieren**. Daraufhin wird eine Seite zum Hochladen eines neuen Datasets angezeigt.

![Seite zum Importieren von Akustikdaten](media/stt/speech-acoustic-datasets-import.png)

Geben Sie in den Feldern **Name** und **Beschreibung** die entsprechenden Informationen ein. Aussagekräftige Beschreibungen sind hilfreich, um den Überblick über die verschiedenen hochgeladenen Datasets zu behalten. 

Klicken Sie in den Feldern **Transcriptions file (.txt)** (Transkriptionsdatei (.txt)) und **Audiodateien (.zip)** auf **Durchsuchen**, und wählen Sie Ihre Transkriptionsdatei im Nur-Text-Format bzw. das ZIP-Archiv mit den WAV-Dateien aus. Klicken Sie nach Abschluss der Vorbereitungen auf **Importieren**, um Ihre Daten hochzuladen. Daraufhin werden Ihre Daten hochgeladen. Bei umfangreicheren Datasets kann der Importvorgang mehrere Minuten dauern.

Kehren Sie nach Abschluss des Uploadvorgangs zur Tabelle mit den **Akustikdatasets zurück**. Ein Eintrag wird angezeigt, der Ihrem Akustikdataset entspricht. Wie Sie sehen, wurde ihm eine eindeutige ID (GUID) zugewiesen. Außerdem wird der aktuelle Status der Daten angezeigt: *NotStarted* (Nicht gestartet) bedeutet, dass sich die Daten in einer Verarbeitungswarteschlange befinden. *Running* (Wird ausgeführt) bedeutet, dass die Daten gerade überprüft werden. *Complete* (Abgeschlossen) bedeutet, dass die Daten verwendungsbereit sind.

Die Datenüberprüfung beinhaltet einige Überprüfungen in den Audiodateien in Bezug auf das Dateiformat, die Länge und die Samplingrate und in den Transkriptionsdateien in Bezug auf das Dateiformat und die Textnormalisierung.

Wenn der Status *Erfolgreich* lautet, können Sie auf **Details** klicken, um den Überprüfungsbericht für die Akustikdaten anzuzeigen. Die Anzahl von Äußerungen, die die Überprüfung bestanden oder nicht bestanden haben, sowie Details zu den fehlerhaften Äußerungen werden angezeigt. Im Beispiel auf der folgenden Abbildung war die Überprüfung von zwei WAV-Dateien aufgrund eines nicht ordnungsgemäßen Audioformats nicht erfolgreich. In diesem Dataset hat eine Datei eine falsche Samplingrate und die andere ein falsches Dateiformat.

![Detailseite für die Anpassung von Daten](media/stt/speech-acoustic-datasets-report.png)

Wenn Sie den Namen oder die Beschreibung des Datasets ändern möchten, können Sie auf den Link **Bearbeiten** klicken und den entsprechenden Eintrag ändern. Die Transkriptions- und Audiodateieinträge können nicht geändert werden.

## <a name="create-a-custom-acoustic-model"></a>Erstellen eines benutzerdefinierten Akustikmodells

Wenn der Status Ihres Akustikdatasets *Complete* (Abgeschlossen) lautet, können Sie das Dataset zur Erstellung eines benutzerdefinierten Akustikmodells verwenden. Wählen Sie hierzu in der Dropdownliste **Custom Speech** die Option **Acoustic Models** (Akustikmodelle) aus. In einer Tabelle namens **Ihre Modelle** werden alle Ihre benutzerdefinierten Akustikmodelle aufgeführt. Bei der ersten Verwendung ist diese Tabelle leer. Im Tabellentitel wird das aktuelle Gebietsschema angezeigt. Akustikmodelle können derzeit nur für Englisch (USA) erstellt werden.

Klicken Sie zum Erstellen eines neuen Modells unter dem Tabellentitel auf den Link **Neu erstellen**. Geben Sie wie zuvor einen Namen und eine Beschreibung ein, um dieses Modell identifizieren zu können. Im Feld **Beschreibung** kann beispielsweise erfasst werden, welches Startmodell und Akustikdataset für die Erstellung des Modells verwendet wurden. 

Wählen Sie als Nächstes in der Dropdownliste **Base Acoustic Model** (Basisakustikmodell) ein Basismodell aus. Das Basismodell bildet den Ausgangspunkt für Ihre Anpassung. Sie können zwischen zwei Basisakustikmodellen wählen:
* Das **Microsoft-Akustikmodell für Suche und Diktat** eignet sich für anwendungsspezifische Spracheingaben wie Befehle, Suchabfragen oder diktierten Text. 
* Das **Microsoft-Konversationsmodell** eignet sich für die Erkennung von Spracheingaben im Gesprächsstil. Diese Art von Spracheingabe richtet sich üblicherweise an eine andere Person und wird in Callcentern oder Besprechungen verwendet. 

Die Wartezeit für die Teilergebnisse in Konversationsmodellen ist höher als bei Modellen für Suche und Diktat.

> [!NOTE]
> Derzeit führen wir das neue **universelle** Modell ein, das sämtliche Szenarien abdecken soll. Die oben genannten Modelle bleiben weiterhin öffentlich verfügbar.

Wählen Sie als Nächstes in der Dropdownliste **Acoustic Data** (Akustikdaten) die Akustikdaten aus, die Sie für die Anpassung verwenden möchten.

![Die Seite zum Erstellen des Akustikmodells](media/stt/speech-acoustic-models-create2.png)

Nach Abschluss der Verarbeitung können Sie Ihr neues Modell optional einer Genauigkeitsprüfung unterziehen. Bei diesem Test wird eine Spracherkennungsprüfung für ein bestimmtes Akustikdataset unter Verwendung des angepassten Akustikmodells ausgeführt und ein Bericht mit den Ergebnissen generiert. Wenn Sie diesen Test ausführen möchten, aktivieren Sie das Kontrollkästchen **Genauigkeitsprüfung**. Wählen Sie anschließend in der Dropdownliste ein Sprachmodell aus. Falls Sie noch keine benutzerdefinierten Sprachmodelle erstellt haben, werden in der Dropdownliste nur die Basissprachmodelle angezeigt. Informationen zur Wahl des am besten geeigneten Sprachmodells finden Sie unter [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md).

Wählen Sie abschließend das Akustikdataset aus, das zur Überprüfung des benutzerdefinierten Modells verwendet werden soll. Bei der Genauigkeitsprüfung ist es wichtig, ein Akustikdataset auszuwählen, das sich vom für die Modellerstellung verwendeten Dataset unterscheidet, um eine realistische Vorstellung von der Leistung des Modells zu bekommen. Wenn Sie die Genauigkeitsprüfung mit Trainingsdaten durchführen, können Sie die Leistung des angepassten Modells unter realen Bedingungen nicht beurteilen. Das Ergebnis wäre zu optimistisch. Beachten Sie zudem, dass die Genauigkeitsprüfung auf 1.000 Äußerungen beschränkt ist. Ist das zu testende Akustikdataset umfangreicher, werden nur die ersten 1.000 Äußerungen ausgewertet.

Klicken Sie auf **Erstellen**, um den Anpassungsprozess zu starten.

In der Tabelle mit den Akustikmodellen wird ein neuer Eintrag angezeigt, der diesem neuen Modell entspricht. Darüber hinaus wird in der Tabelle auch der Status des Prozesses angezeigt: *Warten*, *Wird verarbeitet...* oder *Abgeschlossen*.

![Die Seite mit den Akustikmodellen](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Nächste Schritte

- [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
- [Git-Beispieldaten](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)

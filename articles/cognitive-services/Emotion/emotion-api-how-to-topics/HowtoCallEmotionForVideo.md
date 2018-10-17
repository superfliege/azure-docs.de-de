---
title: 'Beispiel: Aufrufen der Emotionen-API für Videos'
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Emotionen-API für Video in Cognitive Services aufrufen.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: sample
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 2687145a89c11efb4a3bcb1494a39806e9aae551
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238606"
---
# <a name="example-call-emotion-api-for-video"></a>Beispiel: Aufrufen der Emotionen-API für Videos

> [!IMPORTANT]
> Die Emotionen-API wird am 15. Februar 2019 eingestellt. Die Funktion zur Erkennung von Emotionen ist jetzt als Teil der [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/) allgemein verfügbar. 

Dieses Handbuch veranschaulicht das Aufrufen der Emotionen-API für Video. Die Beispiele sind mithilfe der Clientbibliothek der Emotionen-API für Video in C# geschrieben.

### <a name="Prep">Vorbereitung</a>
Um die Emotionen-API für Video zu verwenden, benötigen Sie ein Video, das Personen zeigt, vorzugsweise mit Blick in die Kamera.

### <a name="Step1">Schritt 1: Autorisieren des API-Aufrufs</a>
Für jeden Aufruf der Emotionen-API für Video benötigen Sie einen Abonnementschlüssel. Dieser Schlüssel muss entweder über einen Abfragezeichenfolgen-Parameter übergeben oder im Anforderungsheader angegeben werden. Wenn Sie den Abonnementschlüssel über eine Abfragezeichenfolge übergeben möchten, sehen Sie sich die folgende Anforderungs-URL für die Emotionen-API für Video als Beispiel an:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Alternativ können Sie den Abonnementschlüssel auch im HTTP-Anforderungsheader angeben:

```
ocp-apim-subscription-key: <Your subscription key>
```

Wenn Sie eine Clientbibliothek verwenden, wird der Abonnementschlüssel durch den Konstruktor der Klasse VideoServiceClient übergeben. Beispiel: 

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Wie Sie einen Abonnementschlüssel erhalten, erfahren Sie unter [Abonnements] (https://azure.microsoft.com/try/cognitive-services/).

### <a name="Step2">Schritt 2: Hochladen eines Videos in den Dienst und Überprüfen des Status</a>
Die grundlegendste Art und Weise, einen Aufruf der Emotionen-API für Video auszuführen, ist das direkte Hochladen eines Videos. Dies erfolgt durch Senden einer POST-Anforderung des Inhaltstyps „application/octet-stream“ beim Lesen von Daten aus einer Videodatei. Die maximale Größe des Videos beträgt 100 MB.

Bei Verwendung der Clientbibliothek erfolgt die Stabilisierung per Upload durch Übergabe in ein Datenstromobjekt. Betrachten Sie das folgende Beispiel:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Beachten Sie, dass die Methode CreateOperationAsync von VideoServiceClient asynchron ist. Die aufrufende Methode sollte für die Verwendung der Await-Klausel ebenfalls als asynchron gekennzeichnet werden.
Wenn das Video bereits im Web ist und eine öffentliche URL besitzt, ist der Zugriff auf die Emotionen-API für Video durch Angeben der URL möglich. In diesem Beispiel handelt es sich beim Anforderungstext um eine JSON-Zeichenfolge, die die URL enthält.

Bei Verwendung der Clientbibliothek kann die Stabilisierung über eine URL problemlos mit einer anderen Überladung der Methode CreateOperationAsync ausgeführt werden.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Diese Uploadmethode ist für alle Aufrufe der Emotionen-API für Video identisch.

Nachdem Sie ein Video hochgeladen haben, sollten Sie als Nächstes dessen Status überprüfen. Da Videodateien in der Regel größer und vielfältiger als andere Dateien sind, können Benutzer in diesem Schritt eine lange Verarbeitungszeit erwarten. Die Zeit hängt von der Größe und der Dauer der Datei ab.

Mit der Clientbibliothek können Sie den Vorgangsstatus und das Ergebnis mithilfe der Methode GetOperationResultAsync abrufen.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
In der Regel sollte die Clientseite in regelmäßigen Abständen den Vorgangsstatus abrufen, bis der Status „Erfolgreich“ oder „Fehler“ angezeigt wird.

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

Wenn für VideoOperationResult der Status „Erfolgreich“ angezeigt wird, kann das Ergebnis durch Umwandlung von VideoOperationResult in VideoOperationInfoResult<VideoAggregateRecognitionResult> und Zugriff auf das ProcessingResult-Feld abgerufen werden.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Schritt 3: Abrufen und Verstehen der Emotionenerkennung und Nachverfolgen der JSON-Ausgabe</a>

Das Ausgabeergebnis enthält die Metadaten aus den Gesichtern in der jeweiligen Datei im JSON-Format.

Wie in Schritt 2 beschrieben, ist die JSON-Ausgabe im Feld ProcessingResult von OperationResult verfügbar, wenn dafür der Status „Erfolgreich“ angezeigt wird.

Die JSON-Datei für Gesichtserkennung und -nachverfolgung umfasst die folgenden Attribute:

Attribut | BESCHREIBUNG
-------------|-------------
Version | Bezieht sich auf die Version der Emotionen-API für Video-JSON-Datei.
Zeitskala | „Teilstriche“ pro Sekunde des Videos.
Offset  |Der Zeitoffset für Zeitstempel. In Version 1.0 von Emotionen-API für Video ist dies immer 0. In zukünftigen unterstützten Szenarien kann sich dieser Wert ändern.
Framerate | Frames des Videos pro Sekunde.
Fragments   | Die Metadaten werden in verschiedene kleinere, als Fragmente bezeichnete Stücke aufgeteilt. Jedes Fragment enthält Startzeitpunkt, Dauer, Intervallnummer und Ereignis(se).
Start   | Die Startzeit des ersten Ereignisses in „Ticks“.
Duration |  Die Länge des Fragments in „Ticks“.
Intervall |  Die Länge jedes Ereignisses innerhalb des Fragments in „Ticks“.
Ereignisse  | Ein Array von Ereignissen. Das äußere Array stellt ein Zeitintervall dar. Das innere Array besteht aus 0 oder mehr Ereignissen, die zu diesem Zeitpunkt aufgetreten sind.
windowFaceDistribution |    Prozentueller Anteil der Gesichter, die während des Ereignisses eine bestimmte Emotion zeigten.
windowMeanScores |  Mittlere Bewertungen für jede Emotion der Gesichter im Bild.

Der Grund für diese JSON-Formatierung ist, die APIs für zukünftige Szenarios einzurichten, bei denen es darauf ankommen wird, Metadaten schnell abzurufen und einen großen Datenstrom von Ergebnissen zu verwalten. Diese Formatierung verwendet sowohl die Techniken der Fragmentierung (sodass Sie die Metadaten in zeitbasierte Teile aufteilen können, um die Möglichkeit zu haben, nur das herunterzuladen, was Sie benötigen) und Segmentierung (sodass Sie die Ereignisse aufteilen können, wenn sie zu groß werden). Einige einfache Berechnungen können Ihnen helfen, die Daten zu transformieren. Wenn ein Ereignis beispielsweise mit 6.300 (Ticks), einer Zeitskala von 2.997 (Ticks/s) und Framerate von 29,97 (Bilder/s) begonnen hat, dann gilt:

*   Start/Zeitskala = 2,1 Sekunden
*   Sekunden x (Framerate/Zeitskala) = 63 Bilder

Im Folgenden finden Sie ein einfaches Beispiel für das Extrahieren der JSON-Daten in ein Pro-Frame-Format für Gesichtserkennung und -nachverfolgung:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Wenn Sie einmal eine Visualisierung erstellen sollten, in der Sie das ursprüngliche Video mit Ihren Ergebnissen überlagern, subtrahieren Sie 250 Millisekunden von den bereitgestellten Zeitstempeln, weil Emotionen im Zeitverlauf geglättet werden.

### <a name="Summary">Zusammenfassung</a>
In diesem Handbuch haben Sie die Funktionen der Emotionen-API für Video kennen gelernt: wie Sie ein Video hochladen, seinen Status überprüfen und Metadaten zur Erkennung von Emotionen abrufen können.

Weitere Informationen zu API-Details finden Sie im API-Referenzhandbuch „[Referenz zur Emotionen-API für Video](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)“.

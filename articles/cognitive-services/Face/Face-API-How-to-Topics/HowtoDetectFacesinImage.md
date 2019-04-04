---
title: Erkennen von Gesichtern auf einem Bild – Gesichtserkennungs-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die verschiedenen Daten verwenden, die vom Gesichtserkennungsfeature zurückgegeben werden.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: sbowles
ms.openlocfilehash: bf3af8f5d1d2f063199a8275c2f49c70140e8732
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588770"
---
# <a name="get-face-detection-data"></a>Abrufen von Gesichtserkennungsdaten

In diesem Leitfaden wird gezeigt, wie mithilfe der Gesichtserkennung aus einem bestimmten Bild Attribute wie Geschlecht, Alter oder Körperhaltung extrahiert werden können. Die Codeausschnitte in diesem Leitfaden sind mithilfe der Clientbibliothek der Gesichtserkennungs-API in C# geschrieben; dieselbe Funktionalität steht aber auch über die [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) zur Verfügung.

In diesem Leitfaden wird Folgendes gezeigt:

- Abrufen der Position und der Dimensionen von Gesichtern auf einem Bild
- Abrufen der Position verschiedener Gesichtsmerkmale (Pupillen, Nase, Mund usw.) auf einem Bild.
- Schätzen des Geschlechts, des Alters, der Stimmung und anderer Attribute des erkannten Gesichts.

## <a name="setup"></a>Einrichtung

Für diesen Leitfaden wird davon ausgegangen, dass Sie bereits ein **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)**-Objekt namens `faceClient` mit einem Gesichtserkennungsabonnementschlüssel und einer Gesichtserkennungs-Endpunkt-URL erstellt haben. Davon ausgehend können Sie das Gesichtserkennungsfeature verwenden, indem Sie entweder **[DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet)** (wird für diesen Leitfaden verwendet) oder **[DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet)** aufrufen. Im [Schnellstart: Erkennen von Gesichtern in einem Bild mit dem Azure Face .NET SDK](../quickstarts/csharp-detect-sdk.md) erhalten Sie eine Einrichtungsanleitung.

Das Hauptaugenmerk dieses Leitfadens liegt auf dem Detect-Aufruf, d. h., welche Argumente Sie übergeben können, und was Sie mit den zurückgegebenen Daten tun können. Wir empfehlen, nur Abfragen für die Features durchzuführen, die Sie benötigen, da die Durchführung jedes Vorgangs zusätzlich Zeit beansprucht.

## <a name="get-basic-face-data"></a>Abrufen von einfachen Gesichtsdaten

Wenn Sie auf einem Bild Gesichter erkennen und deren Position auf einem Bild bestimmen möchten, rufen Sie die Methode auf. Dabei muss der _returnFaceId_-Parameter auf **true** (Standard) festgelegt sein.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Die zurückgegebenen **[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)**-Objekte können mit ihrer eindeutigen ID abgefragt werden, sowie ein Rechteck (Gesichtsrahmen), das die Pixelkoordinaten des Gesichts angibt.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Sehen Sie sich die Klasse **[FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet)** an, um weitere Informationen zu erhalten, wie Sie die Position und die Dimensionen des Gesichts analysieren. In der Regel umfasst dieses Rechteck die Augen, Augenbrauen, die Nase und den Mund – die Stirn, die Ohren und das Kinn sind nicht zwangsläufig enthalten. Wenn Sie diesen Gesichtsrahmen verwenden möchten, um ein Bild auf den gesamten Kopf oder einen Bereich in der Halbtotalen im Hochformat (ein Bild vom Foto-ID-Typ) zuzuschneiden, sollten Sie den Bereich des rechteckigen Gesichtsrahmens in alle Richtungen etwas vergrößern.

## <a name="get-face-landmarks"></a>Abrufen von Gesichtsmerkmalpunkten

Gesichtsmerkmale bestehen aus mehreren einfach zu findenden Punkten im Gesicht. Beispiel sind die Pupillen oder die Nasenspitze. Sie erhalten die Gesichtsmerkmalpunktdaten, indem Sie den Parameter _returnFaceLandmarks_ auf **true** festlegen.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

Standardmäßig sind 27 Bezugspunkte vordefiniert. Die folgende Abbildung zeigt alle 27 Punkte:

![Screenshot: Eine Abbildung eines Gesichts mit den 27 beschrifteten Gesichtsmerkmalpunkten](../Images/landmarks.1.jpg)

Die Punkte werden genau wie beim rechteckigen Gesichtsrahmen in Pixel zurückgegeben. Der folgende Code zeigt, wie Sie die Position von Nase und Pupillen abrufen können:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

Gesichtsmerkmalpunktdaten können auch verwendet werden, um die Ausrichtung eines Gesichts exakt zu bestimmen. Beispielsweise kann die Ausrichtung des Gesichts als Vektor vom Mittelpunkt des Munds zum Mittelpunkt der Augen definiert werden. Der Code unten berechnet diesen Vektor:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Wenn die Ausrichtung des Gesichts bekannt ist, können Sie den rechteckigen Gesichtsrahmen drehen, um ihn noch besser am Gesicht auszurichten. Wenn Sie Gesichter aus einem Bild ausschneiden möchten, können Sie Bilder programmgesteuert drehen, sodass Gesichter immer senkrecht dargestellt werden.

## <a name="get-face-attributes"></a>Abrufen von Gesichtsattributen

Zusätzlich zu Gesichtsrahmen und Gesichtsmerkmalpunkten kann die Gesichtserkennungs-API mehrere konzeptuelle Gesichtsattribute analysieren. Das umfasst:

- Alter
- Geschlecht
- Intensität des Lächelns
- Gesichtsbehaarung
- Brille
- 3D-Kopfhaltung
- Emotion

> [!IMPORTANT]
> Diese Attribute werden mithilfe statistischer Algorithmen vorhergesagt und sind möglicherweise nicht immer genau. Treffen Sie Entscheidungen auf Grundalge von Attributdaten nur vorsichtig.
>

Wenn Sie Gesichtsattribute analysieren möchten, legen Sie den Parameter _returnFaceAttributes_ auf eine Liste von **[FaceAttributeType-Enumerationswerten](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** fest.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Rufen Sie dann Verweise für die zurückgegebenen Daten ab, und führen Sie je nach Bedarf weitere Vorgänge aus.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Weitere Informationen zu den einzelnen Attributen finden Sie im [Glossar](../Glossary.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie erfahren, wie die verschiedenen Funktionen der Gesichtserkennung verwendet werden. Sehen Sie sich als nächstes das [Glossar](../Glossary.md) an, um detailliertere Informationen zu den Gesichtsdaten zu erhalten, die Sie abgerufen haben.

## <a name="related-topics"></a>Verwandte Themen

- [Referenzdokumentation (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referenzdokumentation (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)

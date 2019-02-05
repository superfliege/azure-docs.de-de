---
title: 'Beispiel: Erkennen von Gesichtern in Bildern – Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Gesichtserkennungs-API Gesichter in Bildern erkennen.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 8c89a43910a5e98286a82de8626870d3aec55b94
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214210"
---
# <a name="example-how-to-detect-faces-in-image"></a>Beispiel: Gewusst wie: Gesichtserkennung in Bildern

In diesem Leitfaden wird gezeigt, wie die Gesichtserkennung in einem Bild durch extrahierte Gesichtsattribute wie Geschlecht, Alter oder Körperhaltung erfolgt. Die Beispiele sind in C# geschrieben und verwenden die Clientbibliothek für die Gesichtserkennungs-API. 

## <a name="concepts"></a>Konzepte

Wenn Sie mit den folgenden Begriffen in diesem Leitfaden nicht vertraut sind, können Sie jederzeit im [Glossar](../Glossary.md) nach den entsprechenden Definitionen suchen: 

- Gesichtserkennung
- Gesichtszüge
- Kopfhaltung
- Gesichtsattribute

## <a name="preparation"></a>Vorbereitung

In diesem Beispiel werden folgende Features veranschaulicht: 

- Erkennen von Gesichtern in einem Bild und Markieren der Gesichter durch rechteckige Rahmen
- Analysieren der Position von Pupillen, Nase oder Mund und Markieren dieser Punkte im Bild
- Analysieren von Kopfhaltung, Geschlecht und Alter des Gesichts

Um diese Features ausführen zu können, müssen Sie ein Bild mit mindestens einem deutlich erkennbarem Gesicht vorbereiten. 

## <a name="step-1-authorize-the-api-call"></a>Schritt 1: Autorisieren des API-Aufrufs

Für jeden Aufruf der Gesichtserkennungs-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel muss entweder über einen Abfragezeichenfolgenparameter übergeben oder im Anforderungsheader angegeben werden. Wenn Sie den Abonnementschlüssel über eine Abfragezeichenfolge übergeben möchten, sehen Sie sich die Anforderungs-URL für [Gesicht – Erkennen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als Beispiel an:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Alternativ können Sie den Abonnementschlüssel auch im HTTP-Anforderungsheader angeben: **ocp-apim-subscription-key: &lt;Abonnementschlüssel&gt;**. Wenn Sie eine Clientbibliothek verwenden, wird der Abonnementschlüssel durch den Konstruktor der FaceServiceClient-Klasse übergeben. Beispiel: 
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step-2-upload-an-image-to-the-service-and-execute-face-detection"></a>Schritt 2: Hochladen eines Bilds in den Dienst und Ausführen der Gesichtserkennung

Die grundlegendste Möglichkeit zum Ausführen der Gesichtserkennung ist der direkte Upload eines Bilds. Dies erfolgt durch Senden einer POST-Anforderung mit dem Inhaltstyp „application/octet-stream“ beim Datenlesevorgang eines JPEG-Bilds. Die maximale Größe des Bilds beträgt 4 MB.

Bei Verwendung der Clientbibliothek erfolgt die Gesichtserkennung per Upload durch Übergabe in ein Datenstromobjekt. Betrachten Sie das folgende Beispiel:

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

Beachten Sie, dass die DetectAsync-Methode des FaceServiceClient asynchron ist. Die aufrufende Methode sollte für die Verwendung der Await-Klausel ebenfalls als asynchron gekennzeichnet werden.
Wenn das Bild bereits im Internet veröffentlicht ist und eine URL aufweist, kann die Gesichtserkennung auch durch Angabe der URL ausgeführt werden. In diesem Beispiel handelt es sich beim Anforderungstext um eine JSON-Zeichenfolge, die die URL enthält.
Bei Verwendung der Clientbibliothek kann die Gesichtserkennung über eine URL ganz einfach mit einer anderen Überladung der DetectAsync-Methode ausgeführt werden.

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

Die FaceRectangle-Eigenschaft, die mit den erkannten Gesichtern zurückgegeben wird, enthält im Wesentlichen die Positionen auf dem Gesicht in Pixel. In der Regel umfasst dieses Rechteck die Augen, Augenbrauen, die Nase und den Mund – die Stirn, die Ohren und das Kinn sind nicht enthalten. Wenn Sie ein Bild auf den gesamten Kopf oder einen Bereich in der Halbtotalen im Hochformat (ein Bild vom Foto-ID-Typ) zuschneiden, sollten Sie den Bereich des rechteckigen Gesichtsrahmens vergrößern, da der Gesichtsbereich für einige Anwendungen möglicherweise zu klein ist. Um ein Gesicht präziser zu lokalisieren, ist die im nächsten Abschnitt beschriebene Verwendung von Gesichtszügen (Lokalisierung von Gesichtsmerkmalen oder der Gesichtsausrichtung) besonders hilfreich.

## <a name="step-3-understanding-and-using-face-landmarks"></a>Schritt 3: Grundlegendes zu Gesichtszügen und ihrer Verwendung

Gesichtsmerkmale stellen eine Reihe von einzelnen Punkten auf einem Gesicht dar, in der Regel Punkte in Gesichtskomponenten wie u.a. Pupillen, Augenwinkel oder Nase. Gesichtszüge sind optionale Attribute, die bei der Gesichtserkennung analysiert werden können. Sie können „true“ beim Aufrufen von [Gesicht – Erkennen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) entweder als booleschen Wert an den Abfrageparameter „returnFaceLandmarks“ übergeben oder den optionalen Parameter „returnFaceLandmarks“ für die DetectAsync-Methode der FaceServiceClient-Klasse verwenden, um die Gesichtszüge in den Erkennungsergebnissen einzuschließen.

Standardmäßig sind 27 Bezugspunkte vordefiniert. Die folgende Abbildung zeigt alle 27 definierten Punkte:

![HowToDetectFace](../Images/landmarks.1.jpg)

Die Punkte werden in Pixel zurückgegeben genau wie beim rechteckigen Gesichtsrahmen. So ist es einfacher, bestimmte Bezugspunkte im Bild zu markieren. Der folgende Code zeigt die Positionen von Nase und Pupillen:

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

Neben der Markierung von Gesichtsmerkmalen in einem Bild können Gesichtszüge auch verwendet werden, um die Ausrichtung des Gesichts präzise zu berechnen. Beispielsweise kann die Ausrichtung des Gesichts als Vektor vom Mittelpunkt des Munds zum Mittelpunkt der Augen definiert werden. Im folgenden Code wird dies im Detail behandelt:

```CSharp
var landmarks = face.FaceLandmarks;
 
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

Durch Angabe der Ausrichtung des Gesichts können Sie den rechteckigen Gesichtsrahmen drehen, um es am Gesicht auszurichten. Die Verwendung von Gesichtszügen bietet daher zweifelsohne einen höheren Detailgrad und somit einen größeren Nutzen.

## <a name="step-4-using-other-face-attributes"></a>Schritt 4: Verwenden von anderen Gesichtsattributen

Neben Gesichtszügen kann die API „Gesicht – Erkennen“ auch verschiedene andere Attribute auf einem Gesicht analysieren. Zu diesen Attributen zählen Folgende:

- Alter
- Geschlecht
- Intensität des Lächelns
- Gesichtsbehaarung
- 3D-Kopfhaltung

Diese Attribute werden mithilfe statistischer Algorithmen vorhergesagt und sind möglicherweise nicht immer zu 100 % genau. Allerdings sind sie trotzdem hilfreich, wenn Sie Gesichter anhand dieser Attribute klassifizieren möchten. Weitere Informationen zu den einzelnen Attributen finden Sie im [Glossar](../Glossary.md).

Im Folgenden finden Sie ein einfaches Beispiel zum Extrahieren von Gesichtsattributen bei der Gesichtserkennung:

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden haben Sie die Funktionen der API [Gesicht – Erkennen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) kennengelernt, und erfahren, wie mit dieser Gesichter für lokale hochgeladene Bilder oder Bild-URLs im Internet erkannt, Gesichter durch Rückgabe rechteckiger Gesichtsrahmen erkannt und auch Gesichtszüge, die 3D-Kopfhaltung sowie andere Gesichtsattribute analysiert werden können.

Weitere Informationen zu API-Details finden Sie im Leitfaden zur API-Referenz für [Gesicht – Erkennen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Verwandte Themen

[Gesichtsidentifikation in Bildern](HowtoIdentifyFacesinImage.md)

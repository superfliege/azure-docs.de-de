---
title: 'Beispiel: Identifizieren von Gesichtern in Bildern – Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Gesichtserkennungs-API Gesichter in Bildern identifizieren.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 013467cb64220b525d429c901c48028bf65b3852
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864926"
---
# <a name="example-how-to-identify-faces-in-images"></a>Beispiel: Gesichtsidentifikation in Bildern

In diesem Leitfaden wird veranschaulicht, wie Sie mithilfe von PersonGroups, die im Voraus aus bekannten Personen erstellt werden, unbekannte Gesichter identifizieren. Die Beispiele sind in C# geschrieben und verwenden die Clientbibliothek für die Gesichtserkennungs-API.

## <a name="concepts"></a>Konzepte

Wenn Sie mit den folgenden Konzepten in diesem Leitfaden nicht vertraut sind, können Sie jederzeit im [Glossar](../Glossary.md) nach der entsprechenden Definition suchen:

- Face – Detect
- Face – Identify
- PersonGroup

## <a name="preparation"></a>Vorbereitung

In diesem Beispiel werden folgende Aktionen veranschaulicht:

- Erstellen einer PersonGroup: Die PersonGroup enthält eine Liste mit bekannten Personen.
- Zuweisen von Gesichtern zu einzelnen Personen: Diese Gesichter dienen als Grundlage zur Identifizierung von Personen. Es wird empfohlen, hierfür frontale Profilansichten zu verwenden, wie etwa bei Passbildern. Am besten sollten für eine Person mehrere Bilder mit unterschiedlichen Posen, Kleidungsfarben oder Frisuren vorliegen.

Damit Sie die Demonstration dieses Beispiels durchführen können, müssen Sie eine Reihe von Fotos vorbereiten:

- Mehrere Fotos, auf denen das Gesicht der Person abgebildet ist. [Klicken Sie hier, um Beispielfotos für Anna, Bill und Clare herunterzuladen.](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)
- Mehrere Testfotos, von denen auf manchen die Gesichter von Anna, Bill oder Clare abgebildet sind und auf manchen nicht. Damit soll die Identifizierung getestet werden. Unter dem vorherigen Link können Sie auch Beispielbilder auswählen.

## <a name="step-1-authorize-the-api-call"></a>Schritt 1: Autorisieren des API-Aufrufs

Für jeden Aufruf der Gesichtserkennungs-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel kann entweder über einen Abfragezeichenfolge-Parameter übergeben oder im Anforderungsheader angegeben werden. Wenn Sie den Abonnementschlüssel über eine Abfragezeichenfolge übergeben möchten, sehen Sie sich die Anforderungs-URL für [Gesicht – Erkennen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) als Beispiel an:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Alternativ können Sie den Abonnementschlüssel auch im HTTP-Anforderungsheader angeben: **ocp-apim-subscription-key: &lt;Abonnementschlüssel&gt;**. Wenn Sie eine Clientbibliothek verwenden, wird der Abonnementschlüssel durch den Konstruktor der FaceServiceClient-Klasse übergeben. Beispiel: 
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
Sie können den Abonnementschlüssel auf der Seite „Marketplace“ des Azure-Portals abrufen. Weitere Informationen finden Sie unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>Schritt 2: Erstellen von „PersonGroup“

In diesem Schritt erstellen Sie eine „PersonGroup“ mit dem Namen „MyFriends“, die drei Personen enthält: Anna, Bill und Clare. Für jede Person werden mehrere Gesichtsabbildungen registriert. Die Gesichter müssen auf den Bildern erkannt werden. Nachdem Sie alle Schritte ausgeführt haben, sieht Ihre PersonGroup etwa folgendermaßen aus:

![Identifizieren1](../Images/group.image.1.jpg)

### <a name="21-define-people-for-the-persongroup"></a>2.1 Definieren von Personen für PersonGroup
Eine Person ist die Basiseinheit für die Identifizierung. Für eine Person können eine oder mehrere bekannte Gesichtsabbildungen registriert werden. Eine PersonGroup stellt jedoch eine Sammlung von Personen dar, und jede Person wird in einer bestimmten PersonGroup definiert. Die Identifizierung erfolgt über die jeweilige PersonGroup. Daher muss zunächst eine PersonGroup erstellt werden und anschließend die zugehörigen Personen, z.B. Anna, Bill und Clare.

Erstellen Sie zunächst eine neue PersonGroup. Verwenden Sie hierzu die API [PersonGroup – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244). Die entsprechende API für die Clientbibliothek ist die Methode „CreatePersonGroupAsync“ für die Klasse „FaceServiceClient“. Die beim Erstellen der Gruppe angegebene Gruppen-ID ist für jedes Abonnement eindeutig. Sie können PersonGroups mit anderen PersonGroup-APIs auch abrufen, aktualisieren oder löschen. Nachdem die Gruppe definiert ist, können Sie mit der API [PersonGroup Person – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) Personen innerhalb der Gruppe definieren. Die Methode der Clientbibliothek lautet „CreatePersonAsync“. Nachdem Sie eine Person erstellt haben, können Sie ihr Gesichter hinzufügen.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 Erkennen von Gesichtern und Zuordnen zur richtigen Person
Senden Sie zur Gesichtserkennung eine POST-Webanforderung an die API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) mit der Bilddatei im Text der HTTP-Anforderung. Bei Verwendung einer Clientbibliothek wird die Gesichtserkennung über die Methode „DetectAsync“ für die Klasse „FaceServiceClient“ ausgeführt.

Rufen Sie für jedes erkannte Gesicht [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) auf, um es der richtigen Person hinzuzufügen.

Der folgende Code veranschaulicht, wie ein Gesicht auf einem Bild erkannt und einer Person hinzugefügt wird:
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Beachten Sie, dass bei mehreren Gesichtern auf einem Bild nur das größte hinzugefügt wird. Sie können der Person weitere Gesichter hinzufügen, indem Sie eine Zeichenfolge im Format „targetFace = links, oben, Breite, Höhe“ an den Abfrageparameter „targetFace“ der API [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) übergeben. Alternativ dazu können Sie auch den optionalen Parameter „targetFace“ für die Methode „AddPersonFaceAsync“ verwenden. Jedem hinzugefügten Gesicht wird eine eindeutige permanente Gesichts-ID zugeteilt, die Sie für [PersonGroup Person – Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) und [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) verwenden können.

## <a name="step-3-train-the-persongroup"></a>Schritt 3: Trainieren der „PersonGroup“

Eine PersonGroup muss vor ihrer Verwendung trainiert werden. Außerdem muss sie nach jedem Hinzufügen oder Entfernen einer Person oder nach der Bearbeitung eines registrierten Gesichts neu trainiert werden. Das Training erfolgt über die API [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249). Bei Verwendung der Clientbibliothek müssen Sie hierzu nur die Methode „TrainPersonGroupAsync“ aufrufen:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
Das Training ist ein asynchroner Prozess. Es ist durchaus möglich, dass es selbst nach Rückgabe der Methode „TrainPersonGroupAsync“ noch nicht abgeschlossen ist. Sie können den Trainingsstatus über die API [PersonGroup – Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) oder über die Methode „GetPersonGroupTrainingStatusAsync“ der Clientbibliothek abfragen. Das folgende Codebeispiel veranschaulicht eine einfache Logik des Wartens, bis das Training der PersonGroup abgeschlossen ist:
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Schritt 4: Identifizieren eines Gesichts anhand einer definierten „PersonGroup“

Bei der Identifizierung kann die Gesichtserkennungs-API die Ähnlichkeit eines Testgesichts unter allen Gesichtern einer Gruppe berechnen und die Person(en) mit der größten Ähnlichkeit zurückgeben. Dies erfolgt über die API [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) oder die Methode „IdentifyAsync“ der Clientbibliothek.

Das Testgesicht muss mithilfe der vorherigen Schritte erkannt werden. Anschließend wird die Gesichts-ID als zweites Argument an die „Identify“-API übergeben. Es können mehrere Gesichts-IDs gleichzeitig identifiziert werden, und alle Ergebnisse der Identifizierung werden ausgegeben. Als Standardeinstellung wird bei der Identifizierung nur die Person zurückgegeben, die dem Testgesicht am meisten ähnelt. Sie können aber auch den optionalen Parameter „MaxNumOfCandidatesReturned“ angeben, damit bei der Identifizierung mehr als ein Kandidat zurückgegeben wird.

Der folgende Code veranschaulicht den Identifizierungsprozess:

```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Nach Abschluss der Schritte können Sie versuchen, verschiedene Gesichter zu identifizieren. Achten Sie dabei darauf, ob die Gesichter von Anna, Bill oder Clare entsprechend der für die Gesichtserkennung hochgeladenen Bilder korrekt erkannt werden. Hierzu folgende Beispiele:

![Identifzieren2](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Schritt 5: Anfordern der Verarbeitung in großem Umfang

Eine PersonGroup kann aufgrund der Einschränkung des vorherigen Entwurfs bis zu 10.000 Personen enthalten.
Weitere Informationen zu Szenarios mit bis zu einer Million finden Sie unter [How to use the large-scale feature (Verwenden der Funktion für die Verarbeitung in großem Umfang)](how-to-use-large-scale.md).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden haben Sie erfahren, wie eine PersonGroup erstellt und eine Person identifiziert wird. Die folgenden Funktionen wurden dabei erklärt und vorgestellt:

- Erkennen von Gesichtern mithilfe der API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d)
- Erstellen von PersonGroups mithilfe der API [PersonGroup – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Erstellen von Personen mithilfe der API [PersonGroup Person – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Trainieren einer PersonGroup mithilfe der API [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
- Identifizieren von unbekannten Gesichtern über eine PersonGroup mithilfe der API [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)

## <a name="related-topics"></a>Verwandte Themen

- [How to Detect Faces in Image](HowtoDetectFacesinImage.md) (Gesichtserkennung in Bildern)
- [Hinzufügen von Gesichtern](how-to-add-faces.md)
- [Verwenden der Funktion für die Verarbeitung in großem Umfang](how-to-use-large-scale.md)

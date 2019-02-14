---
title: Migrieren Ihrer Gesichtserkennungsdaten zwischen Abonnements – Gesichtserkennungs-API
titleSuffix: Azure Cognitive Services
description: Dieser Leitfaden zeigt auf, wie Sie Ihre gespeicherten Gesichtserkennungsdaten von einem Gesichtserkennungs-API-Abonnement in ein anderes migrieren können.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 5eb198ecf76556e632c5f42bc22362b2f20f8916
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55771530"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Migrieren Ihrer Gesichtserkennungsdaten in ein anderes Abonnement für die Gesichtserkennung

In diesem Leitfaden erfahren Sie, wie Sie Gesichtserkennungsdaten (z.B. ein gespeichertes **PersonGroup**-Objekt mit Gesichtern) anhand der Momentaufnahmefunktion in ein anderes Gesichtserkennungs-API-Abonnement verschieben können. Auf diese Weise können Sie vermeiden, dass Sie bei einer Verlagerung oder Erweiterung Ihrer Betriebsabläufe **PersonGroup**- oder **FaceList**-Objekte wiederholt erstellen und trainieren müssen. Dies ist beispielsweise in folgenden Fällen nützlich: Sie haben unter Verwendung eines kostenlosen Testabonnements ein **PersonGroup**-Objekt erstellt und möchten dieses zu Ihrem kostenpflichtigen Abonnement migrieren, oder Sie müssen Gesichtserkennungsdaten für ein Großunternehmen regionsübergreifend synchronisieren.

Dieselbe Migrationsstrategie gilt auch für **LargePersonGroup**- und **LargeFaceList**-Objekte. Wenn Sie mit den folgenden Konzepten in diesem Leitfaden nicht vertraut sind, finden Sie die Definitionen im [Glossar](../Glossary.md). In diesem Leitfaden wird die .NET-Clientbibliothek für die Gesichtserkennungs-API mit C# verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Zwei Abonnementschlüssel für die Gesichtserkennungs-API (ein Abonnement mit vorhandenen Daten, ein Abonnement als Migrationsziel). Folgen Sie den Anweisungen unter [Erstellen eines Cognitive Services-Kontos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel abzurufen.
- Die Abonnement-ID der Gesichtserkennungs-API für das Zielabonnement (diese finden Sie im Blatt **Übersicht** im Azure-Portal). 
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).


## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

In diesem Leitfaden wird die Migration der Daten zur Gesichtserkennung anhand einer einfachen Konsolen-App durchgeführt. Eine vollständige Implementierung finden Sie im [Momentaufnahmebeispiel für die Gesichtserkennungs-API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) auf GitHub.

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**, und nennen Sie es **FaceApiSnapshotSample**. 
1. Rufen Sie die erforderlichen NuGet-Pakete ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten...** aus. Klicken Sie auf die Registerkarte **Durchsuchen**, und wählen Sie **Vorabversion einbeziehen** aus. Suchen Sie anschließend das folgende Paket, und installieren Sie es:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)


## <a name="create-face-clients"></a>Erstellen von Clients für die Gesichtserkennung

Erstellen Sie in der **Main**-Methode in *Program.cs* zwei **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)**-Instanzen für Ihre Quell- und Zielabonnements. In diesem Beispiel verwenden wir als Quelle ein Abonnement für die Gesichtserkennung in der Region „Asien, Osten“ und als Ziel ein Abonnement in der Region „USA, Westen“. Auf diese Weise wird veranschaulicht, wie Daten von einer Azure-Region zu einer anderen migriert werden. Wenn sich Ihre Abonnements in unterschiedlichen Regionen befinden, müssen Sie die `Endpoint`-Zeichenfolgen ändern.

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Sie müssen die Abonnementschlüsselwerte und die Endpunkt-URLs für Ihre Quell- und Zielabonnements angeben.


## <a name="prepare-a-persongroup-for-migration"></a>Vorbereiten eines PersonGroup-Objekts für die Migration

Sie benötigen die ID des **PersonGroup**-Objekts in Ihrem Quellabonnement, das zum Zielabonnement migriert werden soll. Verwenden Sie die **[PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet)**-Methode, um eine Liste Ihrer **PersonGroup**-Objekte abzurufen, und rufen Sie anschließend die **[PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId)**-Eigenschaft ab. Je nachdem, über welche **PersonGroup**-Objekte Sie verfügen, sieht dieser Prozess unterschiedlich aus. In diesem Leitfaden ist die **PersonGroup**-Quell-ID in `personGroupId` gespeichert.

> [!NOTE]
> Im [Beispielcode](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) wird ein neues **PersonGroup**-Objekt für die Migration erstellt und trainiert, aber in den meisten Fällen sollten Sie bereits über ein **PersonGroup**-Objekt verfügen.

## <a name="take-snapshot-of-persongroup"></a>Erstellen einer PersonGroup-Momentaufnahme

Eine Momentaufnahme ist ein temporärer Remotespeicher für bestimmte Arten von Gesichtserkennungsdaten. Die Momentaufnahme fungiert als eine Art Zwischenablage, um Daten von einem Abonnement in ein anderes zu kopieren. Zuerst erstellt der Benutzer eine Momentaufnahme der Daten im Quellabonnement, anschließend wird die Momentaufnahme auf ein neues Datenobjekt im Zielabonnement angewendet.

Verwenden Sie die **FaceClient**-Instanz im Quellabonnement, um eine Momentaufnahme des **PersonGroup**-Objekts zu erstellen. Verwenden Sie hierbei **[TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet)** mit der **PersonGroup**-ID und der ID des Zielabonnements. Wenn Sie über mehrere Zielabonnements verfügen, können Sie diese als Arrayeinträge im dritten Parameter hinzufügen.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Durch das Erstellen und Anwenden von Momentaufnahmen werden reguläre Aufrufe von **PersonGroup** (oder **FaceList**) in Quelle oder Ziel nicht unterbrochen. Es wird jedoch nicht empfohlen, gleichzeitig Aufrufe zur Quellobjektänderung durchzuführen (z.B. [Aufrufe zum Verwalten von Gesichtserkennungslisten](https://docs.microsoft.com/rest/api/cognitiveservices/face/facelist) oder Aufrufe zum [Trainieren von Personengruppen](https://docs.microsoft.com/rest/api/cognitiveservices/face/persongroup/train)), da der Momentaufnahmevorgang vor oder nach diesen Vorgängen ausgeführt werden oder zu Fehlern führen kann. 

## <a name="retrieve-the-snapshot-id"></a>Abrufen der Momentaufnahme-ID

Die Methode zur Momentaufnahmeerstellung ist asynchron, deshalb müssen Sie auf ihren Abschluss warten (Momentaufnahmevorgänge können nicht abgebrochen werden). In diesem Code überwacht die `WaitForOperation`-Methode den asynchronen Aufruf, der Status wird alle 100 ms überprüft. Nach Abschluss des Vorgangs können Sie eine Vorgangs-ID abrufen. Der Abruf kann durch Analyse des Felds `OperationLocation` erfolgen. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Ein typischer `OperationLocation`-Wert sieht wie folgt aus:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

Die `WaitForOperation`-Hilfsmethode befindet sich hier:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Wenn der Vorgangsstatus als `Succeeded` markiert ist, können Sie die Momentaufnahme-ID abrufen, indem Sie das Feld `ResourceLocation` der zurückgegebenen Instanz **OperationStatus** analysieren.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Ein typischer `resourceLocation`-Wert sieht wie folgt aus:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-snapshot-to-target-subscription"></a>Anwenden der Momentaufnahme auf das Zielabonnement

Als Nächstes erstellen Sie das neue **PersonGroup**-Objekt im Zielabonnement. Hierbei verwenden Sie eine zufällig generierte ID. Anschließend verwenden Sie die **FaceClient**-Instanz des Zielabonnements, um die Momentaufnahme auf dieses PersonGroup-Objekt anzuwenden, indem Sie die Momentaufnahme-ID und die ID des neuen **PersonGroup**-Objekts übergeben. 

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Ein Momentaufnahmeobjekt ist nur für 48 Stunden gültig. Sie sollten nur dann eine Momentaufnahme erstellen, wenn Sie kurz darauf eine Datenmigration durchführen möchten.

Bei der Anforderung zur Momentaufnahmeanwendung wird eine weitere Vorgangs-ID zurückgegeben. Sie können diese ID abrufen, indem Sie das Feld `OperationLocation` der zurückgegebenen **applySnapshotResult**-Instanz analysieren. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Der Prozess zur Momentaufnahmeanwendung ist ebenfalls synchron. Verwenden Sie deshalb erneut `WaitForOperation`, um auf den Abschluss des Vorgangs zu warten.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Testen der Datenmigration

Nachdem Sie die Momentaufnahme angewendet haben, sollte das neue **PersonGroup**-Objekt im Zielabonnement mit den ursprünglichen Gesichtserkennungsdaten aufgefüllt werden. Standardmäßig werden auch Trainingsergebnisse kopiert, deshalb steht das neue **PersonGroup**-Objekt für Aufrufe zur Gesichtserkennung zur Verfügung, ohne erneut trainiert werden zu müssen.

Um die Datenmigration zu testen, können Sie die folgenden Vorgänge ausführen und die an die Konsole ausgegebenen Ergebnisse vergleichen.

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Verwenden Sie die folgenden Hilfsmethoden:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

Jetzt können Sie das neue **PersonGroup**-Objekt im Zielabonnement verwenden. 

Wenn Sie das **PersonGroup**-Zielobjekt später aktualisieren möchten, müssen Sie (mithilfe der Schritte in diesem Leitfaden) ein neues **PersonGroup**-Objekt erstellen, um die Momentaufnahme zu empfangen. Auf ein einzelnes **PersonGroup**-Objekt kann pro Arbeitsschritt nur eine Momentaufnahme angewendet werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nachdem Sie die Gesichtserkennungsdaten migriert haben, wird empfohlen, das Momentaufnahmeobjekt manuell zu löschen.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="related-topics"></a>Verwandte Themen

- [Referenzdokumentation zu Momentaufnahmen (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Momentaufnahmebeispiel für die Gesichtserkennungs-API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Hinzufügen von Gesichtern](how-to-add-faces.md)
- [Gesichtserkennung in Bildern](HowtoDetectFacesinImage.md)
- [Gesichtsidentifikation in Bildern](HowtoIdentifyFacesinImage.md)

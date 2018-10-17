---
title: 'Beispiel: Verwenden der Funktion für die Verarbeitung in großem Umfang – Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Funktion für die Verarbeitung in großem Umfang in der Gesichtserkennungs-API verwenden.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: e8bbf78da84ddb77ce956e37f91be46e96144991
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123078"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Beispiel: Verwenden der Funktion für die Verarbeitung in großem Umfang

Dieses Handbuch ist ein erweiterter Artikel zur Codemigration zum Hochskalieren von den herkömmlichen Funktionen PersonGroup und FaceList zu LargePersonGroup bzw. LargeFaceList.
Dieses Handbuch veranschaulicht die Migration unter der Voraussetzung, dass die grundlegende Verwendung von PersonGroup und FaceList bekannt ist.
Um mit den Standardvorgängen vertraut zu werden, lesen Sie bitte andere Tutorials wie z.B. [Identifizieren von Gesichtern in Bildern](HowtoIdentifyFacesinImage.md).

Für die Gesichtserkennungs-API wurden vor Kurzem die Funktionen LargePersonGroup und LargeFaceList für umfangreiche Szenarien veröffentlicht, die zusammen als „Verarbeitung in großem Umfang“ bezeichnet werden.
LargePersonGroup kann bis zu 1.000.000 Personen mit maximal 248 Gesichtern enthalten und LargeFaceList bis zu 1.000.000 Gesichter.

Die umfangreichen Vorgänge ähneln den herkömmlichen Funktionen PersonGroup und FaceList, weisen jedoch aufgrund der neuen Architektur einige wichtige Unterschiede auf.
Dieses Handbuch veranschaulicht die Migration unter der Voraussetzung, dass die grundlegende Verwendung von PersonGroup und FaceList bekannt ist.
Die Beispiele sind in C# geschrieben und verwenden die Clientbibliothek für die Gesichtserkennungs-API.

Um die Gesichtssuchleistung für Identification und FindSimilar in großem Maßstab zu aktivieren, müssen Sie zur Vorabverarbeitung für LargeFaceList und LargePersonGroup ein Training durchführen.
Die Trainingszeit variiert abhängig von der tatsächlichen Kapazität von Sekunden bis zu rund einer halben Stunde.
Während des Trainingszeitraums können weiterhin Identification und FindSimilar ausgeführt werden, wenn zuvor ein erfolgreiches Training durchgeführt wurde.
Der Nachteil ist jedoch, dass die neu hinzugefügten Personen/Gesichter nicht im Ergebnis vorkommen, bis eine neue Nachmigration zu umfangreichem Training abgeschlossen ist.

## <a name="concepts"></a>Konzepte

Wenn Sie mit den folgenden Konzepten in diesem Handbuch nicht vertraut sind, finden Sie die Definitionen im [Glossar](../Glossary.md):

- LargePersonGroup: Eine Sammlung von Personen mit einer Kapazität bis zu 1.000.000.
- LargeFaceList: Eine Sammlung von Gesichtern mit einer Kapazität bis zu 1.000.000.
- Train: Eine Vorabverarbeitung, um die Leistung von Identification/FindSimilar sicherzustellen.
- Identification: Identifizieren eines oder mehrerer Gesichter in einer PersonGroup oder LargePersonGroup.
- FindSimilar: Suchen ähnlicher Gesichter in einer FaceList oder LargeFaceList.

## <a name="step-1-authorize-the-api-call"></a>Schritt 1: Autorisieren des API-Aufrufs

Wenn Sie eine Gesichtserkennungs-API-Clientbibliothek verwenden, werden der Abonnementschlüssel und der Subskriptionsendpunkt durch den Konstruktor der Klasse FaceServiceClient übergeben. Beispiel: 

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

Sie können den Abonnementschlüssel mit dem entsprechenden Endpunkt auf der Seite Marketplace des Azure-Portals abrufen.
Weitere Informationen finden Sie unter [Abonnements](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration-in-action"></a>Schritt 2: Migrieren von Code

Dieser Abschnitt konzentriert sich nur auf die Migration der PersonGroup/FaceList-Implementierung zu LargePersonGroup/LargeFaceList.
Obwohl LargePersonGroup/LargeFaceList sich in Entwurf und interner Implementierung von PersonGroup/FaceList unterscheiden, ähneln sich die API-Schnittstellen im Interesse der Abwärtskompatibilität.

Migration von Daten wird nicht unterstützt – Sie müssen stattdessen LargePersonGroup/LargeFaceList neu erstellen.

## <a name="step-21-migrate-persongroup-to-largepersongroup"></a>Schritt 2.1: Migrieren von PersonGroup zu LargePersonGroup

Die Migration von PersonGroup zu LargePersonGroup ist reibungslos, weil ihre Vorgänge auf Gruppenebene identisch sind.

Bei der auf PersonGroup/Person bezogenen Implementierung ist nur zu beachten, die API-Pfade bzw- die SDK-Klasse/das SDK-Modul zu LargePersonGroup und LargePersonGroup Person zu ändern.

Eine Referenz zur Datenmigration finden Sie unter [Hinzufügen von Gesichtern](how-to-add-faces.md).

## <a name="step-22-migrate-facelist-to-largefacelist"></a>Schritt 2.2: Migrieren von FaceList zu LargeFaceList

| FaceList-APIs | LargeFaceList-APIs |
|:---:|:---:|
| Erstellen | Erstellen |
| Löschen | Löschen |
| Get | Get |
| Auflisten | Auflisten |
| Aktualisieren | Aktualisieren |
| - | Trainieren |
| - | Trainingsstatus abrufen |

Die vorstehende Tabelle ist ein Vergleich der Vorgänge von FaceList und LargeFaceList auf Listenebene.
Wie gezeigt, weist LargeFaceList im Vergleich zu FaceList die neuen Vorgänge Trainieren und „Trainingsstatus abrufen“ auf.
Das Abrufen von LargeFaceList wird als Vorbedingung des Vorgangs [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) trainiert, während für FaceList kein Trainieren erforderlich ist.
Der folgende Codeausschnitt zeigt eine Hilfsfunktion zum Warten auf das Training einer LargeFaceList.

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Bisher würde eine typische Verwendung von FaceList mit Hinzufügen von Gesichtern und FindSimilar so aussehen:

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

Beim Migrieren zu LargeFaceList würde daraus dieses:

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Wie oben gezeigt, sind Datenverwaltungs- und FindSimilar-Teil fast identisch.
Die einzige Ausnahme ist, dass ein neues Training zur Vorabverarbeitung in LargeFaceList abgeschlossen werden muss, damit FindSimilar funktioniert.

## <a name="step-3-train-suggestions"></a>Schritt 3: Vorschläge zum Trainieren

Obwohl das Training[FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) und [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) beschleunigt, leidet die Trainingszeit insbesondere bei umfangreicher Verarbeitung.
Die geschätzte Trainingszeit in verschiedenen Maßstäben ist in der folgenden Tabelle aufgeführt:

| Skalierung (Gesichter oder Personen) | Geschätzte Trainingszeit |
|:---:|:---:|
| 1.000 | 1-2 S |
| 10.000 | 5-10 S |
| 100.000 | 1-2 Min |
| 1.000.000 | 10-30 Min |

Um die Funktion für die Verarbeitung in großem Umfang besser zu nutzen, sollten Sie ein paar Strategien befolgen.

## <a name="step-31-customize-time-interval"></a>Schritt 3.1: Anpassen des Zeitintervalls

Wie in `TrainLargeFaceList()` gezeigt, verzögert ein `timeIntervalInMilliseconds` den unendlichen Trainingsstatus-Prüfvorgang.
Bei LargeFaceList mit mehr Gesichtern reduziert ein längeres Intervall die Anzahl der Aufrufe und die Kosten.
Das Zeitintervall sollte der erwarteten Kapazität von LargeFaceList angepasst werden.

Die gleiche Strategie gilt auch für LargePersonGroup.
Beispielsweise könnte `timeIntervalInMilliseconds` beim Training einer LargePersonGroup mit 1.000.000 Personen 60.000 betragen (auch 1-Minuten-Intervall genannt).

## <a name="step-32-small-scale-buffer"></a>Schritt 3.2: Kleiner Puffer

Personen/Gesichter können in LargePersonGroup/LargeFaceList erst nach dem Trainieren gesucht werden.
In einem dynamischen Szenario werden ständig neue Personen/Gesichter hinzugefügt und müssen sofort suchbar sein, doch das Training könnte länger dauern als gewünscht.
Um dieses Problem zu minimieren, können Sie eine sehr kleine LargePersonGroup/LargeFaceList nur als Puffer für die neu hinzugefügten Einträge verwenden.
Dieser Puffer benötigt aufgrund seiner wesentlich geringeren Größe weniger Zeit, und die sofortige Suche in diesem temporären Puffer sollte funktionieren.
Verwenden Sie diesen Puffer in Kombination mit dem Training für Haupt-LargePersonGroup/-LargeFaceList, indem Sie das Haupttraining in einem längeren Intervall ausführen, z.B. täglich um Mitternacht.

Ein Beispielworkflow:
1. Erstellen Sie eine Haupt-LargePersonGroup/-LargeFaceList (Hauptsammlung) und eine Puffer-LargePersonGroup/-LargeFaceList (Puffersammlung). Die Puffersammlung gilt nur für neu hinzugefügte Personen/Gesichter.
1. Fügen Sie neue Personen/Gesichter sowohl der Haupt- als auch der Puffersammlung hinzu.
1. Trainieren Sie nur die Puffersammlung mit einem kurzen Zeitintervall, damit die neu hinzugefügten Einträge wirksam werden.
1. Rufen Sie Identification/FindSimilar sowohl für die Haupt- als auch die Puffersammlung aus, und führen Sie die Ergebnisse zusammen.
1. Wenn die Größe der Puffersammlung einen Schwellenwert erreicht, oder während einer Leerlaufzeit des Systems, erstellen Sie eine neue Puffersammlung, und lösen Sie das Training der Hauptsammlung aus.
1. Löschen Sie nach dem Ende des Trainings der Hauptsammlung die alte Puffersammlung.

## <a name="step-33-standalone-training"></a>Schritt 3.3: Eigenständiges Trainieren

Wenn eine relativ lange Latenz akzeptabel ist, muss das Training nicht direkt nach dem Hinzufügen neuer Daten ausgelöst werden.
Stattdessen kann das Training von der Hauptlogik getrennt und regelmäßig ausgelöst werden.
Diese Strategie eignet sich für dynamische Szenarien mit akzeptabler Latenz und kann auf statische Szenarien angewendet werden, um die Häufigkeit des Trainings weiter zu verringern.

Angenommen, eine `TrainLargePersonGroup`-Funktion ähnelt `TrainLargeFaceList`.
Eine typische Implementierung des eigenständigen Trainings für LargePersonGroup durch Aufrufen der [`Timer`](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx)
-Klasse in `System.Timers` wäre:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Weitere Informationen zu Datenverwaltung und identifikationsbezogenen Implementierungen finden Sie unter [Hinzufügen von Gesichtern](how-to-add-faces.md) und [Identifizieren von Gesichtern in Bildern](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch haben Sie gelernt, vorhandenen PersonGroup-/FaceList-Code (nicht Daten) zu LargePersonGroup/LargeFaceList zu migrieren:

- LargePersonGroup und LargeFaceList funktionieren ähnlich wie PersonGroup/FaceList, abgesehen davon, dass bei LargeFaceList ein Training erforderlich ist.
- Wählen Sie die richtige Trainingsstrategie zur dynamischen Datenaktualisierung für umfangreiche Datasets.

## <a name="related-topics"></a>Verwandte Themen

- [Gesichtsidentifikation in Bildern](HowtoIdentifyFacesinImage.md)
- [Hinzufügen von Gesichtern](how-to-add-faces.md)

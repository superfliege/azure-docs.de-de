---
title: Hinzufügen von Gesichtern mit der Gesichtserkennungs-API| Microsoft-Dokumentation
titleSuffix: Microsoft Cognitive Services
description: Verwenden Sie die Gesichtserkennungs-API in Cognitive Services, um Gesichter in Bildern hinzuzufügen.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3306c13d6c3d231ddbda38cfcbc5419fcdbd30db
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372994"
---
# <a name="how-to-add-faces"></a>Hinzufügen von Gesichtern

Dieser Leitfaden veranschaulicht die bewährte Methode, um einer PersonGroup eine große Anzahl von Personen und Gesichtern hinzuzufügen.
Die gleiche Strategie gilt auch für FaceList und LargePersonGroup.
Die Beispiele sind in C# geschrieben und verwenden die Clientbibliothek für die Gesichtserkennungs-API.

## <a name="step1"></a> Schritt 1: Initialisierung

Es sind mehrere Variablen deklariert und eine Hilfsfunktion wird implementiert, um die Anforderungen zu planen.

- `PersonCount` ist die Gesamtzahl an Personen.
- `CallLimitPerSecond` ist die maximale Anzahl an Aufrufen pro Sekunde gemäß der Abonnementebene.
- `_timeStampQueue` ist eine Warteschlange zum Aufzeichnen der Zeitstempel der Anforderung.
- `await WaitCallLimitPerSecondAsync()` wartet, bis die nächste Anforderung gesendet werden kann.

```CSharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step2"></a> Schritt 2: Autorisieren des API-Aufrufs

Wenn Sie eine Clientbibliothek verwenden, wird der Abonnementschlüssel durch den Konstruktor der Klasse FaceServiceClient übergeben. Beispiel: 

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

Sie können den Abonnementschlüssel auf der Seite „Marketplace“ des Azure-Portals abrufen. Weitere Informationen finden Sie unter [Abonnements](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step3"></a> Schritt 3: Erstellen der PersonGroup

Eine PersonGroup mit dem Namen „MyPersonGroup“ wird erstellt, um Personen zu speichern.
Die Anforderungszeit wird in `_timeStampQueue` in die Warteschlange eingereiht, um die gesamte Validierung sicherzustellen.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step4"></a> Schritt 4: Erstellen der Personen für PersonGroup

Personen werden gleichzeitig erstellt und `await WaitCallLimitPerSecondAsync()` wird ebenfalls angewendet, um zu vermeiden, dass die Aufrufbeschränkung überschritten wird.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step5"></a> Schritt 5: Hinzufügen von Gesichtern zu den Personen

Das Hinzufügen von Gesichtern zu verschiedenen Personen wird gleichzeitig verarbeitet, während es für eine bestimmte Person sequentiell ist.
`await WaitCallLimitPerSecondAsync()` wird erneut aufgerufen, um sicherzustellen, dass sich die Häufigkeit der Anforderungen innerhalb der Beschränkungen bewegt.

```CSharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a> Zusammenfassung

In diesem Leitfaden haben Sie erfahren, wie eine PersonGroup mit einer großen Anzahl an Personen und Gesichtern erstellt wird. Einige Hinweise:

- Diese Strategie gilt auch für FaceList und LargePersonGroup.
- Das Hinzufügen/Löschen von Gesichtern für andere FaceLists oder Personen in LargePersonGroup kann gleichzeitig verarbeitet werden.
- Derselbe Vorgang für eine bestimmte FaceList oder Person in der LargePersonGroup sollten sequentiell durchgeführt werden.
- Der Einfachheit halber wird in diesem Leitfaden auf die Behandlung möglicher Ausnahmen verzichtet. Wenn Sie mehr Stabilität benötigen, sollten Sie die richtige Wiederholungsrichtlinie anwenden.

Im Folgenden finden Sie eine kurze Erinnerung an die zuvor erläuterten und demonstrierten Funktionen:

- Erstellen von PersonGroups mithilfe der API [PersonGroup – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Erstellen von Personen mithilfe der API [PersonGroup Person – Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Hinzufügen von Gesichtern zu Personen mithilfe der API [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)

## <a name="related"></a> Verwandte Themen
- [Informationen zur Gesichtsidentifikation in Bildern](HowtoIdentifyFacesinImage.md)
- [Informationen zur Gesichtserkennung in Bildern](HowtoDetectFacesinImage.md)
- [Verwenden der Funktion für die Verarbeitung in großem Umfang](how-to-use-large-scale.md)

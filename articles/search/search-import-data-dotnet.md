---
title: Laden von Daten in einem Azure Search-Index in C# (.NET SDK) – Azure Search
description: Erfahren Sie, wie Sie mit C#-Beispielcode und dem .NET SDK Daten in einen durchsuchbaren Volltextindex in Azure Search hochladen.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: d2d54d1425bbb67a3f5ba1b6081a9f74ff87f4d6
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286906"
---
# <a name="quickstart-2---load-data-to-an-azure-search-index-using-c"></a>Schnellstart: 2 – Laden von Daten in einen Azure Search-Index mit C#

In diesem Artikel wird beschrieben, wie Sie mit C# und dem [.NET SDK](https://aka.ms/search-sdk) Daten in einen [Azure Search-Index](search-what-is-an-index.md) importieren. Um Dokumente in Ihren Index zu pushen, müssen folgende Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen Sie ein [`SearchIndexClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet)-Objekt, um eine Verbindung mit dem Suchindex herzustellen.
> * Erstellen Sie ein [`IndexBatch`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet)-Objekt mit den Dokumenten, die hinzugefügt, geändert oder gelöscht werden sollen.
> * Rufen Sie die `Documents.Index`-Methode auf dem `SearchIndexClient` auf, um Dokumente zu einem Index hochzuladen.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen eines Azure Search-Indexes](search-create-index-dotnet.md) und eines `SearchServiceClient`-Objekts, wie in [Erstellen eines Clients](search-create-index-dotnet.md#CreateSearchServiceClient) gezeigt.


## <a name="create-a-client"></a>Erstellen eines Clients
Zum Importieren von Daten benötigen Sie eine Instanz der `SearchIndexClient`-Klasse. Es gibt verschiedene Ansätze zum Erstellen dieser Klasse, einschließlich der Verwendung der bereits erstellten `SearchServiceClient`-Instanz. 

Wie das folgende Beispiel veranschaulicht, können Sie die `SearchServiceClient`-Instanz verwenden und ihre `Indexes.GetClient`-Methode aufrufen. Dieser Codeausschnitt ruft einen `SearchIndexClient` für den Index mit dem Namen „hotels“ von einem `SearchServiceClient` mit dem Namen `serviceClient` ab.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

`SearchIndexClient` verfügt über eine `Documents`-Eigenschaft. Mit dieser Eigenschaft werden alle Methoden bereitgestellt, die Sie benötigen, um Dokumente im Index hinzuzufügen, zu ändern, zu löschen oder abzufragen.

> [!NOTE]
> In einer typischen Suchanwendung werden Abfragen und Indizieren separat behandelt. Während `Indexes.GetClient` praktisch ist, da Sie Objekte wie `SearchCredentials` wiederverwenden können, umfasst ein stabilerer Ansatz das direkte Erstellen des `SearchIndexClient`, damit Sie einen Abfrageschlüssel statt eines Administratorschlüssels übergeben können. Diese Praxis entspricht dem [Prinzip der geringsten Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) und trägt dazu bei, die Anwendung sicherer zu machen. In der nächsten Übung erstellen Sie einen `SearchIndexClient`. Weitere Informationen zu Schlüsseln finden Sie unter [Erstellen und Verwalten von API-Schlüsseln für einen Azure Search-Dienst](search-security-api-keys.md).
> 
> 

<a name="construct-indexbatch"></a>

## <a name="construct-indexbatch"></a>Erstellen des IndexBatch-Elements

Zum Importieren von Daten mit dem .NET SDK verpacken Sie Ihre Daten in einem `IndexBatch`-Objekt. Ein `IndexBatch`-Element kapselt eine Sammlung mit `IndexAction`-Objekten. Jedes Objekt enthält ein Dokument und eine Eigenschaft, die Azure Search mitteilt, welche Aktion für das jeweilige Dokument durchgeführt werden soll (Hochladen, Zusammenführen, Löschen und mergeOrUpload). Weitere Informationen zu Indizierungen finden Sie unter [Indizierungsaktionen: Hochladen, Zusammenführen, MergeOrUpload, Löschen](search-what-is-data-import.md#indexing-actions).

Nachdem Sie nun wissen, welche Aktionen für die Dokumente durchgeführt werden, können Sie das `IndexBatch`-Element erstellen. Im folgenden Beispiel wird veranschaulicht, wie Sie einen Batch mit einigen anderen Aktionen erstellen. Im Beispiel wird eine benutzerdefinierte Klasse mit dem Namen `Hotel` verwendet, die einem Dokument im Index „hotels“ zugeordnet ist.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In diesem Fall verwenden wir `Upload`, `MergeOrUpload` und `Delete` als Suchaktionen. Diese werden von den Methoden angegeben, die für die `IndexAction`-Klasse aufgerufen werden.

Wir gehen davon aus, dass der Index in diesem Beispiel („hotels“) bereits mit einigen Dokumenten gefüllt ist. Beachten Sie, dass bei `MergeOrUpload` nicht alle möglichen Dokumentfelder festgelegt werden mussten und bei `Delete` nur der Dokumentschlüssel (`HotelId`) definiert werden musste.

Beachten Sie außerdem, dass in einer Indizierungsanforderung nur bis zu 1.000 Dokumente enthalten sein können.

> [!NOTE]
> In diesem Beispiel wenden wir unterschiedliche Aktionen auf unterschiedliche Dokumente an. Falls Sie für alle Dokumente im Batch die gleichen Aktionen durchführen möchten, können Sie die anderen statischen Methoden von `IndexBatch` verwenden, anstatt `IndexBatch.New` aufzurufen. Beispielsweise können Sie Batches erstellen, indem Sie `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` oder `IndexBatch.Delete` aufrufen. Für diese Methoden wird anstelle von `IndexAction`-Objekten eine Sammlung mit Dokumenten verwendet (in diesem Beispiel Objekte vom Typ `Hotel`).
> 
> 

## <a name="call-documentsindex"></a>Aufrufen von „Documents.Index“
Nachdem Sie nun über ein initialisiertes `IndexBatch`-Objekt verfügen, können Sie es an den Index senden, indem Sie `Documents.Index` für das `SearchIndexClient`-Objekt aufrufen. Das folgende Beispiel zeigt, wie Sie `Index`aufrufen, sowie einige zusätzliche Schritte, die Sie ausführen müssen:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Beachten Sie das Element `try`/`catch`, von dem der Aufruf der `Index`-Methode umgeben ist. Mit dem Catch-Block wird ein wichtiger Fehlerfall im Zusammenhang mit der Indizierung behandelt. Falls der Azure Search-Dienst Dokumente des Batch nicht indizieren kann, löst `Documents.Index` eine `IndexBatchException` aus. Dies kann bei der Indizierung von Dokumenten geschehen, wenn der Dienst stark ausgelastet ist. **Es wird dringend empfohlen, diesen Fall in Ihrem Code explizit zu behandeln.** Zum Beispiel kann die Indizierung der zuvor nicht indizierten Dokumente nach einer Weile wieder aufgenommen werden oder der Vorgang kann, wie im Beispiel gezeigt, nach der Aufzeichnung des Fehlers fortgeführt werden. Je nach Datenkonsistenzanforderungen Ihrer Anwendung sind aber auch andere Lösungen möglich.

Schließlich wird der Code im obigen Beispiel um zwei Sekunden verzögert. Da die Indizierung in Ihrem Azure Search-Dienst asynchron erfolgt, muss die Beispielanwendung einen Augenblick warten, damit sichergestellt ist, dass die Dokumente für Suchen zur Verfügung stehen. Verzögerungen wie diese sind in der Regel nur in Demos, Tests und Beispielanwendungen erforderlich.

Weitere Informationen zur Verarbeitung von Dokumenten finden Sie unter [Behandeln von Dokumenten durch das .NET SDK](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).


## <a name="next-steps"></a>Nächste Schritte
Nach dem Auffüllen des Azure Search-Indexes werden im nächsten Schritt Abfragen für die Suche nach Dokumenten ausgeführt. 

> [!div class="nextstepaction"]
> [Abfragen eines Azure Search-Indexes in C#](search-query-dotnet.md)

---
title: Abfragen von Daten in einem Azure Search-Index in C# (.NET SDK) – Azure Search
description: C#-Codebeispiel für die Erstellung einer Suchabfrage in Azure Search. Fügen Sie Suchparameter zum Filtern und Sortieren von Suchergebnissen hinzu.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: 1194407122123797c2564c96ac452b9582b017a4
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264946"
---
# <a name="quickstart-3---query-an-azure-search-index-in-c"></a>Schnellstart: 3 – Abfragen eines Azure Search-Indexes in C#

In diesem Artikel wird beschrieben, wie Sie mit C# und dem [.NET SDK](https://aka.ms/search-sdk) einen [Azure Search-Index](search-what-is-an-index.md) abfragen. Um Dokumente in Ihrem Index zu suchen, müssen folgende Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen Sie ein [`SearchIndexClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet)-Objekt, um eine Verbindung mit einem Suchindex mit schreibgeschütztem Zugriff herzustellen.
> * Erstellen Sie ein [`SearchParameters`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet)-Objekt, das die Such- oder Filterdefinition enthält.
> * Rufen Sie die `Documents.Search`-Methode in `SearchIndexClient` auf, um Abfragen an einen Index zu senden.

## <a name="prerequisites"></a>Voraussetzungen

[Laden Sie einen Azure Search-Index](search-import-data-dotnet.md) mit den Beispieldaten für „hotels“.

Rufen Sie einen Abfrageschlüssel ab, der für den schreibgeschützten Zugriff auf Dokumente verwendet wird. Bisher haben Sie einen API-Administratorschlüssel verwendet, sodass Sie Objekte und Inhalte in einem Suchdienst erstellen konnten. Für die Unterstützung von Abfragen in Apps empfehlen wir jedoch ausdrücklich die Verwendung eines Abfrageschlüssels. Eine Anleitung dazu finden Sie unter [Erstellen eines Abfrageschlüssels](search-security-api-keys.md#create-query-keys).

## <a name="create-a-client"></a>Erstellen eines Clients
Erstellen Sie eine Instanz der `SearchIndexClient`-Klasse, damit Sie dieser einen Abfrageschlüssel für den schreibgeschützten Zugriff übergeben können (im Gegensatz zu den Schreibzugriffsrechten, die auf den in der vorherigen Lektion verwendeten `SearchServiceClient` übertragen wurden).

Diese Klasse verfügt über mehrere Konstruktoren. Sie benötigen einen Konstruktor, der den Namen und den Indexnamen Ihres Suchdiensts sowie ein [`SearchCredentials`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials?view=azure-dotnet)-Objekt als Parameter akzeptiert. `SearchCredentials` umfasst Ihren API-Schlüssel.

Der folgende Code erstellt einen neuen `SearchIndexClient` für den Index „hotels“ mit den Werten für den Namen des Suchdiensts und den API-Schlüssel, die in der Konfigurationsdatei der Anwendung (`appsettings.json` im Fall der [Beispielanwendung](https://aka.ms/search-dotnet-howto)) gespeichert sind:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` verfügt über eine [`Documents`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient.documents?view=azure-dotnet)-Eigenschaft. Diese Eigenschaft stellt alle Methoden bereit, die Sie benötigen, um Azure Search-Indizes abzufragen.

## <a name="construct-searchparameters"></a>Erstellen eines SearchParameters-Objekts
Die Suche mit dem .NET SDK erfolgt ganz einfach durch Aufrufen der `Documents.Search`-Methode für `SearchIndexClient`. Diese Methode verwendet einige Parameter, einschließlich des Suchtexts, zusammen mit einem `SearchParameters` -Objekt, mit dem die Abfrage weiter verfeinert werden kann.

### <a name="types-of-queries"></a>Abfragetypen
Die zwei wichtigsten [Abfragetypen](search-query-overview.md#types-of-queries), die Sie verwenden werden, sind `search` und `filter`. Eine `search`-Abfrage sucht in Ihrem Index in allen *durchsuchbaren* Feldern nach einem oder mehreren Begriffen. Eine `filter`-Abfrage wertet einen booleschen Ausdruck für alle *filterbaren* Felder in einem Index aus. Suchvorgänge und Filter können separat oder zusammen verwendet werden.

Suchen und Filtern erfolgt mithilfe der `Documents.Search` -Methode. Eine Suchabfrage kann im Parameter `searchText` und ein Filterausdruck kann in der Eigenschaft `Filter` der Klasse `SearchParameters` übergeben werden. Um ohne Suche zu filtern, übergeben Sie einfach `"*"` für den Parameter `searchText`. Wenn Sie ohne Filter suchen möchten, legen Sie die Eigenschaft `Filter` nicht fest, oder übergeben Sie einfach keine `SearchParameters`-Instanz.

### <a name="example-queries"></a>Beispielabfragen
Der folgende Beispielcode zeigt verschiedene Methoden zum Abfragen des Indexes „hotels“, der unter [Erstellen eines Azure Search-Indexes in C#](search-create-index-dotnet.md#DefineIndex) definiert wurde. Beachten Sie, dass die in den Suchergebnissen zurückgegebenen Dokumente Instanzen der Klasse `Hotel` sind, die unter [Importieren von Daten in einen Azure Search-Index mit C#](search-import-data-dotnet.md#construct-indexbatch) definiert wurde. Der Beispielcode nutzt eine `WriteDocuments` -Methode, um die Suchergebnisse auf der Konsole auszugeben. Diese Methode wird im nächsten Abschnitt beschrieben.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a>Verarbeiten von Suchergebnissen
Die Methode `Documents.Search` gibt ein `DocumentSearchResult`-Objekt zurück, das die Ergebnisse der Abfrage enthält. Das Beispiel im vorherigen Abschnitt verwendet eine Methode namens `WriteDocuments` , um die Suchergebnisse auf der Konsole auszugeben:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Hier ist dargestellt, wie die Ergebnisse für die Abfragen im vorherigen Abschnitt aussehen. Dabei wird davon ausgegangen, dass der Index „hotels“ mit den Beispieldaten aufgefüllt wurde:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

Der obige Beispielcode verwendet die Konsole zum Ausgeben von Suchergebnissen. Sie müssen ebenso Suchergebnisse in Ihrer eigenen Anwendung anzeigen. Ein Beispiel zum Rendern von Suchergebnissen in einer ASP.NET MVC-basierten Webanwendung finden Sie im [DotNetSample-Projekt](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) auf GitHub.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie dies nicht schon getan haben, sehen Sie sich den Beispielcode in [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) auf GitHub an, und lesen Sie [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md), um detailliertere Beschreibungen des Beispielcodes zu erhalten. 
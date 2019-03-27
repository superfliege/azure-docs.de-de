---
title: Erstellen eines Index im Code mithilfe der .NET-API – Azure Search
description: Hier erfahren Sie, wie Sie mithilfe von Azure Search .NET SDK and C#-Beispielcode einen durchsuchbaren Volltextindex erstellen.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: dbaac1478fdbf1b42fc6b597c3a5c541e007e413
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287144"
---
# <a name="quickstart-1---create-an-azure-search-index-in-c"></a>Schnellstart: 1 – Erstellen eines Azure Search-Indexes in C#

In diesem Artikel lernen Sie, wie Sie einen [Azure Search-Index](search-what-is-an-index.md) mit C# und dem [.NET SDK](https://aka.ms/search-sdk) erstellen. Dies ist die erste Lektion einer dreiteiligen Übung zum Erstellen, Laden und Abfragen eines Indexes. Ein Index wird mit folgenden Schritten erstellt:

> [!div class="checklist"]
> * Erstellen Sie ein [`SearchServiceClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet)-Objekt, um eine Verbindung mit einem Suchdienst herzustellen.
> * Erstellen Sie ein [`Index`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet)-Objekt, das Sie als Parameter an `Indexes.Create` übergeben.
> * Rufen Sie die `Indexes.Create`-Methode in `SearchServiceClient` auf, um den `Index` an einen Dienst zu senden.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Für diese Schnellstartanleitung können Sie einen kostenlosen Dienst verwenden.

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), beliebige Edition. Der Beispielcode und die Anleitung wurden in der kostenlosen Community-Edition getestet.

Ein URL-Endpunkt und ein API-Administratorschlüssel für Ihren Suchdienst. Hierfür wird jeweils ein Suchdienst erstellt. Wenn Sie Azure Search also Ihrem Abonnement hinzugefügt haben, können Sie diese Schritte ausführen, um die erforderlichen Informationen zu erhalten:

  1. Rufen Sie die URL im Azure-Portal auf der Seite **Übersicht** Ihres Suchdiensts ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

  2. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie ein Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie entweder den primären oder den sekundären Schlüssel verwenden.

  ![Abrufen eines HTTP-Endpunkts und Zugriffsschlüssels](media/search-fiddler/get-url-key.png "Abrufen eines HTTP-Endpunkts und Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="1---open-the-project"></a>1 – Öffnen des Projekts

Laden Sie den Beispielcode [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) von GitHub herunter. 

Ersetzen Sie in der Datei „appsettings.json“ den Standardinhalt durch das folgende Beispiel, und geben Sie dann den Dienstnamen und den API-Administratorschlüssel für Ihren Dienst an. Als Dienstnamen benötigen Sie nur den Namen selbst. Wenn Ihre URL z.B. https://mydemo.search.windows.net lautet, fügen Sie `mydemo` zur JSON-Datei hinzu.


```json
{
    "SearchServiceName": "Put your search service name here",
    "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
}
```

Sobald diese Werte festgelegt wurden, können Sie die Projektmappe über die Taste F5 erstellen, um die Konsolen-App auszuführen. Die verbleibenden Schritte in dieser Übung sowie die darauf folgenden dienen zur Erkundung der Funktionsweise dieses Codes. 

Alternativ dazu finden Sie eine detaillierte Beschreibung des Verhaltens des SDK unter [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md). 

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-a-client"></a>2 – Erstellen eines Clients

Um das Azure Search .NET SDK zu verwenden, erstellen Sie eine Instanz der `SearchServiceClient`-Klasse. Diese Klasse verfügt über mehrere Konstruktoren. Für den gewünschten Konstruktor werden der Name Ihres Suchdiensts und ein `SearchCredentials` -Objekt als Parameter verwendet. `SearchCredentials` umfasst Ihren API-Schlüssel.

Den folgenden Code finden Sie in der Datei „Program.cs“. Dieser Code erstellt einen neuen `SearchServiceClient` mit den Werten für den Namen des Suchdiensts und den API-Schlüssel, die in der Konfigurationsdatei der Anwendung (appsettings.json) gespeichert sind.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient` verfügt über eine `Indexes`-Eigenschaft. Diese Eigenschaft stellt alle Methoden bereit, die Sie benötigen, um Azure Search-Indizes zu erstellen, aufzulisten, zu aktualisieren oder zu löschen.

> [!NOTE]
> Die `SearchServiceClient`-Klasse verwaltet die Verbindungen mit Ihrem Suchdienst. Um zu vermeiden, dass zu viele Verbindungen geöffnet werden, sollten Sie nur eine Instanz von `SearchServiceClient` in Ihrer Anwendung freigeben, sofern dies möglich ist. Die zugehörigen Methoden sind threadsicher und ermöglichen diese Freigabe.
> 
> 

<a name="DefineIndex"></a>

## <a name="3---construct-index"></a>3 – Erstellen des Indexes
Ein einziger Aufruf der `Indexes.Create`-Methode erstellt einen Index. Diese Methode akzeptiert ein `Index`-Objekt als Parameter, das Ihren Azure Search-Index definiert. Erstellen Sie ein `Index`-Objekt, und initialisieren Sie es wie folgt:

1. Legen Sie die `Name`-Eigenschaft des `Index`-Objekts auf den Namen des Index fest.

2. Legen Sie die `Fields`-Eigenschaft des `Index`-Objekts auf ein Array von `Field`-Objekten fest. Am einfachsten können Sie die `Field`-Objekte durch Aufrufen der `FieldBuilder.BuildForType`-Methode erstellen und dabei eine Modellklasse für den Typparameter übergeben. Eine Modellklasse verfügt über Eigenschaften, die den Felder Ihres Index zugeordnet werden. Dadurch können Sie Dokumente aus Ihrem Suchindex an Instanzen Ihrer Modellklasse binden.

> [!NOTE]
> Falls Sie keine Modellklasse verwenden möchten, können Sie `Field`-Objekte auch direkt erstellen, um Ihren Index zu definieren. Sie können den Namen des Felds zusammen mit dem Datentyp (oder dem Analyzer bei Zeichenfolgenfeldern) für den Konstruktor angeben. Sie können auch andere Eigenschaften festlegen, z. B. `IsSearchable` oder `IsFilterable`, um nur einige zu nennen.
>
>

Berücksichtigen Sie beim Gestalten des Indexes die Benutzerfreundlichkeit und die geschäftlichen Anforderungen. Jedem Feld müssen die [Attribute](https://docs.microsoft.com/rest/api/searchservice/Create-Index) zugewiesen werden, die steuern, welche Suchfunktionen (Filterung, Facettierung, Sortierung usw.) auf welche Felder angewendet werden. Für jede Eigenschaft, die Sie nicht explizit festlegen, wird mit der `Field`-Klasse automatisch die entsprechende Suchfunktion deaktiviert, bis Sie sie ausdrücklich aktivieren.

In diesem Beispiel heißt der Index „hotels“, und Felder werden mithilfe einer Modellklasse definiert. Jede Eigenschaft der Modellklasse verfügt über Attribute, die das suchbezogene Verhalten des entsprechenden Indexfelds bestimmen. Die Modellklasse ist wie folgt definiert:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Die Attribute für die einzelnen Eigenschaften wurden so gewählt, wie sie unseres Erachtens in einer Anwendung verwendet werden. So ist es beispielsweise wahrscheinlich, dass für Personen, die nach Hotels suchen, Übereinstimmungen mit Schlüsselwörtern im Feld `description` relevant sind. Daher aktivieren wir für dieses Feld die Volltextsuche, indem wir der `Description`-Eigenschaft das `IsSearchable`-Attribut hinzufügen.

In Ihrem Index vom Typ `string` muss genau ein Feld als *key*-Feld festgelegt sein. Hierzu muss das `Key`-Attribut hinzugefügt werden (wie unter `HotelId` im obigen Beispiel zu sehen).

Diese Indexdefinition verwendet für das Feld `description_fr` eine Sprachanalyse, da es für Text in französischer Sprache vorgesehen ist. Weitere Informationen finden Sie unter [Hinzufügen von Sprachanalysetools zu einem Azure Search-Index](index-add-language-analyzers.md).

> [!NOTE]
> Standardmäßig entspricht der Name jeder Eigenschaft in Ihrer Modellklasse dem jeweiligen Feldnamen im Index. Wenn Sie alle Eigenschaftennamen Feldnamen in Camel-Case-Schreibweise zuordnen möchten, markieren Sie die Klasse mit dem `SerializePropertyNamesAsCamelCase`-Attribut. Wenn Sie als Zuweisungsziel einen anderen Namen verwenden möchten, können Sie das `JsonProperty`-Attribut wie die `DescriptionFr`-Eigenschaft weiter oben verwenden. Das `JsonProperty`-Attribut hat Vorrang vor dem `SerializePropertyNamesAsCamelCase`-Attribut.
> 
> 

Nach dem Definieren einer Modellklasse können wir nun ganz problemlos eine Indexdefinition erstellen:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="4---call-indexescreate"></a>4 – Aufrufen von „Indexes.Create“
Nachdem Sie nun über ein initialisiertes `Index`-Objekt verfügen, erstellen Sie den Index, indem Sie `Indexes.Create` in Ihrem `SearchServiceClient`-Objekt aufrufen:

```csharp
serviceClient.Indexes.Create(definition);
```

Bei einer erfolgreich durchgeführten Anforderung wird die Methode normal zurückgegeben. Wenn ein Problem mit der Anforderung auftritt, z. B. ein ungültiger Parameter vorhanden ist, gibt die Methode `CloudException` zurück.

Wenn Sie einen Index nicht mehr benötigen und ihn löschen möchten, rufen Sie die `Indexes.Delete`-Methode in `SearchServiceClient` auf. Beispiel: 

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Der Beispielcode in diesem Artikel verwendet der Einfachheit halber die synchronen Methoden des Azure Search-.NET-SDK. Für Ihre eigenen Anwendungen empfehlen wir aus Gründen der Skalierbarkeit und Reaktionsfähigkeit die asynchronen Methoden. In den Beispielen oben können Sie `Create` und `Delete` beispielsweise auch durch `CreateAsync` und `DeleteAsync` ersetzen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie anhand eines Schemas, das Felddatentypen und Verhalten definiert, einen Azure Search-Index erstellt. Die nächste Schnellstartanleitung in dieser Reihe erläutert, wie Sie den Index mit durchsuchbarem Inhalt laden.

> [!div class="nextstepaction"]
> [Laden von Daten in einen Azure Search-Index mit C#](search-import-data-dotnet.md)
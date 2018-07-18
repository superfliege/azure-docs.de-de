---
title: Verwalten von Verbindungen in Azure Functions
description: Erfahren Sie, wie Sie durch die Verwendung statischer Verbindungsclients Leistungsprobleme in Azure Functions vermeiden.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: tdykstra
ms.openlocfilehash: 6c0af8f6f7e1d4aea8880a7af311aaa21f474f7e
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969003"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Verwalten von Verbindungen in Azure Functions

Azure Functions nutzt in einer Funktionen-App gemeinsame Ressourcen. Zu diesen gemeinsam genutzten Ressourcen zählen Verbindungen &mdash; HTTP-Verbindungen, Datenbankverbindungen und Verbindungen zu Azure-Diensten wie Azure Storage. Wenn viele Funktionen gleichzeitig ausgeführt werden, kann es sein, dass nicht mehr genügend Verbindungen zur Verfügung stehen. In diesem Artikel erfahren Sie, wie Sie Ihre Funktionen codieren, um zu vermeiden, dass Sie mehr Verbindungen als eigentlich erforderlich verwenden.

## <a name="connections-limit"></a>Verbindungslimit

Die Anzahl der verfügbaren Verbindungen ist teilweise begrenzt, da eine Funktionen-App im [Azure App Service-Sandkasten](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) ausgeführt wird. Eine Einschränkung, die der Sandkasten Ihrem Code auferlegt, ist eine [Obergrenze für die Anzahl der Verbindungen (derzeit 300)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Wenn dieses Limit erreicht ist, erstellt Functions Runtime ein Protokoll mit folgender Meldung: `Host thresholds exceeded: Connections`.

Die Chancen, den Grenzwert zu überschreiten, steigen, wenn der [Skalierungscontroller Funktionen-App-Instanzen hinzufügt](functions-scale.md#how-the-consumption-plan-works). Jede Funktionen-App-Instanz kann Funktionen mehrfach auf einmal aufrufen, und all diese Funktionen verwenden Verbindungen, die unter den Grenzwert von 300 fallen.

## <a name="use-static-clients"></a>Verwenden von statischen Clients

Um zu vermeiden, dass mehr Verbindungen als nötig gehalten werden, erstellen Sie bei jedem Funktionsaufruf keine neuen Instanzen, sondern verwenden die Clientinstanzen erneut. .NET-Clients wie `HttpClient`, `DocumentClient` und Azure Storage-Clients können Verbindungen verwalten, wenn Sie einen einzelnen, statischen Client verwenden.

Es folgen einige Richtlinien, die zu beachten sind, wenn Sie einen dienstspezifischen Client in einer Azure Functions-Anwendung verwenden:

- **NEIN** Erstellen Sie keinen neuen Client bei jedem Funktionsaufruf.
- **JA** Erstellen Sie einen einzelnen, statischen Client, der von jedem Funktionsaufruf verwendet werden kann.
- **VIELLEICHT** Denken Sie über die Erstellung eines einzelnen, statischen Clients in einer freigegebenen Hilfsprogrammklasse nach, wenn verschiedene Funktionen den gleichen Dienst verwenden.

## <a name="httpclient-code-example"></a>Codebeispiel für HttpClient

Es folgt ein Beispiel für einen Funktionscode, der einen statischen `HttpClient` erstellt:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Eine häufig gestellte Frage zum .NET `HttpClient` lautet: „Soll ich meinen Client löschen?“ Im Allgemeinen löschen Sie Objekte, die `IDisposable` implementieren, wenn Sie sie nicht mehr verwenden. Sie löschen jedoch keinen statischen Client, da dessen Verwendung mit dem Funktionsende nicht abgeschlossen ist. Der statische Client soll für die Dauer der Anwendung gültig sein.

## <a name="documentclient-code-example"></a>Codebeispiel für DocumentClient

`DocumentClient` stellt eine Verbindung zu einer Cosmos DB-Instanz her. Die Cosmos DB-Dokumentation empfiehlt das [Verwenden eines Singleton-Azure Cosmos DB-Clients für die Lebensdauer der Anwendung](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). Das folgende Beispiel zeigt ein Muster dafür in einer Funktion.

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client; 

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Empfehlung von statischen Clients finden Sie unter [Antimuster für ungeeignete Instanziierung](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Weitere Tipps zur Leistungssteigerung von Azure Functions finden Sie unter [Optimieren der Leistung und Zuverlässigkeit von Azure Functions](functions-best-practices.md).
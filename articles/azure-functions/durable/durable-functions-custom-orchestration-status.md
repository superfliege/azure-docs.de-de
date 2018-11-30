---
title: Status der benutzerdefinierten Orchestrierung in Durable Functions – Azure
description: Erfahren Sie, wie Sie den Status der benutzerdefinierten Orchestrierung für Durable Functions konfigurieren und verwenden.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: b8017288adb75c990113b0f2ff5ba29a1f1e0a18
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52637505"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Status der benutzerdefinierten Orchestrierung in Durable Functions (Azure Functions)

Mit dem Status der benutzerdefinierten Orchestrierung können Sie einen benutzerdefinierten Statuswert für Ihre Orchestrator-Funktion festlegen. Dieser Status wird über die HTTP GetStatus-API oder die `DurableOrchestrationClient.GetStatusAsync`-API bereitgestellt.

> [!NOTE]
> Der Status der benutzerdefinierten Orchestrierung für JavaScript wird in einem zukünftigen Release verfügbar sein.

## <a name="sample-use-cases"></a>Beispiele für Anwendungsfälle 

### <a name="visualize-progress"></a>Visualisieren des Fortschritts

Clients können den Statusendpunkt per Poll abrufen und eine Statusanzeige zur Visualisierung der aktuellen Ausführungsphase anzeigen. Das folgende Beispiel veranschaulicht die Ausführungsfreigabe:

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  var outputs = new List<string>();

  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
  context.SetCustomStatus("Tokyo");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
  context.SetCustomStatus("Seattle");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));
  context.SetCustomStatus("London");

  // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
  return outputs;
}

[FunctionName("E1_SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
  return $"Hello {name}!";
}
```

Und dann erhält der Client die Ausgabe der Orchestrierung nur, wenn das `CustomStatus`-Feld auf "London" festgelegt ist:

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
  [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
  [OrchestrationClient] DurableOrchestrationClientBase starter,
  string functionName,
  ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    DurableOrchestrationStatus durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    while (durableOrchestrationStatus.CustomStatus.ToString() != "London")
    {
      await Task.Delay(200);
      durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    }

    HttpResponseMessage httpResponseMessage = new HttpResponseMessage(HttpStatusCode.OK)
    {
      Content = new StringContent(JsonConvert.SerializeObject(durableOrchestrationStatus))
    };

    return httpResponseMessage;
  }
}
```

### <a name="output-customization"></a>Anpassung der Ausgabe 

Ein weiteres interessantes Szenario ist die Segmentierung der Benutzer durch Zurückgeben einer angepassten Ausgabe basierend auf bestimmten Eigenschaften oder Interaktionen. Mithilfe des Status der benutzerdefinierten Orchestrierung bleibt der clientseitige Code generisch. Alle wichtigen Änderungen erfolgen auf der Serverseite, wie im folgenden Beispiel gezeigt:

```csharp
[FunctionName("CityRecommender")]
public static void Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  int userChoice = context.GetInput<int>();

  switch (userChoice)
  {
    case 1:
    context.SetCustomStatus(new
    {
      recommendedCities = new[] {"Tokyo", "Seattle"},
      recommendedSeasons = new[] {"Spring", "Summer"}
     });
      break;
    case 2:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Seattle, London"},
        recommendedSeasons = new[] {"Summer"}
      });
        break;
      case 3:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Tokyo, London"},
        recommendedSeasons = new[] {"Spring", "Summer"}
      });
        break;
  }

  // Wait for user selection and refine the recommendation
} 
```

### <a name="instruction-specification"></a>Anweisungsspezifikation 

Der Orchestrator kann den Clients eindeutige Anweisungen über den benutzerdefinierten Status bereitstellen. Anweisungen zum benutzerdefinierten Status werden den Schritten im Orchestrierungscode zugeordnet:

```csharp
[FunctionName("ReserveTicket")]
public static async Task<bool> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  string userId = context.GetInput<string>();

  int discount = await context.CallActivityAsync<int>("CalculateDiscount", userId);

  context.SetCustomStatus(new
  {
    discount = discount,
    discountTimeout = 60,
    bookingUrl = "https://www.myawesomebookingweb.com",
  });

  bool isBookingConfirmed = await context.WaitForExternalEvent<bool>("BookingConfirmed");

  context.SetCustomStatus(isBookingConfirmed
    ? new {message = "Thank you for confirming your booking."}
    : new {message = "The booking was not confirmed on time. Please try again."});

  return isBookingConfirmed;
}
```

## <a name="sample"></a>Beispiel

Im folgenden Beispiel wird zuerst der benutzerdefinierte Status festgelegt.

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { nextActions = new [] {"A", "B", "C"}, foo = 2, };
    ctx.SetCustomStatus(customStatus);

    // ...do more work...
}
```

Während der Ausführung der Orchestrierung können externe Clients diesen benutzerdefinierten Status abrufen:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Clients erhalten folgende Antwort: 

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
>  Die Nutzlast des benutzerdefinierten Status ist auf UTF-16-JSON-Text mit einer Größe von 16 KB beschränkt, da dieser in eine Azure Table Storage-Spalte passen muss. Für eine größere Nutzlast können Entwickler externen Speicher verwenden.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu HTTP-APIs in Durable Functions](durable-functions-http-api.md)



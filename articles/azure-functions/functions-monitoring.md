---
title: "Überwachen von Azure Functions"
description: "Es wird beschrieben, wie Sie Azure Application Insights mit Azure Functions zum Überwachen der Funktionsausführung verwenden."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/15/2017
ms.author: tdykstra
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 90720774f956149dc159de1d5457e556a52ddc82
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="monitor-azure-functions"></a>Überwachen von Azure Functions

## <a name="overview"></a>Übersicht 

[Azure Functions](functions-overview.md) verfügt über eine integrierte Integration in [Azure Application Insights](../application-insights/app-insights-overview.md) zum Überwachen von Funktionen. In diesem Artikel wird veranschaulicht, wie Sie Functions zum Senden von Telemetriedaten an Application Insights konfigurieren.

![Metrik-Explorer für Application Insights](media/functions-monitoring/metrics-explorer.png)

Außerdem verfügt Functions über eine integrierte Überwachung, für die nicht Application Insights verwendet wird. Wir empfehlen die Nutzung von Application Insights, da Sie damit mehr Daten erhalten und diese Daten besser analysieren können. Informationen zur integrierten Überwachung finden Sie im [letzten Abschnitt dieses Artikels](#monitoring-without-application-insights).

## <a name="enable-application-insights-integration"></a>Aktivieren der Application Insights-Integration

Damit eine Funktionen-App Daten an Application Insights senden kann, muss sie den Instrumentierungsschlüssel einer Application Insights-Instanz kennen. Es gibt zwei Möglichkeiten, diese Verbindung im [Azure-Portal](https://portal.azure.com) herzustellen:

* [Erstellen Sie eine verbundene Application Insights-Instanz, wenn Sie die Funktionen-App erstellen](#new-function-app).
* [Stellen Sie für eine Application Insights-Instanz eine Verbindung mit einer vorhandenen Funktionen-App her](#existing-function-app).
 
### <a name="new-function-app"></a>Neue Funktionen-App

Aktivieren Sie Application Insights auf der Seite **Erstellen** der Funktionen-App:

1. Legen Sie den Switch **Application Insights** auf **Ein** fest.

2. Wählen Sie einen **Application Insights-Standort** aus.

   ![Aktivieren von Application Insights beim Erstellen einer Funktionen-App](media/functions-monitoring/enable-ai-new-function-app.png)

### <a name="existing-function-app"></a>Vorhandene Funktionen-App

Rufen Sie einen Instrumentierungsschlüssel ab, und speichern Sie ihn in einer Funktionen-App:

1. Erstellen Sie die Application Insights-Instanz. Legen Sie den Anwendungstyp auf **Allgemein** fest.

   ![Erstellen einer Application Insights-Instanz vom Typ „Allgemein“](media/functions-monitoring/ai-general.png)

2. Kopieren Sie den Instrumentierungsschlüssel von der Seite **Zusammenfassung** der Application Insights-Instanz. Bewegen Sie den Mauszeiger auf das Ende des angezeigten Schlüsselwerts, um die Schaltfläche **Klicken Sie zum Kopieren** anzuzeigen.

   ![Kopieren des Application Insights-Instrumentierungsschlüssels](media/functions-monitoring/copy-ai-key.png)

1. Gehen Sie auf der Seite **Anwendungseinstellungen** der Funktionen-App wie folgt vor: [Fügen Sie eine App-Einstellung hinzu](functions-how-to-use-azure-function-app-settings.md#settings), die den Namen APPINSIGHTS_INSTRUMENTATIONKEY hat, und fügen Sie den Instrumentierungsschlüssel ein.

   ![Hinzufügen des Instrumentierungsschlüssels zu den App-Einstellungen](media/functions-monitoring/add-ai-key.png)

1. Klicken Sie auf **Speichern**.

## <a name="view-telemetry-data"></a>Anzeigen von Telemetriedaten

Wählen Sie auf der Seite **Übersicht** den Link **Application Insights** aus, um von einer Funktionen-App im Portal zu Application Insights zu navigieren.

Informationen zur Verwendung von Application Insights finden Sie in der [Application Insights-Dokumentation](https://docs.microsoft.com/azure/application-insights/). In diesem Abschnitt sind einige Beispiele für das Anzeigen von Daten in Application Insights enthalten. Falls Sie mit Application Insights bereits vertraut sind, können Sie direkt zu den [Abschnitten zur Konfiguration und Anpassung der Telemetriedaten](#configure-categories-and-log-levels) springen.

Im [Metrik-Explorer](../application-insights/app-insights-metrics-explorer.md) können Sie Diagramme und Warnungen basierend auf Metriken erstellen, z.B. der Anzahl von Funktionsaufrufen, Ausführungsdauer und Erfolgsrate.

![Metrik-Explorer](media/functions-monitoring/metrics-explorer.png)

Auf der Registerkarte [Fehler](../application-insights/app-insights-asp-net-exceptions.md) können Sie Diagramme und Warnungen basierend auf Funktionsfehlern und Serverausnahmen erstellen. Der **Vorgangsname** ist der Funktionsname. Fehler in Abhängigkeiten werden nur angezeigt, wenn Sie die [benutzerdefinierte Telemetrie](#custom-telemetry-in-c-functions) für Abhängigkeiten implementieren.

![Fehler](media/functions-monitoring/failures.png)

Auf der Registerkarte [Leistung](../application-insights/app-insights-performance-counters.md) können Sie Leistungsprobleme analysieren.

![Leistung](media/functions-monitoring/performance.png)

Auf der Registerkarte **Server** werden die Ressourcenverwendung und der Durchsatz pro Server angezeigt. Diese Daten können nützlich für Debugszenarien sein, in denen Ihre zugrunde liegenden Ressourcen durch Funktionen eingeschränkt werden. Server werden als *Cloudrolleninstanzen* bezeichnet. 

![Server](media/functions-monitoring/servers.png)

Auf der Registerkarte [Live Metrics Stream](../application-insights/app-insights-live-stream.md) werden Metrikdaten während der Erstellung in Echtzeit angezeigt.

![Livestream](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Abfragen von Telemetriedaten

Mit [Application Insights Analytics](../application-insights/app-insights-analytics.md) haben Sie Zugriff auf alle Telemetriedaten in Form von Tabellen in einer Datenbank. Analytics umfasst eine Abfragesprache zum Extrahieren und Bearbeiten der Daten.

![Auswählen von Analytics](media/functions-monitoring/select-analytics.png)

![Analytics-Beispiel](media/functions-monitoring/analytics-traces.png)

Hier ist ein Beispiel für eine Abfrage angegeben. In diesem Beispiel ist die Verteilung von Anforderungen pro Worker in den letzten 30 Minuten dargestellt.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Die verfügbaren Tabellen werden im linken Bereich auf der Registerkarte **Schema** angezeigt. Daten, die durch Funktionsaufrufe generiert wurden, sind in den folgenden Tabellen enthalten:

* **traces**: Protokolle, die von der Laufzeit und durch Funktionscode erstellt wurden.
* **requests**: Jeweils eine Anforderung pro Funktionsaufruf.
* **exceptions**: Alle Ausnahmen, die von der Laufzeit ausgelöst werden.
* **customMetrics**: Anzahl von erfolgreichen und nicht erfolgreichen Aufrufen, Erfolgsrate, Dauer.
* **customEvents**: Von der Laufzeit nachverfolgte Ereignisse, z.B. HTTP-Anforderungen, die eine Funktion auslösen.
* **performanceCounters**: Informationen zur Leistung der Server, auf denen die Funktionen ausgeführt werden.

Die anderen Tabellen sind für Verfügbarkeitstests und Client/Browser-Telemetrie bestimmt. Sie können die benutzerdefinierte Telemetrie implementieren, um Daten hinzuzufügen.

In jeder Tabelle befinden sich einige der Functions-spezifischen Daten im Feld `customDimensions`.  Mit der folgenden Abfrage werden beispielsweise alle Ablaufverfolgungen mit der Protokollebene `Error` abgerufen.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Die Laufzeit stellt `customDimensions.LogLevel` und `customDimensions.Category` bereit. Sie können zusätzliche Felder in Protokollen angeben, die Sie in Ihren Funktionscode schreiben. Informationen hierzu finden Sie unter [Strukturierte Protokollierung](#structured-logging) in diesem Artikel.

## <a name="configure-categories-and-log-levels"></a>Konfigurieren von Kategorien und Protokollebenen

Sie können Application Insights ohne benutzerdefinierte Konfiguration verwenden, aber die Standardkonfiguration kann zu hohen Datenvolumina führen. Wenn Sie ein Visual Studio Azure-Abonnement verwenden, erreichen Sie unter Umständen Ihr Datenlimit für App Insights. Im restlichen Teil dieses Artikels wird beschrieben, wie Sie die Daten konfigurieren und anpassen, die Ihre Funktionen an Application Insights senden.

### <a name="categories"></a>Kategorien

Die Azure Functions-Protokollierung enthält eine *Kategorie* für jedes Protokoll. Mit der Kategorie wird angegeben, von welchem Teil des Laufzeitcodes bzw. Ihres Funktionscodes das Protokoll geschrieben wurde. 

Mit der Functions-Laufzeit werden Protokolle mit einer Kategorie erstellt, die mit „Host“ beginnt. Beispielsweise verfügen die Protokolle „function started“, „function executed“ und „function completed“ über die Kategorie „Host.Executor“. 

Wenn Sie Protokolle in Ihren Funktionscode schreiben, lautet die Kategorie „Function“.

### <a name="log-levels"></a>Protokollebenen

Die Azure Functions-Protokollierung enthält auch eine *Protokollebene* für jedes Protokoll. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) ist eine Enumeration, und der Code (eine Zahl) weist auf die relative Bedeutung hin:

|LogLevel    |Code|
|------------|---|
|Trace       | 0 |
|Debuggen       | 1 |
|Information | 2 |
|Warnung     | 3 |
|Error       | 4 |
|Kritisch    | 5 |
|Keine        | 6 |

Die Protokollebene `None` wird im nächsten Abschnitt beschrieben. 

### <a name="configure-logging-in-hostjson"></a>Konfigurieren der Protokollierung in „host.json“

Mit der Datei *host.json* wird konfiguriert, welcher Protokollierungsgrad von einer Funktionen-App an Application Insights gesendet wird. Für jede Kategorie geben Sie zu sendende Mindestprotokollebene an. Hier sehen Sie ein Beispiel:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

In diesem Beispiel werden die folgenden Regeln eingerichtet:

1. Für Protokolle mit der Kategorie „Host.Results“ oder „Function“ soll nur die Ebene `Error` und höher an Application Insights gesendet werden. Protokolle für die Ebene `Information` und niedriger werden ignoriert.
2. Für Protokolle mit der Kategorie „Host. Aggregator“ soll nur die Ebene `Information` und höher an Application Insights gesendet werden. Protokolle für die Ebene `Debug` und niedriger werden ignoriert.
3. Für alle anderen Protokolle soll nur die Ebene `Information` und höher an Application Insights gesendet werden.

Der Kategoriewert in *host.json* steuert die Protokollierung für alle Kategorien, die mit dem gleichen Wert beginnen. Mit „Host“ in *host.json* wird beispielsweise die Protokollierung für „Host.General“, „Host.Executor“, „Host.Results“ usw. gesteuert.

Wenn *host.json* mehrere Kategorien enthält, die mit der gleichen Zeichenfolge beginnen, werden zuerst die längeren abgeglichen. Nehmen Sie Folgendes an: Sie möchten, dass für die Laufzeit alles mit Ausnahme von „Host.Aggregator“ auf der Ebene `Error` protokolliert wird, während „Host.Aggregator“ auf der Ebene `Information` protokolliert wird:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Um alle Protokolle für eine Kategorie zu unterdrücken, können Sie die Protokollebene `None` verwenden. Mit dieser Kategorie werden keine Protokolle geschrieben, und darüber ist keine Protokollebene vorhanden.

In den folgenden Abschnitten werden die Hauptkategorien der Protokolle beschrieben, die von der Laufzeit erstellt werden. 

### <a name="category-hostresults"></a>Kategorie „Host.Results“

Diese Protokolle werden in Application Insights als „requests“ (Anforderungen) angezeigt. Sie geben für eine Funktion den Erfolg oder das Fehlschlagen an.

![Diagramm zu Anforderungen](media/functions-monitoring/requests-chart.png)

Alle diese Protokolle werden auf der Ebene `Information` geschrieben. Wenn Sie auf der Ebene `Warning` oder höher filtern, werden diese Daten nicht angezeigt.

### <a name="category-hostaggregator"></a>Kategorie „Host.Aggregator“

Diese Protokolle stellen die Anzahl und Durchschnittswerte von Funktionsaufrufen für einen [konfigurierbaren](#configure-the-aggregator) Zeitraum dar. Der Standardzeitraum beträgt 30 Sekunden oder 1.000 Ergebnisse, je nachdem, was früher eintritt. 

Die Protokolle werden in Application Insights als „customMetrics“ angezeigt. Beispiele hierfür sind die Ausführungsanzahl, Erfolgsrate und Dauer.

![customMetrics-Abfrage](media/functions-monitoring/custom-metrics-query.png)

Alle diese Protokolle werden auf der Ebene `Information` geschrieben. Wenn Sie auf der Ebene `Warning` oder höher filtern, werden diese Daten nicht angezeigt.

### <a name="other-categories"></a>Weitere Kategorien

Alle Protokolle für andere Kategorien, die nicht bereits aufgeführt sind, werden in Application Insights als „traces“ (Ablaufverfolgungen) angezeigt.

![traces-Abfrage](media/functions-monitoring/analytics-traces.png)

Alle Protokolle mit Kategorien, die mit „Host“ beginnen, werden von der Functions-Laufzeit geschrieben. Die Protokolle „Function started“ und „Function completed“ verfügen über die Kategorie „Host.Executor“. Für erfolgreiche Ausführungen gilt für diese Protokolle die Ebene `Information`. Ausnahmen werden unter der Ebene `Error` protokolliert. Die Laufzeit erstellt auch Protokolle der Ebene `Warning`, z.B. an die Warteschlange für nicht verarbeitete Nachrichten gesendete Warteschlangennachrichten.

Protokolle, die von Ihrem Funktionscode geschrieben werden, verfügen über die Kategorie „Function“ und können jede Protokollebene aufweisen.

## <a name="configure-the-aggregator"></a>Konfigurieren des Aggregators

Wie im vorherigen Abschnitt erwähnt, werden von der Laufzeit Daten zu den Funktionsausführungen in einem bestimmten Zeitraum aggregiert. Der Standardzeitraum beträgt 30 Sekunden oder 1.000 Ausführungen, je nachdem, was früher eintritt. Sie können diese Einstellung in der Datei *host.json* konfigurieren.  Hier sehen Sie ein Beispiel:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Konfigurieren des Samplings

Application Insights verfügt über ein [Sampling](../application-insights/app-insights-sampling.md)-Feature als Schutz davor, dass bei Spitzenlast zu viele Telemetriedaten produziert werden. Wenn die Anzahl von Telemetrieelementen eine bestimmte Rate übersteigt, beginnt Application Insights damit, einige der eingehenden Elemente nach dem Zufallsprinzip zu ignorieren. Sie können das Sampling in der Datei *host.json* konfigurieren.  Hier sehen Sie ein Beispiel:

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

## <a name="write-logs-in-c-functions"></a>Schreiben von Protokollen in C#-Funktionen

Sie können Protokolle in Ihrem Funktionscode schreiben, die in Application Insights als Ablaufverfolgungen angezeigt werden.

### <a name="ilogger"></a>ILogger

Verwenden Sie den Parameter [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) in Ihren Funktionen anstelle des Parameters `TraceWriter`. Protokolle, die mit `TraceWriter` erstellt werden, werden an Application Insights geleitet, aber mit `ILogger` können Sie die [strukturierte Protokollierung](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) durchführen.

Mit einem `ILogger`-Objekt rufen Sie `Log<level>`-[Erweiterungsmethoden in ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions#Methods_) auf, um Protokolle zu erstellen. Mit dem folgenden Code werden beispielsweise Protokolle vom Typ `Information` mit der Kategorie „Function“ geschrieben.

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Strukturierte Protokollierung

Die Reihenfolge der Platzhalter, nicht der Namen, bestimmt, welche Parameter in der Protokollnachricht verwendet werden. Angenommen, Sie verwenden den folgenden Code:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Wenn Sie die gleiche Nachrichtenzeichenfolge beibehalten und die Reihenfolge der Parameter umkehren, befinden sich die Werte im resultierenden Nachrichtentext an den falschen Stellen.

Platzhalter werden auf diese Weise verarbeitet, damit Sie die strukturierte Protokollierung durchführen können. Application Insights speichert die Name/Wert-Paare für Parameter zusätzlich zur Nachrichtenzeichenfolge. Das Ergebnis ist, dass die Nachrichtenargumente zu Feldern werden, anhand denen Sie Abfragen durchführen können.

Wenn Ihr Methodenaufruf für die Protokollierung beispielsweise wie im vorherigen Beispiel aussieht, können Sie das Feld `customDimensions.prop__rowKey` abfragen. Durch das Hinzufügen des Präfix soll sichergestellt werden, dass es zwischen den Feldern, die von der Laufzeit hinzugefügt werden, und Feldern, die von Ihrem Funktionscode hinzugefügt werden, nicht zu Konflikten kommt.

Sie können auch die ursprüngliche Nachrichtenzeichenfolge abfragen, indem Sie auf das Feld `customDimensions.prop__{OriginalFormat}` verweisen.  

Hier ist eine JSON-Beispieldarstellung von `customDimensions`-Daten angegeben:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="logging-custom-metrics"></a>Protokollieren von benutzerdefinierten Metriken  

In C#-Skriptfunktionen können Sie die `LogMetric`-Erweiterungsmethode in `ILogger` verwenden, um in Application Insights benutzerdefinierte Metriken zu erstellen. Hier ist ein Beispiel für einen Methodenaufruf angegeben:

```csharp
logger.LogMetric("TestMetric", 1234); 
```

Dieser Code ist eine Alternative zum Aufrufen von `TrackMetric` mithilfe der [Application Insights-API für .NET](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>Schreiben von Protokollen in JavaScript-Funktionen

Verwenden Sie in Node.js-Funktionen `context.log` zum Schreiben von Protokollen. Die strukturierte Protokollierung ist nicht aktiviert.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Protokollieren von benutzerdefinierten Metriken  

In Node.js-Funktionen können Sie die `context.log.metric`-Methode verwenden, um in Application Insights benutzerdefinierte Metriken zu erstellen. Hier ist ein Beispiel für einen Methodenaufruf angegeben:

```javascript
context.log.metric("TestMetric", 1234); 
```

Dieser Code ist eine Alternative zum Aufrufen von `trackMetric` mithilfe des [Node.js SDK für Application Insights](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>Benutzerdefinierte Telemetrie in C#-Funktionen

Sie können das NuGet-Paket [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) verwenden, um benutzerdefinierte Telemetriedaten an Application Insights zu senden.

Hier ist ein Beispiel für C#-Code angegeben, in dem die [API für benutzerdefinierte Telemetrie](../application-insights/app-insights-api-custom-events-metrics.md) verwendet wird. Das Beispiel gilt für eine .NET-Klassenbibliothek, aber der Application Insights-Code ist für C#-Skript identisch.

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetry = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;

            telemetry.Context.Operation.Id = context.InvocationId.ToString();
            telemetry.Context.Operation.Name = "cs-http";
            if (!String.IsNullOrEmpty(name))
            {
                telemetry.Context.User.Id = name;
            }
            telemetry.TrackEvent("Function called");
            telemetry.TrackMetric("Test Metric", DateTime.Now.Millisecond);
            telemetry.TrackDependency("Test Dependency", 
                "swapi.co/api/planets/1/", 
                start, DateTime.UtcNow - start, true);

            return name == null
                ? req.CreateResponse(HttpStatusCode.BadRequest, 
                    "Please pass a name on the query string or in the request body")
                : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
        }
    }
}
```

Vermeiden Sie es, `TrackRequest` oder `StartOperation<RequestTelemetry>` aufzurufen, da in diesem Fall für einen Funktionsaufruf doppelte Anforderungen angezeigt werden.  Mit der Functions-Laufzeit werden Anforderungen automatisch nachverfolgt.

Legen Sie `telemetry.Context.Operation.Id` bei jedem Start der Funktion auf die Aufrufkennung fest. Dies ermöglicht die Korrelation aller Telemetrieelemente für einen bestimmten Funktionsaufruf.

```cs
telemetry.Context.Operation.Id = context.InvocationId.ToString();
```

## <a name="custom-telemetry-in-javascript-functions"></a>Benutzerdefinierte Telemetrie in JavaScript-Funktionen

Das [Application Insights Node.js SDK](https://www.npmjs.com/package/applicationinsights) ist derzeit als Betaversion verfügbar. Hier ist Beispielcode angegeben, mit dem benutzerdefinierte Telemetriedaten an Application Insights gesendet werden:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

Mit dem Parameter `tagOverrides` wird `operation_Id` auf die Aufrufkennung der Funktion festgelegt. Mithilfe dieser Einstellung können Sie die gesamte automatisch generierte und benutzerdefinierte Telemetrie für einen bestimmten Funktionsaufruf korrelieren.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="dependencies"></a>Abhängigkeiten

Abhängigkeiten werden nicht automatisch angezeigt, aber Sie können benutzerdefinierten Code schreiben, um Abhängigkeiten anzuzeigen. Dies wird im Beispielcode im [Abschnitt zu den benutzerdefinierten C#-Telemetriedaten](#create-custom-telemetry-data-in-c-function-code) veranschaulicht. Der Beispielcode ergibt in Application Insights eine *Anwendungszuordnung*, die wie folgt aussieht:

![Anwendungszuordnung](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Melden von Problemen

[Erstellen Sie auf GitHub einen Problemeintrag](https://github.com/Azure/Azure-Functions/issues/new), um ein Problem mit der Application Insights-Integration in Functions zu melden oder einen Vorschlag oder eine Anfrage zu erstellen.

## <a name="monitoring-without-application-insights"></a>Überwachen ohne Application Insights

Wir empfehlen die Nutzung von Application Insights zum Überwachen von Funktionen, da Sie damit mehr Daten erhalten und diese Daten besser analysieren können. Sie finden Telemetrie- und Protokolldaten aber auch auf den Seiten des Azure-Portals für eine Funktionen-App. 

Wenn Sie für eine Funktion die Registerkarte **Überwachen** wählen, erhalten Sie eine Liste mit Funktionsausführungen. Wählen Sie eine Funktionsausführung aus, um die Dauer, Eingabedaten, Fehler und zugehörigen Protokolldateien zu überprüfen.

> [!IMPORTANT]
> Wenn Sie den [Verbrauchstarif](functions-overview.md#pricing) für Azure Functions verwenden, zeigt die Kachel **Überwachung** der Funktionen-App keine Daten. Das liegt an der dynamischen Skalierung und Verwaltung von Computeinstanzen durch die Plattform. Diese Metriken sind in einem Verbrauchstarif ohne Bedeutung.

### <a name="real-time-monitoring"></a>Überwachung in Echtzeit

Die Überwachung in Echtzeit ist verfügbar, indem Sie auf der Registerkarte **Überwachen** der Funktion auf **Liveereignisdatenstrom** klicken. Der Liveereignisdatenstrom wird in einem Diagramm auf einer neuen Registerkarte im Browser angezeigt.

> [!NOTE]
> Es gibt ein bekanntes Problem, das dazu führen kann, dass Ihre Daten nicht aufgefüllt werden. Unter Umständen müssen Sie die Browserregisterkarte mit dem Liveereignisdatenstrom schließen und dann erneut auf **Liveereignisdatenstrom** klicken, damit Ihre Ereignisdatenstromdaten richtig aufgefüllt werden. 

Diese Statistiken werden in Echtzeit erfasst, aber die grafische Darstellung der Ausführungsdaten kann ca. 10 Sekunden verzögert erfolgen.

### <a name="monitor-log-files-from-a-command-line"></a>Überwachen von Protokolldateien über eine Befehlszeile

Sie können Protokolldateien mithilfe der Azure-Befehlszeilenschnittstelle 1.0 oder über PowerShell in eine Befehlszeilensitzung auf einer lokalen Arbeitsstation streamen.

### <a name="monitor-function-app-log-files-with-the-azure-cli-10"></a>Überwachen der Protokolldateien von Funktionen-Apps mit der Azure-Befehlszeilenschnittstelle 1.0

[Installieren Sie zuerst die Azure CLI 1.0](../cli-install-nodejs.md) (Azure-Befehlszeilenschnittstelle 1.0), und [melden Sie sich an Azure an](../xplat-cli-connect.md).

Verwenden Sie die folgenden Befehle, um den klassischen Dienstverwaltungsmodus zu aktivieren, Ihr Abonnement auszuwählen und Protokolldateien zu streamen:

```
azure config mode asm
azure account list
azure account set <subscriptionNameOrId>
azure site log tail -v <function app name>
```

### <a name="monitor-function-app-log-files-with-powershell"></a>Überwachen der Protokolldateien von Funktionen-Apps mit PowerShell

Als Erstes [installieren und konfigurieren Sie Azure PowerShell](/powershell/azure/overview).

Verwenden Sie die folgenden Befehle, um Ihr Azure-Konto hinzuzufügen, Ihr Abonnement auszuwählen und Protokolldateien zu streamen:

```
PS C:\> Add-AzureAccount
PS C:\> Get-AzureSubscription
PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription
PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail
```

Weitere Informationen finden Sie unter [Vorgehensweise: Streaming von Protokollen](../app-service/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu Application Insights](https://docs.microsoft.com/azure/application-insights/)

> [!div class="nextstepaction"]
> [Weitere Informationen zum von Functions verwendeten Protokollierungsframework](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)


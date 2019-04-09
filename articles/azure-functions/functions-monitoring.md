---
title: Überwachen von Azure Functions
description: Erfahren Sie, wie Sie Azure Application Insights mit Azure Functions zum Überwachen der Funktionsausführung verwenden.
services: functions
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: glenga
ms.openlocfilehash: 0224d9ba5a430635e4675c2fb2bf354e7c975f31
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518727"
---
# <a name="monitor-azure-functions"></a>Überwachen von Azure Functions

[Azure Functions](functions-overview.md) bietet von Haus aus Integration in [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) zum Überwachen von Funktionen. In diesem Artikel wird veranschaulicht, wie Sie Azure Functions zum Senden von systemseitig generierten Protokolldateien an Application Insights konfigurieren.

![Metrik-Explorer für Application Insights](media/functions-monitoring/metrics-explorer.png)

Außerdem verfügt Azure Functions über eine integrierte Überwachung, für die nicht Application Insights verwendet wird. Wir empfehlen die Nutzung von Application Insights, da Sie damit mehr Daten erhalten und diese Daten besser analysieren können.

## <a name="application-insights-pricing-and-limits"></a>Application Insights – Preise und Limits

Sie können die Application Insights-Integration in Funktionen-Apps kostenlos testen. Für die Menge der Daten, die täglich kostenlos verarbeitet werden können, gibt es einen Grenzwert. Beim Testen können Sie an diesen Grenzwert stoßen. In Azure erhalten Sie Benachrichtigungen im Portal und per E-Mail, wenn Ihr Tageslimit beinahe erreicht ist. Wenn Sie diese Benachrichtigungen ignorieren und das Limit erreichen, werden neue Protokolle nicht in Application Insights-Abfragen angezeigt. Seien Sie sich des Grenzwerts bewusst, um unnötigen Zeitaufwand für die Problembehandlung zu vermeiden. Weitere Informationen finden Sie unter [Verwalten von Preisen und Datenvolumen in Application Insights](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Aktivieren der Application Insights-Integration

Damit eine Funktionen-App Daten an Application Insights senden kann, muss sie den Instrumentierungsschlüssel einer Application Insights-Ressource kennen. Der Schlüssel muss in der App-Einstellung **APPINSIGHTS_INSTRUMENTATIONKEY** angegeben werden.

Sie können diese Verbindung im [Azure-Portal](https://portal.azure.com) einrichten:

* [Automatisches Herstellen einer Verbindung mit einer neuen Funktions-App](#new-function-app)
* [Manuelles Herstellen einer Verbindung mit einer Application Insights-Ressource](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Neue Funktionen-App
<!-- Add a transitional sentence to introduce the procedure. -->

1. Navigieren Sie zur Seite **Erstellen** der Funktionen-App.

1. Legen Sie den Switch **Application Insights** auf **Ein** fest.

1. Wählen Sie einen **Application Insights-Standort** aus. Wählen Sie die der Region der Funktions-App nächstgelegene Region in einem [Azure-Gebiet](https://azure.microsoft.com/global-infrastructure/geographies/) aus, in dem Ihre Daten gespeichert werden sollen.

   ![Aktivieren von Application Insights beim Erstellen einer Funktionen-App](media/functions-monitoring/enable-ai-new-function-app.png)

1. Geben Sie die erforderlichen Informationen ein, und wählen Sie **Erstellen** aus.

Im nächsten Schritt wird die [integrierte Protokollierung deaktiviert](#disable-built-in-logging).


<a id="manually-connect-an-app-insights-resource"></a>
### <a name="application-insights-resource"></a>Application Insights-Ressource 
<!-- Add a transitional sentence to introduce the procedure. -->

1. Erstellen Sie die Application Insights-Ressource. Legen Sie den Anwendungstyp auf **Allgemein** fest.

   ![Erstellen einer Application Insights-Ressource vom Typ „Allgemein“](media/functions-monitoring/ai-general.png)

1. Kopieren Sie den Instrumentierungsschlüssel von der Seite **Zusammenfassung** der Application Insights-Ressource. Zeigen Sie mit dem Mauszeiger auf das Ende des angezeigten Schlüsselwerts, um die Schaltfläche **Zum Kopieren klicken** anzuzeigen.

   ![Kopieren des Application Insights-Instrumentierungsschlüssels](media/functions-monitoring/copy-ai-key.png)

1. Fügen Sie auf der Seite **Anwendungseinstellungen** der Funktions-App [eine App-Einstellung](functions-how-to-use-azure-function-app-settings.md#settings) hinzu, indem Sie **Neue Einstellung hinzufügen** auswählen. Benennen Sie die neue Einstellung **APPINSIGHTS_INSTRUMENTATIONKEY**, und fügen Sie den kopierten Instrumentierungsschlüssel ein.

   ![Hinzufügen des Instrumentierungsschlüssels zu den App-Einstellungen](media/functions-monitoring/add-ai-key.png)

1. Wählen Sie **Speichern** aus.

<!-- Before the next H2 heading, add transitional sentences to summarize why the procedures were necessary. -->

## <a name="disable-built-in-logging"></a>Deaktivieren der integrierten Protokollierung

Wenn Sie Application Insights aktivieren, deaktivieren Sie die integrierte Protokollierung mit Verwendung von Azure Storage. Die integrierte Protokollierung ist für Tests mit einfachen Workloads hilfreich, aber sie ist nicht für die Nutzung in der Produktion mit hohen Auslastungen bestimmt. Für die Produktionsüberwachung empfehlen wir die Verwendung von Application Insights. Bei Nutzung der integrierten Protokollierung in der Produktion kann der Protokollierungsdatensatz aufgrund einer Drosselung von Azure Storage ggf. unvollständig sein.

Löschen Sie die App-Einstellung `AzureWebJobsDashboard`, um die integrierte Protokollierung zu deaktivieren. Informationen zum Löschen von App-Einstellungen im Azure-Portal finden Sie im Abschnitt **Anwendungseinstellungen** unter [Verwalten einer Funktionen-App im Azure-Portal](functions-how-to-use-azure-function-app-settings.md#settings). Stellen Sie vor dem Löschen der App-Einstellung sicher, dass sie nicht für vorhandene Funktionen in derselben Funktions-App für Azure Storage-Trigger oder -Bindungen verwendet wird.

## <a name="view-telemetry-in-monitor-tab"></a>Anzeigen von Telemetriedaten auf der Registerkarte „Überwachen“

Nachdem Sie die Application Insights-Integration wie in den vorherigen Abschnitten gezeigt eingerichtet haben, können Sie auf der Registerkarte **Überwachen** Telemetriedaten anzeigen.

1. Wählen Sie auf der Seite der Funktions-App eine Funktion aus, die nach der Konfiguration von Application Insights mindestens einmal ausgeführt wurde. Wählen Sie dann die Registerkarte **Überwachen** aus.

   ![Auswählen der Registerkarte „Überwachen“](media/functions-monitoring/monitor-tab.png)

1. Wählen Sie wiederholt die Option **Aktualisieren** aus, bis die Liste der Funktionsaufrufe angezeigt wird.

   Es kann bis zu 5 Minuten dauern, bis die Liste angezeigt wird, während der Telemetrieclient Daten zur Übermittlung an den Server in Batches zusammenfasst. (Diese Verzögerung gilt nicht für [Live Metrics Stream](../azure-monitor/app/live-stream.md). Bei diesem Dienst wird eine Verbindung mit dem Functions-Host hergestellt, wenn Sie die Seite laden, sodass Protokolle direkt an die Seite gestreamt werden.)

   ![Liste der Funktionsaufrufe](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. Wählen Sie zum Anzeigen der Protokolle für einen bestimmten Funktionsaufruf den Link für diesen Aufruf in der Spalte **Datum** aus.

   ![Link für Aufrufdetails](media/functions-monitoring/invocation-details-link-ai.png)

   Die Protokollausgabe für diesen Aufruf wird auf einer neuen Seite angezeigt.

   ![Aufrufdetails](media/functions-monitoring/invocation-details-ai.png)

Beide Seiten (Aufrufliste und Aufrufdetails) sind mit der Application Insights Analytics-Abfrage verknüpft, die die Daten abruft:

![In Application Insights ausführen](media/functions-monitoring/run-in-ai.png)

![Application Insights Analytics-Aufrufliste](media/functions-monitoring/ai-analytics-invocation-list.png)

Aus diesen Abfragen können Sie ersehen, dass die Aufrufliste auf die letzten 30 Tage beschränkt ist. Die Liste zeigt nicht mehr als 20 Zeilen an (`where timestamp > ago(30d) | take 20`). Die Liste der Aufrufdetails betrifft die letzten 30 Tage ohne Beschränkung.

Weitere Informationen finden Sie weiter unten in diesem Artikel unter [Abfragen von Telemetriedaten](#query-telemetry-data).

## <a name="view-telemetry-in-application-insights"></a>Anzeigen von Telemetriedaten in Application Insights

Um Application Insights aus einer Funktions-App im Azure-Portal zu öffnen, wechseln Sie zur Seite **Übersicht** der Funktions-App. Wählen Sie unter **Konfigurierte Features** **Application Insights** aus.

![Öffnen von Application Insights auf der Seite „Übersicht“ von Funktions-Apps](media/functions-monitoring/ai-link.png)

Informationen zur Verwendung von Application Insights finden Sie in der [Application Insights-Dokumentation](https://docs.microsoft.com/azure/application-insights/). In diesem Abschnitt sind einige Beispiele für das Anzeigen von Daten in Application Insights enthalten. Falls Sie mit Application Insights bereits vertraut sind, können Sie direkt zu den [Abschnitten zur Konfiguration und Anpassung der Telemetriedaten](#configure-categories-and-log-levels) springen.

Im [Metrik-Explorer](../azure-monitor/app/metrics-explorer.md) können Sie Diagramme und Warnungen auf der Grundlage von Metriken erstellen. Metriken enthalten die Anzahl der Funktionsaufrufe, die Ausführungsdauer und die Erfolgsquote.

![Metrik-Explorer](media/functions-monitoring/metrics-explorer.png)

Auf der Registerkarte [Fehler](../azure-monitor/app/asp-net-exceptions.md) können Sie Diagramme und Warnungen basierend auf Funktionsfehlern und Serverausnahmen erstellen. Der **Vorgangsname** ist der Funktionsname. Fehler in Abhängigkeiten werden nur angezeigt, wenn Sie die benutzerdefinierte Telemetrie für Abhängigkeiten implementieren.

![Fehler](media/functions-monitoring/failures.png)

Auf der Registerkarte [Leistung](../azure-monitor/app/performance-counters.md) können Sie Leistungsprobleme analysieren.

![Leistung](media/functions-monitoring/performance.png)

Auf der Registerkarte **Server** werden die Ressourcenverwendung und der Durchsatz pro Server angezeigt. Diese Daten können nützlich für Debugszenarien sein, in denen Ihre zugrunde liegenden Ressourcen durch Funktionen eingeschränkt werden. Server werden als **Cloudrolleninstanzen** bezeichnet.

![Server](media/functions-monitoring/servers.png)

Auf der Registerkarte [Live Metrics Stream](../azure-monitor/app/live-stream.md) werden Metrikdaten während der Erstellung in Echtzeit angezeigt.

![Livestream](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Abfragen von Telemetriedaten

Mit [Application Insights Analytics](../azure-monitor/app/analytics.md) haben Sie Zugriff auf alle Telemetriedaten in Form von Tabellen in einer Datenbank. Analytics stellt eine Abfragesprache zum Extrahieren, Bearbeiten und Visualisieren der Daten bereit.

![Auswählen von Analytics](media/functions-monitoring/select-analytics.png)

![Analytics-Beispiel](media/functions-monitoring/analytics-traces.png)

Diese Abfragebeispiel zeigt Verteilung von Anforderungen pro Worker in den letzten 30 Minuten.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Die verfügbaren Tabellen werden links auf der Registerkarte **Schema** angezeigt. Daten, die durch Funktionsaufrufe generiert wurden, sind in den folgenden Tabellen enthalten:

* **traces**: Protokolle, die von der Laufzeit und durch Funktionscode erstellt wurden.
* **requests**: Jeweils eine Anforderung pro Funktionsaufruf.
* **exceptions**: Alle Ausnahmen, die von der Laufzeit ausgelöst werden.
* **customMetrics**: Die Anzahl von erfolgreichen und nicht erfolgreichen Aufrufen, Erfolgsrate und Dauer.
* **customEvents**: Ereignisse, die von der Runtime verfolgt werden, z. B.: HTTP-Anforderungen, die eine Funktion auslösen.
* **performanceCounters**: Informationen zur Leistung der Server, auf denen die Funktionen ausgeführt werden.

Die anderen Tabellen sind für Verfügbarkeitstests und Client/Browser-Telemetrie bestimmt. Sie können die benutzerdefinierte Telemetrie implementieren, um Daten hinzuzufügen.

In jeder Tabelle befinden sich einige der Functions-spezifischen Daten im Feld `customDimensions`.  Mit der folgenden Abfrage werden beispielsweise alle Ablaufverfolgungen mit der Protokollebene `Error` abgerufen.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Die Runtime stellt die Felder `customDimensions.LogLevel` und `customDimensions.Category` zur Verfügung. Sie können zusätzliche Felder in Protokollen angeben, die Sie in Ihren Funktionscode schreiben. Informationen hierzu finden Sie unter [Strukturierte Protokollierung](#structured-logging) in diesem Artikel.

## <a name="configure-categories-and-log-levels"></a>Konfigurieren von Kategorien und Protokollebenen

Sie können Application Insights ganz ohne benutzerdefinierte Konfiguration verwenden. Die Standardkonfiguration kann zu großen Datenmengen führen. Wenn Sie ein Visual Studio Azure-Abonnement verwenden, erreichen Sie unter Umständen Ihr Datenlimit für Application Insights. Später in diesem Artikel erfahren Sie, wie Sie die Daten konfigurieren und anpassen, die Ihre Funktionen an Application Insights senden.

### <a name="categories"></a>Categories

Die Azure Functions-Protokollierung enthält eine *Kategorie* für jedes Protokoll. Mit der Kategorie wird angegeben, von welchem Teil des Laufzeitcodes bzw. Ihres Funktionscodes das Protokoll geschrieben wurde. 

Mit der Functions-Runtime werden Protokolle mit einer Kategorie erstellt, die mit „Host“ beginnen. Die Protokolle „function started“, „function executed“ und „function completed“ weisen die Kategorie „Host.Executor“ auf. 

Wenn Sie Protokolle in Ihren Funktionscode schreiben, lautet deren Kategorie „Function“.

### <a name="log-levels"></a>Protokollebenen

Die Azure Functions-Protokollierung enthält auch einen *Protokolliergrad* für jedes Protokoll. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) ist eine Enumeration, und der Code (eine Zahl) weist auf die relative Bedeutung hin:

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

### <a name="log-configuration-in-hostjson"></a>Protokollkonfiguration in „host.json“

Mit der Datei [host.json](functions-host-json.md) wird konfiguriert, welcher Protokollierungsgrad von einer Funktionen-App an Application Insights gesendet wird. Für jede Kategorie geben Sie zu sendende Mindestprotokollebene an. Es gibt zwei Beispiele: Das erste hat die Zielplattform [Functions Version 2.x-Runtime](functions-versions.md#version-2x) (.NET Core), das zweite bezieht sich auf die Version 1.x-Runtime.

### <a name="version-2x"></a>Version 2.x

Die v2.x-Runtime verwendet jetzt die [.NET Core-Protokollfilterhierarchie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Version 1.x

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

In diesem Beispiel werden die folgenden Regeln eingerichtet:

* Für Protokolle mit der Kategorie `Host.Results` oder `Function` wird nur die Stufe `Error` oder höher an Application Insights gesendet. Protokolle für die Ebene `Warning` und niedriger werden ignoriert.
* Für Protokolle mit der Kategorie `Host.Aggregator` werden alle Protokolle an Application Insights gesendet. Die Protokollebene `Trace` entspricht dem, was in einigen Protokollierungen als `Verbose` bezeichnet wird. In der [host.json](functions-host-json.md)-Datei soll jedoch `Trace` verwendet werden.
* Für alle anderen Protokolle soll nur die Ebene `Information` und höher an Application Insights gesendet werden.

Der Kategoriewert in [host.json](functions-host-json.md) steuert die Protokollierung für alle Kategorien, die mit dem gleichen Wert beginnen. `Host` in [host.json](functions-host-json.md) steuert die Protokollierung für `Host.General`, `Host.Executor`, `Host.Results` usw.

Wenn [host.json](functions-host-json.md) mehrere Kategorien enthält, die mit der gleichen Zeichenfolge beginnen, werden zuerst die längeren abgeglichen. Nehmen Sie an, Sie möchten alles für die Runtime mit Ausnahme von `Host.Aggregator` auf der Ebene `Error` protokollieren, während `Host.Aggregator` auf der Ebene `Information` protokolliert werden soll:

### <a name="version-2x"></a>Version 2.x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Version 1.x 

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

Alle diese Protokolle werden auf der Stufe `Information` geschrieben. Wenn Sie nach `Warning` oder höheren Stufen filtern, finden Sie keine dieser Daten.

### <a name="category-hostaggregator"></a>Kategorie „Host.Aggregator“

Diese Protokolle stellen die Anzahl und Durchschnittswerte von Funktionsaufrufen für einen [konfigurierbaren](#configure-the-aggregator) Zeitraum dar. Der Standardzeitraum beträgt 30 Sekunden oder 1.000 Ergebnisse, je nachdem, was früher eintritt. 

Die Protokolle sind in Application Insights in der Tabelle **customMetrics** verfügbar. Beispiele hierfür sind die Ausführungsanzahl, die Erfolgsrate und die Dauer.

![customMetrics-Abfrage](media/functions-monitoring/custom-metrics-query.png)

Alle diese Protokolle werden auf der Stufe `Information` geschrieben. Wenn Sie nach `Warning` oder höheren Stufen filtern, finden Sie keine dieser Daten.

### <a name="other-categories"></a>Weitere Kategorien

Alle Protokolle für andere Kategorien, die nicht bereits aufgeführt sind, werden in Application Insights in der Tabelle **traces** angezeigt.

![traces-Abfrage](media/functions-monitoring/analytics-traces.png)

Alle Protokolle mit Kategorien, die mit `Host` beginnen, werden von der Functions-Laufzeit geschrieben. Die Protokolle „Function started“ und „Function completed“ verfügen über die Kategorie `Host.Executor`. Bei erfolgreichen Ausführungen haben diese Protokolle die Stufe `Information`. Ausnahmen werden mit der Stufe `Error` protokolliert. Die Laufzeit erstellt auch Protokolle der Ebene `Warning`, z.B. an die Warteschlange für nicht verarbeitete Nachrichten gesendete Warteschlangennachrichten.

Protokolle, die von Ihrem Funktionscode geschrieben werden, verfügen über die Kategorie `Function` und können jede Protokollstufe aufweisen.

## <a name="configure-the-aggregator"></a>Konfigurieren des Aggregators

Wie im vorherigen Abschnitt erwähnt, werden von der Laufzeit Daten zu den Funktionsausführungen in einem bestimmten Zeitraum aggregiert. Der Standardzeitraum beträgt 30 Sekunden oder 1.000 Ausführungen, je nachdem, was früher eintritt. Sie können diese Einstellung in der Datei [host.json](functions-host-json.md) konfigurieren.  Hier sehen Sie ein Beispiel:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Konfigurieren des Samplings

Application Insights verfügt über ein [Sampling](../azure-monitor/app/sampling.md)-Feature als Schutz davor, dass bei Spitzenlast zu viele Telemetriedaten produziert werden. Wenn die Rate der eingehenden Telemetrieelemente einen bestimmten Schwellenwert übersteigt, beginnt Application Insights, einige der eingehenden Elemente zufällig zu ignorieren. Die Standardeinstellung für die maximale Anzahl von Elementen pro Sekunde ist 5. Sie können das Sampling in der Datei [host.json](functions-host-json.md) konfigurieren.  Hier sehen Sie ein Beispiel:

### <a name="version-2x"></a>Version 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 5
      }
    }
  }
}
```

### <a name="version-1x"></a>Version 1.x 

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

> [!NOTE]
> Das [Sampling](../azure-monitor/app/sampling.md) ist standardmäßig aktiviert. Wenn Ihnen Daten zu fehlen scheinen, müssen Sie möglicherweise nur die Sampling-Einstellungen an Ihr spezielles Überwachungsszenario anpassen.

## <a name="write-logs-in-c-functions"></a>Schreiben von Protokollen in C#-Funktionen

Sie können Protokolle in Ihrem Funktionscode schreiben, die in Application Insights als Ablaufverfolgungen angezeigt werden.

### <a name="ilogger"></a>ILogger

Verwenden Sie den Parameter [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) in Ihren Funktionen anstelle des Parameters `TraceWriter`. Protokolle, die mit `TraceWriter` erstellt werden, werden an Application Insights geleitet, aber mit `ILogger` können Sie die [strukturierte Protokollierung](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) durchführen.

Mit einem `ILogger`-Objekt rufen Sie `Log<level>`-[Erweiterungsmethoden in ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) auf, um Protokolle zu erstellen. Mit dem folgenden Code werden Protokolle vom Typ `Information` mit der Kategorie „Function“ geschrieben.

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

Platzhalter werden auf diese Weise verarbeitet, damit Sie die strukturierte Protokollierung durchführen können. Application Insights speichert die Name/Wert-Paare für Parameter und die Nachrichtenzeichenfolge. Das Ergebnis ist, dass die Nachrichtenargumente zu Feldern werden, anhand denen Sie Abfragen durchführen können.

Wenn Ihr Methodenaufruf für die Protokollierung wie im vorherigen Beispiel aussieht, können Sie das Feld `customDimensions.prop__rowKey` abfragen. Durch das Hinzufügen des Präfix `prop__` soll sichergestellt werden, dass es zwischen den Feldern, die von der Runtime hinzugefügt werden, und Feldern, die von Ihrem Funktionscode hinzugefügt werden, nicht zu Konflikten kommt.

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

### <a name="custom-metrics-logging"></a>Protokollieren von benutzerdefinierten Metriken

In C#-Skriptfunktionen können Sie die `LogMetric`-Erweiterungsmethode in `ILogger` verwenden, um in Application Insights benutzerdefinierte Metriken zu erstellen. Hier ist ein Beispiel für einen Methodenaufruf angegeben:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Dieser Code ist eine Alternative zum Aufrufen von `TrackMetric` mithilfe der Application Insights-API für .NET.

## <a name="write-logs-in-javascript-functions"></a>Schreiben von Protokollen in JavaScript-Funktionen

Verwenden Sie in Node.js-Funktionen `context.log` zum Schreiben von Protokollen. Die strukturierte Protokollierung ist nicht aktiviert.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Protokollieren von benutzerdefinierten Metriken

Bei der Ausführung von [Version 1.x](functions-versions.md#creating-1x-apps) der Functions-Runtime können Node.js-Funktionen die `context.log.metric`-Methode verwenden, um in Application Insights benutzerdefinierte Metriken zu erstellen. Diese Methode wird derzeit in Version 2.x nicht unterstützt. Hier ist ein Beispiel für einen Methodenaufruf angegeben:

```javascript
context.log.metric("TestMetric", 1234);
```

Dieser Code ist eine Alternative zum Aufrufen von `trackMetric` mithilfe des Node.js SDK für Application Insights.

## <a name="log-custom-telemetry-in-c-functions"></a>Protokollieren von benutzerdefinierter Telemetrie in C#-Funktionen

Sie können das NuGet-Paket [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) verwenden, um benutzerdefinierte Telemetriedaten an Application Insights zu senden. Das folgenden C#-Beispiel verwendet die [benutzerdefinierte Telemetrie-API](../azure-monitor/app/api-custom-events-metrics.md). Das Beispiel gilt für eine .NET-Klassenbibliothek, aber der Application Insights-Code ist für C#-Skript identisch.

### <a name="version-2x"></a>Version 2.x

Die Version 2.x-Runtime verwendet neue Features in Application Insights-Telemetriedaten, um die Telemetrie automatisch mit dem aktuellen Vorgang zu korrelieren. Es ist nicht erforderlich, für den Vorgang die Felder `Id`, `ParentId` oder `Name` festzulegen.

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        // In Functions v2, TelemetryConfiguration.Active is initialized with the InstrumentationKey
        // from APPINSIGHTS_INSTRUMENTATIONKEY. Creating a default TelemetryClient like this will 
        // automatically use that key for all telemetry. It will also enable telemetry correlation
        // with the current operation.
        // If you require a custom TelemetryConfiguration, create it initially with
        // TelemetryConfiguration.CreateDefault() to include this automatic correlation.
        private static TelemetryClient telemetryClient = new TelemetryClient();

        [FunctionName("HttpTrigger2")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            telemetryClient.TrackMetric(metric);

            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Version 1.x

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
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

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // Correlate all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Vermeiden Sie es, `TrackRequest` oder `StartOperation<RequestTelemetry>` aufzurufen, da in diesem Fall für einen Funktionsaufruf doppelte Anforderungen angezeigt werden.  Mit der Functions-Laufzeit werden Anforderungen automatisch nachverfolgt.

Legen Sie nicht `telemetryClient.Context.Operation.Id` fest. Diese globale Einstellung führt zu falschen Korrelationen, wenn viele Funktionen gleichzeitig ausgeführt werden. Erstellen Sie stattdessen eine neue Telemetrieinstanz (`DependencyTelemetry`, `EventTelemetry`), und ändern Sie die `Context`-Eigenschaft. Übergeben Sie in der Telemetrie-Instanz dann die entsprechende `Track`-Methode `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Durch diese Methode wird sichergestellt, dass die Telemetrie die richtigen Korrelationsdetails für den aktuellen Funktionsaufruf enthält.

## <a name="log-custom-telemetry-in-javascript-functions"></a>Protokollieren von benutzerdefinierter Telemetrie in JavaScript-Funktionen

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

    context.done();
};
```

Mit dem Parameter `tagOverrides` wird die `operation_Id` auf die Aufrufkennung der Funktion festgelegt. Mithilfe dieser Einstellung können Sie die gesamte automatisch generierte und benutzerdefinierte Telemetrie für einen bestimmten Funktionsaufruf korrelieren.

## <a name="known-issues"></a>Bekannte Probleme
<!-- Add a transitional sentence to introduce the section. -->

### <a name="dependencies"></a>Abhängigkeiten

Abhängigkeiten der Funktion von anderen Diensten werden nicht automatisch angezeigt. Sie können benutzerdefinierten Code schreiben, um die Abhängigkeiten anzuzeigen. Dies wird beispielsweise im Beispielcode im [Abschnitt zu den benutzerdefinierten C#-Telemetriedaten](#log-custom-telemetry-in-c-functions) veranschaulicht. Der Beispielcode ergibt in Application Insights eine *Anwendungszuordnung*, die wie die folgende Abbildung aussieht:

![Anwendungszuordnung](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Melden von Problemen

[Erstellen Sie auf GitHub einen Problemeintrag](https://github.com/Azure/Azure-Functions/issues/new), um ein Problem mit der Application Insights-Integration in Functions zu melden oder einen Vorschlag oder eine Anfrage zu erstellen.

## <a name="monitor-without-application-insights"></a>Überwachen ohne Application Insights

Wir empfehlen Application Insights zum Überwachen von Funktionen. Es bietet mehr Daten und bessere Möglichkeiten zur Analyse der Daten. Wenn Sie jedoch das integrierte Protokollierungssystem bevorzugen, in dem Azure Storage verwendet wird, können Sie diese Methode beibehalten.

### <a name="azure-storage-account-for-logging"></a>Azure Storage-Konto zur Protokollierung

Für die integrierte Protokollierung wird das Speicherkonto verwendet, das über die Verbindungszeichenfolge in der App-Einstellung `AzureWebJobsDashboard` angegeben wird. Wählen Sie auf der Seite einer Funktions-App eine Funktion und anschließend die Registerkarte **Überwachen** aus, und behalten Sie die **klassische Ansicht** bei.

![Zur klassischen Ansicht wechseln](media/functions-monitoring/switch-to-classic-view.png)

Eine Liste der Funktionsausführungen wird angezeigt. Wählen Sie eine Funktionsausführung aus, um die Dauer, Eingabedaten, Fehler und zugehörigen Protokolldateien zu überprüfen.

Wenn Sie Application Insights aktiviert haben, können Sie zur integrierten Protokollierung zurückkehren. Deaktivieren Sie Application Insights manuell, und wählen sie dann die Registerkarte **Überwachen** aus. Zum Deaktivieren der Application Insights-Integration löschen Sie die App-Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY`.

Auch wenn auf der Registerkarte **Überwachen** Application Insights-Daten angezeigt werden, können Sie Protokolldaten im Dateisystem sehen, wenn Sie [die integrierte Protokollierung nicht deaktiviert](#disable-built-in-logging). Wechseln Sie in der Storage-Ressource zu **Dateien**, und wählen Sie den Dateidienst für die Funktion aus. Navigieren Sie dann zu **LogFiles** > **Anwendung** > **Funktionen** > **Funktion** > **Ihre_Funktion**, um die Protokolldatei anzuzeigen.

### <a name="real-time-monitoring"></a>Überwachung in Echtzeit

Sie können Protokolldateien in eine Befehlszeilensitzung auf einer lokalen Arbeitsstation streamen. Verwenden Sie die [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/install-azure-cli) oder [Azure PowerShell](/powershell/azure/overview).  

Verwenden Sie für die Azure CLI die folgenden Befehle, um sich anzumelden, Ihr Abonnement auszuwählen und Protokolldateien zu streamen:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Verwenden Sie für Azure PowerShell die folgenden Befehle, um Ihr Azure-Konto hinzuzufügen, Ihr Abonnement auszuwählen und Protokolldateien zu streamen:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <function app name> -Tail
```

Weitere Informationen finden Sie unter [Vorgehensweise: Streaming von Protokollen](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core-Protokollierung](/aspnet/core/fundamentals/logging/)

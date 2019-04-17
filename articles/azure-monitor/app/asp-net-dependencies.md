---
title: Abhängigkeitsnachverfolgung in Azure Application Insights | Microsoft Docs
description: Analysieren Sie die Auslastung, Verfügbarkeit und Leistung Ihrer lokalen oder Microsoft Azure-Webanwendung mit Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbullwin
ms.openlocfilehash: c77b5810164aef7508f717a0f75d90cf6cba2089
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273106"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Einrichten von Application Insights: Abhängigkeitsüberwachung
Eine *Abhängigkeit* ist eine externe Komponente, die von Ihrer App aufgerufen wird. In der Regel handelt es sich um einen Dienst, der über HTTP oder eine Datenbank oder ein Dateisystem aufgerufen wird. [Application Insights](../../azure-monitor/app/app-insights-overview.md) misst, wie lange die Anwendung auf Abhängigkeiten wartet, und wie oft ein Abhängigkeitsaufruf nicht funktioniert. Sie können bestimmte Aufrufe untersuchen, und diese mit Anforderungen und Ausnahmen in Verbindung bringen.

Der standardmäßig verfügbare Abhängigkeitsmonitor meldet derzeit Aufrufe an diese Abhängigkeitstypen:

* Server
  * SQL-Datenbanken
  * ASP.NET-Web- und WCF-Dienste, die HTTP-basierte Bindungen verwenden
  * Lokale oder Remote-HTTP-Aufrufe
  * Azure Cosmos DB, Tabelle, Blob Storage und Warteschlange 
* Webseiten
  * AJAX-Aufrufe

Überwachung funktioniert unter Verwendung der [Bytecodeinstrumentierung](https://msdn.microsoft.com/library/z9z62c29.aspx) mit ausgewählten Methoden, oder basierend auf DiagnosticSource-Rückrufen (in den neuesten .NET SDKs) aus dem .NET Framework. Der Leistungsaufwand ist minimal.

Mit [TrackDependency-API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)können Sie auch eigene SDK-Aufrufe zum Überwachen anderer Abhängigkeiten schreiben, sowohl in Client- als auch Servercode.

> [!NOTE]
> Azure Cosmos DB wird nur dann automatisch nachverfolgt, wenn [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) verwendet wird. Der TCP-Modus wird von Application Insights nicht erfasst.

## <a name="set-up-dependency-monitoring"></a>Einrichten der Abhängigkeitsüberwachung
Teilinformationen von Abhängigkeiten werden automatisch durch das [Application Insights-SDK](asp-net.md) gesammelt. Installieren Sie den entsprechenden Agent für den Hostserver, um umfassende Daten zu erhalten.

| Plattform | Installieren |
| --- | --- |
| IIS-Server |[Installieren Sie den Statusmonitor auf dem Server,](../../azure-monitor/app/monitor-performance-live-website-now.md), oder [aktualisieren Sie Ihre Anwendung auf .NET Framework 4.6 oder höher](https://go.microsoft.com/fwlink/?LinkId=528259), und installieren Sie das [Application Insights SDK](asp-net.md) in Ihrer App. |
| Azure-Web-App |[Öffnen Sie das Blatt „Application Insights“ in der Systemsteuerung Ihrer Web-App](../../azure-monitor/app/azure-web-apps.md) und wählen Sie bei Aufforderung „Installieren“ aus. |
| Azure Cloud Service |[Verwenden Sie die Startaufgabe](../../azure-monitor/app/cloudservices.md), oder [Installieren Sie .NET Framework 4.6 oder höher](../../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Hier finden Sie Abhängigkeitsdaten
* [Anwendungszuordnung](#application-map) visualisiert Abhängigkeiten zwischen Ihrer App und angrenzenden Komponenten.
* [Die Blätter „Performance“ (Leistung) „Browser“ und „Failure“ (Fehler)](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-performance) zeigen Serverabhängigkeitsdaten.
* [Das Blatt „Browser“](#ajax-calls) zeigt AJAX-Aufrufe von Browsern Ihrer Benutzer.
* Navigieren Sie zu langsamen oder fehlgeschlagenen Aufrufen, um ihre Abhängigkeitsaufrufe zu überprüfen.
* [Analyse](#analytics) kann verwendet werden, um Abhängigkeitsdaten abzufragen.

## <a name="application-map"></a>Anwendungszuordnung
Die Anwendungszuordnung dient als visuelle Hilfe zum Erkennen von Abhängigkeiten zwischen den Komponenten Ihrer Anwendung. Sie wird automatisch aus der Telemetrie Ihrer App generiert. Dieses Beispiel zeigt die AJAX-Aufrufe aus den Browserskripts und REST-Aufrufe aus der Serveranwendung zu zwei externen Diensten.

![Anwendungszuordnung](./media/asp-net-dependencies/cloud-rolename.png)

* **Navigieren Sie aus den Feldern** zur relevanten Abhängigkeit und zu anderen Diagrammen.
* **Heften Sie die Zuordnung** an das [Dashboard](../../azure-monitor/app/app-insights-dashboards.md) an, wo sie voll funktionsfähig sein wird.

[Weitere Informationen](../../azure-monitor/app/app-map.md)

## <a name="performance-and-failure-blades"></a>Blätter „Performance“ und „Failure“
Das Blatt „Performance“ zeigt die Dauer von Abhängigkeitsaufrufen, die durch die Serveranwendung durchgeführt werden.

**Die Fehleranzahl** wird auf dem Blatt **Ausfälle** angezeigt. Ein Fehler ist jeder Rückgabecode, der nicht im Bereich 200-399 liegt, oder unbekannt ist.

> [!NOTE]
> **100 % Fehler?** - Dies liegt wahrscheinlich daran, dass Sie nur teilweise Abhängigkeitsdaten erhalten. Sie müssen [die für Ihre Plattform geeignete Abhängigkeitsüberwachung einrichten](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-Aufrufe
Das Blatt „Browser“ zeigt die Dauer und Fehlerquote von AJAX-Aufrufen durch [JavaScript auf Ihren Webseiten](../../azure-monitor/app/javascript.md) an. Sie werden als Abhängigkeiten angezeigt.

## <a name="diagnosis"></a>Diagnostizieren langsamer Anforderungen
Jedes Anforderungsereignis bezieht sich auf Abhängigkeitsaufrufe, Ausnahmen und andere Ereignisse, die nachverfolgt werden, während Ihre App die Anforderung verarbeitet. Wenn einige Anforderungen also eine schlechte Leistung zeigen, können Sie herausfinden, ob es an langsamen Antworten einer Abhängigkeit liegt.

### <a name="profile-your-live-site"></a>Erstellen eines Profils Ihrer Livewebsite

Sie möchten wissen, was am längsten gedauert hat? Der [Application Insights-Profiler](../../azure-monitor/app/profiler.md) verfolgt HTTP-Aufrufe Ihrer Livewebsite und zeigt an, welche Funktionen im Code die meiste Zeit in Anspruch genommen haben.

## <a name="analytics"></a>Analytics
Sie können Abhängigkeiten in der [Abfragesprache Kusto](/azure/kusto/query/) verfolgen. Hier einige Beispiele.

* Suchen fehlgeschlagener Abhängigkeitsaufrufe:

```

    dependencies | where success != "True" | take 10
```

* Suchen von AJAX-Aufrufen:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Suchen von mit Anforderungen verbundenen Abhängigkeitsaufrufen:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Suchen von mit Seitenaufrufen verbundenen AJAX-Aufrufen:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Benutzerdefinierte Nachverfolgung von Abhängigkeiten
Das Standardmodul für die Nachverfolgung von Abhängigkeiten ermittelt automatisch externe Abhängigkeiten wie Datenbanken und REST-APIs. Es kann jedoch gewünscht sein, einige zusätzliche Komponenten auf die gleiche Weise zu behandeln.

Sie können Code schreiben, der Abhängigkeitsinformationen unter Verwendung der gleichen [TrackDependency-API](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency) sendet, die von den Standardmodulen verwendet wird.

Beispiel: Wenn Sie Ihren Code mit einer Assembly erstellen, die Sie nicht selbst geschrieben haben, könnten Sie die Zeit aller Aufrufe ermitteln, um herauszufinden, welchen Beitrag sie an Ihren Reaktionszeiten hat. Um diese Daten in den Abhängigkeitsdiagrammen in Application Insights anzuzeigen, senden Sie sie mit `TrackDependency`.

```csharp

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
                // The call above has been made obsolete in the latest SDK. The updated call follows this format:
                // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
            }
```

Wenn Sie das Standardmodul für die Nachverfolgung von Abhängigkeiten deaktivieren möchten, entfernen Sie den Verweis auf "DependencyTrackingTelemetryModule" in [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Problembehandlung
*Das Erfolgsflag für die Abhängigkeit zeigt immer entweder TRUE oder FALSE.*

*SQL-Abfrage, die nicht vollständig angezeigt wird.*

Sehen Sie in der Tabelle unten nach, und stellen Sie sicher, dass Sie die richtige Konfiguration ausgewählt haben, um die Abhängigkeitsüberwachung für Ihre Anwendung zu aktivieren.

| Plattform | Installieren |
| --- | --- |
| IIS-Server |Entweder [Installieren Sie Statusmonitor auf Ihrem Server](../../azure-monitor/app/monitor-performance-live-website-now.md), oder [aktualisieren Sie Ihre Anwendung auf .NET Framework 4.6 oder höher](https://go.microsoft.com/fwlink/?LinkId=528259), und installieren Sie das [Application Insights SDK](asp-net.md) in Ihrer App. |
| IIS Express |Verwenden Sie stattdessen IIS-Server. |
| Azure-Web-App |[Öffnen Sie das Blatt „Application Insights“ in der Systemsteuerung Ihrer Web-App](../../azure-monitor/app/azure-web-apps.md) und wählen Sie bei Aufforderung „Installieren“ aus. |
| Azure Cloud Service |[Verwenden Sie die Startaufgabe](../../azure-monitor/app/cloudservices.md), oder [Installieren Sie .NET Framework 4.6 oder höher](../../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="next-steps"></a>Nächste Schritte
* [Ausnahmen](../../azure-monitor/app/asp-net-exceptions.md)
* [Daten zu Seiten und Benutzern](../../azure-monitor/app/javascript.md)
* [Verfügbarkeit](../../azure-monitor/app/monitor-web-app-availability.md)

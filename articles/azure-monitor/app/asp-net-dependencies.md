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
ms.openlocfilehash: 7cde9fa0849729ba9db9b604dd0ad26cf362a484
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969861"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Einrichten von Application Insights: Abhängigkeitsüberwachung
Eine *Abhängigkeit* ist eine externe Komponente, die von Ihrer App aufgerufen wird. In der Regel handelt es sich um einen Dienst, der über HTTP oder eine Datenbank oder ein Dateisystem aufgerufen wird. [Application Insights](../../application-insights/app-insights-overview.md) misst, wie lange die Anwendung auf Abhängigkeiten wartet, und wie oft ein Abhängigkeitsaufruf nicht funktioniert. Sie können bestimmte Aufrufe untersuchen, und diese mit Anforderungen und Ausnahmen in Verbindung bringen.

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
* [Die Blätter „Performance“ (Leistung) „Browser“ und „Failure“ (Fehler)](#performance-and-blades) zeigen Serverabhängigkeitsdaten.
* [Das Blatt „Browser“](#ajax-calls) zeigt AJAX-Aufrufe von Browsern Ihrer Benutzer.
* [Navigieren Sie zu langsamen oder fehlgeschlagenen Aufrufen](#diagnose-slow-requests), um ihre Abhängigkeitsaufrufe zu überprüfen.
* [Analyse](#analytics) kann verwendet werden, um Abhängigkeitsdaten abzufragen.

## <a name="application-map"></a>Anwendungszuordnung
Die Anwendungszuordnung dient als visuelle Hilfe zum Erkennen von Abhängigkeiten zwischen den Komponenten Ihrer Anwendung. Sie wird automatisch aus der Telemetrie Ihrer App generiert. Dieses Beispiel zeigt die AJAX-Aufrufe aus den Browserskripts und REST-Aufrufe aus der Serveranwendung zu zwei externen Diensten.

![Anwendungszuordnung](./media/asp-net-dependencies/08.png)

* **Navigieren Sie aus den Feldern** zur relevanten Abhängigkeit und zu anderen Diagrammen.
* **Heften Sie die Zuordnung** an das [Dashboard](../../azure-monitor/app/app-insights-dashboards.md) an, wo sie voll funktionsfähig sein wird.

[Weitere Informationen](../../azure-monitor/app/app-map.md)

## <a name="performance-and-failure-blades"></a>Blätter „Performance“ und „Failure“
Das Blatt „Performance“ zeigt die Dauer von Abhängigkeitsaufrufen, die durch die Serveranwendung durchgeführt werden. Es gibt ein zusammenfassendes Diagramm und eine nach Aufrufen unterteilte Tabelle.

![Abhängigkeitsdiagramme Blatt „Performance“](./media/asp-net-dependencies/dependencies-in-performance-blade.png)

Klicken Sie sich durch die Abhängigkeitsdiagramme oder die Tabellenelemente, um nach Rohvorkommen dieser Aufrufe zu suchen.

![Instanzen Abhängigkeitsaufrufe](./media/asp-net-dependencies/dependency-call-instance.png)

**Die Fehleranzahl** wird auf dem Blatt **Ausfälle** angezeigt. Ein Fehler ist jeder Rückgabecode, der nicht im Bereich 200-399 liegt, oder unbekannt ist.

> [!NOTE]
> **100 % Fehler?** - Dies liegt wahrscheinlich daran, dass Sie nur teilweise Abhängigkeitsdaten erhalten. Sie müssen [die für Ihre Plattform geeignete Abhängigkeitsüberwachung einrichten](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-Aufrufe
Das Blatt „Browser“ zeigt die Dauer und Fehlerquote von AJAX-Aufrufen durch [JavaScript auf Ihren Webseiten](../../azure-monitor/app/javascript.md) an. Sie werden als Abhängigkeiten angezeigt.

## <a name="diagnosis"></a>Diagnostizieren langsamer Anforderungen
Jedes Anforderungsereignis bezieht sich auf Abhängigkeitsaufrufe, Ausnahmen und andere Ereignisse, die nachverfolgt werden, während Ihre App die Anforderung verarbeitet. Wenn einige Anforderungen also eine schlechte Leistung zeigen, können Sie herausfinden, ob es an langsamen Antworten einer Abhängigkeit liegt.

Wir sehen uns nun ein Beispiel dazu an.

### <a name="tracing-from-requests-to-dependencies"></a>Ablaufverfolgung von Anforderungen bis Abhängigkeiten
Öffnen Sie das Blatt „Performance“, und betrachten Sie das Raster der Anforderungen:

![Liste der Anforderungen mit Mittelwerten und Anzahlen](./media/asp-net-dependencies/02-reqs.png)

Die zuerst aufgeführte Instanz dauert sehr lange. Mal sehen, ob wir herausfinden, wo die Zeit beansprucht wird.

Klicken Sie auf diese Zeile, um einzelne Anforderungsereignisse anzuzeigen:

![Liste der Anforderungsinstanzen](./media/asp-net-dependencies/03-instances.png)

Klicken Sie auf eine beliebige Instanz mit langer Ausführungsdauer, um diese näher zu überprüfen, und scrollen Sie nach unten zu den Remoteabhängigkeitsaufrufen, die im Zusammenhang mit dieser Anforderung bestehen:

![Finden von Aufrufen von Remoteabhängigkeiten, Identifizieren ungewöhnlich langer Laufzeiten](./media/asp-net-dependencies/04-dependencies.png)

Der Großteil der Zeit zur Verarbeitung dieser Anforderung wurde für einen Aufruf eines lokalen Diensts aufgewendet.

Wählen Sie diese Zeile aus, um weitere Informationen zu erhalten:

![Klicken Sie sich durch diese Remoteabhängigkeit, um die Ursache herauszufinden](./media/asp-net-dependencies/05-detail.png)

Anscheinend befindet sich das Problem hier. Nachdem das Problem jetzt identifiziert ist, müssen wir noch herausfinden, warum dieser Aufruf so lange dauert.

### <a name="request-timeline"></a>Anfordern der Zeitachse
In einem anderen Fall handelt es sich nicht um einen Abhängigkeitsaufruf mit langer Ausführungsdauer. Wenn wir aber zur Zeitachsenansicht wechseln, können wir sehen, wo die Verzögerung in unserer internen Verarbeitung aufgetreten ist:

![Finden von Aufrufen von Remoteabhängigkeiten, Identifizieren ungewöhnlich langer Laufzeiten](./media/asp-net-dependencies/04-1.png)

Es scheint eine große Unterbrechung nach dem ersten Abhängigkeitsaufruf zu geben, daher sollten wir den Code betrachten, um den Grund dafür herauszufinden.

### <a name="profile-your-live-site"></a>Erstellen eines Profils Ihrer Livewebsite

Sie möchten wissen, was am längsten gedauert hat? Der [Application Insights-Profiler](../../application-insights/app-insights-profiler.md) verfolgt HTTP-Aufrufe zu Ihrer Livewebsite zurück, und zeigt an, welche Funktionen im Code die meiste Zeit in Anspruch genommen haben.

## <a name="failed-requests"></a>Anforderungsfehler
Anforderungsfehler können auch fehlgeschlagenen Aufrufen von Abhängigkeiten zugeordnet werden. Wir können erneut bis zum Problem durchklicken.

![Klicken Sie auf das Diagramm mit Anforderungsfehlern](./media/asp-net-dependencies/06-fail.png)

Klicken Sie auf einen Anforderungsfehler, und sehen Sie sich die zugeordneten Ereignisse an.

![Klicken Sie auf den Anforderungstyp, klicken Sie auf die Instanz, um eine andere Ansicht derselben Instanz abzurufen, und klicken Sie darauf, um Details zur Ausnahme zu erhalten.](./media/asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analytics
Sie können Abhängigkeiten in der [Log Analytics-Abfragesprache](https://aka.ms/LogAnalyticsLanguage) nachverfolgen. Hier einige Beispiele.

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

*SQL-Abfrage, die nicht vollständig angezeigt wird*

Sehen Sie in der Tabelle unten nach, und stellen Sie sicher, dass Sie die richtige Konfiguration ausgewählt haben, um die Abhängigkeitsüberwachung für Ihre Anwendung zu aktivieren.

| Plattform | Installieren |
| --- | --- |
| IIS-Server |Entweder [Installieren Sie Statusmonitor auf Ihrem Server](../../azure-monitor/app/monitor-performance-live-website-now.md), oder [aktualisieren Sie Ihre Anwendung auf .NET Framework 4.6 oder höher](https://go.microsoft.com/fwlink/?LinkId=528259), und installieren Sie das [Application Insights SDK](asp-net.md) in Ihrer App. |
| IIS Express |Verwenden Sie stattdessen IIS-Server. |
| Azure-Web-App |[Öffnen Sie das Blatt „Application Insights“ in der Systemsteuerung Ihrer Web-App](../../azure-monitor/app/azure-web-apps.md) und wählen Sie bei Aufforderung „Installieren“ aus. |
| Azure Cloud Service |[Verwenden Sie die Startaufgabe](../../azure-monitor/app/cloudservices.md), oder [Installieren Sie .NET Framework 4.6 oder höher](../../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nächste Schritte
* [Ausnahmen](../../azure-monitor/app/asp-net-exceptions.md)
* [Daten zu Seiten und Benutzern](../../azure-monitor/app/javascript.md)
* [Availability](../../azure-monitor/app/monitor-web-app-availability.md)

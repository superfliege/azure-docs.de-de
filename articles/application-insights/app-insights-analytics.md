---
title: "Analytics: Das leistungsfähige Such- und Abfragetool von Azure Application Insights | Microsoft-Dokumentation"
description: "Übersicht über Analytics, das leistungsfähige Diagnosesuchtool von Application Insights. "
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 80a9e248ca50c11ef61a5c50c4986c4f8f4ead9d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="analytics-in-application-insights"></a>Analytics in Application Insights
Analytics ist das leistungsfähige Such- und Abfragetool von [Application Insights](app-insights-overview.md). Analytics ist ein Webtool, sodass keine Installation erforderlich ist. Wenn Sie Application Insights bereits für eine Ihrer Apps konfiguriert haben, können Sie die Daten Ihrer App analysieren, indem Sie Analytics vom [Übersichtsblatt](app-insights-dashboards.md) Ihrer App aus öffnen.

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics/001.png)

Sie können auch den [Analytics-Playground](https://go.microsoft.com/fwlink/?linkid=859557) verwenden, eine kostenlose Demo-Umgebung mit einer Vielzahl von Beispieldaten.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Abfragen von Daten in Analytics
Eine typische Abfrage beginnt mit einem Tabellennamen, gefolgt von einer Reihe von *Operatoren*, die durch `|` getrennt sind.
Wir möchten z.B. herausfinden, wie viele Anforderungen unsere App während der letzten 3 Stunden aus verschiedenen Ländern empfangen hat:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

Wir beginnen mit dem Tabellennamen *requests* und fügen nach Bedarf weitergeleitete Elemente hinzu.  Zuerst definieren wir einen Zeitfilter, um nur Datensätze aus den letzten 3 Stunden zu überprüfen.
Wir zählen dann die Anzahl der Datensätze pro Land (diese Daten befinden sich in der Spalte *client_CountryOrRegion*). Schließlich werden die Ergebnisse in einem Kreisdiagramm ausgegeben.
<br>

![Abfrageergebnisse](./media/app-insights-analytics/030.png)

Die Sprache verfügt über viele attraktive Features:

* [Filtern](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) der Rohdaten Ihrer App-Telemetrie nach beliebigen Feldern, einschließlich Ihrer benutzerdefinierten Eigenschaften und Metriken.
* [Verbinden](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) mehrerer Tabellen – Korrelation von Anforderungen mit Seitenansichten, Aufrufen von Abhängigkeiten, Ausnahmen und Protokollablaufverfolgungen.
* Leistungsstarke statistische [Aggregationen](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Sofortige und leistungsfähige Visualisierungen.
* Eine [REST-API](https://dev.applicationinsights.io/), die Sie verwenden können, um Abfragen programmgesteuert auszuführen, z.B. über PowerShell.

Die [vollständige Sprachreferenz](https://go.microsoft.com/fwlink/?linkid=856079) erläutert jeden unterstützten Befehl und wird regelmäßig aktualisiert.

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit dem [Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587)
* Erste Schritte mit dem [Schreiben von Abfragen](https://go.microsoft.com/fwlink/?linkid=856078)
* Überprüfen Sie, ob der [Spickzettel für SQL-Benutzer](https://aka.ms/sql-analytics) Übersetzungen der am häufigsten verwendeten Begriffe enthält.
* Testen Sie Analytics auf unserem [Playground](https://analytics.applicationinsights.io/demo), wenn Ihre App noch keine Daten an Application Insights sendet.
* Sehen Sie sich das [Einführungsvideo](https://applicationanalytics-media.azureedge.net/home_page_video.mp4) an.
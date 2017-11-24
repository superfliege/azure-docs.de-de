---
title: Teilen von benutzerdefinierten Azure Time Series Insights-Ansichten mit parametrisierten URLs | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie parametrisierte URLs in Azure Time Series Insights entwickeln, damit eine Kundenansicht schnell geteilt werden kann.
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: MarkMcGeeAtAquent
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: rest-api
ms.topic: get-started-article
ms.workload: big-data
ms.date: 11/21/2017
ms.openlocfilehash: ac48969a9166080384dccf606f0401a82016a60a
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Teilen einer benutzerdefinierten Ansicht mit einer parametrisierten URL

Sie können programmgesteuert eine parametrisierte URL der benutzerdefinierten Ansicht erstellen, um im Time Series Insights-Explorer eine benutzerdefinierte Ansicht zu teilen.

Der Time Series Insights-Explorer unterstützt URL-Abfrageparameter, um Ansichten auf der Benutzeroberfläche direkt über die URL anzugeben.  Indem Sie nur die URL verwenden, können Sie beispielsweise eine Zielumgebung, ein Suchprädikat und eine gewünschte Zeitspanne angeben. Wenn ein Benutzer auf die benutzerdefinierte URL klickt, stellt die Oberfläche einen direkten Link zu diesem Objekt im Time Series Insights-Portal bereit.  Es gelten die Richtlinien für den Datenzugriff. 

## <a name="environment-id"></a>Umgebungs-ID

Mit dem Parameter `environmentId=<guid>` wird die ID der Zielumgebung angegeben.  Dies ist eine Komponente des FQDN für den Datenzugriff. Sie finden sie im Azure-Portal oben rechts in der Umgebungsübersicht.  Es ist die gesamte Angabe, die vor `env.timeseries.azure.com` steht. Ein Beispielparameter für die Umgebungs-ID ist `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Zeit

Sie können mit einer parametrisierten URL absolute oder relative Zeitwerte angeben.

### <a name="absolute-time-values"></a>Absolute Zeitwerte

Verwenden Sie für absolute Zeitwerte die Parameter `from=<integer>` und `to=<integer>`. 

`from=<integer>` ist ein Wert in JavaScript-Millisekunden für die Startzeit der Suchzeitspanne.

`to=<integer>` ist ein Wert in JavaScript-Millisekunden für die Endzeit der Suchzeitspanne. 

Informationen zum Identifizieren der JavaScript-Millisekunden für ein Datum finden Sie unter [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html) (Epochen/Unix-Zeitstempelkonvertierung).

### <a name="relative-time-values"></a>Relative Zeitwerte

Verwenden Sie für einen relativen Zeitwert `relativeMillis=<value>`, wobei *value* für einen Wert in JavaScript-Millisekunden der letzten Daten auf dem Back-End steht.

Für `&relativeMillis=3600000` werden beispielsweise die Daten der letzten 60 Minuten angezeigt.

Die akzeptierten Werte entsprechen dem Menü **quick time** (Zeitauswahl) im Time Series Insights-Explorer und umfassen Folgendes:

- 1800000 (letzte 30 Minuten)
- 3600000 (letzte 60 Minuten)
- 10800000 (letzte 3 Stunden)
- 21600000 (letzte 6 Stunden)
- 43200000 (letzte 12 Stunden)
- 86400000 (letzte 24 Stunden)
- 604800000 (letzte 7 Tage)
- 2592000000 (letzte 30 Stunden)

### <a name="optional-parameters"></a>Optionale Parameter

Mit dem Parameter `timeSeriesDefinitions=<collection of term objects>` werden die Ausdrücke einer Time Series Insights-Ansicht angegeben:

- `name=<string>`
  - Der Name der *Bedingung*.
- `splitBy=<string>`
  - Der Spaltenname für *Teilen nach*.
- `measureName=<string>`
  - Der Spaltenname des *Measure*.
- `predicate=<string>`
  - Die *where*-Klausel für die serverseitige Filterung.

 
### <a name="examples"></a>Beispiele

Sie können beispielsweise Folgendes verwenden, um Zeitreihendefinitionen als URL-Parameter hinzuzufügen:

```https
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Verwenden Sie diese Zeitreihendefinitionen: 

- Umgebungs-ID
- Daten der letzten 60 Minuten
- Bedingungen (F1PressureID, F2TempStation und F3VibrationPL), aus denen optionale Parameter bestehen
 
Mit diesen Elementen können Sie die folgende parametrisierte URL für eine Ansicht erstellen:

```https
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Wenn Sie den Time Series Insights-Explorer zum Erstellen der mit der obigen URL beschriebenen Ansicht verwenden, wird Folgendes angezeigt:

![Ausdrücke im Time Series Insights-Explorer](media/parameterized-url/url1.png)

Die vollständige Ansicht (einschließlich des Diagramms) sieht dann wie folgt aus:

![Diagrammansicht](media/parameterized-url/url2.png)

## <a name="next-steps"></a>Nächste Schritte
[Abfragen von Daten mithilfe von C#](time-series-insights-query-data-csharp.md)

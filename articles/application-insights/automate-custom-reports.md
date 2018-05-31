---
title: Automatisieren von benutzerdefinierten Berichten mit Azure Application Insights-Daten
description: Automatisieren von benutzerdefinierten täglichen, wöchentlichen oder monatlichen Berichten mit Azure Application Insights-Daten
services: application-insights
documentationcenter: ''
author: sdash
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: sdash
ms.openlocfilehash: 804e8c7a43d1ab16d11b6075be44599b33b46a3e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34072657"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Automatisieren von benutzerdefinierten Berichten mit Azure Application Insights-Daten

Regelmäßig erstellte Berichte halten ein Team darüber im Laufenden, wie sich ihre unternehmenskritischen Dienste entwickeln. Entwickler, DevOps/SRE-Teams und deren Vorgesetzte können so produktiv arbeiten, indem automatisierte Berichte zuverlässig Informationen bereitstellen, ohne dass sich Benutzer beim Portal anmelden müssen. Derartige Berichte können auch dabei unterstützen, allmähliche Erhöhungen bei Latenzen, Lasten oder Fehlerraten zu ermitteln, die möglicherweise keine Warnungsregeln auslösen.

Jedes Unternehmen stellt einzigartige Berichterstellungsanforderungen wie etwa Folgende: 

* Aggregationen von Metriken mit einem bestimmten Quantil oder benutzerdefinierte Metriken in einem Bericht
* Verschiedene Berichte für tägliche, wöchentliche und monatliche Rollups von Daten für verschiedene Zielgruppen
* Segmentierung durch benutzerdefinierte Attribute wie Region oder Umgebung 
* Gruppierung einiger KI-Ressourcen in einem einzelnen Bericht, auch wenn sie eventuell in verschiedenen Abonnements oder Ressourcengruppen usw. enthalten sind
* Separate Berichte mit vertraulichen Metriken, die an ausgewählte Zielgruppe gesendet werden
* Berichte für Projektbeteiligte, die womöglich keinen Zugriff auf die Ressourcen des Portals haben

> [!NOTE] 
> Bei der Application Insights-E-Mail mit der wöchentlichen Zusammenfassung waren keine Anpassungen möglich, weshalb diese zugunsten der unten aufgeführten benutzerdefinierten Optionen eingestellt wird. Die letzte E-Mail mit wöchentlicher Zusammenfassung wird am 11. Juni 2018 versendet. Konfigurieren Sie eine der folgenden Optionen, um ähnliche benutzerdefinierte Berichte zu erhalten (verwenden Sie die unten vorgeschlagene Abfrage).

## <a name="to-automate-custom-report-emails"></a>Automatisieren von benutzerdefinierten Berichts-E-Mails

[Application Insights-Daten können programmgesteuert abgefragt werden](https://dev.applicationinsights.io/), um benutzerdefinierte Berichte nach einem Zeitplan zu generieren. Die folgenden Optionen ermöglichen Ihnen einen schnellen Einstieg:

* [Berichte mit Microsoft Flow automatisieren](app-insights-automate-with-flow.md)
* [Berichte mit Logic Apps automatisieren](automate-with-logic-apps.md)
* Verwenden Sie im Überwachungsszenario die [Azure-Funktionsvorlage](https://azure.microsoft.com/services/functions/) für „geplante Application Insights-Zusammenfassungen“. Bei dieser Funktion werden E-Mails über SendGrid übermittelt. 

    ![Auswählen der Azure-Funktionsvorlage](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Beispielabfrage für eine E-Mail mit wöchentlicher Zusammenfassung
Die folgende Abfrage zeigt, wie mehrere Datasets für eine E-Mail mit wöchentlicher Zusammenfassung, wie etwa bei einem Bericht, verknüpft werden können. Passen Sie sie nach Bedarf an, und verwenden Sie sie mit einer der oben aufgeführten Optionen zur Automatisierung eines wöchentlichen Berichts.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

  
## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](app-insights-analytics-using.md).
- Erfahren Sie mehr über das [programmgesteuerte Abfragen von Application Insights-Daten](https://dev.applicationinsights.io/).
- Informieren Sie sich ausführlicher über [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).



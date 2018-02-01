---
title: "Problembehandlung für Nutzungsanalysen in Azure Application Insights"
description: "Leitfaden zur Problembehandlung – Analysieren von Website- und Appnutzung mithilfe von Application Insights."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/16/2018
ms.author: mbullwin
ms.openlocfilehash: cb5f3052301b23eb10cd6b84ab6fae98bcc7ea18
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-usage-analytics-in-application-insights"></a>Problembehandlung für Nutzungsanalysen in Application Insights
Haben Sie Fragen zu den [Tools für Nutzungsanalysen in Application Insights](app-insights-usage-overview.md): [Benutzer, Sitzungen, Ereignisse](app-insights-usage-segmentation.md), [Trichter](usage-funnels.md), [Benutzerabläufe](app-insights-usage-flows.md), [Vermerkdauer](app-insights-usage-retention.md) oder Kohorten? Hier finden Sie ein paar Antworten.

## <a name="counting-users"></a>Zählen von Benutzern
**Das Tool zur Nutzungsanalyse zeigt an, dass meine App einen Benutzer/eine Sitzung aufweist, ich weiß aber, dass es viele Benutzer/Sitzungen sind. Wie kann ich diesen falschen Zahlenwert korrigieren?**

Alle Telemetrieereignisse in Application Insights weisen als Standardeigenschaften eine [anonyme Benutzer-ID](application-insights-data-model-context.md) und eine [Sitzungs-ID](application-insights-data-model-context.md) auf. Standardmäßig zählen alle Tools zur Nutzungsanalyse die Benutzer und Sitzungen auf der Grundlage dieser IDs. Wenn diese Standardeigenschaften nicht mit eindeutigen IDs für jeden Benutzer und jede Sitzung Ihrer App aufgefüllt werden, wird in den Tools zur Nutzungsanalyse eine falsche Anzahl für Benutzer und Sitzungen angezeigt.

Wenn Sie eine Web-App überwachen, besteht die einfachste Lösung darin, Ihrer App das [Application Insights JavaScript-SDK](app-insights-javascript.md) hinzuzufügen und dafür zu sorgen, dass der Skriptcodeausschnitt auf jeder Seite geladen wird, die Sie überwachen möchten. Das JavaScript-SDK generiert automatisch anonyme Benutzer- und Sitzungs-IDs und füllt die Telemetrieereignisse dann automatisch mit diesen IDs auf, wenn sie von Ihrer App gesendet werden.

Wenn Sie einen Webdienst (keine Benutzeroberfläche) überwachen, [erstellen Sie einen Telemetrieinitialisierer, der die Eigenschaften „anonyme Benutzer-ID“ und „Sitzungs-ID“](app-insights-usage-send-user-context.md) entsprechend den Auffassungen von eindeutigen Benutzern und Sitzungen Ihres Diensts auffüllt.

Wenn Ihre App [authentifizierte Benutzer-IDs](app-insights-api-custom-events-metrics.md#authenticated-users) sendet, können Sie anhand von authentifizierten Benutzer-IDs im Tool Benutzer zählen. Wählen Sie in der Dropdownliste „Anzeigen“ „Authentifizierte Benutzer“ aus.

Die Tools zur Nutzungsanalyse unterstützen aktuell das Zählen von Benutzern oder Sitzungen nur auf der Grundlage der Eigenschaften „anonyme Benutzer-ID“, „authentifizierte Benutzer-ID“ oder „Sitzungs-ID“.

## <a name="naming-events"></a>Benennen von Ereignissen
**Meine App weist Tausende verschiedener Namen für Seitenansichten und benutzerdefinierte Ereignisse auf. Sie sind nur schwer zu unterscheiden, und die Tools zur Nutzungsanalyse zeigen häufig keine Reaktion. Wie kann ich diese Benennungsprobleme beheben?**

Namen für Seitenansichten und benutzerdefinierte Ereignisse werden überall in den Tools zur Nutzungsanalyse verwendet. Die gute Benennung von Ereignissen ist entscheidend dafür, Werte aus diesen Tools abzurufen. Das Ziel ist ein Gleichgewicht zwischen zu wenigen, übermäßig allgemeinen Namen („Klick auf Schaltfläche“) und zu vielen, übermäßig spezifischen Namen („Klick auf Schaltfläche ‚Bearbeiten‘ auf http://www.contoso.com/index“).

Um Änderungen an den Namen von Seitenansichten und benutzerdefinierten Ereignissen vorzunehmen, die von Ihrer App gesendet werden, müssen Sie den Quellcode Ihrer App ändern und sie erneut bereitstellen. **Alle Telemetriedaten in Application Insights werden 90 Tage lang gespeichert und können nicht gelöscht werden**, sodass Änderungen, die Sie an Ereignisnamen vornehmen, erst nach 90 Tagen vollständig umgesetzt sind. Für die 90 Tage nach dem Vornehmen einer Änderung treten in Ihrer Telemetrie sowohl die alten als auch die neuen Namen auf, also passen Sie die Abfragen entsprechend an und kommunizieren Sie dies in Ihren Teams.

Wenn Ihre App zu viele Namen von Seitenansichten sendet, überprüfen Sie, ob diese Namen von Seitenansichten manuell im Code angegeben sind oder automatisch vom Application Insights JavaScript-SDK gesendet werden:

* Wenn die Namen von Seitenansichten manuell mithilfe der [`trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md) im Code angegeben wurden, ändern Sie die Namen in weniger spezifische. Vermeiden Sie häufige Fehler, wie das Einsetzen der URLs in die Namen von Seitenansichten. Verwenden Sie stattdessen den URL-Parameter der `trackPageView` API. Verschieben Sie andere Details aus dem Namen der Seitenansicht in benutzerdefinierte Eigenschaften.

* Wenn das Application Insights JavaScript-SDK Namen von Seitenansichten automatisch sendet, können Sie entweder die Titel Ihrer Seiten ändern oder auf das manuelle Senden der Namen von Seitenansichten umsteigen. Das SDK sendet standardmäßig den [Titel](https://developer.mozilla.org/docs/Web/HTML/Element/title) jeder Seite als Namen der Seitenansicht. Sie können Ihre Titel in allgemeinere ändern, gehen Sie jedoch umsichtig vor, da diese Änderung sich u.a. auf SEO auswirken kann. Beim manuellen Angeben von Namen von Seitenansichten mithilfe der `trackPageView`-API werden die automatisch gesammelten Namen überschrieben, sodass Sie in der Telemetrie allgemeinere Namen senden können, ohne die Seitentitel zu ändern.   

Wenn Ihre App zu viele Namen von benutzerdefinierten Ereignissen sendet, ändern Sie den Namen im Code in einen weniger spezifischen. Vermeiden Sie auch hier das direkte Einfügen von URLs und sonstigen seitenbezogenen oder dynamischen Informationen in die Namen von benutzerdefinierten Ereignissen. Verschieben Sie stattdessen mithilfe der `trackEvent`-API diese Details in benutzerdefinierte Eigenschaften des benutzerdefinierten Ereignisses. Beispielsweise schlagen wir anstelle von `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")` etwas in der Art von `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })` vor.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Nutzungsanalyse](app-insights-usage-overview.md)

## <a name="get-help"></a>Hier erhalten Sie Hilfe
* [Stapelüberlauf](http://stackoverflow.com/questions/tagged/ms-application-insights)


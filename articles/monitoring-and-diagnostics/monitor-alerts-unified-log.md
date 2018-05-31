---
title: Protokollwarnungen in Azure Monitor – Warnungen | Microsoft-Dokumentation
description: Lösen Sie E-Mails, Benachrichtigungen, den Aufruf von Website-URLs (Webhooks) oder eine Automatisierung aus, wenn die von Ihnen angegebenen analytischen Abfragebedingungen für Azure-Warnungen erfüllt sind.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 8bf534177e8236a7d72d6dfdd4612b5f6f492b17
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057320"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Protokollwarnungen in Azure Monitor – Warnungen 
Dieser Artikel enthält Details zu Protokollwarnungen. Dies ist einer der Typen von Warnungen, die im Rahmen der neuen [Azure-Warnungen](monitoring-overview-unified-alerts.md) unterstützt werden. Sie ermöglichen es Benutzern, die Analyseplattform von Azure als Basis für die Bereitstellung von Warnungen zu verwenden. Details zu Metrikwarnungen, die Protokolle verwenden, finden Sie unter [Metrikwarnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md).


Protokollwarnungen umfassen Regeln für die Protokollsuche, die für [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) oder [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events) erstellt werden.


## <a name="log-search-alert-rule---definition-and-types"></a>Warnungsregel für Protokollsuche – Definition und Typen

Regeln für die Protokollsuche werden von Azure-Warnungen erstellt, um in regelmäßigen Abständen automatisch angegebene Protokollabfragen auszuführen.  Wenn die Ergebnisse der Protokollabfrage bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt. Die Regel kann mithilfe von [Aktionsgruppen](monitoring-action-groups.md) dann automatisch eine oder mehrere Aktionen durchführen. 

Regeln für die Protokollsuche werden anhand der folgenden Details definiert:
- **Protokollabfrage**:  Die Abfrage, die bei jeder Auslösung der Warnungsregel ausgeführt wird.  Mit den von dieser Abfrage zurückgegebenen Datensätzen wird ermittelt, ob eine Warnung erstellt werden muss. Eine *Azure Application Insights*-Abfrage kann auch [anwendungsübergreifende Aufrufe](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) umfassen, sofern der Benutzer über Zugriffsrechte für die externen Anwendungen verfügt. 

    > [!IMPORTANT]
    > Da sich die Unterstützung von [anwendungsübergreifenden Abfragen für Application Insights](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) in der Vorschauphase befindet, können sich Funktionen und die Benutzeroberfläche also noch ändern. Die Verwendung von [übergreifenden Abfragen für Arbeitsbereiche](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) und [ressourcenübergreifenden Abfragen für Log Analytics](../log-analytics/log-analytics-cross-workspace-search.md) wird für Azure-Warnungen derzeit **nicht unterstützt**.

- **Zeitraum**:  Gibt den Zeitraum für die Abfrage an. Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden. Mit dem Zeitraum werden die Daten eingeschränkt, die für eine Protokollabfrage abgerufen werden, um Missbrauch zu verhindern, und alle in einer Protokollabfrage verwendeten Zeitbefehle (z.B. „ago“) werden umgangen. <br>*Wenn der Zeitraum beispielsweise auf 60 Minuten festgelegt ist und die Abfrage um 13:15 Uhr ausgeführt wird, werden nur Datensätze zurückgegeben, die zwischen 12:15 und 13:15 Uhr erstellt wurden, um die Protokollabfrage auszuführen. Falls für die Protokollabfrage ein Zeitbefehl wie „ago (7d)“ verwendet wird, wird die Protokollabfrage nur für Daten zwischen 12:15 und 13:15 Uhr ausgeführt, als ob nur Daten für die letzten 60 Minuten vorhanden wären. Und nicht Daten für sieben Tage, wie in der Protokollabfrage angegeben.*
- **Häufigkeit**:  Gibt an, wie oft die Abfrage ausgeführt werden soll. Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Er sollte kleiner als oder gleich dem Zeitraumwert sein.  Wenn der Wert größer als der Zeitraum ist, besteht das Risiko, dass Datensätze ausgelassen werden.<br>*Angenommen, Sie verwenden einen Zeitraum von 30 Minuten und eine Häufigkeit von 60 Minuten.  Wenn die Abfrage um 13:00 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 12:30 und 13:00 Uhr zurück.  Wenn die Abfrage dann das nächste Mal um 14 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 13:30 und 14:00 Uhr zurück.  Alle Datensätze, die zwischen 13:00 und 13:30 erstellt werden, werden also nicht ausgewertet.*
- **Schwellenwert**:  Die Ergebnisse der Protokollsuche werden ausgewertet, um festzustellen, ob eine Warnung generiert werden soll.  Der Schwellenwert ist für verschiedene Typen von Warnungsregeln der Protokollsuche unterschiedlich.

Regeln für die Protokollsuche – sowohl für [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) als auch für [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events) – können einen von zwei Typen aufweisen. Diese Typen werden in den nachstehenden Abschnitten ausführlich beschrieben.

- **[Anzahl von Ergebnissen](#number-of-results-alert-rules)**. Einzelne Warnung, die generiert wird, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze einen angegebenen Wert überschreitet.
- **[Metrische Maßeinheit](#metric-measurement-alert-rules)**.  Warnung, die für jedes Objekt in den Ergebnissen der Protokollsuche bei Werten generiert wird, die den angegebenen Schwellenwert überschreiten.

Die Unterschiede zwischen den Warnungsregeltypen sind wie folgt.

- Warnungsregeln vom Typ *Anzahl von Ergebnissen* erstellen stets eine einzelne Warnung, während die Warnungsregel *Metrische Maßeinheit* eine Warnung für jedes Objekt erzeugt, das den Schwellenwert überschreitet.
- Warnungsregeln vom Typ *Anzahl von Ergebnissen* erzeugen eine Warnung, wenn der Schwellenwert ein einziges Mal überschritten wird. Warnungsregeln des Typs *Metrische Maßeinheit* können eine Warnung generieren, wenn der Schwellenwert in einem bestimmten Zeitintervall mit einer bestimmten Häufigkeit überschritten wird.

### <a name="number-of-results-alert-rules"></a>Warnungsregeln des Typs „Anzahl von Ergebnissen“
Warnungsregeln des Typs **Anzahl von Ergebnissen** erzeugen eine einzige Warnung, wenn die von der Suchabfrage zurückgegebene Anzahl von Datensätze den angegebenen Schwellenwert überschreitet. Diese Art von Warnungsregel eignet sich optimal für Ereignisse wie Windows-Ereignisprotokolle, Syslog, WebApp-Antwort und benutzerdefinierte Protokolle.  Es kann ratsam sein, eine Warnung zu erstellen, wenn ein bestimmtes Fehlerereignis erstellt wird oder wenn mehrere Fehlerereignisse innerhalb eines bestimmten Zeitraums erstellt werden.

**Schwellenwert**: Der Schwellenwert für Warnungsregeln vom Typ „Anzahl von Ergebnissen“ ist größer oder kleiner als ein bestimmter Wert.  Eine Warnung wird erstellt, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze dieses Kriterium erfüllt.

Wenn eine Warnung für ein einzelnes Ereignis generiert werden soll, legen Sie die Anzahl von Ergebnissen auf größer Null fest, und überprüfen Sie, ob seit der letzten Ausführung der Abfrage ein einzelnes Ereignis aufgetreten ist. Für einige Anwendungen wird unter Umständen gelegentlich ein Fehler protokolliert, für den nicht unbedingt eine Warnung ausgelöst werden muss.  Es kann beispielsweise sein, dass die Anwendung versucht, den Vorgang mit dem Fehler erneut durchzuführen, und dass der Vorgang dann erfolgreich ist.  In diesem Fall sollten Sie die Warnung ggf. nur erstellen, wenn mehrere Ereignisse innerhalb eines bestimmten Zeitraums erstellt werden.  

Es kann auch vorkommen, dass Sie eine Warnung erstellen möchten, ohne dass ein entsprechendes Ereignis vorliegt.  Für einen Prozess können beispielsweise regelmäßig Ereignisse protokolliert werden, um anzugeben, dass er richtig funktioniert.  Wenn innerhalb eines bestimmten Zeitraums nicht eines dieser Ereignisse protokolliert wird, sollte eine Warnung erstellt werden.  In diesem Fall sollten Sie den Schwellenwert auf **Kleiner als 1** festlegen.

#### <a name="example"></a>Beispiel
Angenommen, Sie möchten wissen, wann Ihre webbasierte App einem Benutzer eine Antwort mit dem Code 500 (interner Fehler) zurückgibt. Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  
- **Abfrage:** requests | where resultCode == "500"<br>
- **Zeitraum:** 30 Minuten<br>
- **Warnungshäufigkeit**: Fünf Minuten<br>
- **Schwellenwert**: Größer Null<br>

In diesem Fall wird die Abfrage alle fünf Minuten mit Daten für 30 Minuten ausgeführt, um nach Datensätzen mit dem Ergebniscode 500 zu suchen. Sobald ein solcher Datensatz gefunden wird, werden die Warnung und die konfigurierte Aktion ausgelöst.

### <a name="metric-measurement-alert-rules"></a>Warnungsregeln des Typs „Metrische Maßeinheit“

- Warnungsregeln des Typs **Metrische Maßeinheit** erzeugen eine Warnung für jedes Objekt in einer Abfrage mit einem Wert, der einen angegebenen Schwellenwert überschreitet.  Sie weisen gegenüber Warnungsregeln des Typs **Anzahl von Ergebnissen** die folgenden Unterschiede auf.
- **Aggregatfunktion**: Bestimmt die zu erfolgende Berechnung und möglicherweise ein numerisches zu aggregierendes Feld.  Beispielsweise gibt **count()** die Anzahl der Datensätze in der Abfrage zurück, während **avg(CounterValue)** den Durchschnitt des Felds „CounterValue“ in diesem Intervall zurückgibt. Die Aggregatfunktion in der Abfrage muss als „AggregatedValue“ benannt und aufgerufen werden und einen numerischen Wert bereitstellen. 
- **Gruppenfeld**: Ein Datensatz mit einem aggregierten Wert wird für jede Instanz dieses Felds erstellt, und für jede kann eine Warnung generiert werden.  Wenn Sie beispielsweise eine Warnung für jeden Computer generieren möchten, wählen Sie **Computer**. 

    > [!NOTE]
    > Bei Warnungsregeln für Metrikmessungen, die auf Application Insights basieren, können Sie das Feld für die Gruppierung der Daten angeben. Verwenden Sie zu diesem Zweck die Option **Aggregieren auf** in der Regeldefinition.   
    
- **Intervall**: Definiert das Zeitintervall, über das die Daten aggregiert werden.  Bei Angabe von **Fünf Minuten** wird beispielsweise ein Datensatz für jede Instanz des Gruppenfelds erstellt, das für den für die Warnung angegebenen Zeitraum in 5-Minuten-Intervallen aggregiert wird.

    > [!NOTE]
    > Die Funktion „bin“ muss in der Abfrage zum Angeben des Intervalls verwendet werden. Da „bin()“ zu ungleichen Zeitintervallen führen kann, wird der Befehl „bin“ für die Warnung zur Laufzeit in den Befehl „bin_at“ mit der richtigen Zeit konvertiert, um Ergebnisse mit Fixpunkt sicherzustellen.
    
- **Schwellenwert**: Der Schwellenwert für Warnungsregeln des Typs „Metrische Maßeinheit“ wird mittels eines Aggregatwerts und einer Anzahl von Verstößen definiert.  Wenn bei der Protokollsuche ein Datenpunkt diesen Wert überschreitet, gilt dies als Verstoß.  Wenn die Anzahl der Verstöße für ein beliebiges Objekt in den Ergebnissen den angegebenen Wert überschreitet, wird eine Warnung für dieses Objekt generiert.

#### <a name="example"></a>Beispiel
Angenommen, Sie wünschen sich eine Warnung, wenn ein beliebiger Computer binnen 30 Minuten dreimal die Prozessornutzung von 90 % überschreitet.  Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  

- **Abfrage:** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5 m), Computer<br>
- **Zeitraum:** 30 Minuten<br>
- **Warnungshäufigkeit**: Fünf Minuten<br>
- **Aggregatwert**: Größer als 90<br>
- **Warnung auslösen basierend auf**: Gesamtanzahl von Verstößen größer als 2<br>

Die Abfrage ermittelt einen Durchschnittswert für jeden Computer in 5-Minuten-Intervallen.  Diese Abfrage wird alle 5 Minuten für die in den letzten 30 Minuten gesammelten Daten ausgeführt.  Nachstehend sehen Sie Beispieldaten für drei Computer.

![Ergebnisse der Beispielabfrage](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

Bei diesem Beispiel werden separate Warnungen für srv02 und srv03 erstellt, da diese Computer im Zeitraum dreimal gegen den Schwellenwert 90% verstoßen haben.  Wenn **Warnung auslösen basierend auf** in **Aufeinander folgend** geändert wird, wird nur für srv03 eine Warnung generiert, da dieser bei drei aufeinander folgenden Stichproben gegen den Schwellenwert verstoßen hat.


## <a name="log-search-alert-rule---creation-and-modification"></a>Warnungsregel für Protokollsuche – Erstellung und Änderung

Sie können die Protokollwarnung und die dazugehörige Warnungsregel für die Protokollsuche hier anzeigen, erstellen oder ändern:
- Azure-Portal
- REST-APIs (einschließlich PowerShell)
- Azure Resource Manager-Vorlagen

### <a name="azure-portal"></a>Azure-Portal
Seit Einführung der [neuen Azure-Warnungen](monitoring-overview-unified-alerts.md) können Benutzer jetzt alle Arten von Warnungen zentral im Azure-Portal und mit ähnlichen Schritten verwalten. Informieren Sie sich über die [Verwendung der neuen Azure-Warnungen](monitor-alerts-unified-usage.md).

Benutzer können ihre Abfragen zudem in der Analytics-Plattform ihrer Wahl in Azure optimieren und anschließend *die Abfrage speichern, um sie in Warnungen zu importieren*. Vorgehensweise:
- *Für Application Insights*: Überprüfen Sie die Abfrage und die dazugehörigen Ergebnisse im Analytics-Portal. Speichern Sie sie dann unter einem eindeutigen Namen in *Freigegebene Abfragen*.
- *Für Log Analytics*: Überprüfen Sie die Abfrage und die dazugehörigen Ergebnisse in der Protokollsuche. Speichern Sie sie dann unter einem eindeutigen Namen in einer beliebigen Kategorie.

Die gespeicherte Warnung wird daraufhin beim [Erstellen einer Protokollwarnung in Warnungen](monitor-alerts-unified-usage.md) als Signaltyp **Protokoll (gespeicherte Abfrage)** aufgelistet, wie im folgenden Beispiel zu sehen: ![In Warnungen importierte gespeicherte Abfrage](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Die Verwendung von **Protokoll (gespeicherte Abfrage)** hat einen Import in Warnungen zur Folge. Dies bedeutet, dass jegliche Änderung, die danach in Analytics vorgenommen wird, in den Warnungsregeln der Protokollsuche nicht berücksichtigt wird (und umgekehrt).

### <a name="rest-apis"></a>REST-APIs
Für Protokollwarnungen bereitgestellte APIs sind „RESTful“, und es kann über die Azure Resource Manager-REST-API darauf zugegriffen werden. Daher ist auch der Zugriff per PowerShell und andere Optionen möglich, um die APIs zu nutzen.

Ausführliche Informationen und Beispiele für die Verwendung der REST-API finden Sie unter:
- [Erstellen und Verwalten von Warnungsregeln in Log Analytics mithilfe der REST-API](../log-analytics/log-analytics-api-alerts.md): Enthält eine Beschreibung, wie Sie Warnungsregeln der Protokollsuche für Azure Log Analytics erstellen und verwalten.
- [Azure Monitor Scheduled Query Rules REST API](https://docs.microsoft.com/en-us/rest/api/monitorr/scheduledqueryrules/) (Azure Monitor-Regeln für geplante Abfragen – REST-API): Enthält eine Beschreibung, wie Sie Warnungsregeln der Protokollsuche für Azure Application Insights erstellen und verwalten.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
Benutzer können auch die Flexibilität von [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nutzen, um Ressourcen zu erstellen und zu aktualisieren, die für die Erstellung oder Aktualisierung von Protokollwarnungen dienen.

Ausführliche Informationen und Beispiele zur Verwendung von Resource Manager-Vorlagen finden Sie unter:
- [Verwaltung gespeicherter Suchen und Warnungen](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) für Protokollwarnungen, die auf Azure Log Analytics basieren
- [Geplante Abfrageregel](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) für Protokollwarnungen basierend auf Azure Application Insights
 

## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit [Protokollwarnungen in Azure](monitor-alerts-unified-log-webhook.md) vertraut.
* Informieren Sie sich über die neuen [Azure-Warnungen](monitoring-overview-unified-alerts.md).
* Erfahren Sie mehr über [Application Insights](../application-insights/app-insights-analytics.md).
* Erfahren Sie mehr über [Log Analytics](../log-analytics/log-analytics-overview.md).    

---
title: Protokollwarnungen in Azure Monitor
description: Lösen Sie E-Mails, Benachrichtigungen, den Aufruf von Website-URLs (Webhooks) oder eine Automatisierung aus, wenn die von Ihnen angegebenen analytischen Abfragebedingungen für Azure-Warnungen erfüllt sind.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: b3949fefac1cc230a98687b3b5ff9c7a01c6a0e9
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789575"
---
# <a name="log-alerts-in-azure-monitor"></a>Protokollwarnungen in Azure Monitor
Dieser Artikel enthält Details zu Protokollwarnungen. Dies ist einer der Typen von Warnungen, die im Rahmen der [Azure-Warnungen](../../azure-monitor/platform/alerts-overview.md) unterstützt werden. Sie ermöglichen es Benutzern, die Analyseplattform von Azure als Basis für die Bereitstellung von Warnungen zu verwenden.

Protokollwarnungen umfassen Regeln für die Protokollsuche, die für [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) oder [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostic-events) erstellt werden. Weitere Informationen zur Verwendung finden Sie unter [Erstellen von Protokollwarnungen in Azure](../../azure-monitor/platform/alerts-log.md).

> [!NOTE]
> Gängige Protokolldaten aus [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) sind nun auch auf der Metrikplattform in Azure Monitor verfügbar. Eine Detailansicht finden Sie unter [Metrikwarnung für Protokolle](../../azure-monitor/platform/alerts-metric-logs.md).


## <a name="log-search-alert-rule---definition-and-types"></a>Warnungsregel für Protokollsuche – Definition und Typen

Regeln für die Protokollsuche werden von Azure-Warnungen erstellt, um in regelmäßigen Abständen automatisch angegebene Protokollabfragen auszuführen.  Wenn die Ergebnisse der Protokollabfrage bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt. Die Regel kann mithilfe von [Aktionsgruppen](../../azure-monitor/platform/action-groups.md) dann automatisch eine oder mehrere Aktionen durchführen. 

Regeln für die Protokollsuche werden anhand der folgenden Details definiert:
- **Protokollabfrage**:  Die Abfrage, die bei jeder Auslösung der Warnungsregel ausgeführt wird.  Mit den von dieser Abfrage zurückgegebenen Datensätzen wird ermittelt, ob eine Warnung erstellt werden muss. Eine Analytics-Abfrage kann auch [anwendungsübergreifende Aufrufe](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), arbeitsbereichsübergreifende Aufrufe und [ressourcenübergreifende Aufrufe](../../azure-monitor/log-query/cross-workspace-query.md) umfassen, sofern der Benutzer über Zugriffsrechte für die externen Anwendungen verfügt. 

    > [!IMPORTANT]
    > Der Benutzer muss über die Rolle [Mitwirkender der Azure-Überwachung](../../azure-monitor/platform/roles-permissions-security.md) zum Erstellen, Ändern und Aktualisieren von Protokollwarnungen in Azure Monitor sowie über Zugriffs- und Abfrageausführungsrechte für die Analyseziele in der Warnungsregel oder Warnungsabfrage verfügen. Wenn der erstellende Benutzer nicht Zugriff auf alle Analyseziele in der Warnungsregel oder Warnungsabfrage hat, kann bei der Regelerstellung ein Fehler auftreten oder die Protokollwarnungsregel wird mit Teilergebnissen ausgeführt.

- **Zeitraum**:  Gibt den Zeitraum für die Abfrage an. Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden. Mit dem Zeitraum werden die Daten eingeschränkt, die für eine Protokollabfrage abgerufen werden, um Missbrauch zu verhindern, und alle in einer Protokollabfrage verwendeten Zeitbefehle (z.B. „ago“) werden umgangen. <br>*Wenn der Zeitraum beispielsweise auf 60 Minuten festgelegt ist und die Abfrage um 13:15 Uhr ausgeführt wird, werden nur Datensätze zurückgegeben, die zwischen 12:15 und 13:15 Uhr erstellt wurden, um die Protokollabfrage auszuführen. Falls für die Protokollabfrage ein Zeitbefehl wie „ago (7d)“ verwendet wird, wird die Protokollabfrage nur für Daten zwischen 12:15 und 13:15 Uhr ausgeführt, als ob nur Daten für die letzten 60 Minuten vorhanden wären. Und nicht Daten für sieben Tage, wie in der Protokollabfrage angegeben.*
- **Häufigkeit**:  Gibt an, wie oft die Abfrage ausgeführt werden soll. Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Er sollte kleiner als oder gleich dem Zeitraumwert sein.  Wenn der Wert größer als der Zeitraum ist, besteht das Risiko, dass Datensätze ausgelassen werden.<br>*Angenommen, Sie verwenden einen Zeitraum von 30 Minuten und eine Häufigkeit von 60 Minuten.  Wenn die Abfrage um 13:00 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 12:30 und 13:00 Uhr zurück.  Wenn die Abfrage dann das nächste Mal um 14 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 13:30 und 14:00 Uhr zurück.  Alle Datensätze, die zwischen 13:00 und 13:30 erstellt werden, werden also nicht ausgewertet.*
- **Schwellenwert**:  Die Ergebnisse der Protokollsuche werden ausgewertet, um festzustellen, ob eine Warnung generiert werden soll.  Der Schwellenwert ist für verschiedene Typen von Warnungsregeln der Protokollsuche unterschiedlich.

Regeln für die Protokollsuche – sowohl für [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) als auch für [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostic-events) – können einen von zwei Typen aufweisen. Diese Typen werden in den nachstehenden Abschnitten ausführlich beschrieben.

- **[Anzahl von Ergebnissen](#number-of-results-alert-rules)**. Einzelne Warnung, die generiert wird, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze einen angegebenen Wert überschreitet.
- **[Metrische Maßeinheit](#metric-measurement-alert-rules)**.  Warnung, die für jedes Objekt in den Ergebnissen der Protokollsuche bei Werten generiert wird, die den angegebenen Schwellenwert überschreiten.

Die Unterschiede zwischen den Warnungsregeltypen sind wie folgt.

- Warnungsregeln vom Typ *Anzahl von Ergebnissen* erstellen stets eine einzelne Warnung, während die Warnungsregel *Metrische Maßeinheit* eine Warnung für jedes Objekt erzeugt, das den Schwellenwert überschreitet.
- Warnungsregeln vom Typ *Anzahl von Ergebnissen* erzeugen eine Warnung, wenn der Schwellenwert ein einziges Mal überschritten wird. Warnungsregeln des Typs *Metrische Maßeinheit* können eine Warnung generieren, wenn der Schwellenwert in einem bestimmten Zeitintervall mit einer bestimmten Häufigkeit überschritten wird.

### <a name="number-of-results-alert-rules"></a>Warnungsregeln des Typs „Anzahl von Ergebnissen“
Warnungsregeln des Typs **Anzahl von Ergebnissen** erzeugen eine einzige Warnung, wenn die von der Suchabfrage zurückgegebene Anzahl von Datensätze den angegebenen Schwellenwert überschreitet. Diese Art von Warnungsregel eignet sich optimal für Ereignisse wie Windows-Ereignisprotokolle, Syslog, WebApp-Antwort und benutzerdefinierte Protokolle.  Es kann ratsam sein, eine Warnung zu erstellen, wenn ein bestimmtes Fehlerereignis erstellt wird oder wenn mehrere Fehlerereignisse innerhalb eines bestimmten Zeitraums erstellt werden.

**Schwellenwert**: Der Schwellenwert für die Warnungsregel „Anzahl von Ergebnissen“ ist größer oder kleiner als ein bestimmter Wert.  Eine Warnung wird erstellt, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze dieses Kriterium erfüllt.

Wenn eine Warnung für ein einzelnes Ereignis generiert werden soll, legen Sie die Anzahl von Ergebnissen auf größer Null fest, und überprüfen Sie, ob seit der letzten Ausführung der Abfrage ein einzelnes Ereignis aufgetreten ist. Für einige Anwendungen wird unter Umständen gelegentlich ein Fehler protokolliert, für den nicht unbedingt eine Warnung ausgelöst werden muss.  Es kann beispielsweise sein, dass die Anwendung versucht, den Vorgang mit dem Fehler erneut durchzuführen, und dass der Vorgang dann erfolgreich ist.  In diesem Fall sollten Sie die Warnung ggf. nur erstellen, wenn mehrere Ereignisse innerhalb eines bestimmten Zeitraums erstellt werden.  

Es kann auch vorkommen, dass Sie eine Warnung erstellen möchten, ohne dass ein entsprechendes Ereignis vorliegt.  Für einen Prozess können beispielsweise regelmäßig Ereignisse protokolliert werden, um anzugeben, dass er richtig funktioniert.  Wenn innerhalb eines bestimmten Zeitraums nicht eines dieser Ereignisse protokolliert wird, sollte eine Warnung erstellt werden.  In diesem Fall sollten Sie den Schwellenwert auf **Kleiner als 1** festlegen.

#### <a name="example-of-number-of-records-type-log-alert"></a>Beispiel für den Protokollwarnungstyp „Anzahl von Datensätze“
Angenommen, Sie möchten wissen, wann Ihre webbasierte App einem Benutzer eine Antwort mit dem Code 500 (interner Fehler) zurückgibt. Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  
- **Abfrage:** requests | where resultCode == "500"<br>
- **Zeitraum:** 30 Minuten<br>
- **Warnungshäufigkeit**: Fünf Minuten<br>
- **Schwellenwert:** Größer als 0<br>

In diesem Fall wird die Abfrage alle fünf Minuten mit Daten für 30 Minuten ausgeführt, um nach Datensätzen mit dem Ergebniscode 500 zu suchen. Sobald ein solcher Datensatz gefunden wird, werden die Warnung und die konfigurierte Aktion ausgelöst.

### <a name="metric-measurement-alert-rules"></a>Warnungsregeln des Typs „Metrische Maßeinheit“

- Warnungsregeln des Typs **Metrische Maßeinheit** erzeugen eine Warnung für jedes Objekt in einer Abfrage mit einem Wert, der einen angegebenen Schwellenwert überschreitet.  Sie weisen gegenüber Warnungsregeln des Typs **Anzahl von Ergebnissen** die folgenden Unterschiede auf.
- **Aggregatfunktion**: Bestimmt die zu erfolgende Berechnung und möglicherweise ein numerisches zu aggregierendes Feld.  Beispielsweise gibt **count()** die Anzahl der Datensätze in der Abfrage zurück, während **avg(CounterValue)** den Durchschnitt des Felds „CounterValue“ in diesem Intervall zurückgibt. Die Aggregatfunktion in der Abfrage muss benannt/aufgerufen werden: AggregatedValue, und einen numerischen Wert bereitstellen. 
- **Gruppierungsfeld**: Ein Datensatz mit einem aggregierten Wert wird für jede Instanz dieses Felds erstellt, und für jede kann eine Warnung generiert werden.  Wenn Sie beispielsweise eine Warnung für jeden Computer generieren möchten, wählen Sie **Computer**. 

    > [!NOTE]
    > Bei Warnungsregeln für Metrikmessungen, die auf Application Insights basieren, können Sie das Feld für die Gruppierung der Daten angeben. Verwenden Sie zu diesem Zweck die Option **Aggregieren auf** in der Regeldefinition.   
    
- **Intervall**:  Definiert das Intervall, über das die Daten aggregiert werden.  Bei Angabe von **Fünf Minuten** wird beispielsweise ein Datensatz für jede Instanz des Gruppenfelds erstellt, das für den für die Warnung angegebenen Zeitraum in 5-Minuten-Intervallen aggregiert wird.

    > [!NOTE]
    > Die Funktion „bin“ muss in der Abfrage zum Angeben des Intervalls verwendet werden. Da „bin()“ zu ungleichen Zeitintervallen führen kann, wird der Befehl „bin“ für die Warnung zur Laufzeit in den Befehl „bin_at“ mit der richtigen Zeit konvertiert, um Ergebnisse mit Fixpunkt sicherzustellen. Der Protokollwarnungstyp „Metrische Maßeinheit“ wurde für die Arbeit mit Abfragen konzipiert, die einen einzelnen bin()-Befehl aufweisen.
    
- **Schwellenwert**: Der Schwellenwert für Warnungsregeln des Typs „Metrische Maßeinheit“ wird mittels eines Aggregatwerts und einer Anzahl von Verstößen definiert.  Wenn bei der Protokollsuche ein Datenpunkt diesen Wert überschreitet, gilt dies als Verstoß.  Wenn die Anzahl der Verstöße für ein beliebiges Objekt in den Ergebnissen den angegebenen Wert überschreitet, wird eine Warnung für dieses Objekt generiert.

#### <a name="example-of-metric-measurement-type-log-alert"></a>Beispiel für den Protokollwarnungstyp „Metrische Maßeinheit“
Angenommen, Sie wünschen sich eine Warnung, wenn ein beliebiger Computer binnen 30 Minuten dreimal die Prozessornutzung von 90 % überschreitet.  Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  

- **Abfrage:** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer<br>
- **Zeitraum:** 30 Minuten<br>
- **Warnungshäufigkeit**: Fünf Minuten<br>
- **Aggregatwert:** Größer als 90<br>
- **Warnung auslösen basierend auf:** Sicherheitsverletzungen gesamt größer als 2<br>

Die Abfrage ermittelt einen Durchschnittswert für jeden Computer in 5-Minuten-Intervallen.  Diese Abfrage wird alle 5 Minuten für die in den letzten 30 Minuten gesammelten Daten ausgeführt.  Nachstehend sehen Sie Beispieldaten für drei Computer.

![Ergebnisse der Beispielabfrage](media/alerts-unified-log/metrics-measurement-sample-graph.png)

Bei diesem Beispiel werden separate Warnungen für srv02 und srv03 erstellt, da diese Computer im Zeitraum dreimal gegen den Schwellenwert 90 % verstoßen haben.  Wenn **Warnung auslösen basierend auf** in **Aufeinander folgend** geändert wird, wird nur für srv03 eine Warnung generiert, da dieser bei drei aufeinander folgenden Stichproben gegen den Schwellenwert verstoßen hat.

## <a name="log-search-alert-rule---firing-and-state"></a>Warnungsregel für die Protokollsuche – Auslösung und Zustand
Die Warnungsregel für die Protokollsuche funktioniert nach der Logik, die vom Benutzer gemäß der Konfiguration und der verwendeten benutzerdefinierten Analyseabfrage festgelegt wurde. Da die Logik der genauen Bedingung, warum die Warnungsregel ausgelöst werden soll, in einer Analytics-Abfrage gekapselt ist – diese kann in jeder Protokollwarnungsregel unterschiedlich sein. Azure-Warnungen verfügt über wenige Informationen über die spezifische zugrunde liegende Ursache innerhalb der Protokollergebnisse, wenn die Schwellenwertbedingung der Warnungsregel der Protokollsuche erfüllt oder überschritten wird. Daher werden Protokollwarnungen als zustandslos bezeichnet und jedes Mal ausgelöst, wenn das Protokollsuchergebnis ausreicht, um den in den Protokollwarnungen angegebenen Schwellenwert der Bedingungsart *Anzahl der Ergebnisse* oder *metrische Maßeinheit* zu überschreiten. Und die Protokollwarnungsregeln werden kontinuierlich weiter ausgelöst, solange die Warnungsbedingung durch das Ergebnis der bereitgestellten benutzerdefinierten Analytics-Abfrage erfüllt wird; ohne dass die Warnung aufgelöst wird. Da die Logik der genauen Grundursache für den Überwachungsfehler innerhalb der vom Benutzer bereitgestellten Analytics-Abfrage maskiert wird, gibt es keine Möglichkeit, mit der Azure-Warnungen abschließend feststellen kann, ob die Lösung des Problems dadurch angezeigt wird, dass das Protokollsuchergebnis den Schwellenwert nicht erreicht.

Nehmen Sie jetzt an, es gibt eine Protokollwarnungsregel namens *Contoso-Log-Alert*, wie in der Konfiguration im bereitgestellten [Beispiel für den Protokollwarnungstyp „Anzahl von Ergebnissen“](#example-of-number-of-records-type-log-alert) angegeben. 
- Um 13:05 Uhr, als „Contoso-Log-Alert“ von Azure-Warnungen ausgeführt wurde, lieferte das Protokollsuchergebnis 0 Datensätze. Dies liegt unterhalb des Schwellenwerts und daher wurde die Warnung nicht ausgelöst. 
- Bei der nächsten Ausführung um 13:10 Uhr, als „Contoso-Log-Alert“ von Azure-Warnungen ausgeführt wurde, lieferte das Protokollsuchergebnis 5 Datensätze. Dadurch wurde der Schwellenwert überschritten und kurz danach die Warnung ausgelöst, indem die zugehörige [Aktionsgruppe](../../azure-monitor/platform/action-groups.md) ausgelöst wurde. 
- Um 13:15 Uhr, als „Contoso-Log-Alert“ von Azure-Warnungen ausgeführt wurde, lieferte das Protokollsuchergebnis 2 Datensätze. Dadurch wurde der Schwellenwert überschritten und kurz danach die Warnung ausgelöst, indem die zugehörige [Aktionsgruppe](../../azure-monitor/platform/action-groups.md) ausgelöst wurde.
- Bei der nächsten Ausführung um 13:20 Uhr, als „Contoso-Log-Alert“ von Azure-Warnungen ausgeführt wurde, lieferte das Protokollsuchergebnis erneut 0 Datensätze. Dies liegt unterhalb des Schwellenwerts und daher wurde die Warnung nicht ausgelöst.

Aber im oben genannten Fall kann Azure-Warnungen um 13:15 Uhr nicht feststellen, dass die zugrundeliegenden Probleme, die um 13:10 Uhr erkannt wurden, bestehen bleiben und ob neue Fehler auftreten. Da die vom Benutzer bereitgestellte Abfrage möglicherweise frühere Datensätze berücksichtigt – kann Azure-Warnungen sicher sein. Um also auf Nummer sicher zu gehen, wird bei der Ausführung von „Contoso-Log-Alert“ um 13:15 Uhr die konfigurierte [Aktionsgruppe](../../azure-monitor/platform/action-groups.md) erneut ausgelöst. Wenn jetzt um 13:20 Uhr keine Datensätze mehr angezeigt werden, kann Azure-Warnungen sicher sein, dass die Ursache der Datensätze behoben wurde. Daher wechselt „Contoso-Log-Alert“ im Azure Alert-Dashboard nicht zu „Gelöst“ und/oder es werden Benachrichtigungen gesendet, um die Lösung einer Warnung anzuzeigen.


## <a name="pricing-and-billing-of-log-alerts"></a>Preise und Abrechnung von Protokollwarnungen
Preise für Protokollwarnungen sind auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/) aufgeführt. In Azure-Rechnungen sind Protokollwarnungen als Typ `microsoft.insights/scheduledqueryrules` wie folgt angegeben:
- Protokollwarnungen für Application Insights werden mit dem genauen Namen der Warnung zusammen mit der Ressourcengruppe und Warnungseigenschaften angezeigt.
- Protokollwarnungen für Log Analytics werden mit dem Namen der Warnung als `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` zusammen mit der Ressourcengruppe und Warnungseigenschaften angezeigt.

    > [!NOTE]
    > Die Namen aller gespeicherten Suchvorgänge, Zeitpläne und Aktionen, die mit der Log Analytics-API erstellt werden, müssen in Kleinbuchstaben geschrieben werden. Wenn ungültige Zeichen wie `<, >, %, &, \, ?, /` verwendet werden, werden sie in der Rechnung durch `_` ersetzt.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Erstellen von Protokollwarnungen in Azure](../../azure-monitor/platform/alerts-log.md).
* Machen Sie sich mit [Webhooks in Protokollwarnungen in Azure](alerts-log-webhook.md) vertraut.
* Erfahren Sie mehr über [Azure-Warnungen](../../azure-monitor/platform/alerts-overview.md).
* Erfahren Sie mehr über [Application Insights](../../azure-monitor/app/analytics.md).
* Erfahren Sie mehr über [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).    

---
title: Protokollwarnungen in Azure Monitor – Warnungen | Microsoft-Dokumentation
description: Lösen Sie E-Mails, Benachrichtigungen, den Aufruf von Website-URLs (Webhooks) oder eine Automatisierung aus, wenn die von Ihnen angegebenen komplexen Abfragebedingungen für Azure-Warnungen erfüllt sind.
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
ms.date: 03/17/2018
ms.author: vinagara
ms.openlocfilehash: 5928bbcec08d6ba4ac0b0d03b66fa4bfc8f5e3d7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Protokollwarnungen in Azure Monitor – Warnungen 
Dieser Artikel enthält Details zur Funktionsweise von Warnungsregeln aus Analytics-Abfragen in Azure-Warnungen und beschreibt die Unterschiede verschiedener Arten von Protokollwarnungsregeln. Details zu Metrikwarnungen, die Protokolle verwenden, finden Sie unter [Metrikwarnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md).

Derzeit unterstützen Azure-Warnungen Protokollwarnungen zu Abfragen von [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) und [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> Protokollwarnungen in Azure-Warnungen unterstützen derzeit keine arbeitsbereichs- oder App-übergreifenden Abfragen. Protokollwarnungen für Application Insights befinden sich in der öffentlichen Vorschauphase; Funktionen und Benutzererfahrung können sich also noch ändern.

Benutzer können ihre Abfragen zudem in der Analytics-Plattform ihrer Wahl in Azure optimieren und anschließend *die Abfrage speichern, um sie in Warnungen zu importieren*. Vorgehensweise:
- Für Application Insights: Überprüfen Sie die Abfrage und ihre Ergebnisse im Analytics-Portal. Speichern Sie sie dann unter einem eindeutigen Namen in *Freigegebene Abfragen*.
- Für Log Analytics: Überprüfen Sie die Abfrage und ihre Ergebnisse in der Protokollsuche. Speichern Sie sie dann unter einem eindeutigen Namen in einer beliebigen Kategorie.

Die gespeicherte Warnung wird daraufhin beim [Erstellen einer Protokollwarnung in Warnungen](monitor-alerts-unified-usage.md) als Signaltyp **Protokoll (gespeicherte Abfrage)** aufgelistet, wie im folgenden Beispiel zu sehen: ![In Warnungen importierte gespeicherte Abfrage](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Die Verwendung von **Protokoll (gespeicherte Abfrage)** hat einen Import in Warnungen zur Folge. Das bedeutet, dass jegliche Änderung, die danach in Analytics vorgenommen wird, in den gespeicherten Warnungsregeln nicht berücksichtigt wird (und umgekehrt).

## <a name="log-alert-rules"></a>Protokollwarnungsregeln

Warnungen werden von Azure-Warnungen erstellt, um in regelmäßigen Abständen automatisch Protokollabfragen auszuführen.  Wenn die Ergebnisse der Protokollabfrage bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt. Die Regel kann dann automatisch Aktionen ausführen, um Sie proaktiv über die Warnung zu informieren oder einen anderen Prozess wie das Senden von Daten an eine externe Anwendung mit [JSON-basiertem Webhook](monitor-alerts-unified-log-webhook.md) unter Verwendung von [Aktionsgruppen](monitoring-action-groups.md) aufzurufen. Verschiedene Typen von Warnungsregeln verwenden für diese Analyse unterschiedliche Logik.

Warnungsregeln werden anhand der folgenden Details definiert:

- **Protokollabfrage**:  Die Abfrage, die bei jeder Auslösung der Warnungsregel ausgeführt wird.  Mit den von dieser Abfrage zurückgegebenen Datensätzen wird ermittelt, ob eine Warnung erstellt werden muss.
- **Zeitfenster**:  Gibt den Zeitraum für die Abfrage an.  Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden.  Das Zeitfenster kann einen beliebigen Wert zwischen 5 Minuten und 1440 Minuten oder 24 Stunden enthalten. Wenn das Zeitfenster beispielsweise auf 60 Minuten festgelegt ist und die Abfrage um 13:15 Uhr ausgeführt wird, werden nur Datensätze zurückgegeben, die zwischen 12:15 und 13:15 Uhr erstellt wurden.
- **Häufigkeit**:  Gibt an, wie oft die Abfrage ausgeführt werden soll. Dies kann ein beliebiger Wert zwischen 5 Minuten und 24 Stunden sein. Er sollte kleiner als oder gleich dem Zeitfensterwert sein.  Wenn der Wert größer als das Zeitfenster ist, besteht das Risiko, dass Datensätze ausgelassen werden.<br>Angenommen, Sie verwenden ein Zeitfenster von 30 Minuten und eine Häufigkeit von 60 Minuten.  Wenn die Abfrage um 13:00 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 12:30 und 13:00 Uhr zurück.  Wenn die Abfrage dann das nächste Mal um 14 Uhr ausgeführt wird, gibt sie die Datensätze für den Zeitraum zwischen 13:30 und 14:00 Uhr zurück.  Alle Datensätze, die zwischen 13:00 und 13:30 erstellt werden, werden also nicht ausgewertet.
- **Schwellenwert**:  Die Ergebnisse der Protokollsuche werden ausgewertet, um festzustellen, ob eine Warnung generiert werden soll.  Der Schwellenwert ist für verschiedene Typen von Warnungsregeln unterschiedlich.

Es gibt zwei Typen von Warnungsregeln in Log Analytics.  Diese Typen werden in den nachstehenden Abschnitten ausführlich beschrieben.

- **[Anzahl von Ergebnissen](#number-of-results-alert-rules)**. Einzelne Warnung, die generiert wird, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze einen angegebenen Wert überschreitet.
- **[Metrische Maßeinheit](#metric-measurement-alert-rules)**.  Warnung, die für jedes Objekt in den Ergebnissen der Protokollsuche bei Werten generiert wird, die den angegebenen Schwellenwert überschreiten.

Die Unterschiede zwischen den Warnungsregeltypen sind wie folgt.

- Warnungsregeln vom Typ „Anzahl von Ergebnissen“ erstellen stets eine einzelne Warnung, während die Warnungsregel **Metrische Maßeinheit** eine Warnung für jedes Objekt erzeugt, das den Schwellenwert überschreitet.
- Warnungsregeln vom Typ **Anzahl von Ergebnissen** erzeugen eine Warnung, wenn der Schwellenwert ein einziges Mal überschritten wird. Warnungsregeln des Typs **Metrische Maßeinheit** können eine Warnung generieren, wenn der Schwellenwert in einem bestimmten Zeitintervall mit einer bestimmten Häufigkeit überschritten wird.

## <a name="number-of-results-alert-rules"></a>Warnungsregeln des Typs „Anzahl von Ergebnissen“
Warnungsregeln des Typs **Anzahl von Ergebnissen** erzeugen eine einzige Warnung, wenn die von der Suchabfrage zurückgegebene Anzahl von Datensätze den angegebenen Schwellenwert überschreitet. Diese Art von Warnungsregel eignet sich optimal für Ereignisse wie Windows-Ereignisprotokolle, Syslog, WebApp-Antwort und benutzerdefinierte Protokolle.  Es kann ratsam sein, eine Warnung zu erstellen, wenn ein bestimmtes Fehlerereignis erstellt wird oder wenn mehrere Fehlerereignisse innerhalb eines bestimmten Zeitfensters erstellt werden.

**Schwellenwert**: Der Schwellenwert für Warnungsregeln vom Typ „Anzahl von Ergebnissen“ ist größer oder kleiner als ein bestimmter Wert.  Eine Warnung wird erstellt, wenn die Anzahl der von der Protokollsuche zurückgegebenen Datensätze dieses Kriterium erfüllt.

Wenn eine Warnung für ein einzelnes Ereignis generiert werden soll, legen Sie die Anzahl von Ergebnissen auf größer Null fest, und überprüfen Sie, ob seit der letzten Ausführung der Abfrage ein einzelnes Ereignis aufgetreten ist. Für einige Anwendungen wird unter Umständen gelegentlich ein Fehler protokolliert, für den nicht unbedingt eine Warnung ausgelöst werden muss.  Es kann beispielsweise sein, dass die Anwendung versucht, den Vorgang mit dem Fehler erneut durchzuführen, und dass der Vorgang dann erfolgreich ist.  In diesem Fall sollten Sie die Warnung ggf. nur erstellen, wenn mehrere Ereignisse innerhalb eines bestimmten Zeitfensters erstellt werden.  

Es kann auch vorkommen, dass Sie eine Warnung erstellen möchten, ohne dass ein entsprechendes Ereignis vorliegt.  Für einen Prozess können beispielsweise regelmäßig Ereignisse protokolliert werden, um anzugeben, dass er richtig funktioniert.  Wenn innerhalb eines bestimmten Zeitfensters nicht eines dieser Ereignisse protokolliert wird, sollte eine Warnung erstellt werden.  In diesem Fall sollten Sie den Schwellenwert auf **Kleiner als 1** festlegen.

### <a name="example"></a>Beispiel
Angenommen, Sie möchten wissen, wann Ihre webbasierte App einem Benutzer eine Antwort mit dem Code 500 (interner Fehler) zurückgibt. Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  
**Abfrage:** requests | where resultCode == "500"<br>
**Zeitfenster**: 30 Minuten<br>
**Warnungshäufigkeit**: Fünf Minuten<br>
**Schwellenwert**: Größer Null<br>

In diesem Fall wird die Abfrage alle fünf Minuten mit Daten für 30 Minuten ausgeführt, um nach Datensätzen mit dem Ergebniscode 500 zu suchen. Sobald ein solcher Datensatz gefunden wird, werden die Warnung und die konfigurierte Aktion ausgelöst.

## <a name="metric-measurement-alert-rules"></a>Warnungsregeln des Typs „Metrische Maßeinheit“

Warnungsregeln des Typs **Metrische Maßeinheit** erzeugen eine Warnung für jedes Objekt in einer Abfrage mit einem Wert, der einen angegebenen Schwellenwert überschreitet.  Sie weisen gegenüber Warnungsregeln des Typs **Anzahl von Ergebnissen** die folgenden Unterschiede auf.

**Aggregatfunktion**: Bestimmt die zu erfolgende Berechnung und möglicherweise ein numerisches zu aggregierendes Feld.  Beispielsweise gibt **count()** die Anzahl der Datensätze in der Abfrage zurück, während **avg(CounterValue)** den Durchschnitt des Felds „CounterValue“ in diesem Intervall zurückgibt.

> [!NOTE]

> Die Aggregatfunktion in der Abfrage muss als „AggregatedValue“ benannt und aufgerufen werden und einen numerischen Wert bereitstellen. 


**Gruppenfeld**: Ein Datensatz mit einem aggregierten Wert wird für jede Instanz dieses Felds erstellt, und für jede kann eine Warnung generiert werden.  Wenn Sie beispielsweise eine Warnung für jeden Computer generieren möchten, wählen Sie **Computer**.   

> [!NOTE]

> Bei Warnungsregeln für Metrikmessungen, die auf Application Insights basieren, können Sie das Feld für die Gruppierung der Daten angeben. Verwenden Sie zu diesem Zweck die Option **Aggregieren auf** in der Regeldefinition.   

**Intervall**: Definiert das Zeitintervall, über das die Daten aggregiert werden.  Bei Angabe von **Fünf Minuten** wird beispielsweise ein Datensatz für jede Instanz des Gruppenfelds erstellt, das für das für die Warnung angegebene Zeitfenster in 5-Minuten-Intervallen aggregiert wird.
> [!NOTE]
> In der Abfrage muss die Funktion „bin“ verwendet werden. Da „bin()“ zu ungleichen Zeitintervallen führen kann, wird stattdessen die Funktion „bin_at“ mit der richtigen Zeit zur Laufzeit verwendet, um Ergebnisse mit Fixpunkt sicherzustellen.

**Schwellenwert**: Der Schwellenwert für Warnungsregeln des Typs „Metrische Maßeinheit“ wird mittels eines Aggregatwerts und einer Anzahl von Verstößen definiert.  Wenn bei der Protokollsuche ein Datenpunkt diesen Wert überschreitet, gilt dies als Verstoß.  Wenn die Anzahl der Verstöße für ein beliebiges Objekt in den Ergebnissen den angegebenen Wert überschreitet, wird eine Warnung für dieses Objekt generiert.

#### <a name="example"></a>Beispiel
Angenommen, Sie wünschen sich eine Warnung, wenn ein beliebiger Computer binnen 30 Minuten dreimal die Prozessornutzung von 90 % überschreitet.  Dazu erstellen Sie eine Warnungsregel mit den folgenden Details:  

**Abfrage:** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5 m), Computer<br>
**Zeitfenster**: 30 Minuten<br>
**Warnungshäufigkeit**: Fünf Minuten<br>
**Aggregatwert**: Größer als 90<br>
**Warnung auslösen basierend auf**: Gesamtanzahl der Verstöße größer als 5<br>

Die Abfrage ermittelt einen Durchschnittswert für jeden Computer in 5-Minuten-Intervallen.  Diese Abfrage wird alle 5 Minuten für die in den letzten 30 Minuten gesammelten Daten ausgeführt.  Nachstehend sehen Sie Beispieldaten für drei Computer.

![Ergebnisse der Beispielabfrage](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Bei diesem Beispiel werden separate Warnungen für srv02 und srv03 erstellt, da diese Computer im Zeitfenster dreimal gegen den Schwellenwert 90 % verstoßen haben.  Wenn **Warnung auslösen basierend auf** in **Aufeinander folgend** geändert wird, wird nur für srv03 eine Warnung generiert, da dieser bei drei aufeinander folgenden Stichproben gegen den Schwellenwert verstoßen hat.


## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit [Webhookaktionen für Protokollwarnungen](monitor-alerts-unified-log-webhook.md) vertraut.
* [Verschaffen Sie sich einen Überblick über Azure-Warnungen.](monitoring-overview-unified-alerts.md)
* Erfahren Sie mehr über die [Verwendung von Azure-Warnungen](monitor-alerts-unified-usage.md).
* Weitere Informationen zu [Application Insights](../application-insights/app-insights-analytics.md)
* Erfahren Sie mehr über [Log Analytics](../log-analytics/log-analytics-overview.md).    

---
title: Analysieren der Datennutzung in Log Analytics | Microsoft Docs
description: Beurteilen Sie anhand des Log Analytics-Dashboards für Nutzung und geschätzte Kosten, wie viele Daten an Log Analytics gesendet werden, und ermitteln Sie mögliche Ursachen für unvorhergesehene Zunahmen.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/19/2018
ms.author: magoedte
ms.openlocfilehash: 9a9c898cf0f2e0b1387bbc2ac18b5009838d138b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317302"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Analysieren der Datennutzung in Log Analytics
Log Analytics enthält Informationen zur Menge der gesammelten Daten, zu den Quellen, die die Daten gesendet haben, und zu den unterschiedlichen Arten gesendeter Daten.  Die Datennutzung können Sie mithilfe des Dashboards **Protokollanalysenutzung** überprüfen und analysieren. Im Dashboard ist angegeben, wie viele Daten von jeder Lösung gesammelt werden und wie viele Daten von den Computern gesendet werden.

## <a name="understand-the-usage-dashboard"></a>Grundlagen des Dashboards „Nutzung“
Im Log Analytics-Dashboard zur **Nutzung** werden die folgenden Informationen angezeigt:

- Datenvolume
    - Datenvolume im Zeitverlauf (basierend auf dem aktuellen Zeitbereich)
    - Datenvolumen nach Lösung
    - Daten, die keinem Computer zugeordnet sind
- Computer
    - Computer, die Daten senden
    - Computer ohne Daten in den letzten 24 Stunden
- Angebote
    - Knoten vom Typ „Insight & Analytics“
    - Knoten vom Typ „Automation & Control“
    - Knoten vom Typ „Security“  
- Leistung
    - Zeit zum Erfassen und Indizieren von Daten  
- Liste der Abfragen

![Dashboard für Nutzung und geschätzte Kosten](./media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>
)

### <a name="to-work-with-usage-data"></a>So arbeiten Sie mit Nutzungsdaten
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.<br><br> ![Azure-Portal](./media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche einen Arbeitsbereich aus.
4. Wählen Sie in der Liste im linken Bereich die Option **Nutzung und geschätzte Kosten** aus.
5. Auf dem Dashboard für **Nutzung und geschätzte Kosten** können Sie den Zeitraum anpassen. Klicken Sie dazu auf **Zeit: Letzte 24 Stunden**, und ändern Sie das Zeitintervall.<br><br> ![Zeitintervall](./media/log-analytics-usage/usage-time-filter-01.png)<br><br>
6. Zeigen Sie die Blätter mit der Nutzungskategorie an, auf denen die für Sie interessanten Bereiche enthalten sind. Wählen Sie ein Blatt aus, und klicken Sie darin dann auf ein Element, um für die [Protokollsuche](log-analytics-log-searches.md) mehr Details anzuzeigen.<br><br> ![Beispiel-KPI für die Datennutzung](media/log-analytics-usage/data-volume-kpi-01.png)<br><br>
7. Überprüfen Sie im Dashboard „Protokollsuche“ die Ergebnisse, die für die Suche ausgegeben werden.<br><br> ![Beispiel zur Nutzung für die Protokollsuche](./media/log-analytics-usage/usage-log-search-01.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Erstellen einer Warnung für den Fall, dass die Datensammlung höher als erwartet ist
In diesem Abschnitt wird beschrieben, wie Sie eine Warnung erstellen, wenn Folgendes gilt:
- Das Datenvolumen übersteigt eine angegebene Menge.
- Für das Datenvolumen besteht die Vorhersage, dass eine bestimmte Menge überschritten wird.

Azure-Warnungen unterstützen [Protokollwarnungen](../monitoring-and-diagnostics/monitor-alerts-unified-log.md), die Suchabfragen nutzen. 

Für die folgende Abfrage wird ein Ergebnis erzielt, wenn innerhalb der letzten 24 Stunden mehr als 100 GB an Daten gesammelt wurden:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

Für die folgende Abfrage wird eine einfache Formel verwendet, um vorherzusagen, wenn an einem Tag mehr als 100 GB an Daten gesendet werden: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Wenn die Warnung für ein anderes Datenvolumen gelten soll, ändern Sie den Wert 100 in den Abfragen einfach in den gewünschten GB-Wert.

Führen Sie die Schritte unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) aus, um eine Benachrichtigung zu erhalten, wenn die Datensammlung höher als erwartet ausfällt.

Legen Sie beim Erstellen der Warnung für die erste Abfrage Folgendes fest, wenn mehr als 100 GB an Daten innerhalb von 24 Stunden anfallen:  

- **Definieren der Warnungsbedingung**: Festlegen Ihres Log Analytics-Arbeitsbereichs als Ressourcenziel
- **Warnungskriterien**: Geben Sie Folgendes an:
   - **Signalname**: **Benutzerdefinierte Protokollsuche**
   - **Suchabfrage** auf `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **Warnungslogik**: **Basiert auf** *Anzahl von Ergebnissen* und **Bedingung** ist *Größer als* ein **Schwellenwert** von *0*
   - **Zeitraum**: *1440* Minuten, **Warnungshäufigkeit**: alle *60* Minuten, da die Nutzungsdaten nur einmal pro Stunde aktualisiert werden
- **Definieren der Warnungsdetails**:
   - **Name** auf *Datenvolumen größer als 100 GB in 24 Stunden*
   - **Schweregrad** auf *Warnung*

Geben Sie eine vorhandene [Aktionsgruppe](../monitoring-and-diagnostics/monitoring-action-groups.md) an, oder erstellen Sie eine neue, damit Sie benachrichtigt werden, wenn die Protokollwarnung Kriterien erfüllt.

Legen Sie beim Erstellen der Warnung für die zweite Abfrage Folgendes fest, wenn die Vorhersage besteht, dass innerhalb von 24 Stunden mehr als 100 GB an Daten anfallen:

- **Definieren der Warnungsbedingung**: Festlegen Ihres Log Analytics-Arbeitsbereichs als Ressourcenziel
- **Warnungskriterien**: Geben Sie Folgendes an:
   - **Signalname**: **Benutzerdefinierte Protokollsuche**
   - **Suchabfrage** auf `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **Warnungslogik**: **Basiert auf** *Anzahl von Ergebnissen* und **Bedingung** ist *Größer als* ein **Schwellenwert** von *0*
   - **Zeitraum**: *180* Minuten, **Warnungshäufigkeit**: alle *60* Minuten, da die Nutzungsdaten nur einmal pro Stunde aktualisiert werden
- **Definieren der Warnungsdetails**:
   - **Name** auf *Erwartetes Datenvolumen von mehr als 100 GB in 24 Stunden*
   - **Schweregrad** auf *Warnung*

Geben Sie eine vorhandene [Aktionsgruppe](../monitoring-and-diagnostics/monitoring-action-groups.md) an, oder erstellen Sie eine neue, damit Sie benachrichtigt werden, wenn die Protokollwarnung Kriterien erfüllt.

Wenn Sie eine Warnung erhalten, können Sie die Schritte im folgenden Abschnitt verwenden, um per Problembehandlung zu ermitteln, warum die Nutzung höher als erwartet ist.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Ermittlung per Problembehandlung, warum die Nutzung höher als erwartet ist
Mit dem Dashboard „Nutzung“ können Sie ermitteln, warum die Nutzung (und somit die Kosten) höher als erwartet ist.

Eine höhere Nutzung wird durch eine bzw. beide der folgenden Bedingungen verursacht:
- Mehr Daten als erwartet werden an Log Analytics gesendet
- Mehr Knoten als erwartet senden Daten an Log Analytics

### <a name="check-if-there-is-more-data-than-expected"></a>Überprüfen, ob mehr Daten als erwartet vorhanden sind 
Die Seite „Nutzung“ enthält zwei wichtige Abschnitte, in denen Sie identifizieren können, welche Gründe die vermehrte Sammlung von Daten hat.

Das Diagramm *Datenmenge im Zeitverlauf* enthält die Informationen zum Gesamtvolumen der gesendeten Daten und zu den Computern, die die meisten Daten senden. Im Diagramm im oberen Bereich können Sie ablesen, ob die Gesamtdatennutzung zunimmt, stabil ist oder abnimmt. In der Liste mit den Computern sind die zehn Computer angegeben, die die meisten Daten senden.

Im Diagramm *Datenmenge nach Lösung* werden das Volumen der Daten, die von den einzelnen Lösungen gesendet werden, und die Lösungen angezeigt, von denen die meisten Daten gesendet werden. Im Diagramm im oberen Bereich ist das Gesamtvolumen der Daten angegeben, die von den einzelnen Lösungen im Zeitverlauf gesendet werden. Mit diesen Informationen können Sie ermitteln, ob von einer Lösung in Abhängigkeit der Zeit eine größere Datenmenge, ungefähr die gleiche Menge an Daten oder eine kleinere Datenmenge gesendet wird. In der Liste mit den Lösungen sind die zehn Lösungen aufgeführt, die die meisten Daten senden. 

Diese beiden Diagramme zeigen alle Daten an. Einige Daten sind gebührenpflichtig, andere sind kostenlos. Um nur gebührenpflichtige Daten zu berücksichtigen, ändern Sie die Abfrage auf der Suchseite, um `IsBillable=true` einzuschließen.  

![Diagramme zum Datenvolumen](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Sehen Sie sich das Diagramm *Datenmenge im Zeitverlauf* an. Klicken Sie jeweils auf den Namen eines Computers, um die Lösungen und Datentypen anzuzeigen, die für einen Computer die meisten Daten senden. Klicken Sie auf den Namen des ersten Computers in der Liste.

Im folgenden Screenshot werden für den Computer vom Datentyp *LogManagement/Perf* (Protokollverwaltung/Leistung) die meisten Daten gesendet.<br><br> ![Datenvolumen für einen Computer](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)<br><br>

Wechseln Sie als Nächstes zurück zum Dashboard *Nutzung*, und sehen Sie sich das Diagramm *Datenmenge nach Lösung* an. Klicken Sie in der Liste auf den Namen einer Lösung, um die Computer anzuzeigen, die die meisten Daten für die Lösung senden. Klicken Sie auf den Namen der ersten Lösung in der Liste. 

Im folgenden Screenshot ist zu erkennen, dass der Computer *mycon* die meisten Daten für die Protokollverwaltungslösung sendet.<br><br> ![Datenvolumen für eine Lösung](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)<br><br>

Führen Sie bei Bedarf zusätzliche Analysen aus, um große Mengen innerhalb einer Lösung oder eines Datentyps zu identifizieren. Beispiele für Abfragen:

+ **Sicherheitslösung**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Protokollverwaltungslösung**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Datentyp **Perf**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Datentyp **Event**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Datentyp **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Datentyp **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

Führen Sie die folgenden Schritte aus, um das Volumen der erfassten Protokolle zu reduzieren:

| Quelle mit hohem Datenvolumen | Reduzieren des Datenvolumens |
| -------------------------- | ------------------------- |
| Sicherheitsereignisse            | Wählen Sie [Sicherheitsereignisse vom Typ „Allgemein“ oder „Minimal“](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) aus. <br> Ändern der Sicherheitsüberwachungsrichtlinie, sodass nur benötigte Ereignisse erfasst werden. Überprüfen Sie insbesondere die Notwendigkeit zum Erfassen von Ereignissen für die <br> - [Überwachung der Filterplattform](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [Überwachung der Registrierung](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [Überwachung des Dateisystems](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [Überwachung des Kernelobjekts](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [Überwachung der Handleänderung](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - Überwachung von Wechselmedien |
| Leistungsindikatoren       | Ändern Sie [Leistungsindikatoren-Konfiguration](log-analytics-data-sources-performance-counters.md) in: <br> - Reduzieren der Sammlungshäufigkeit <br> - Reduzieren der Anzahl von Leistungsindikatoren |
| Ereignisprotokolle                 | Ändern Sie die [Ereignisprotokollkonfiguration](log-analytics-data-sources-windows-events.md) in: <br> - Reduzieren der Anzahl von erfassten Ereignisprotokollen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebene *Informationen*. |
| syslog                     | Ändern Sie die [syslog-Konfiguration](log-analytics-data-sources-syslog.md) in: <br> - Reduzieren der Anzahl von erfassten Einrichtungen <br> - Ausschließliches Erfassen von erforderlichen Ereignisebenen. Erfassen Sie beispielsweise keine Ereignisse der Ebenen *Informationen* und *Debuggen*. |
| AzureDiagnostics           | Ändern Sie die Ressourcenprotokollsammlung, um Folgendes zu erreichen: <br> - Verringern der Anzahl von Ressourcen, die Protokolle an Log Analytics senden <br> - Ausschließliches Erfassen von erforderlichen Protokollen |
| Lösungsdaten von Computern, für die die Lösung nicht erforderlich ist | Verwenden Sie die [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Überprüfen, ob mehr Knoten als erwartet vorhanden sind
Wenn Sie den Tarif *Pro Knoten (OMS)* nutzen, werden Ihre Gebühren basierend auf der Anzahl von genutzten Knoten und Lösungen berechnet. Sie können im Dashboard „Nutzung“ im Abschnitt mit den *Angeboten* anzeigen, wie viele Knoten eines Angebots jeweils verwendet werden.<br><br> ![Dashboard „Nutzung“](./media/log-analytics-usage/log-analytics-usage-offerings.png)<br><br>

Klicken Sie auf **Alle anzeigen...**, um die vollständige Liste mit Computern anzuzeigen, von denen für das ausgewählte Angebot Daten gesendet werden.

Verwenden Sie die [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen dazu, wie Sie die Suchsprache verwenden, finden Sie unter [Protokollsuchen in Log Analytics](log-analytics-log-searches.md). Sie können Suchabfragen verwenden, um für die Nutzungsdaten eine zusätzliche Analyse durchzuführen.
* Führen Sie die unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) beschriebenen Schritte aus, um benachrichtigt zu werden, wenn ein Suchkriterium erfüllt ist.
* Verwenden Sie die [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md), um Daten nur für erforderliche Gruppen mit Computern zu erfassen.
* Lesen Sie zum Konfigurieren einer effektiven Richtlinie zur Erfassung von Sicherheitsereignissen die Informationen unter [Datensammlung in Azure Security Center](../security-center/security-center-enable-data-collection.md).
* Ändern Sie die [Leistungsindikatorenkonfiguration](log-analytics-data-sources-performance-counters.md).
* Informationen zum Ändern der Einstellungen für die Ereigniserfassung finden Sie unter [Datenquellen für Windows-Ereignisprotokolle in Log Analytics](log-analytics-data-sources-windows-events.md).
* Informationen zum Ändern der Einstellungen für die Syslog-Sammlung finden Sie unter [Syslog-Datenquellen in Log Analytics](log-analytics-data-sources-syslog.md).

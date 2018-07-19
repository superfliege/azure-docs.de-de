---
title: Datenerfassungszeit in Azure Log Analytics | Microsoft-Dokumentation
description: Erläutert die verschiedenen Faktoren, die sich auf die Wartezeit beim Sammeln von Daten in Azure Log Analytics auswirken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2018
ms.author: bwren
ms.openlocfilehash: 0e513cc4f6a7d5d030ded807870de9eb0fdc0ed8
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38973182"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Datenerfassungszeit in Log Analytics
Azure Log Analytics ist ein Hochleistungs-Datendienst, der Tausende Kunden bedient, die mit zunehmender Tendenz jeden Monat Terabytes von Daten senden. Häufig werden Fragen nach dem Zeitbedarf gestellt, der nach dem Sammeln der Daten bis zu ihrer Verfügbarkeit in Log Analytics zu veranschlagen ist. Dieser Artikel erläutert die verschiedenen Faktoren, die sich auf diese Wartezeit auswirken.

## <a name="typical-latency"></a>Typische Wartezeit
Wartezeit bezeichnet hier die Zeitspanne zwischen der Erstellung der Daten auf dem überwachten System und dem Zeitpunkt, zu dem sie für die Analyse in Log Analytics verfügbar werden. Die typische Wartezeit für die Erfassung von Daten in Log Analytics liegt zwischen 3 und 10 Minuten, wobei 95 % der Daten in unter 7 Minuten erfasst werden. Die spezifische Wartezeit für bestimmte Daten hängt von einer Reihe von Faktoren ab, die nachfolgend erläutert werden.

## <a name="sla-for-log-analytics"></a>SLA für Log Analytics
Die [Vereinbarung zum Servicelevel (Service Level Agreement, SLA) für Log Analytics](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) ist ein rechtlich bindender Vertrag, der definiert, in welchen Fällen Microsoft zur Erstattung an Kunden verpflichtet ist, wenn der Dienst die vereinbarten Ziele verfehlt. Dabei wird nicht die normale Leistung des Systems zugrunde gelegt, sondern der schlimmste anzunehmende Fall, der die Ursache potenziell schwerwiegender Situationen bildet.

## <a name="factors-affecting-latency"></a>Faktoren, die die Wartezeit beeinflussen
Die gesamte Erfassungszeit für ein bestimmtes Dataset kann in die folgenden allgemeinen Bereiche aufgeteilt werden. 

- Agentzeit: Die Zeit für die Erkennung eines Ereignisses, seine Erfassung und das Senden an den Log Analytics-Erfassungspunkt als Protokolldatensatz. In den meisten Fällen wird dieser Prozess von einem Agent behandelt.
- Pipelinezeit: Die Zeit, die die Erfassungspipeline für die Verarbeitung des Protokolldatensatzes benötigt. Diese umfasst das Analysieren der Ereigniseigenschaften und potenziell das Hinzufügen berechneter Informationen.
- Indizierungszeit: Die Zeit für die Erfassung eines Protokolldatensatzes im Big Data-Speicher von Log Analytics.

Detailinformationen zu den verschiedenen Wartezeiten in diesem Prozess werden nachfolgend beschrieben.

### <a name="agent-collection-latency"></a>Agent-Sammlungswartezeit
Agents und Managementlösungen verwenden verschiedene Strategien, um Daten eines virtuellen Computers zu erfassen, was sich auf die Wartezeit auswirken kann. Dies sind einige spezifische Beispiele:

- Windows-Ereignisse, Syslog-Ereignisse und Leistungsmetriken werden sofort erfasst. Linux-Leistungsindikatoren werden in 30-Sekunden-Intervallen abgerufen.
- IIS-Protokolle und benutzerdefinierte Protokolle werden bei jeder Änderung ihres Zeitstempels erfasst. Bei IIS-Protokollen wird dies durch den [in IIS konfigurierten Rolloverzeitplan](log-analytics-data-sources-iis-logs.md) beeinflusst. 
- Die Active Directory-Replikationslösung führt ihre Bewertung alle fünf Tage aus, während die Active Directory-Bewertungslösung eine wöchentliche Bewertung Ihrer Active Directory-Infrastruktur ausführt. Der Agent erfasst diese Protokolle nur, wenn die Bewertung abgeschlossen ist.

### <a name="agent-upload-frequency"></a>Uploadhäufigkeit des Agents
Um einen schlanken Log Analytics-Agent zu gewährleisten, speichert der Agent Protokolle zwischen und lädt sie in regelmäßigen Abständen nach Log Analytics hoch. Die Uploadhäufigkeit schwankt zwischen 30 Sekunden und 2 Minuten, abhängig vom Datentyp. Die meisten Daten werden in unter 1 Minute hochgeladen. Die Netzwerkbedingungen können sich für diese Daten nachteilig auf die Wartezeit bis zum Erreichen des Log Analytics-Erfassungspunkts auswirken.

### <a name="azure-logs-and-metrics"></a>Azure-Protokolle und -Metriken 
Bis zur Verfügbarkeit von Daten des Aktivitätsprotokolls in Log Analytics vergehen ungefähr 5 Minuten. Für Daten aus Diagnoseprotokollen und Metriken kann es 1–5 Minuten bis zur Verfügbarkeit brauchen, abhängig vom Azure-Dienst. Anschließend braucht es weitere 30–60 Sekunden bei Protokollen und 3 Minuten bei Metriken, bis die Daten an den Erfassungspunkt von Log Analytics gesendet werden.

### <a name="management-solutions-collection"></a>Sammlung von Verwaltungslösungen
Einige Lösungen sammeln ihre Daten nicht mithilfe eines Agents, sondern verwenden eine Erfassungsmethode, die zusätzliche Wartezeit mit sich bringt. Einige Lösungen erfassen Daten in regelmäßigen Intervallen, ohne eine Sammlung nahezu in Echtzeit zu versuchen. Dies sind einige spezifische Beispiele:

- Die Office 365-Lösung ruft Aktivitätsprotokolle mithilfe der Office 365-Verwaltungsaktivitäts-API ab, die aktuell keine Garantien für Wartezeiten bietet, die einer Echtzeiterfassung nahekommen.
- Daten aus Windows Analytics-Lösungen (z.B. Updatekonformität) werden von der Lösung mit täglicher Häufigkeit erfasst.

Informationen zur Erfassungshäufigkeit der einzelnen Lösungen finden Sie in der Dokumentation der jeweiligen Lösung.

### <a name="pipeline-process-time"></a>Pipeline-Verarbeitungszeit
Sobald Protokolldatensätze in der Log Analytics-Pipeline erfasst werden, werden sie in einen temporären Speicher geschrieben, um Mandantenisolation und Schutz vor Datenverlust sicherzustellen. Dieser Vorgang wirkt sich normalerweise mit zusätzlichen 5–15 Sekunden aus. Einige Verwaltungslösungen implementieren aufwändigere Algorithmen zum Aggregieren von Daten und Ableiten von Erkenntnissen aus im Datenstrom eingehenden Daten. Beispielsweise aggregiert die Netzwerkleistungsüberwachung Daten über 3-Minuten-Intervalle, wodurch sich die Wartezeit um 3 Minuten verlängert.

### <a name="new-custom-data-types-provisioning"></a>Bereitstellung von neuen, benutzerdefinierten Datentypen
Wenn aus einem [benutzerdefinierten Protokoll](../log-analytics/log-analytics-data-sources-custom-logs.md) oder der [Datensammler-API](../log-analytics/log-analytics-data-collector-api.md) ein neuer benutzerdefinierter Datentyp erstellt wird, erstellt das System einen dedizierten Speichercontainer. Dies bedingt einen einmaligen Mehraufwand, der nur beim ersten Auftreten dieses Datentyps eintritt.

### <a name="surge-protection"></a>Schutz vor Überflutung
Die oberste Priorität für Log Analytics besteht darin, sicherzustellen, dass keine Kundendaten verloren gehen, daher weist das System einen integrierten Schutz vor Datenüberflutung auf. Dieser schließt Puffer ein, um sicherzustellen, dass das System selbst unter extremer Last noch funktioniert. Unter normalen Lastverhältnissen wird durch diese Steuerungsmechanismen weniger als eine Minute hinzugefügt, jedoch können sie unter extremen Bedingungen und bei Ausfällen zu erheblicher Verlängerung der Wartezeit führen, während sie die Sicherheit der Daten gewährleisten.

### <a name="indexing-time"></a>Indizierungszeit
Jede Big Data-Plattform vollzieht einen Kompromiss zwischen dem Bereitstellen von Analysen und erweiterten Suchfunktionen einerseits und sofortigem Zugriff auf die Daten andererseits. Azure Log Analytics ermöglicht Ihnen das Ausführen leistungsstarker Abfragen über Milliarden von Datensätzen und ruft Ergebnisse innerhalb weniger Sekunden ab. Dies wird dadurch ermöglicht, dass die Infrastruktur die Daten während ihrer Erfassung erheblich transformiert und sie in einzigartig kompakten Strukturen speichert. Das System puffert die Daten, bis genügend davon vorhanden sind, um diese Strukturen zu erstellen. Dieser Vorgang muss abgeschlossen sein, bevor der Protokolldatensatz in den Suchergebnissen angezeigt wird.

Dieser Prozess nimmt derzeit ungefähr 5 Minuten bei geringem Datenaufkommen in Anspruch, weniger bei hohem Datendurchsatz. Dies scheint der Intuition zu widersprechen, jedoch ermöglicht dieser Prozess die Optimierung der Wartezeit für Produktionsworkloads mit großem Datenaufkommen.



## <a name="checking-ingestion-time"></a>Überprüfen der Erfassungszeit
Sie können die **Heartbeat**-Tabelle verwenden, um eine Abschätzung der Wartezeit für Daten von Agents zu erhalten. Da der Heartbeat ein Mal pro Minute gesendet wird, liegt der Unterschied zwischen der aktuellen Uhrzeit und dem letzten Heartbeatdatensatz im Idealfall so nahe wie möglich an einer Minute.

Verwenden Sie die folgende Abfrage, um die Computer mit der längsten Wartezeit aufzulisten.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Verwenden Sie die folgende Abfrage in großen Umgebungen, um die Wartezeit für verschiedene Prozentsätze der Gesamtmenge der Computer zusammenzufassen.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) für Log Analytics.


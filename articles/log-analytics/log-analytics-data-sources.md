---
title: Konfigurieren von Datenquellen in Azure Log Analytics | Microsoft-Dokumentation
description: Datenquellen definieren die Daten, die Log Analytics aus Agents und anderen verbundenen Quellen sammelt.  Dieser Artikel beschreibt das Konzept, nach dem Log Analytics Datenquellen verwendet, erläutert Details zur Konfiguration der Quellen und bietet eine Übersicht über die verschiedenen verfügbaren Datenquellen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 4b7b1a9dc25b1bfaf72ab67dd0725a4518263ca5
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42146467"
---
# <a name="data-sources-in-log-analytics"></a>Datenquellen in Log Analytics
Log Analytics sammelt Daten aus Ihren verbundenen Quellen und speichert diese in Ihrem Log Analytics-Arbeitsbereich.  Welche Daten gesammelt werden, wird durch die von Ihnen konfigurierten Datenquellen definiert.  Daten in Log Analytics werden als Datensatzgruppe gespeichert.  Jede Datenquelle erstellt Datensätze eines bestimmten Typs, von denen jeder über einen eigenen Satz von Eigenschaften verfügt.

![Log Analytics-Datensammlung](./media/log-analytics-data-sources/overview.png)

Datenquellen unterscheiden sich von [Verwaltungslösungen](log-analytics-add-solutions.md), die ebenfalls Daten aus verbundenen Quellen sammeln und Datensätze in Log Analytics erstellen.  Neben der Sammlung von Daten beinhalten Lösungen in der Regel Protokollsuchvorgänge und Ansichten, mit denen Sie den Betrieb einer bestimmten Anwendung oder eines Diensts analysieren können.


## <a name="summary-of-data-sources"></a>Übersicht über Datenquellen
In der folgenden Tabelle werden die zurzeit in Log Analytics verfügbaren Datenquellen aufgeführt.  In den Links zu den Datenquellen finden Sie weitere Informationen zur jeweiligen Datenquelle.   Außerdem finden Sie hier Informationen zur jeweiligen Methode und Häufigkeit der Datensammlung in Log Analytics.  Sie können anhand der Informationen in diesem Artikel die verschiedenen verfügbaren Lösungen ermitteln sowie den Datenfluss und die Verbindungsanforderungen für unterschiedliche Verwaltungslösungen nachvollziehen. Erläuterungen zu den einzelnen Spalten finden Sie unter [Data collection details for management solutions in Azure](../monitoring/monitoring-solutions-inventory.md) (Ausführliche Informationen zu Datensammlungen für Verwaltungslösungen in Azure).


| Datenquelle | Plattform | Microsoft Monitoring Agent | Operations Manager-Agent | Azure-Speicher | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | Bei der Ankunft |
| [Benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | Bei der Ankunft |
| [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |richtet sich nach Einstellung „Protokolldateirollover“ |
| [Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |Gemäß Zeitplan, mindestens 10 Sekunden |
| [Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |Gemäß Zeitplan, mindestens 10 Sekunden |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Von Azure-Speicher: 10 Minuten; von Agent: bei Ankunft |
| [Windows-Ereignisprotokolle](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | Bei der Ankunft |


## <a name="configuring-data-sources"></a>Konfigurieren von Datenquellen
Sie konfigurieren Datenquellen in Log Analytics über das Menü **Daten** unter **Erweiterte Einstellungen**.  Jede Konfiguration wird an alle verbundenen Quellen in Ihrem Arbeitsbereich übermittelt.  Sie können zurzeit keine Agents aus dieser Konfiguration ausschließen.

![Windows-Ereignisse konfigurieren](./media/log-analytics-data-sources/configure-events.png)

1. Klicken Sie im Azure-Portal auf **Log Analytics**, auf Ihren Arbeitsbereich und anschließend auf **Erweiterte Einstellungen**.
2. Wählen Sie **Daten**aus.
3. Klicken Sie auf die Datenquelle, die Sie konfigurieren möchten.
4. Folgen Sie den Links in der oben stehenden Tabelle, um zur Dokumentation für jede Datenquelle zu gelangen und detaillierte Informationen zur jeweiligen Konfiguration zu erhalten.


## <a name="data-collection"></a>Datensammlung
Die Konfigurationen der Datenquellen werden innerhalb weniger Minuten an Agents übermittelt, die direkt mit Log Analytics verbunden sind.  Die angegebenen Daten werden vom Agent gesammelt und in den für jede Datenquelle spezifischen Intervallen direkt an Log Analytics übermittelt.  Informationen zu diesen Spezifikationen finden Sie in der Dokumentation zu jeder Datenquelle.

Bei System Center Operations Manager-Agents in einer verbundenen Verwaltungsgruppe werden Datenquellenkonfigurationen in Management Packs übersetzt und standardmäßig alle fünf Minuten an die Verwaltungsgruppe übermittelt.  Der Agent lädt das Management Pack wie jedes andere Paket herunter und sammelt die angegebenen Daten. Je nach Datenquelle werden die Daten entweder an einen Verwaltungsserver gesendet, der die Daten an Log Analytics weiterleitet, oder der Agent sendet die Daten ohne den Umweg über den Verwaltungsserver direkt an Log Analytics. Einzelheiten hierzu finden Sie unter [Ausführliche Informationen zu Datensammlungen für Verwaltungslösungen in Azure](../monitoring/monitoring-solutions-inventory.md).  Informationen zum Verbinden von Operations Manager und Log Analytics und zum Ändern der Häufigkeit, mit der die Konfiguration übermittelt wird, finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md).

Falls der Agent keine Verbindung mit Log Analytics oder Operations Manager herstellen kann, sammelt er weiter Daten und übermittelt diese, sobald eine Verbindung hergestellt wird.  Daten können verloren, wenn die Datenmenge die maximale Cachegröße für den Client erreicht oder der Agent 24 Stunden lang keine Verbindung herstellen kann.

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Alle von Log Analytics gesammelten Daten werden im Arbeitsbereich als Datensätze gespeichert.  Datensätze, die aus verschiedenen Datenquellen gesammelt wurde, verfügen über einen eigenen Eigenschaftensatz und werden über die Eigenschaft **Typ** identifiziert.  Weitere Informationen zu den Datensatztypen finden Sie in der Dokumentation zur jeweiligen Datenquelle und Lösung.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Lösungen](../monitoring/monitoring-solutions.md), die Log Analytics um zusätzliche Funktionen erweitern und ebenfalls Daten für den Arbeitsbereich sammeln.
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
* Konfigurieren Sie [Warnungen](log-analytics-alerts.md), damit Sie bei kritischen Daten, die aus Datenquellen und Lösungen gesammelt werden, direkt benachrichtigt werden.

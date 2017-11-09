---
title: Filter in Azure Log Analytics-Ansichten | Microsoft-Dokumentation
description: "Mit Filtern in einer Log Analytics-Ansicht können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: bwren
ms.openlocfilehash: 5c2201292eb085dcc043e4257580c7971dbaffbd
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="filters-in-log-analytics-views"></a>Filter in Log Analytics-Ansichten
Mit **Filtern** in einer [Log Analytics-Ansicht](log-analytics-view-designer.md) können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Beispielsweise können Sie Benutzern Ihrer Ansicht ermöglichen, die Ansicht nach Daten nur von einem bestimmten Computer oder eine Gruppe von Computern zu filtern.  Sie können mehrere Filter für eine einzelne Ansicht erstellen, um Benutzern das Filtern nach mehreren Eigenschaften zu ermöglichen.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen.

## <a name="using-a-filter"></a>Verwenden eines Filters
Klicken Sie auf **Filter**, um den Filterbereich für eine Ansicht zu öffnen.  Dadurch können Sie einen Zeitraum und Werte für alle Filter auswählen, die für die Ansicht verfügbar sind.  Wenn Sie einen Filter auswählen, wird eine Liste der verfügbaren Werte angezeigt.  Sie können Werte auswählen oder sie eingeben. Die Ansicht wird automatisch aktualisiert und nach den angegebenen Werten gefiltert. 

Wenn für einen Filter kein Wert ausgewählt ist, wird dieser Filter nicht auf die Ansicht angewendet.  Wenn Sie alle Werte für einen Filter entfernen, wird dieser Filter nicht mehr angewendet.


![Filterbeispiel](media/log-analytics-view-designer/filters-example.png)


## <a name="creating-a-filter"></a>Erstellen eines Filters

Erstellen Sie einen Filter über die Registerkarte **Filter**, wenn Sie [eine Ansicht bearbeiten](log-analytics-view-designer.md).  Der Filter gilt global für die Ansicht und wird auf alle Teile der Ansicht angewendet.  

![Filtereinstellungen](media/log-analytics-view-designer/filters-settings.png)

In der folgenden Tabelle werden die Einstellungen für Filter beschrieben.

| Einstellung | Beschreibung |
|:---|:---|
| Feldname | Der Name des zum Filtern verwendeten Felds.  Dieser muss dem Zusammenfassungsfeld unter **Werte abfragen** entsprechen. |
| Werte abfragen | Die Abfrage, die zum Auffüllen der Filterdropdownliste für den Benutzer ausgeführt werden soll.  Hierfür muss [Zusammenfassen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) oder [Verschieden](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator) verwendet werden, um eindeutige Werte für ein bestimmtes Feld bereitzustellen, und die Abfrage muss dem **Feldnamen** entsprechen.  Sie können [Sortieren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator) verwenden, um die dem Benutzer angezeigten Werte zu sortieren. |
| Tag | Name für das Feld, der in Abfragen mit dem Filter verwendet und dem Benutzer angezeigt wird. |

### <a name="examples"></a>Beispiele

Die folgende Tabelle enthält einige Beispiele für häufig verwendete Filter.  

| Feldname | Werte abfragen | Tag |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sort by Computer asc | Computer |
| EventLevelName | Event &#124; distinct EventLevelName | Severity |
| SeverityLevel | Syslog &#124; distinct SeverityLevel | Severity |
| SvcChangeType | ConfigurationChange &#124; distinct svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Ändern von Ansichtsabfragen

Damit ein Filter wirkt, müssen Sie Abfragen in der Ansicht so ändern, dass über die ausgewählten Werte gefiltert wird.  Wenn Sie keine Abfragen in der Ansicht ändern, haben die vom Benutzer ausgewählten Werte keine Auswirkungen.

Die Syntax für die Verwendung eines Filterwerts in einer Abfrage lautet: 

    where ${filter name}  

Wenn Ihre Ansicht beispielsweise eine Abfrage aufweist, die Ereignisse zurückgibt und einen Filter namens „Computer“ verwendet, können Sie Folgendes verwenden.

    Event | where ${Computers} | summarize count() by EventLevelName

Wenn Sie einen weiteren Filter mit dem Namen „Schweregrad“ hinzugefügt haben, können Sie mit der folgenden Abfrage beide Filter verwenden.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Visualisierungsteile](log-analytics-view-designer-parts.md), die Sie der benutzerdefinierten Ansicht hinzufügen können.
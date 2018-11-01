---
title: Filter in Azure Log Analytics-Ansichten | Microsoft-Dokumentation
description: Mit Filtern in einer Log Analytics-Ansicht können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 87cfcefd91652194adc0a5ca4580a8d00ba4d234
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412745"
---
# <a name="filters-in-log-analytics-views"></a>Filter in Log Analytics-Ansichten
Mit **Filtern** in einer [Log Analytics-Ansicht](log-analytics-view-designer.md) können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Beispielsweise können Sie Benutzern Ihrer Ansicht ermöglichen, die Ansicht nach Daten nur von einem bestimmten Computer oder eine Gruppe von Computern zu filtern.  Sie können mehrere Filter für eine einzelne Ansicht erstellen, um Benutzern das Filtern nach mehreren Eigenschaften zu ermöglichen.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen.

## <a name="using-a-filter"></a>Verwenden eines Filters
Klicken Sie oben in der Ansicht auf den Datums-/Uhrzeitbereich, um die Dropdownliste zu öffnen, in der Sie den Datums-/Uhrzeitbereich für die Ansicht ändern können.

![Filterbeispiel](media/log-analytics-view-designer-filters/filters-example-time.png)

Klicken Sie auf **+**, um einen Filter mithilfe benutzerdefinierter Filter hinzuzufügen, die für die Ansicht definiert sind. Wählen Sie entweder einen Wert für den Filter aus der Dropdownliste, oder geben Sie einen Wert ein. Klicken Sie auf **+**, um weitere Filter hinzuzufügen. 


![Filterbeispiel](media/log-analytics-view-designer-filters/filters-example-custom.png)

Wenn Sie alle Werte für einen Filter entfernen, wird dieser Filter nicht mehr angewendet.


## <a name="creating-a-filter"></a>Erstellen eines Filters

Erstellen Sie einen Filter über die Registerkarte **Filter**, wenn Sie [eine Ansicht bearbeiten](log-analytics-view-designer.md).  Der Filter gilt global für die Ansicht und wird auf alle Teile der Ansicht angewendet.  

![Filtereinstellungen](media/log-analytics-view-designer-filters/filters-settings.png)

In der folgenden Tabelle werden die Einstellungen für Filter beschrieben.

| Einstellung | BESCHREIBUNG |
|:---|:---|
| Feldname | Der Name des zum Filtern verwendeten Felds.  Dieser muss dem Zusammenfassungsfeld unter **Werte abfragen** entsprechen. |
| Werte abfragen | Die Abfrage, die zum Auffüllen der Filterdropdownliste für den Benutzer ausgeführt werden soll.  Hierfür muss [Zusammenfassen](/azure/kusto/query/summarizeoperator) oder [Verschieden](/azure/kusto/query/distinctoperator) verwendet werden, um eindeutige Werte für ein bestimmtes Feld bereitzustellen, und die Abfrage muss dem **Feldnamen** entsprechen.  Sie können [Sortieren](/azure/kusto/query/sortoperator) verwenden, um die dem Benutzer angezeigten Werte zu sortieren. |
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

---
title: Standardeigenschaften in Azure Monitor Log Analytics-Datensätzen | Microsoft-Dokumentation
description: Beschreibt Eigenschaften, die mehreren Datentypen in Azure Monitor Log Analytics gemein sind.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: c43f8acf7f9278ca412bee9ad15f2d51541cd27b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044330"
---
# <a name="standard-properties-in-log-analytics-records"></a>Standardeigenschaften in Log Analytics-Datensätzen
Daten in [Log Analytics](../log-analytics/log-analytics-queries.md) werden als eine Menge von Datensätzen gespeichert, von denen jeder einen bestimmten Datentyp aufweist, der über eine eindeutige Menge von Eigenschaften verfügt. Viele Datentypen weisen Standardeigenschaften auf, die sie mit mehreren Typen gemein haben. In diesem Artikel werden diese Eigenschaften beschrieben, zusammen mit Beispielen für ihre Verwendung in Abfragen.

Die Implementierung einiger dieser Eigenschaften ist noch nicht abgeschlossen, daher finden Sie sie in einigen Datentypen, in anderen aber noch nicht.

## <a name="timegenerated"></a>TimeGenerated
Die **TimeGenerated**-Eigenschaft enthält das Datum und die Uhrzeit, zu der der Datensatz erstellt wurde. Sie bietet eine gemeinsame Eigenschaft, die zum zeitbezogenen Filtern oder Zusammenfassen verwendet werden kann. Wenn Sie im Azure-Portal einen Zeitbereich für eine Ansicht oder ein Dashboard auswählen, verwendet es „TimeGenerated“, um die Ergebnisse zu filtern.

### <a name="examples"></a>Beispiele

Die folgende Abfrage gibt die Anzahl der Fehlerereignisse zurück, die für jeden Tag der vorherigen Woche erstellt wurden.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

## <a name="type"></a>Typ
Die **Type**-Eigenschaft enthält den Namen der Tabelle, aus der der Datensatz abgerufen wurde, der auch als Datensatztyp betrachtet werden kann. Diese Eigenschaft ist bei Abfragen hilfreich, die Datensätze aus mehreren Tabellen kombinieren, z. B. Abfragen mit dem Operator `search`, um zwischen Datensätzen verschiedener Typen zu unterscheiden. An einigen Stellen kann **$table** anstelle von **Type** verwendet werden.

### <a name="examples"></a>Beispiele
Die folgende Abfrage gibt die Anzahl der Datensätze nach Typ zurück, die in der letzten Stunde gesammelt wurden.

```Kusto
search * 
| where TimeGenerated > ago(1h) 
| summarize count() by Type 
```

## <a name="resourceid"></a>\_ResourceId
Die **\_ResourceId**-Eigenschaft enthält einen eindeutigen Bezeichner für die Ressource, der der Datensatz zugeordnet ist. Damit haben Sie eine Standardeigenschaft, die Sie verwenden können, um den Bereich Ihrer Abfrage auf Datensätze aus einer bestimmten Quelle einzuschränken oder verwandte Daten aus mehreren Tabellen zusammenzuführen.

Für Azure-Ressourcen ist der Wert von **_ResourceId** die [Azure-Ressourcen-ID-URL](../azure-resource-manager/resource-group-template-functions-resource.md). Die Eigenschaft ist zurzeit auf Azure-Ressourcen beschränkt, sie wird aber auf Ressourcen außerhalb von Azure, etwa auf lokale Computer, ausgeweitet.

> [!NOTE]
> Einige Datentypen verfügen bereits über Felder, die die Azure-Ressourcen-ID oder zumindest Teile davon wie die Abonnement-ID enthalten. Während diese Felder aus Gründen der Abwärtskompatibilität beibehalten werden, wird empfohlen, „_ResourceId“ zu verwenden, um eine Kreuzkorrelation durchzuführen, da sie konsistenter ist.

### <a name="examples"></a>Beispiele
Die folgende Abfrage führt die Leistungs- und Ereignisdaten für die einzelnen Computer zusammen. Sie zeigt alle Ereignisse mit der ID _101_ und einer Prozessorauslastung von mehr als 50 %.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Die folgende Abfrage verknüpft die _AzureActivity_-Datensätze mit _SecurityEvent_-Datensätzen. Sie zeigt alle Aktivitätsvorgänge mit Benutzern, die an diesen Computern angemeldet waren.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie mehr über die Art der [Speicherung von Log Analytics-Daten](../log-analytics/log-analytics-queries.md).
- Rufen Sie eine Lektion zum [Schreiben von Abfragen in Log Analytics](../log-analytics/query-language/get-started-queries.md) ab.
- Arbeiten Sie eine Lektion zum [Verknüpfen von Tabellen in Log Analytics-Abfragen](../log-analytics/query-language/joins.md) durch.

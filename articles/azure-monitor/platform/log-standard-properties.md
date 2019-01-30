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
ms.topic: article
ms.date: 01/14/2019
ms.author: bwren
ms.openlocfilehash: 27c732a2ddd21401ffbefa727cbb8001ec288293
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381952"
---
# <a name="standard-properties-in-log-analytics-records"></a>Standardeigenschaften in Log Analytics-Datensätzen
Daten in [Log Analytics](../log-query/log-query-overview.md) werden als eine Menge von Datensätzen gespeichert, von denen jeder einen bestimmten Datentyp aufweist, der über eine eindeutige Menge von Eigenschaften verfügt. Viele Datentypen weisen Standardeigenschaften auf, die sie mit mehreren Typen gemein haben. In diesem Artikel werden diese Eigenschaften beschrieben, zusammen mit Beispielen für ihre Verwendung in Abfragen.

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

Für Azure-Ressourcen ist der Wert von **_ResourceId** die [Azure-Ressourcen-ID-URL](../../azure-resource-manager/resource-group-template-functions-resource.md). Die Eigenschaft ist zurzeit auf Azure-Ressourcen beschränkt, sie wird aber auf Ressourcen außerhalb von Azure, etwa auf lokale Computer, ausgeweitet.

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

## <a name="isbillable"></a>\_IsBillable
Die **\_IsBillable**-Eigenschaft gibt an, ob erfasste Daten gebührenpflichtig sind. Daten, für die **\_IsBillable** gleich _false_ ist, werden kostenlos gesammelt und Ihrem Azure-Konto nicht in Rechnung gestellt.

### <a name="examples"></a>Beispiele
Um eine Liste von Computern abzurufen, die kostenpflichtige Datentypen senden, führen Sie die folgende Abfrage aus:

> [!NOTE]
> Verwenden Sie Abfragen mit `union withsource = tt *` mit Bedacht, da Scans über verschiedene Datentypen kostenintensiv sind. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Diese Abfrage kann so erweitert werden, dass die Anzahl von Computern pro Stunde zurückgegeben wird, die kostenpflichtige Datentypen senden:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="billedsize"></a>\_BilledSize
Die **\_BilledSize**-Eigenschaft gibt die Größe in Datenbytes an, die Ihrem Azure-Konto in Rechnung gestellt wird, wenn **\_IsBillable** gleich „true“ ist.

### <a name="examples"></a>Beispiele
Um die Größe der pro Computer erfassten abrechenbaren Ereignisse anzuzeigen, verwenden Sie die `_BilledSize`-Eigenschaft, die die Größe in Bytes bereitstellt:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Um die Anzahl von erfassten Ereignissen pro Computer anzuzeigen, führen Sie die folgende Abfrage aus:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Um die Anzahl von erfassten abrechenbaren Ereignissen pro Computer anzuzeigen, führen Sie die folgende Abfrage aus: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Wenn Sie die Anzahl gebührenpflichtiger Datentypen anzeigen möchten, die Daten an einen bestimmten Computer senden, führen Sie die folgende Abfrage aus:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```


## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie mehr über die Art der [Speicherung von Log Analytics-Daten](../log-query/log-query-overview.md).
- Rufen Sie eine Lektion zum [Schreiben von Abfragen in Log Analytics](../../azure-monitor/log-query/get-started-queries.md) ab.
- Arbeiten Sie eine Lektion zum [Verknüpfen von Tabellen in Log Analytics-Abfragen](../../azure-monitor/log-query/joins.md) durch.

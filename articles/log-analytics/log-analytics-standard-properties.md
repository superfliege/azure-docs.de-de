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
ms.date: 08/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 663f0b04c528c180e4130c1c157441cbc0ceb98b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955866"
---
# <a name="standard-properties-in-log-analytics-records"></a>Standardeigenschaften in Log Analytics-Datensätzen
Daten in [Log Analytics](../log-analytics/log-analytics-queries.md) werden als eine Menge von Datensätzen gespeichert, von denen jeder einen bestimmten Datentyp aufweist, der über eine eindeutige Menge von Eigenschaften verfügt. Viele Datentypen weisen Standardeigenschaften auf, die sie mit mehreren Typen gemein haben. In diesem Artikel werden diese Eigenschaften beschrieben, zusammen mit Beispielen für ihre Verwendung in Abfragen.

Die Implementierung einiger dieser Eigenschaften ist noch nicht abgeschlossen, daher finden Sie sie in einigen Datentypen, in anderen aber noch nicht.


## <a name="resourceid"></a>_ResourceId
Die **_ResourceId**-Eigenschaft enthält einen eindeutigen Bezeichner für die Ressource, der der Datensatz zugeordnet ist. Damit haben Sie eine Standardeigenschaft, die Sie verwenden können, um den Bereich Ihrer Abfrage auf Datensätze aus einer bestimmten Quelle einzuschränken oder verwandte Daten aus mehreren Tabellen zusammenzuführen.

Für Azure-Ressourcen ist der Wert von **_ResourceId** die [Azure-Ressourcen-ID-URL](../azure-resource-manager/resource-group-template-functions-resource.md). Die Eigenschaft ist zurzeit auf Azure-Ressourcen beschränkt, sie wird aber auf Ressourcen außerhalb von Azure, etwa auf lokale Computer, ausgeweitet. 

### <a name="examples"></a>Beispiele
Das folgende Beispiel zeigt eine Abfrage, die Leistungs- und Ereignisdaten für jeden Computer zusammenführt. Sie zeigt alle Ereignisse mit der ID _101_ und einer Prozessorauslastung von mehr als 50 %.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Das folgende Beispiel zeigt eine Abfrage, die _AzureActivity_-Datensätze mit _SecurityEvent_-Datensätzen verknüpft. Sie zeigt alle Aktivitätsvorgänge mit Benutzern, die an diesen Computern angemeldet waren.

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
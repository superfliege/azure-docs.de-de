---
title: Azure Monitor – Log Analytics-Sprachreferenz | Microsoft-Dokumentation
description: Enthält Referenzinformation für die Kusto-Sprache, die von Log Analytics verwendet wird. Enthält zusätzliche Elemente, die spezifisch für Log Analytics sind, und Elemente, die in Log Analytics-Abfragen nicht unterstützt werden.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 5173790436a29fa9947346d711da1a2ddb32bf62
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451067"
---
# <a name="log-analytics-query-language-reference"></a>Referenz zur Log Analytics-Abfragesprache
Für [Log Analytics-Abfragen](../log-analytics-queries.md) wird die gleiche Abfragesprache und Engine wie für [Azure-Daten-Explorer](/azure/data-explorer/) verwendet. Zugriff auf die Sprachreferenz und andere Informationen erhalten Sie in der [Referenz zur Kusto-Sprache](/azure/kusto/query).



## <a name="kusto-elements-not-support-in-log-analytics"></a>In Log Analytics nicht unterstützte Kusto-Elemente
Für Log Analytics-Abfragen wird zwar eine Implementierung von Kusto verwendet, aber es gibt einige Kusto-Elemente, die nicht unterstützt werden. Dies wird in den folgenden Abschnitten beschrieben.

### <a name="statements-not-supported-in-log-analytics"></a>In Log Analytics nicht unterstützte Anweisungen

* [Alias](/kusto/query/aliasstatement)
* [Abfrageparameter](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>In Log Analytics nicht unterstützte Funktionen

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>In Log Analytics nicht unterstützte Operatoren

* [Clusterübergreifender Join-Vorgang](/azure/kusto/query/joincrosscluster)
* [externaldata-Operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>In Log Analytics nicht unterstützte Plug-Ins

* [Plug-In „sql_request“](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Zusätzliche Operatoren in Log Analytics
Zur Unterstützung von bestimmten Log Analytics-Features werden die folgenden zusätzlichen Kusto-Operatoren bereitgestellt, die außerhalb von Log Analytics nicht verfügbar sind. 

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über Abfragen in [Log Analytics](../log-analytics-queries.md).
- Arbeiten Sie eine Lektion zum Schreiben einer [Log Analytics-Abfrage](/log-analytics/query-language/get-started-queries.md) durch.
- Greifen Sie auf die vollständige [Referenzdokumentation für Kusto](/azure/kusto/query/) zu.
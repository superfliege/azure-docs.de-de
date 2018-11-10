---
title: Azure Monitor – Log Analytics-Sprachreferenz | Microsoft-Dokumentation
description: Referenzinformationen für die von Log Analytics verwendete Abfragesprache des Daten-Explorers. Enthält zusätzliche Elemente, die spezifisch für Log Analytics sind, und Elemente, die in Log Analytics-Abfragen nicht unterstützt werden.
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
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e6d097749dae49cf6f1d710bcf01cf99dcd98a4c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243895"
---
# <a name="log-analytics-query-language-differences"></a>Unterschiede in der Log Analytics-Abfragesprache

Zwar basiert [Log Analytics](../log-analytics-queries.md) auf dem [Azure Daten-Explorer](/azure//data-explorer) und verwendet die [gleiche Abfragesprache](/azure/kusto/query), die Sprachversion bringt jedoch einige Unterschiede mit sich. In diesem Artikel werden Elemente benannt, die sich in der für den Daten-Explorer und der für Log Analytics-Abfragen verwendeten Version der Sprache unterscheiden.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>In Log Analytics nicht unterstützte Daten-Explorer-Elemente
In den folgenden Abschnitten sind Elemente der Daten-Explorer-Abfragesprache beschrieben, die von Log Analytics nicht unterstützt werden.

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
Die folgenden Operatoren unterstützen für Log Analytics spezifische Features und stehen außerhalb von Log Analytics nicht zur Verfügung.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Nächste Schritte

<<<<<<< HEAD:articles/log-analytics/query-language/data-explorer-difference.md
- Abrufen von Verweisen auf verschiedene [Ressourcen zum Schreiben von Log Analytics-Abfragen](kusto.md).
- Hier finden Sie die vollständige [Referenzdokumentation für die Daten-Explorer-Abfragesprache](/azure/kusto/query/).
=======
- Informieren Sie sich über Abfragen in [Log Analytics](../log-analytics-queries.md).
- Arbeiten Sie eine Lektion zum Schreiben einer [Log Analytics-Abfrage](/log-analytics/query-language/get-started-queries.md) durch.
- Greifen Sie auf die vollständige [Referenzdokumentation für Kusto](/azure/kusto/query/) zu.
>>>>>>> 4bccab5ecb17c887658a4d2ed1bab6b22bf29ffd:articles/log-analytics/query-language/kusto.md

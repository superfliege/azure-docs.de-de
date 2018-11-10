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
ms.date: 10/31/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 65f3388074ff7a84c6e0516a64665019d5883ab1
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913171"
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

- Abrufen von Verweisen auf verschiedene [Ressourcen zum Schreiben von Log Analytics-Abfragen](query-language.md).
- Hier finden Sie die vollständige [Referenzdokumentation für die Daten-Explorer-Abfragesprache](/azure/kusto/query/).

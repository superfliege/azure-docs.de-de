---
title: Grundlegendes zur Abfragesprache
description: Beschreibt die verfügbaren Kusto-Operatoren und Funktionen, die mit Azure Resource Graph verwendet werden können.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 08e4f09665a3501073f55b7f5b82bf51cf508ea9
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276676"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Grundlegendes zur Azure Resource Graph-Abfragesprache

Die Abfragesprache für Azure Resource Graph unterstützt eine Reihe von Operatoren und Funktionen. Diese arbeiten jeweils basierend auf [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md).

Der beste Weg, die von Resource Graph verwendete Abfragesprache kennenzulernen, besteht darin, zunächst mit der Dokumentation zur Azure Data Explorer-[Abfragesprache](/azure/kusto/query/index) zu beginnen. Sie vermittelt ein Verständnis dafür, wie die Sprache aufgebaut ist und wie die verschiedenen unterstützten Operatoren und Funktionen zusammenwirken.

## <a name="supported-tabular-operators"></a>Unterstützte tabellarische Operatoren

Die folgenden tabellarischen Operatoren werden in Resource Graph unterstützt:

- [count](/azure/kusto/query/countoperator)
- [distinct](/azure/kusto/query/distinctoperator)
- [extend](/azure/kusto/query/extendoperator)
- [limit](/azure/kusto/query/limitoperator)
- [order by](/azure/kusto/query/orderoperator)
- [project](/azure/kusto/query/projectoperator)
- [project-away](/azure/kusto/query/projectawayoperator)
- [Beispiel](/azure/kusto/query/sampleoperator)
- [sample-distinct](/azure/kusto/query/sampledistinctoperator)
- [sort by](/azure/kusto/query/sortoperator)
- [summarize](/azure/kusto/query/summarizeoperator)
- [take](/azure/kusto/query/takeoperator)
- [top](/azure/kusto/query/topoperator)
- [top-nested](/azure/kusto/query/topnestedoperator)
- [top-hitters](/azure/kusto/query/tophittersoperator)
- [Hierbei gilt:](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Unterstützte Funktionen

Die folgenden Funktionen werden in Resource Graph unterstützt:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [tostring()](/azure/kusto/query/tostringfunction)
- [zip()](/azure/kusto/query/zipfunction)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Lernen Sie, [Ressourcen zu untersuchen](explore-resources.md)
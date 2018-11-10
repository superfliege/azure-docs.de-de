---
title: Grundlegendes zur Azure Resource Graph-Abfragesprache
description: Beschreibt die Funktionsweise der Abfragesprache für Azure Resource Graph.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 09bcedc5250755f06ba23b84a0ae90b4d43a23db
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086164"
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
- [sample](/azure/kusto/query/sampleoperator)
- [sample-distinct](/azure/kusto/query/sampledistinctoperator)
- [sort by](/azure/kusto/query/sortoperator)
- [summarize](/azure/kusto/query/summarizeoperator)
- [take](/azure/kusto/query/takeoperator)
- [top](/azure/kusto/query/topoperator)
- [top-nested](/azure/kusto/query/topnestedoperator)
- [top-hitters](/azure/kusto/query/tophittersoperator)
- [where](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Unterstützte Funktionen

Die folgenden Funktionen werden in Resource Graph unterstützt:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [tostring()](/azure/kusto/query/tostringfunction)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Lernen Sie, [Ressourcen zu untersuchen](explore-resources.md)

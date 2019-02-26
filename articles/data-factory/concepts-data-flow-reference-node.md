---
title: Datenzuordnungsflow in Azure Data Factory – Verweisknoten
description: Im Data Factory-Datenfluss wird ein Verweisknoten für Joins, Suchvorgänge und Unions hinzugefügt.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 2e318210d96822b13f65eadeef79798b1b4595d1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325893"
---
# <a name="mapping-data-flow-reference-node"></a>Mapping Data Flow: Verweisknoten

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Verweisknoten](media/data-flow/referencenode.png "Verweisknoten")

Ein Verweisknoten wird in der Canvas automatisch hinzugefügt, um anzugeben, dass der Knoten an Verweise auf einen anderen Knoten in der Canvas angefügt ist. Sie können sich einen Verweisknoten als Zeiger oder Verweis auf eine andere Datenflusstransformation vorstellen.

Beispiel:  Wenn Sie mehrere Datenströme verknüpfen oder vereinigen, wird in der Datenfluss-Canvas möglicherweise ein Verweisknoten hinzugefügt, der den Namen und die Einstellungen des nicht primären eingehenden Datenstroms darstellt.

Der Verweisknoten kann nicht verschoben oder gelöscht werden. Sie können jedoch in den Knoten klicken, um die ursprünglichen Transformationseinstellungen zu ändern.

Die Benutzeroberflächenregeln, die angewandt werden, wenn der Verweisknoten im Datenfluss hinzugefügt wird, basieren auf dem verfügbaren Platz und vertikalen Abstand zwischen den Zeilen.

---
title: Azure Data Factory-Mappingdatenfluss-Transformation für abgeleitete Spalten
description: 'Gewusst wie: Transformieren von Daten nach Maß mit Azure Data Factory-Mappingdatenfluss-Transformation für abgeleitete Spalten'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: f53e122eb1b2a5b6dabb9a44aef42394d0c7edb6
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999483"
---
# <a name="mapping-data-flow-derived-column-transformation"></a>Mappingdatenfluss-Transformation für abgeleitete Spalten

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für abgeleitete Spalten, um in Ihrem Datenfluss neue Spalten zu generieren oder vorhandene Felder zu ändern.

![Spalte ableiten](media/data-flow/dc1.png "Abgeleitete Spalte")

Sie können in einer einzigen Transformation für abgeleitete Spalten mehrere Aktionen für abgeleitete Spalten ausführen. Klicken Sie auf „Spalte hinzufügen“, um beim Schritt einer Einzeltransformation mehr als eine Spalte zu transformieren.

Wählen Sie im Feld „Spalte“ eine vorhandene Spalte aus, die mit einem neuen abgeleiteten Wert überschrieben werden soll, oder klicken Sie auf „Neue Spalte erstellen“, um eine neue Spalte mit dem neu abgeleiteten Wert zu erstellen.

Über das Textfeld „Ausdruck“ wird der Ausdrucks-Generator geöffnet, in dem Sie den Ausdruck für die abgeleiteten Spalten mithilfe der Ausdrucksfunktionen erstellen können.

## <a name="column-patterns"></a>Spaltenmuster

Wenn die Spaltennamen aus Ihren Quellen variabel sind, möchten Sie möglicherweise in der abgeleiteten Spalte Transformationen mithilfe von Spaltenmustern anstelle von benannten Spalten verwenden. Weitere Informationen finden Sie im Artikel [Schemaabweichung](concepts-data-flow-schema-drift.md).

![Spaltenmuster](media/data-flow/columnpattern.png "Spaltenmuster")

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Data Factory-Ausdruckssprache für Transformationen](http://aka.ms/dataflowexpressions) und den [Ausdrucks-Generator](concepts-data-flow-expression-builder.md).

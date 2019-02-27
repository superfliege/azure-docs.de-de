---
title: 'Azure Data Factory Data Flow: Verknüpfungstransformation'
description: 'Azure Data Factory Data Flow: Verknüpfungstransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 517afe21fbf9241e2b2423525e9caee12a5603f6
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271185"
---
# <a name="azure-data-factory-data-flow-join-transformation"></a>Azure Data Factory Data Flow: Verknüpfungstransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie Join zum Kombinieren von Daten aus zwei Tabellen in Ihrer Data Flow-Instanz. Klicken Sie auf die Transformation, die die linke Beziehung sein soll, und fügen Sie eine Join-Transformation aus der Toolbox hinzu. Wählen Sie in der Join-Transformation einen anderen Datenstrom aus Ihrem Datenfluss als rechte Beziehung aus.

![Join-Transformation](media/data-flow/join.png "Join")

## <a name="join-types"></a>Join-Typen

Die Auswahl des Join-Typs ist für die Join-Transformation erforderlich.

### <a name="inner-join"></a>Innerer Join

Der innere Join durchläuft nur Zeilen, die die Spaltenbedingungen aus beiden Tabellen erfüllen.

### <a name="left-outer"></a>Linker äußerer Join

Alle Zeilen aus dem linken Datenstrom, die die Join-Bedingung nicht erfüllen, werden durchlaufen, und die Ausgabespalten aus der anderen Tabelle werden auf NULL festgelegt, zusätzlich zu allen Zeilen, die vom inneren Join zurückgegeben werden.

### <a name="right-outer"></a>Rechter äußerer Join

Alle Zeilen aus dem rechten Datenstrom, die die Join-Bedingung nicht erfüllen, werden durchlaufen, und die Ausgabespalten, die der anderen Tabelle entsprechen, werden auf NULL festgelegt, zusätzlich zu allen Zeilen, die vom inneren Join zurückgegeben werden.

### <a name="full-outer"></a>Vollständiger äußerer Join

Der vollständige äußere Join gibt alle Spalten und Zeilen auf beiden Seiten zurück mit NULL-Werten für Spalten, die in der anderen Tabelle nicht vorhanden sind.

### <a name="cross-join"></a>Cross Join

Gibt das Kreuzprodukt der beiden Datenströme mit einem Ausdruck an.

## <a name="specify-join-conditions"></a>Angeben der Join-Bedingungen

Die „Linker äußerer Join“-Bedingung stammt aus dem Datenstrom, der mit der linken Beziehung Ihres Joins verbunden ist. Die „Rechter äußerer Join“-Bedingung ist der zweite Datenstrom, der mit Ihrem Join im unteren Bereich verbunden ist, der entweder ein direkter Connector zu einen anderen Datenstrom oder ein Verweis auf einen anderen Datenstrom ist.

Sie müssen mindestens 1 (1..n)-Join-Bedingungen eingeben. Dies können Felder sein, auf die entweder direkt verwiesen wird, ausgewählt im Dropdownmenü, oder Ausdrücke.

## <a name="join-performance-optimizations"></a>Optimierungen der Join-Leistung

Im Gegensatz zu „Merge Join“ in Tools wie SSIS ist „Join“ im ADF-Datenflow kein obligatorischer „Merge Join“-Vorgang. Aus diesem Grund müssen die Join-Schlüssel nicht zuerst sortiert werden. Der Join-Vorgang tritt in Spark mit Databricks basierend auf dem optimalen Join-Vorgang in Spark auf: Broadcast- / Map-Side-Join:

![Optimieren der Join-Transformation](media/data-flow/joinoptimize.png "Join-Optimierung")

Wenn Ihr Dataset in den Databricks-Workerknotenspeicher eingepasst werden kann, können wir Ihre Join-Leistung optimieren. Sie können auch die Partitionierung Ihrer Daten im Join-Vorgang angeben, um Datasets zu erstellen, die besser in den Speicher pro Worker passen.

## <a name="self-join"></a>Selbstverknüpfung

Selbstverknüpfungsbedingungen können Sie in ADF-Datenflow erzielen, wenn Sie mithilfe der SELECT-Transformation einen Alias für einen vorhandenen Datenstrom erstellen. Erstellen Sie zuerst eine „Neue Verzweigung“ aus einem Datenstrom, und fügen Sie dann eine SELECT-Anweisung hinzu, um einen Alias für den gesamten Originaldatenstrom zu erstellen.

![Selbstverknüpfung](media/data-flow/selfjoin.png "Selbstverknüpfung")

Im obigen Diagramm befindet sich die SELECT-Transformation ganz oben. Sie weist nur dem ursprünglichen Datenstrom den Alias „OrigSourceBatting“ zu. In der hervorgehobenen Verknüpfungstransformation darunter können Sie sehen, dass wir diesen SELECT-Aliasdatenstrom der Auswahltransformation als rechte Verknüpfung verwenden, sodass wir sowohl auf der linken als auch auf der rechten Seite der inneren Verknüpfung (des inneren Join) auf den gleichen Schlüssel verweisen können.

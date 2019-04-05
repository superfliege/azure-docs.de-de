---
title: 'Azure Data Factory Mapping Data Flow: Pivottransformation'
description: 'Azure Data Factory Mapping Data Flow: Pivottransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 5548a62218aaac2e4da3853e8e5d43a584922bc0
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569891"
---
# <a name="azure-data-factory-mapping-data-flow-pivot-transformation"></a>Azure Data Factory Mapping Data Flow: Pivottransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie Pivot in ADF-Datenfluss als Aggregation, bei der die unterschiedlichen Zeilenwerte einer oder mehrerer Gruppierungsspalten in einzelne Spalten transformiert wurden. Im Wesentlichen können Sie Zeilenwerte in neue Spalten pivotieren (Daten in Metadaten umwandeln).

![Pivotoptionen](media/data-flow/pivot1.png "Pivot 1")

## <a name="group-by"></a>Gruppieren nach

![Pivotoptionen](media/data-flow/pivot2.png "Pivot 2")

Legen Sie zuerst die Spalten fest, nach denen Sie für Ihre Pivotaggregation gruppieren möchten. Sie können mit dem Pluszeichen (+) neben der Spaltenliste mehr als 1 Spalte festlegen.

## <a name="pivot-key"></a>Pivotschlüssel

![Pivotoptionen](media/data-flow/pivot3.png "Pivot 3")

Der Pivotschlüssel ist die Spalte, die ADF von Zeile in Spalte pivotiert. Standardmäßig wird jeder eindeutige Wert im Dataset für dieses Feld in eine Spalte pivotiert. Sie können jedoch die Werte aus dem Dataset optional eingeben, die in Spaltenwerte pivotiert werden sollen.

## <a name="pivoted-columns"></a>Pivotierte Spalten

![Pivotoptionen](media/data-flow/pivot4.png "Pivot 4")

Wählen Sie abschließend die für die pivotierten Werte zu verwendende Aggregation aus, und wählen Sie aus, wie die Spalten in der neuen Ausgabeprojektion der Transformation angezeigt werden sollen.

(Optional) Sie können ein Benennungsmuster mit einem Präfix, einem Mittelteil und einem Suffix festlegen, das/der jedem neuen Spaltennamen aus den Zeilenwerten hinzugefügt wird.

Wenn Sie beispielsweise „Umsatz“ nach „Region“ pivotieren, erhalten Sie neue Spaltenwerte aus den einzelnen Umsatzzahlen. Beispiel: „25“, „50“, „1000“ usw. Wenn Sie jedoch den Präfixwert „Sales-“ festlegen, wird „Sales-“ am Anfang jedes Spaltenwerts hinzugefügt.

![Pivotoptionen](media/data-flow/pivot5.png "Pivot 5")

Wenn Sie die Spaltenanordnung auf „Normal“ festlegen, werden alle pivotierten Spalten zusammen mit ihren aggregierten Werten gruppiert. Wenn Sie die Spaltenanordnung auf „Lateral“ festlegen, wird zwischen Spalte und Wert gewechselt.

### <a name="aggregation"></a>Aggregation

Klicken Sie zum Festlegen der für die Pivotwerte zu verwendenden Aggregation auf das Feld am unteren Rand des Bereichs „Pivotierte Spalten“. Der ADF-Datenfluss-Ausdrucks-Generator wird geöffnet, in dem Sie einen Aggregationsausdruck erstellen und einen beschreibenden Aliasnamen für Ihre neuen aggregierten Werte angeben können.

Verwenden Sie die ADF-Datenfluss-Ausdruckssprache, um die Transformationen der pivotierten Spalten im Ausdrucks-Generator zu beschreiben: https://aka.ms/dataflowexpressions.

### <a name="how-to-rejoin-original-fields"></a>So verknüpfen Sie die ursprünglichen Felder erneut
> [!NOTE]
> Die Pivottransformation projiziert nur die in der Aggregation, Gruppierung und Pivotaktion verwendeten Spalten. Wenn Sie die anderen Spalten aus dem vorherigen Schritt in Ihren Datenfluss einbeziehen möchten, verwenden Sie eine neue Verzweigung aus dem vorherigen Schritt, und verwenden Sie dann das Selbstverknüpfungsmuster, um den Datenfluss mit den ursprünglichen Metadaten zu verbinden.

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, die [Entpivotierungstransformation](data-flow-unpivot.md) durchzuführen, um Spaltenwerte in Zeilenwerte umzuwandeln. 

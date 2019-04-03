---
title: Azure Data Factory Mapping Data Flow – Datasets
description: Azure Data Factory Mapping Data Flow bietet Kompatibilität mit bestimmten Datasets
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: ad6cfdad519ab3901c58979970ea07439b3106e9
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726922"
---
# <a name="mapping-data-flow-datasets"></a>Mapping Data Flow-Datasets

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Datasets sind ein Data Factory-Konstrukt, das die Form der Daten definiert, mit denen Sie in Ihrer Pipeline arbeiten. Im Data Flow erfordern Daten auf Zeilen- und Spaltenebene eine detaillierte Definition des Datasets. Für Datasets, die in Pipelines für die Ablaufsteuerung verwendet, ist nicht so ein tiefgreifendes Verständnis der Daten erforderlich.

Datasets in Datenfluss werden in Transformationen von Quellen und Senken verwendet. Sie werden zum Definieren der grundlegenden Datenschemas verwendet. Wenn Sie über kein Schema in den Daten verfügen, können Sie für die Quelle und die Senke die Schemaabweichung aktivieren. Wenn das Schema aus dem Dataset definiert ist, verfügen Sie über die zugehörigen Datentypen, Datenformate, Dateispeicherort und Verbindungsinformationen aus dem zugehörigen verknüpften Dienst. Die Metadaten in den Datasets werden in Ihrer Quelltransformation als „Projektion“ der Quelle angezeigt. Das Schema im Dataset repräsentiert den physischen Datentyp und die Form, während die Projektion in der Quelltransformation die Datenflussdarstellung der Daten mit definierten Namen und Typen darstellt.

## <a name="dataset-types"></a>Datasettypen

In Data Flow finden Sie derzeit vier Datasettypen:

* Azure SQL-Datenbank
* Azure SQL DW
* Parquet (aus ADLS und Blob)
* Text mit Trennzeichen (aus ADLS und Blob)

Datenfluss-Datasets trennen die den *Quelltyp* vom *Verbindungstyp „Verknüpfter Dienst“*. In der Regel wählen Sie in der Data Factory den Verbindungstyp (Blob, ADLS, etc.) und definieren dann den Dateityp im Dataset. Innerhalb von Data Flow wählen Sie die Quelltypen aus, die mit verschiedenen Verbindungsarten des verknüpften Dienstes verknüpft sein können.

![Optionen für die Quelltransformation](media/data-flow/dataset1.png "Quellen")

## <a name="data-flow-compatible-datasets"></a>Data Flow-kompatible Datasets

Beim Erstellen eines neuen Datasets befindet sich oben rechts ein Kontrollkästchen mit der Bezeichnung „Data Flow-kompatibel“. Wenn Sie auf diese Schaltfläche klicken, werden nur die Datasets gefiltert, die für Data Flows verwendet werden können. 

## <a name="import-schemas"></a>Importieren von Schemas

Wenn Sie das Schema von Data Flow-Datasets importieren, wird die Schaltfläche „Schema importieren“ angezeigt. Wenn Sie auf diese Schaltfläche klicken, werden zwei Optionen angezeigt: „Aus der Quelle importieren“ oder „Aus einer lokalen Datei importieren“. In den meisten Fällen importieren Sie das Schema direkt aus der Quelle. Wenn Sie jedoch eine vorhandene Schemadatei (Parquet-Datei oder CSV mit Headern) haben, können Sie auf diese lokale Datei verweisen und Data Factory definiert das Schema basierend auf dieser Schemadatei.

## <a name="delimited-text-dataset"></a>Dataset mit Text mit Trennzeichen

Im Dataset mit Text mit Trennzeichen legen Sie das Trennzeichen so fest, dass entweder einzelne („\t“ für TSV, „,“ für CSV, „|“...) oder mehrere Zeichen für das Trennzeichen verwendet werden. Legen Sie die Umschaltfläche für die Kopfzeile fest, und wechseln Sie dann zur Quelltransformation, um Datentypen automatisch erkennen zu lassen.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Erstellen eines neuen Datenflusses](data-flow-create.md) und Hinzufügen einer Quelltransformation. Konfigurieren Sie dann das Dataset für Ihre Quelle.

Verwenden Sie die [Kopieraktivität](copy-activity-overview.md), um Daten aus einer beliebigen ADF-Datenquelle einzulesen und sie in ADLS oder Blob für den Zugriff durch den Datenfluss bereitzustellen.


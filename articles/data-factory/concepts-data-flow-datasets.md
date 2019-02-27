---
title: Azure Data Factory Mapping Data Flow – Datasets
description: Azure Data Factory Mapping Data Flow hat bestimmte Datasetkompatibilitäten
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: 36ca5e07adf79de77ac4ab4149ff8e96a1dece8d
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408747"
---
# <a name="mapping-data-flow-datasets"></a>Mapping Data Flow-Datasets

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Datasets sind ein Data Factory-Konstrukt, das die Form der Daten definiert, mit denen Sie in Ihrer Pipeline arbeiten. Im Data Flow erfordern Daten auf Zeilen- und Spaltenebene eine detaillierte Definition des Datasets. Für Datasets, die in Pipelines für die Ablaufsteuerung verwendet, ist nicht so ein tiefgreifendes Verständnis der Daten erforderlich.

Datasets in Data Flow-Quellen- und Senkentransformationen werden verwendet, um das grundlegende Datenschema zu definieren. Wenn Sie über kein Schema in den Daten verfügen, können Sie für die Quelle und die Senke die Schemaabweichung aktivieren. Wenn das Schema aus dem Dataset definiert ist, verfügen Sie über die zugehörigen Datentypen, Datenformate, Dateispeicherort und Verbindungsinformationen aus dem zugehörigen verknüpften Dienst.

## <a name="dataset-types"></a>Datasettypen

In Data Flow finden Sie derzeit vier Datasettypen:

* Azure SQL-Datenbank
* Azure SQL DW
* Parquet
* Durch Trennzeichen getrennter Text

Data Flow-Datasets trennen die den *Quelltyp* vom Verbindungstyp „Verknüpfter Dienst“. In der Regel wählen Sie in der Data Factory den Verbindungstyp (Blob, ADLS, etc.) und definieren dann den Dateityp im Dataset. Innerhalb von Data Flow wählen Sie die Quelltypen aus, die mit verschiedenen Verbindungsarten des verknüpften Dienstes verknüpft sein können.

![Optionen für die Quelltransformation](media/data-flow/dataset1.png "Quellen")

## <a name="data-flow-compatible-datasets"></a>Data Flow-kompatible Datasets

Beim Erstellen eines neuen Datasets befindet sich oben rechts ein Kontrollkästchen mit der Bezeichnung „Data Flow-kompatibel“. Wenn Sie auf diese Schaltfläche klicken, werden nur die Datasets gefiltert, die für Data Flows verwendet werden können. 

## <a name="import-schemas"></a>Importieren von Schemas

Wenn Sie das Schema von Data Flow-Datasets importieren, wird die Schaltfläche „Schema importieren“ angezeigt. Wenn Sie auf diese Schaltfläche klicken, werden zwei Optionen angezeigt: „Aus der Quelle importieren“ oder „Aus einer lokalen Datei importieren“. In den meisten Fällen importieren Sie das Schema direkt aus der Quelle. Wenn Sie jedoch eine vorhandene Schemadatei (Parquet-Datei oder CSV mit Headern) haben, können Sie auf diese lokale Datei verweisen und Data Factory definiert das Schema basierend auf dieser Schemadatei.


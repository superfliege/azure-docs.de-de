---
title: Transformieren von Daten mit einem Databricks-Notebook – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten verarbeiten oder transformieren, indem Sie ein Databricks-Notebook ausführen.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: d4a57e45d5ddf55906fcf575df39135a227418ec
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Transformieren von Daten durch Ausführen eines Databricks-Notebooks

Die Azure Databricks-Notebook-Aktivität in einer [Data Factory-Pipeline](concepts-pipelines-activities.md) führt ein Databricks-Notebook in Ihrem Azure Databricks-Arbeitsbereich aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet. Azure Databricks ist eine verwaltete Plattform für die Ausführung von Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>Definition der Databricks-Notebook-Aktivität

Dies ist die JSON-Beispieldefinition der Databricks-Notebook-Aktivität:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            }
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Eigenschaften der Databricks-Notebook-Aktivität

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden:

|Eigenschaft|BESCHREIBUNG|Erforderlich|
|---|---|---|
|name|Der Name der Aktivität in der Pipeline.|Ja|
|Beschreibung|Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.|Nein |
|type|Bei Databricks-Notebook-Aktivitäten lautet der Aktivitätstyp DatabricksNotebook.|Ja|
|linkedServiceName|Der Name des verknüpften Databricks-Diensts, in dem das Databricks-Notebook ausgeführt wird. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md).|Ja|
|notebookPath|Der absolute Pfad des Notebooks, das im Databricks-Arbeitsbereich ausgeführt werden soll. Dieser Pfad muss mit einem Schrägstrich beginnen.|Ja|
|baseParameters|Ein Array aus Schlüssel-Wert-Paaren. Für jede Aktivitätsausführung können Basisparameter verwendet werden. Wenn das Notebook einen nicht spezifizierten Parameter akzeptiert, wird der Standardwert des Notebooks verwendet. Erfahren Sie mehr über Parameter in [Databricks-Notebooks](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Nein |

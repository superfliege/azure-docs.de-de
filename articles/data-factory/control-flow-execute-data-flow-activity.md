---
title: Aktivität „Datenfluss ausführen“ in Azure Data Factory | Microsoft-Dokumentation
description: Über die Aktivität „Datenfluss ausführen“ werden Datenflüsse ausgeführt.
services: data-factory
documentationcenter: ''
author: kromerm
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: 856f4bd9c2b04ff10ed598c5e641955e1de99398
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792411"
---
# <a name="execute-data-flow-activity-in-azure-data-factory"></a>Aktivität „Datenfluss ausführen“ in Azure Data Factory
Verwenden Sie die Aktivität „Datenfluss ausführen“, um Ihren ADF-Datenfluss in (der Sandbox) in Läufen zum Debuggen der Pipeline oder in von der Pipeline ausgelösten Läufen auszuführen.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="syntax"></a>Syntax

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "dataflow1",
         "type": "DataFlowReference"
      },
        "compute": {
          "computeType": "General",
          "coreCount": 8,
      }
}

```

## <a name="type-properties"></a>Typeigenschaften

* ```dataflow``` ist der Name der Datenflussentität, die Sie ausführen möchten.
* ```compute``` beschreibt die Spark-Ausführungsumgebung.
* ```coreCount``` ist die Anzahl der Kerne, die dieser Aktivität zur Ausführung Ihres Datenflusses zugewiesen werden sollen.

![Datenfluss ausführen](media/data-flow/activity-data-flow.png "Datenfluss ausführen")

### <a name="run-on"></a>Run on (Ausführen auf)

Wählen Sie die Compute-Umgebung für die Ausführung Ihres Datenflusses. Standardeinstellung ist die standardmäßige Azure Integration Runtime mit automatischer Auflösung. Bei dieser Wahl wird der Datenfluss in der Spark-Umgebung in der Region Ihrer Data Factory ausgeführt. Der Computetyp ist ein Auftragscluster, d.h. das Starten der Compute-Umgebung benötigt mehrere Minuten.

### <a name="debugging-pipelines-with-data-flows"></a>Debuggen von Pipelines mit Datenflüssen

![Schaltfläche „Debuggen“](media/data-flow/debugbutton.png "Schaltfläche „Debuggen“")

Verwenden Sie das Debuggen des Datenflusses, um einen aufgewärmten Cluster zum interaktiven Testen Ihrer Datenflüsse in einem Lauf zum Debuggen der Pipeline zu nutzen. Verwenden Sie die Option zum Debuggen der Pipeline, um Ihre Datenflüsse innerhalb einer Pipeline zu testen.

### <a name="compute-type"></a>Computetyp

Je nach Anforderungen Ihres Datenflusses können „Universell“, „Für Compute optimiert“ oder „Arbeitsspeicheroptimiert“ wählen.

### <a name="core-count"></a>Anzahl Kerne

Wählen Sie, wie viele Kerne Sie dem Auftrag zuweisen möchten. Bei kleineren Aufträgen funktionieren weniger Kerne besser.

### <a name="staging-area"></a>Stagingbereich

Wenn Sie Azure Data Warehouse als Senke verwenden, müssen Sie einen Stagingspeicherort für Ihre Polybase-Batchlast wählen.

## <a name="parameterized-datasets"></a>Parametrisierte Datasets

Wenn Sie parametrisierte Datasets verwenden, müssen Sie die Parameterwerte festlegen.

![Parameter für „Datenfluss ausführen“](media/data-flow/params.png "Parameter")

### <a name="debugging-parameterized-data-flows"></a>Debuggen parametrisierter Datenflüsse

Sie können Datenflüsse mit parametrisierten Datasets im Lauf zum Debuggen der Pipeline nur unter Verwendung der Aktivität „Datenfluss ausführen“ debuggen. Derzeit funktionieren interaktive Debugsitzungen im ADF-Datenfluss mit parametrisierten Datasets nicht. Ausführungen der Pipeline und Debugläufe funktionieren mit Parametern.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „If Condition“](control-flow-if-condition-activity.md)
- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Webaktivität](control-flow-web-activity.md)
- [Until-Aktivität](control-flow-until-activity.md)

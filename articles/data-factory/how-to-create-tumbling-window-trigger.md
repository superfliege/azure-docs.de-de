---
title: "Erstellen von Triggern für rollierende Fenster in Azure Data Factory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie in Azure Data Factory ein Trigger erstellt wird, der eine Pipeline gemäß einem rollierenden Fenster ausführt."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Erstellen eines Triggers in Azure Data Factory, der eine Pipeline gemäß einem rollierenden Fenster ausführt
Dieser Artikel enthält die Schritte zum Erstellen, Starten und Überwachen eines Triggers für rollierende Fenster. Allgemeine Informationen zu Triggern und unterstützten Triggertypen finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Trigger für ein rollierendes Fenster werden ab einem angegebenen Startzeitpunkt in regelmäßigen Zeitintervallen ausgelöst, während der Zustand beibehalten wird. Bei rollierenden Fenstern handelt es sich um eine Reihe von nicht überlappenden, aneinandergrenzenden Zeitintervallen mit einer festen Größe. Ein Trigger für ein rollierendes Fenster hat eine 1:1-Beziehung zu einer Pipeline und kann nur auf eine einzelne Pipeline verweisen.

## <a name="tumbling-window-trigger-type-properties"></a>Eigenschaften des Triggertyps für ein rollierendes Fenster
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "parameter1": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter2": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
                }
            }
      }    
}
```  

Die folgende Tabelle enthält eine allgemeine Übersicht über die wichtigsten Elemente für die Wiederholung und Zeitplanung in einem Trigger für ein rollierendes Fenster.

| **JSON-Name** | **Beschreibung** | **Zulässige Werte** | **Erforderlich** |
|:--- |:--- |:--- |:--- |
| **type** | Typ des Triggers. Dieser ist auf „TumblingWindowTrigger“ festgelegt. | Zeichenfolge | Ja |
| **runtimeState** | <readOnly> Mögliche Werte: Started, Stopped, Disabled | Zeichenfolge | Ja, schreibgeschützt |
| **frequency** |Die Zeichenfolge *frequency* stellt die Häufigkeitseinheit für die Triggerwiederholung dar. Unterstützte Werte: „minute“ und „hour“. Wenn die Startzeit Datumskomponenten hat, die detaillierter als die Häufigkeit sind, werden sie bei der Berechnung der Fenstergrenzen berücksichtigt. Beispiel: Wenn die Frequenz stündlich und die Startzeit 2016-04-01T10:10:10:10Z ist, ist das erste Fenster (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z.)  | Zeichenfolge. Unterstützte Typen: „minute“, „hour“ | Ja |
| **interval** |*interval* ist eine positive ganze Zahl und gibt das Intervall für *frequency* an, das bestimmt, wie oft der Trigger ausgeführt wird. Wenn beispielsweise *interval* auf 3 und *frequency* auf „hour“ festgelegt ist, wird der Trigger alle drei Stunden ausgeführt. | Ganze Zahl | Ja |
| **startTime**|*startTime* ist eine Datums-/Uhrzeitangabe. *startTime* ist das erste Vorkommen und kann in der Vergangenheit liegen. Der erste Triggerintervall ist (startTime, startTime + interval). | Datetime | Ja |
| **endTime**|*endTime* ist eine Datums-/Uhrzeitangabe. *startTime* ist das letzte Vorkommen und kann in der Vergangenheit liegen. | Datetime | Ja |
| **delay** | Geben Sie die Verzögerung an, mit der die Datenverarbeitung des Fensters beginnt. Die Ausführung der Pipeline wird nach der erwarteten Ausführungszeit + Verzögerung gestartet. „delay“ legt fest, wie lange der Trigger nach Ablauf der fälligen Zeit wartet, bevor er eine neue Ausführung auslöst. Die Fensterstartzeit wird dadurch nicht geändert. | Zeitraum (Beispiel: 00:10:00 bedeutet eine Verzögerung von 10 Minuten) |  Nein. Der Standardwert ist „00:00:00“. |
| **max concurrency** | Anzahl der gleichzeitigen Triggerausführungen, die für bereite Fenster ausgelöst werden. Beispiel: Wenn wir versuchen, für gestern stündlich abzugleichen, wären das 24 Fenster. Wenn „concurrency = 10“, werden Triggerereignisse nur für die ersten 10 Fenster ausgelöst (00:00-01:00 - 09:00-10:00). Nachdem die ersten 10 ausgelösten Pipelineausführungen erfolgt sind, werden Triggerausführungen für die nächsten 10 (10:00-11:00 - 19:00-20:00) ausgelöst. Wenn Sie mit dem Beispiel „concurrency = 10“ fortfahren, erfolgen, sobald 10 Fenster bereit sind, 10 Pipelineausführungen. Wenn nur ein Fenster bereit ist, erfolgt nur eine Pipelineausführung. | Ganze Zahl  | Ja. Mögliche Werte: 1-50 |
| **retryPolicy: Count** | Die Anzahl der Wiederholungen, bevor die Ausführung der Pipeline als „Failed“ markiert wird.  | Ganze Zahl  |  Nein. Standardwert ist 0 Wiederholungen. |
| **retryPolicy: intervalInSeconds** | Verzögerung zwischen den Wiederholungsversuchen in Sekunden. | Ganze Zahl  |  Nein. Der Standardwert ist 30 Sekunden. |

### <a name="using-system-variables-windowstart-and-windowend"></a>Verwenden von Systemvariablen: WindowStart und WindowEnd

Wenn Sie „WindowStart“ und „WindowEnd“ des Triggers für ein rollierendes Fenster in Ihrer **pipeline**-Definition (d.h. für einen Teil einer Abfrage) verwenden möchten, müssen Sie die Variablen als Parameter in der **trigger**-Definition an Ihre Pipeline übergeben:
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "MyWindowStart": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "MyWindowEnd": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    }
                }
            }
      }    
}
```  

Um die Werte von „WindowStart“ und „WindowEnd“ in der Pipelinedefinition zu nutzen, verwenden Sie die Parameter „MyWindowStart“ und „MyWindowEnd“ entsprechend.

### <a name="notes-on-backfill"></a>Hinweise zum Abgleich
Wenn mehrere Fenster zur Ausführung anstehen (insbesondere im Abgleichszenario), ist die Reihenfolge der Ausführung von Fenstern deterministisch (von den ältesten bis zu den neuesten Intervallen). Es gibt derzeit keine Möglichkeit, dieses Verhalten zu ändern.

### <a name="updating-an-existing-triggerresource"></a>Aktualisieren einer vorhandenen TriggerResource
* Wenn die Frequenz (oder Fenstergröße) des Triggers geändert wird, wird der Zustand der bereits bearbeiteten Fenster *nicht* zurückgesetzt. Der Trigger setzt das Auslösen für die Fenster ab dem letzten ausgeführten Fenster mit der neuen Fenstergröße fort.
* Wenn die Endzeit des Triggers geändert (erweitert oder aktualisiert) wird, wird der Zustand der bereits bearbeiteten Fenster *nicht* zurückgesetzt. Der Trigger berücksichtigt einfach die neue Endzeit. Wenn die Endzeit vor den bereits ausgeführten Fenstern liegt, wird der Trigger beendet. Andernfalls wird er beendet, sobald die neue Endzeit erreicht wird.

## <a name="sample-using-azure-powershell"></a>Beispiel mithilfe von Azure PowerShell
In diesem Abschnitt erfahren Sie, wie Sie mit Azure PowerShell einen Trigger erstellen, starten und überwachen.

1. Erstellen Sie im Ordner „C:\ADFv2QuickStartPSH“ eine JSON-Datei mit dem Namen „MyTrigger.json“ und dem folgenden Inhalt:

   > [!IMPORTANT]
   > Legen Sie **startTime** auf die aktuelle UTC-Zeit und **endTime** auf eine Stunde nach der aktuellen UTC-Zeit fest, bevor Sie die JSON-Datei speichern.

    ```json   
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```  
2. Erstellen Sie mit dem Cmdlet **Start-AzureRmDataFactoryV2Trigger** einen Trigger.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
4. Starten Sie den Trigger mit dem Cmdlet **Start-AzureRmDataFactoryV2Trigger**:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
5. Bestätigen Sie, dass der Status des Triggers **Started** ist, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** verwenden.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
6.  Rufen Sie Triggerausführungen mithilfe von PowerShell mit dem Cmdlet **Get-AzureRmDataFactoryV2TriggerRun** ab. Um die Informationen über Triggerausführungen zu erhalten, führen Sie den folgenden Befehl regelmäßig aus: Aktualisieren Sie die Werte **TriggerRunStartedAfter** und **TriggerRunStartedBefore**, damit sie mit den Werten in der Triggerdefinition übereinstimmen.

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Nächste Schritte
Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#triggers).

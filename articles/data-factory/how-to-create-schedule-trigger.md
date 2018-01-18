---
title: Erstellen von Zeitplantriggern in Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie in Azure Data Factory ein Trigger erstellt wird, der eine Pipeline gemäß einem Zeitplan ausführt."
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
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: 6b92e8402d372e29e264dc70b128124973d66bb9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Erstellen eines Triggers in Azure Data Factory, der eine Pipeline gemäß einem Zeitplan ausführt
Dieser Artikel enthält Informationen zum Zeitplantrigger und die Schritte zum Erstellen, Starten und Überwachen eines Triggers. Informationen zu anderen Triggertypen finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, lesen Sie die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

### <a name="schedule-trigger-json-definition"></a>JSON-Definition für Zeitplantrigger
Wenn Sie einen Zeitplantrigger erstellen, können Sie Zeitplanung und Wiederholung mithilfe von JSON wie im Beispiel in diesem Abschnitt gezeigt festlegen.

Damit der Zeitplantrigger die Ausführung der Pipeline startet, verwenden Sie in der Triggerdefinition einen Pipelineverweis auf die jeweilige Pipeline. Pipelines und Trigger haben eine m:m-Beziehung. Mehrere Trigger können eine einzelne Pipeline starten. Ein einzelnder Trigger kann mehrere Pipelines starten.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  Die **parameters**-Eigenschaft ist eine obligatorische Eigenschaft unter **pipelines**. Auch wenn für Ihre Pipeline keine Parameter verwendet werden, sollten Sie eine leere JSON-Datei für Parameter einfügen, da die Eigenschaft vorhanden sein muss.


### <a name="overview-schedule-trigger-schema"></a>Übersicht: Schema des Zeitplantriggers
Die folgende Tabelle enthält eine allgemeine Übersicht über die wichtigsten Elemente für die Wiederholung und Zeitplanung in einem Trigger:

JSON-Eigenschaft |     BESCHREIBUNG
------------- | -------------
startTime | „startTime“ ist eine Datums-/Uhrzeitangabe. Bei einfachen Zeitplänen ist „startTime“ das erste Vorkommen. Bei komplexen Zeitplänen wird der Trigger frühestens bei „startTime“ gestartet.
endTime | Gibt Datum und Uhrzeit für das Ende des Triggers an. Der Trigger wird nach diesem Zeitpunkt nicht ausgeführt. „endTime“ darf nicht in der Vergangenheit liegen. Diese Eigenschaft ist optional.
timeZone | Derzeit wird nur UTC unterstützt.
recurrence | Das recurrence-Objekt gibt Wiederholungsregeln für den Trigger an. Es unterstützt die Elemente „frequency“, „interval“, „endTime“, „count“ und „schedule“. Bei Angabe von „recurrence“ muss auch „frequency“ angegeben werden. Die anderen Elemente von „recurrence“ sind optional.
frequency | Stellt die Einheit der Häufigkeit dar, mit der der Trigger wiederholt wird. Folgende Werte werden unterstützt: `minute`, `hour`, `day`, `week` und `month`
interval | Das interval-Objekt ist eine positive ganze Zahl. Es gibt das Intervall für die Häufigkeit an, die bestimmt, wie oft der Trigger ausgeführt wird. Ist „interval“ also beispielsweise auf „3“ und „frequency“ auf „week“ festgelegt, wird der Trigger alle drei Wochen ausgeführt.
schedule | Die Wiederholung eines Triggers mit einer bestimmten Häufigkeit wird auf der Grundlage eines Wiederholungszeitplans angepasst. Ein Zeitplan enthält Anpassungen auf der Grundlage von Minuten, Stunden, Wochentagen, Monatstagen und Wochennummer.


### <a name="overview-schedule-trigger-schema-defaults-limits-and-examples"></a>Übersicht: Schemastandardwerte, Einschränkungen und Beispiele für einen Zeitplantrigger

JSON-Name | Werttyp | Erforderlich | Standardwert | Gültige Werte | Beispiel
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Zeichenfolge | Ja | Keine | Datum/Uhrzeit (nach ISO 8601) | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Objekt | Ja | Keine | Wiederholungsobjekt | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Number | Nein  | 1 | 1 bis 1000. | ```"interval":10```
endTime | Zeichenfolge | Ja | Keine | Datums-/Uhrzeitwert für einen Zeitpunkt in der Zukunft | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Objekt | Nein  | Keine | Zeitplanobjekt | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Ausführliche Betrachtung: „startTime“
Die folgende Tabelle verdeutlicht, wie „startTime“ die Ausführung eines Triggers beeinflusst:

startTime-Wert | Wiederholung ohne Zeitplan | Wiederholung mit Zeitplan
--------------- | --------------------------- | ------------------------
Startzeit in der Vergangenheit | Berechnet die erste weitere Ausführungszeit nach Startzeit und nimmt die Ausführung zu diesem Zeitpunkt vor.<p>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.</p><p>Betrachten Sie das Beispiel nach dieser Tabelle.</p> | Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird. <p>Weitere Ausführungen auf der Grundlage des Wiederholungszeitplans</p>
Startzeit in der Zukunft oder Gegenwart | Wird einmalig zur angegebenen Startzeit ausgeführt. <p>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.</p> | Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<p>Berechnet weitere Ausführungen auf Grundlage des Wiederholungszeitplans.</p>

Das folgende Beispiel veranschaulicht, was passiert, wenn „startTime“ in der Vergangenheit liegt und nur „recurrence“, aber kein Zeitplan angegeben ist. In diesem Beispiel wird davon ausgegangen, dass die aktuelle Zeit `2017-04-08 13:00` ist, „startTime“ auf `2017-04-07 14:00` festgelegt ist und für „recurrence“ ein Zwei-Tages-Intervall (mit „frequency: day“ und „interval: 2“) angegeben wurde. Beachten Sie, dass „startTime“ in der Vergangenheit liegt.

Unter diesen Umständen erfolgt die erste Ausführung um `2017-04-09 at 14:00`. Die Scheduler-Engine berechnet die Ausführungen auf Grundlage der Startzeit. In der Vergangenheit liegende Instanzen werden verworfen. Die Engine verwendet die nächste in der Zukunft liegende Instanz. In diesem Fall ist „startTime“ auf `2017-04-07 at 2:00pm` festgelegt. Die nächste Instanz folgt zwei Tage nach diesem Zeitpunkt, also um `2017-04-09 at 2:00pm`.

Die erste Ausführungszeit ist auch dieselbe, wenn als Startzeit `2017-04-05 14:00` oder `2017-04-01 14:00` angegeben ist. Nach der ersten Ausführung werden nachfolgende Ausführungen anhand des Zeitplans berechnet. Daher finden sie um `2017-04-11 at 2:00pm`, dann `2017-04-13 at 2:00pm` und dann `2017-04-15 at 2:00pm`usw. statt.

Wenn für einen Trigger ein Zeitplan, aber keine Stunden- und/oder Minutenangabe festgelegt ist, werden standardmäßig die Stunden und/oder Minuten der ersten Ausführung verwendet.

### <a name="deep-dive-schedule"></a>Ausführliche Betrachtung: Zeitplan
Mithilfe eines Zeitplans lässt sich einerseits die Anzahl der Triggerausführungen begrenzen. Beispiel: Wird für einen Trigger mit einer monatlichen Häufigkeit ein Zeitplan angegeben wird, der nur am 31. Tag ausgeführt wird, wird der Trigger nur in Monaten mit 31 Tagen ausgeführt.

Ein Zeitplan kann dagegen auch zum Erweitern der Anzahl von Triggerausführungen verwendet werden. Beispiel: Wird für einen Trigger mit einer monatlichen Häufigkeit ein Zeitplan angegeben, der am ersten und zweiten Monatstag ausgeführt wird, wird der Trigger nicht einmal im Monat, sondern jeweils am ersten und zweiten Tag des Monats ausgeführt.

Bei Angabe mehrerer Zeitplanelemente werden diese in absteigender Reihenfolge ausgewertet – also von der Wochennummer über Monatstag und Wochentag bis hin zu Stunde und Minute.

Die folgende Tabelle enthält eine ausführliche Beschreibung der Zeitplanelemente:


JSON-Name | BESCHREIBUNG | Gültige Werte
--------- | ----------- | ------------
minutes | Minuten der Stunde, zu denen der Trigger ausgeführt wird | <ul><li>Ganze Zahl </li><li>Array mit ganzen Zahlen</li></ul>
hours | Stunden des Tages, zu denen der Trigger ausgeführt wird | <ul><li>Ganze Zahl </li><li>Array mit ganzen Zahlen</li></ul>
weekDays | Tage der Woche, an denen der Trigger ausgeführt wird Kann nur bei wöchentlicher Häufigkeit angegeben werden. | <ul><li>Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag und Sonntag</li><li>Array mit beliebigen Werten (maximale Arraygröße: 7)</li></p>Keine Beachtung der Groß-/Kleinschreibung</p>
monthlyOccurrences | Bestimmt, an welchen Tagen im Monat der Trigger ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. | Array mit monthlyOccurence-Objekten: `{ "day": day,  "occurrence": occurence }` <p> Der Tag ist der Wochentag, an dem der Trigger ausgeführt wird. `{Sunday}` steht beispielsweise für jeden Sonntag im Monat. Erforderlich.<p>Das Vorkommen steht für den Tag innerhalb des Monats. Mit `{Sunday, -1}` wird beispielsweise der letzte Sonntag des Monats angegeben. Optional.
monthDays | Tag des Monats, an dem der Trigger ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. | <ul><li>Beliebiger Wert, für den Folgendes gilt: <= -1 und >= -31</li><li>Beliebiger Wert, für den Folgendes gilt: >= 1 und <= 31</li><li>Ein Array von Werten</li>


## <a name="examples-recurrence-schedules"></a>Beispiele: Wiederholungszeitpläne
Dieser Abschnitt enthält verschiedene Beispiele für Wiederholungszeitpläne, bei denen jeweils das schedule-Objekt und die dazugehörigen Elemente im Mittelpunkt stehen.

Bei den Beispielzeitplänen wird jeweils davon ausgegangen, dass „interval“ auf „1“ festgelegt ist. Darüber hinaus wird davon ausgegangen, dass für den Zeitplan jeweils eine passende Häufigkeit angegeben ist. Beispielsweise kann der Zeitplan bei Angabe einer täglichen Häufigkeit nicht mit „monthDays“ angepasst werden. Diese Einschränkungen werden in der Tabelle im vorhergehenden Abschnitt erwähnt.

Beispiel | BESCHREIBUNG
------- | -----------
`{"hours":[5]}` | Tägliche Ausführung um 5:00 Uhr
`{"minutes":[15], "hours":[5]}` | Tägliche Ausführung um 5:15 Uhr
`{"minutes":[15], "hours":[5,17]}` | Tägliche Ausführung um 5:15 Uhr und um 17:15 Uhr
`{"minutes":[15,45], "hours":[5,17]}` | Tägliche Ausführung um 5:15 Uhr, 5:45 Uhr, 17:15 Uhr und um 17:45 Uhr
`{"minutes":[0,15,30,45]}` | Ausführung alle 15 Minuten
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Stündliche Ausführung Dieser Trigger wird stündlich ausgeführt. Der Minutenwert wird von „startTime“ gesteuert. Ist hier kein Minutenwert angegeben, wird der Minutenwert des Erstellungszeitpunkts verwendet. Lautet die Start- oder Erstellungszeit also beispielsweise 12:25 Uhr, wird der Trigger um 00:25 Uhr, 01:25 Uhr, 02:25 Uhr, …, 23:25 Uhr ausgeführt. Der Zeitplan entspricht einem Trigger mit „frequency“ = „hour“, „interval“ = „1“ und ohne Angabe für „schedule“. Der Unterschied besteht darin, dass dieser Zeitplan mit unterschiedlichen Angaben für „frequency“ und „interval“ auch zur Erstellung anderer Trigger verwendet werden kann. Wird „frequency“ beispielsweise auf „month“ festgelegt, wird der Zeitplan nur einmal im Monat ausgeführt (und nicht täglich wie bei „frequency“ = „day“).
`{"minutes":[0]}` | Ausführung jeweils zur vollen Stunde Dieser Trigger wird ebenfalls stündlich ausgeführt, aber immer zur vollen Stunde (beispielsweise um 12:00 Uhr, 13:00 Uhr, 14:00 Uhr usw.). Diese Einstellung entspricht einem Trigger mit stündlicher Häufigkeit, einer Startzeit mit null Minuten und ohne Zeitplan. Bei wöchentlicher oder monatlicher Häufigkeit würde der Zeitplan dagegen nur einmal in der Woche bzw. nur einmal im Monat ausgeführt.
`{"minutes":[15]}` | Stündliche Ausführung jeweils 15 Minuten nach der vollen Stunde Der Auftrag wird ab 00:15 Uhr stündlich (also um 1:15 Uhr, 2:15 Uhr usw.) bis 23:15 Uhr ausgeführt.
`{"hours":[17], "weekDays":["saturday"]}` | Wöchentliche Ausführung, immer samstags um 17:00 Uhr
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:00 Uhr
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:15 Uhr und um 17:45 Uhr
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Ausführung an Wochentagen im 15-Minuten-Takt
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Ausführung an Wochentagen zwischen 9:00 Uhr und 16:45 Uhr im 15-Minuten-Takt
`{"weekDays":["tuesday", "thursday"]}` | Ausführung jeweils dienstags und donnerstags zur festgelegten Startzeit
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Ausführung um 6:00 Uhr am 28. Tag des Monats (bei monatlicher Häufigkeit)
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Ausführung um 6:00 Uhr am letzten Tag des Monats Wenn Sie einen Trigger am letzten Tag eines Monats ausführen möchten, verwenden Sie „-1“ anstatt Tag 28, 29, 30 oder 31.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Ausführung jeweils am ersten und letzten Tag jedes Monats um 6:00 Uhr
`{monthDays":[1,14]}` | Ausführung jeweils am ersten und 14. Tag jedes Monats zur festgelegten Startzeit
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Ausführung am ersten Freitag jedes Monats um 5:00 Uhr
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Ausführung am ersten Freitag jedes Monats zur festgelegten Startzeit
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Monatliche Ausführung am dritten Freitag vom Ende des Monats zur Startzeit
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Ausführung am ersten und letzten Freitag jedes Monats um 5:15 Uhr
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Ausführung am ersten und letzten Freitag jedes Monats zur festgelegten Startzeit
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Ausführung am fünften Freitag jedes Monats zur Startzeit Ist in einem Monat kein fünfter Freitag vorhanden, wird die Pipeline nicht ausgeführt, da die Ausführung ausschließlich für den fünften Freitag geplant ist.  Soll der Trigger am letzten Freitag des Monats ausgeführt werden, empfiehlt sich u. U. die Verwendung von „-1“ anstelle von „5“.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ausführung im 15-Minuten-Takt am letzten Freitag des Monats
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Ausführung um 5:15 Uhr, 5:45 Uhr, 17:15 Uhr und 17:45 Uhr am dritten Mittwoch jedes Monats


## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
In diesem Abschnitt erfahren Sie, wie Sie mit Azure PowerShell einen Zeitplantrigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieses Beispiel funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit Azure PowerShell](quickstart-create-data-factory-powershell.md). Fügen Sie anschließend den folgenden Code zur „main“-Methode hinzu, der einen Zeitplantrigger erstellt und startet, der alle 15 Minuten ausgeführt wird. Der Trigger ist einer Pipeline zugeordnet (**Adfv2QuickStartPipeline**), die Sie als Teil des Schnellstarts erstellen.

1. Erstellen Sie im Ordner „C:\ADFv2QuickStartPSH“ eine JSON-Datei mit dem Namen „MyTrigger.json“ und dem folgenden Inhalt:

    > [!IMPORTANT]
    > Legen Sie **startTime** auf die aktuelle UTC-Zeit und **endTime** auf eine Stunde nach der aktuellen UTC-Zeit fest, bevor Sie die JSON-Datei speichern.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```

    Im JSON-Codeausschnitt:
    - **type** des Triggers wird auf **ScheduleTrigger** festgelegt.
    - **frequency** wird auf **Minute** und **interval** auf **15** festgelegt. Deshalb führt der Trigger die Pipeline alle 15 Minuten zwischen dem Start- und Endzeitpunkt aus.
    - **endTime** ist eine Stunde nach **startTime**, weshalb der Trigger die Pipeline 15 Minuten, 30 Minuten und 45 Minuten nach „startTime“ ausführt. Vergessen Sie nicht, „startTime“ in die aktuelle UTC-Zeit und „endTime“ in eine Stunde nach „startTime“ zu ändern.  
    - Der Trigger ist der Pipeline **Adfv2QuickStartPipeline** zugeordnet. Um einen Trigger mehreren Pipelines zuzuordnen, fügen Sie weitere **pipelineReference**-Abschnitte hinzu.
    - Die Pipeline im Schnellstart akzeptiert zwei **Parameter**. Aus diesem Grund übergeben Sie Werte für diese Parameter aus dem Trigger.
2. Erstellen Sie mit dem Cmdlet **Start-AzureRmDataFactoryV2Trigger** einen Trigger.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Bestätigen Sie, dass der Status des Triggers **Stopped** ist, indem Sie das Cmdlet **Get-AzureRmDataFactoryV2Trigger** verwenden.

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

## <a name="use-net-sdk"></a>Verwenden von .NET SDK
In diesem Abschnitt erfahren Sie, wie Sie mit dem .NET SDK einen Trigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieser Code funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit .NET SDK](quickstart-create-data-factory-dot-net.md). Fügen Sie anschließend den folgenden Code zur „main“-Methode hinzu, der einen Zeitplantrigger erstellt und startet, der alle 15 Minuten ausgeführt wird. Der Trigger ist einer Pipeline zugeordnet (**Adfv2QuickStartPipeline**), die Sie als Teil des Schnellstarts erstellen.

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method.
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Fügen Sie zum Überwachen einer Triggerausführung den folgenden Code vor der letzten `Console.WriteLine`-Anweisung hinzu:

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="use-python-sdk"></a>Verwenden des Python SDK
In diesem Abschnitt erfahren Sie, wie Sie mit dem Python SDK einen Trigger erstellen, starten und überwachen. Wenn Sie sehen möchten, wie dieser Code funktioniert, durchlaufen zuerst den [Schnellstart: Erstellen einer Data Factory mit dem Python SDK](quickstart-create-data-factory-python.md). Fügen Sie dann im Python-Skript den folgenden Codeblock nach dem Codeblock „monitor the pipeline run“ hinzu. Dieser Code erzeugt einen Zeitplantrigger, der alle 15 Minuten zwischen den angegebenen Start- und Endzeiten ausgeführt wird. Ändern Sie „start_time“ in die aktuelle UTC-Zeit und „end_time“ in eine Stunde nach der aktuellen UTC-Zeit.

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC')
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Informationen zum Überwachen von Trigger- bzw. Pipelineausführungen im Azure-Portal finden Sie unter [Überwachen der Pipelineausführungen](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="use-resource-manager-template"></a>Verwenden von Resource Manager-Vorlagen
Sie können mit einer Azure Resource Manager-Vorlage einen Trigger erstellen. Detaillierte Anweisungen finden Sie unter [Erstellen einer Azure Data Factory mit einer Resource Manager-Vorlage](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Übergeben der Startzeit des Triggers an eine Pipeline
In Version 1 unterstützte Azure Data Factory das Lesen oder Schreiben von partitionierten Daten mithilfe der Systemvariablen SliceStart/SliceEnd/WindowStart/WindowEnd. In Version 2 können Sie dieses Verhalten mithilfe eines Pipelineparameters und der Anfangszeit/geplanten Zeit eines Triggers als Wert des Parameters erreichen. Im folgenden Beispiel wird die geplante Zeit des Triggers als Wert an den Pipelineparameter „scheduledRunTime“ übergeben.

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Weitere Informationen finden Sie unter [Lesen oder Schreiben partitionierter Daten](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Nächste Schritte
Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#triggers).

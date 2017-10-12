---
title: "Pipelineausführung und Trigger in Azure Data Factory | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zur Ausführung einer Pipeline in Azure Data Factory entweder bei Bedarf oder durch Erstellen eines Triggers."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: c319979cce23da69965d4fbab037919461f67b3a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Pipelineausführung und Trigger in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-scheduling-and-execution.md)
> * [Version 2 – Vorschauversion](concepts-pipeline-execution-triggers.md)

**Pipelineausführung** ist ein Begriff in Azure Data Factory, Version 2, der eine Instanz einer Pipelineausführung definiert. Beispiel: Angenommen, Sie haben eine Pipeline, die um 8:00, 9:00 und 10:00 Uhr ausgeführt wird. In diesem Fall erfolgen drei separate Ausführungen der Pipeline (Pipelineausführungen). Jede Pipelineausführung weist eine eindeutige Pipelineausführungs-ID aus. Dabei handelt es sich um eine GUID, die die jeweilige Pipelineausführung eindeutig definiert. Zur Instanziierung von Pipelineausführungen werden in der Regel Argumente an in Pipelines definierte Parameter übergeben. Es gibt zwei Möglichkeiten zum Ausführen einer Pipeline: **manuell** oder über einen **Trigger**. Dieser Artikel enthält Informationen zu beiden Möglichkeiten der Ausführung einer Pipeline. 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Data Factory – Planung und Ausführung](v1/data-factory-scheduling-and-execution.md) für V1.

## <a name="run-pipeline-on-demand"></a>Pipeline bedarfsgesteuert ausführen
Bei dieser Methode führen Sie Ihre Pipeline manuell aus. Sie wird auch als eine bedarfsgesteuerte Ausführung einer Pipeline betrachtet. 

Beispiel: Angenommen, Sie haben eine Pipeline mit dem Namen **CopyPipeline**, die Sie ausführen möchten. Dabei handelt es sich um eine einfache Pipeline mit einer einzelnen Aktivität, die Daten aus einem Quellordner in Azure Blob Storage in einen Zielordner im selben Speicher kopiert. Definition der Beispielpipeline: 

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
Die Pipeline nutzt zwei Parameter: „sourceBlobContainer“ und „sinkBlobContainer“, wie in der JSON-Definition dargestellt. Sie können Werte an diese Parameter zur Laufzeit übergeben. 

Zur manuellen Ausführung der Pipeline können Sie eine der folgenden Methoden verwenden: .NET, PowerShell, REST und Python. 

### <a name="rest-api"></a>REST-API
Beispiel für einen REST-Befehl:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Ein vollständiges Beispiel finden Sie unter [Quickstart: create a data factory using REST API](quickstart-create-data-factory-rest-api.md) (Schnellstart: Erstellen einer Data Factory mithilfe der REST-API).

### <a name="powershell"></a>PowerShell
Beispiel für einen PowerShell-Befehl: 

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Sie übergeben Parameter im Text in der Anforderungsnutzlast. In .NET, Powershell und Python werden die Werte in einem Wörterbuch übergeben, das als Argument an den Aufruf übergeben übergeben wird.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

Die Antwortnutzlast ist eine eindeutige ID der Pipelineausführung:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Ein vollständiges Beispiel finden Sie unter [Quickstart: create a data factory using PowerShell](quickstart-create-data-factory-powershell.md) (Schnellstart: Erstellen einer Data Factory mithilfe von PowerShell).

### <a name="net"></a>.NET 
Beispiel für einen .NET-Aufruf: 

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Ein vollständiges Beispiel finden Sie unter [Quickstart: create a data factory using .NET](quickstart-create-data-factory-dot-net.md) (Schnellstart: Erstellen einer Data Factory mithilfe von .NET).

> [!NOTE]
> Mit der .NET API können Sie Data Factory-Pipelines in Azure Functions, Ihren eigenen Webdiensten usw. aufrufen.

## <a name="triggers"></a>Trigger
Trigger bieten die zweite Möglichkeit zur Ausführung einer Pipeline. Trigger stellen eine Verarbeitungseinheit dar, die bestimmt, wann eine Pipelineausführung initiiert werden soll. Data Factory unterstützt derzeit einen Trigger, der eine Pipeline nach einem Realzeitplan aufruft. Es heißt **Planer-Trigger**. Derzeit unterstützt Data Factory keine ereignisbasierten Trigger wie einen Trigger einer Pipelineausführung im Fall eines Dateieingangs.

Pipelines und Trigger haben eine n:m-Beziehung. Mehrere Trigger können eine einzelne Pipeline starten, und ein einzelner Trigger kann mehrere Pipelines starten. In der folgenden JSON-Definition eines Triggers bezieht sich die **Pipelines**-Eigenschaft auf eine Liste der Pipelines, die vom jeweiligen Trigger ausgelöst werden, sowie auf Werte für Pipeline-Parameter.

### <a name="basic-trigger-definition"></a>Grundlegende Triggerdefinition: 
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
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
```

## <a name="scheduler-trigger"></a>Planer-Trigger
Der Planer-Trigger führt Pipelines nach einem Realzeitplan aus. Dieser Trigger unterstützt periodische und erweiterte Kalenderoptionen (wöchentlich, Montag um 17:00 Uhr und Donnerstag um 9:00 Uhr). Er ist insofern flexibel, als er von DataSet-Mustern unabhängig ist und nicht zwischen Zeitreihendaten und Nicht-Zeitreihendaten unterscheidet.

### <a name="scheduler-trigger-json-definition"></a>JSON-Definition für Planer-Trigger
Wenn Sie einen Planer-Trigger erstellen, können Sie die Zeitplanung und Wiederholung mithilfe von JSON wie im Beispiel in diesem Abschnitt gezeigt festlegen. 

Damit der Planer-Trigger die Ausführung der Pipeline startet, verwenden Sie in der Triggerdefinition einen Pipelineverweis auf die jeweilige Pipeline. Pipelines und Trigger haben eine n:m-Beziehung. Mehrere Trigger können eine einzelne Pipeline starten. Derselbe Trigger kann mehrere Pipelines starten.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": <<default UTC>>
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ] 
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
                    "<parameter 2 Name> : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

### <a name="overview-scheduler-trigger-schema"></a>Übersicht: Schema des Planer-Triggers
Die folgende Tabelle enthält eine allgemeine Übersicht über die wichtigsten Elemente für die Wiederholung und Zeitplanung in einem Trigger:

JSON-Eigenschaft |     Beschreibung
------------- | -------------
startTime | „startTime“ ist eine Datums-/Uhrzeitangabe. Bei einfachen Zeitplänen ist „startTime“ das erste Vorkommen. Bei komplexen Zeitplänen wird der Trigger frühestens bei „startTime“ gestartet.
recurrence | Das recurrence-Objekt gibt Wiederholungsregeln für den Trigger an. Es unterstützt die Elemente „frequency“, „interval“, „endTime“, „count“ und „schedule“. Bei Angabe von „recurrence“ muss auch „frequency“ angegeben werden. Die anderen Elemente von „recurrence“ sind optional.
frequency | Stellt die Einheit der Häufigkeit dar, mit der der Trigger wiederholt wird. Folgende Werte werden unterstützt: `minute`, `hour`, `day`, `week` und `month`
interval | Das interval-Objekt ist eine positive ganze Zahl. Es gibt das Intervall für die Häufigkeit an, die bestimmt, wie oft der Trigger ausgeführt wird. Ist „interval“ also beispielsweise auf „3“ und „frequency“ auf „week“ festgelegt, wird der Trigger alle drei Wochen ausgeführt.
endTime | Gibt Datum und Uhrzeit für das Ende des Triggers an. Der Trigger wird nach diesem Zeitpunkt nicht ausgeführt. „endTime“ darf nicht in der Vergangenheit liegen.
schedule | Die Wiederholung eines Triggers mit einer bestimmten Häufigkeit wird auf der Grundlage eines Wiederholungszeitplans angepasst. Ein Zeitplan enthält Anpassungen auf der Grundlage von Minuten, Stunden, Wochentagen, Monatstagen und Wochennummer.

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Übersicht: Standardwerte, Einschränkungen und Beispiele Planer-Trigger-Schema

JSON-Name | Werttyp | Erforderlich | Standardwert | Gültige Werte | Beispiel
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | String | Ja | Keine | Datum/Uhrzeit (nach ISO 8601) | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Objekt | Ja | Keine | Wiederholungsobjekt | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Number | Nein | 1 | 1 bis 1000. | ```"interval":10```
endTime | String | Ja | Keine | Datums-/Uhrzeitwert für einen Zeitpunkt in der Zukunft | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Objekt | Nein | Keine | Zeitplanobjekt | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Ausführliche Betrachtung: „startTime“
Die folgende Tabelle verdeutlicht, wie „startTime“ die Ausführung eines Triggers beeinflusst:

startTime-Wert | Wiederholung ohne Zeitplan | Wiederholung mit Zeitplan
--------------- | --------------------------- | ------------------------
Startzeit in der Vergangenheit | Berechnet die erste weitere Ausführungszeit nach Startzeit und nimmt die Ausführung zu diesem Zeitpunkt vor.<p>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.</p><p>Betrachten Sie das Beispiel nach dieser Tabelle.</p> | Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird. <p>Weitere Ausführungen auf der Grundlage des Wiederholungszeitplans</p>
Startzeit in der Zukunft oder Gegenwart | Wird einmalig zur angegebenen Startzeit ausgeführt. <p>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.</p> | Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<p>Berechnet weitere Ausführungen auf Grundlage des Wiederholungszeitplans.</p>

Das folgende Beispiel veranschaulicht, was passiert, wenn „startTime“ in der Vergangenheit liegt und nur „recurrence“, aber kein Zeitplan angegeben ist. In diesem Beispiel wird davon ausgegangen, dass die aktuelle Zeit `2017-04-08 13:00` ist, „startTime“ auf `2017-04-07 14:00` festgelegt ist und für „recurrence“ ein Zwei-Tages-Intervall (mit „frequency: day“ und „interval: 2“) angegeben wurde. Beachten Sie, dass „startTime“ in der Vergangenheit liegt.

Unter diesen Umständen erfolgt die erste Ausführung um `2017-04-09 at 14:00`. Das Scheduler-Modul berechnet die Ausführungen auf Grundlage der Startzeit. In der Vergangenheit liegende Instanzen werden verworfen. Das Modul verwendet die nächste in der Zukunft liegende Instanz. In diesem Fall ist „startTime“ auf `2017-04-07 at 2:00pm` festgelegt. Die nächste Instanz folgt zwei Tage nach diesem Zeitpunkt, also um `2017-04-09 at 2:00pm`.

Die erste Ausführungszeit ist auch dieselbe, wenn als Startzeit `2017-04-05 14:00` oder `2017-04-01 14:00` angegeben ist. Nach der ersten Ausführung werden nachfolgende Ausführungen anhand des Zeitplans berechnet. Daher finden sie um `2017-04-11 at 2:00pm`, dann `2017-04-13 at 2:00pm` und dann `2017-04-15 at 2:00pm`usw. statt.

Wenn für einen Trigger ein Zeitplan, aber keine Stunden- und/oder Minutenangabe festgelegt ist, werden standardmäßig die Stunden und/oder Minuten der ersten Ausführung verwendet.

### <a name="deep-dive-schedule"></a>Ausführliche Betrachtung: Zeitplan
Mithilfe eines Zeitplans lässt sich einerseits die Anzahl der Triggerausführungen begrenzen. Beispiel: Wird für einen Trigger mit einer monatlichen Häufigkeit ein Zeitplan angegeben wird, der nur am 31. Tag ausgeführt wird, wird der Trigger nur in Monaten mit 31 Tagen ausgeführt.

Andererseits kann ein Zeitplan auch zum Erweitern der Anzahl von Triggerausführungen verwendet werden. Beispiel: Wird für einen Trigger mit einer monatlichen Häufigkeit ein Zeitplan angegeben, der am ersten und zweiten Monatstag ausgeführt wird, wird der Trigger nicht einmal im Monat, sondern jeweils am ersten und zweiten Tag des Monats ausgeführt.

Bei Angabe mehrerer Zeitplanelemente werden diese in absteigender Reihenfolge ausgewertet – also von der Wochennummer über Monatstag und Wochentag bis hin zu Stunde und Minute.

Die folgende Tabelle enthält eine ausführliche Beschreibung der Zeitplanelemente:


JSON-Name | Beschreibung | Gültige Werte
--------- | ----------- | ------------
minutes | Minuten der Stunde, zu denen der Trigger ausgeführt wird | <ul><li>Integer</li><li>Array mit ganzen Zahlen</li></ul>
hours | Stunden des Tages, zu denen der Trigger ausgeführt wird | <ul><li>Integer</li><li>Array mit ganzen Zahlen</li></ul>
weekDays | Tage der Woche, an denen der Trigger ausgeführt wird Kann nur bei wöchentlicher Häufigkeit angegeben werden. | <ul><li>Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag und Sonntag</li><li>Array mit beliebigen der oben angegebenen Werte (maximale Arraygröße: 7)</li></p>Keine Beachtung der Groß-/Kleinschreibung</p>
monthlyOccurrences | Bestimmt, an welchen Tagen im Monat der Trigger ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. | Array mit monthlyOccurence-Objekten: `{ "day": day,  "occurrence": occurence }` <p> Der Tag ist der Wochentag, an dem der Trigger ausgeführt wird. `{Sunday}` steht beispielsweise für jeden Sonntag im Monat. Erforderlich.<p>Das Vorkommen steht für den Tag innerhalb des Monats. Mit `{Sunday, -1}` wird beispielsweise der letzte Sonntag des Monats angegeben. Optional.
monthDays | Tag des Monats, an dem der Trigger ausgeführt wird. Kann nur bei monatlicher Häufigkeit angegeben werden. | <ul><li>Beliebiger Wert, für den Folgendes gilt: <= -1 und >= -31</li><li>Beliebiger Wert, für den Folgendes gilt: >= 1 und <= 31</li><li>Array mit den oben genannten Werten</li>


## <a name="examples-recurrence-schedules"></a>Beispiele: Wiederholungszeitpläne
Dieser Abschnitt enthält verschiedene Beispiele für Wiederholungszeitpläne, bei denen jeweils das schedule-Objekt und die dazugehörigen Elemente im Mittelpunkt stehen.

Bei den Beispielzeitplänen wird jeweils davon ausgegangen, dass „interval“ auf „1“ festgelegt ist. Darüber hinaus wird davon ausgegangen, dass für den Zeitplan jeweils eine passende Häufigkeit angegeben ist. Beispielsweise kann der Zeitplan bei Angabe einer täglichen Häufigkeit nicht mit „monthDays“ angepasst werden. Diese Einschränkungen werden in der Tabelle im vorhergehenden Abschnitt erwähnt. 

Beispiel | Beschreibung
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




## <a name="next-steps"></a>Nächste Schritte
Arbeiten Sie die folgenden Tutorials durch: 

- [Schnellstart: Erstellen einer Data Factory mithilfe von .NET](quickstart-create-data-factory-dot-net.md)

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
ms.date: 01/03/2018
ms.author: shlo
ms.openlocfilehash: fc34cfbab796c6e1e4cd25ce13dcc63c39c6699d
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Pipelineausführung und Trigger in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Version 1: allgemein verfügbar](v1/data-factory-scheduling-and-execution.md)
> * [Version 2 – Vorschauversion](concepts-pipeline-execution-triggers.md)

_Pipelineausführung_ ist ein Begriff in Azure Data Factory, Version 2, der eine Instanz einer Pipelineausführung definiert. Beispiel: Angenommen, Sie verfügen über eine Pipeline, die um 8:00, 9:00 und 10:00 Uhr ausgeführt wird. In diesem Fall erfolgen drei separate Ausführungen der Pipeline (oder Pipelineausführungen). Jede Pipelineausführung weist eine eindeutige Pipelineausführungs-ID aus. Dabei handelt es sich um eine GUID, die die jeweilige Pipelineausführung eindeutig definiert. Zur Instanziierung von Pipelineausführungen werden in der Regel Argumente an in Pipelines definierte Parameter übergeben. Es gibt zwei Möglichkeiten zum Ausführen einer Pipeline: _manuell_ oder über einen Trigger. Dieser Artikel enthält Informationen zu beiden Möglichkeiten der Ausführung einer Pipeline.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Azure Data Factory, die sich derzeit in der Vorschauphase befindet. Wenn Sie Version 1 von Azure Data Factory verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Planung und Ausführung in Azure Data Factory Version 1](v1/data-factory-scheduling-and-execution.md).

## <a name="manual-execution-on-demand"></a>Manuelle Ausführung (bei Bedarf)
Die manuelle Ausführung einer Pipeline wird auch als _bedarfsgesteuerte_ Ausführung bezeichnet.

Beispiel: Angenommen, Sie haben eine einfache Pipeline mit dem Namen **CopyPipeline**, die Sie ausführen möchten. Dabei handelt es sich um eine Pipeline mit einer einzelnen Aktivität, die Daten aus einem Azure Blob Storage-Quellordner in einen Zielordner im selben Speicher kopiert. Die folgende JSON-Definition zeigt diese Beispielpipeline:

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

In der JSON-Definition nutzt die Pipeline nutzt zwei Parameter: **sourceBlobContainer** und **sinkBlobContainer**. Sie können Werte an diese Parameter zur Laufzeit übergeben.

Sie können Ihre Pipeline manuell mithilfe der folgenden Methoden ausführen:
- .NET SDK
- Azure PowerShell-Modul
- REST-API
- Python SDK

### <a name="the-rest-api"></a>REST-API
Der folgende Befehl zeigt, wie Ihre Pipeline manuell mithilfe der REST-API ausgeführt wird:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```

Ein vollständiges Beispiel finden Sie unter [Schnellstart: Erstellen einer Azure Data Factory und einer Pipeline mithilfe der REST-API](quickstart-create-data-factory-rest-api.md).

### <a name="azure-powershell"></a>Azure PowerShell
Der folgende Befehl zeigt, wie Ihre Pipeline manuell mithilfe von Azure PowerShell ausgeführt wird:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Sie übergeben Parameter im Text in der Anforderungsnutzlast. Im .NET SDK, in Azure Powershell und im Python SDK werden die Werte in einem Wörterbuch übergeben, das als Argument an den Aufruf übergeben wird:

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

Ein vollständiges Beispiel finden Sie unter [Schnellstart: Erstellen einer Azure Data Factory mithilfe von PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="the-net-sdk"></a>.NET SDK
Der folgende Beispielaufruf zeigt, wie Ihre Pipeline manuell mithilfe des .NET SDK ausgeführt wird:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Ein vollständiges Beispiel finden Sie unter [Schnellstart: Erstellen einer Data Factory und Pipeline mit dem .NET SDK](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> Mit dem .NET SDK können Sie Azure Data Factory-Pipelines in Azure Functions, Ihren eigenen Webdiensten usw. aufrufen.

<h2 id="triggers">Triggerausführung</h2>
Trigger sind die zweite Möglichkeit zur Ausführung einer Pipeline. Trigger stellen eine Verarbeitungseinheit dar, die bestimmt, wann eine Pipelineausführung initiiert werden soll. Derzeit unterstützt Azure Data Factory zwei Arten von Triggern:
- Zeitplantrigger: ein Trigger, der eine Pipeline nach einem Realzeitplan aufruft.
- Trigger für ein rollierendes Fenster: ein Trigger, der in einem regelmäßigen Intervall ausgeführt wird, während der Zustand beibehalten wird. Azure Data Factory unterstützt derzeit keine ereignisbasierten Trigger. Zu diesen gehört z.B. der Trigger für eine Pipelineausführung als Reaktion auf ein Dateieingangsereignis.

Pipelines und Trigger haben eine m:m-Beziehung. Mehrere Trigger können eine einzelne Pipeline starten, oder ein einzelner Trigger kann mehrere Pipelines starten. In der folgenden Triggerdefinition bezieht sich die **Pipelines**-Eigenschaft auf eine Liste von Pipelines, die vom jeweiligen Trigger ausgelöst werden. Die Eigenschaftendefinition enthält Werte für die Pipelineparameter.

### <a name="basic-trigger-definition"></a>Grundlegende Triggerdefinition

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

## <a name="schedule-trigger"></a>Zeitplantrigger
Ein Zeitplantrigger führt Pipelines nach einem Realzeitplan aus. Dieser Trigger unterstützt regelmäßige und erweiterte Kalenderoptionen. Der Trigger unterstützt z.B. Intervalle wie „wöchentlich“ oder „Montag um 17:00 Uhr und Donnerstag um 21:00 Uhr“. Der Zeitplantrigger ist flexibel, da das DataSet-Muster agnostisch ist und der Trigger nicht zwischen Zeitreihendaten und Nicht-Zeitreihendaten unterscheidet.

Weitere Informationen zu Zeitplantriggern und Beispiele finden Sie unter [Erstellen eines Triggers in Azure Data Factory, der eine Pipeline gemäß einem Zeitplan ausführt](how-to-create-schedule-trigger.md).

## <a name="tumbling-window-trigger"></a>Trigger für ein rollierendes Fenster
Trigger für ein rollierendes Fenster werden ab einem angegebenen Startzeitpunkt in regelmäßigen Zeitintervallen ausgelöst, während der Zustand beibehalten wird. Bei rollierenden Fenstern handelt es sich um eine Reihe von nicht überlappenden, aneinandergrenzenden Zeitintervallen mit einer festen Größe. Weitere Informationen zu Triggern für ein rollierendes Fenster und Beispiele finden Sie unter [Erstellen eines Triggers in Azure Data Factory, der eine Pipeline gemäß einem rollierenden Fenster ausführt](how-to-create-tumbling-window-trigger.md).

## <a name="schedule-trigger-definition"></a>Definition für Zeitplantrigger
Wenn Sie einen Zeitplantrigger erstellen, geben den Zeitplan und die Wiederholung mithilfe einer JSON-Definition an. 

Damit der Zeitplantrigger die Ausführung der Pipeline startet, verwenden Sie in der Triggerdefinition einen Pipelineverweis auf die jeweilige Pipeline. Pipelines und Trigger haben eine m:m-Beziehung. Mehrere Trigger können eine einzelne Pipeline starten. Ein einzelnder Trigger kann mehrere Pipelines starten.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // How often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
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
> Die **parameters**-Eigenschaft ist eine obligatorische Eigenschaft des **pipelines**-Elements. Wenn für Ihre Pipeline keine Parameter verwendet werden, müssen Sie eine leere JSON-Datei für die **parameters**-Eigenschaft einfügen.

### <a name="schema-overview"></a>Schemaübersicht
Die folgende Tabelle enthält eine allgemeine Übersicht über die wichtigsten Schemaelemente im Zusammenhang mit der Wiederholung und Zeitplanung eines Triggers:

| JSON-Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| **startTime** | Ein Datums-/Uhrzeitwert. Bei einfachen Zeitplänen gilt der Wert der **startTime**-Eigenschaft für das erste Vorkommen. Bei komplexen Zeitplänen wird der Trigger frühestens beim festgelegten **startTime**-Wert gestartet. |
| **endTime** | Enddatum und -uhrzeit für den Trigger. Der Trigger wird nicht nach dem angegebenen Enddatum und der Enduhrzeit ausgeführt. Der Wert für die Eigenschaft kann nicht in der Vergangenheit liegen. <!-- This property is optional. --> |
| **timeZone** | Die Zeitzone. Derzeit wird nur die UTC-Zeitzone unterstützt. |
| **recurrence** | Ein recurrence-Objekt, das die Wiederholungsregeln für den Trigger angibt. Das recurrence-Objekt unterstützt die Elemente **frequency**, **interval**, **endTime**, **count** und **schedule**. Wenn ein recurrence-Objekt definiert ist, ist das **frequency**-Element erforderlich. Die anderen Elemente des recurrence-Objekts sind optional. |
| **frequency** | Die Einheit der Häufigkeit, mit welcher der Trigger wiederholt wird. Zu den unterstützten Werten gehören „minute“, „hour“, „day“, „week“ und „month“. |
| **interval** | Eine positive ganze Zahl, die das Intervall für den **frequency**-Wert angibt, der bestimmt, wie oft der Trigger ausgeführt wird. Ist **interval** also beispielsweise auf „3“ und **frequency** auf „week“ festgelegt, wird der Trigger alle drei Wochen ausgeführt. |
| **schedule** | Der Wiederholungszeitplan für den Trigger. Die Wiederholung eines Triggers mit einem festgelegten **frequency**-Wert wird auf der Grundlage eines Wiederholungszeitplans angepasst. Die **schedule**-Eigenschaft enthält Anpassungen für die Wiederholung auf der Grundlage von Minuten, Stunden, Wochentagen, Monatstagen und Wochennummer.

### <a name="schedule-trigger-example"></a>Beispiel für Zeitplantrigger

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="schema-defaults-limits-and-examples"></a>Schemastandards, Einschränkungen und Beispiele

| JSON-Eigenschaft | Typ | Erforderlich | Standardwert | Gültige Werte | Beispiel |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Zeichenfolge | Ja | Keine | Datum/Uhrzeit (nach ISO 8601) | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Objekt | Ja | Keine | Wiederholungsobjekt | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Number | Nein  | 1 | 1 bis 1.000 | `"interval":10` |
| **endTime** | Zeichenfolge | Ja | Keine | Eine Datums-/Uhrzeitwert, der eine Zeit in der Zukunft darstellt. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Objekt | Nein  | Keine | Zeitplanobjekt | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>startTime-Eigenschaft
Die folgende Tabelle zeigt, wie die **startTime**-Eigenschaft eine Triggerausführung steuert:

| startTime-Wert | Wiederholung ohne Zeitplan | Wiederholung mit Zeitplan |
|:--- |:--- |:--- |
| Startzeit in der Vergangenheit | Berechnet die erste weitere Ausführungszeit nach der Startzeit und nimmt die Ausführung zu diesem Zeitpunkt vor.<br/><br/>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit.<br/><br/>Betrachten Sie das Beispiel nach dieser Tabelle. | Der Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<br/><br/>Berechnet weitere Ausführungen auf Grundlage des Wiederholungszeitplans. |
| Startzeit in der Zukunft oder Gegenwart | Wird einmalig zur angegebenen Startzeit ausgeführt.<br/><br/>Berechnet weitere Ausführungen auf Grundlage der letzten Ausführungszeit. | Der Trigger startet _frühestens_ zur angegebenen Startzeit. Das erste Vorkommen basiert auf dem Zeitplan, der auf der Grundlage der Startzeit berechnet wird.<br/><br/>Berechnet weitere Ausführungen auf Grundlage des Wiederholungszeitplans. |

Das folgende Beispiel veranschaulicht, was passiert, wenn die Startzeit in der Vergangenheit liegt und nur eine Wiederholung, aber kein Zeitplan angegeben ist. Angenommen, die aktuelle Uhrzeit ist `2017-04-08 13:00`, die Startzeit ist `2017-04-07 14:00`, und die Wiederholung erfolgt alle zwei Tage. (Der **recurrence**-Wert wird definiert, indem die **frequency**-Eigenschaft auf „day“ und die **interval**-Eigenschaft auf „2“ festgelegt wird.) Beachten Sie, dass der **startTime**-Wert in der Vergangenheit liegt.

Unter diesen Umständen erfolgt die erste Ausführung um `2017-04-09 at 14:00`. Die Scheduler-Engine berechnet die Ausführungen auf Grundlage der Startzeit. In der Vergangenheit liegende Instanzen werden verworfen. Die Engine verwendet die nächste in der Zukunft liegende Instanz. In diesem Szenario ist die Startzeit `2017-04-07 at 2:00pm`, sodass die nächste Instanz zwei Tage nach diesem Zeitpunkt folgt, also um `2017-04-09 at 2:00pm`.

Die erste Ausführungszeit ist auch dieselbe, wenn der **startTime**-Wert `2017-04-05 14:00` oder `2017-04-01 14:00` ist. Nach der ersten Ausführung werden nachfolgende Ausführungen anhand des Zeitplans berechnet. Daher erfolgen die weiteren Ausführungen um `2017-04-11 at 2:00pm`, dann um `2017-04-13 at 2:00pm`, dann um `2017-04-15 at 2:00pm` und so weiter.

Wenn die Stunden oder Minuten im Zeitplan für einen Trigger nicht festgelegt sind, werden die Stunden oder Minuten der ersten Ausführung als Standardwerte verwendet.

### <a name="schedule-property"></a>schedule-Eigenschaft
Mithilfe eines Zeitplans lässt sich einerseits die Anzahl der Triggerausführungen begrenzen. Beispiel: Wird für einen Trigger mit einer monatlichen Häufigkeit nur die Ausführung am 31. Tag geplant ist, wird der Trigger nur in Monaten mit 31 Tagen ausgeführt.

Ein Zeitplan kann dagegen auch zum Erweitern der Anzahl von Triggerausführungen verwendet werden. Beispiel: Ein Trigger mit einer monatlichen Frequenz, dessen Ausführung für die Monatstage 1 und 2 geplant ist, wird an den 1. und 2. Tagen des Monats anstatt einmal im Monat ausgeführt.

Bei Angabe mehrerer **schedule**-Elemente werden diese in absteigender Reihenfolge der Zeitplaneinstellungen ausgewertet. Die Auswertung beginnt mit der Wochennummer und wird mit dem Monatstag, dem Wochentag, der Stunde und schließlich der Minute fortgesetzt.

Die folgende Tabelle enthält eine ausführliche Beschreibung der **schedule**-Elemente:

| JSON-Element | BESCHREIBUNG | Gültige Werte |
|:--- |:--- |:--- |
| **minutes** | Minuten der Stunde, zu denen der Trigger ausgeführt wird | <ul><li>Ganze Zahl </li><li>Array mit ganzen Zahlen</li></ul>
| **hours** | Stunden des Tages, zu denen der Trigger ausgeführt wird | <ul><li>Ganze Zahl </li><li>Array mit ganzen Zahlen</li></ul> |
| **weekDays** | Tage der Woche, an denen der Trigger ausgeführt wird. Der Wert kann nur bei wöchentlicher Häufigkeit angegeben werden. | <ul><li>Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag</li><li>Array von Tageswerten (die maximale Arraygröße ist 7)</li><li>Bei Tageswerten wird nicht zwischen Groß- und Kleinschreibung unterschieden.</li></ul> |
| **monthlyOccurrences** | Tage des Monats, an denen der Trigger ausgeführt wird. Der Wert kann nur bei monatlicher Häufigkeit angegeben werden. | <ul><li>Array von **monthlyOccurrence**-Objekten: `{ "day": day,  "occurrence": occurence }`.</li><li>Das **day**-Attribut ist der Tag der Woche, an dem der Trigger ausgeführt wird. Beispiel: Eine **monthlyOccurrences**-Eigenschaft mit dem **day**-Wert `{Sunday}` bedeutet jeden Sonntag des Monats. Das **day**-Attribut ist erforderlich.</li><li>Das **occurrence**-Attribut ist das Vorkommen des angegebenen **Tags** innerhalb des Monats. Beispiel: Eine **monthlyOccurrences**Eigenschaft mit dem **day**- und **occurrence**-Wert `{Sunday, -1}` bedeutet den letzten Sonntag des Monats. Das **occurrence**-Attribut ist optional.</li></ul> |
| **monthDays** | Tag des Monats, an dem der Trigger ausgeführt wird. Der Wert kann nur bei monatlicher Häufigkeit angegeben werden. | <ul><li>Beliebiger Wert, für den Folgendes gilt: <= -1 und >= -31</li><li>Beliebiger Wert, für den Folgendes gilt: >= 1 und <= 31</li><li>Array von Werten</li></ul> |

## <a name="examples-of-trigger-recurrence-schedules"></a>Beispiele für Wiederholungszeitpläne von Triggern
Dieser Abschnitt enthält verschiedene Beispiele für Wiederholungszeitpläne, bei denen jeweils das **schedule**-Objekt und die dazugehörigen Elemente im Mittelpunkt stehen.

In den Beispielen wird angenommen, dass der **interval**-Wert 1 festgelegt ist, und der **frequency**-Wert gemäß der Zeitplandefinition richtig ist. Beispielsweise kann nicht gleichzeitig der **frequency**-Wert „day“ und die Anpassung „monthDays“ im **schedule**-Objekt angegeben werden. Solche Einschränkungen werden in der Tabelle im vorhergehenden Abschnitt erwähnt.

| Beispiel | BESCHREIBUNG |
|:--- |:--- |
| `{"hours":[5]}` | Ausführung täglich um 17:00 Uhr. |
| `{"minutes":[15], "hours":[5]}` | Ausführung täglich um 5:15 Uhr. |
| `{"minutes":[15], "hours":[5,17]}` | Ausführung täglich um 5:15 und 17:15 Uhr. |
| `{"minutes":[15,45], "hours":[5,17]}` | Ausführung täglich um 5:15 Uhr, 5:45 Uhr, 17:15 Uhr und 17:45 Uhr. |
| `{"minutes":[0,15,30,45]}` | Ausführung alle 15 Minuten. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Stündliche Ausführung. Dieser Trigger wird stündlich ausgeführt. Die Minuten werden vom **startTime**-Wert gesteuert, wenn ein Wert angegeben ist. Wenn kein Wert angegeben ist, werden die Minuten von der Erstellungszeit gesteuert. Lautet die Start- oder Erstellungszeit also beispielsweise 12:25 Uhr, wird der Trigger um 00:25 Uhr, 01:25 Uhr, 02:25 Uhr, …, und 23:25 Uhr ausgeführt.<br/><br/>Dieser Zeitplan entspricht einem Trigger mit dem **frequency**-Wert „hour“, dem **interval**-Wert „1“ und ohne **schedule**-Wert. Dieser Zeitplan kann mit anderen Werten für **frequency** und **interval** auch zur Erstellung anderer Trigger verwendet werden. Beispiel: Mit dem **frequency**-Wert „month“ wird der Zeitplan nur einmal im Monat anstatt jeden Tag wie mit dem **frequency**-Wert „day“ ausgeführt. |
| `{"minutes":[0]}` | Ausführung jeweils zur vollen Stunde. Dieser Trigger wird immer zur vollen Stunde ab 12:00 Uhr, dann 1:00 Uhr, 2:00 Uhr und so weiter, ausgeführt.<br/><br/>Dieser Zeitplan entspricht einem Trigger mit dem **frequency**-Wert „hour“ und einem **startTime**-Wert von null Minuten bzw. ohne **Zeitplan**, aber dem **frequency**-Wert „day“. Wenn der **frequency**-Wert „week“ oder „month“ ist, wird der Zeitplan nur an einem Tag in der Woche bzw. im Monat ausgeführt. |
| `{"minutes":[15]}` | Stündliche Ausführung jeweils 15 Minuten nach der vollen Stunde. Dieser Trigger wird stündlich immer 15 Minuten nach der vollen Stunde ab 0:15 Uhr, dann 1:15 Uhr, 2:15 Uhr und so weiter ausgeführt und endet um 23:15 Uhr. |
| `{"hours":[17], "weekDays":["saturday"]}` | Wöchentliche Ausführung, immer samstags um 17:00 Uhr. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:00 Uhr. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wöchentliche Ausführung am Montag, Mittwoch und Freitag, jeweils um 17:15 und 17:45 Uhr. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Ausführung an Wochentagen im 15-Minuten-Takt. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Ausführung an Wochentagen zwischen 9:00 Uhr und 16:45 Uhr im 15-Minuten-Takt. |
| `{"weekDays":["tuesday", "thursday"]}` | Ausführung jeweils dienstags und donnerstags zur festgelegten Startzeit. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Ausführung um 6:00 Uhr am 28. Tag des Monats (beim **frequency**-Wert „month“). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Ausführung um 6:00 Uhr am letzten Tag des Monats. Zur Ausführung eines Triggers am letzten Tag eines Monats verwenden Sie „-1“ anstatt Tag 28, 29, 30 oder 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Ausführung jeweils am ersten und letzten Tag jedes Monats um 6:00 Uhr. |
| `{monthDays":[1,14]}` | Ausführung jeweils am ersten und 14. Tag jedes Monats zur festgelegten Startzeit. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Ausführung am ersten Freitag jedes Monats um 5:00 Uhr. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Ausführung am ersten Freitag jedes Monats zur festgelegten Startzeit. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Monatliche Ausführung am dritten Freitag ab Monatsende zur festgelegten Startzeit. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Ausführung am ersten und letzten Freitag jedes Monats um 5:15 Uhr. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Ausführung am ersten und letzten Freitag jedes Monats zur festgelegten Startzeit. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Ausführung am fünften Freitag jedes Monats zur festgelegten Startzeit. Wenn in einem Monat kein fünfter Freitag vorhanden ist, wird die Pipeline nicht ausgeführt, da die Ausführung ausschließlich für den fünften Freitag geplant ist. Zur Ausführung des Triggers am letzten Freitag des Monats empfiehlt sich u.U. die Verwendung von „-1“ anstelle von „5“ als **occurrence**-Wert. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ausführung im 15-Minuten-Takt am letzten Freitag des Monats. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Ausführung um 5:15, 5:45, 17:15 und 17:45 Uhr am dritten Mittwoch jedes Monats. |

## <a name="trigger-type-comparison"></a>Vergleich von Triggertypen
Wenn der Trigger für ein rollierendes Fenster und der Zeitplantrigger jeweils auf Zeit-Heartbeats basieren, inwiefern unterscheiden sie sich dann?

In der folgenden Tabelle werden der Trigger für ein rollierendes Fenster und der Zeitplantrigger verglichen:

|  | Trigger&nbsp;für&nbsp;rollierendes Fenster | Zeitplan&nbsp;trigger |
|:--- |:--- |:--- |
| **Abgleich&nbsp;szenarien** | Unterstützt. Pipelineausführungen können für Fenster in der Vergangenheit geplant werden. | Nicht unterstützt. Pipelineausführungen können nur in Zeiträumen ab der aktuellen Zeit und der Zukunft ausgeführt werden. |
| **Zuverlässigkeit** | 100 % zuverlässig. Pipelineausführungen können für alle Fenster ab einem festgelegten Datum ohne Lücken geplant werden. | Weniger zuverlässig. |
| **Wiederholungs&nbsp;funktion** | Unterstützt. Für fehlgeschlagene Pipelineausführungen gilt standardmäßig eine Wiederholungsrichtlinie mit dem Wert 0 oder eine vom Benutzer in der Triggerdefinition angegebene Richtlinie. Eine Wiederholung erfolgt automatisch, wenn Pipelineausführungen aufgrund von Parallelitäts-/Server-/Einschränkungsgrenzwerten (d.h. mit den Statuscodes „400: Benutzerfehler“, „429: Zu viele Anforderungen“ und „500: Interner Serverfehler“) fehlschlagen. | Nicht unterstützt. |
| **Concurrency** | Unterstützt. Benutzer können Parallelitätsgrenzwerte für den Trigger explizit festlegen. Eine bis maximal 50 gleichzeitige ausgelöste Pipelineausführungen sind zulässig. | Nicht unterstützt. |
| **System&nbsp;variablen** | Unterstützt die Verwendung der Systemvariablen **WindowStart** und **WindowEnd**. Benutzer haben Zugriff auf `triggerOutputs().windowStartTime` und `triggerOutputs().windowEndTime` als Systemvariablen in der Triggerdefinition. Die Werte werden jeweils als Start- und Endzeit des Fensters verwendet. Beispiel: Die Definition eines stündlich ausgeführten Triggers für ein rollierendes Fenster lautet für das Fenster von 1:00 Uhr bis 2:00 Uhr `triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z` und `triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z`. | Nicht unterstützt. |
| **Beziehung zwischen Pipeline und Trigger** | Unterstützt eine 1:1-Beziehung. Nur eine Pipeline kann ausgelöst werden. | Unterstützt m:m-Beziehungen. Mehrere Trigger können eine einzelne Pipeline starten. Ein einzelnder Trigger kann mehrere Pipelines starten. | 

## <a name="next-steps"></a>Nächste Schritte
Arbeiten Sie die folgenden Tutorials durch:

- [Schnellstart: Erstellen einer Data Factory und Pipeline mit dem .NET SDK](quickstart-create-data-factory-dot-net.md)
- [Erstellen eines Zeitplantriggers](how-to-create-schedule-trigger.md)
- [Erstellen eines Triggers für ein rollierendes Fenster](how-to-create-tumbling-window-trigger.md)

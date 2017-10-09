---
title: "Fehlertoleranz der Kopieraktivität in Azure Data Factory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie der Kopieraktivität von Azure Data Factory Fehlertoleranz hinzufügen, indem inkompatible Zeilen übersprungen werden."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: d96c89ed3650c09ac6465e30754ef1155b06d601
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Fehlertoleranz der Kopieraktivität in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Version 2 – Vorschau](copy-activity-fault-tolerance.md)

Die Kopieraktivität in Azure Data Factory bietet beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern zwei Optionen für den Umgang mit nicht kompatiblen Zeilen:

- Die Kopieraktivität kann mit einem Fehler abgebrochen werden, wenn sie auf inkompatible Daten stößt (Standardverhalten).
- Der Kopiervorgang kann für alle Daten fortgesetzt werden, indem Fehlertoleranz hinzugefügt wird und die inkompatiblen Datenzeilen übersprungen werden. Darüber hinaus können die inkompatiblen Zeilen in Azure Blob Storage protokolliert werden. Sie können dann das Protokoll überprüfen, um die Ursache des Fehlers zu ermitteln, die Daten in der Datenquelle korrigieren und die Kopieraktivität wiederholen.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (GA) ist, lesen Sie [Fehlertoleranz der Kopieraktivität in V1](v1/data-factory-copy-activity-fault-tolerance.md).


 ## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Kopieraktivität unterstützt drei Szenarien zum Erkennen, Überspringen und Protokollieren inkompatibler Daten:

- **Inkompatibilität zwischen dem Quelldatentyp und dem nativen Senkentyp**. <br/><br/> Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die drei Spalten vom Typ INT enthält. Die Zeilen der CSV-Datei, die numerische Daten wie z.B. 123,456,789 enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen mit nicht numerischen Werten, z.B. 123,456, abc, werden dagegen als nicht kompatibel erkannt und übersprungen.
- **Fehlende Übereinstimmung bei der Anzahl der Spalten zwischen der Quelle und der Senke**. <br/><br/> Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die sechs Spalten enthält. Die Zeilen der CSV-Datei, die sechs Spalten enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen der CSV-Datei mit weniger oder mehr als sechs Spalten werden als inkompatibel erkannt und übersprungen.
- **Primärschlüsselverletzung beim Schreiben in eine relationale Datenbank**.<br/><br/> Beispiel: Kopieren von Daten von einer SQL Server-Instanz in eine SQL-Datenbank. In der SQL-Datenbank der Senke ist ein Primärschlüssel definiert, in der SQL Server-Instanz der Quelle ist dagegen kein Primärschlüssel definiert. Die doppelten Zeilen, die in der Quelle vorhanden sind, können nicht in die Senke kopiert werden. Die Kopieraktivität kopiert nur die erste Zeile der Quelldaten in die Senke. Die nachfolgenden Quellzeilen, die den doppelten Primärschlüsselwert enthalten, werden als inkompatibel erkannt und übersprungen.

## <a name="configuration"></a>Konfiguration
Das folgende Beispiel umfasst eine JSON-Definition zum Konfigurieren des Überspringens inkompatibler Datenzeilen in der Kopieraktivität:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "AzureBlobLinkedService",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```
Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Gibt an, ob nicht kompatible Zeilen beim Kopieren übersprungen werden sollen. | true<br/>False (Standardwert) | Nein
redirectIncompatibleRowSettings | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn Sie die inkompatiblen Zeilen protokollieren möchten. | &nbsp; | Nein
linkedServiceName | Der verknüpfte Azure Storage-Dienst zum Speichern des Protokolls mit den übersprungenen Zeilen. | Der Name eines verknüpften AzureStorage- oder AzureStorageSas-Diensts, der auf die Storage-Instanz verweist, in der die Protokolldatei gespeichert werden soll. | Nein
path | Der Pfad der Protokolldatei, die die übersprungenen Zeilen enthält. | Geben Sie den gewünschten Blob Storage-Pfad für die Protokollierung der inkompatiblen Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein

## <a name="monitor-skipped-rows"></a>Überwachen der übersprungenen Zeilen
Nach Abschluss der Ausführung der Kopieraktivität wird die Anzahl übersprungener Zeilen in der Ausgabe der Kopieraktivität angezeigt:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Wenn Sie das Protokollieren der nicht kompatiblen Zeilen konfigurieren, finden Sie die Protokolldatei in diesem Pfad: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

In der Protokolldatei sind die Zeilen, die übersprungen wurden, und die Ursache für die Inkompatibilität angegeben.

In der Datei werden sowohl die ursprünglichen Daten als auch der entsprechende Fehler protokolliert. Der Inhalt der Protokolldatei kann beispielsweise wie folgt aussehen:

```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Leistung der Kopieraktivität](copy-activity-performance.md)




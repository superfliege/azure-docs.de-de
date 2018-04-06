---
title: Hinzufügen von Fehlertoleranz der Kopieraktivität von Azure Data Factory durch Überspringen inkompatibler Zeilen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Fehlertoleranz der Kopieraktivität von Azure Data Factory durch Überspringen inkompatibler Zeilen hinzufügen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8b91b76d049d1a7f299b7890665dd9281e08321e
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Hinzufügen von Fehlertoleranz der Kopieraktivität durch Überspringen inkompatibler Zeilen
> [!NOTE]
> Dieser Artikel bezieht sich auf Version 1 von Data Factory, die allgemein verfügbar (GA) ist. Wenn Sie Version 2 des Data Factory-Diensts verwenden, die sich derzeit in der Vorschauphase befindet, finden Sie weitere Informationen unter [fault tolerance in copy activity of Data Factory version 2 (Fehlertoleranz bei Kopieraktivitäten in Version 2 von Data Factory)](../copy-activity-fault-tolerance.md).

Die [Kopieraktivität](data-factory-data-movement-activities.md) von Azure Data Factory bietet beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern zwei Optionen für den Umgang mit inkompatiblen Zeilen:

- Die Kopieraktivität kann mit einem Fehler abgebrochen werden, wenn sie auf inkompatible Daten stößt (Standardverhalten).
- Der Kopiervorgang kann für alle Daten fortgesetzt werden, indem Fehlertoleranz hinzugefügt wird und die inkompatiblen Datenzeilen übersprungen werden. Darüber hinaus können die inkompatiblen Zeilen in Azure Blob Storage protokolliert werden. Sie können dann das Protokoll überprüfen, um die Ursache des Fehlers zu ermitteln, die Daten in der Datenquelle korrigieren und die Kopieraktivität wiederholen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Kopieraktivität unterstützt drei Szenarien zum Erkennen, Überspringen und Protokollieren inkompatibler Daten:

- **Inkompatibilität zwischen dem Quelldatentyp und dem nativen Senkentyp**

    Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die drei Spalten vom Typ **INT** enthält. Die Zeilen der CSV-Datei, die numerische Daten wie z.B. `123,456,789` enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen mit nicht numerischen Werten, z.B. `123,456,abc`, werden dagegen als inkompatibel erkannt und übersprungen.

- **Fehlende Übereinstimmung bei der Anzahl der Spalten zwischen der Quelle und der Senke**

    Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die sechs Spalten enthält. Die Zeilen der CSV-Datei, die sechs Spalten enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen der CSV-Datei mit weniger oder mehr als sechs Spalten werden als inkompatibel erkannt und übersprungen.

- **Primärschlüsselverletzung beim Schreiben in SQL Server/Azure SQL Database/Azure Cosmos DB**

    Beispiel: Kopieren von Daten von einer SQL Server-Instanz in eine SQL-Datenbank. In der SQL-Datenbank der Senke ist ein Primärschlüssel definiert, in der SQL Server-Instanz der Quelle ist dagegen kein Primärschlüssel definiert. Die doppelten Zeilen, die in der Quelle vorhanden sind, können nicht in die Senke kopiert werden. Die Kopieraktivität kopiert nur die erste Zeile der Quelldaten in die Senke. Die nachfolgenden Quellzeilen, die den doppelten Primärschlüsselwert enthalten, werden als inkompatibel erkannt und übersprungen.

>[!NOTE]
>Diese Funktion gilt nicht, wenn die Kopieraktivität so konfiguriert ist, dass sie externe Datenlademechanismen wie [Azure SQL Data Warehouse PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) oder [Amazon Redshift Unload](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift) aufruft. Um Daten mit PolyBase in SQL Data Warehouse zu laden, verwenden Sie die native Fehlertoleranzunterstützung von PolyBase, indem Sie in der Kopieraktivität [polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink) angeben.

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
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Gibt an, ob inkompatible Zeilen beim Kopieren übersprungen werden sollen. | True<br/>False (Standardwert) | Nein  |
| **redirectIncompatibleRowSettings** | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn Sie die inkompatiblen Zeilen protokollieren möchten. | &nbsp; | Nein  |
| **linkedServiceName** | Der verknüpfte Azure Storage-Dienst zum Speichern des Protokolls mit den übersprungenen Zeilen. | Der Name eines verknüpften Diensts vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service), der auf die Storage-Instanz verweist, in der die Protokolldatei gespeichert werden soll. | Nein  |
| **path** | Der Pfad der Protokolldatei, die die übersprungenen Zeilen enthält. | Geben Sie den gewünschten Blob Storage-Pfad für die Protokollierung der inkompatiblen Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein  |

## <a name="monitoring"></a>Überwachung
Nach Abschluss der Kopieraktivität wird die Anzahl übersprungener Zeilen im Überwachungsabschnitt angezeigt:

![Überwachen übersprungener inkompatibler Zeilen](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Wenn Sie das Protokollieren inkompatibler Zeilen konfiguriert haben, finden Sie die Protokolldatei unter folgendem Pfad: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In der Protokolldatei können Sie die übersprungenen Zeilen und die Ursache der Inkompatibilität einsehen.

In der Datei werden sowohl die ursprünglichen Daten als auch der entsprechende Fehler protokolliert. Der Inhalt der Protokolldatei kann beispielsweise wie folgt aussehen:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Kopieraktivität von Azure Data Factory finden Sie unter [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md).

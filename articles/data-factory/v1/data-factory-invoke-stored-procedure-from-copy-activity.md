---
title: "Aufrufen von gespeicherten Prozeduren aus der Azure Data Factory-Kopieraktivität | Microsoft-Dokumente"
description: "Erfahren Sie, wie eine gespeicherte Prozedur in Azure SQL-Datenbank oder SQL-Server aus einer Kopieraktivität von Azure Data Factory aufgerufen wird."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 460785d0d3f8b3d8a0a53d544788cb1a74db8c00
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Aufrufen von gespeicherten Prozeduren aus der Kopieraktivität in Azure Data Factory
> [!NOTE]
> Dieser Artikel bezieht sich auf Version 1 von Data Factory, die allgemein verfügbar (GA) ist. Wenn Sie Version 2 des Data Factory-Diensts verwenden, die sich derzeit in der Vorschauphase befindet, finden Sie weitere Informationen unter [transform data using Hive activity in Data Factory version 2 (Transformieren von Daten mithilfe der Aktivität „Gespeicherte Prozedur“ in Version 2 von Data Factory)](../transform-data-using-stored-procedure.md).


Beim Kopieren von Daten nach [SQL Server](data-factory-sqlserver-connector.md) oder [Azure SQL-Datenbank](data-factory-azure-sql-connector.md) können Sie die **SqlSink** in der Kopieraktivität für das Aufrufen einer gespeicherten Prozedur konfigurieren. Es kann sinnvoll sein, eventuelle zusätzliche Verarbeitungsschritte (Zusammenführen von Spalten, Nachschlagen von Werten, Einfügungen in mehrere Tabellen zugleich usw.), die vor dem Einfügen von Daten in die Zieltabelle erforderlich sind, von der gespeicherten Prozedur ausführen zu lassen. Diese Funktion nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx). 

Das folgende Beispiel zeigt den Aufruf einer gespeicherten Prozedur in einer SQL Server-Datenbank aus einer Data Factory-Pipeline (Kopieraktivität):  

## <a name="output-dataset-json"></a>JSON des Ausgabedatasets
Legen Sie im Ausgabedataset-JSON den **type** auf **SqlServerTable** fest. Legen Sie ihn für die Verwendung mit einer Azure SQL-Datenbank auf **AzureSqlTable** fest. Der Wert der Eigenschaft **tableName** muss mit dem Namen des ersten Parameters der gespeicherten Prozedur übereinstimmen.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>JSON im Abschnitt „SqlSink“ in der Kopieraktivität
Definieren Sie den Abschnitt **SqlSink** im JSON der Kopieraktivität wie folgt. Um beim Einfügen von Daten in die Senken-/Zieldatenbank eine gespeicherte Prozedur aufzurufen, geben Sie Werte sowohl für die **SqlWriterStoredProcedureName**-Eigenschaft als auch für die **SqlWriterTableType**-Eigenschaft an. Beschreibungen dieser Eigenschaften finden Sie im [SqlSink-Abschnitt im SQL Server-Connector-Artikel](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Definition der gespeicherten Prozedur 
Definieren Sie in Ihrer Datenbank die gespeicherte Prozedur mit dem gleichen Namen wie **SqlWriterStoredProcedureName**. Die gespeicherte Prozedur verarbeitet Eingabedaten aus dem Quelldatenspeicher und fügt Daten in eine Tabelle in der Zieldatenbank ein. Der Name des ersten Parameters der gespeicherten Prozedur muss mit dem im Dataset-JSON definierten „tableName“ übereinstimmen (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tabellentypdefinition
Definieren Sie in Ihrer Datenbank den Tabellentyp mit dem gleichen Namen wie **SqlWriterTableType**. Das Schema des Tabellentyps muss mit dem Schema des Eingabedatasets übereinstimmen.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Nächste Schritte
Gehen Sie die folgenden Connector-Artikel durch, um vollständige JSON-Beispiele zu finden: 

- [Azure SQL-Datenbank](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)

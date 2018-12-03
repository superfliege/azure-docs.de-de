---
title: Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Daten mit Azure Data Factory auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz verschieben.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: jingwang
ms.openlocfilehash: 561e672436c38cd0b3e637b794662483fc630676
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706720"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-using-azure-data-factory"></a>Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten von einer verwalteten Azure SQL-Datenbank-Instanz in einen beliebigen unterstützten Senkendatenspeicher oder Daten aus einem beliebigen unterstützten Quelldatenspeicher auf die verwaltete Instanz kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Diese verwaltete Azure SQL-Datenbank-Instanz unterstützt insbesondere Folgendes:

- Kopieren von Dateien unter Verwendung der Authentifizierung des Typs **SQL** oder **Windows**
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur.
- Als Senke das Anfügen von Daten an die Zieltabelle oder Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung des Kopierens von Daten von einer verwalteten Azure SQL-Datenbank-Instanz in einem VNET müssen Sie in demselben VNET eine selbstgehostete Integration Runtime einrichten, die Zugriff auf die Datenbank hat. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Connector für die verwaltete Azure SQL-Datenbank-Instanz verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit der verwalteten Azure SQL-Datenbank-Instanz verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **SqlServer** festgelegt sein. | JA |
| connectionString |Geben Sie „connectionString“-Informationen an, die zum Herstellen einer Verbindung mit der verwalteten Instanz mithilfe der SQL- oder Windows-Authentifizierung benötigt werden. Sehen Sie sich das folgende Beispiel an. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |JA |
| userName |Geben Sie den Benutzernamen an, wenn Sie die Windows-Authentifizierung verwenden. Beispiel: **Domainname\\Benutzername**. |Nein  |
| password |Geben Sie das Kennwort für das Benutzerkonto an, das Sie für „userName“ angegeben haben. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |Nein  |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Stellen Sie die selbstgehostete Integration Runtime in demselben VNET wie Ihre verwaltete Instanz bereit. |JA |

>[!TIP]
>Wenn ein Fehler mit dem Fehlercode „UserErrorFailedToConnectToSqlServer“ auftritt und eine Meldung wie „Das Sitzungslimit für die Datenbank ist XXX und wurde erreicht“ angezeigt wird, fügen Sie `Pooling=false` zu Ihrer Verbindungszeichenfolge hinzu, und versuchen Sie es erneut.

**Beispiel 1: Verwenden der SQL-Authentifizierung**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 2: Verwenden der Windows-Authentifizierung**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
             "userName": "<domain\\username>",
             "password": {
                "type": "SecureString",
                "value": "<password>"
             }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
     }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die vom Dataset der verwalteten Azure SQL-Datenbank-Instanz unterstützt werden.

Legen Sie die Typeigenschaft des Datasets auf **SqlServerTable** fest, um Daten auf die bzw. von der verwalteten Azure SQL-Datenbank-Instanz zu kopieren. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft des Datasets muss auf **SqlServerTable** festgelegt werden. | JA |
| tableName |Name der Tabelle oder Sicht in der Datenbankinstanz, auf die der verknüpfte Dienst verweist. | Quelle: Nein, Senke: Ja |

**Beispiel**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die von der Quelle und Senke der verwalteten Azure SQL-Datenbank-Instanz unterstützt werden.

### <a name="azure-sql-database-managed-instance-as-source"></a>Verwaltete Azure SQL-Datenbank-Instanz als Quelle

Legen Sie zum Kopieren von Daten von der verwalteten Azure SQL-Datenbank-Instanz den Quelltyp in der Kopieraktivität auf **SqlSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SqlSource** festgelegt werden. | JA |
| SqlReaderQuery |Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `select * from MyTable`. |Nein  |
| sqlReaderStoredProcedureName |Der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. |Nein  |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte: Name/Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein  |

**Wichtige Hinweise**

- Wenn **sqlReaderQuery** für „SqlSource“ angegeben ist, wendet die Kopieraktivität diese Abfrage auf die Quelle „Verwaltete Instanz“ an, um die Daten abzurufen. Alternativ dazu können Sie eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben (sofern die gespeicherten Prozeduren Parameter verwenden).
- Ohne Angabe der Eigenschaft „sqlReaderQuery“ oder „sqlReaderStoredProcedureName“ werden die im Abschnitt „structure“ des Dataset-JSON-Bereichs definierten Spalten verwendet, um eine Abfrage (`select column1, column2 from mytable`) für die verwaltete Instanz zu erstellen. Falls die Datasetdefinition nicht den Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel: Verwenden einer SQL-Abfrage**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Beispiel: Verwenden einer gespeicherten Prozedur**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Definition der gespeicherten Prozedur**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-sink"></a>Verwaltete Azure SQL-Datenbank-Instanz als Senke

Legen Sie zum Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz den Senkentyp in der Kopieraktivität auf **SqlSink** fest. Folgende Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SqlSink** festgelegt werden. | JA |
| writeBatchSize |Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht.<br/>Zulässige Werte: Ganze Zahlen (Anzahl der Zeilen). |Nein (Standard = 10000) |
| writeBatchTimeout |Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br/>Zulässige Werte: Zeitraum  Beispiel: 00:30:00 (30 Minuten) |Nein  |
| preCopyScript |Geben Sie eine auszuführende SQL-Abfrage für die Copy-Aktivität an, ehe Daten auf die verwaltete Instanz geschrieben werden. Sie wird pro Ausführung der Copy-Aktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. |Nein  |
| sqlWriterStoredProcedureName |Der Name der gespeicherten Prozedur, die definiert, wie Quelldaten in der Zieltabelle angewendet werden (etwa durch Ausführen von Upserts oder Transformationen mit eigener Geschäftslogik). <br/><br/>Beachten Sie, dass diese gespeicherte Prozedur **pro Batch aufgerufen wird**. Verwenden Sie bei einem Vorgang, der nur einmal ausgeführt wird und nicht mit Quelldaten in Zusammenhang steht (etwa Löschen/Kürzen), die `preCopyScript`-Eigenschaft. |Nein  |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte: Name/Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein  |
| sqlWriterTableType |Geben Sie einen Tabellentypnamen an, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. |Nein  |

> [!TIP]
> Beim Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz fügt die Kopieraktivität standardmäßig Daten an die Senkentabelle an. Um einen UPSERT-Vorgang oder zusätzliche Geschäftslogik auszuführen, verwenden Sie die in „SqlSink“ gespeicherte Prozedur. Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur für die SQL-Senke](#invoking-stored-procedure-for-sql-sink).

**Beispiel 1: Anfügen von Daten**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Beispiel 2: Aufrufen einer gespeicherten Prozedur während des Kopierens für den upsert-Vorgang**

Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur für die SQL-Senke](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Identitätsspalten in der Zieldatenbank

Dieser Abschnitt enthält ein Beispiel zum Kopieren von Daten aus einer Quelltabelle ohne eine Identitätsspalte in eine Zieltabelle mit einer Identitätsspalte.

**Quelltabelle**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Zieltabelle**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Beachten Sie, dass die Zieltabelle über eine Identitätsspalte verfügt.

**Definition der Quell-Dataset-JSON**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Definition der Ziel-Dataset-JSON**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Beachten Sie, dass die Quell- und die Zieltabelle unterschiedliche Schemas besitzen (das Ziel verfügt über eine zusätzliche Spalte mit der Identität). In diesem Szenario müssen Sie eine **structure** -Eigenschaft in der Definition des Zieldatasets angeben, die nicht die Identitätsspalte enthält.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz konnte eine vom Benutzer angegebene gespeicherte Prozedur konfiguriert und mit zusätzlichen Parametern aufgerufen werden.

Eine gespeicherte Prozedur kann genutzt werden, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Sie wird in der Regel genutzt, wenn ein upsert-Vorgang (Insert + Update, Einfügen + Aktualisieren) oder eine zusätzliche Verarbeitung (Zusammenführen von Spalten, Suchen nach zusätzlichen Werten, Einfügen in mehrere Tabellen usw.) vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle durchgeführt werden muss.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen upsert-Vorgang in eine Tabelle der verwalteten Instanz auszuführen. Vorausgesetzt, Eingabedaten sind vorhanden, und die Senkentabelle heißt „Marketing“, sind drei Spalten vorhanden: „ProfileID“, „State“ und „Category“. Führen Sie einen upsert-Vorgang basierend auf der Spalte „ProfileID“ aus, der nur für eine bestimmte Kategorie gelten soll.

**Ausgabedataset**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definieren Sie den Abschnitt „SqlSink“ in der Kopieraktivität wie folgt.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Definieren Sie die gespeicherte Prozedur in der Datenbank mit demselben Namen wie "SqlWriterStoredProcedureName". Sie verarbeitet die Eingabedaten aus der angegebenen Quelle und führt sie mit der Ausgabetabelle zusammen. Der Parametername des Tabellentyps in der gespeicherten Prozedur muss mit dem Element „tableName“ identisch sein, das im Dataset definiert ist.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Definieren Sie in der Datenbank den Tabellentyp mit demselben Namen wie „sqlWriterTableType“. Beachten Sie, dass das Schema des Tabellentyps mit dem Schema übereinstimmen sollte, das von den Eingabedaten zurückgegeben wird.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Wenn Sie durch Aufrufen einer gespeicherten Prozedur in den Datentyp „Money/Smallmoney“ schreiben, werden die Werte unter Umständen gerundet. Geben Sie den entsprechenden Datentyp im Tabellenwertparameter nicht als „Money/Smallmoney“, sondern als „Decimal“ an, um dies zu vermeiden. 

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Datentypzuordnung für verwaltete Azure SQL-Datenbank-Instanz

Beim Kopieren von Daten auf die bzw. von der verwalteten Azure SQL-Datenbank-Instanz werden die folgenden Zuordnungen von Datentypen der verwalteten Instanz zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Datentyp der verwalteten Azure SQL-Datenbank-Instanz | Data Factory-Zwischendatentyp |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| Bit |Boolescher Wert |
| char |String, Char[] |
| date |Datetime |
| DateTime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| DECIMAL |DECIMAL |
| FILESTREAM-Attribut (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |DECIMAL |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |DECIMAL |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |DECIMAL |
| sql_variant |Object * |
| text |String, Char[] |
| time |Zeitraum |
|  timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

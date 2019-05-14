---
title: Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten mit Azure Data Factory auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz verschieben.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: 3f29db5786c682188b4eadec12275df46ae3b547
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153335"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer verwalteten Azure SQL-Datenbank-Instanz in beliebige unterstützte Senkendatenspeicher und umgekehrt kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in eine verwaltete Instanz kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Diese verwaltete Azure SQL-Datenbank-Instanz unterstützt insbesondere Folgendes:

- Kopieren von Daten unter Verwendung der SQL- oder Windows-Authentifizierung
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur
- Als Senke das Anfügen von Daten an die Zieltabelle oder Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs

SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wird derzeit nicht unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

Richten Sie zur Verwendung des Kopierens von Daten von einer verwalteten Azure SQL-Datenbank-Instanz in ein virtuelles Netzwerk eine selbstgehostete Integration Runtime ein, die Zugriff auf die Datenbank hat. Weitere Informationen finden Sie unter [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md).

Wenn Sie Ihre selbstgehostete Integration Runtime im gleichen virtuellen Netzwerk wie Ihre verwaltete Instanz bereitstellen, stellen Sie sicher, dass sich Ihr Computer mit der Integration Runtime in einem anderen Subnetz als Ihre verwaltete Instanz befindet. Wenn Sie Ihre selbstgehostete Integration Runtime mit einem anderen virtuellen Netzwerk bereitstellen als Ihre verwaltete Instanz, empfiehlt sich ein Peering virtueller Netzwerke oder eine Verbindung zwischen den virtuellen Netzwerken. Weitere Informationen finden Sie in [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Connector für die verwaltete Azure SQL-Datenbank-Instanz verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit der verwalteten Azure SQL-Datenbank-Instanz verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **SqlServer** festgelegt sein. | Ja. |
| connectionString |Diese Eigenschaft gibt die „connectionString“-Informationen an, die zum Herstellen einer Verbindung mit der verwalteten Instanz mithilfe der SQL- oder Windows-Authentifizierung benötigt werden. Weitere Informationen finden Sie in den folgenden Beispielen. <br/>Markieren Sie dieses Feld als „SecureString“, um es sicher in Data Factory zu speichern. Sie können auch das Kennwort in Azure Key Vault speichern und bei Verwendung der SQL-Authentifizierung die `password`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie im JSON-Beispiel unter der Tabelle und im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja. |
| userName |Diese Eigenschaft gibt einen Benutzernamen an, wenn Sie Windows-Authentifizierung verwenden. Ein Beispiel lautet **domainname\\username**. |Nein. |
| password |Diese Eigenschaft gibt ein Kennwort für das Benutzerkonto an, das Sie für „username“ angegeben haben. Wählen Sie [SecureString](store-credentials-in-key-vault.md) aus, um die connectionString-Informationen sicher in Data Factory zu speichern, oder **verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis**. |Nein. |
| connectVia | Diese [Integration Runtime](concepts-integration-runtime.md) wird zum Herstellen einer Verbindung mit dem Datenspeicher verwendet. Stellen Sie die selbstgehostete Integration Runtime in demselben virtuellen Netzwerk wie Ihre verwaltete Instanz bereit. |Ja. |

>[!TIP]
>Möglicherweise wird der Fehlercode "UserErrorFailedToConnectToSqlServer" mit einer Meldung wie „Das Sitzungslimit für die Datenbank beträgt XXX und wurde erreicht“ angezeigt. Wenn dieser Fehler auftritt, fügen Sie `Pooling=false` zu Ihrer Verbindungszeichenfolge hinzu, und versuchen Sie es erneut.

**Beispiel 1: SQL-Authentifizierung verwenden**

```json
{
    "name": "AzureSqlMILinkedService",
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

**Beispiel 2: Verwenden der SQL-Authentifizierung mit Kennwort in Azure Key Vault**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 3: Windows-Authentifizierung verwenden**

```json
{
    "name": "AzureSqlMILinkedService",
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

Zum Kopieren von Daten in die bzw. aus der verwaltete(n) Azure SQL-Datenbank-Instanz werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **SqlServerTable** festgelegt werden. | Ja. |
| tableName |Diese Eigenschaft ist der Name der Tabelle oder Ansicht in der Datenbankinstanz, auf die der verknüpfte Dienst verweist. | Nein für „Quelle“. Ja für „Senke“. |

**Beispiel**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die von der Quelle und Senke der verwalteten Azure SQL-Datenbank-Instanz unterstützt werden.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Verwaltete Azure SQL-Datenbank-Instanz als Quelle

Legen Sie zum Kopieren von Daten von der verwalteten Azure SQL-Datenbank-Instanz den Quelltyp in der Kopieraktivität auf **SqlSource** fest. Die folgenden Eigenschaften werden im Abschnitt „source“ der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SqlSource** festgelegt sein. | Ja. |
| sqlReaderQuery |Diese Eigenschaft verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `select * from MyTable`. |Nein. |
| sqlReaderStoredProcedureName |Diese Eigenschaft ist der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. |Nein. |
| storedProcedureParameters |Diese Parameter werden für die gespeicherte Prozedur verwendet.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung der Parameter müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein. |

Beachten Sie folgende Punkte:

- Wenn **sqlReaderQuery** für **SqlSource** angegeben ist, wendet die Kopieraktivität diese Abfrage auf die Quelle „Verwaltete Instanz“ an, um die Daten abzurufen. Sie können auch eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben, sofern die gespeicherten Prozeduren Parameter verwenden.
- Ohne Angabe von **sqlReaderQuery** oder der Eigenschaft **sqlReaderStoredProcedureName** werden die im Abschnitt „structure“ des JSON-Codes des Datasets definierten Spalten zum Erstellen einer Abfrage verwendet. Die Abfrage `select column1, column2 from mytable` wird für die verwaltete Instanz ausgeführt. Falls die Datasetdefinition keinen Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel: Verwenden einer SQL-Abfrage**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
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
        "name": "CopyFromAzureSqlMI",
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

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Verwaltete Azure SQL-Datenbank-Instanz als Senke

Legen Sie zum Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz den Senkentyp in der Kopieraktivität auf **SqlSink** fest. Die folgenden Eigenschaften werden im Abschnitt „sink“ der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SqlSink** festgelegt sein. | Ja. |
| writeBatchSize |Anzahl der Zeilen, die in die SQL-Tabelle **pro Batch** eingefügt werden sollen.<br/>Zulässige Werte sind Integer-Werte für die Anzahl der Zeilen. Standardmäßig bestimmt Data Factory die geeignete Batchgröße dynamisch anhand der Zeilengröße.  |Nein  |
| writeBatchTimeout |Diese Eigenschaft gibt die Wartezeit für den Abschluss der Batcheinfügung an, bevor ein Timeout auftritt.<br/>Zulässige Werte werden für die Zeitspanne verwendet. Ein Beispiel ist „00:30:00“. Das sind 30 Minuten. |Nein. |
| preCopyScript |Diese Eigenschaft gibt eine auszuführende SQL-Abfrage für die Kopieraktivität an, ehe Daten auf die verwaltete Instanz geschrieben werden. Sie wird pro Ausführung der Kopieraktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. | Nein. |
| sqlWriterStoredProcedureName |Dies ist der Name der gespeicherten Prozedur, die definiert, wie Quelldaten auf eine Zieltabelle angewandt werden. Ein Beispiel für diese Prozedur ist die Ausführung eines Upsert-Vorgangs bzw. einer Transformation mithilfe einer eigenen Geschäftslogik. <br/><br/>Diese gespeicherte Prozedur wird *pro Batch aufgerufen*. Auf einen Vorgang auszuführen, der nur einmal ausgeführt wird und nicht mit Quelldaten in Zusammenhang steht (etwa Löschen/Kürzen), verwenden Sie die `preCopyScript`-Eigenschaft. | Nein. |
| storedProcedureParameters |Diese Parameter werden für die gespeicherte Prozedur verwendet.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung der Parameter müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein. |
| sqlWriterTableType |Diese Eigenschaft gibt einen Tabellentypnamen an, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. | Nein. |

> [!TIP]
> Beim Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz fügt die Kopieraktivität standardmäßig Daten an die Senkentabelle an. Um einen UPSERT-Vorgang oder zusätzliche Geschäftslogik auszuführen, verwenden Sie die in „SqlSink“ gespeicherte Prozedur. Weitere Informationen finden Sie unter [Aufrufen von gespeicherten Prozeduren aus einer SQL-Senke](#invoke-a-stored-procedure-from-a-sql-sink).

**Beispiel 1: Anfügen von Daten**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
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

**Beispiel 2: Aufrufen einer gespeicherten Prozedur während des Kopierens für einen Upsert-Vorgang**

Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur aus einer SQL-Senke](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
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

Das folgende Beispiel kopiert Daten aus einer Quelltabelle ohne eine Identitätsspalte in eine Zieltabelle mit einer Identitätsspalte.

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

Beachten Sie, dass Ihre Quell- und Zieltabelle unterschiedliche Schemas aufweisen. Die Zieltabelle enthält eine Identitätsspalte. In diesem Szenario müssen Sie eine „structure“-Eigenschaft in der Definition des Zieldatasets angeben, die nicht die Identitätsspalte enthält.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz kann eine gespeicherte Prozedur konfiguriert und mit zusätzlichen Parametern aufgerufen werden, die Sie angeben.

Sie können eine gespeicherte Prozedur nutzen, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Sie werden in der Regel verwendet, wenn ein Upsert-Vorgang (Update + Einfügen) oder eine zusätzliche Verarbeitung erfolgen muss, bevor die letzte Einfügung von Quelldaten in die Zieltabelle durchgeführt wird. Zusätzliche Verarbeitungen können Aufgaben wie das Zusammenführen von Spalten, die Suche nach zusätzlichen Werten und das Einfügen in mehrere Tabellen beinhalten.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen Upsert-Vorgang in eine Tabelle in der SQL Server-Datenbank auszuführen. Wenn Eingabedaten vorhanden sind und die Senkentabelle **Marketing** heißt, sind drei Spalten vorhanden: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus, und wenden Sie ihn nur auf eine bestimmte Kategorie an.

**Ausgabedataset:** Der „tableName“ sollte in Ihrer gespeicherten Prozedur denselben Tabellentypparameternamen haben. Im Folgenden finden Sie das Skript für die gespeicherte Prozedur:

```json
{
    "name": "AzureSqlMIDataset",
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

Definieren Sie den Abschnitt **SqlSink** in der Kopieraktivität wie folgt.

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

Definieren Sie die gespeicherte Prozedur in der Datenbank mit demselben Namen wie **SqlWriterStoredProcedureName**. Sie verarbeitet die Eingabedaten aus der angegebenen Quelle und führt sie mit der Ausgabetabelle zusammen. Der Parametername des Tabellentyps in der gespeicherten Prozedur muss mit dem Element **tableName** identisch sein, das im Dataset definiert ist.

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
      VALUES (source.ProfileID, source.State, source.Category);
END
```

Definieren Sie in der Datenbank den Tabellentyp mit demselben Namen wie „sqlWriterTableType“. Das Schema des Tabellentyps muss mit dem Schema übereinstimmen, das von den Eingabedaten zurückgegeben wird.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Datentypzuordnung für verwaltete Azure SQL-Datenbank-Instanz

Beim Kopieren von Daten auf die bzw. von der verwalteten Azure SQL-Datenbank-Instanz werden die folgenden Zuordnungen von Datentypen der verwalteten Azure SQL-Datenbank-Instanz zu Azure Data Factory-Zwischendatentypen verwendet. Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Datentyp der verwalteten Azure SQL-Datenbank-Instanz | Azure Data Factory-Zwischendatentyp |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |Datetime |
| Datetime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |Xml |

>[!NOTE]
> Für Datentypen, die dem Zwischentyp „Decimal“ zugeordnet sind, unterstützt Azure Data Factory derzeit eine Genauigkeit von bis zu 28. Wenn Ihre Daten eine höhere Genauigkeit als 28 erfordern, erwägen Sie, sie per SQL-Abfrage in eine Zeichenfolge zu konvertieren.

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).

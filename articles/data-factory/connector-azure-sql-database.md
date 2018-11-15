---
title: Kopieren von Daten in und aus Azure SQL-Datenbank mit Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie mithilfe der Data Factory Daten von unterstützten Quelldatenspeichern nach Azure SQL-Datenbank oder aus SQL-Datenbank in unterstützte Senkendatenspeicher kopiert werden.
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
ms.date: 11/08/2018
ms.author: jingwang
ms.openlocfilehash: cd137462235431f0a0c1562e15a32951fe2a41c5
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51346704"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Kopieren von Daten nach und aus Azure SQL-Datenbank mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Version 1](v1/data-factory-azure-sql-connector.md)
> * [Aktuelle Version](connector-azure-sql-database.md)

In diesem Artikel wird die Verwendung der Kopieraktivität in Azure Data Factory zum Kopieren von Daten in und aus Azure SQL-Datenbank beschrieben. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Azure SQL-Datenbank in beliebige unterstützte Senkendatenspeicher und umgekehrt kopieren. Sie können Daten aus jedem beliebigen unterstützten Quelldatenspeicher in Azure SQL-Datenbank kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

Der Azure SQL-Datenbank-Connector unterstützt insbesondere folgende Funktionen:

- Kopieren von Daten mit SQL-Authentifizierung und Azure Active Directory-Anwendungstokenauthentifizierung (Azure AD) mit einem Dienstprinzipal oder verwalteten Identitäten für Azure-Ressourcen.
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur
- Als Senke das Anfügen von Daten an die Zieltabelle oder Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs

> [!IMPORTANT]
> Wenn Sie Daten mithilfe der Azure Data Factory Integration Runtime kopieren, konfigurieren Sie die [Azure SQL Server-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure), damit Azure-Dienste Zugriff auf den Server erhalten.
> Wenn Sie Daten mithilfe einer selbstgehosteten Integration Runtime kopieren, konfigurieren Sie die Azure SQL-Server-Firewall, um den entsprechenden IP-Adressbereich zuzulassen. Dieser Bereich schließt die IP-Adresse des Computers ein, der für die Verbindung mit Azure SQL-Datenbank verwendet wird.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für einen Azure SQL-Datenbank-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Diese Eigenschaften werden für den mit Azure SQL-Datenbank verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft muss auf **AzureSqlDatabase** festgelegt sein. | JA |
| connectionString | Geben Sie Informationen, die zur Verbindung mit der Azure SQL-Datenbank-Instanz erforderlich sind, für die **connectionString**-Eigenschaft ein. Markieren Sie dieses Feld als **SecureString**, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | JA |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| Mandant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie den Mauszeiger über den rechten oberen Bereich im Azure-Portal bewegen. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein  |

Weitere Voraussetzungen und JSON-Beispiele für die verschiedenen Authentifizierungstypen finden Sie in den folgenden Abschnitten:

- [SQL-Authentifizierung](#sql-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Dienstprinzipal](#service-principal-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Verwaltete Identitäten für Azure-Ressourcen](#managed-identity)

>[!TIP]
>Wenn ein Fehler mit dem Fehlercode „UserErrorFailedToConnectToSqlServer“ auftritt und eine Meldung wie „Das Sitzungslimit für die Datenbank ist XXX und wurde erreicht“ angezeigt wird, fügen Sie `Pooling=false` zu Ihrer Verbindungszeichenfolge hinzu, und versuchen Sie es erneut.

### <a name="sql-authentication"></a>SQL-Authentifizierung

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Beispiel eines verknüpften Diensts mit SQL-Authentifizierung

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Um die Azure AD-Anwendungstokenauthentifizierung basierend auf dem Dienstprinzipal zu verwenden, gehen Sie folgendermaßen vor:

1. **[Erstellen Sie eine Azure Active Directory-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** im Azure-Portal. Notieren Sie sich den Anwendungsnamen und die folgenden Werte zum Definieren des verknüpften Diensts:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

1. **[Geben Sie einen Azure Active Directory-Administrator](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** für Ihre Azure SQL Server-Instanz im Azure-Portal an, falls dies noch nicht geschehen ist. Der Azure AD-Administrator muss ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein, jedoch kein Dienstprinzipal. Dieser Schritt ist erforderlich, damit Sie im nachfolgenden Schritt eine Azure AD-Identität verwenden können, um einen Benutzer einer eigenständigen Datenbank für den Dienstprinzipal erstellen können.

1. **[Erstellen Sie eigenständige Datenbankbenutzer](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** für den Dienstprinzipal. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten mithilfe von Tools wie SSMS kopieren möchten. Verwenden Sie dazu eine Azure AD-Identität, die mindestens die Berechtigung ALTER ANY USER aufweist. Führen Sie den folgenden T-SQL-Befehl aus: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. **Gewähren Sie dem Dienstprinzipal die notwendigen Berechtigungen**, wie bei SQL- oder anderen Benutzern üblich. Führen Sie den folgenden Code aus:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. **Konfigurieren Sie einen mit Azure SQL-Datenbank verknüpften Dienst** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Beispiel eines verknüpften Diensts mit Dienstprinzipalauthentifizierung

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Verwaltete Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese Dienstidentität für die Azure SQL-Datenbank-Authentifizierung verwenden. Die angegebene Factory kann mithilfe dieser Identität auf Daten in Ihrer Datenbank zuzugreifen und diese kopieren.

Um die Azure AD-Anwendungstokenauthentifizierung basierend auf der MSI zu verwenden, gehen Sie folgendermaßen vor:

1. **Erstellen Sie eine Gruppe in Azure AD.** Fügen Sie die Factory-MSI der Gruppe als Mitglied hinzu.
    
    1. Suchen Sie die Data Factory-Dienstidentität im Azure-Portal. Wechseln Sie zu den **Eigenschaften** Ihrer Data Factory. Kopieren Sie die DIENSTIDENTITÄTS-ID.
    
    1. Installieren Sie das [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)-Modul. Melden Sie sich mit dem Befehl `Connect-AzureAD` an. Führen Sie die folgenden Befehle aus, um eine Gruppe zu erstellen und dieser die Data Factory-MSI als Mitglied hinzuzufügen.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```
    
1. **[Geben Sie einen Azure Active Directory-Administrator](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** für Ihre Azure SQL Server-Instanz im Azure-Portal an, falls dies noch nicht geschehen ist. Der Azure AD-Administrator kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn Sie der Gruppe mit der MSI eine Administratorrolle zuweisen möchten, überspringen Sie die Schritte 3 und 4. Der Administrator erhält Vollzugriff auf die Datenbank.

1. **[Erstellen Sie eigenständige Datenbankbenutzer](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** für die Azure AD-Gruppe. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten mithilfe von Tools wie SSMS kopieren möchten. Verwenden Sie dazu eine Azure AD-Identität, die mindestens die Berechtigung ALTER ANY USER aufweist. Führen Sie den folgenden T-SQL-Befehl aus: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

1. **Gewähren Sie der Azure AD-Gruppe die notwendigen Berechtigungen**, wie bei SQL- und anderen Benutzern üblich. Führen Sie beispielsweise den folgenden Code aus:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

1. **Konfigurieren Sie einen mit Azure SQL-Datenbank verknüpften Dienst** in Azure Data Factory.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Beispiel eines verknüpften Diensts mit MSI-Authentifizierung

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure SQL-Datenbank-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus bzw. nach Azure SQL-Datenbank die **type**-Eigenschaft des Datasets auf **AzureSqlTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft des Datasets muss auf **AzureSqlTable** festgelegt sein. | JA |
| tableName | Name der Tabelle oder Sicht in der Azure SQL-Datenbank-Instanz, auf die der verknüpfte Dienst verweist. | Quelle: Nein, Senke: Ja |

#### <a name="dataset-properties-example"></a>Beispiel für Dataseteigenschaften

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure SQL-Datenbank-Quelle und -Senke unterstützt werden.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL-Datenbank als Quelle

Legen Sie zum Kopieren von Daten aus Azure SQL-Datenbank die **type**-Eigenschaft der Quelle der Kopieraktivität auf **SqlSource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Quelle der Kopieraktivität muss auf **SqlSource** festgelegt sein. | JA |
| SqlReaderQuery | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `select * from MyTable`. | Nein  |
| sqlReaderStoredProcedureName | Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. | Nein  |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein  |

### <a name="points-to-note"></a>Beachten Sie Folgendes

- Wenn **sqlReaderQuery** für **SqlSource** angegeben wurde, führt die Kopieraktivität diese Abfrage für die Azure SQL-Datenbank-Quelle aus, um die Daten abzurufen. Sie können auch eine gespeicherte Prozedur angeben. Geben Sie dazu **sqlReaderStoredProcedureName** und **storedProcedureParameters** an (sofern die gespeicherte Prozedur Parameter akzeptiert).
- Ohne Angabe von **sqlReaderQuery** oder **sqlReaderStoredProcedureName** werden die im Abschnitt **structure** des JSON-Codes des Datasets definierten Spalten zum Erstellen einer Abfrage verwendet. `select column1, column2 from mytable` wird für Azure SQL-Datenbank ausgeführt. Falls die Datasetdefinition keinen Abschnitt **structure** enthält, werden alle Spalten der Tabelle ausgewählt.

#### <a name="sql-query-example"></a>Beispiel für eine SQL-Abfrage

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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

#### <a name="stored-procedure-example"></a>Beispiel für eine gespeicherte Prozedur

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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

### <a name="stored-procedure-definition"></a>Definition der gespeicherten Prozedur

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL-Datenbank als Senke

Legen Sie zum Kopieren von Daten aus Azure SQL-Datenbank die **type**-Eigenschaft in der Kopieraktivität auf **SqlSink** fest. Die folgenden Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Senke der Kopieraktivität muss auf **SqlSink** festgelegt sein. | JA |
| writeBatchSize | Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße **writeBatchSize** erreicht.<br/> Zulässiger Wert: **integer** (Anzahl der Zeilen) | Nein. Standard: 10000 |
| writeBatchTimeout | Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br/> Zulässiger Wert: **timespan**. Beispiel: 00:30:00 (30 Minuten) | Nein  |
| preCopyScript | Geben Sie eine auszuführende SQL-Abfrage für die Kopieraktivität an, ehe Sie Daten in Azure SQL-Datenbank schreiben. Sie wird pro Ausführung der Kopieraktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um vorab geladene Daten zu bereinigen. | Nein  |
| sqlWriterStoredProcedureName | Der Name der gespeicherten Prozedur, die definiert, wie Quelldaten auf eine Zieltabelle angewandt werden. Ein Beispiel ist für die ein Upsert-Vorgang bzw. eine Transformation mithilfe einer eigenen Geschäftslogik. <br/><br/>Diese gespeicherte Prozedur wird **pro Batch aufgerufen**. Für Vorgänge, die nur einmal ausgeführt werden und nichts mit den Quelldaten zu tun haben, verwenden Sie die `preCopyScript`-Eigenschaft. Beispielvorgänge sind das Löschen und Abschneiden. | Nein  |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte: Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein  |
| sqlWriterTableType | Geben Sie einen Tabellentypnamen an, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. | Nein  |

> [!TIP]
> Beim Kopieren von Daten nach Azure SQL-Datenbank fügt die Kopieraktivität standardmäßig Daten an die Senkentabelle an. Um einen Upsert-Vorgang oder zusätzliche Geschäftslogik auszuführen, verwenden Sie die in **SqlSink** gespeicherte Prozedur. Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur aus der SQL-Senke](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Beispiel für das Anfügen von Daten

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Aufrufen einer gespeicherten Prozedur während des Kopierens für einen Upsert-Vorgang

Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur aus der SQL-Senke](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
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

In diesem Abschnitt wird das Kopieren von Daten aus einer Quelltabelle ohne eine Identitätsspalte in eine Zieltabelle mit einer Identitätsspalte beschrieben.

#### <a name="source-table"></a>Quelltabelle

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Zieltabelle

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> Die Zieltabelle enthält eine Identitätsspalte.

#### <a name="source-dataset-json-definition"></a>JSON-Definition des Quelldatasets

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
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

#### <a name="destination-dataset-json-definition"></a>JSON-Definition des Zieldatasets

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
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

> [!NOTE]
> Ihre Quell- und Zieltabelle weisen unterschiedliche Schemas auf. 

Das Ziel enthält eine zusätzliche Spalte mit einer Identität. In diesem Szenario müssen Sie eine **structure**-Eigenschaft in der Definition des Zieldatasets angeben, die nicht die Identitätsspalte enthält.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten in Azure SQL-Datenbank können Sie auch eine vom Benutzer angegebene gespeicherte Prozedur mit zusätzlichen Parametern konfigurieren und aufrufen.

Sie können eine gespeicherte Prozedur nutzen, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Sie werden in der Regel verwendet, wenn ein Upsert-Vorgang, also eine Einfügung plus Update, oder eine zusätzliche Verarbeitung erfolgen muss, bevor die letzte Einfügung von Quelldaten in die Zieltabelle durchgeführt wird. Beispiele für zusätzliche Verarbeitungsschritte sind das Zusammenführen von Spalten, das Suchen zusätzlicher Werte und das Einfügen in mehr als einer Tabelle.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen Upsert-Vorgang in eine Tabelle in Azure SQL-Datenbank auszuführen. Wenn Eingabedaten vorhanden sind und die Senkentabelle **Marketing** heißt, sind drei Spalten vorhanden: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus, und wenden Sie ihn nur auf eine bestimmte Kategorie an.

#### <a name="output-dataset"></a>Ausgabedataset

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definieren Sie den Abschnitt **SqlSink** in der Kopieraktivität wie folgt:

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
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Definieren Sie in der Datenbank den Tabellentyp mit demselben Namen wie **sqlWriterTableType**. Das Schema des Tabellentyps sollte mit dem Schema übereinstimmen, das von den Eingabedaten zurückgegeben wird.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Wenn Sie durch Aufrufen einer gespeicherten Prozedur in den Datentyp „Money/Smallmoney“ schreiben, werden die Werte unter Umständen gerundet. Geben Sie den entsprechenden Datentyp im Tabellenwertparameter nicht als „Money/Smallmoney“, sondern als „Decimal“ an, um dies zu vermeiden. 

## <a name="data-type-mapping-for-azure-sql-database"></a>Datentypzuordnung für Azure SQL-Datenbank

Beim Kopieren von Daten aus bzw. nach Azure SQL-Datenbank werden die folgenden Zuordnungen von Azure SQL-Datenbank-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Azure SQL-Datenbankdatentyp | Data Factory-Zwischendatentyp |
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
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher und -formate](copy-activity-overview.md##supported-data-stores-and-formats).

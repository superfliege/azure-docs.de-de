---
title: "Importieren von Daten für die Verwendung mit der Table-API von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Daten für die Verwendung mit der Table-API von Azure Cosmos DB importieren."
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: b60743e2-0227-43ab-965a-0ae3ebacd917
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: fd0454252080679fa880eecb677d609ea0734f09
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="import-data-for-use-with-the-azure-cosmos-db-table-api"></a>Importieren von Daten für die Verwendung mit der Table-API von Azure Cosmos DB

In diesem Tutorial erfahren Sie, wie Sie Daten für die Verwendung mit der [Table-API](table-introduction.md) von Azure Cosmos DB importieren. Wenn Sie Daten in Azure Table Storage gespeichert haben, können Sie entweder das Datenmigrationstool oder AzCopy verwenden, um Ihre Daten zu importieren. Wenn Sie Daten in einem Konto der Tabellen-API von Azure Cosmos DB (Vorschauversion) gespeichert haben, müssen Sie Ihre Daten mit dem Datenmigrationstool migrieren. Nach dem Importieren der Daten stehen Ihnen die Premiumfunktionen von Azure Cosmos DB wie globale, sofort einsatzbereite Verteilung, dedizierter Durchsatz, Wartezeiten im einstelligen Millisekundenbereich im 99. Perzentil, garantierte Hochverfügbarkeit und automatische sekundäre Indizierung zur Verfügung.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Importieren von Daten mit dem Datenmigrationstool
> * Importieren von Daten mit AzCopy
> * Migrieren von der Tabellen-API (Vorschauversion) zur Tabellen-API 

## <a name="data-migration-tool"></a>Datenmigrationstool

Das Datenmigrationstool von Azure Cosmos DB für die Befehlszeile („dt.exe“) kann verwendet werden, um Ihre vorhandenen Azure Table Storage-Daten in ein Konto der allgemein verfügbaren Tabellen-API zu importieren oder um Daten aus einem Konto der Tabellen-API (Vorschauversion) zu einem Konto der allgemein verfügbaren Version der Tabellen-API zu migrieren. Andere Quellen werden derzeit nicht unterstützt.

Um eine Migration von Tabellendaten durchzuführen, führen Sie die folgenden Aufgaben aus:

1. Laden Sie das Migrationstool aus dem [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) oder von [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool) herunter.
2. Führen Sie `dt.exe` mit den Befehlszeilenargumenten für Ihr Szenario aus.

„dt.exe“ akzeptiert einen Befehl im folgenden Format:

    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 

Optionen für den Befehl:

    /ErrorLog: Optional. Name of the CSV file to redirect data transfer failures
    /OverwriteErrorLog: Optional. Overwrite error log file
    /ProgressUpdateInterval: Optional, default is 00:00:01. Time interval to refresh on-screen data transfer progress
    /ErrorDetails: Optional, default is None. Specifies that detailed error information should be displayed for the following errors: None, Critical, All

### <a name="command-line-source-settings"></a>Befehlszeilen-Quelleinstellungen

Verwenden Sie die folgenden Quelloptionen, wenn Sie Azure Table Storage oder die Vorschauversion der Tabellen-API als Quelle für die Migration festlegen.

    /s:AzureTable: Reads data from Azure Table storage
    /s.ConnectionString: Connection string for the table endpoint. This can be retrieved from the Azure portal
    /s.LocationMode: Optional, default is PrimaryOnly. Specifies which location mode to use when connecting to Azure Table storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
    /s.Table: Name of the Azure Table
    /s.InternalFields: Set to All for table migration as RowKey and PartitionKey are required for import.
    /s.Filter: Optional. Filter string to apply
    /s.Projection: Optional. List of columns to select

Um die Verbindungszeichenfolge für die Quelle beim Importieren aus Azure Table Storage abzurufen, öffnen Sie das Azure-Portal, klicken Sie auf **Speicherkonten** > **Konto** > **Zugriffsschlüssel**, und verwenden Sie dann die Schaltfläche „Kopieren“, um die **Verbindungszeichenfolge** zu kopieren.

![Screenshot der Optionen für HBase-Quellen](./media/table-import/storage-table-access-key.png)

Um die Verbindungszeichenfolge für die Quelle beim Importieren aus der Tabellen-API von Azure Cosmos DB abzurufen, öffnen Sie das Azure-Portal, klicken Sie auf **Azure Cosmos DB** > **Konto** > **Verbindungszeichenfolge**, und kopieren Sie dann die **Verbindungszeichenfolge** über die Schaltfläche „Kopieren“.

![Screenshot der Optionen für HBase-Quellen](./media/table-import/cosmos-connection-string.png)

[Beispielbefehl für Azure Table Storage](#azure-table-storage)

[Beispielbefehl für Azure Cosmos DB-Tabellen-API (Vorschauversion)](#table-api-preview)

### <a name="command-line-target-settings"></a>Befehlszeilen-Zieleinstellungen

Verwenden Sie die folgenden Zieloptionen, wenn Sie die Tabellen-API von Azure Cosmos DB als Ziel für die Migration festlegen.

    /t:TableAPIBulk: Uploads data into Azure CosmosDB Table in batches
    /t.ConnectionString: Connection string for the table endpoint
    /t.TableName: Specifies the name of the table to write to
    /t.Overwrite: Optional, default is false. Specifies if existing values should be overwritten
    /t.MaxInputBufferSize: Optional, default is 1GB. Approximate estimate of input bytes to buffer before flushing data to sink
    /t.Throughput: Optional, service defaults if not specified. Specifies throughput to configure for table
    /t.MaxBatchSize: Optional, default is 2MB. Specify the batch size in bytes

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Beispielbefehl: Azure Table Storage als Quelle

Hier finden Sie ein Beispiel für eine Befehlszeile zum Importieren aus Azure Table Storage in die Tabellen-API:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey==<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```
<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Beispielbefehl: Azure Cosmos DB-Tabellen-API (Vorschauversion) als Quelle

Hier finden Sie ein Beispiel für eine Befehlszeile zum Importieren aus der Vorschauversion der Tabellen-API in die allgemein verfügbare Version:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="azcopy-command"></a>AzCopy-Befehl

Die andere Option zum Migrieren von Daten aus Azure Table Storage in die Table-API von Azure Cosmos DB ist das AzCopy-Befehlszeilenprogramm. Bei Verwendung von AzCopy exportieren Sie Ihre Daten zunächst wie in [Exportieren von Daten aus Table Storage](../storage/common/storage-use-azcopy.md#export-data-from-table-storage) beschrieben und importieren sie anschließend wie in [Tabellen-API von Azure Cosmos DB](../storage/common/storage-use-azcopy.md#import-data-into-table-storage) beschrieben in Azure Cosmos DB.

Das folgende Beispiel zeigt das Importieren in Azure Cosmos DB. Beachten Sie, dass für „/dest“ der Wert „cosmosdb“ und nicht „core“ verwendet wird.

Beispiel für den import-Befehl:

```
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Migrieren von der Tabellen-API (Vorschauversion) zur Tabellen-API

> [!WARNING]
> Wenn Sie sofort von den Vorteilen der allgemein verfügbaren Tabellen profitieren möchten, migrieren Sie Ihre vorhandenen Tabellen aus der Vorschauversion wie in diesem Abschnitt beschrieben. Andernfalls führen wir in den kommenden Wochen automatische Migrationen für bestehende Kunden der Vorschauversion durch. Beachten Sie jedoch, dass für die automatisch migrierten Tabellen aus der Vorschauversion bestimmte Einschränkungen gelten, die neu erstellte Tabellen nicht betreffen.
> 

Die Tabellen-API ist jetzt allgemein verfügbar. Es gibt Unterschiede zwischen Tabellen der Vorschauversion und der allgemein verfügbaren Version – bei Code, der in der Cloud ausgeführt wird, ebenso wie bei Code, der auf dem Client ausgeführt wird. Es wird daher nicht empfohlen, einen Client der SDK-Vorschauversion mit einem Konto der allgemein verfügbaren Tabellen-API zu kombinieren. Benutzer der Tabellen-API, die weiterhin ihre vorhandenen Tabellen, jedoch in einer Produktionsumgebung verwenden möchten, müssen von der Vorschauversion zur allgemein verfügbaren Umgebung migrieren oder auf die automatische Migration warten. Wenn Sie auf die automatische Migration warten, werden Sie über die Einschränkungen bei den migrierten Tabellen benachrichtigt. Nach der Migration können Sie ohne Einschränkungen neue Tabellen in Ihrem vorhandenen Konto erstellen (nur für migrierte Tabellen gelten Einschränkungen).

So migrieren Sie von der Tabellen-API (Vorschauversion) zur allgemein verfügbaren Tabellen-API

1. Erstellen Sie ein neues Azure Cosmos DB-Konto, und legen Sie den API-Typ wie in [Erstellen eines Datenbankkontos](create-table-dotnet.md#create-a-database-account) beschrieben auf Azure-Tabelle fest.

2. Ändern Sie die Clients, sodass diese die allgemein verfügbare Version der [Tabellen-API SDKs](table-sdk-dotnet.md) verwenden.

3. Migrieren Sie die Clientdaten mit dem Datenmigrationstool aus den Tabellen der Vorschauversion zu den Tabellen der allgemein verfügbaren Version. Anweisungen zum Verwenden des Datenmigrationstools für diesen Zweck finden Sie unter [Datenmigrationstool](#data-migration-tool). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren von Daten mit dem Datenmigrationstool
> * Importieren von Daten mit AzCopy
> * Migrieren von der Tabellen-API (Vorschauversion) zur Tabellen-API

Im nächsten Tutorial erfahren Sie, wie Sie Daten mithilfe der Table-API von Azure Cosmos DB abfragen. 

> [!div class="nextstepaction"]
>[Abfragen von Daten](../cosmos-db/tutorial-query-table.md)

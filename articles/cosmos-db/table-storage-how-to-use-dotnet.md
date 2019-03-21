---
title: Erste Schritte mit Azure Table Storage und der Azure Cosmos DB-Tabellen-API mit .NET
description: Speichern Sie mit Azure Table Storage oder der Azure Cosmos DB-Tabellen-API strukturierte Daten in der Cloud.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: sample
ms.date: 08/17/2018
ms.openlocfilehash: 8d9037a0f557a39ba4384981d9f31daa5f1ca6e4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57842650"
---
# <a name="get-started-with-azure-table-storage-and-the-azure-cosmos-db-table-api-using-net"></a>Erste Schritte mit Azure Table Storage und der Azure Cosmos DB-Tabellen-API mit .NET
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Sie können den Azure Table Storage-Dienst oder die Azure Cosmos DB-Tabellen-API verwenden, um strukturierte NoSQL-Daten in der Cloud zu speichern. Dazu wird ein Schlüssel-/Attributspeicher mit einem schemalosen Design angeboten. Aufgrund der Schemalosigkeit von Table Storage und der Azure Cosmos DB-Tabellen-API ist es einfach, Ihre Daten an die Entwicklung Ihrer Anwendungen anzupassen. Viele Arten von Anwendungen können schnell und kostengünstig auf Table Storage-Daten und die Azure Cosmos DB-Tabellen-API zugreifen, und die Kosten liegen in der Regel unter den Kosten herkömmlicher SQL-Lösungen für vergleichbare Datenmengen.

Mit Table Storage oder der Azure Cosmos DB-Tabellen-API können Sie flexible Datasets wie Benutzerdaten für Webanwendungen, Adressbücher, Geräteinformationen und andere Arten von Metadaten speichern, die Ihr Dienst benötigt. Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern, und ein Speicherkonto oder ein Tabellen-API-Konto kann eine beliebige Anzahl von Tabellen enthalten (bis zur Speicherkapazitätsgrenze des Speicher- oder Tabellen-API-Kontos).

### <a name="about-this-sample"></a>Informationen zu diesem Beispiel
In diesem Beispiel erfahren Sie, wie Sie die [Microsoft Azure CosmosDB-Tabellenbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) in allgemeinen Azure Table Storage- und Tabellen-API-Szenarien verwenden. Der Name des Pakets weist darauf hin, dass es für Azure Cosmos DB gedacht ist, das Paket funktioniert aber sowohl mit der Azure Cosmos DB-Tabellen-API als auch mit Azure Table Storage, da jeder Dienst einen eindeutigen Endpunkt aufweist. Diese Szenarien werden mithilfe von C#-Beispielen untersucht, die Folgendes veranschaulichen:
* Erstellen und Löschen von Tabellen
* Einfügen, Aktualisieren und Löschen von Zeilen
* Abfragen von Tabellen

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel benötigen Sie Folgendes:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Allgemeine Azure Storage-Bibliothek für .NET (Vorschau)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/) –  ein erforderliches Vorschaupaket, das in Produktionsumgebungen unterstützt wird. 
* [Microsoft Azure Cosmos DB-Tabellenbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table): Diese Bibliothek ist derzeit nur für .NET Standard verfügbar, für .NET Core noch nicht.
* [Azure-Speicherkonto](../storage/common/storage-quickstart-create-account.md)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Weitere Beispiele
Weitere Beispiele für die Verwendung von Table Storage finden Sie unter [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)(Erste Schritte mit Azure Table Storage in .NET). Sie können die Beispielanwendung herunterladen und ausführen oder den Code auf GitHub durchsuchen.

## <a name="create-an-azure-service-account"></a>Erstellen eines Azure-Dienstkontos
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos
* Ihr erstes Azure-Speicherkonto erstellen Sie am einfachsten im [Azure-Portal](https://portal.azure.com). Weitere Informationen finden Sie unter [Erstellen von Speicherkonten](../storage/common/storage-quickstart-create-account.md).

* Ein Azure-Speicherkonto können Sie auch mit [Azure PowerShell](../storage/common/storage-powershell-guide-full.md), der [Azure-Befehlszeilenschnittstelle](../storage/common/storage-azure-cli.md) oder der [Storage Resource Provider-Clientbibliothek für .NET](/dotnet/api/microsoft.azure.management.storage) erstellen.

* Wenn Sie zu diesem Zeitpunkt kein Speicherkonto erstellen möchten, können Sie auch den Azure-Speicheremulator zum Ausführen und Testen Ihres Codes in einer lokalen Umgebung verwenden. Weitere Informationen finden Sie unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Erstellen eines Kontos für die Azure Cosmos DB-Tabellen-API
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

### <a name="create-a-windows-console-application-project"></a>Erstellen eines Windows-Konsolenanwendungsprojekts
Erstellen Sie in Visual Studio eine neue Windows-Konsolenanwendung. In den folgenden Schritten wird veranschaulicht, wie Sie eine Konsolenanwendung in Visual Studio 2017 erstellen. Die Schritte sind in anderen Versionen von Visual Studio ähnlich.

1. Wählen Sie **Datei** > **Neu** > **Projekt**.
2. Wählen Sie **Installiert** > **Visual C#** > **Klassischer Windows-Desktop** aus.
3. Wählen Sie **Konsolen-App (.NET Framework)**.
4. Geben Sie im Feld **Name** einen Namen für Ihre Anwendung ein.
5. Klicken Sie auf **OK**.

Alle Codebeispiele in diesem Beispiel können in der Datei `Program.cs` Ihrer Konsolenanwendung der `Main()`-Methode hinzugefügt werden.

Sie können die Azure Cosmos DB-Tabellenbibliothek in jeder Art von .NET-Anwendung nutzen, z.B. einem Azure-Clouddienst, einer Azure-Web-App, einer Desktopanwendung oder einer mobilen Anwendung. In diesem Leitfaden verwenden wir der Einfachheit halber eine Konsolenanwendung.

### <a name="install-the-required-nuget-packages"></a>Installieren der erforderlichen NuGet-Pakete
Es gibt drei empfohlene Pakete, auf die Sie in Ihrem Projekt verweisen müssen, um dieses Beispiel abzuschließen:

* [Allgemeine Azure Storage-Bibliothek für .NET (Vorschau)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common) Verwenden Sie höchstens Version 9.0.0.1 (< = 9.0.0.1).

* [Microsoft Azure Cosmos DB-Tabellenbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) Mit diesem Paket erhalten Sie programmgesteuerten Zugriff auf Datenressourcen in Ihrem Azure Table Storage-Konto bzw. Ihrem Konto für die Azure Cosmos DB-Tabellen-API. Diese Bibliothek ist derzeit nur für .NET Standard verfügbar, für .NET Core noch nicht.

Führen Sie diese Schritte aus, um die NuGet-Pakete abzurufen:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten**.
2. Suchen Sie online nach „Microsoft.Azure.Storage.Common“, wählen Sie Version <= 9.0.0.1, und wählen Sie **Installieren** aus, um die allgemeine Azure Storage-Bibliothek für .NET (Vorschau) und die zugehörigen Abhängigkeiten zu installieren. Stellen Sie sicher, dass das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist, da es sich um ein Vorschaupaket handelt.
3. Suchen Sie online nach „Microsoft.Azure.CosmosDB.Table“, und wählen Sie **Installieren** aus, um die Microsoft Azure Cosmos DB-Tabellenbibliothek zu installieren.

> [!NOTE]
> Die ODataLib-Abhängigkeiten in der allgemeinen Storage-Bibliothek für .NET werden mit den ODataLib-Paketen aufgelöst, die auf NuGet verfügbar sind (nicht von WCF Data Services). Die ODataLib-Bibliotheken können direkt heruntergeladen werden, oder es wird über Ihr Codeprojekt durch NuGet darauf verwiesen. Die spezifischen ODataLib-Pakete, die von der Speicherclientbibliothek verwendet werden, lauten [OData](https://nuget.org/packages/Microsoft.Data.OData/), [Edm](https://nuget.org/packages/Microsoft.Data.Edm/) und [Spatial](https://nuget.org/packages/System.Spatial/). Diese Bibliotheken werden von Azure Table Storage-Klassen verwendet und sind erforderliche Abhängigkeiten für die Programmierung mit der allgemeinen Storage-Bibliothek.
> 
> 

> [!TIP]
> Entwickler, die bereits mit Azure Table Storage vertraut sind, haben in der Vergangenheit möglicherweise das [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)-Paket verwendet. Es wird empfohlen, dass alle neuen Tabellenanwendungen die [allgemeine Azure Storage-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common) und [Azure Cosmos DB-Tabellenbibliothek](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) verwenden, wobei auch das WindowsAzure.Storage-Paket weiterhin unterstützt wird. Wenn Sie die WindowsAzure.Storage-Bibliothek verwenden, schließen Sie Microsoft.WindowsAzure.Storage.Table in Ihre using-Anweisungen ein.
>
>

### <a name="determine-your-target-environment"></a>Bestimmen der Zielumgebung
Es gibt für die Ausführung der Beispiele in diesem Leitfaden drei Umgebungsoptionen:

* Sie können den Code über ein Azure Storage-Konto in der Cloud ausführen. 
* Sie können den Code über ein Azure Cosmos DB-Konto in der Cloud ausführen.
* Sie können den Code über den Azure-Speicheremulator ausführen. Der Speicheremulator ist eine lokale Umgebung, in der ein Azure Storage-Konto in der Cloud emuliert wird. Der Emulator ist eine kostenlose Option zum Testen und Debuggen von Code, während sich Ihre Anwendung in der Entwicklung befindet. Für den Emulator wird ein bekanntes Konto mit Schlüssel verwendet. Weitere Informationen finden Sie unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](../storage/common/storage-use-emulator.md).

Wenn Sie ein Speicherkonto in der Cloud verwenden möchten, müssen Sie den primären Zugriffsschlüssel für Ihr Speicherkonto aus dem Azure-Portal kopieren. Weitere Informationen finden Sie unter [Speicherkonto-Zugriffsschlüssel](../storage/common/storage-account-manage.md#access-keys).

> [!NOTE]
> Sie können den Speicheremulator als Ziel festlegen, um jegliche Kosten im Zusammenhang mit Azure Storage zu vermeiden. Wenn jedoch ein Azure-Speicherkonto in der Cloud das Ziel sein soll, sind die Kosten für das Ausführen dieses Beispiels vernachlässigbar.
> 
> 

Wenn Sie ein Azure Cosmos DB-Konto verwenden möchten, kopieren Sie den primären Zugriffsschlüssel für Ihr Tabellen-API-Konto aus dem Azure-Portal. Weitere Informationen finden Sie unter [Aktualisieren der Verbindungszeichenfolge](create-table-dotnet.md#update-your-connection-string).

### <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
Die allgemeine Azure Storage-Bibliothek für .NET unterstützt die Verwendung einer Speicherverbindungszeichenfolge zum Konfigurieren von Endpunkten und Anmeldeinformationen für den Zugriff auf Speicherdienste. Am einfachsten lässt sich die Speicherverbindungszeichenfolge in einer Konfigurationsdatei verwalten. 

Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Verbindungszeichenfolgen für Azure Storage](../storage/common/storage-configure-connection-string.md).

> [!NOTE]
> Ihr Kontoschlüssel entspricht dem Stammkennwort für das Speicherkonto. Achten Sie darauf, den Speicherkontoschlüssel immer gut zu schützen. Geben Sie ihn nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie ihn nicht in einer Klartextdatei, auf die andere Benutzer zugreifen können. Generieren Sie Ihren Schlüssel mithilfe des Azure-Portals neu, wenn Sie der Meinung sind, dass er nicht mehr sicher ist.
> 

Öffnen Sie zum Konfigurieren der Verbindungszeichenfolge die Datei `app.config` im Projektmappen-Explorer in Visual Studio. Fügen Sie den Inhalt des `<appSettings>` -Elements hinzu (wie unten dargestellt). Ersetzen Sie `account-name` durch den Namen Ihres Kontos und `account-key` durch den Zugriffsschlüssel des Kontos:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Wenn Sie ein Azure Storage-Konto verwenden, sollten Ihre Konfigurationseinstellungen etwa so aussehen:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>" />
```

Wenn Sie ein Azure Cosmos DB-Konto verwenden, sollten Ihre Konfigurationseinstellungen etwa so aussehen:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=tableapiacct;AccountKey=<account-key>;TableEndpoint=https://tableapiacct.table.cosmosdb.azure.com:443/;" />
```

Wenn Sie den Speicheremulator verwenden möchten, können Sie eine Verknüpfung nutzen, mit der eine Zuordnung zum bekannten Kontonamen und dem Schlüssel hergestellt wird. In diesem Fall lautet die Verbindungszeichenfolge wie folgt:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

### <a name="add-using-directives"></a>Hinzufügen von using-Direktiven
Fügen Sie oben in der Datei `Program.cs` die folgenden **using**-Direktiven hinzu:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for StorageAccounts
using Microsoft.Azure.CosmosDB.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Erstellen des Tabellenspeicherdienst-Clients
Mit der [CloudTableClient][dotnet_CloudTableClient]-Klasse können Sie in Table Storage gespeicherte Tabellen und Entitäten abrufen. Hier sehen Sie ein Beispiel für die Erstellung des Tabellenspeicherdienst-Clients:

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Jetzt können Sie Code schreiben, der Daten aus dem Tabellenspeicher liest und Daten in den Tabellenspeicher schreibt.

## <a name="create-a-table"></a>Erstellen einer Tabelle
Dieses Beispiel zeigt, wie Sie eine Tabelle erstellen, falls sie nicht bereits vorhanden ist:

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
Entitäten werden C#-Objekten mithilfe einer benutzerdefinierten, von [TableEntity][dotnet_TableEntity] abgeleiteten Klasse zugeordnet. Erstellen Sie eine Klasse, mit der die Eigenschaften der Entität definiert werden, um eine Entität zu einer Tabelle hinzuzufügen. Mit dem folgenden Code wird eine Entitätsklasse definiert, die den Vornamen des Kunden als Zeilenschlüssel und den Nachnamen als Partitionsschlüssel verwendet. Eine Entität in der Tabelle wird durch die Kombination aus Partitions- und Zeilenschlüssel eindeutig identifiziert. Entitäten mit dem gleichen Partitionsschlüssel können schneller abgerufen werden als Entitäten mit unterschiedlichen Partitionsschlüsseln, die Verwendung verschiedener Partitionsschlüssel ermöglicht jedoch eine größere Skalierbarkeit paralleler Vorgänge. Bei Entitäten, die in Tabellen gespeichert werden sollen, muss es sich um einen unterstützten Typ handeln (beispielsweise abgeleitet von der [TableEntity][dotnet_TableEntity]-Klasse). Bei Entitätseigenschaften, die Sie in einer Tabelle speichern möchten, muss es sich um öffentliche Eigenschaften des Typs handeln, und sie müssen sowohl das Abrufen als auch das Festlegen von Werten unterstützen. Darüber hinaus *muss* der Entitätstyp einen parameterlosen Konstruktor verfügbar machen.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Tabellenvorgänge mit Entitäten werden über das [CloudTable][dotnet_CloudTable]-Objekt ausgeführt, das Sie im Abschnitt „Erstellen einer Tabelle“ erstellt haben. Der auszuführende Vorgang wird durch ein [TableOperation][dotnet_TableOperation]-Objekt dargestellt. Das folgende Codebeispiel zeigt die Erstellung des [CloudTable][dotnet_CloudTable]-Objekts und eines **CustomerEntity**-Objekts. Zur Vorbereitung des Vorgangs wird ein [TableOperation][dotnet_TableOperation]-Objekt erstellt, um die Kundenentität in die Tabelle einzufügen. Abschließend wird der Vorgang durch Aufrufen von [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute] ausgeführt.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Einfügen eines Entitätsbatchs
Sie können einen Entitätsbatch in einem Schreibvorgang in eine Tabelle einfügen. Beachten Sie im Zusammenhang mit Batchvorgängen Folgendes:

* Sie können Aktualisierungs-, Lösch- und Einfügevorgänge in einem einzigen Batchvorgang ausführen.
* Ein einzelner Batchvorgang kann bis zu 100 Entitäten umfassen.
* Alle Entitäten in einem Batchvorgang müssen über denselben Partitionsschlüssel verfügen.
* Eine Abfrage kann als Batchvorgang durchgeführt werden, dabei muss es sich jedoch um den einzigen Vorgang im Batch handeln.

Im folgenden Codebeispiel werden zwei Entitätsobjekte erstellt und jeweils mithilfe der [Insert][dotnet_TableBatchOperation_Insert]-Methode zu [TableBatchOperation][dotnet_TableBatchOperation] hinzugefügt. Danach wird [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] aufgerufen, um den Vorgang auszuführen.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a>Abrufen aller Entitäten einer Partition
Verwenden Sie ein [TableQuery][dotnet_TableQuery]-Objekt, um für eine Tabelle alle Entitäten in einer Partition abzufragen. Im folgenden Codebeispiel wird ein Filter für Entitäten erstellt, wobei "Smith" der Partitionsschlüssel ist. In diesem Beispiel werden die Felder der einzelnen Entitäten in den Abfrageergebnissen an die Konsole ausgegeben.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Abrufen eines Entitätsbereichs in einer Partition
Wenn Sie nicht alle Entitäten in einer Partition abfragen möchten, können Sie einen Bereich angeben, indem Sie den Partitionsschlüsselfilter mit einem Zeilenschlüsselfilter kombinieren. Im folgenden Codebeispiel werden zwei Filter verwendet, um alle Entitäten in der Partition „Smith“ abzurufen, deren Zeilenschlüssel (Vorname) mit einem Buchstaben vor „E“ im Alphabet beginnt. Danach werden die Abfrageergebnisse ausgegeben.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a>Abrufen einer einzelnen Entität
Sie können eine Abfrage schreiben, um eine einzelne bestimmte Entität abzurufen. Im folgenden Code wird [TableOperation][dotnet_TableOperation] verwendet, um den Kunden „Ben Smith“ anzugeben. Diese Methode gibt anstelle einer Sammlung nur eine einzelne Entität zurück, und bei dem zurückgegebenen Wert in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] handelt es sich um ein **CustomerEntity**-Objekt. Die Angabe beider Schlüssel, Partition und Zeile, in einer Abfrage ist die schnellste Möglichkeit, um eine einzelne Entität aus dem Tabellenspeicherdienst abzurufen.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a>Ersetzen einer Entität
Um eine Entität zu aktualisieren, rufen Sie sie aus dem Tabellendienst ab, ändern Sie das Entitätsobjekt, und speichern Sie die Änderungen dann im Tabellendienst. Mit dem folgenden Code wird die Telefonnummer eines vorhandenen Kunden geändert. Anstelle von [Insert][dotnet_TableOperation_Insert] wird in diesem Code [Replace][dotnet_TableOperation_Replace] verwendet. Durch [Replace][dotnet_TableOperation_Replace] wird die Entität auf dem Server vollständig ersetzt, sofern sich die Entität auf dem Server seit dem letzten Abruf nicht geändert hat. In diesem Fall wäre der Vorgang nicht erfolgreich. Auf diese Weise soll verhindert werden, dass eine Änderung, die zwischen dem Abruf und der Aktualisierung durch eine andere Komponente der Anwendung vorgenommen wurde, unabsichtlich überschrieben wird. Die richtige Vorgehensweise in diesem Fall besteht darin, die Entität erneut abzurufen, die Änderungen vorzunehmen (falls diese noch gültig sind) und dann einen weiteren [Replace][dotnet_TableOperation_Replace]-Vorgang auszuführen. Im nächsten Abschnitt erfahren Sie, wie Sie dieses Verhalten überschreiben.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a>Einfügen-oder-Ersetzen einer Entität
[Replace][dotnet_TableOperation_Replace]-Vorgänge sind nicht erfolgreich, wenn die Entität seit dem letzten Abruf vom Server geändert wurde. Darüber hinaus müssen Sie zuerst die Entität vom Server abrufen, damit der [Replace][dotnet_TableOperation_Replace]-Vorgang erfolgreich ist. Manchmal ist jedoch nicht bekannt, ob die Entität auf dem Server vorhanden ist, und die darin aktuell gespeicherten Werte sind nicht relevant. In diesem Fall sollten letztere durch die Aktualisierung vollständig überschrieben werden. Dazu müsste ein [InsertOrReplace][dotnet_TableOperation_InsertOrReplace]-Vorgang verwendet werden. Bei diesem Vorgang wird die Entität eingefügt, falls sie nicht vorhanden ist, oder ersetzt, falls sie vorhanden ist, unabhängig davon, wann die letzte Aktualisierung stattgefunden hat.

Im folgenden Codebeispiel wird eine Kundenentität für „Fred Jones“ erstellt und in die Tabelle „people“ eingefügt. Als Nächstes speichern wir mithilfe des [InsertOrReplace][dotnet_TableOperation_InsertOrReplace]-Vorgangs eine Entität mit dem gleichen Partitionsschlüssel (Jones) und Zeilenschlüssel (Fred) auf dem Server – diesmal allerdings mit einem anderen Wert für die PhoneNumber-Eigenschaft. Aufgrund der Verwendung von [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] werden alle dazugehörigen Eigenschaftswerte ersetzt. Falls die Entität „Fred Jones“ in der Tabelle noch nicht vorhanden gewesen wäre, wäre sie eingefügt worden.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute the operation.
table.Execute(insertOperation);

// Create another customer entity with the same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it to the
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create the InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute the operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, the entity would be
// added to the table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a>Abfragen einer Teilmenge von Entitätseigenschaften
Mit einer Tabellenabfrage können nicht nur alle, sondern auch nur einige Eigenschaften aus einer Entität abgerufen werden. Mit dieser Technik, der sogenannten Projektion, wird die Bandbreite reduziert und die Abfrageleistung gesteigert, vor allem bei großen Entitäten. Mit der Abfrage im folgenden Code werden nur die E-Mail-Adressen von Entitäten in der Tabelle zurückgegeben. Hierzu werden eine Abfrage vom Typ [DynamicTableEntity][dotnet_DynamicTableEntity] sowie [EntityResolver][dotnet_EntityResolver] verwendet. Die Projektion wird vom Speicheremulator nicht unterstützt, weshalb dieser Code nur ausgeführt wird, wenn Sie ein Konto im Tabellenspeicherdienst verwenden.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a>Löschen einer Entität
Eine Entität kann nach dem Abrufen problemlos gelöscht werden. Verwenden Sie dazu das gleiche Muster wie beim Aktualisieren einer Entität. Durch den nachstehenden Code wird eine Kundenentität aufgerufen und gelöscht.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a>Löschen einer Tabelle
Schließlich wird mit dem folgenden Codebeispiel eine Tabelle aus einem Speicherkonto gelöscht. Eine gelöschte Tabelle kann für eine bestimmte Zeitdauer nach dem Löschvorgang nicht neu erstellt werden.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchrones Abrufen von Entitäten in Seiten
Wenn Sie eine große Anzahl von Entitäten lesen und die Entitäten verarbeiten/anzeigen möchten, während sie abgerufen werden, statt zu warten, bis sie alle zurückgeben worden sind, können Sie Entitäten mit einer segmentierten Abfrage abrufen. In diesem Beispiel wird gezeigt, wie Sie mit dem Async-Await-Muster die Ergebnisse seitenweise zurückgeben, sodass die Ausführung nicht blockiert ist, während Sie auf die Rückgabe eines großen Ergebnissatzes warten. Weitere Informationen zur Verwendung des Async-Await-Musters in .NET finden Sie unter [Asynchrone Programmierung mit Async und Await (C# und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen des Tabellenspeichers vertraut gemacht haben, folgen Sie diesen Links, um zu erfahren, wie komplexere Speicheraufgaben ausgeführt werden.

* Beim [Microsoft Azure Storage-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
* Weitere Beispiele für Tabellenspeicher finden Sie unter [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
* In der Referenzdokumentation für den Tabellenspeicherdienst finden Sie alle Details zu verfügbaren APIs:
* [Referenz zur Storage-Clientbibliothek für .NET](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [REST-API-Referenz](https://msdn.microsoft.com/library/azure/dd179355)
* Erfahren Sie, wie Sie mithilfe des [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki)
* Weitere Informationen zu zusätzlichen Optionen für das Speichern von Daten in Azure finden Sie in den anderen Featureleitfäden.
* [Erste Schritte mit Azure Blob Storage mit .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) zum Speichern unstrukturierter Daten
* Informationen zum Speichern relationaler Daten finden Sie unter [Herstellen von Verbindungen mit SQL-Datenbank mithilfe von .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md).

[Download and install the Azure SDK for .NET]: /develop/net/

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx
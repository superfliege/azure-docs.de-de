---
title: Erste Schritte mit Azure Stack-Speicher-Entwicklungstools
description: Anleitung zu ersten Schritten mit Azure Stack-Speicher-Entwicklungstools
services: azure-stack
author: xiaofmao
ms.author: xiaofmao
ms.date: 9/25/2017
ms.topic: get-started-article
ms.service: azure-stack
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5b2898c64c0f1b5d804e63fa4e4e1218fa7a672c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="get-started-with-azure-stack-storage-development-tools"></a>Erste Schritte mit Azure Stack-Speicher-Entwicklungstools

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*


Microsoft Azure Stack bietet eine Reihe von Speicherdiensten einschließlich Azure Blob, Table und Queue Storage.

Dieser Artikel ist eine Kurzanleitung zum Einstieg in die Verwendung von Azure Stack-Speicher-Entwicklungstools. Ausführlichere Informationen und Beispielcode finden Sie in den entsprechenden Azure Storage-Tutorials.

Es gibt bekannte Unterschiede zwischen Azure Storage und Azure Stack-Speicher, einschließlich einiger besonderen Anforderungen für jede Plattform. Beispielsweise gelten bestimmte Clientbibliotheken- und Endpunktsuffix-Anforderungen für Azure Stack. Weitere Informationen finden Sie unter [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Azure-Clientbibliotheken
Die unterstützte REST-API-Version für Azure Stack-Speicher ist 2015-04-05. Sie ist der neuesten Version der REST-API von Azure Storage nicht vollständig gleichgestellt. Daher müssen Sie bei Speicherclientbibliotheken auf die Version achten, die mit REST-API 2015-04-05 kompatibel ist.


|Clientbibliothek|Von Azure Stack unterstützte Version|Link|Endpunktspezifikation|
|---------|---------|---------|---------|
|.NET     |6.2.0|NuGet-Paket:<br>[https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>GitHub-Release:<br>[https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|app.config-Datei|
|Java|4.1.0|Maven-Paket:<br>[http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>GitHub-Release:<br> [https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|Verbindungszeichenfolgen-Setup|
|Node.js     |1.1.0|NPM-Link:<br>[https://www.npmjs.com/package/azure-storage](https://www.npmjs.com/package/azure-storage)<br>(Ausführen: `npm install azure-storage@1.1.0)`<br><br>GitHub-Release:<br>[https://github.com/Azure/azure-storage-node/releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|Dienstinstanzdeklaration||C++|2.4.0|NuGet-Paket:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub-Release:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Verbindungszeichenfolgen-Setup|
|C++|2.4.0|NuGet-Paket:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>GitHub-Release:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Verbindungszeichenfolgen-Setup|
|PHP|0.15.0|GitHub-Release:<br>[https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>Installation über Composer (Details siehe unten)|Verbindungszeichenfolgen-Setup|
|Python     |0.30.0|PIP-Paket:<br> [https://pypi.python.org/pypi/azure-storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>(Ausführen: `pip install -v azure-storage==0.30.0)`<br><br>GitHub-Release:<br> [https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|Dienstinstanzdeklaration|
|Ruby|0.12.1<br>Vorschau|RubyGems-Paket:<br> [https://rubygems.org/gems/azure-storage/versions/0.12.1.preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>GitHub-Release:<br> [https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|Verbindungszeichenfolgen-Setup|

> [!NOTE]
> PHP-Details<br><br>
>So führen Sie eine Installation mit dem Composer durch:
>1. Erstellen Sie eine Datei mit dem Namen `composer.json` im Stammverzeichnis des Projekts mit folgendem Code:<br>
>
>   ```
>   {
>       "require":{
>           "Microsoft/azure-storage":"0.15.0"
>        }
>    }
>   ```
>
>2. Laden Sie [composer.phar](http://getcomposer.org/composer.phar) in das Stammverzeichnis des Projekts herunter.
>3. Führen Sie `php composer.phar install` aus.
>


## <a name="endpoint-declaration"></a>Endpunktdeklaration
Ein Azure Stack-Endpunkt besteht aus zwei Teilen: dem Namen einer Region und der Azure Stack-Domäne.
Im Azure Stack Development Kit ist der Standardendpunkt **local.azurestack.external**.
Wenn Sie sich über Ihren Endpunkt nicht sicher sind, wenden Sie sich an den Cloudadministrator.

## <a name="examples"></a>Beispiele


### <a name="net"></a>.NET

Für Azure Stack ist das Endpunktsuffix in der Datei „app.config“ angegeben:

```
<add key="StorageConnectionString" 
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```
### <a name="java"></a>Java

Für Azure Stack ist das Endpunktsuffix im Setup der Verbindungszeichenfolge angegeben:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Für Azure Stack ist das Endpunktsuffix in der Deklarationsinstanz angegeben:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```
### <a name="c"></a>C++

Für Azure Stack ist das Endpunktsuffix im Setup der Verbindungszeichenfolge angegeben:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Für Azure Stack ist das Endpunktsuffix im Setup der Verbindungszeichenfolge angegeben:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Für Azure Stack ist das Endpunktsuffix in der Deklarationsinstanz angegeben:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```
### <a name="ruby"></a>Ruby

Für Azure Stack ist das Endpunktsuffix im Setup der Verbindungszeichenfolge angegeben:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob Storage

Die folgenden Tutorials zu Azure Blob Storage gelten für Azure Stack. Beachten Sie die im vorherigen Abschnitt [Beispiele](#examples) beschriebene bestimmte Endpunktsuffix-Voraussetzung für Azure Stack.

* [Erste Schritte mit Azure Blob Storage mit .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Gewusst wie: Verwenden von Blob Storage mit Java](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Verwenden von Blob Storage mit Node.js]../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Verwenden des BLOB-Speichers mit C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Gewusst wie: Verwenden von Blob Storage mit PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Verwenden des Azure-Blob-Speichers mit Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Gewusst wie: Verwenden von Blob Storage mit Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Queue Storage

Die folgenden Tutorials zu Azure Queue Storage gelten für Azure Stack. Beachten Sie die im vorherigen Abschnitt [Beispiele](#examples) beschriebene bestimmte Endpunktsuffix-Voraussetzung für Azure Stack.

* [Erste Schritte mit Azure Queue Storage mit .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Gewusst wie: Verwenden von Queue Storage mit Java](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Gewusst wie: Verwenden von Queue Storage mit Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Verwenden des Warteschlangenspeichers mit C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Gewusst wie: Verwenden von Queue Storage mit PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Gewusst wie: Verwenden von Queue Storage mit Python](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Gewusst wie: Verwenden von Queue Storage mit Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)


## <a name="table-storage"></a>Table Storage

Die folgenden Tutorials zu Azure Table Storage gelten für Azure Stack. Beachten Sie die im vorherigen Abschnitt [Beispiele](#examples) beschriebene bestimmte Endpunktsuffix-Voraussetzung für Azure Stack.

* [Erste Schritte mit Azure Table Storage mit .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Gewusst wie: Verwenden von Table Storage mit Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Verwenden des Azure-Tabellenspeichers mit Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Verwenden des Tabellenspeichers mit C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Gewusst wie: Verwenden von Table Storage mit PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Verwenden von Table Storage in Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Gewusst wie: Verwenden von Table Storage mit Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Microsoft Azure Storage](../../storage/common/storage-introduction.md)

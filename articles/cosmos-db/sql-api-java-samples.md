---
title: 'Azure Cosmos DB: Java-Beispiele für die SQL-API'
description: Java-Beispiele für häufige Aufgaben mit der SQL-API von Azure Cosmos DB, einschließlich CRUD-Vorgängen, finden Sie auf GitHub.
keywords: NoSQL-Beispiel
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: sample
ms.date: 02/08/2018
ms.author: sngun
ms.openlocfilehash: 135613be26ed0ebc5dfd180112fabb0401f60027
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874174"
---
# <a name="azure-cosmos-db-java-examples-for-the-sql-api"></a>Azure Cosmos DB: Java-Beispiele für die SQL-API

> [!div class="op_single_selector"]
> * [.NET-Beispiele](sql-api-dotnet-samples.md)
> * [Java-Beispiele](sql-api-java-samples.md)
> * [Async Java-Beispiele](sql-api-async-java-samples.md)
> * [Node.js-Beispiele](sql-api-nodejs-samples.md)
> * [Python-Beispiele](sql-api-python-samples.md)
> * [Katalog mit Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Die neuesten Beispielanwendungen, in denen CRUD-Vorgänge und andere gängige Vorgänge für Azure Cosmos DB-Ressourcen ausgeführt werden, finden Sie im GitHub-Repository [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java). Dieser Artikel enthält Folgendes:

* Links zu den Aufgaben in den einzelnen Java-Beispielprojektdateien. 
* Links zum zugehörigen API-Referenzinhalt.

**Voraussetzungen**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Sie können Ihre [Vorteile für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Ihr Visual Studio-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste nutzen können.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Zum Ausführen dieser Beispielanwendung benötigen Sie Folgendes:

* Java Development Kit 7
* Microsoft Azure DocumentDB Java SDK

Optional können Sie Maven verwenden, um die neuesten Binärdateien des Microsoft Azure DocumentDB Java SDK zur Verwendung in Ihrem Projekt abzurufen. Maven fügt alle erforderlichen Abhängigkeiten automatisch hinzu. Andernfalls können Sie die in der Datei „pom.xml“ aufgelisteten Abhängigkeiten direkt herunterladen und Ihrem Buildpfad hinzufügen.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>LATEST</version>
</dependency>
```

**Ausführen der Beispielanwendungen**

Klonen des Beispielrepositorys:
```bash
$ git clone https://github.com/Azure/azure-documentdb-java.git

$ cd azure-documentdb-java
```

Sie können die Beispiele entweder mit Eclipse oder über die Befehlszeile mit Maven ausführen.

So führen Sie die Beispiele über Eclipse aus:
* Laden Sie die Datei „pom.xml“ des übergeordneten Hauptprojekts in Eclipse. Die documentdb-Beispiele sollten automatisch geladen werden.
* Zum Ausführen der Beispiele benötigen Sie einen gültigen Azure Cosmos DB-Endpunkt. Die Endpunkte werden aus `src/test/java/com/microsoft/azure/documentdb/examples/AccountCredentials.java` gelesen.
* Sie können Ihre Endpunktanmeldeinformationen als VM-Argumente in der Eclipse JUnit-Ausführungskonfiguration übergeben oder in „AccountCredentials.java“ einfügen.
    ```bash
    -DACCOUNT_HOST="https://REPLACE_THIS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS"
    ```
* Jetzt können Sie die Beispiele als JUnit-Tests in Eclipse ausführen.

So führen Sie die Beispiele über die Befehlszeile aus:
* Die zweite Möglichkeit zum Ausführen der Beispiele ist die Verwendung von Maven:
* Führen Sie Maven aus, und übergeben Sie Ihre Azure Cosmos DB-Endpunktanmeldeinformationen:
    ```bash
    mvn test -DACCOUNT_HOST="https://REPLACE_THIS_WITH_YOURS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS_WITH_YOURS"
    ```

   > [!NOTE]
   > Jedes Beispiel ist eigenständig mit eigener Einrichtung und Bereinigung. Die Beispiele richten mehrere Aufrufe an [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createcollection). Dabei wird Ihrem Abonnement jedes Mal 1 Stunde Nutzung gemäß der Leistungsstufe der erstellten Sammlung berechnet. 
   > 
   > 

## <a name="database-examples"></a>Datenbankbeispiele
Die Datei [DatabaseCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen und Lesen einer Datenbank](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L64-L79) | [DocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdatabase)<br>[DocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdatabase)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource.setid) |
| [Erstellen und Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L82-L93) | [DocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletedatabase) |
| [Erstellen und Abfragen einer Datenbank](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L96-L111) | [DocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydatabases) |

## <a name="collection-examples"></a>Sammlungsbeispiele
Die Datei [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung mit nur einer Partition](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L74-L84) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [Erstellen einer Sammlung mit mehreren Partitionen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L103-L155) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._partition_key_definition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._request_options) |
| [Löschen einer Sammlung](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L97-L99) | [DocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletecollection) |

## <a name="document-examples"></a>Dokumentbeispiele
Die Datei [DocumentCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen, Lesen und Löschen eines Dokuments](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L84-L122) | [DocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument)<br>[DocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdocument)<br>[DocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) |
| [Erstellen eines Dokuments mit einer programmierbaren Dokumentdefinition](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L126-L147) | [Dokument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource.setid) |

## <a name="indexing-examples"></a>Indizierungsbeispiele
Die Datei [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen eines Index und Festlegen der Indizierungsrichtlinie](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L125-L141) | [Index](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy) |

Weitere Informationen zur Indizierung finden Sie unter [Indizierungsrichtlinien für Azure Cosmos DB](index-policy.md).

## <a name="query-examples"></a>Abfragebeispiele
Die Datei [DocumentQuerySamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Ausführen einer einfachen partitionsübergreifenden Dokumentabfrage](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L108-L129) | [DocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments)<br>[FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setenablecrosspartitionquery) |
| [Sortieren nach Abfrage](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L132-L154) | [FeedResponse<T>.getQueryIterator](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_response.getqueryiterator) |

Weitere Informationen zum Schreiben von Abfragen finden Sie unter [SQL-Abfragen für Azure Cosmos DB-DocumentDB-API](how-to-sql-query.md).

## <a name="offer-examples"></a>Angebotsbeispiele
Die Datei [OfferCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung und Festlegen des Durchsatzes](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L76-L102) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createcollection)<br>[RequestOptions.setOfferThroughput ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._request_options.setofferthroughput) |
| [Lesen einer Sammlung, um das zugehörige Angebot zu finden](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L108-L132) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._offer.getcontent)<br>[DocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer)<br>[DocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readcollection)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.queryoffers) |

## <a name="partition-key-examples"></a>Partitionsschlüsselbeispiele
Die Datei [SinglePartitionCollectionDocumentCrudSample](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung mit nur einer Partition](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L164-L207) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [Ändern des Durchsatzangebots für eine Sammlung mit nur einer Partition](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L209-L223) | [DocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer) |

## <a name="stored-procedure-examples"></a>Beispiele für gespeicherte Prozeduren
Die Datei [StoredProcedureSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer gespeicherten Prozedur](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L85-L118) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._stored_procedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createstoredprocedure) |
| [Ausführen einer gespeicherten Prozedur mit Argumenten](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L121-L144) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |
| [Ausführen einer gespeicherten Prozedur mit einem Objektargument](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L147-L177) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |

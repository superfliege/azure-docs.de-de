---
title: Erstellen einer Java-App mit dem Async Java SDK zum Verwalten von Daten der Azure Cosmos DB-SQL-API | Microsoft-Dokumentation
description: In diesem Tutorial wird veranschaulicht, wie Sie SQL-API-Konten für Azure Cosmos DB verwenden, um Daten zu speichern und darauf zuzugreifen, indem Sie eine Async Java-Anwendung nutzen.
keywords: NoSQL-Tutorial, Onlinedatenbank, Java-Konsolenanwendung
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: sngun
ms.openlocfilehash: 0c458892c56634c97eec92a9439059c087a32144
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52165474"
---
# <a name="tutorial-build-a-java-app-with-async-java-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Erstellen einer Java-App mit dem Async Java SDK zum Verwalten von Daten der Azure Cosmos DB-SQL-API

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

In diesem Tutorial erfahren Sie, wie Sie mit dem Async Java SDK eine Java-Anwendung erstellen, um Daten der Azure Cosmos DB-SQL-API zu speichern und auf sie zuzugreifen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos DB-Kontos und Herstellen der Verbindung
> * Konfigurieren Ihrer Lösung
> * Erstellen einer Sammlung
> * Erstellen von JSON-Dokumenten
> * Abfragen der Sammlung

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über die folgenden Ressourcen verfügen:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Development Kit (JDK) 8 oder höher](https://aka.ms/azure-jdks).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Kontos
Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie mit [Klonen des GitHub-Projekts](#GitClone) fortfahren. Falls Sie den Azure Cosmos DB-Emulator verwenden, können Sie die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators ausführen und dann mit [Klonen des GitHub-Projekts](#GitClone) fortfahren.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Schritt 2: Klonen des GitHub-Repositorys

Sie können beginnen, indem Sie das GitHub-Repository für [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) (Erste Schritte mit Azure Cosmos DB und Java) klonen. Führen Sie in einem lokalen Verzeichnis beispielsweise Folgendes aus, um das Beispielprojekt lokal abzurufen:

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started
```

Das Verzeichnis enthält die Datei `pom.xml` und den Ordner `src/main/java/com/microsoft/azure/cosmosdb/sample` mit Java-Quellcode (einschließlich `Main.java`). Das Projekt enthält Code zum Ausführen von Vorgängen mit Azure Cosmos DB. Hierzu zählen beispielsweise das Erstellen von Dokumenten sowie das Abfragen von Daten in einer Auflistung. Die Datei `pom.xml` enthält eine Abhängigkeit im [Azure Cosmos DB Java SDK in Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>Schritt 3: Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto

Kehren Sie als Nächstes zum [Azure-Portal](https://portal.azure.com) zurück, um Ihren Endpunkt und den primären Hauptschlüssel abzurufen. Der Endpunkt und der Primärschlüssel von Azure Cosmos DB sind erforderlich, damit Ihre Anwendung weiß, womit die Verbindung hergestellt werden soll, und damit Azure Cosmos DB weiß, dass die Verbindung Ihrer Anwendung vertrauenswürdig ist. Die Datei `AccountSettings.java` enthält den Primärschlüssel und URI-Werte. 

Navigieren Sie im Azure-Portal zu Ihrem Azure Cosmos DB-Konto, und klicken Sie auf **Schlüssel**. Kopieren Sie den URI und den PRIMÄRSCHLÜSSEL aus dem Portal, und fügen Sie diese Angaben in die Datei `AccountSettings.java` ein. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos DB account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos DB account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Screenshot: Abrufen von Schlüsseln über das Portal][keys]

## <a name="step-4-initialize-the-client-object"></a>Schritt 4: Initialisieren des Clientobjekts

Initialisieren Sie das Clientobjekt, indem Sie die in der Datei „AccountSettings.java“ definierten Werte für Host-URI und Primärschlüssel verwenden.

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>Schritt 5: Erstellen einer Datenbank

Ihre Azure Cosmos DB-[Datenbank](databases-containers-items.md#azure-cosmos-databases) kann mithilfe der createDatabaseIfNotExists()-Methode der DocumentClient-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für JSON-Dokumentspeicher, der auf Sammlungen aufgeteilt ist.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a id="CreateColl"></a>Schritt 6: Erstellen einer Sammlung

> [!WARNING]
> Mit **createCollection** wird eine neue Sammlung mit reserviertem Durchsatz erstellt. Dies wirkt sich auf die Kosten aus. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 
Eine Sammlung kann mithilfe der createDocumentCollectionIfNotExists()-Methode der DocumentClient-Klasse erstellt werden. Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik.

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a id="CreateDoc"></a>Schritt 7: Erstellen von JSON-Dokumenten

Ein Dokument kann mithilfe der createDocument-Methode der DocumentClient-Klasse erstellt werden. Dokumente sind benutzerdefinierter (beliebiger) JSON-Inhalt. Wir können jetzt eines oder mehrere Dokumente einfügen. In der Datei „src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java“ ist die Familie mit den JSON-Dokumenten definiert. 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a id="Query"></a>Schritt 8: Abfragen von Azure Cosmos DB-Ressourcen

Azure Cosmos DB unterstützt umfassende [Abfragen](how-to-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente. Der folgende Beispielcode veranschaulicht, wie Sie Dokumente in Azure Cosmos DB abfragen, indem Sie SQL-Syntax mit der [queryDocuments](/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments)-Methode verwenden.

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a id="Run"></a>Schritt 9: Ausführen Ihrer Java-Konsolenanwendung

Zum Ausführen der Anwendung in der Konsole navigieren Sie zum Projektordner, und kompilieren Sie mit Maven:

```bash
mvn package
```

Wenn Sie `mvn package` ausführen, wird die aktuelle Azure Cosmos DB-Bibliothek aus Maven heruntergeladen, und die Datei `GetStarted-0.0.1-SNAPSHOT.jar` wird erzeugt. Führen Sie die App dann wie folgt aus:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```

Glückwunsch! Sie haben dieses NoSQL-Tutorial abgeschlossen und verfügen über eine funktionierende Java-Konsolenanwendung!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine Java-App mit dem Async Java SDK zum Verwalten von Daten der Azure Cosmos DB-SQL-API erstellen. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Node.js-Tutorial: Erstellen einer Node.js-Konsolenanwendung mit dem JavaScript SDK zum Verwalten der SQL-API-Daten für Azure Cosmos DB](sql-api-nodejs-get-started.md)

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png

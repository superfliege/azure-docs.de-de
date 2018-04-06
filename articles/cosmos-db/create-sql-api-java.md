---
title: Erstellen einer Azure Cosmos DB-Dokumentdatenbank mit Java | Microsoft-Dokumentation
description: Hier finden Sie ein Java-Codebeispiel, mit dem Sie eine Verbindung mit der SQL-API von Azure Cosmos DB herstellen und diese API abfragen können.
services: cosmos-db
documentationcenter: ''
author: mimig1
manager: jhubbard
editor: ''
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 03/26/2018
ms.author: mimig
ms.openlocfilehash: 99c4c15893d1318be13781558fb7ed35203bd971
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Erstellen einer Dokumentdatenbank mit Java und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Mit Azure Cosmos DB können Sie verwaltete Dokument-, Tabellen- und Diagrammdatenbanken erstellen und abfragen.

In dieser Schnellstartanleitung wird mithilfe der Tools des Azure-Portals für die [SQL-API](sql-api-introduction.md) von Azure Cosmos DB eine Dokumentdatenbank erstellt. Darüber hinaus erfahren Sie hier, wie Sie unter Verwendung der [SQL-Java-API](sql-api-sdk-java.md) schnell eine Java-Konsolen-App erstellen. Die Anweisungen in dieser Schnellstartanleitung gelten für alle Betriebssysteme, unter denen Java ausgeführt werden kann. Nach Abschluss dieser Schnellstartanleitung können Sie Dokumentdatenbankressourcen sowohl über die Benutzeroberfläche als auch programmgesteuert erstellen und ändern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Außerdem haben Sie folgende Möglichkeiten: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Führen Sie unter Ubuntu `apt-get install default-jdk` aus, um das JDK zu installieren.
    * Achten Sie darauf, dass die Umgebungsvariable „JAVA_HOME“ auf den Ordner verweist, in dem das JDK installiert ist.
* Ein binäres [Maven](http://maven.apache.org/)-Archiv ([Download](http://maven.apache.org/download.cgi)/[Installationsanleitung](http://maven.apache.org/install.html))
    * Unter Ubuntu können Sie `apt-get install maven` ausführen, um Maven zu installieren.
* [Git-Client](https://www.git-scm.com/)
    * Unter Ubuntu können Sie `sudo apt-get install git` ausführen, um Git zu installieren.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie ein SQL-API-Konto mit Azure Cosmos DB erstellen.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

Sie können nun mithilfe des Daten-Explorers Daten zu einer neuen Sammlung hinzufügen.

1. Erweitern Sie die Sammlung **Elemente**, und klicken Sie auf **Dokumente** > **Neues Dokument**.

   ![Neue Dokumente im Daten-Explorer im Azure-Portal erstellen](./media/create-sql-api-java/azure-cosmosdb-data-explorer-new-document.png)
  
2. Fügen Sie nun der Sammlung ein Dokument mit folgender Struktur hinzu, und klicken Sie dann auf **Speichern**. Verwenden Sie die Schaltfläche **Kopieren** im Codefeld, um den JSON-Code in Ihre Zwischenablage zu kopieren.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

    ![Kopieren Sie JSON-Daten, und klicken Sie im Azure-Portal im Daten-Explorer auf „Speichern“.](./media/create-sql-api-java/azure-cosmosdb-data-explorer-save-document.png)

3.  Erstellen und speichern Sie ein weiteres Dokument, in dem Sie `id` in 2 und die anderen Eigenschaften nach Bedarf ändern. Ihre neuen Dokumente können jede gewünschte Struktur aufweisen, da Azure Cosmos DB kein Schema für Ihre Daten vorgibt.

## <a name="query-your-data"></a>Abfragen Ihrer Daten

Sie können jetzt zum Abrufen und Filtern Ihrer Daten Abfragen im Daten-Explorer verwenden.

1. Beachten Sie, dass die Abfrage standardmäßig auf `SELECT * FROM c` festgelegt ist. Dieser Standardabfrage ruft alle Dokumente in der Sammlung ab und zeigt sie an. 

    ![Die Standardabfrage im Daten-Explorer ist „SELECT * FROM c“.](./media/create-sql-api-java/azure-cosmosdb-data-explorer-query.png)

2. Bleiben Sie auf der Registerkarte **Dokumente**, und ändern Sie die Abfrage, indem Sie auf die Schaltfläche **Filter bearbeiten** klicken, `ORDER BY c._ts DESC` dem Abfrageprädikatfeld hinzufügen und anschließend auf **Filter anwenden** klicken.

    ![Ändern Sie die Standardabfrage, indem Sie ORDER BY c._ts DESC hinzufügen und auf „Filter anwenden“ klicken.](./media/create-sql-api-java/azure-cosmosdb-data-explorer-edit-query.png)

Mithilfe dieser geänderten Abfrage werden die Dokumente auf Basis ihrer Zeitstempel in absteigender Reihenfolge sortiert, damit Ihr zweites Dokument zuerst aufgeführt wird. Wenn Sie mit der SQL-Syntax vertraut sind, können Sie eine der unterstützten [SQL-Abfragen](sql-api-sql-query.md) in dieses Feld eingeben. 

Damit ist unsere Arbeit im Daten-Explorer abgeschlossen. Bevor wir mit der Arbeit mit Code fortfahren, beachten Sie, dass der Daten-Explorer auch zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern verwendet werden kann, um sowohl serverseitige Geschäftslogik als auch einen Skalierungsdurchsatz auszuführen. Der Daten-Explorer stellt den gesamten integrierten programmgesteuerten Datenzugriff in den APIs zur Verfügung, bietet jedoch einfachen Zugriff auf Ihre Daten im Azure-Portal.

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Beginnen wir nun mit der Verwendung von Code. Klonen Sie zunächst eine SQL-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung, erstellen Sie einen neuen Ordner namens „git-samples“, und schließen Sie die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z. B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren. 

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

Die folgenden Codeausschnitte stammen alle aus der Datei „C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted\Program.java“.

* `DocumentClient`-Initialisierung [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) bietet die clientseitige logische Darstellung für den Azure Cosmos DB-Datenbankdienst. Mit diesem Client werden Anforderungen für den Dienst konfiguriert und ausgeführt. Die `FILLME`-Teile dieses Codes werden später in der Schnellstartanleitung aktualisiert.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Erstellung der [Datenbank](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._database)

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Erstellung von [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection)

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Dokumenterstellung mithilfe der [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument)-Methode

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* SQL-Abfragen über JSON erfolgen mithilfe der [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments)-Methode.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) auf **Schlüssel**. 

    Verwenden Sie die zum Kopieren vorgesehenen Schaltflächen auf der rechten Seite des Bildschirms, um den oberen Wert, den URI, zu kopieren.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf der Seite „Schlüssel“](./media/create-sql-api-java/keys.png)

2. Öffnen Sie die `Program.java`-Datei im Ordner „C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted“. 

3. Fügen Sie den URI-Wert aus dem Portal über `https://FILLME.documents.azure.com` in Zeile 45 ein.

4. Wechseln Sie zurück zum Portal, und kopieren Sie den PRIMÄRSCHLÜSSEL-Wert, wie im Screenshot dargestellt. Fügen Sie den PRIMÄRSCHLÜSSEL-Wert aus dem Portal über `FILLME` in Zeile 46 ein.

    Die getStartedDemo-Methode sollte jetzt wie folgt aussehen: 
    
    ```java
    private void getStartedDemo() throws DocumentClientException, IOException {
        this.client = new DocumentClient("https://youraccountname.documents.azure.com:443/",
                "your-primary-key...RJhQrqQ5QQ==", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);
    ```

5. Speichern Sie die Datei „Program.java“.

## <a name="run-the-app"></a>Ausführen der App

1. Wechseln Sie im Terminalfenster von Git mithilfe von `cd` zum Ordner „azure-cosmos-db-documentdb-java-getting-started“.

    ```git
    cd "C:\git-samples\azure-cosmos-db-documentdb-java-getting-started"
    ```

2. Verwenden Sie im Git-Terminalfenster den folgenden Befehl, um die erforderlichen Java-Pakete zu installieren.

    ```
    mvn package
    ```

3. Verwenden Sie im Git-Terminalfenster den folgenden Befehl, um die Java-Anwendung zu starten.

    ```
    mvn exec:java -D exec.mainClass=GetStarted.Program
    ```

    Im Terminalfenster wird eine Benachrichtigung angezeigt, dass die FamilyDB-Datenbank erstellt wurde. 
    
4. Drücken Sie eine Taste, um die Datenbank zu erstellen, und dann ein weitere Taste, um die Sammlung zu erstellen. 

    Am Ende des Programms werden alle Ressourcen gelöscht. Wechseln Sie in Ihrem Browser deshalb zurück zum Daten-Explorer, um zu überprüfen, ob er jetzt eine FamilyDB-Datenbank und eine FamilyCollection-Sammlung enthält.

5. Wechseln Sie in das Konsolenfenster, und drücken Sie eine Taste, um das erste Dokument zu erstellen, und dann eine weitere Taste, um das zweite Dokument zu erstellen. Wechseln Sie anschließend wieder zurück in den Daten-Explorer, um die Dokumente anzuzeigen. 

6. Drücken Sie eine Taste, um eine Abfrage auszuführen und die Ausgabe im Konsolenfenster anzuzeigen. 

7. Mit der nächsten Taste, die Sie drücken, werden die Ressourcen gelöscht. Wenn Sie die Ressourcen beibehalten möchten, können Sie im Konsolenfenster STRG+C drücken, um das Programm zu beenden. Drücken Sie andernfalls eine beliebige Taste, um die Ressourcen aus Ihrem Konto zu löschen, damit keine Gebühren anfallen. 

    ![Konsolenausgabe](./media/create-sql-api-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mit dem Daten-Explorer ein Azure Cosmos DB-Konto, eine Dokumentdatenbank und eine Sammlung erstellen und wie Sie das gleiche Ergebnis programmgesteuert mithilfe einer eine App erreichen. Sie können jetzt zusätzliche Daten in Ihre Azure Cosmos DB-Sammlung importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)



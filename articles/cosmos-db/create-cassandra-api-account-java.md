---
title: Erstellen eines Azure Cosmos DB-Cassandra-API-Kontos, einer Datenbank und einer Tabelle mithilfe einer Java-Anwendung
description: In diesem Artikel wird gezeigt, wie Sie ein Cassandra-API-Konto, eine Datenbank (auch als „Keyspace“ bezeichnet) und eine Tabelle für dieses Konto mithilfe einer Java-Anwendung erstellen können.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: b5e3d87e026b65a602b7bdf2e52365d13b21f62f
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166830"
---
# <a name="create-an-azure-cosmos-db-cassandra-api-account-database-and-a-table-by-using-a-java-application"></a>Erstellen eines Azure Cosmos DB-Cassandra-API-Kontos, einer Datenbank und einer Tabelle mithilfe einer Java-Anwendung

In diesem Tutorial wird beschrieben, wie mithilfe einer Java-Anwendung ein Cassandra-API-Konto in Azure Cosmos DB erstellt und eine Datenbank (auch als „Keyspace“ bezeichnet) sowie eine Tabelle hinzugefügt wird. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver), um eine Benutzerdatenbank zu erstellen, die Details wie Benutzer-ID, Benutzernamen und Stadt des Benutzers enthält.  

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Cassandra-Datenbankkontos
> * Abrufen der Kontoverbindungszeichenfolge
> * Erstellen eines Maven-Projekts und der Abhängigkeiten
> * Hinzufügen einer Datenbank und Tabelle
> * Ausführen der App

## <a name="prerequisites"></a>Voraussetzungen 

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein  [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  erstellen, bevor Sie beginnen. Alternativ können Sie  [Azure Cosmos DB ohne Azure-Abonnement testen](https://azure.microsoft.com/try/cosmosdb/)  – kostenlos und unverbindlich. 

* Beziehen Sie die aktuelle Version des [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). 

* Führen Sie den [Download](http://maven.apache.org/download.cgi) und die [Installation](http://maven.apache.org/install.html) des binären [Maven](http://maven.apache.org/)-Archivs durch. 
  - Unter Ubuntu können Sie  `apt-get install maven`  ausführen, um Maven zu installieren. 

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos 

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com/) an. 

2. Klicken Sie auf  **Ressource erstellen** > **Datenbanken** > **Azure Cosmos DB**. 

3. Geben Sie im Bereich  **Neues Konto**  die Einstellungen für das neue Azure Cosmos DB-Konto ein. 

   |Einstellung   |Empfohlener Wert  |BESCHREIBUNG  |
   |---------|---------|---------|
   |ID   |   Geben Sie einen eindeutigen Namen ein.    | Geben Sie einen eindeutigen Namen ein, der das Azure Cosmos DB-Konto identifiziert. <br/><br/>Da cassandra.cosmosdb.azure.com an die ID angefügt wird, die Sie bereitstellen, um Ihren Kontaktpunkt zu erstellen, sollten Sie eine eindeutige, aber identifizierbare ID verwenden.         |
   |API    |  Cassandra   |  Die API bestimmt den Typ des zu erstellenden Kontos. <br/> Wählen Sie **Cassandra** aus, da Sie in diesem Artikel eine Datenbank mit breiten Spalten erstellen werden, die mit der CQL-Syntax abgefragt werden kann.  |
   |Abonnement    |  Ihr Abonnement        |  Wählen Sie das Azure-Abonnement, das Sie für dieses Azure Cosmos DB-Konto verwenden möchten, aus.        |
   |Ressourcengruppe   | Geben Sie einen Namen ein.    |  Wählen Sie  **Neu erstellen** aus, und geben Sie dann einen neuen Ressourcengruppennamen für Ihr Konto ein. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden.    |
   |Standort    |  Wählen Sie die Region aus, die Ihren Benutzern am nächsten liegt.    |  Wählen Sie den geografischen Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll, aus. Verwenden Sie einen Standort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.    |

   ![Erstellen eines Kontos mit dem Portal](./media/create-cassandra-api-account-java/create-account.png)

4. Klicken Sie als Nächstes auf  **Erstellen**. <br/>Die Kontoerstellung dauert einige Minuten. Nachdem die Ressource erstellt wurde, können Sie in der rechten Ecke des Portals die Benachrichtigung **Bereitstellung erfolgreich** sehen.

## <a name="get-the-connection-details-of-your-account"></a>Abrufen der Verbindungsdetails zu Ihrem Konto  

Rufen Sie die Informationen der Verbindungszeichenfolge über das Azure-Portal ab, und fügen Sie sie in die Java-Konfigurationsdatei ein. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren. 

1. Navigieren Sie über das  [Azure-Portal](http://portal.azure.com/) zu Ihrem Cosmos DB-Konto. 

2. Öffnen Sie den Bereich  **Verbindungszeichenfolge**.  

3. Kopieren Sie die Werte von **KONTAKTPUNKT**, **PORT**, **BENUTZERNAME**, und **PRIMÄRES KENNWORT**. Diese werden Sie in den nächsten Schritten benötigen.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Erstellen eines Maven-Projekts, von Abhängigkeiten und Hilfsprogrammklassen 

Das Java-Beispielprojekt, das Sie in diesem Artikel verwenden, wird in GitHub gehostet. Sie können es über das Repository [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started) herunterladen. 

Aktualisieren Sie nach dem Download der Dateien, die Informationen zur Verbindungszeichenfolge in der Datei `java-examples\src\main\resources\config.properties`, und führen Sie sie aus.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_port = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Alternativ können Sie das Beispiel auch neu erstellen.  

1. Erstellen Sie über das Terminal oder die Eingabeaufforderung ein neues Maven-Projekt namens „Cassandra-demo“. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Suchen Sie den Ordner `cassandra-demo`. Öffnen Sie in einem Text-Editor die generierte Datei `pom.xml`. 

   Fügen Sie die Cassandra-Abhängigkeiten hinzu, und erstellen Sie die für Ihr Projekt erforderlichen Plug-Ins, die in der Datei [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml) gezeigt werden.  

3. Erstellen Sie im Ordner `cassandra-demo\src\main` einen neuen Ordner namens `resources`.  Fügen Sie im Ressourcenordner die Dateien „config.properties“ und „log4j.properties“ hinzu:

   - Die Datei [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) speichert den Verbindungsendpunkt und die Schlüsselwerte der Azure Cosmos DB-Cassandra-API. 
   
   - Die Datei [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) definiert die Ebene der Protokollierung, die für die Interaktion mit der Cassandra-API erforderlich ist.  

4. Navigieren Sie nun zum Ordner `src/main/java/com/azure/cosmosdb/cassandra/`. Erstellen Sie im Cassandra-Ordner einen weiteren Ordner mit dem Namen `utils`. Der neue Ordner speichert die Hilfsprogrammklassen, die zum Herstellen einer Verbindung mit der Cassandra-API-Konto erforderlich sind. 

   Fügen Sie die Klasse [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) hinzu, um den Cluster zu erstellen und Cassandra-Sitzungen zu öffnen und zu schließen. Der Cluster stellt eine Verbindung mit der Cassandra-API von Azure Cosmos DB her und gibt eine Sitzung für den Zugriff zurück. Lesen Sie mithilfe der Klasse [Configurations](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) Informationen zur Verbindungszeichenfolge aus der Datei „config.properties“. 

5. Das Java-Beispiel erstellt eine Datenbank mit Benutzerinformationen wie Benutzernamen, Benutzer-ID und Stadt des Benutzers. Sie müssen GET- und SET-Methoden definieren, um auf die Benutzerdetails in der main-Funktion zuzugreifen.
 
   Erstellen Sie eine [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java)-Klasse im Ordner `src/main/java/com/azure/cosmosdb/cassandra/` mit GET- und SET-Methoden. 

## <a name="add-a-database-and-a-table"></a>Hinzufügen einer Datenbank und Tabelle  

In diesem Abschnitt wird beschrieben, wie Sie eine Datenbank (Keyspace) und eine Tabelle mit Cassandra Query Language (CQL) hinzufügen. Weitere Informationen zur CQL-Syntax für diese Befehle finden Sie in den Artikeln zum [Erstellen eines Keyspace](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) und [Erstellen einer Tabelle](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable) für die Abfragesyntax. 

1. Erstellen Sie im Ordner `src\main\java\com\azure\cosmosdb\cassandra` einen neuen Ordner namens `repository`. 

2. Erstellen Sie als Nächstes die Java-Klasse `UserRepository`, und fügen Sie den folgenden Code hinzu: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Suchen Sie den Ordner `src\main\java\com\azure\cosmosdb\cassandra`, und erstellen Sie einen neuen Unterordner namens `examples`.

4. Erstellen Sie als Nächstes die Java-Klasse `UserProfile`. Diese Klasse enthält die main-Methode, die die Methoden createKeyspace und createTable aufruft, die Sie zuvor definiert haben: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Ausführen der App 

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster. Fügen Sie folgenden Codeblock ein. 

   Dieser Code ändert das Verzeichnis (cd) in den Ordnerpfad, in dem das Projekt erstellt wurde. Anschließend führt er den Befehl `mvn clean install` zum Generieren der Datei `cosmosdb-cassandra-examples.jar` im Zielordner aus. Abschließend führt er die Java-Anwendung aus.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   Im Terminalfenster werden Benachrichtigungen angezeigt, dass der Keyspace und die Tabelle erstellt wurden. 
   
2. Öffnen Sie im Azure-Portal nun den **Daten-Explorer**, um sicherzustellen, dass der Keyspace und die Tabelle erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie ein Azure Cosmos DB-Cassandra-API-Konto, eine Datenbank und eine Tabelle mithilfe einer Java-Anwendung erstellen. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Beispieldaten in die Cassandra-API-Tabelle laden](cassandra-api-load-data.md)

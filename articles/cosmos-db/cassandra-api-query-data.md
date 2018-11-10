---
title: Abfragen von Daten von einem Azure Cosmos DB-Cassandra-API-Konto
description: In diesem Artikel wird das Abfragen von Benutzerdaten von einem Cassandra-API-Konto von Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt.
services: cosmos-db
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: a06e7e6159953bfeffa966759d29b91bbcbafd37
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739208"
---
# <a name="tutorial-query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Tutorial: Abfragen von Daten von einem Azure Cosmos DB-Cassandra-API-Konto

In diesem Tutorial wird das Abfragen von Benutzerdaten von einem Cassandra-API-Konto von Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver) und fragt Benutzerdaten wie z.B. Benutzer-ID, Benutzername und Stadt des Benutzers ab. 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Abfragen von Daten aus einer Cassandra-Tabelle
> * Ausführen der App

## <a name="prerequisites"></a>Voraussetzungen

* Dieser Artikel gehört zu einem mehrteiligen Tutorial. Bevor Sie beginnen, führen Sie die vorherigen Schritte zum Erstellen des Cassandra-API-Kontos, Keyspace oder der Tabelle und zum [Laden von Beispieldaten in die Tabelle](cassandra-api-load-data.md) aus. 

## <a name="query-data"></a>Abfragen von Daten

Führen Sie zum Abfragen von Daten aus Ihrem Cassandra-API-Konto die folgenden Schritte aus:

1. Öffnen Sie die Datei `UserRepository.java` unter dem Ordner `src\main\java\com\azure\cosmosdb\cassandra`. Fügen Sie folgenden Codeblock an. Dieser Code bietet drei Methoden: 

   * Abfragen aller Benutzer in der Datenbank
   * Abfragen eines bestimmten Benutzers, gefiltert nach Benutzer-ID
   * Löschen einer Tabelle

   ```java
   /**
   * Select all rows from user table
   */
   public void selectAllUsers() {

     final String query = "SELECT * FROM uprofile.user";
     List<Row> rows = session.execute(query).all();

     for (Row row : rows) {
        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
     }
   }

   /**
   * Select a row from user table
   *
   * @param id user_id
   */
   public void selectUser(int id) {
      final String query = "SELECT * FROM uprofile.user where user_id = 3";
      Row row = session.execute(query).one();

      LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
   }

   /**
   * Delete user table.
   */
   public void deleteTable() {
     final String query = "DROP TABLE IF EXISTS uprofile.user";
     session.execute(query);
   }
   ```

2. Öffnen Sie die Datei `UserProfile.java` unter dem Ordner `src\main\java\com\azure\cosmosdb\cassandra`. Diese Klasse enthält die main-Methode, die die Methoden zum Einfügen von Daten createKeyspace und createTable aufruft, die Sie zuvor definiert haben. Fügen Sie nun den folgenden Code an, der alle Benutzer oder einen bestimmten Benutzer abfragt:

   ```java
   LOGGER.info("Select all users");
   repository.selectAllUsers();

   LOGGER.info("Select a user by id (3)");
   repository.selectUser(3);

   LOGGER.info("Delete the users profile table");
   repository.deleteTable();
   ```

## <a name="run-the-java-app"></a>Ausführen der Java-App
1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster. Fügen Sie folgenden Codeblock ein. 

   Dieser Code ändert das Verzeichnis (cd) in den Ordnerpfad, in dem das Projekt erstellt wurde. Anschließend führt er den Befehl `mvn clean install` zum Generieren der Datei `cosmosdb-cassandra-examples.jar` im Zielordner aus. Abschließend führt er die Java-Anwendung aus.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Öffnen Sie nun im Azure-Portal den **Daten-Explorer**, und vergewissern Sie sich, dass die Benutzertabelle gelöscht ist.

## <a name="next-steps"></a>Nächste Schritte

* In diesem Tutorial haben Sie gelernt, Ihre Daten aus einem Cassandra-API-Konto von Azure Cosmos DB abzufragen. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Daten zum Cassandra-API-Konto migrieren](cassandra-import-data.md)



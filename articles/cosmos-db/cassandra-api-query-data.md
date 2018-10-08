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
ms.openlocfilehash: c1fb4c27f897e3c0952ed6419e167613ac8204f7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223490"
---
# <a name="query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Abfragen von Daten von einem Azure Cosmos DB-Cassandra-API-Konto

In diesem Tutorial wird das Abfragen von Benutzerdaten von einem Cassandra-API-Konto von Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver) und fragt Benutzerdaten wie z.B. Benutzer-ID, Benutzername und Stadt des Benutzers ab. 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Abfragen von Daten aus einer Cassandra-Tabelle
> * Ausführen der App

## <a name="prerequisites"></a>Voraussetzungen

* Dieser Artikel gehört zu einem mehrteiligen Tutorial. Bevor Sie beginnen, führen Sie die vorherigen Schritte zum [Erstellen eines Cassandra-API-Kontos, Keyspace oder einer Tabelle mithilfe einer Java-Anwendung](create-cassandra-api-account-java.md) und zum [Laden von Beispieldaten in die Tabelle](cassandra-api-load-data.md) durch. 

## <a name="query-data"></a>Abfragen von Daten

Öffnen Sie die Datei `UserRepository.java` unter dem Ordner `src\main\java\com\azure\cosmosdb\cassandra`. Fügen Sie folgenden Codeblock an. Dieser Code bietet drei Funktionen: Abfragen aller Benutzer in der Datenbank, Abfragen eines bestimmten nach Benutzer-ID gefilterten Benutzers und Löschen einer Tabelle. 

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

Öffnen Sie die Datei `UserProfile.java` unter dem Ordner `src\main\java\com\azure\cosmosdb\cassandra`. Diese Klasse enthält die main-Methode, die die Methoden zum Einfügen von Daten createKeyspace und createTable aufruft, die Sie zuvor definiert haben. Fügen Sie nun den folgenden Code an, der alle Benutzer oder einen bestimmten Benutzer abfragt:

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



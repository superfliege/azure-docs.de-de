---
title: Laden von Beispieldaten in eine Cassandra-API-Tabelle von Azure Cosmos DB mit einer Java-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel wird das Laden von Beispielbenutzerdaten in eine Tabelle im Cassandra-API-Konto von Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 662d4b8812ca4b92c1130b9c2c38771e7ec30a06
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393993"
---
# <a name="load-sample-data-into-an-azure-cosmos-db-cassandra-api-table"></a>Laden von Beispieldaten in eine Cassandra-API-Tabelle von Azure Cosmos DB

In diesem Tutorial wird das Laden von Beispielbenutzerdaten in eine Tabelle im Cassandra-API-Konto von Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver) und lädt Benutzerdaten wie z.B. Benutzer-ID, Benutzername, Benutzerort. 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Laden von Daten in die Cassandra-Tabelle
> * Ausführen der App

## <a name="prerequisites"></a>Voraussetzungen

* Dieser Artikel gehört zu einem mehrteiligen Tutorial. Bevor Sie mit diesem Dokument beginnen, stellen Sie sicher, dass Sie [Cassandra-API-Konto, Keyspace und Tabelle erstellen](create-cassandra-api-account-java.md).   

## <a name="load-data-into-the-table"></a>Laden von Daten in die Tabelle

Öffnen Sie die Datei „UserRepository.java“ im Ordner „src\main\java\com\azure\cosmosdb\cassandra“, und fügen Sie den Code zum Einfügen der Felder „user_id“, „user_name“ und „user_bcity“ in die Tabelle hinzu:

```java
/**
* Insert a row into user table
*
* @param id   user_id
* @param name user_name
* @param city user_bcity
*/
public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
}

/**
* Create a PrepareStatement to insert a row to user table
*
* @return PreparedStatement
*/
public PreparedStatement prepareInsertStatement() {
    final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
    return session.prepare(insertStatement);
}
```
 
Öffnen Sie die Datei „UserProfile.java“ im Ordner „src\main\java\com\azure\cosmosdb\cassandra“. Diese Klasse enthält die main-Methode, die die Methoden createKeyspace und createTable aufruft, die Sie zuvor definiert haben. Fügen Sie nun den folgenden Code zum Einfügen einiger Beispieldaten in die Cassandra-API-Tabelle hinzu.

```java
//Insert rows into user table
PreparedStatement preparedStatement = repository.prepareInsertStatement();
    repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
    repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
    repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
    repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
    repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
```

## <a name="run-the-app"></a>Ausführen der App

Öffnen Sie die Eingabeaufforderung oder ein Terminalfenster, und ändern Sie den Ordnerpfad gemäß des Ordners, in dem Sie das Projekt erstellt haben. Führen Sie den Befehl „mvn clean install“ zum Generieren der Datei „cosmosdb-cassandra-examples.jar“ im Zielordner aus, und führen Sie die Anwendung aus. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Sie können jetzt den Daten-Explorer im Azure-Portal öffnen, um zu bestätigen, dass die Benutzerinformationen der Tabelle hinzugefügt wurden.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, Ihre Beispieldaten in das Cassandra-API-Konto von Azure Cosmos DB zu laden. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Abfragen von Daten aus dem Cassandra-API-Konto](cassandra-api-query-data.md)

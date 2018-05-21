---
title: Abfragen von Azure SQL-Datenbank mithilfe von Java | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Java ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
author: ajlam
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: java
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: andrela
ms.openlocfilehash: e19006d177777b18e1665ec5039f9c28f7e54df5
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="use-java-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Java

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) eine Verbindung mit einer Azure SQL-Datenbank herstellen und anschließend Daten mithilfe von Transact-SQL-Anweisungen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für diese Schnellstartanleitung verwenden

- Java und die zugehörige Software müssen für das Betriebssystem installiert sein:

    - **macOS:** Installieren Sie Homebrew und Java und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).
    - **Ubuntu:** Installieren Sie das Java Development Kit und Maven. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3 und 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).
    - **Windows:** Installieren Sie das Java Development Kit und Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-maven-project-and-dependencies"></a>**Erstellen von Maven-Projekt und Abhängigkeiten**
1. Erstellen Sie über das Terminal ein neues Maven-Projekt namens **sqltest**. 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. Geben Sie **Y** ein, wenn Sie dazu aufgefordert werden.
3. Wechseln Sie zum Verzeichnis **sqltest**, und öffnen Sie ***pom.xml*** in Ihrem bevorzugten Text-Editor.  Fügen Sie den Abhängigkeiten Ihres Projekts den **Microsoft JDBC-Treiber für SQL Server** hinzu. Verwenden Sie dazu den folgenden Code:

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.4.0.jre8</version>
   </dependency>
   ```

4. Fügen Sie Ihrem Projekt in ***pom.xml*** auch die folgenden Eigenschaften hinzu.  Sollte kein Abschnitt für Eigenschaften vorhanden sein, können Sie im Anschluss an die Abhängigkeiten einen entsprechenden Abschnitt hinzufügen.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. Speichern und schließen Sie ***pom.xml***.

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Ihr Maven-Projekt müsste bereits eine Datei namens ***App.java*** enthalten (unter „...\sqltest\src\main\java\com\sqlsamples\App.java“).

2. Öffnen Sie die Datei, ersetzen Sie ihren Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect to database
           String hostName = "your_server.database.windows.net";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
           Connection connection = null;

           try {
                   connection = DriverManager.getConnection(url);
                   String schema = connection.getSchema();
                   System.out.println("Successful connection - Schema: " + schema);

                   System.out.println("Query data example:");
                   System.out.println("=========================================");

                   // Create and execute a SELECT SQL statement.
                   String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                       + "FROM [SalesLT].[ProductCategory] pc "  
                       + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                   try (Statement statement = connection.createStatement();
                       ResultSet resultSet = statement.executeQuery(selectSql)) {

                           // Print results from select statement
                           System.out.println("Top 20 categories:");
                           while (resultSet.next())
                           {
                               System.out.println(resultSet.getString(1) + " "
                                   + resultSet.getString(2));
                           }
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.


## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- 
  [Microsoft JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)


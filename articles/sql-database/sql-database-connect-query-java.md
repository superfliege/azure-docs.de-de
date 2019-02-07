---
title: Abfragen von Azure SQL-Datenbank mithilfe von Java | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Verwendung von Java ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von T-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
manager: craigg
ms.date: 12/01/2018
ms.openlocfilehash: 3a036ac1260923a5030b8b0c3345482346c183fe
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563121"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Java

In diesem Artikel erfahren Sie, wie Sie mithilfe von [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Sie können dann T-SQL-Anweisungen zum Abfragen von Daten verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Ausführen dieses Beispiels sicher, dass die folgenden erforderlichen Komponenten vorhanden sind:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Java-Software für Ihr Betriebssystem:

  - **MacOS:** Installieren Sie Homebrew und Java und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu:** Installieren Sie Java, das Java Development Kit und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3 und 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows:** Installieren Sie Java und anschließend Maven. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-database-connection"></a>Abrufen der Datenbankverbindung

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-the-project"></a>Erstellen des Projekts

1. Erstellen Sie über die Eingabeaufforderung ein neues Maven-Projekt namens *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Wechseln Sie zum Ordner *sqltest*, und öffnen Sie *pom.xml* in Ihrem bevorzugten Text-Editor. Fügen Sie den Abhängigkeiten Ihres Projekts den **Microsoft JDBC-Treiber für SQL Server** hinzu. Verwenden Sie dazu den folgenden Code:

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Fügen Sie Ihrem Projekt in *pom.xml* auch die folgenden Eigenschaften hinzu. Sollte kein Abschnitt für Eigenschaften vorhanden sein, können Sie im Anschluss an die Abhängigkeiten einen entsprechenden Abschnitt hinzufügen.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Speichern und schließen Sie *pom.xml*.

## <a name="add-code-to-query-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

1. Ihr Maven-Projekt müsste bereits eine Datei namens *App.java* enthalten (unter

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*).

1. Öffnen Sie die Datei, und ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie anschließend die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

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
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
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

   > [!NOTE]
   > Im Codebeispiel wird die Beispieldatenbank **AdventureWorksLT** für Azure SQL verwendet.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die App an der Eingabeaufforderung aus.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)  

- [Microsoft JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)  

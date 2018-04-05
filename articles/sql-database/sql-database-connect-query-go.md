---
title: Abfragen von Azure SQL-Datenbank mithilfe von Go | Microsoft-Dokumentation
description: Erstellen Sie unter Verwendung von Go ein Programm, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und verwenden Sie Transact-SQL-Anweisungen zum Abfragen und Ändern von Daten.
services: sql-database
author: David-Engel
manager: craigg
ms.reviewer: MightyPen
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: go
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: v-daveng
ms.openlocfilehash: ae13f6e3c530e2807d1f52e322663b65aebbb076
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="use-go-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Go

In diesem Schnellstarttutorial erfahren Sie, wie Sie mithilfe von [Go](https://godoc.org/github.com/denisenkom/go-mssqldb) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Außerdem werden Transact-SQL-Anweisungen zum Abfragen und Ändern von Daten veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Schnellstarttutorial müssen Sie folgende Voraussetzungen erfüllen:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für dieses Schnellstarttutorial verwenden

- Go und die zugehörige Software müssen für das Betriebssystem installiert sein:

    - **macOS:** Installieren Sie Homebrew und GoLang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/mac/).
    - **Ubuntu:** Installieren Sie GoLang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/ubuntu/).
    - **Windows:** Installieren Sie GoLang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-go-project-and-dependencies"></a>Erstellen des Go-Projekts und der Abhängigkeiten

1. Erstellen Sie über das Terminal einen neuen Projektordner mit dem Namen **SqlServerSample**. 

   ```bash
   mkdir SqlServerSample
   ```

2. Ändern Sie das Verzeichnis in **SqlServerSample**, rufen Sie die SQL Server-Treiber für Go ab, und installieren Sie diese:

   ```bash
   cd SqlServerSample
   go get github.com/denisenkom/go-mssqldb
   go install github.com/denisenkom/go-mssqldb
   ```

## <a name="create-sample-data"></a>Erstellen von Beispieldaten

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine Datei namens **CreateTestData.sql** im Ordner **SqlServerSample**. Kopieren Sie den folgenden Text, und fügen Sie ihn in den enthaltenen T-SQL-Code ein. Dieser Code erstellt ein Schema und eine Tabelle und fügt einige Zeilen ein.

   ```sql
   CREATE SCHEMA TestSchema;
   GO

   CREATE TABLE TestSchema.Employees (
     Id       INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
     Name     NVARCHAR(50),
     Location NVARCHAR(50)
   );
   GO

   INSERT INTO TestSchema.Employees (Name, Location) VALUES
     (N'Jared',  N'Australia'),
     (N'Nikita', N'India'),
     (N'Tom',    N'Germany');
   GO

   SELECT * FROM TestSchema.Employees;
   GO
   ```

2. Stellen Sie mithilfe von sqlcmd eine Verbindung mit der Datenbank her, und führen Sie das SQL-Skript aus, um das Schema und die Tabelle zu erstellen und einige Zeilen einzufügen. Setzen Sie dabei die entsprechenden Werte für Server, Datenbank, Benutzernamen und Kennwort ein.

   ```bash
   sqlcmd -S your_server.database.windows.net -U your_username -P your_password -d your_database -i ./CreateTestData.sql
   ```

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie eine Datei mit dem Namen **sample.go** im Ordner **SqlServerSample**.

2. Öffnen Sie die Datei, und ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzernamen und Kennwort hinzu. In diesem Beispiel werden die GoLang-Kontextmethoden verwendet, um sicherzustellen, dass eine aktive Verbindung mit dem Datenbankserver besteht.

   ```go
   package main

   import (
       _ "github.com/denisenkom/go-mssqldb"
       "database/sql"
       "context"
       "log"
       "fmt"
   )

   var db *sql.DB

   var server = "your_server.database.windows.net"
   var port = 1433
   var user = "your_username"
   var password = "your_password"
   var database = "your_database"

   func main() {
       // Build connection string
       connString := fmt.Sprintf("server=%s;user id=%s;password=%s;port=%d;database=%s;",
           server, user, password, port, database)

       var err error

       // Create connection pool
       db, err = sql.Open("sqlserver", connString)
       if err != nil {
           log.Fatal("Error creating connection pool:", err.Error())
       }
       fmt.Printf("Connected!\n")

       // Create employee
       createId, err := CreateEmployee("Jake", "United States")
       fmt.Printf("Inserted ID: %d successfully.\n", createId)

       // Read employees
       count, err := ReadEmployees()
       fmt.Printf("Read %d rows successfully.\n", count)

       // Update from database
       updateId, err := UpdateEmployee("Jake", "Poland")
       fmt.Printf("Updated row with ID: %d successfully.\n", updateId)

       // Delete from database
       rows, err := DeleteEmployee("Jake")
       fmt.Printf("Deleted %d rows successfully.\n", rows)
   }

   func CreateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()
       var err error

       if db == nil {
           log.Fatal("What?")
       }

       // Check if database is alive.
       err = db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("INSERT INTO TestSchema.Employees (Name, Location) VALUES (@Name,@Location);")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))

       if err != nil {
           log.Fatal("Error inserting new row: " + err.Error())
           return -1, err
       }

       return result.LastInsertId()
   }

   func ReadEmployees() (int, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("SELECT Id, Name, Location FROM TestSchema.Employees;")

       // Execute query
       rows, err := db.QueryContext(ctx, tsql)
       if err != nil {
           log.Fatal("Error reading rows: " + err.Error())
           return -1, err
       }

       defer rows.Close()

       var count int = 0

       // Iterate through the result set.
       for rows.Next() {
           var name, location string
           var id int

           // Get values from row.
           err := rows.Scan(&id, &name, &location)
           if err != nil {
               log.Fatal("Error reading rows: " + err.Error())
               return -1, err
           }

           fmt.Printf("ID: %d, Name: %s, Location: %s\n", id, name, location)
           count++
       }

       return count, nil
   }

   // Update an employee's information
   func UpdateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("UPDATE TestSchema.Employees SET Location = @Location WHERE Name= @Name")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))
       if err != nil {
           log.Fatal("Error updating row: " + err.Error())
           return -1, err
       }

       return result.LastInsertId()
   }

   // Delete an employee from database
   func DeleteEmployee(name string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("DELETE FROM TestSchema.Employees WHERE Name=@Name;")

       // Execute non-query with named parameters
       result, err := db.ExecContext(ctx, tsql, sql.Named("Name", name))
       if err != nil {
           fmt.Println("Error deleting row: " + err.Error())
           return -1, err
       }

       return result.RowsAffected()
   }
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```bash
   go run sample.go
   ```

2. Überprüfen der Ausgabe:

   ```text
   Connected!
   Inserted ID: 4 successfully.
   ID: 1, Name: Jared, Location: Australia
   ID: 2, Name: Nikita, Location: India
   ID: 3, Name: Tom, Location: Germany
   ID: 4, Name: Jake, Location: United States
   Read 4 rows successfully.
   Updated row with ID: 4 successfully.
   Deleted 1 rows successfully.
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Go-Treiber für Microsoft SQL Server](https://github.com/denisenkom/go-mssqldb)
- [Seite für Problemmeldungen und Fragen](https://github.com/denisenkom/go-mssqldb/issues)


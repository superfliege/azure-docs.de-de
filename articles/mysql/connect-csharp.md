---
title: Herstellen einer Verbindung mit Azure Database for MySQL per C#
description: Diese Schnellstartanleitung enthält ein C#-Codebeispiel (.NET), mit dem Sie eine Verbindung mit den Daten aus Azure-Datenbank für MySQL herstellen und Daten abfragen können.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.devlang: csharp
ms.topic: quickstart
ms.date: 02/28/2018
ms.openlocfilehash: e0536440ea38f127a34639ffb40e5015154ec550
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547455"
---
# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a>Azure Database for MySQL: Verwenden von .NET (C#) zum Herstellen einer Verbindung und Abfragen von Daten
Dieser Schnellstart zeigt, wie Sie mit einer C#-Anwendung eine Verbindung mit einer Azure Database for MySQL-Instanz herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der C#-Entwicklung vertraut sind, aber noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren Sie [.NET](https://www.microsoft.com/net/download). Führen Sie die Schritte aus dem verlinkten Artikel durch, um .NET für Ihre Plattform (Windows, Ubuntu Linux oder macOS) zu installieren. 
- Installieren Sie [Visual Studio](https://www.visualstudio.com/downloads/).

## <a name="create-a-c-project"></a>Erstellen eines C#-Projekts
Führen Sie an der Eingabeaufforderung Folgendes aus:

```
mkdir AzureMySqlExample
cd AzureMySqlExample
dotnet new console
dotnet add package MySqlConnector
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie dann nach dem soeben erstellten Server, z.B. **mydemoserver**.
3. Klicken Sie auf den Servernamen.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-csharp/1_server-overview-name-login.png)

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um die Daten mit SQL-Anweisungen vom Typ `CREATE TABLE` und `INSERT INTO` zu verbinden und zu laden. Im Code wird die Klasse `MySqlConnection` mit der [OpenAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.createcommand)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQueryAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbcommand.executenonqueryasync)-Methode aufgerufen, um die Datenbankbefehle auszuführen. 

Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace AzureMySqlExample
{
    class MySqlCreate
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER@YOUR-SERVER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "DROP TABLE IF EXISTS inventory;";
                    await command.ExecuteNonQueryAsync();
                    Console.WriteLine("Finished dropping table (if existed)");

                    command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
                    await command.ExecuteNonQueryAsync();
                    Console.WriteLine("Finished creating table");

                    command.CommandText = @"INSERT INTO inventory (name, quantity) VALUES (@name1, @quantity1),
                        (@name2, @quantity2), (@name3, @quantity3);";
                    command.Parameters.AddWithValue("@name1", "banana");
                    command.Parameters.AddWithValue("@quantity1", 150);
                    command.Parameters.AddWithValue("@name2", "orange");
                    command.Parameters.AddWithValue("@quantity2", 154);
                    command.Parameters.AddWithValue("@name3", "apple");
                    command.Parameters.AddWithValue("@quantity3", 100);

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows inserted={0}", rowCount));
                }

                // connection will be closed by the 'using' block
                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="read-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `SELECT` zu verbinden und zu lesen. Im Code wird die `MySqlConnection`-Klasse mit der [OpenAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden [CreateCommand()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.createcommand) und [ExecuteReaderAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbcommand.executereaderasync) verwendet, um die Datenbankbefehle auszuführen. Als Nächstes wird im Code [ReadAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbdatareader.readasync#System_Data_Common_DbDataReader_ReadAsync) verwendet, um auf die Datensätze in den Ergebnissen zuzugreifen. Mithilfe von GetInt32 und GetString werden im Code dann die Werte im Datensatz analysiert.

Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace AzureMySqlExample
{
    class MySqlRead
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER@YOUR-SERVER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "SELECT * FROM inventory;";

                    using (var reader = await command.ExecuteReaderAsync())
                    {
                        while (await reader.ReadAsync())
                        {
                            Console.WriteLine(string.Format(
                                "Reading from table=({0}, {1}, {2})",
                                reader.GetInt32(0),
                                reader.GetString(1),
                                reader.GetInt32(2)));
                        }
                    }
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `UPDATE` zu verbinden und zu lesen. Im Code wird die `MySqlConnection`-Klasse mit der [OpenAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.createcommand)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQueryAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbcommand.executenonqueryasync)-Methode aufgerufen, um die Datenbankbefehle auszuführen. 

Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace AzureMySqlExample
{
    class MySqlUpdate
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER@YOUR-SERVER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "UPDATE inventory SET quantity = @quantity WHERE name = @name;";
                    command.Parameters.AddWithValue("@quantity", 200);
                    command.Parameters.AddWithValue("@name", "banana");

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows updated={0}", rowCount));
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `DELETE` zu verbinden und zu löschen. 

Im Code wird die `MySqlConnection`-Klasse mit der [OpenAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://docs.microsoft.com/dotnet/api/system.data.common.dbconnection.createcommand)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQueryAsync()](https://docs.microsoft.com/dotnet/api/system.data.common.dbcommand.executenonqueryasync)-Methode aufgerufen, um die Datenbankbefehle auszuführen. 

Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace AzureMySqlExample
{
    class MySqlDelete
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER@YOUR-SERVER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "DELETE FROM inventory WHERE name = @name;";
                    command.Parameters.AddWithValue("@name", "orange");

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows deleted={0}", rowCount));
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)

---
title: Abfragen von Azure SQL-Datenbank mithilfe von .NET Core | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von .NET Core ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/10/2018
ms.openlocfilehash: 5f2c8c2a9a2b21a15aa997ff0cc98860ceec76cc
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744835"
---
# <a name="quickstart-use-net-core-c-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von .NET Core (C#)

In dieser Schnellstartanleitung verwenden Sie [.NET Core](https://www.microsoft.com/net/) und C#-Code, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend führen Sie eine Transact-SQL-Anweisung zum Abfragen von Daten aus.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal-firewall.md) für die öffentliche IP-Adresse Ihres Computers.

- [.NET Core für Ihr Betriebssystem](https://www.microsoft.com/net/core) ist installiert. 

> [!NOTE]
> Dieser Schnellstart verwendet die *mySampleDatabase*-Datenbank. Wenn Sie eine andere Datenbank verwenden möchten, müssen Sie die Datenbankverweise ändern und die `SELECT`-Abfrage im C# Code ändern.


## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

#### <a name="get-adonet-connection-information-optional"></a>Abrufen von ADO.NET-Verbindungsinformationen (optional)

1. Navigieren Sie zur Seite **mySampleDatabase**, und wählen Sie unter **Einstellungen** die Option **Verbindungszeichenfolgen**.

2. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-connect-query-dotnet/adonet-connection-string2.png)

3. Kopieren Sie die **ADO.NET**-Verbindungszeichenfolge, wenn Sie sie verwenden möchten.
  
## <a name="create-a-new-net-core-project"></a>Erstellen eines neuen .NET Core-Projekts

1. Öffnen Sie eine Eingabeaufforderung, und erstellen Sie einen Ordner namens **sqltest**. Navigieren Sie zu diesem Ordner, und führen Sie den folgenden Befehl aus:

    ```cmd
    dotnet new console
    ```
    Mit diesem Befehl werden neue App-Projektdateien erstellt, darunter eine erste C#-Codedatei (**Program.cs**), eine XML-Konfigurationsdatei (**sqltest.csproj**) und erforderliche Binärdateien.

2. Öffnen Sie in einem Editor **sqltest.csproj**, und fügen Sie den nachstehenden XML-Code zwischen den `<Project>`-Tags ein. Mit diesem XML wird `System.Data.SqlClient` als Abhängigkeit hinzugefügt.

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.6.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Öffnen Sie in einem Editor die Datei **Program.cs**.

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzername und Kennwort hinzu.

> [!NOTE]
> Um eine ADO.NET-Verbindungszeichenfolge zu verwenden, ersetzen Sie die 4 Zeilen in der Codeeinstellung von Server, Datenbank, Benutzername und Kennwort durch die folgende Zeile. Geben Sie in der Zeichenfolge Ihren Benutzernamen und Ihr Kennwort ein.
>
>    `builder.ConnectionString="<your_ado_net_connection_string>";`

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();

                builder.DataSource = "<your_server.database.windows.net>"; 
                builder.UserID = "<your_username>";            
                builder.Password = "<your_password>";     
                builder.InitialCatalog = "<your_database>";
         
                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
            Console.WriteLine("\nDone. Press enter.");
            Console.ReadLine(); 
        }
    }
}
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```cmd
   dotnet restore
   dotnet run
   ```

2. Stellen Sie sicher, dass die ersten 20 Zeilen zurückgegeben werden.

   ```text
   Query data example:
   =========================================

   Road Frames HL Road Frame - Black, 58
   Road Frames HL Road Frame - Red, 58
   Helmets Sport-100 Helmet, Red
   Helmets Sport-100 Helmet, Black
   Socks Mountain Bike Socks, M
   Socks Mountain Bike Socks, L
   Helmets Sport-100 Helmet, Blue
   Caps AWC Logo Cap
   Jerseys Long-Sleeve Logo Jersey, S
   Jerseys Long-Sleeve Logo Jersey, M
   Jerseys Long-Sleeve Logo Jersey, L
   Jerseys Long-Sleeve Logo Jersey, XL
   Road Frames HL Road Frame - Red, 62
   Road Frames HL Road Frame - Red, 44
   Road Frames HL Road Frame - Red, 48
   Road Frames HL Road Frame - Red, 52
   Road Frames HL Road Frame - Red, 56
   Road Frames LL Road Frame - Black, 58
   Road Frames LL Road Frame - Black, 60
   Road Frames LL Road Frame - Black, 62

   Done. Press enter.
   ```
3. Drücken Sie die **EINGABETASTE**, um das Anwendungsfenster zu schließen.

## <a name="next-steps"></a>Nächste Schritte

- [Getting started with .NET Core on Windows/Linux/macOS using the command line](/dotnet/core/tutorials/using-with-xplat-cli) (Erste Schritte mit .NET Core unter Windows/Linux/macOS bei Verwendung der Befehlszeile)
- Informieren Sie sich darüber, wie Sie [unter Verwendung von .NET Framework und Visual Studio eine Verbindung mit einer Azure SQL-Datenbank herstellen und die Datenbank abfragen](sql-database-connect-query-dotnet-visual-studio.md).  
- Erfahren Sie, wie Sie [Ihre erste Azure SQL-Datenbank mithilfe von SSMS entwerfen](sql-database-design-first-database.md) oder wie Sie [eine Azure SQL-Datenbank entwerfen und mit C# und ADO.NET verbinden](sql-database-design-first-database-csharp.md).
- Weitere Informationen zu .NET finden Sie in der [.NET-Dokumentation](https://docs.microsoft.com/dotnet/).

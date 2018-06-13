---
title: Abfragen von Azure SQL-Datenbank mithilfe von Node.js | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Node.js ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 0d1cdd40264ff76b0175c861b3084ed7e7b62a31
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791000"
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Node.js

In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von [Node.js](https://nodejs.org/en/) ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für diese Schnellstartanleitung verwenden

- Node.js und die zugehörige Software müssen für das Betriebssystem installiert sein:
    - **macOS:** Installieren Sie Homebrew und Node.js, und installieren Sie anschließend den ODBC-Treiber und SQLCMD. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
    - **Ubuntu:** Installieren Sie Node.js und anschließend den ODBC-Treiber und SQLCMD. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
    - **Windows:** Installieren Sie Chocolatey und Node.js, und installieren Sie anschließend den ODBC-Treiber und SQLCMD. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Sie benötigen eine Firewallregel für die öffentliche IP-Adresse des Computers, auf dem Sie dieses Tutorial ausführen. Wenn Sie an einem anderen Computer arbeiten oder eine andere öffentliche IP-Adresse verwenden, [erstellen Sie über das Azure-Portal eine Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="create-a-nodejs-project"></a>Erstellen eines Node.js-Projekts

Öffnen Sie eine Eingabeaufforderung, und erstellen Sie einen Ordner namens *sqltest*. Navigieren Sie zu dem erstellten Ordner, und führen Sie den folgenden Befehl aus:

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie in Ihrer Entwicklungsumgebung oder in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.js**.

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```js
   node sqltest.js
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über den [Microsoft Node.js-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/).
- Informieren Sie sich darüber, wie Sie unter Linux/Windows/macOS [unter Verwendung von .NET Core eine Verbindung mit einer Azure SQL-Datenbank herstellen und die Datenbank abfragen](sql-database-connect-query-dotnet-core.md).  
- Informieren Sie sich über [die ersten Schritte mit .NET Core unter Windows/Linux/macOS bei Verwendung der Befehlszeile](/dotnet/core/tutorials/using-with-xplat-cli).
- Erfahren Sie, wie Sie [Ihre erste Azure SQL-Datenbank mithilfe von SSMS entwerfen](sql-database-design-first-database.md) oder wie Sie [Ihre erste Azure SQL-Datenbank mithilfe von .NET entwerfen](sql-database-design-first-database-csharp.md).
- Informieren Sie sich über das [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informieren Sie sich über das [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).


---
title: Abfragen von Azure SQL-Datenbank mithilfe von Ruby | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: ruby
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: f86c53465636f82cf36d5099699fc9e6efeb55a5
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31794683"
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Ruby

In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von [Ruby](https://www.ruby-lang.org) ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für diese Schnellstartanleitung verwenden

- Ruby und die zugehörige Software müssen für das Betriebssystem installiert sein:
    - **MacOS**: Installieren Sie nacheinander Homebrew, rbenv und ruby-build, Ruby und FreeTDS. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3, 1.4 und 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: Installieren Sie nacheinander die erforderlichen Komponenten für Ruby, rbenv und ruby-build, Ruby und dann FreeTDS. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3, 1.4 und 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Sie benötigen eine Firewallregel für die öffentliche IP-Adresse des Computers, auf dem Sie dieses Tutorial ausführen. Wenn Sie an einem anderen Computer arbeiten oder eine andere öffentliche IP-Adresse verwenden, [erstellen Sie über das Azure-Portal eine Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.rb**.

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```bash
   ruby sqltest.rb
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.


## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [GitHub-Repository für TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Report issues or ask questions about TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues) (Melden von Problemen oder Stellen von Fragen zu TinyTDS)
- [Ruby Driver for SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/) (Ruby-Treiber für SQL Server)

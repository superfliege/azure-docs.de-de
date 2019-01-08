---
title: Abfragen von Azure SQL-Datenbank mithilfe von Ruby | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 66819cbd65f6f044d0dac68326eb5890476964b6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993908"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Ruby

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von [Ruby](https://www.ruby-lang.org) eine Verbindung mit einer Azure SQL-Datenbank herstellen und Daten mit Transact-SQL-Anweisungen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal-firewall.md) für die öffentliche IP-Adresse des Computers, den Sie für diese Schnellstartanleitung verwenden
  
- Ruby und verwandte Software für Ihr Betriebssystem:
  
  - **MacOS**: Installieren Sie Homebrew, rbenv und ruby-build, Ruby, FreeTDS und TinyTDS. Siehe Schritte 1.2, 1.3, 1.4, 1.5 und 2.1 in [Erstellen von Ruby-Apps mit SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Installieren Sie die Voraussetzungen für Ruby, rbenv und ruby-build, Ruby, FreeTDS und TinyTDS. Siehe Schritte 1.2, 1.3, 1.4, 1.5 und 2.1 in [Erstellen von Ruby-Apps mit SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows:** Installieren Sie Ruby, Ruby Devkit und TinyTDS. Siehe [Konfigurieren der Entwicklungsumgebung für die Ruby-Entwicklung](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-your-sql-database"></a>Erstellen von Code zum Abfragen Ihrer SQL-Datenbank

1. Erstellen Sie in einem Text- oder Code-Editor eine neue Datei namens *sqltest.rb*.
   
1. Fügen Sie den folgenden Code hinzu: Ersetzen Sie die Werte aus der Azure SQL-Datenbank für `<server>`, `<database>`, `<username>` und `<password>`.
   
   >[!IMPORTANT]
   >Der Code in diesem Beispiel verwendet die AdventureWorksLT-Beispieldaten, die Sie beim Erstellen Ihrer Datenbank als Datenquelle auswählen können. Wenn Ihre Datenbank unterschiedliche Daten aufweist, verwenden Sie Tabellen aus Ihrer eigenen Datenbank in der SELECT-Abfrage. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
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

1. Führen Sie an der Eingabeaufforderung folgenden Befehl aus:

   ```bash
   ruby sqltest.rb
   ```
   
1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen aus Ihrer Datenbank zurückgegeben werden. 

## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [GitHub-Repository für TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Problemmeldungen oder Fragestellungen für TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ruby-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)

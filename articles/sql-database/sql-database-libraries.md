---
title: "Verbindungsbibliotheken für SQL-Datenbank | Microsoft-Dokumentation"
description: "Enthält Links für Moduldownloads, die Verbindungen mit SQL Server und SQL-Datenbank von zahlreichen Clientprogrammiersprachen ermöglichen."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: 012acd2b53fc9205511530d3cc30803dceef88a0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Verbindungsbibliotheken und -frameworks für SQL Server

Unsere [Tutorials „Erste Schritte“](http://aka.ms/sqldev) ermöglichen Ihnen den schnellen Einstieg in Programmiersprachen wie C#, Java, Node.js, PHP und Python. Erstellen Sie anschließend mit SQL Server unter Linux oder Windows oder mit Docker unter macOS eine App.

Die folgende Tabelle enthält Verbindungsbibliotheken oder *Treiber*, die Clientanwendungen unter Verwendung zahlreicher Programmiersprachen nutzen können, um eine Verbindung mit SQL Server in einer lokalen Umgebung oder in der Cloud herzustellen und SQL Server zu verwenden. Sie können unter Linux, Windows oder mit Docker verwendet werden, um Verbindungen mit Azure SQL-Datenbank und Azure SQL Data Warehouse herzustellen. 

| Sprache | Plattform | Zusätzliche Ressourcen | Download | Erste Schritte |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET für SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Download](https://www.microsoft.com/net/download/) | [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC-Treiber für SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Download](https://go.microsoft.com/fwlink/?linkid=852460) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP SQL-Treiber für SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Betriebssystem: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Node.js-Treiber für SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Installieren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python-SQL-Treiber](http://msdn.microsoft.com/library/mt652092.aspx) | Installationsoptionen: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby-Treiber für SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Installieren](https://msdn.microsoft.com/library/mt711041.aspx) | [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC-Treiber für SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Download](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

Die folgende Tabelle enthält Beispiele für ORM (Object-Relational Mapping)-Frameworks und Webframeworks, die Clientanwendungen mit SQL Server in einer lokalen Umgebung oder in der Cloud nutzen können. Die Frameworks können unter Linux, Windows oder mit Docker verwendet werden, um Verbindungen mit SQL-Datenbank und SQL Data Warehouse herzustellen. 

| Sprache | Plattform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>Verwandte Links
- [SQL Server-Treiber](http://msdn.microsoft.com/library/mt654049.aspx) zum Herstellen einer Verbindung von Clientanwendungen
- Verbinden mit SQL-Datenbank:
    - [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP](sql-database-connect-query-php.md)
    - [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js](sql-database-connect-query-nodejs.md)
    - [Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Java](sql-database-connect-query-java.md)
    - [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python](sql-database-connect-query-python.md)
    - [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby](sql-database-connect-query-ruby.md)
- Codebeispiele für Wiederholungslogik:
    - [Herstellen robuster Verbindungen mit SQL mit ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Herstellen robuster Verbindungen mit SQL mit PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php


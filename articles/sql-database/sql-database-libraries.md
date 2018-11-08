---
title: Verbindungsbibliotheken für SQL-Datenbank | Microsoft-Dokumentation
description: Enthält Links für Moduldownloads, die Verbindungen mit SQL Server und SQL-Datenbank von zahlreichen Clientprogrammiersprachen ermöglichen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: d7c0a2060d1ec084e575edaf128d72495b1d7b85
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259637"
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Verbindungsbibliotheken und -frameworks für SQL Server

Unsere [Tutorials „Erste Schritte“](https://aka.ms/sqldev) ermöglichen Ihnen den schnellen Einstieg in Programmiersprachen wie C#, Java, Node.js, PHP und Python. Erstellen Sie anschließend mit SQL Server unter Linux oder Windows oder mit Docker unter macOS eine App.

Die folgende Tabelle enthält Verbindungsbibliotheken oder *Treiber*, die Clientanwendungen unter Verwendung zahlreicher Programmiersprachen nutzen können, um eine Verbindung mit SQL Server in einer lokalen Umgebung oder in der Cloud herzustellen und SQL Server zu verwenden. Sie können unter Linux, Windows oder mit Docker verwendet werden, um Verbindungen mit Azure SQL-Datenbank und Azure SQL Data Warehouse herzustellen. 

| Sprache | Plattform | Zusätzliche Ressourcen | Download | Erste Schritte |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET für SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Download](https://www.microsoft.com/net/download/) | [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC-Treiber für SQL Server](https://msdn.microsoft.com/library/mt484311.aspx) | [Download](https://go.microsoft.com/fwlink/?linkid=852460) |  [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP SQL-Treiber für SQL Server](https://msdn.microsoft.com/library/dn865013.aspx) | Betriebssystem: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=55642) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Node.js-Treiber für SQL Server](https://msdn.microsoft.com/library/mt652093.aspx) | [Installieren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python-SQL-Treiber](https://msdn.microsoft.com/library/mt652092.aspx) | Installationsoptionen: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby-Treiber für SQL Server](https://msdn.microsoft.com/library/mt691981.aspx) | [Installieren](https://msdn.microsoft.com/library/mt711041.aspx) | [Erste Schritte](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC-Treiber für SQL Server](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [Download](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

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
- [SQL Server-Treiber](https://msdn.microsoft.com/library/mt654049.aspx) zum Herstellen einer Verbindung von Clientanwendungen
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


---
title: "Verbindungsbibliotheken für SQL-Datenbank | Microsoft-Dokumentation"
description: "Enthält Links für Moduldownloads, die die Verbindung mit SQL Server und SQL-Datenbank von zahlreichen Clientprogrammiersprachen ermöglichen."
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
ms.date: 07/05/2017
ms.author: genemi
ms.openlocfilehash: bdf83fac9bd0ac6790062f802748a18045c7a171
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Verbindungsbibliotheken und Frameworks für Microsoft SQL Server

Sehen Sie sich unsere [Tutorials zu ersten Schritten](http://aka.ms/sqldev), um einen schnellen Einstieg in Programmiersprachen wie C#, Java, Node.js, PHP und Python zu erhalten und eine Anwendung mithilfe von SQL Server unter Linux oder Windows oder in Docker unter macOS zu erstellen.

Die folgende Tabelle listet Verbindungsbibliotheken oder *Treiber* auf, die Clientanwendungen in einer Vielzahl von Sprachen verwenden können, um eine Verbindung mit Microsoft SQL Server herzustellen und dieses lokal oder in der Cloud auszuführen unter Linux, Windows oder in Docker. Ebenso kann eine Verbindung mit Azure SQL-Datenbank und Azure SQL Data Warehouse hergestellt werden. 

| Sprache | Plattform | Zusätzliche Ressourcen | Herunterladen | Erste Schritte |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET für SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Download](https://www.microsoft.com/net/download/) | [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC Driver für SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Download](https://go.microsoft.com/fwlink/?linkid=852460) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP SQL-Treiber für SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Betriebssystem: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Node.js-Treiber für SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Installieren](https://msdn.microsoft.com/library/mt652094.aspx) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python-SQL-Treiber](http://msdn.microsoft.com/library/mt652092.aspx) | Installationsoptionen: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby-Treiber für SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Installieren](https://msdn.microsoft.com/library/mt711041.aspx) | [Erste Schritte](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC-Treiber für SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Download](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

In der folgenden Tabelle sind einige Beispiele für Frameworks und Webframeworks der objektrelationalen Abbildung (Object Relational Mapping, ORM) aufgelistet, die Clientanwendungen mit lokalem Microsoft SQL Server oder in der Cloud unter Linux, Windows oder in Docker und auch in Azure SQL-Datenbank und Azure SQL Data Warehouse verwenden können. 

| Sprache | Plattform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](http://rubyonrails.org/) |

## <a name="related-links"></a>Verwandte Links
- [SQL Server-Treiber](http://msdn.microsoft.com/library/mt654049.aspx) zum Herstellen einer Verbindung von Clientanwendungen
- [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-connect-query-dotnet.md)
- [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP](sql-database-connect-query-php.md)
- [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js](sql-database-connect-query-nodejs.md)
- [Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Java](sql-database-connect-query-java.md)
- [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python](sql-database-connect-query-python.md)
- [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby](sql-database-connect-query-ruby.md)

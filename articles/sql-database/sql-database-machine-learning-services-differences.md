---
title: 'Wichtige Unterschiede von Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion): Übersicht'
description: Dieses Thema beschreibt die wichtigsten Unterschiede zwischen Machine Learning Services von Azure SQL-Datenbank (mit R) und SQL Server Machine Learning Services.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: adc303e9b375aaa6f37e9e79ea5434675c75523d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824897"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-and-sql-server"></a>Wichtige Unterschiede zwischen Machine Learning Services in Azure SQL-Datenbank und SQL Server

Die Funktionalität von Machine Learning Services (mit R) in Azure SQL-Datenbank ähnelt [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Nachfolgend finden Sie einige wesentliche Unterschiede zwischen diesen Diensten.

## <a name="language-support"></a>Sprachunterstützung

SQL Server bietet Unterstützung für R und Python über das [Erweiterbarkeitsframework](https://docs.microsoft.com/en-us/sql/advanced-analytics/concepts/extensibility-framework). SQL-Datenbank unterstützt nicht beide Sprachen. Folgende wichtige Unterschiede bestehen:

- R ist die einzige unterstützte Sprache in SQL-Datenbank. Momentan ist keine Unterstützung für Python vorhanden.
- Die Version von R ist 3.4.4.
- Es ist nicht erforderlich, `external scripts enabled` über `sp_configure` zu konfigurieren. Sobald Sie [registriert](sql-database-machine-learning-services-overview.md#signup) sind, wird Machine Learning für Ihre SQL-Datenbank aktiviert.

## <a name="package-management"></a>Paketverwaltung

Die R-Paketverwaltung und -Installation funktionieren für SQL-Datenbank und SQL Server unterschiedlich. Die folgenden Unterschiede bestehen:

- R-Pakete werden über [sqlmlutils](https://github.com/Microsoft/sqlmlutils) oder [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) installiert.
- Pakete können keine ausgehenden Netzwerkaufrufe ausführen. Diese Einschränkung ähnelt den [Standardfirewallregeln für Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) in SQL Server, kann jedoch in SQL-Datenbank nicht geändert werden.
- Es gibt keine Unterstützung für Pakete, die von externen Runtimes (z.B. Java) abhängig sind oder Zugriff auf Betriebssystem-APIs für die Installation oder Verwendung benötigen.

## <a name="resource-governance"></a>Ressourcenkontrolle

Es ist nicht möglich, R-Ressourcen durch [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) und externe Ressourcenpools zu beschränken. R-Ressourcen stellen einen Prozentsatz der Ressourcen von SQL-Datenbank dar und hängen von der ausgewählten Dienstebene ab. Weitere Informationen finden Sie unter [Kaufmodelle für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

## <a name="security-isolation"></a>Sicherheitsisolierung

In Azure SQL-Datenbank bietet SQLPAL (SQL Plattform Abstraction Layer) eine Isolierung für externe Prozesse. Diese Isolierung stellt eine zusätzliche Sicherheitsebene für die Ausführung von R-Skripts bereit.

## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen finden Sie in der Dokumentation [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics).
- Informationen zum Verwenden von Machine Learning Services (mit R) in Azure SQL-Datenbank finden Sie unter [Schnellstart](sql-database-connect-query-r.md).
- Erfahren Sie mehr mit [Tutorials für die R-Sprache von SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)
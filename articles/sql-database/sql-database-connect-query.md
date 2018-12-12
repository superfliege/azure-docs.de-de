---
title: Azure SQL-Datenbank – Schnellstartanleitung für Verbindungsherstellung und Abfragen | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung zu Azure SQL-Datenbank wird beschrieben, wie Sie für Azure SQL-Datenbank eine Verbindung herstellen und Abfragen durchführen.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: ab1f96b2348c24a746aaa7bdd670f396884ee3f5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52870196"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Schnellstarts: Azure SQL-Datenbank – Verbindungsherstellung und Abfragen

Das folgende Dokument enthält Links zu Azure-Beispielen, in denen veranschaulicht wird, wie Sie eine Verbindung mit einer Azure SQL-Datenbank herstellen und Daten in der Datenbank abfragen. Darüber hinaus enthält es einige Empfehlungen zur Sicherheit auf Transportebene.

## <a name="quickstarts"></a>Schnellstarts

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|In diesem Schnellstart wird veranschaulicht, wie Sie SSMS zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und anschließend mithilfe von Transact-SQL-Anweisungen Daten in der Datenbank abfragen, aktualisieren oder löschen bzw. in diese einfügen.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure Data Studio zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz verwenden und dann mit T-SQL-Anweisungen (Transact-SQL) die in Tutorials für Azure Data Studio verwendete TutorialDB-Komponente erstellen.|
|[Azure-Portal](sql-database-connect-query-portal.md)|In diesem Schnellstart wird veranschaulicht, wie Sie den Abfrage-Editor zum Herstellen einer Verbindung mit einer SQL-Datenbank verwenden und anschließend Transact-SQL-Anweisungen nutzen, um Daten in der Datenbank abzufragen, einzufügen, zu aktualisieren und zu löschen.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|In diesem Schnellstart wird veranschaulicht, wie Sie Visual Studio Code zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und anschließend Transact-SQL-Anweisungen nutzen, um Daten in der Datenbank abzufragen, zu aktualisieren und zu löschen bzw. in diese einzufügen.|
|[.NET mit Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework ein C#-Programm mit Visual Studio erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von .NET Core unter Windows/Linux/macOS ein C#-Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.|
|[Go](sql-database-connect-query-go.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Go eine Verbindung mit einer Azure SQL-Datenbank herstellen. Außerdem werden Transact-SQL-Anweisungen zum Abfragen und Ändern von Daten veranschaulicht.|
|[Java](sql-database-connect-query-java.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Java eine Verbindung mit einer Azure SQL-Datenbank herstellen und anschließend Daten mithilfe von Transact-SQL-Anweisungen abfragen.|
|[Node.js](sql-database-connect-query-nodejs.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Node.js ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.|
|[PHP](sql-database-connect-query-php.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von PHP ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.|
|[Python](sql-database-connect-query-python.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python eine Verbindung mit einer Azure SQL-Datenbank herstellen und Daten mithilfe von Transact-SQL-Anweisungen abfragen. |
|[Ruby](sql-database-connect-query-ruby.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt, und mithilfe von Transact-SQL-Anweisungen Daten abfragen.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>Überlegungen zu TLS für Verbindungen mit einer SQL-Datenbank
Transport Layer Security (TLS) wird von allen Treibern verwendet, die Microsoft zum Herstellen einer Verbindung mit Azure SQL-Datenbank bereitstellt oder unterstützt. Es ist keine spezielle Konfiguration erforderlich. Für alle Verbindungen mit SQL Server oder Azure SQL-Datenbank empfehlen wir, dass alle Anwendungen auf die folgenden Konfigurationen oder deren Entsprechungen festgelegt sind:

 - **Verschlüsseln = Ein**
 - **TrustServerCertificate = Aus**

Einige Systeme verwenden andere, jedoch entsprechende Schlüsselwörter für diese Konfigurationsschlüsselwörter. Durch diese Konfigurationen wird sichergestellt, dass der Clienttreiber die Identität des vom Server empfangenen TLS-Zertifikats überprüft.

Außerdem empfiehlt es sich, TLS 1.1 und 1.0 auf dem Client zu deaktivieren, wenn Sie den Payment Card Industry Data Security Standard (PCI-DSS) einhalten müssen.

Nicht von Microsoft stammende Treiber verwenden TLS möglicherweise nicht standardmäßig. Dies kann beim Herstellen einer Verbindung mit Azure SQL-Datenbank ein Faktor sein. Bei Anwendungen mit eingebetteten Treibern können Sie diese Verbindungseinstellungen möglicherweise nicht steuern. Wir empfehlen Ihnen, die Sicherheit solcher Treiber und Anwendungen zu überprüfen, bevor Sie sie auf Systemen verwenden, die mit vertraulichen Daten interagieren.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verbindungsarchitektur finden Sie unter [Verbindungsarchitektur der Azure SQL-Datenbank](sql-database-connectivity-architecture.md).
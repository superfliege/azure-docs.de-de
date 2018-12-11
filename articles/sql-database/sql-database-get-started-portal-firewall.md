---
title: 'Azure-Portal: Erstellen einer SQL-Datenbank-Firewallregel | Microsoft-Dokumentation'
description: Erstellen einer SQL-Datenbank-Firewallregel auf Serverebene
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 12/01/2018
ms.openlocfilehash: cab92539b5019d4807ddefb2b84279c844f53016
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721900"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Schnellstart: Erstellen einer SQL-Datenbank-Firewallregel auf Serverebene mithilfe des Azure-Portals

In dieser Schnellstartanleitung wird gezeigt, die Sie eine Firewallregel auf Serverebene für eine Azure SQL-Datenbank erstellen, um eine Verbindung von einer lokalen Ressource herstellen zu können.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung werden als Ausgangspunkt die Ressourcen verwendet, die unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](sql-database-get-started-portal.md) erstellt werden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbank-Dienst erstellt eine Firewall auf Serverebene. Diese Firewall verhindert, dass Anwendungen und Tools eine Verbindung mit dem Server oder einer Serverdatenbank herstellen, sofern Sie keine Firewallregel erstellen, mit der die Firewall geöffnet wird. Für eine Verbindung von einer IP-Adresse, die sich außerhalb von Azure befindet, müssen Sie eine Firewallregel für eine bestimmte IP-Adresse oder einen IP-Adressbereich erstellen. Weitere Informationen zu Firewallregeln finden Sie unter [Firewallregeln für Azure SQL-Datenbank und SQL Data Warehouse](sql-database-firewall-configure.md).

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung über ein Unternehmensnetzwerk herzustellen, wird ausgehender Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

Führen Sie die hier angegebenen Schritte zum Erstellen einer Firewallregel auf Serverebene für die IP-Adresse Ihres Clients und zum Zulassen der externen Konnektivität durch die SQL-Datenbank-Firewall nur für Ihre IP-Adresse aus.

1. Nachdem die [Voraussetzungen für die Bereitstellung von Azure SQL-Datenbank](#prerequisites) erfüllt sind, können Sie im Menü auf der linken Seite **SQL-Datenbanken** und dann auf der Seite **SQL-Datenbanken** die Option **mySampleDatabase** wählen. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170824.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

2. Kopieren Sie diesen vollqualifizierten Servernamen, damit Sie ihn beim Herstellen der Verbindung mit Ihrem Server und den zugehörigen Datenbanken in anderen Schnellstartanleitungen verwenden können.

   ![Servername](./media/sql-database-get-started-portal/server-name.png)

3. Wählen Sie in der Symbolleiste die Option **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet.

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Wählen Sie in der Symbolleiste die Option **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

   > [!IMPORTANT]
   > Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Wählen Sie auf dieser Seite die Option **AUS**, um dies für alle Azure-Dienste zu deaktivieren.
   >

5. Wählen Sie **Speichern**aus. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

6. Schließen Sie die Seite **Firewalleinstellungen**.

Sie können für diese IP-Adresse jetzt eine Verbindung mit dem SQL-Datenbank-Server und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Speichern Sie diese Ressourcen, um unter [Nächste Schritte](#next-steps) zu erfahren, wie Sie eine Verbindung herstellen und Ihre Datenbank mit verschiedenen Methoden abfragen. Sie können die Ressourcen, die Sie im Rahmen dieser Schnellstartanleitung erstellt haben, aber auch wieder löschen. Gehen Sie hierzu wie folgt vor:


1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** und dann **myResourceGroup**.
2. Wählen Sie auf der Seite mit Ihrer Ressourcengruppe die Option **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und wählen Sie anschließend **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Sie nun über eine Datenbank verfügen, können Sie mit Ihren bevorzugten Tools oder Sprachen [eine Verbindung herstellen und Abfragen ausführen](sql-database-connect-query.md). Dazu zählen auch:
  - [Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](sql-database-connect-query-ssms.md)
  - [Verbinden und Abfragen mit Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Informationen zum Entwerfen Ihrer ersten Datenbank, zum Erstellen von Tabellen und zum Einfügen von Daten finden Sie in diesen Tutorials:
  - [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
  - [Entwerfen einer Azure SQL-Datenbank und Herstellen einer Verbindung mit C# und ADO.NET](sql-database-design-first-database-csharp.md)

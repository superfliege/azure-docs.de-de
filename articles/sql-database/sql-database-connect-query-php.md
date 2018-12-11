---
title: Abfragen von Azure SQL-Datenbank mithilfe von PHP | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Verwendung von PHP ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von T-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: be3ac9fab6c89c65ad9673811e108cefe2c80d00
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724246"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von PHP

In diesem Artikel erfahren Sie, wie Sie mithilfe von [PHP](http://php.net/manual/en/intro-whatis.php) eine Verbindung mit einer Azure SQL-Datenbank herstellen. Sie können dann T-SQL-Anweisungen zum Abfragen von Daten verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Ausführen dieses Beispiels sicher, dass die folgenden erforderlichen Komponenten vorhanden sind:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal-firewall.md) für die öffentliche IP-Adresse des Computers, den Sie verwenden

- PHP-Software für Ihr Betriebssystem:

    - **MacOS:** Installieren Sie Homebrew und PHP, den ODBC-Treiber und SQLCMD und anschließend den PHP-Treiber für SQL Server. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).

    - **Ubuntu:** Installieren Sie PHP und andere erforderliche Pakete, und installieren Sie anschließend den PHP-Treiber für SQL Server. Weitere Informationen finden Sie unter [Schritt 1.2 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).

    - **Windows:** Installieren Sie PHP für IIS Express und Chocolatey und anschließend den ODBC-Treiber und SQLCMD. Weitere Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-database-connection"></a>Abrufen der Datenbankverbindung

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens *sqltest.php*.  

1. Ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie anschließend die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die App an der Eingabeaufforderung aus.

   ```bash
   php sqltest.php
   ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)

- [Microsoft PHP Drivers for SQL Server](https://github.com/Microsoft/msphpsql/) (Microsoft-PHP-Treiber für SQL Server)

- [Seite für Problemmeldungen und Fragen](https://github.com/Microsoft/msphpsql/issues)

- [Beispiel für Wiederholungslogik: Herstellen robuster Verbindungen mit SQL mit PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)

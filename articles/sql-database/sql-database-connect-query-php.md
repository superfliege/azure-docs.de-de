---
title: Abfragen von Azure SQL-Datenbank mithilfe von PHP | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von PHP ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: quickstart
ms.date: 11/29/2017
ms.author: carlrab
ms.openlocfilehash: a562851330d325ca4a8be20ec82b5f421e23c4f1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="use-php-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von PHP

In diesem Schnellstarttutorial erfahren Sie, wie Sie unter Verwendung von [PHP](http://php.net/manual/en/intro-whatis.php) ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und mithilfe von Transact-SQL-Anweisungen Daten abfragt.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Schnellstarttutorial benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für dieses Schnellstarttutorial verwenden

- PHP und die zugehörige Software müssen für das Betriebssystem installiert sein:

    - **MacOS:** Installieren Sie Homebrew und PHP, installieren Sie den ODBC-Treiber und SQLCMD, und installieren Sie anschließend den PHP-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).
    - **Ubuntu:** Installieren Sie PHP und andere erforderliche Pakete, und installieren Sie anschließend den PHP-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: Installieren Sie die neueste Version von PHP für IIS Express, die neueste Version der Microsoft-Treiber für SQL Server in IIS Express, Chocolatey, den ODBC-Treiber und SQLCMD. Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.php**.  

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
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

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```php
   php sqltest.php
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft PHP Drivers for SQL Server](https://github.com/Microsoft/msphpsql/) (Microsoft-PHP-Treiber für SQL Server)
- [Seite für Problemmeldungen und Fragen](https://github.com/Microsoft/msphpsql/issues)
- [Beispiel für Wiederholungslogik: Herstellen robuster Verbindungen mit SQL mit PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php


---
title: Kompatibilität von MySQL-Treibern und -Verwaltungstools
description: In diesem Artikel werden die MySQL-Treiber und -Verwaltungstools beschrieben, die mit Azure Database for MySQL kompatibel sind.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 9e56c2bd65f8a9a517a7cdebe02a1d051c689df6
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985886"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL-Treiber und -Verwaltungstools, die mit Azure Database for MySQL kompatibel sind
In diesem Artikel werden die Treiber und Verwaltungstools beschrieben, die mit Azure Database for MySQL kompatibel sind.

## <a name="mysql-drivers"></a>MySQL-Treiber
Azure Database for MySQL verwendet die weltweit am häufigsten verwendete Communityedition von MySQL-Datenbank. Daher ist es mit einer Vielzahl von Programmiersprachen und Treibern kompatibel. Das Ziel besteht darin, die drei letzten Versionen von MySQL-Treibern zu unterstützen. Außerdem soll durch einen ständigen Austausch mit den Autoren der Open-Source-Community eine kontinuierliche Verbesserung von Funktionalität und Verwendbarkeit der MySQL-Treibern erreicht werden. Eine Liste der Treiber, die getestet und als mit Azure Database for MySQL 5.6 und 5.7 kompatibel eingestuft wurden, finden Sie in der folgenden Tabelle:

| **Treiber** | **Links** | **Kompatible Versionen** | **Nicht kompatible Versionen** | **Hinweise** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Fügen Sie für PHP-7.0-Verbindungen mit SSL MySQLi das MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in die Verbindungszeichenfolge ein. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Für PDO legen Sie die Option ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT```auf FALSE fest.|
| .Net | [MySqlConnector auf GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Installationspaket von Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0.27 und höher | 0.26.5 und früher | |
| Nodejs |  [MySQLjs auf GitHub](https://github.com/mysqljs/mysql/releases) <br> Installationspaket von NPM:<br> Führen Sie `npm install mysql` von NPM aus. | 2.15 | 2.14.1 und früher | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 und früher | Verwenden Sie „allowNativePasswords=true“ in der Verbindungszeichenfolge. |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 und früher | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 und früher | |

## <a name="management-tools"></a>Verwaltungstools
Der Kompatibilitätsvorteil erstreckt sich bis in die Datenbank-Verwaltungstools. Ihre vorhandenen Tools sollten auch mit Azure Database for MySQL funktionieren, solange Änderungen an der Datenbank innerhalb der Grenzen der Benutzerberechtigungen ausgeführt werden. Drei häufig verwendete Datenbank-Verwaltungstools, die getestet und als mit Azure Database for MySQL 5.6 und 5.7 kompatibel eingestuft wurden, finden Sie in der folgenden Tabelle:

|                                     | **MySQL Workbench ab Version 6.x** | **Navicat 12** | **PHPMyAdmin ab Version 4.x** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Erstellen, Aktualisieren, Lesen, Schreiben, Löschen | X | X | X |
| SSL-Verbindung | X | X | X |
| Automatische Vervollständigung von SQL-Abfragen | X | X |  |
| Importieren und Exportieren von Daten | X | X | X |
| Exportieren in mehreren Formaten | X | X | X |
| Sichern und Wiederherstellen |  | X |  |
| Anzeigen von Serverparametern | X | X | X |
| Anzeigen von Clientverbindungen | X | X | X |

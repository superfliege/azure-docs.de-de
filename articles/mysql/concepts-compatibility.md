---
title: "Kompatibilität von MySQL-Treibern und -Verwaltungstools | Microsoft-Dokumentation"
description: MySQL-Treiber und -Verwaltungstools, die mit Azure Database for MySQL kompatibel sind
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/27/2017
ms.openlocfilehash: 4df0dcc7d0f2bde24c1a7e12eea6fa142612a0e7
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
## <a name="mysql-drivers"></a>MySQL-Treiber
Azure Database for MySQL verwendet die weltweit am häufigsten verwendete Communityedition von MySQL-Datenbank. Daher ist es mit einer Vielzahl von Programmiersprachen und Treibern kompatibel. Das Ziel besteht darin, die drei letzten Versionen von MySQL-Treibern zu unterstützen. Außerdem soll durch einen ständigen Austausch mit den Autoren der Open-Source-Community eine kontinuierliche Verbesserung von Funktionalität und Verwendbarkeit der MySQL-Treibern erreicht werden. Eine Liste der Treiber, die getestet und als mit Azure Database for MySQL 5.6 und 5.7 kompatibel eingestuft wurden, finden Sie in der folgenden Tabelle:

| **Treiber** | **Links** | **Kompatible Versionen** | **Nicht kompatible Versionen** | **Hinweise** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Fügen Sie für PHP-7.0-Verbindungen mit SSL MySQLi das MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in die Verbindungszeichenfolge ein. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Für PDO legen Sie die Option ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT```auf FALSE fest.|
| .Net | [MySqlConnector auf GitHub]: https://github.com/mysql-net/MySqlConnector/releases <br> [Installationspaket von NuGet]:<br> https://www.nuget.org/packages/MySqlConnector/ | 0.27 und höher | 0.26.5 und früher | |
| Nodejs |  [MySQLjs auf GitHub]:<br> https://github.com/mysqljs/mysql/releases <br> [Installationspaket von NPM]:<br> Führen Sie „npm install mysql“ über NPM aus. | 2.15 | 2.14.1 und früher | |
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
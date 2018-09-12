---
title: Abfragen von Azure SQL-Datenbank mithilfe von Python | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Python ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: python
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: 2f907ccd371a9b2c4601a5ea266326a32c4f665d
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054932"
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Python

 In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von [Python](https://python.org) eine Verbindung mit einer Azure SQL-Datenbank herstellen und Daten mithilfe von Transact-SQL-Anweisungen abfragen. Ausführlichere SDK-Informationen finden Sie in unserer [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/sql), in [diesem pyodbc-Beispiel](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) und im GitHub-Repository [pyodbc](https://github.com/mkleehammer/pyodbc/wiki/).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal-firewall.md) für die öffentliche IP-Adresse des Computers, den Sie für diese Schnellstartanleitung verwenden

- Python und die zugehörige Software müssen für das Betriebssystem installiert sein:

    - **MacOS:** Installieren Sie Homebrew und Python, installieren Sie den ODBC-Treiber und SQLCMD, und installieren Sie anschließend den Python-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu:** Installieren Sie Python und andere erforderliche Pakete, und installieren Sie anschließend den Python-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows:** Installieren Sie die neueste Version von Python (die Umgebungsvariable wird nun für Sie konfiguriert), installieren Sie den ODBC-Treiber und SQLCMD, und installieren Sie anschließend den Python-Treiber für SQL Server. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank 

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.py**.  

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```Python
   python sqltest.py
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft Python Drivers for SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/) (Microsoft-Python-Treiber für SQL Server)
- [Python Developer Center](https://azure.microsoft.com/develop/python/?v=17.23h)


---
title: Abfragen von Azure SQL-Datenbank mithilfe von Python | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Python ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/28/2019
ms.openlocfilehash: b611eb02203c872e3497b5b7c12acddd9eab14c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188387"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Python

 In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von [Python](https://python.org) eine Verbindung mit einer Azure SQL-Datenbank herstellen und Daten mithilfe von Transact-SQL-Anweisungen abfragen. Ausführlichere SDK-Informationen finden Sie in unserer [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/sql), im [pyodbc-GitHub-Repository](https://github.com/mkleehammer/pyodbc/wiki/) und in einem [pyodbc-Beispiel](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Python und verwandte Software für Ihr Betriebssystem:
  
  - **MacOS**: Installieren Sie Homebrew und Python, installieren Sie den ODBC-Treiber und SQLCMD, und installieren Sie anschließend den Python-Treiber für SQL Server. Siehe Schritte 1.2, 1.3 und 2.1 in [Erstellen von Python-Apps mit SQL Server auf macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/). Weitere Informationen finden Sie unter [Installieren des Microsoft ODBC-Treibers unter Linux und macOS](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).
    
  - **Ubuntu**: Installieren Sie Python und andere erforderliche Pakete mit `sudo apt-get install python python-pip gcc g++ build-essential`. Laden Sie den ODBC-Treiber, SQLCMD und den Python-Treiber für SQL Server herunter, und installieren Sie sie. Anweisungen hierzu finden Sie unter [Konfigurieren einer Entwicklungsumgebung für pyodbc-Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).
    
  - **Windows:** Installieren Sie Python, den ODBC-Treiber und SQLCMD sowie den Python-Treiber für SQL Server. Anweisungen hierzu finden Sie unter [Konfigurieren einer Entwicklungsumgebung für pyodbc-Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="create-code-to-query-your-sql-database"></a>Erstellen von Code zum Abfragen Ihrer SQL-Datenbank 

1. Erstellen Sie in einem Text-Editor eine neue Datei namens *sqltest.py*.  
   
1. Fügen Sie den folgenden Code hinzu: Geben Sie für \<Server>, \<Datenbank>, \<Benutzername> und \<Kennwort> Ihre eigenen Werte ein.
   
   >[!IMPORTANT]
   >Der Code in diesem Beispiel verwendet die AdventureWorksLT-Beispieldaten, die Sie beim Erstellen Ihrer Datenbank als Datenquelle auswählen können. Wenn Ihre Datenbank unterschiedliche Daten aufweist, verwenden Sie Tabellen aus Ihrer eigenen Datenbank in der SELECT-Abfrage. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung folgenden Befehl aus:

   ```cmd
   python sqltest.py
   ```

1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen zurückgegeben werden, und schließen Sie das Befehlsfenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft-Python-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Developer Center](https://azure.microsoft.com/develop/python/?v=17.23h)


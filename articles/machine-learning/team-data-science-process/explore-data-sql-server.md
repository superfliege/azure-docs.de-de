---
title: Durchsuchen von Daten auf einer SQL Server-VM in Azure | Microsoft Docs
description: Durchsuchen von auf einer SQL Server-VM in Azure gespeicherten Daten.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: da01a1a386e1ffc590f4cbbb85d9d50a180d4973
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442929"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Durchsuchen von Daten auf einer SQL Server-VM in Azure

In diesem Artikel wird beschrieben, wie Sie Daten durchsuchen, die in einer SQL Server-VM in Azure gespeichert sind. Dies kann über eine Datenanalyse mithilfe von SQL oder über die Verwendung einer Programmiersprache wie Python erreicht werden.

Dieser Task ist ein Schritt im [Team Data Science-Prozess](overview.md).

> [!NOTE]
> Bei den SQL-Beispielanweisungen in diesem Dokument wird davon ausgegangen, dass die Daten auf einem SQL Server gespeichert sind. Wenn dies nicht der Fall ist, finden Sie im Cloud Data Science-Prozess Anweisungen zum Verschieben der Daten zu einer SQL Server-Instanz.
> 
> 

## <a name="sql-dataexploration"></a>Durchsuchen von SQL-Daten mit SQL-Skripts
Hier finden Sie einige SQL-Beispielskripts, die zum Durchsuchen von Daten auf einem SQL Server verwendet werden können.

1. Abrufen der Anzahl der Vorkommen pro Tag:
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Abrufen der Ebenen in einer Kategoriespalte:
   
    `select  distinct <column_name> from <databasename>`
3. Abrufen der Anzahl von Ebenen in Kombination zweier kategorischer Spalten: 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Abrufen der Verteilung der numerischen Spalten 
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Für ein praktisches Beispiel können Sie das [NYC Taxi-Dataset](http://www.andresmh.com/nyctaxitrips/) verwenden. Eine vollständige exemplarische Vorgehensweise finden Sie im IPython Notebook-Beispiel unter [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb).
> 
> 

## <a name="python"></a>Durchsuchen von SQL-Daten mit Python
Die Verwendung von Python zum Durchsuchen von Daten und zum Generieren von Funktionen mit Daten in SQL Server ähnelt der Datenverarbeitung in Azure-Blobs mit Python, die unter [Verarbeiten von Azure-Blobdaten in Ihrer Data Science-Umgebung](data-blob.md)beschrieben ist. Die Daten müssen aus der Datenbank in ein Pandas-DataFrame geladen werden, um dann weiter verarbeitet werden zu können. In diesem Abschnitt werden das Herstellen einer Verbindung mit der Datenbank und das Laden der Daten in den DataFrame beschrieben.

Das folgende Format für die Verbindungszeichenfolge kann verwendet werden, um aus Python mit „pyodbc“ eine Verbindung mit einer SQL Server-Datenbank herzustellen (ersetzen Sie „servername“, „dbname“, „username“ und „password“ durch Ihre Daten):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Die [Pandas-Bibliothek](http://pandas.pydata.org/) in Python bietet eine Vielzahl an Datenstrukturen und Datenanalysetools für die Datenbearbeitung zur Python-Programmierung. Der folgende Code liest die aus einer SQL Server-Datenbank zurückgegebenen Ergebnisse in ein Pandas-DataFrame ein:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Sie können nun mit dem Pandas-DataFrame arbeiten. Die Vorgehensweise wird im Thema [Verarbeiten von Azure-Blobdaten mit erweiterter Analyse](data-blob.md) beschrieben.

## <a name="the-team-data-science-process-in-action-example"></a>Der Team Data Science-Prozess in Aktion: Beispiel
Eine lückenlose exemplarische Vorgehensweise zur Verwendung des Team Data Science-Prozesses mit einem öffentlichen Dataset finden Sie unter [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](sql-walkthrough.md).


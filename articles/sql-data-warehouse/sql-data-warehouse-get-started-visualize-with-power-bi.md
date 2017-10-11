---
title: Visualisieren von SQL Data Warehouse-Daten mit Power BI Microsoft Azure
description: Visualisieren von SQL Data Warehouse-Daten mit Power BI
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In diesem Lernprogramm wird gezeigt, wie mit Power BI eine Verbindung mit SQL Data Warehouse und einige grundlegende Visualisierungen erstellen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Lernprogramm durchlaufen möchten, müssen wie folgt vor:

* Eine SQL Data Warehouse, die bereits mit der AdventureWorksDW-Datenbank geladen wird. Um diese bereitzustellen, finden Sie unter [Erstellen eines SQL Data Warehouse] [ Create a SQL Data Warehouse] , und wählen Sie die Beispieldaten zu laden. Wenn Sie bereits ein Datawarehouse aufweisen, aber keine Beispieldaten, können Sie [manuell laden, Beispieldaten][load sample data manually].

## <a name="1-connect-to-your-database"></a>1. Herstellen einer Verbindung mit Ihrer Datenbank
So öffnen Sie Power BI und Herstellen einer Verbindung mit der AdventureWorksDW-Datenbank:

1. Melden Sie sich die [Azure-Portal][Azure portal].
2. Klicken Sie auf **SQL-Datenbanken** , und wählen Sie die AdventureWorks SQL Data Warehouse-Datenbank.
   
    ![Suchen Sie Ihre Datenbank][1]
3. Klicken Sie auf die Schaltfläche 'Open in Power BI' klicken.
   
    ![Power BI-Schaltfläche][2]
4. Die SQL Data Warehouse-Verbindungsseite Anzeigen Ihrer Datenbank Webadresse sollte jetzt angezeigt werden. Klicken Sie auf Weiter.
   
    ![Power BI-Verbindung][3]
5. Geben Sie Ihren Azure-SQL Server-Benutzernamen und Kennwort, und Sie werden vollständig auf die SQL Data Warehouse-Datenbank verbunden werden.
   
    ![Power BI anmelden][4]
6. Sobald Sie in Power BI angemeldet haben, klicken Sie auf die AdventureWorksDW-Dataset auf dem Blatt "left". Dadurch wird die Datenbank geöffnet.
   
    ![Power BI öffnen AdventureWorksDW][5]

## <a name="2-create-a-report"></a>2. Erstellen eines Berichts
Sie können nun Power BI verwenden, um die AdventureWorksDW-Beispieldaten zu analysieren. Zum Durchführen der Analyse wurde AdventureWorksDW eine Ansicht mit der AggregateSales. In dieser Ansicht enthält einige der wichtigsten Metriken für den Umsatz des Unternehmens analysieren.

1. Um eine Zuordnung der Betrag der Verkäufe nach Postleitzahl, im Bereich rechts Felder zu erstellen, klicken Sie auf die AggregateSales-Ansicht, um ihn zu erweitern. Klicken Sie auf die Spalten PostalCode und "SalesAmount", um sie auszuwählen.
   
    ![Wählen Sie Power BI AggregateSales][6]
   
    Power BI erkennt automatisch, dies ist der geographische Daten enthalten, und fügen Sie ihn in eine Zuordnung für Sie.
   
    ![Power BI-Karte][7]
2. Dieser Schritt erstellt ein Balkendiagramm, in der Betrag der Verkäufe pro Einkommen der Kunden angezeigt. Wechseln Sie zur erweiterten Ansicht AggregateSales dazu zu erstellen. Klicken Sie auf das Feld "SalesAmount". Ziehen Sie das Einkommen der Kunden-Feld auf der linken Seite, und legen Sie sie in der Achse.
   
    ![Wählen Sie Power BI-Achse][8]
   
    Wir haben das Balkendiagramm über links verschoben.
   
    ![Power BI-Leiste][9]
3. Dieser Schritt erstellt ein Liniendiagramm, in der Umsatz pro Bestelldatum angezeigt. Wechseln Sie zur erweiterten Ansicht AggregateSales dazu zu erstellen. Klicken Sie auf "SalesAmount" und OrderDate. Klicken Sie in den Visualisierungen Spalte auf das Symbol "Liniendiagramm". Dies ist das erste Symbol in der zweiten Zeile unter Visualisierungen.
   
    ![Select Liniendiagramm für Power BI][10]
   
    Sie haben jetzt einen Bericht mit drei verschiedenen Visualisierungen der Daten.
   
    ![Power BI-Befehlszeile][11]

Sie können den Status zu einem beliebigen Zeitpunkt speichern, indem Sie auf **Datei** auswählen und **speichern**.

## <a name="next-steps"></a>Nächste Schritte
Nun, dass es einige Zeit mit den Beispieldaten Aufwärmen gewährt haben, finden Sie unter Vorgehensweise [entwickeln][develop], [laden][load], oder [migrieren][migrate]. Oder sehen Sie sich die [Power BI-Website][Power BI website].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/

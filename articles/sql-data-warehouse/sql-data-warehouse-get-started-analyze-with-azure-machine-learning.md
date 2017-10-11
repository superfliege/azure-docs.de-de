---
title: Analysieren von Daten mit Azure Machine Learning | Microsoft Docs
description: Stellen Sie mithilfe von Azure Machine Learning eine predictive Machine learning-Modells, die basierend auf in Azure SQL Data Warehouse gespeicherten Daten erstellen.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3197948e32fe5c95b111fe5495a0e5f85966a24b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a>Analysieren von Daten mit Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Dieses Lernprogramm verwendet Azure Machine Learning, um eine predictive Machine learning-Modells, die basierend auf in Azure SQL Data Warehouse gespeicherten Daten erstellen. Insbesondere erstellt dies durch die Vorhersage, ob ein Kunde wahrscheinlich ein Fahrrad kaufen, davon wird eine gezielte Marketingkampagne für Adventure Works, das Fahrrad kaufen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Lernprogramm durchlaufen möchten, müssen wie folgt vor:

* Ein SQL Data Warehouse mit Beispieldaten AdventureWorksDW vorab geladen. Um diese bereitzustellen, finden Sie unter [Erstellen eines SQL Data Warehouse] [ Create a SQL Data Warehouse] , und wählen Sie die Beispieldaten zu laden. Wenn Sie bereits ein Datawarehouse aufweisen, aber keine Beispieldaten, können Sie [manuell laden, Beispieldaten][load sample data manually].

## <a name="1-get-the-data"></a>1. Abrufen von Daten
Die Daten sind in der Ansicht dbo.vTargetMail in der AdventureWorksDW-Datenbank. Diese Daten zu lesen:

1. Melden Sie sich [Azure Machine Learning Studio] [ Azure Machine Learning studio] , und klicken Sie auf Meine Experimente.
2. Klicken Sie auf **+ neu** , und wählen Sie **leere Experiment**.
3. Geben Sie einen Namen für Ihr Experiment: Marketing vorgesehen.
4. Ziehen Sie die **Reader** Modul aus dem Bereich Module in der Canvas.
5. Geben Sie die Details Ihrer SQL Data Warehouse-Datenbank, in dem Bereich "Eigenschaften".
6. Geben Sie die Datenbank **Abfrage** zum Lesen der Daten von Interesse sind.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Führen Sie das Experiment, indem Sie auf **ausführen** unter den experimentbereich.
![Führen Sie das experiment][1]

Nachdem Sie das Experiment erfolgreich zu Ende ausgeführt, klicken Sie auf den Ausgabeport am unteren Rand der Reader-Modul, und wählen **visualisieren** um die importierten Daten anzuzeigen.
![Zeigen Sie importierte Daten][3]

## <a name="2-clean-the-data"></a>2. Die Daten zu bereinigen
Um die Daten zu bereinigen, löschen Sie einige Spalten, die nicht für das Modell relevant sind. Dazu gehen Sie folgendermaßen vor:

1. Ziehen Sie die **Project Columns** -Modul in der Canvas.
2. Klicken Sie auf **starten Spaltenauswahl** im Bereich Eigenschaften an, welche Spalten Sie löschen möchten.
   ![Project-Spalten][4]
3. Zwei Spalten ausschließen: CustomerAlternateKey und GeographyKey.
   ![Entfernen Sie keine unnötige Spalten][5]

## <a name="3-build-the-model"></a>3. Erstellen des Modells
Wir werden die Daten 80 bis 20 aufzuteilen: 80 % und Trainieren ein Machine learning-Modell und 20 % zum Testen des Modells. Stellen wir der Algorithmen "Two-Class" für diese binäre klassifizierungsproblem verwenden.

1. Ziehen Sie die **Teilung** -Modul in der Canvas.
2. Geben Sie 0,8 für den Anteil der Zeilen in das erste ausgabedataset im Bereich "Eigenschaften" aus.
   ![Daten in Trainings-und Testsatz aufteilen][6]
3. Ziehen Sie die **Two-Class Boosted Decision Tree** -Modul in der Canvas.
4. Ziehen Sie die **Train Model** -Modul in die Zeichenfläche und legen Sie die Eingaben. Klicken Sie auf **starten Spaltenauswahl** im Bereich "Eigenschaften".
   * Erste Eingabe: ML-Algorithmus.
   * Zweite Eingabe: Daten, die den Algorithmus trainiert.
     ![Verbinden Sie das Modul "Train Model"][7]
5. Wählen Sie die **BikeBuyer** Spalte als vorherzusagenden Spalte.
   ![Vorherzusagende Spalte auswählen][8]

## <a name="4-score-the-model"></a>4. Bewerten Sie das Modell
Jetzt werden wir testen, wie das Modell für Testdaten ausführt. Vergleichen wir den Algorithmus unsere Wahl mit einem anderen Algorithmus, um anzuzeigen, die eine bessere Leistung bietet.

1. Ziehen Sie **Score Model** -Modul in der Canvas.
    Erste Eingabe: zweite Eingabe Modell trainiert: Testdaten ![Bewerten des Modells][9]
2. Ziehen Sie die **Two-Class Bayes Point Machine** in den experimentbereich. Wir werden verglichen, wie dieser Algorithmus im Vergleich zu Two-Class Boosted Decision Tree ausführt.
3. Kopieren Sie die Module Train Model "und" Score Model im Zeichenbereich.
4. Ziehen Sie die **Evaluate Model** -Modul in den Zeichenbereich, der zwei Algorithmen verglichen werden soll.
5. **Führen Sie** Experiment.
   ![Führen Sie das experiment][10]
6. Klicken Sie auf den Ausgabeport am unteren Rand des Evaluate Model-Moduls, und klicken Sie auf visualisieren.
   ![Visualisieren der Ergebnisse][11]

Die Metriken sind die ROC-Kurven Genauigkeit Rückruf Diagramm, und heben Sie die Kurve. Diese Metriken liefern können, wir sehen Sie sich, dass das erste Modell eine bessere Leistung als das zweite Argument ausgeführt. Betrachten, was das erste Modell vorhergesagt hat, klicken Sie auf den Ausgabeport des Modells Bewertung und klicken Sie auf visualisieren.
![Visualisieren der Ergebnisse der Bewertung][12]

Sehen Sie zwei weitere Spalten hinzugefügt, um das testdataset ein.

* Bewerteter Wahrscheinlichkeiten: die Wahrscheinlichkeit, dass ein Kunde ein Fahrrad kaufen wird.
* Bewertete Bezeichnungen: die Klassifizierung geschieht, indem das Modell – Bike Buyer (1) oder nicht (0). Diese wahrscheinlichkeitsschwellenwert für das bezeichnen ist auf 50 % festgelegt und angepasst werden kann.

Vergleichen die Spalte BikeBuyer (tatsächlich) mit der Scored Labels (Vorhersage), können Sie sehen, wie gut das Modell ausgeführt hat. Als nächsten Schritt können Sie dieses Modell verwenden, um Vorhersagen für neue Kunden und dieses Modell als Webdienst veröffentlichen oder Ergebnisse zurück in SQL Data Warehouse geschrieben.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Erstellen von vorhersehbaren Machine learning-Modellen finden Sie unter [Einführung in Machine Learning in Azure][Introduction to Machine Learning on Azure].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction to Machine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

---
title: "Vorbereiten von Daten zum Tutorial „Klassifizieren von Iris“ in Azure Machine Learning-Diensten (Vorschau) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschau) verwendet werden können. Dies ist der erste Teil, in dem die Datenvorbereitung beschrieben wird."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/28/2017
ms.openlocfilehash: fcef59d38cce5f8860508ebeeda0b1b97f5e83f1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="classify-iris-part-1-prepare-the-data"></a>Klassifizieren von Iris – Teil 1: Vorbereiten der Daten
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der erste Teil einer dreiteiligen Reihe. In diesem Tutorial werden die Grundlagen von Azure Machine Learning-Diensten (Vorschau) Schritt für Schritt beschrieben. Folgendes wird vermittelt:
> [!div class="checklist"]
> * Erstellen eines Projekts in Azure Machine Learning Workbench
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark-Code zum Aufrufen eines Datenvorbereitungspakets

In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)) verwendet. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für Mac OS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
- Erstellen eines Azure Machine Learning-Experimentieren-Kontos
- Installieren von Azure Machine Learning Workbench

Sie können die Anweisungen im Artikel mit der [Schnellstartanleitung zur Erstellung und Installation](quickstart-installation.md) befolgen, um die Azure Machine Learning Workbench-Anwendung zu installieren. Diese Installation enthält auch das plattformübergreifende Azure-Befehlszeilentool (Azure CLI).

## <a name="create-a-new-project-in-azure-machine-learning-workbench"></a>Erstellen eines neuen Projekts in Azure Machine Learning Workbench
1. Öffnen Sie die Azure Machine Learning Workbench-App, und melden Sie sich bei Bedarf an. Wählen Sie im Bereich **PROJEKTE** das Pluszeichen (**+**), um ein **Neues Projekt** zu erstellen.

   ![Neuer Arbeitsbereich](media/tutorial-classifying-iris/new_ws.png)

2. Geben Sie die Details für **Neues Projekt erstellen** an: 

   ![Neues Projekt](media/tutorial-classifying-iris/new_project.png)

   - Geben Sie im Feld **Projektname** den Namen für das Projekt ein. Verwenden Sie beispielsweise den Wert **MyIris**.
   - Wählen Sie das **Projektverzeichnis** aus, in dem das Projekt erstellt wird. Verwenden Sie beispielsweise den Wert `C:\Temp\`. 
   - Geben Sie die **Projektbeschreibung** ein (optional). 
   - Das Feld **Git-Repository** ist ebenfalls optional und kann leer bleiben. Sie können ein vorhandenes leeres Git-Repository (ein Repository ohne Hauptverzweigung) in Visual Studio Team Services bereitstellen. Bei Verwendung eines bereits vorhandenen Git-Repositorys können Sie die Szenarien für das Roaming und die Freigabe später aktivieren. Weitere Informationen finden Sie unter [Verwenden von Git-Repository mit einem Azure Machine Learning Workbench-Projekt](using-git-ml-project.md). 
   - Wählen Sie einen **Arbeitsbereich** aus. In diesem Tutorial wird beispielsweise **IrisGarden** verwendet. 
   - Wählen Sie die **Classifying Iris (Iris Klassifizieren)**-Vorlage aus der Projektvorlage aus. 

3. Wählen Sie die Schaltfläche **Erstellen**. Das Projekt ist jetzt erstellt und für Sie geöffnet.

## <a name="create-a-data-preparation-package"></a>Erstellen eines Datenvorbereitungspakets
1. Öffnen Sie in der **Dateiansicht** die Datei **iris.csv**. Die Datei ist eine Tabelle mit 5 Spalten und 150 Zeilen. Sie verfügt über vier numerische Feature-Spalten und eine Zeichenfolge für die Zielspalte. Sie verfügt nicht über Spaltenheader.

   ![Iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Fügen Sie keine Datendateien in den Projektordner ein. Dies gilt insbesondere dann, wenn es sich um große Dateien handelt. Wir verwenden die Datei **iris.csv** zu Demonstrationszwecken in dieser Vorlage, da sie sehr klein ist. Weitere Informationen finden Sie unter [Beibehalten von Änderungen und Arbeiten mit großen Dateien](how-to-read-write-files.md).

2. Wählen Sie unter **Datenansicht** das Pluszeichen (**+**), um eine neue Datenquelle hinzuzufügen. Die Seite **Datenquelle hinzufügen** wird geöffnet. 

   ![Datenansicht](media/tutorial-classifying-iris/data_view.png)

3. Behalten Sie die Standardwerte bei, und wählen Sie die Schaltfläche **Weiter**.  
 
   ![Auswählen von „iris“](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Achten Sie darauf, dass Sie für diese Übung die Datei **iris.csv** aus dem aktuellen Projektverzeichnis auswählen. Sonst kann es sein, dass die nachfolgenden Schritte fehlschlagen.
   
4. Klicken Sie nach dem Auswählen der Datei auf die Schaltfläche **Fertig stellen**.

4. Eine neue Datei mit dem Namen **iris-1.dsource** wird erstellt. Die Datei ist mit dem Zusatz „-1“ eindeutig benannt, da das Beispielprojekt bereits über eine nicht nummerierte Datei **iris.dsource** verfügt.  

   Die Datei wird geöffnet, und die Daten werden angezeigt. Diesem Dataset werden automatisch mehrere Spaltenüberschriften von **Column1** bis **Column5** hinzugefügt. Scrollen Sie nach unten, und beachten Sie, dass die letzte Zeile des Datasets leer ist. Die Zeile ist leer, weil die CSV-Datei einen zusätzlichen Zeilenumbruch enthält.

   ![Iris-Datenansicht](media/tutorial-classifying-iris/iris_data_view.png)

5. Wählen Sie die Schaltfläche **Metriken**. Sehen Sie sich die Histogramme an. Für jede Spalte wurde ein vollständiger Satz mit Statistiken berechnet. Sie können auch die Schaltfläche **Daten** wählen, um die Daten erneut anzuzeigen. 

   ![Iris-Datenansicht](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Wählen Sie die Schaltfläche **Vorbereiten**. Das Dialogfeld **Vorbereiten** wird geöffnet. 

   Das Beispielprojekt enthält die Datei **iris.dprep**. Standardmäßig werden Sie aufgefordert, im bereits vorhandenen Datenvorbereitungspaket **iris.dprep** einen neuen Datenfluss zu erstellen. 

   Wählen Sie im Dropdownmenü die Option **+ New Data Preparation Package** (+ Neues Datenvorbereitungspaket), geben Sie einen neuen Wert als Paketnamen an, verwenden Sie **iris-1**, und wählen Sie anschließend **OK**.

   ![Iris-Datenansicht](media/tutorial-classifying-iris/new_dprep.png)

   Ein neues Datenvorbereitungspaket mit dem Namen **iris-1.dprep** wird erstellt und im Editor für die Vorbereitung von Daten geöffnet.

7. Nun führen wir einige einfache Datenvorbereitungsaufgaben durch. Benennen Sie die Spaltennamen um. Wählen Sie jeweils eine Spaltenüberschrift aus, damit der Text der Überschrift bearbeitet werden kann. 

   Geben Sie für die fünf Spalten **Sepal Length** (Länge des Kelchblatts), **Sepal Width** (Breite des Kelchblatts), **Petal Length** (Länge des Kronblatts), **Petal Width** (Breite des Kronblatts) bzw. **Species** (Art) ein.

   ![Umbenennen der Spalten](media/tutorial-classifying-iris/rename_column.png)

8. Wählen Sie zum Zählen von unterschiedlichen Werten die Spalte **Species** aus, und klicken Sie mit der rechten Maustaste darauf, um sie zu markieren. Wählen Sie im Dropdownmenü die Option **Value Counts** (Wertanzahl). 

   ![Auswählen der Wertanzahl](media/tutorial-classifying-iris/value_count.png)

   Der Bereich **Inspektoren** wird geöffnet, und es wird ein Histogramm mit vier Balken angezeigt. Die Zielspalte enthält die drei unterschiedlichen Werte **Iris_virginica**, **Iris_versicolor** und **Iris-setosa** und zusätzlich den Wert **(null)**.

9. Wählen Sie zum Herausfiltern von NULL-Werten den Balken des Graphen aus, der für den NULL-Wert steht. Es ist eine Zeile mit dem Wert **(null)** vorhanden. Wählen Sie das Minuszeichen (**-**), um diese Zeile zu entfernen.

   ![Histogramm mit Wert-Anzahl](media/tutorial-classifying-iris/filter_out.png)

10. Beachten Sie die einzelnen Schritte im Bereich **SCHRITTE**. Als Sie die Spalten umbenannt und die Zeilen mit den NULL-Werten gefiltert haben, wurde jede Aktion als Datenvorbereitungsschritt aufgezeichnet. Sie können die einzelnen Schritte bearbeiten, um die Einstellungen anzupassen, und die Schritte neu anordnen und entfernen.

   ![Schritte](media/tutorial-classifying-iris/steps.png)

11. Schließen Sie den Editor für die Vorbereitung von Daten. Wählen Sie auf der Registerkarte **iris-1** mit dem Graphsymbol die Option **Schließen** (x). Ihre Arbeit wird automatisch in der Datei **iris-1.dprep** unter der Überschrift **Data Preparations** (Datenvorbereitungen) gespeichert.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Generieren von Python/PySpark-Code zum Aufrufen eines Datenvorbereitungspakets

1. Klicken Sie mit der rechten Maustaste auf die Datei **Iris-1.dprep**, um das Kontextmenü anzuzeigen, und wählen Sie dann die Option **Generate Data Access Code File** (Datenzugriffs-Codedatei generieren). 

   ![Generieren von Code](media/tutorial-classifying-iris/generate_code.png)

2. Die neue Datei **iris-1.py** mit den folgenden Codezeilen wird geöffnet:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   Dieser Codeausschnitt ruft die Logik auf, die Sie als Datenvorbereitungspaket erstellt haben. Je nach Kontext, in dem dieser Code ausgeführt wird, kann `df` für die unterschiedliche Arten von Datenrahmen stehen. Ein [pandas-Datenrahmen](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) wird verwendet, wenn der Code in Python-Laufzeit ausgeführt wird, und ein [Spark-Datenrahmen](https://spark.apache.org/docs/latest/sql-programming-guide.html) wird verwendet, wenn die Ausführung in einem Spark-Kontext erfolgt. 

   Weitere Informationen zum Vorbereiten von Daten in Azure Machine Learning Workbench finden Sie im Leitfaden [Erste Schritte bei der Datenvorbereitung](data-prep-getting-started.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem ersten Teil der dreiteiligen Tutorial-Reihe wurde beschrieben, wie Sie Azure Machine Learning Workbench für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Erstellen eines neuen Projekts 
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark-Code zum Aufrufen eines Datenvorbereitungspakets

Sie können nun mit dem nächsten Teil der Reihe fortfahren, in dem beschrieben wird, wie Sie ein Azure Machine Learning-Modell erstellen:
> [!div class="nextstepaction"]
> [Erstellen eines Modells](tutorial-classifying-iris-part-2.md)

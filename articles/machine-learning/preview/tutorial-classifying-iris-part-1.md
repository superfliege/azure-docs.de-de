---
title: "Aufbereiten von Daten zum Tutorial „Klassifizieren von Iris“ in Azure Machine Learning-Diensten (Vorschau) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie Azure Machine Learning-Dienste (Vorschau) konsistent verwendet werden können. Dies ist der erste Teil, in dem die Datenaufbereitung beschrieben wird."
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/28/2018
ms.openlocfilehash: 0bef557ee1394e3c786fd2c54e821b5dea28fabf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-classify-iris-part-1---preparing-the-data"></a>Tutorial: Klassifizieren von Iris – Teil 1: Vorbereiten der Daten

Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten aufbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der erste Teil einer dreiteiligen Reihe. In diesem Tutorial werden die Grundlagen von Azure Machine Learning-Diensten (Vorschau) Schritt für Schritt beschrieben. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Projekts in Azure Machine Learning Workbench
> * Erstellen eines Datenaufbereitungspakets
> * Generieren von Python/PySpark-Code zum Aufrufen eines Datenaufbereitungspakets

In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)) verwendet. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für macOS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für dieses Tutorial benötigen Sie Folgendes:
- Ein Azure Machine Learning-Experimentieren-Konto
- Installation von Azure Machine Learning Workbench

Falls Sie noch nicht über diese Komponenten verfügen, können Sie die Schritte im Artikel [Erstellen von Vorschaukonten für Azure Machine Learning und Installieren von Azure Machine Learning Workbench](quickstart-installation.md) ausführen, um dieses Konto einzurichten und die Azure Machine Learning Workbench-Anwendung zu installieren. 

## <a name="create-a-new-project-in-workbench"></a>Erstellen eines neuen Projekts in Workbench

Wenn Sie die Schritte im Artikel [Erstellen von Vorschaukonten für Azure Machine Learning und Installieren von Azure Machine Learning Workbench](quickstart-installation.md) ausgeführt haben, sollte dieses Projekt bereits vorhanden sein. In diesem Fall können Sie zum nächsten Abschnitt springen.

1. Öffnen Sie die Azure Machine Learning Workbench-App, und melden Sie sich bei Bedarf an. 
   
   + Unter Windows starten Sie sie mithilfe der Desktopverknüpfung **Machine Learning Workbench**. 
   + Unter macOS wählen Sie im Launchpad **Azure ML Workbench**.

1. Wählen Sie im Bereich **PROJEKTE** das Pluszeichen (+) und dann **Neues Projekt**.  

   ![Neuer Arbeitsbereich](media/tutorial-classifying-iris/new_ws.png)

1. Füllen Sie die Formularfelder aus, und wählen Sie die Schaltfläche **Erstellen**, um in der Workbench ein neues Projekt zu erstellen.

   Feld|Vorgeschlagener Wert für Tutorial|Beschreibung
   ---|---|---
   Projektname | myIris |Geben Sie einen eindeutigen Namen ein, der Ihr Konto identifiziert. Sie können Ihren eigenen Namen verwenden oder einen Abteilungs- oder Projektnamen, der am besten zu dem Experiment passt. Der Name sollte 2 bis 32 Zeichen lang sein. Es sind nur alphanumerische Zeichen und der Bindestrich (-) zulässig. 
   Projektverzeichnis | c:\Temp\ | Geben Sie das Verzeichnis an, in dem das Projekt erstellt wird.
   Projektbeschreibung | _nicht ausfüllen_ | Optionales Feld für eine Beschreibung der Projekte.
   Visualstudio.com |_nicht ausfüllen_ | Optionales Feld. Zur Quellcodeverwaltung und Zusammenarbeit kann ein Projekt optional einem Git-Repository in Visual Studio Team Services zugeordnet werden. [Hier erfahren Sie, wie Sie das Verfahren einrichten](https://docs.microsoft.com/en-us/azure/machine-learning/preview/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Arbeitsbereich | IrisGarden (falls vorhanden) | Wählen Sie einen Arbeitsbereich aus, den Sie im Azure-Portal für Ihr Experimentieren-Konto erstellt haben. <br/>Wenn Sie nach dem Schnellstart vorgegangen sind, sollten Sie einen Arbeitsbereich namens „IrisGarden“ haben. Wenn dies nicht der Fall ist, wählen Sie den Arbeitsbereich aus, den Sie bei der Erstellung Ihres Experimentieren-Kontos erstellt haben, oder einen anderen Arbeitsbereich, den Sie verwenden möchten.
   Projektvorlage | Klassifizieren von Schwertlilien | Vorlagen enthalten Skripts und Daten, die Sie zum Erkunden des Produkts verwenden können. Diese Vorlage enthält die benötigten Skripts und Daten für diesen Schnellstart und andere Tutorials auf dieser Dokumentationswebsite. 

   ![Neues Projekt](media/tutorial-classifying-iris/new_project.png)
 
 Ein neues Projekt wird erstellt, und das Projektdashboard wird mit diesem Projekt geöffnet. Nun können Sie die Projektstartseite, Datenquellen, Notebooks und Quellcodedateien untersuchen. 

## <a name="create-a-data-preparation-package"></a>Erstellen eines Datenaufbereitungspakets

In diesem Teil des Tutorials erkunden Sie die Daten und starten den Datenaufbereitungsprozess. Wenn Sie Ihre Daten in Azure Machine Learning Workbench vorbereiten, wird eine JSON-Darstellung der Transformationen, die Sie in Workbench durchführen, in einem lokalen Datenaufbereitungspaket (DPREP-Datei) gespeichert. Dieses Datenaufbereitungspaket ist der primäre Container für Ihre Datenaufbereitungsmaßnahmen in Workbench.

Dieses Datenaufbereitungspaket kann für die Ausführung zur Laufzeit weitergegeben werden, z.B. „local-C#/CoreCLR“, „Scala/Spark“ oder „Scala/HDI“. Hierbei wird Code für die entsprechende Laufzeit zur Ausführung generiert. 

1. Wählen Sie das Ordnersymbol aus, um die Dateiansicht zu öffnen, und wählen Sie anschließend **iris.csv**, um diese Datei zu öffnen.  

   Die Datei ist eine Tabelle mit 5 Spalten und 150 Zeilen. Sie verfügt über vier numerische Feature-Spalten und eine Zeichenfolge für die Zielspalte. Sie verfügt nicht über Spaltenheader.

   ![Iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Fügen Sie keine Datendateien in den Projektordner ein. Dies gilt insbesondere dann, wenn es sich um große Dateien handelt. Wir verwenden die Datei **iris.csv** zu Demonstrationszwecken in dieser Vorlage, da sie sehr klein ist. Weitere Informationen finden Sie unter [Beibehalten von Änderungen und Arbeiten mit großen Dateien](how-to-read-write-files.md).

2. Wählen Sie unter **Datenansicht** das Pluszeichen (**+**), um eine neue Datenquelle hinzuzufügen. Die Seite **Datenquelle hinzufügen** wird geöffnet. 

   ![Datenansicht](media/tutorial-classifying-iris/data_view.png)

3. Wählen Sie **Textdateien (*.csv, .json, .txt.,...)**, und klicken Sie auf **Weiter**.
   ![Datenquelle](media/tutorial-classifying-iris/data-source.png)
   

4. Navigieren Sie zur Datei **iris.csv**, und klicken Sie auf **Weiter**.  
 
   ![Auswählen von „iris“](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Achten Sie darauf, dass Sie für diese Übung die Datei **iris.csv** aus dem aktuellen Projektverzeichnis auswählen. Sonst kann es sein, dass die nachfolgenden Schritte fehlschlagen.
   
5. Übernehmen Sie die Standardwerte, und klicken Sie auf **Fertig stellen**.

6. Eine neue Datei mit dem Namen **iris-1.dsource** wird erstellt. Die Datei ist mit dem Zusatz „-1“ eindeutig benannt, da das Beispielprojekt bereits über eine nicht nummerierte Datei **iris.dsource** verfügt.  

   Die Datei wird geöffnet, und die Daten werden angezeigt. Diesem Dataset werden automatisch mehrere Spaltenüberschriften von **Column1** bis **Column5** hinzugefügt. Scrollen Sie nach unten, und beachten Sie, dass die letzte Zeile des Datasets leer ist. Die Zeile ist leer, weil die CSV-Datei einen zusätzlichen Zeilenumbruch enthält.

   ![Iris-Datenansicht](media/tutorial-classifying-iris/iris_data_view.png)

7. Wählen Sie die Schaltfläche **Metriken**. Sehen Sie sich die Histogramme an. Für jede Spalte wurde ein vollständiger Satz mit Statistiken berechnet. Sie können auch auf die Schaltfläche **Daten** klicken, um die Daten erneut anzuzeigen. 

   ![Iris-Datenansicht](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Wählen Sie die Schaltfläche **Vorbereiten**. Das Dialogfeld **Vorbereiten** wird geöffnet. 

   Das Beispielprojekt enthält die Datei **iris.dprep**. Standardmäßig werden Sie aufgefordert, im bereits vorhandenen Datenaufbereitungspaket **iris.dprep** einen neuen Datenfluss zu erstellen. 

   Wählen Sie im Dropdownmenü die Option **+ New Data Preparation Package** (+ Neues Datenaufbereitungspaket), geben Sie einen neuen Wert als Paketnamen an, verwenden Sie **iris-1**, und wählen Sie anschließend **OK**.

   ![Iris-Datenansicht](media/tutorial-classifying-iris/new_dprep.png)

   Ein neues Datenaufbereitungspaket mit dem Namen **iris-1.dprep** wird erstellt und im Editor für die Aufbereitung von Daten geöffnet.

9. Nun führen wir einige einfache Datenaufbereitungsaufgaben durch. Wählen Sie die einzelnen Spaltenüberschriften aus, damit der Text der Überschrift bearbeitet werden kann, und benennen Sie die Spalten wie folgt um: 

   Geben Sie für die fünf Spalten der Reihenfolge nach **Sepal Length** (Länge des Kelchblatts), **Sepal Width** (Breite des Kelchblatts), **Petal Length** (Länge des Kronblatts), **Petal Width** (Breite des Kronblatts) bzw. **Species** (Art) ein.

   ![Umbenennen der Spalten](media/tutorial-classifying-iris/rename_column.png)

10. Wählen Sie zum Zählen von unterschiedlichen Werten die Spalte **Species** aus, und klicken Sie mit der rechten Maustaste darauf, um sie zu markieren. Wählen Sie im Dropdownmenü die Option **Value Counts** (Wertanzahl). 

   Mit dieser Aktion wird der Bereich **Inspectors** (Inspektoren) unterhalb der Daten geöffnet. Ein Histogramm mit vier Balken wird angezeigt. Die Zielspalte enthält die drei unterschiedlichen Werte **Iris_virginica**, **Iris_versicolor** und **Iris-setosa** und zusätzlich den Wert **(null)**.

   ![Auswählen der Wertanzahl](media/tutorial-classifying-iris/value_count.png)

11. Wählen Sie zum Herausfiltern der NULL-Werte die Option „Null“ und das Minuszeichen (**-**) aus. Die Zeile „Null“ wird ausgegraut, um anzuzeigen, dass sie herausgefiltert wurde. 

   ![Histogramm mit Wertanzahl](media/tutorial-classifying-iris/filter_out.png)

12. Beachten Sie die einzelnen Schritte im Bereich **SCHRITTE**. Als Sie die Spalten umbenannt und die Zeilen mit den NULL-Werten gefiltert haben, wurde jede Aktion als Datenaufbereitungsschritt aufgezeichnet. Sie können die einzelnen Schritte bearbeiten, um die Einstellungen anzupassen, und die Schritte neu anordnen und entfernen.

   ![Schritte](media/tutorial-classifying-iris/steps.png)

13. Schließen Sie den Editor für die Aufbereitung von Daten. Wählen Sie auf der Registerkarte **iris-1** mit dem Graphsymbol die Option **Schließen** (x). Ihre Arbeit wird automatisch in der Datei **iris-1.dprep** unter der Überschrift **Data Preparations** (Datenaufbereitungen) gespeichert.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Generieren von Python/PySpark-Code zum Aufrufen eines Datenaufbereitungspakets

<!-- The output/results of a Package can be explored in Python or via a Jupyter Notebook. A Package can be executed across multiple runtimes including local Python, Spark (including in Docker), and HDInsight. A Package contains one or more Dataflows that are the steps and transforms applied to the data. A Package may use another Package as a Data Source (referred to as a Reference Data Flow). -->

1. Suchen Sie auf der Registerkarte „Data Preparations“ (Datenaufbereitungen) nach der Datei **iris-1.dprep**.

1. Klicken Sie mit der rechten Maustaste auf die Datei **Iris-1.dprep**, und wählen Sie im Kontextmenü dann die Option **Generate Data Access Code File** (Datenzugriffs-Codedatei generieren). 

   ![Generieren von Code](media/tutorial-classifying-iris/generate_code.png)

   Eine neue Datei mit dem Namen **iris-1.py** wird mit den folgenden Codezeilen geöffnet, um die Logik aufzurufen, die Sie als Datenaufbereitungspaket erstellt haben:

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

   Je nach Kontext, in dem dieser Code ausgeführt wird, kann `df` für die unterschiedliche Arten von Datenrahmen stehen. Ein [pandas-Datenrahmen](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) wird verwendet, wenn der Code in Python-Laufzeit ausgeführt wird, und ein [Spark-Datenrahmen](https://spark.apache.org/docs/latest/sql-programming-guide.html) wird verwendet, wenn die Ausführung in einem Spark-Kontext erfolgt. 
   
   Weitere Informationen zum Aufbereiten von Daten in Azure Machine Learning Workbench finden Sie im Leitfaden [Erste Schritte bei der Datenaufbereitung](data-prep-getting-started.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure Machine Learning Workbench für folgende Zwecke verwendet:
> [!div class="checklist"]
> * Erstellen eines neuen Projekts
> * Erstellen eines Datenaufbereitungspakets
> * Generieren von Python/PySpark-Code zum Aufrufen eines Datenaufbereitungspakets

Sie können nun mit dem nächsten Teil der Tutorialreihe fortfahren, in dem beschrieben wird, wie Sie ein Azure Machine Learning-Modell erstellen:
> [!div class="nextstepaction"]
> [Tutorial 2: Erstellen von Modellen](tutorial-classifying-iris-part-2.md)

---
title: Vorbereiten von Daten zum Klassifizieren von Iris-Tutorial in Machine Learning-Diensten (Vorschau) | Microsoft-Dokumentation
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschau) verwendet werden können. Dies ist Teil 1 zur Datenvorbereitung."
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
ms.openlocfilehash: 975a86c1f9d9692f6eadd232177f33cbbbeeff2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="classifying-iris-part-1-prepare-data"></a>Klassifizieren von Iris Teil 1: Vorbereiten von Daten
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der erste Teil einer dreiteiligen Serie. In diesem Tutorial betrachten wir gemeinsam die Grundlagen von Machine Learning-Diensten (Vorschau). In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Projekts in der Azure Machine Learning Workbench
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

Dieses Tutorial verwendet den timeless [Iris flower dataset (Iris Blume Dataset)](https://en.wikipedia.org/wiki/Iris_flower_data_set), um die Dinge einfach zu halten. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für macOS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
- Erstellen eines Azure Machine Learning-Experimentieren-Kontos
- Installieren der Azure Machine Learning Workbench

Sie können die [Schnellstartanleitung für die Installation](quickstart-installation.md) verwenden, um die Azure Machine Learning Workbench-Anwendung zu installieren. Diese Installation umfasst auch die Befehlszeilenschnittstelle (CLI).

## <a name="create-a-new-project-in-azure-ml-workbench"></a>Erstellen eines neuen Projekts in Azure ML Workbench
1. Starten Sie die Azure Machine Learning Workbench-App, und melden Sie sich bei Bedarf an. Im **PROJEKTE**-Bereich, klicken Sie auf das **+**-Symbol, um ein **Neues Projekt** zu erstellen.

   ![Neuer Arbeitsbereich](media/tutorial-classifying-iris/new_ws.png)

2. Füllen Sie die Details zum **Erstellen eines neuen Projekts** aus. 

   ![Neues Projekt](media/tutorial-classifying-iris/new_project.png)

   - Füllen Sie das **Projektname**-Feld mit einem Namen für das Projekt aus. Verwenden Sie beispielsweise den Wert **MyIris**.
   - Wählen Sie das **Projektverzeichnis** aus, in dem das Projekt erstellt wird. Verwenden Sie beispielsweise den Wert **C:\Temp**. 
   - Geben Sie die **Projektbeschreibung** ein. 
   - Das **Git-Repository**-Feld ist optional und kann leer bleiben. Sie können ein vorhandenes leeres Git-Repository (ein Repository ohne Hauptverzweigung) auf VSTS bereitstellen. Auf diese Weise können Sie Roaming- und in weiterer Folge Sharing-Szenarios aktivieren. Weitere Informationen konsultieren Sie den [Using Git repo (Verwenden von Git-Repository)](using-git-ml-project.md)-Artikel. 
   - Wählen Sie einen **Arbeitsbereich** aus, dieses Tutorial verwendet z.B.**IrisGarden**. 
   - Wählen Sie die **Classifying Iris (Iris Klassifizieren)**-Vorlage aus der Projektvorlage aus. 

3. Klicken Sie auf die Schaltfläche **Erstellen**, um das Projekt zu erstellen. Das Projekt ist jetzt erstellt und für Sie geöffnet.

## <a name="create-a-data-preparation-package"></a>Erstellen eines Datenvorbereitungspakets
1. Öffnen Sie in der **Dateiansicht** die Datei **iris.csv**. Die Datei ist eine einfache Tabelle mit 5 Spalten und 150 Zeilen. Sie verfügt über vier numerische Feature-Spalten und eine Zeichenfolge für die Zielspalte. Sie verfügt nicht über Spaltenheader.

   ![Iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Es wird nicht empfohlen, Datendateien in den Projektordner einzufügen. Dies gilt insbesondere dann, wenn es sich um große Dateien handelt. Wir verwenden die Datei **iris.csv** zu Demonstrationszwecken in dieser Vorlage, da sie sehr klein ist. Für weitere Informationen konsultieren Sie den [How to read and write large data files (Wie Sie große Datendateien lesen und schreiben)](how-to-read-write-files.md)-Artikel.

2. In der **Datensicht**, klicken Sie auf das **+**-Symbol, um eine neue Datenquelle hinzuzufügen. Der **Datenquelle hinzufügen** -Assistent wird gestartet. 

   ![Datensicht](media/tutorial-classifying-iris/data_view.png)

3. Führen Sie die Schritte im Assistenten aus. 
   - Wählen Sie auf dem ersten Bildschirm die Option **Datei(en)/Verzeichnis**, und klicken Sie auf **Weiter**.
   - Wählen Sie auf dem zweiten Bildschirm die lokale Datei **iris.csv** aus, z.B. „C:\Temp\myIris\iris.csv“.
   - Übernehmen Sie auf dem dritten Bildschirm **Dateidetails** die Standardwerte.
   - Ändern Sie auf dem vierten Bildschirm **Datentypen** für _Column1_ bis _Column4_ den **DATENTYP** von _Zeichenfolge_ in _Numerisch_. Diese Spalten enthalten numerische Werte. 
   - Übernehmen Sie auf dem fünften und sechsten Bildschirm die Standardeinstellungen.
   - Klicken Sie auf die Schaltfläche **Fertig stellen**.

   ![Wählen Sie iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Achten Sie darauf, dass Sie für diese Übung die Datei **iris.csv** aus dem aktuellen Projektverzeichnis auswählen, da nachfolgende Schritte andernfalls ggf. fehlschlagen. 

4. Die neue Datei **iris-1.dsource** wird erstellt. Die Datei ist mit dem Zusatz „-1“ eindeutig benannt, da das Beispielprojekt bereits über eine nicht nummerierte Datei **iris.dsource** verfügt.  

   Die Datei wird geöffnet, und die Daten werden angezeigt. Diesem Dataset werden automatisch mehrere Spaltenüberschriften von **Column1** bis **Column5** hinzugefügt. Scrollen Sie nach unten, und beachten Sie, dass die letzte Zeile des Datasets leer ist. Das hängt mit einem zusätzlichen Zeilenumbruch in der CSV-Datei zusammen.

   ![iris-Datensicht](media/tutorial-classifying-iris/iris_data_view.png)

5. Klicken Sie nun auf die **Metriken**-Schaltfläche. Beachten Sie die Histogramme und eine Reihe von Statistiken, die für jede Spalte für Sie berechnet werden. Sie können auch auf die **Daten**-Schaltfläche klicken, um die Daten erneut anzuzeigen. 

   ![iris-Datensicht](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Klicken Sie auf die Schaltfläche **Vorbereiten**. Das Dialogfeld **Vorbereiten** öffnet sich. 

   Das Beispielprojekt enthält die Datei **iris.dprep**. Sie werden also standardmäßig aufgefordert, in diesem vorhandenen Datenvorbereitungspaket **iris.dprep** einen neuen Datenfluss zu erstellen. 

   Ändern Sie den Dropdownliste-Wert auf **+New Data Preparation Package (+Neues Datenvorbereitungspaket)**, geben Sie einen neuen Wert "Iris-1" ein, und klicken Sie auf **OK**.

   ![Iris-Datenansicht](media/tutorial-classifying-iris/new_dprep.png)

   Ein neues Datenvorbereitungspaket mit dem Namen **iris-1.dprep** wird erstellt und im Editor für die Vorbereitung von Daten geöffnet.

7. Nun führen wir einige einfache Datenvorbereitungsaufgaben durch. Benennen Sie die Spaltennamen durch Klicken auf jede Spaltenüberschrift um, und machen Sie den Kopfzeilentext editierbar. 

   Geben Sie für die fünf Spalten **Sepal Length** (Länge des Kelchblatts), **Sepal Width** (Breite des Kelchblatts), **Petal Length** (Länge des Kronblatts), **Petal Width** (Breite des Kronblatts) bzw. **Species** (Art) ein.

   ![Umbenennen von Spalten](media/tutorial-classifying-iris/rename_column.png)

8. Wählen Sie zum Zählen von unterschiedlichen Werten die Spalte **Species** aus, und klicken Sie mit der rechten Maustaste darauf. Wählen Sie **Value Counts (Wert-Anzahlen)** aus. 

   ![Klicken auf „Value Counts“ (Wert-Anzahlen)](media/tutorial-classifying-iris/value_count.png)

   Der Bereich **Inspektoren** wird geöffnet, und es wird ein Histogramm mit vier Balken angezeigt. Die Zielspalte enthält die drei unterschiedlichen Werte **Iris_virginica**, **Iris_versicolor** und **Iris-setosa** und zusätzlich den Wert **(null)**.

9. Filtern Sie NULL-Werte heraus, indem Sie den Balken des Graphen auswählen, der für den NULL-Wert steht. Es ist eine Zeile mit dem Wert **(null)** vorhanden. Klicken Sie zum Entfernen dieser Zeile auf die Filterschaltfläche **-**.

   ![Histogramm mit Wert-Anzahl](media/tutorial-classifying-iris/filter_out.png)

10. Beachten Sie die einzelnen Schritte im Bereich **SCHRITTE**. Als Sie die das Umbenennen der Spalten und das Herausfiltern der Zeilen mit NULL-Werten durchgeführt haben, wurde jede Aktion als Datenvorbereitungsschritt aufgezeichnet. Sie können die einzelnen Schritte bearbeiten, um die Einstellungen anzupassen, und die Schritte neu anordnen oder sogar entfernen.

   ![Schritte](media/tutorial-classifying-iris/steps.png)

11. Schließen Sie den Editor für die Datenvorbereitung jetzt, indem Sie auf der Registerkarte **iris-1** mit dem Graphsymbol auf **X** klicken. Ihre Arbeit wird automatisch in der Datei **iris-1.dprep** unter der Überschrift **Data Preparations** (Datenvorbereitungen) gespeichert.

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

1. Klicken Sie mit der rechten Maustaste auf die **Iris-1.dprep**-Datei, um das Kontextmenü anzuzeigen, und wählen Sie **Generate Data Access Code File (Datenzugriffs-Codedatei generieren)**. 

   ![Genieren von Code](media/tutorial-classifying-iris/generate_code.png)

2. Die neue Datei **iris-1.py** mit den folgenden Codezeilen wird geöffnet:

   ```python
   # This code snippet will load the referenced package and return a DataFrame.
   # If the code is run in a PySpark environment, the code will return a
   # Spark DataFrame. If not, the code will return a Pandas DataFrame.

   from azureml.dataprep.package import run
   df = run('iris.dprep', dataflow_idx=0)
   ```

   Dieser Codeausschnitt ruft die Logik auf, die Sie als Datenvorbereitungspaket erstellt haben. Je nach Kontext, in dem dieser Code ausgeführt wird, kann `df` für unterschiedliche Arten von Datenrahmen stehen. Ein [pandas-Datenrahmen](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) wird verwendet, wenn der Code in Python-Laufzeit ausgeführt wird, und ein [Spark-Datenrahmen](https://spark.apache.org/docs/latest/sql-programming-guide.html) wird verwendet, wenn die Ausführung in einem Spark-Kontext erfolgt. 

   Für weitere Informationen zum Vorbereiten von Daten in Azure Machine Learning-Workbench, konsultieren Sie den [Getting Started with Data Preparation (Erste Schritte bei der Datenvorbereitung)](data-prep-getting-started.md)-Leitfaden.

## <a name="next-steps"></a>Nächste Schritte
In diesem ersten Teil der dreiteiligen Tutorial-Reihe wurde beschrieben, wie Sie Azure Machine Learning Workbench für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Erstellen eines neuen Projekts 
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

Sie sind jetzt bereit zum nächsten Teil in der Reihe zu wechseln, um ein Machine Learning-Modell zu erstellen.
> [!div class="nextstepaction"]
> [Erstellen eines Modells](tutorial-classifying-iris-part-2.md)

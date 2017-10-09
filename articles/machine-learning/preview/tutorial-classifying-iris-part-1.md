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
ms.date: 09/25/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e88b6bf74b7492353e5d5d004bde12fa4787e405
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-1-prepare-data"></a>Klassifizieren von Iris Teil 1: Vorbereiten von Daten
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der erste Teil einer dreiteiligen Serie. In diesem Tutorial betrachten wir gemeinsam die Grundlagen von Machine Learning-Diensten (Vorschau). In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Projekts in der Azure Machine Learning Workbench
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

Dieses Tutorial verwendet den timeless [Iris flower dataset (Iris Blume Dataset)](https://en.wikipedia.org/wiki/Iris_flower_data_set), um die Dinge einfach zu halten. Die Screenshots sind Windows-spezifisch, aber die macOS-Darstellung ist nahezu identisch.

## <a name="launch-azure-machine-learning-workbench"></a>Starten von Azure Machine Learning Workbench
Folgen Sie dem [Install and create Quickstart (Schnellstart für das Installieren und Erstellen)](quickstart-installation.md), um die Azure Machine Learning Workbench-Anwendung, die auch die Befehlszeilenschnittstelle (CLI) enthält zu installieren. Starten Sie die Azure Machine Learning Workbench-App, und melden Sie sich bei Bedarf an.

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts
1. Im **PROJEKTE**-Bereich, klicken Sie auf das **+**-Symbol, um ein **Neues Projekt** zu erstellen.

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
1. Öffnen Sie die `iris.csv`-Datei in der **Dateiansicht**. Die Datei ist eine einfache Tabelle mit 5 Spalten und 150 Zeilen. Sie verfügt über vier numerische Feature-Spalten und eine Zeichenfolge für die Zielspalte. Sie verfügt nicht über Spaltenheader.

   ![Iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   >Hinweis: Es wird nicht empfohlen, Datendateien in den Projektordner einzuschließen, insbesondere dann, wenn die Datei groß ist. Wir schließen `iris.csv` in diese Vorlage zu Demonstrationszwecken ein, da sie klein ist. Für weitere Informationen konsultieren Sie den [How to read and write large data files (Wie Sie große Datendateien lesen und schreiben)](how-to-read-write-files.md)-Artikel.

2. In der **Datensicht**, klicken Sie auf das **+**-Symbol, um eine neue Datenquelle hinzuzufügen. Der **Datenquelle hinzufügen** -Assistent wird gestartet. 

   ![Datensicht](media/tutorial-classifying-iris/data_view.png)

3. Wählen Sie die **Datei(en)/Verzeichnis**-Option aus, und wählen Sie die `iris.csv` lokale Datei. Akzeptieren Sie die Standardeinstellungen für jeden Bildschirm, und klicken Sie abschließend auf **Fertig stellen**. 

   ![Wählen Sie iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Stellen Sie sicher, dass die Auswahl der `iris.csv`-Datei aus dem aktuellen Projektverzeichnis für diese Übung ausgewählt wird, andernfalls schlagen letztere Schritte möglicherweise fehl. 

4. Eine neue `iris-1.dsource`-Datei wird erstellt. Die Datei ist mit einem „dash-one“ eindeutig benannt, da das Beispielprojekt bereits mit einer nicht nummerierten `iris.dsource`-Datei ausgestattet ist.  Die Datei wird geöffnet, und die Daten werden angezeigt. Eine Reihe von Spaltenüberschriften aus `Column1` bis `Column5` werden automatisch mit diesem Dataset hinzugefügt. Scrollen Sie nach unten, und beachten Sie, dass die letzte Zeile des Datasets leer ist. Das hängt mit einem zusätzlichen Zeilenumbruch in der CSV-Datei zusammen.

   ![iris-Datensicht](media/tutorial-classifying-iris/iris_data_view.png)

5. Klicken Sie nun auf die **Metriken**-Schaltfläche. Beachten Sie die Histogramme und eine Reihe von Statistiken, die für jede Spalte für Sie berechnet werden. Sie können auch auf die **Daten**-Schaltfläche klicken, um die Daten erneut anzuzeigen. 

   ![iris-Datensicht](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Klicken Sie auf die **Vorbereiten**-Schaltfläche neben der **Metriken**-Schaltfläche (oder auf die **Daten**-Schaltfläche, wenn Sie sich in der Ansicht "Nachrichtenmetrik" befinden). Das Dialogfeld **Vorbereiten** öffnet sich. Das Beispielprojekt enthält bereits eine `iris.dprep`-Datei, sodass Sie standardmäßig von dieser aufgefordert werden einen neuen Datenfluss im vorhandenen **iris.dprep**-Datenvorbereitungspaket zu erstellen. Ändern Sie den Dropdownliste-Wert auf **+New Data Preparation Package (+Neues Datenvorbereitungspaket)**, geben Sie einen neuen Wert "Iris-1" ein, und klicken Sie auf **OK**.

   ![iris-Datensicht](media/tutorial-classifying-iris/new_dprep.png)

Ein neues Datenvorbereitungspaket mit dem Namen `iris-1.dprep` wird erstellt und im Editor für die Vorbereitung von Daten geöffnet.

Führen Sie gleich einige einfache Datenvorbereitungsaufgaben durch. Benennen Sie die Spaltennamen durch Klicken auf jede Spaltenüberschrift um, und machen Sie den Kopfzeilentext editierbar. Geben Sie `Sepal Length`, `Sepal Width`, `Petal Length`, `Petal Width`, und `Species` jeweils für die fünf Spalten ein.

![Umbenennen von Spalten](media/tutorial-classifying-iris/rename_column.png)

Wählen Sie die `Species`-Spalte aus, und klicken Sie mit der rechten Maustaste darauf. Wählen Sie **Value Counts (Wert-Anzahlen)** aus. 

![Wert-Anzahl](media/tutorial-classifying-iris/value_count.png)

Diese Aktion erstellt ein Histogramm mit vier Balken. Unser Zielspalte verfügt über drei unterschiedliche Werte `Iris_virginica`, `Iris_versicolor`, `Iris-setosa`. Es gibt auch eine Zeile mit einem `(null)`-Wert. Entfernen Sie diese Zeile durch Auswahl des Balkens, der den NULL-Wert darstellt, und klicken Sie auf **-** der Filterschaltfläche um diesen zu entfernen. 

![Wert-Anzahl](media/tutorial-classifying-iris/filter_out.png)

Da es hier darum geht Spalten umzubenennen und die NULL-Wert-Zeile herausfiltern, wird jede Aktion, die Sie durchführen als Schritt im Rahmen der Datenvorbereitung im **SCHRITTE**-Bereich aufgezeichnet. Sie können diese (um ihre Einstellungen anzupassen) bearbeiten, sie neu anordnen, oder sogar entfernen.

![steps](media/tutorial-classifying-iris/steps.png)

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

Schließen Sie jetzt den DataPrep-Editor. (Keine Sorge, die Speicherung erfolgt automatisch.) Klicken Sie mit der rechten Maustaste auf die **Iris-1.dprep**-Datei, um das Kontextmenü anzuzeigen, und wählen Sie **Generate Data Access Code File (Datenzugriffs-Codedatei generieren)**. 

![Genieren von Code](media/tutorial-classifying-iris/generate_code.png)

Eine **Iris-1.py**-Datei wird mit zwei bereits angegebenen Codezeilen erstellt (zusammen mit einigen Kommentaren):

```python
# This code snippet will load the referenced package and return a DataFrame.
# If the code is run in a PySpark environment, the code will return a
# Spark DataFrame. If not, the code will return a Pandas DataFrame.

from azureml.dataprep.package import run
df = run('iris.dprep', dataflow_idx=0)
```
Dieser Codeausschnitt ruft die Logik auf, die Sie als Datenvorbereitungspaket erstellt haben. Je nachdem in welchem Zusammenhang dieser Code ausgeführt wird, kann `df` ein [Pandas DataFrame (Pandas Datenrahmen)](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) sein, wenn in einer Python-Runtime ausgeführt oder ein [Spark DataFrame (Spark Datenrahmen)](https://spark.apache.org/docs/latest/sql-programming-guide.html), wenn in einem Spark-Kontext ausgeführt. Für weitere Informationen zum Vorbereiten von Daten in Azure Machine Learning-Workbench, konsultieren Sie den [Getting Started with Data Preparation (Erste Schritte bei der Datenvorbereitung)](data-prep-getting-started.md)-Leitfaden.

## <a name="next-steps"></a>Nächste Schritte
In diesem ersten Teil der dreiteiligen Tutorial-Reihe wurde beschrieben, wie Sie Azure Machine Learning Workbench für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Erstellen eines neuen Projekts 
> * Erstellen eines Datenvorbereitungspakets
> * Generieren von Python/PySpark Code zum Aufrufen eines Datenvorbereitungspakets

Sie sind jetzt bereit zum nächsten Teil in der Reihe zu wechseln, um ein Machine Learning-Modell zu erstellen.
> [!div class="nextstepaction"]
> [Erstellen eines Modells](tutorial-classifying-iris-part-2.md)


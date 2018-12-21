---
title: Tutorial zum Erstellen eines Modells für den Azure Machine Learning-Dienst (Vorschauversion) | Microsoft-Dokumentation
description: Dieses umfassende Tutorial zeigt, wie der Azure Machine Learning-Dienst (Vorschauversion) konsistent verwendet werden kann. Dies ist der zweite Teil der Reihe, in dem es um das Experimentieren geht.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 03/15/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2e4ede4a699eaac1e5f8c632901239fb04ebcd30
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276756"
---
# <a name="tutorial-2-classify-iris---build-a-model"></a>Tutorial 2: Klassifizieren von Iris: Erstellen eines Modells

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Der Azure Machine Learning-Dienst (Vorschauversion) ist eine integrierte Lösung für Data Science und erweiterte Analysen, mit der professionelle Datenanalysten Daten aufbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der **zweite Teil einer dreiteiligen Reihe**. In diesem Teil des Tutorials verwenden Sie den Azure Machine Learning-Dienst für folgende Zwecke:

> [!div class="checklist"]
> * Öffnen von Skripts und Überprüfen von Code
> * Ausführen von Skripts in einer lokalen Umgebung
> * Überprüfen von Ausführungsverläufen
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer lokalen Docker-Umgebung
> * Ausführen von Skripts in einer Docker-Remoteumgebung
> * Ausführen von Skripts in einer Azure HDInsight-Cloudumgebung

In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)) verwendet. 

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:
- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 
- Ein Experimentierkonto und eine Installation von Azure Machine Learning Workbench, wie in dieser [Schnellstartanleitung](quickstart-installation.md) beschrieben.
- Das Projekt und vorbereitete Iris-Daten aus [Teil 1 des Tutorials](tutorial-classifying-iris-part-1.md).
- Eine lokal installierte und ausgeführte Docker-Engine. Die Community Edition von Docker ist ausreichend. Hier ist beschrieben, wie Sie Docker installieren: https://docs.docker.com/engine/installation/.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Prüfen der Datei „iris_sklearn.py“ und der Konfigurationsdateien

1. Starten Sie die Azure Machine Learning Workbench-Anwendung.

1. Öffnen Sie das Projekt **myIris**, das Sie in [Teil 1 der Tutorialreihe](tutorial-classifying-iris-part-1.md) erstellt haben.

2. Wählen Sie im geöffneten Projekt im Bereich ganz links die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste in Ihrem Projektordner zu öffnen.

   ![Öffnen des Azure Machine Learning Workbench-Projekts](media/tutorial-classifying-iris/2-project-open.png)

3. Wählen Sie die Python-Skriptdatei **iris_sklearn.py** aus. 

   ![Auswählen eines Skripts](media/tutorial-classifying-iris/2-choose-iris_sklearn.png)

   Der Code wird in Workbench in einer neuen Registerkarte des Text-Editors geöffnet. Dies ist das Skript, das Sie in diesem Teil des Tutorials durchgängig verwenden. 

   >[!NOTE]
   >Der angezeigte Code entspricht unter Umständen nicht genau dem oben abgebildeten Codebeispiel, da dieses Beispielprojekt regelmäßig aktualisiert wird.
   
   ![Öffnen einer Datei](media/tutorial-classifying-iris/open_iris_sklearn.png)

4. Sehen Sie sich den Code des Python-Skripts an, um sich mit dem Codierungsstil vertraut zu machen. 

   Mit dem Skript **iris_sklearn.py** werden die folgenden Aufgaben durchgeführt:

   * Lädt das Standard-Datenaufbereitungspaket **iris.dprep** zum Erstellen eines [Pandas-Datenrahmens](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

   * Fügt zufällige Funktionen hinzu, um die Lösung des Problems zu erschweren. Zufälligkeit ist erforderlich, da Iris ein kleines Dataset ist, das mit nahezu 100-prozentiger Genauigkeit leicht klassifiziert werden kann.

   * Verwendet die Machine Learning-Bibliothek [scikit-learn](http://scikit-learn.org/stable/index.html) zum Erstellen eines logistischen Regressionsmodells.  Diese Bibliothek ist standardmäßig in Azure Machine Learning Workbench enthalten.

   * Serialisiert das Modell, indem die [pickle](https://docs.python.org/3/library/pickle.html)-Bibliothek in einer Datei im Ordner `outputs` verwendet wird. 
   
   * Lädt das serialisierte Modell und führt dann die Deserialisierung zurück in den Arbeitsspeicher durch.

   * Verwendet das deserialisierte Modell zum Treffen einer Vorhersage für einen neuen Datensatz. 

   * Erstellt zwei Graphen, eine Konfusionsmatrix und eine Grenzwertoptimierungskurve (Receiver Operating Characteristic, ROC) für mehrere Klassen, indem die Bibliothek [matplotlib](https://matplotlib.org/) verwendet wird, und speichert sie anschließend im Ordner `outputs`. Sie können diese Bibliothek in Ihrer Umgebung installieren, falls sie nicht bereits vorhanden ist.

   * Nimmt die Regularisierungsrate und die Modellgenauigkeit automatisch in den Ausführungsverlauf auf. Hierbei wird immer das Objekt `run_logger` verwendet, um die Regularisierungsrate und die Modellgenauigkeit in den Protokollen aufzuzeichnen. 


## <a name="run-irissklearnpy-in-your-local-environment"></a>Ausführen von „iris_sklearn.py“ in Ihrer lokalen Umgebung

1. Starten Sie die Azure Machine Learning-Befehlszeilenschnittstelle (CLI):
   1. Starten Sie Azure Machine Learning Workbench.

   1. Wählen Sie im Workbench-Menü die Option **Datei** > **Eingabeaufforderung öffnen**. 
   
   Das Fenster für die Azure Machine Learning-Befehlszeilenschnittstelle (CLI) wird in Windows im Projektordner `C:\Temp\myIris\>` gestartet. Dieses Projekt entspricht dem Projekt, das Sie in Teil 1 des Tutorials erstellt haben.

   >[!IMPORTANT]
   >Sie müssen dieses CLI-Fenster verwenden, um die nächsten Schritte ausführen zu können.

1. Installieren Sie im CLI-Fenster die Python-Bibliothek für das Zeichnen (**matplotlib**), falls Sie dies noch nicht getan haben.

   Das Skript **iris_sklearn.py** verfügt über Abhängigkeiten von zwei Python-Paketen: **scikit-learn** und **matplotlib**.  Das Paket **scikit-learn** wird von Azure Machine Learning Workbench als Hilfe für Sie installiert. Es ist aber erforderlich, **matplotlib** zu installieren, falls Sie die Bibliothek noch nicht installiert haben.

   Wenn Sie fortfahren, ohne **matplotlib** zu installieren, kann der Code in diesem Tutorial trotzdem erfolgreich ausgeführt werden. Mit dem Code können aber nicht die Konfusionsmatrixausgabe und Grenzwertoptimierungskurven (Receiver Operating Characteristic, ROC) für mehrere Klassen erstellt werden (siehe Verlaufsvisualisierungen).

   ```azurecli
   pip install matplotlib
   ```

   Diese Installation dauert ungefähr eine Minute.

1. Wechseln Sie zurück zur Workbench-Anwendung. 

1. Suchen Sie nach der Registerkarte **iris_sklearn.py**. 

   ![Suchen nach Registerkarte mit Skript](media/tutorial-classifying-iris/2-iris_sklearn-tab.png)

1. Wählen Sie in der Symbolleiste dieser Registerkarte die Option **local** als Ausführungsumgebung und `iris_sklearn.py` als auszuführendes Skript aus. Unter Umständen sind die Optionen bereits ausgewählt.

   ![Auswählen von „local“ und des Skripts](media/tutorial-classifying-iris/2-local-script.png)

1. Geben Sie rechts in der Symbolleiste im Feld **Argumente** den Wert `0.01` ein. 

   Dieser Wert entspricht der Regularisierungsrate des Modells für die logistische Regression.

   ![Auswählen von „local“ und des Skripts](media/tutorial-classifying-iris/2-local-script-arguments.png)

1. Wählen Sie die Schaltfläche **Ausführen**. Ein Auftrag wird sofort geplant. Der Auftrag wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet. 

   ![Auswählen von „local“ und des Skripts](media/tutorial-classifying-iris/2-local-script-arguments-run.png)

   Nach kurzer Zeit wechselt der Status des Auftrags von **Wird übermittelt** zu **Wird ausgeführt** und schließlich zu **Abgeschlossen**.

1. Wählen Sie im Bereich **Aufträge** im Text zum Auftragsstatus die Option **Abgeschlossen**. 

   ![Ausführen von „sklearn“](media/tutorial-classifying-iris/2-completed.png)

   Im daraufhin geöffneten Popupfenster wird der Standardausgabetext (stdout) für die Ausführung angezeigt. Wählen Sie zum Schließen des stdout-Texts oben rechts im Popupfenster die Option **Schließen** (**x**).

   ![Standardausgabe](media/tutorial-classifying-iris/2-standard-output.png)

9. Wählen Sie in demselben Auftragsstatus im Bereich **Aufträge** direkt über dem Status **Abgeschlossen** und der Startzeit den blauen Text **iris_sklearn.py [n]** (_n_ ist die Ausführungsanzahl) aus. Das Fenster mit den **Ausführungseigenschaften** wird geöffnet und enthält für die jeweilige Ausführung die folgenden Informationen:
   - Informationen zu den **Ausführungseigenschaften**
   - **Ausgaben**
   - **Metriken**
   - **Visualisierungen**, falls vorhanden
   - **Protokolle** 

   Nach Abschluss der Ausführung werden im Popupfenster die folgenden Ergebnisse angezeigt:

   >[!NOTE]
   >Da im Tutorial für den Trainingssatz zuvor eine zufällige Anordnung verwendet wurde, weichen Ihre Ergebnisse unter Umständen von den hier gezeigten Ergebnissen ab.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```
    
10. Schließen Sie die Registerkarte **Run Properties** (Ausführungseigenschaften), und wechseln Sie anschließend zurück zur Registerkarte **iris_sklearn.py**. 

11. Wiederholen Sie diesen Vorgang für weitere Ausführungen. 

    Geben Sie im Feld **Argumente** mehrere Werte von `0.001` bis `10` ein. Wählen Sie **Ausführen**, um den Code einige weitere Male auszuführen. Der jeweils geänderte Argumentwert wird in das Modell für die logistische Regression im Code eingefügt. Dies führt jeweils zu unterschiedlichen Ergebnissen.

## <a name="review-the-run-history-in-detail"></a>Ausführliches Prüfen des Ausführungsverlaufs
In Azure Machine Learning Workbench wird jede Skriptausführung als Datensatz des Ausführungsverlaufs erfasst. Wenn Sie die Ansicht **Ausführungen** öffnen, können Sie den Ausführungsverlauf eines bestimmten Skripts anzeigen.

1. Wählen Sie zum Öffnen der Liste mit den **Ausführungen** in der linken Symbolleiste die Schaltfläche **Ausführungen** (Uhrsymbol). Wählen Sie anschließend **iris_sklearn.py**, um das **Ausführungsdashboard** von `iris_sklearn.py` anzuzeigen.

   ![Ausführen der Ansicht](media/tutorial-classifying-iris/run_view.png)

1. Die Registerkarte **Run Dashboard** (Ausführungsdashboard) wird geöffnet. 

   Überprüfen Sie die in mehreren Ausführungen erfassten Statistiken. Die Graphen werden oben in der Registerkarte gerendert. Jede Ausführung verfügt über eine fortlaufende Nummer, und die Ausführungsdetails sind in der Tabelle unten auf dem Bildschirm aufgeführt.

   ![Ausführungsdashboard](media/tutorial-classifying-iris/run_dashboard.png)

1. Filtern Sie die Tabelle, und wählen Sie dann einen beliebigen Graphen aus, um für jede Ausführung Status, Dauer, Genauigkeit und Regularisierungsrate anzuzeigen. 

1. Aktivieren Sie in der Tabelle **Ausführungen** die Kontrollkästchen neben mindestens zwei Ausführungen. Wählen Sie die Schaltfläche **Vergleichen**, um einen Bereich für einen ausführlichen Vergleich zu öffnen. Überprüfen Sie die Gegenüberstellung der Ausführungen. 

1. Wählen Sie oben links im Bereich **Vergleich** die Zurück-Schaltfläche **Ausführungsliste**, um wieder zurück zum **Ausführungsdashboard** zu wechseln.

   ![Wechseln zurück zur Liste „Ausführungen“](media/tutorial-classifying-iris/2-compare-back.png)

1. Wählen Sie eine einzelne Ausführung aus, um die Ausführungsdetailansicht anzuzeigen. Beachten Sie, dass die Statistiken für die ausgewählte Ausführung im Abschnitt **Run Properties** (Ausführungseigenschaften) aufgeführt sind. Die Dateien, die in den Ausgabeordner geschrieben werden, sind im Abschnitt **Ausgaben** aufgeführt, und Sie können sie dort herunterladen.

   ![Ausführungsdetails](media/tutorial-classifying-iris/run_details.png)

   Die beiden Diagramme, d.h. die Konfusionsmatrix und die ROC-Kurve für mehrere Klassen, werden im Abschnitt **Visualisierungen** gerendert. Alle Protokolldateien werden zudem im Abschnitt **Protokolle** angezeigt.


## <a name="run-scripts-in-local-docker-environments"></a>Ausführen von Skripts in lokalen Docker-Umgebungen

Sie können optional mit der Ausführung von Skripts für einen lokalen Docker-Container experimentieren. Sie können zusätzliche Ausführungsumgebungen, z.B. Docker, konfigurieren und Ihr Skript in diesen Umgebungen ausführen. 

>[!NOTE]
>Wenn Sie mit der Ausführung von Skripts in einem Docker-Container auf einem virtuellen Azure-Remotecomputer oder in einem Azure HDInsight Spark-Cluster experimentieren möchten, können Sie die [Anweisungen zum Erstellen einer Ubuntu-basierten Azure Data Science-VM oder eines HDInsight-Clusters](how-to-create-dsvm-hdi.md) befolgen.

1. Installieren und starten Sie Docker lokal auf Ihrem Windows- oder MacOS-Computer, falls noch nicht geschehen. Weitere Informationen finden Sie in der Anleitung zur Docker-Installation unter https://docs.docker.com/install/. Die Community Edition ist ausreichend.

1. Wählen Sie im linken Bereich das Symbol **Ordner**, um die Liste **Dateien** für Ihr Projekt zu öffnen. Erweitern Sie den Ordner `aml_config`. 

2. Es sind mehrere Umgebungen vorkonfiguriert: **docker-python**, **docker-spark** und **local**. 

   Jede Umgebung verfügt über zwei Dateien, z.B. `docker.compute` (sowohl für **docker-python** als auch für **docker-spark**) und `docker-python.runconfig`. Wenn Sie die einzelnen Dateien öffnen, sehen Sie, dass bestimmte Optionen im Text-Editor konfiguriert werden können.  

   Wählen Sie zum Bereinigen in den Registerkarten für geöffnete Text-Editoren die Option **Schließen** (**x**).

3. Führen Sie das Skript **iris_sklearn.py** in der Umgebung **docker-python** aus: 

   - Klicken Sie in der linken Symbolleiste auf das **Uhr**-Symbol, um den Bereich **Ausführungen** zu öffnen. Wählen Sie **All Runs** (Alle Ausführungen). 

   - Wählen Sie oben auf der Registerkarte **All Runs** (Alle Ausführungen) anstelle der Standardumgebung **Lokal** die Umgebung **docker-python** als Zielumgebung aus. 

   - Wählen Sie als Nächstes auf der rechten Seite **iris_sklearn.py** als auszuführendes Skript aus. 

   - Lassen Sie das Feld **Argumente** leer, da das Skript einen Standardwert angibt. 

   - Wählen Sie die Schaltfläche **Ausführen**.

4. Sie sehen, dass ein neuer Auftrag gestartet wird. Er wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet.

   Wenn Sie das Skript erstmalig für die Docker-Umgebung ausführen, dauert der Auftrag einige Minuten länger. 

   Im Hintergrund wird von Azure Machine Learning Workbench eine neue Docker-Datei erstellt. 
   In der neuen Datei wird auf das Docker-Basisimage verwiesen, das in der Datei `docker.compute` angegeben ist, sowie auf die davon abhängigen Python-Pakete in der Datei `conda_dependencies.yml`. 
   
   Mit der Docker-Engine werden die folgenden Aufgaben durchgeführt:

    - Herunterladen des Basisimage aus Azure
    - Installieren der Python-Pakete, die in der Datei `conda_dependencies.yml` angegeben sind
    - Starten eines Docker-Containers
    - Kopieren von bzw. Verweisen auf die lokale Kopie des Projektordners (je nach Ausführungskonfiguration)      
    - Ausführen des Skripts `iris_sklearn.py`

   Am Ende sollte genau das gleiche Ergebnis wie beim Angeben von **local** als Ziel angezeigt werden.

5. Als Nächstes testen wir Spark. Das Docker-Basisimage enthält eine vorinstallierte und konfigurierte Spark-Instanz, die Sie zum Ausführen eines PySpark-Skripts verwenden können. Dieses Basisimage ist eine einfache Möglichkeit zum Entwickeln und Testen Ihres Spark-Programms, ohne Spark zeitaufwändig selbst installieren und konfigurieren zu müssen. 

   Öffnen Sie die Datei `iris_spark.py` . Mit diesem Skript wird die Datendatei `iris.csv` geladen und das Iris-Dataset mithilfe des Algorithmus für die logistische Regression aus der Spark Machine Learning-Bibliothek klassifiziert. Ändern Sie nun die Ausführungsumgebung in **docker-spark** und das Skript in **iris_spark.py**, und führen Sie es anschließend erneut aus. Dieser Prozess dauert etwas länger, da eine Spark-Sitzung erstellt und im Docker-Container gestartet werden muss. Sie können auch sehen, dass sich der stdout-Text von dem stdout-Text von `iris_spark.py` unterscheidet.

6. Starten Sie einige weitere Ausführungen, und testen Sie unterschiedliche Argumente. 

7. Öffnen Sie die Datei `iris_spark.py`, um das Modell für die logistische Regression anzuzeigen, das mit der Spark Machine Learning-Bibliothek erstellt wurde. 

8. Interagieren Sie mit dem Bereich **Aufträge**, führen Sie einen Vorgang für die Listenansicht des Verlaufs aus, und zeigen Sie eine Detailansicht Ihrer Ausführungen in verschiedenen Ausführungsumgebungen an.

## <a name="run-scripts-in-the-cli-window"></a>Ausführen von Skripts im CLI-Fenster

1. Starten Sie die Azure Machine Learning-Befehlszeilenschnittstelle (CLI):
   1. Starten Sie Azure Machine Learning Workbench.

   1. Wählen Sie im Workbench-Menü die Option **Datei** > **Eingabeaufforderung öffnen**. 
   
   Die CLI-Eingabeaufforderung beginnt im Projektordner `C:\Temp\myIris\>` unter Windows. Dies ist das Projekt, das Sie in Teil 1 des Tutorials erstellt haben.

   >[!IMPORTANT]
   >Sie müssen dieses CLI-Fenster verwenden, um die nächsten Schritte ausführen zu können.

1. Melden Sie sich im CLI-Fenster an Azure an. [Hier finden Sie weitere Informationen zu „az login“](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   Unter Umständen sind Sie bereits angemeldet. In diesem Fall können Sie diesen Schritt überspringen.

   1. Geben Sie an der Eingabeaufforderung Folgendes ein:
      ```azurecli
      az login
      ```

      Mit diesem Befehl wird ein Code zurückgegeben, den Sie in Ihrem Browser unter https://aka.ms/devicelogin verwenden können.

   1. Navigieren Sie in Ihrem Browser zu https://aka.ms/devicelogin.

   1. Geben Sie bei entsprechender Aufforderung den Code, den Sie in der CLI empfangen haben, in Ihren Browser ein.

   Die Workbench-App und die CLI verwenden unabhängige Anmeldeinformationscaches bei der Authentifizierung von Azure-Ressourcen. Nachdem Sie sich angemeldet haben, müssen Sie sich erst wieder authentifizieren, wenn das zwischengespeicherte Token abgelaufen ist. 

1. Falls Ihre Organisation über mehrere Azure-Abonnements (Unternehmensumgebung) verfügt, müssen Sie das gewünschte Abonnement festlegen. Suchen Sie nach Ihrem Abonnement, legen Sie es mit der Abonnement-ID fest, und testen Sie es.

   1. Listen Sie alle Azure-Abonnements auf, auf die Sie Zugriff haben, indem Sie diesen Befehl verwenden:
   
      ```azurecli
      az account list -o table
      ```

      Mit dem Befehl **az account list** wird die Liste der für Ihre Anmeldung verfügbaren Abonnements zurückgegeben. 
      Bei mehr als einem Abonnement identifizieren Sie den ID-Wert für das gewünschte Abonnement.

   1. Legen Sie das Azure-Abonnement fest, das Sie als Standardkonto verwenden möchten:
   
      ```azurecli
      az account set -s <your-subscription-id>
      ```
      Hierbei ist \<your-subscription-id\> der ID-Wert für das gewünschte Abonnement. Geben Sie die Klammern nicht mit an.

   1. Überprüfen Sie die neue Abonnementeinstellung, indem Sie die Details für das aktuelle Abonnement anfordern. 

      ```azurecli
      az account show
      ```    

1. Installieren Sie im CLI-Fenster die Python-Bibliothek für das Zeichnen (**matplotlib**), falls Sie dies noch nicht getan haben.

   ```azurecli
   pip install matplotlib
   ```

1. Übermitteln Sie das Skript **iris_sklearn.py** im CLI-Fenster als Experiment.

   Die Ausführung von „iris_sklearn.py“ wird basierend auf dem lokalen Computekontext ausgeführt.

   + Unter Windows:
     ```azurecli
     az ml experiment submit -c local .\iris_sklearn.py
     ```

   + Unter MacOS:
     ```azurecli
     az ml experiment submit -c local iris_sklearn.py
     ```
   
   Ihre Ausgabe sollte in etwa wie folgt aussehen:
    ```text
    RunId: myIris_1521077190506
    
    Executing user inputs .....
    ===========================
    
    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]
    
    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679
        
    ==========================================
    Serialize and deserialize using the outputs folder.
    
    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
     [ 1 37 12]
     [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.
    
    Execution Details
    =================
    RunId: myIris_1521077190506
    ```

1. Überprüfen Sie die Ausgabe. Sie verfügen über die gleiche Ausgabe und die gleichen Ergebnisse wie bei Verwendung von Workbench zum Ausführen des Skripts. 

1. Führen Sie im CLI-Fenster das Python-Skript **iris_sklearn.py** aus, indem Sie wieder eine Docker-Ausführungsumgebung verwenden (falls Docker auf Ihrem Computer installiert ist).

   + Bei einem Container unter Windows: 
     |Ausführung<br/>Environment|Befehl unter Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python .\iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark .\iris_spark.py 0.1`|

   + Bei einem Container unter MacOS: 
     |Ausführung<br/>Environment|Befehl unter Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark iris_spark.py 0.1`|

1. Wechseln Sie zurück zu Workbench, und gehen Sie wie folgt vor:
   1. Wählen Sie im linken Bereich das Ordnersymbol aus, um die Projektdateien aufzulisten.
   
   1. Öffnen Sie das Python-Skript mit dem Namen **run.py**. Dieses Skript ist nützlich, um eine Schleife durch verschiedene Regularisierungsraten durchzuführen. 

   ![Wechseln zurück zur Liste „Ausführungen“](media/tutorial-classifying-iris/2-runpy.png)

1. Führen Sie das Experiment mehrfach mit diesen Raten aus. 

   Mit diesem Skript wird der Auftrag `iris_sklearn.py` mit der Regularisierungsrate `10.0` (extrem hoher Wert) gestartet. Über das Skript wird die Rate während der folgenden Ausführung halbiert usw., bis der Mindestwert `0.005` erreicht ist. 

   Das Skript enthält den folgenden Code:

   ![Wechseln zurück zur Liste „Ausführungen“](media/tutorial-classifying-iris/2-runpy-code.png)

1. Führen Sie das Skript **run.py** wie folgt über die Befehlszeile aus:

   ```cmd
   python run.py
   ```

   Mit diesem Befehl wird „iris_sklearn.py“ mehrere Male mit unterschiedlichen Regularisierungsraten gesendet.

   Wenn `run.py` abgeschlossen ist, werden in der Workbench in der Liste mit dem Ausführungsverlauf Graphen mit verschiedenen Metriken angezeigt.

## <a name="run-scripts-in-a-remote-docker-container"></a>Ausführen von Skripts in einem Docker-Remotecontainer
Um Ihr Skript in einem Docker-Container auf einem Linux-Remotecomputer auszuführen, müssen Sie über SSH-Zugriff (Benutzername und Kennwort) für den Remotecomputer verfügen. Zudem muss auf dem Computer eine Docker-Engine installiert sein und ausgeführt werden. Am einfachsten erhalten Sie einen solchen Linux-Computer, indem Sie in Azure einen Ubuntu-basierten virtuellen Data Science-Computer (DSVM) erstellen. Erfahren Sie, wie Sie [einen Ubuntu-DSVM zur Verwendung in Azure ML Workbench erstellen](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>Die CentOS-basierte DSVM wird *nicht* unterstützt.

1. Nach der Erstellung der VM können Sie sie als Ausführungsumgebung anfügen, indem Sie ein Paar mit einer `.runconfig`- und einer `.compute`-Datei generieren. Verwenden Sie den folgenden Befehl, um die Dateien zu generieren. 

 Wir geben dem neuen Computeziel den Namen `myvm`.
 
   ```azurecli
   az ml computetarget attach remotedocker --name myvm --address <your-IP> --username <your-username> --password <your-password>
   ```
   
   >[!NOTE]
   >Die IP-Adresse kann auch ein öffentlich adressierbarer vollqualifizierter Domänenname (FQDN) sein, z.B. `vm-name.southcentralus.cloudapp.azure.com`. Es ist eine bewährte Methode, Ihrer DSVM einen FQDN hinzuzufügen und anstelle einer IP-Adresse zu verwenden. Diese Methode ist sinnvoll, da Sie die VM später ggf. einmal deaktivieren, um Kosten zu sparen. Darüber hinaus kann sich beim nächsten Start des virtuellen Computers die IP-Adresse geändert haben.

   >[!NOTE]
   >Zusätzlich zur Authentifizierung mit Benutzername und Kennwort können Sie einen privaten Schlüssel und die dazugehörige Passphrase (falls zutreffend) angeben, indem Sie die Optionen `--private-key-file` und `--private-key-passphrase` (optional) verwenden. Wenn Sie den privaten Schlüssel verwenden möchten, den Sie beim Erstellen von DSVM verwendet haben, geben Sie die Option `--use-azureml-ssh-key` an.

   Bereiten Sie als Nächstes das Computeziel **myvm** vor, indem Sie diesen Befehl ausführen.
   
   ```azurecli
   az ml experiment prepare -c myvm
   ```
   
   Mit dem vorherigen Befehl wird das Docker-Image auf der VM erstellt, um die Ausführung der Skripts vorzubereiten.
   
   >[!NOTE]
   >Sie können zudem den Wert für `PrepareEnvironment` in `myvm.runconfig` vom Standardwert `false` in `true` ändern. Durch diese Änderung wird der Docker-Container bei der ersten Ausführung automatisch vorbereitet.

2. Bearbeiten Sie die generierte Datei `myvm.runconfig` unter `aml_config`, und ändern Sie den Standardwert für das Framework von `PySpark` in `Python`:

   ```yaml
   Framework: Python
   ```
   >[!NOTE]
   >PySpark funktioniert in der Regel auch, aber die Verwendung von Python ist effizienter, falls Sie zum Ausführen Ihres Python-Skripts keine Spark-Sitzung benötigen.

3. Geben Sie den gleichen Befehl wie eben im CLI-Fenster ein, indem Sie dieses Mal _myvm_ als Ziel verwenden, um „iris_sklearn.py“ in einem Docker-Remotecontainer auszuführen:
   ```azurecli
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   Der Befehl wird genau wie in einer Umgebung vom Typ `docker-python` ausgeführt, aber jetzt erfolgt die Ausführung auf dem virtuellen Linux-Remotecomputer. Im CLI-Fenster werden die gleichen Ausgabeinformationen angezeigt.

4. Wir probieren nun die Verwendung von Spark im Container aus. Öffnen Sie den Datei-Explorer. Erstellen Sie eine Kopie der Datei `myvm.runconfig`, und geben Sie ihr den Namen `myvm-spark.runconfig`. Bearbeiten Sie die neue Datei, und ändern Sie die Einstellung `Framework` von `Python` in `PySpark`:
   ```yaml
   Framework: PySpark
   ```
   Nehmen Sie an der Datei `myvm.compute` keine Änderungen vor. Dasselbe Docker-Image auf demselben virtuellen Computer wird für die Spark-Ausführung verwendet. In der neuen Datei `myvm-spark.runconfig` verweist das Feld `Target` über ihren Namen `myvm` auf dieselbe Datei `myvm.compute`.

5. Geben Sie den folgenden Befehl ein, um das Skript **iris_spark.py** in der Spark-Instanz auszuführen, die im Docker-Remotecontainer ausgeführt wird:
   ```azureli
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="run-scripts-in-hdinsight-clusters"></a>Ausführen von Skripts in HDInsight-Clustern
Sie können dieses Skript auch in einem HDInsight Spark-Cluster ausführen. Erfahren Sie, wie Sie [einen HDInsight Spark-Cluster zur Verwendung in Azure ML Workbench erstellen](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[!NOTE] 
>Der HDInsight-Cluster muss Azure Blob als primären Speicher verwenden. Die Verwendung von Azure Data Lake-Speicher wird noch nicht unterstützt.

1. Wenn Sie Zugriff auf einen Spark für Azure HDInsight-Cluster haben, generieren Sie wie hier gezeigt einen Befehl für die HDInsight-Laufzeitkonfiguration. Geben Sie den Namen des HDInsight-Clusters sowie Ihren Benutzernamen und Ihr Kennwort für HDInsight als Parameter an. 

   Verwenden Sie den folgenden Befehl, um ein Computeziel zu erstellen, das auf einen HDInsight-Cluster verweist:

   ```azurecli
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <your-username> --password <your-password>
   ```

   Führen Sie diesen Befehl aus, um den HDInsight-Cluster vorzubereiten:

   ```
   az ml experiment prepare -c myhdi
   ```

   Der FQDN des Clusterhauptknotens lautet normalerweise `<your_cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >`username` ist der SSH-Benutzername für den Cluster, der während des HDInsight-Setups definiert wird. Standardmäßig lautet der Wert `sshuser`. Der Wert lautet nicht `admin`. Dies ist der andere Benutzer, der während der Einrichtung erstellt wurde, um Zugriff auf die Administratorwebsite des Clusters zu ermöglichen. 

2. Führen Sie das Skript **iris_spark.py** im HDInsight-Cluster mit diesem Befehl aus:

   ```azurecli
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Wenn Sie den Befehl für einen HDInsight-Remotecluster ausführen, können Sie auch die Details zur YARN-Auftragsausführung (Yet Another Resource Negotiator) unter `https://<your_cluster_name>.azurehdinsight.net/yarnui` anzeigen, indem Sie das Benutzerkonto `admin` verwenden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte
In diesem zweiten Teil der dreiteiligen Tutorialreihe wurde Folgendes beschrieben:
> [!div class="checklist"]
> * Öffnen von Skripts und Prüfen des Codes in Workbench
> * Ausführen von Skripts in einer lokalen Umgebung
> * Prüfen des Ausführungsverlaufs
> * Ausführen von Skripts in einer lokalen Docker-Umgebung

Sie können jetzt den dritten Teil dieser Tutorialreihe durcharbeiten, in dem Sie das Modell für die logistische Regression bereitstellen können, das Sie als Echtzeit-Webdienst erstellt haben.

> [!div class="nextstepaction"]
> [Tutorial 3 – Bereitstellen von Modellen](tutorial-classifying-iris-part-3.md)

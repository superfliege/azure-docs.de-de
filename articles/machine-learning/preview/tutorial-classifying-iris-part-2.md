---
title: "Erstellen eines Modells für Azure Machine Learning-Dienste (Vorschau) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschau) verwendet werden können. Dies ist der zweite Teil der Reihe, in dem es um das Experimentieren geht."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 11/06/2017
ms.openlocfilehash: 5bbfe63d159ba2d09a495908f69f707ed04a02f8
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="classify-iris-part-2-build-a-model"></a>Klassifizieren von Iris – Teil 2: Erstellen eines Modells
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der zweite Teil einer dreiteiligen Reihe. In diesem Teil des Tutorials verwenden Sie Azure Machine Learning-Dienste (Vorschauversion), um Folgendes zu erfahren:

> [!div class="checklist"]
> * Verwenden von Azure Machine Learning Workbench
> * Öffnen von Skripts und Überprüfen von Code
> * Ausführen von Skripts in einer lokalen Umgebung
> * Prüfen des Ausführungsverlaufs
> * Ausführen von Skripts in einer lokalen Docker-Umgebung
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer Docker-Remoteumgebung
> * Ausführen von Skripts in einer Azure HDInsight-Cloudumgebung

In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)) verwendet. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für Mac OS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
Arbeiten Sie den ersten Teil dieser Tutorialreihe durch. Führen Sie vor dem vorliegenden Tutorial die Schritte im [Tutorial zum Vorbereiten von Daten](tutorial-classifying-iris-part-1.md) aus, um Azure Machine Learning-Ressourcen zu erstellen und die Azure Machine Learning Workbench-Anwendung zu installieren.

Sie können optional mit der Ausführung von Skripts für einen lokalen Docker-Container experimentieren. Hierzu muss auf Ihrem Windows- oder Mac OS-Computer ein lokal installiertes und gestartetes Docker-Modul vorhanden sein. (Die Community Edition ist ausreichend.) Weitere Informationen zur Docker-Installation finden Sie in der [Docker-Installationsanleitung](https://docs.docker.com/engine/installation/).

Wenn Sie mit der Ausführung von Skripts in einem Docker-Container auf einem virtuellen Azure-Remotecomputer oder in einem Azure HDInsight Spark-Cluster experimentieren möchten, können Sie die [Anweisungen zum Erstellen einer Ubuntu-basierten Azure Data Science-VM oder eines HDInsight-Clusters](how-to-create-dsvm-hdi.md) befolgen.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Prüfen der Datei „iris_sklearn.py“ und der Konfigurationsdateien
1. Öffnen Sie die Anwendung Azure Machine Learning Workbench und dann das Projekt **myIris**, das Sie im vorherigen Teil der Tutorialreihe erstellt haben.

2. Wählen Sie nach dem Öffnen des Projekts im Bereich ganz links die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste in Ihrem Projektordner zu öffnen.

3. Wählen Sie die Datei **iris_sklearn.py** aus. Der Python-Code wird in Workbench in einer neuen Registerkarte des Text-Editors geöffnet.

   ![Öffnen einer Datei](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Der angezeigte Code entspricht unter Umständen nicht genau dem oben abgebildeten Codebeispiel, da dieses Beispielprojekt regelmäßig aktualisiert wird.

4. Sehen Sie sich den Code des Python-Skripts an, um sich mit dem Codierungsstil vertraut zu machen. Das Skript führt Folgendes aus:

   - Lädt das Datenvorbereitungspaket **iris.dprep** zum Erstellen eines [Pandas-Datenrahmens](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Verwenden Sie das zum Beispielprojekt gehörende Datenvorbereitungspaket `iris.dprep`, das identisch mit der Datei `iris-1.dprep` sein sollte, die Sie in Teil 1 dieses Tutorials erstellt haben.

   - Fügt zufällige Funktionen hinzu, um die Lösung des Problems zu erschweren. Zufälligkeit ist erforderlich, da Iris ein kleines Dataset ist, das mit nahezu 100%iger Genauigkeit leicht klassifiziert werden kann.

   - Verwendet die Machine Learning-Bibliothek [scikit-learn](http://scikit-learn.org/stable/index.html) zum Erstellen eines logistischen Regressionsmodells. 

   - Serialisiert das Modell, indem die [pickle](https://docs.python.org/2/library/pickle.html)-Bibliothek in eine Datei im Ordner `outputs` eingefügt wird. Sie wird dann vom Skript geladen und zurück in den Arbeitsspeicher deserialisiert.

   - Verwendet das deserialisierte Modell zum Treffen einer Vorhersage für einen neuen Datensatz. 

   - Erstellt zwei Graphen, eine Konfusionsmatrix und eine Grenzwertoptimierungskurve (Receiver Operating Characteristic, ROC) für mehrere Klassen, indem die Bibliothek [matplotlib](https://matplotlib.org/) verwendet wird, und speichert sie anschließend im Ordner `outputs`.

   - Hierbei wird immer das Objekt `run_logger` verwendet, um die Regularisierungsrate aufzuzeichnen und die Genauigkeit in den Protokollen zu modellieren. Die Protokolle werden automatisch im Ausführungsverlauf ausgegeben.


## <a name="execute-irissklearnpy-script-in-a-local-environment"></a>Ausführen des Skripts „iris_sklearn.py“ in einer lokalen Umgebung

Zunächst bereiten wir das Skript **iris_sklearn.py** für die erste Ausführung vor. Für dieses Skript sind die Pakete **scikit-learn** und **matplotlib** erforderlich. Das Paket **scikit-learn** wurde bereits von Azure Machine Learning Workbench installiert. Es ist aber noch erforderlich, **matplotlib** zu installieren. 

1. Wählen Sie in Azure Machine Learning Workbench das Menü **Datei** und dann die Option **Eingabeaufforderung öffnen**, um die Eingabeaufforderung zu öffnen. Das Befehlszeilenschnittstellenfenster wird als *Azure Machine Learning Workbench-CLI-Fenster* oder kurz als *CLI-Fenster* bezeichnet.

2. Geben Sie im CLI-Fenster den folgenden Befehl ein, um das Python-Paket **matplotlib** zu installieren. Der Vorgang sollte in weniger als einer Minute abgeschlossen sein.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Wenn Sie den vorherigen Befehl `pip install` überspringen, wird der Code in `iris_sklearn.py` erfolgreich ausgeführt. Wenn Sie nur `iris_sklearn.py` ausführen, werden mit dem Code keine Konfusionsmatrixausgabe und Grenzwertoptimierungskurven (Receiver Operating Characteristic, ROC) für mehrere Klassen erstellt (siehe Verlaufsvisualisierungen).

3. Wechseln Sie zurück zum Fenster der Workbench-App. 

4. Wählen Sie in der Symbolleiste oben in der Registerkarte **iris_sklearn.py** das Dropdownmenü neben dem Symbol **Speichern** und anschließend die Option **Laufzeitkonfiguration**. Wählen Sie **Lokal** als Ausführungsumgebung aus, und geben Sie dann `iris_sklearn.py` als auszuführendes Skript ein.

5. Geben Sie anschließend rechts in der Symbolleiste im Feld **Argumente** den Wert `0.01` ein. 

   ![Steuerelement „Ausführen“](media/tutorial-classifying-iris/run_control.png)

6. Wählen Sie die Schaltfläche **Ausführen**. Ein Auftrag wird sofort geplant. Der Auftrag wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet. 

7. Nach kurzer Zeit wechselt der Status des Auftrags von **Wird übermittelt** zu **Wird ausgeführt** und anschließend zu **Abgeschlossen**.

   ![Ausführen von „sklearn“](media/tutorial-classifying-iris/run_sklearn.png)

8. Wählen Sie im Bereich **Aufträge** im Text zum Auftragsstatus die Option **Abgeschlossen**. Im daraufhin geöffneten Popupfenster wird der Standardausgabetext (stdout) des ausgeführten Skripts angezeigt. Wählen Sie zum Schließen des stdout-Texts oben rechts im Popupfenster die Option **Schließen** (**x**).

9. Wählen Sie in demselben Auftragsstatus im Bereich **Aufträge** direkt über dem Status **Abgeschlossen** und der Startzeit den blauen Text **iris_sklearn.py [n]** (_n_ ist die Ausführungsanzahl). Das Fenster mit den **Ausführungseigenschaften** wird geöffnet und enthält für die jeweilige Ausführung die folgenden Informationen:
   - Informationen zu den **Ausführungseigenschaften**
   - Dateien mit **Ausgaben**
   - **Visualisierungen**, falls vorhanden
   - **Protokolle** 

   Nach Abschluss der Ausführung werden im Popupfenster die folgenden Ergebnisse angezeigt:

   >[!NOTE]
   >Da wir für den Trainingssatz zuvor eine zufällige Anordnung verwendet haben, weichen Ihre Ergebnisse unter Umständen von den hier gezeigten Ergebnissen ab.

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

11. Führen Sie zusätzliche Ausführungen aus. 

    Geben Sie im Feld **Argumente** eine Reihe verschiedener numerischer Werte von `0.001` bis `10` ein. Wählen Sie **Ausführen**, um den Code einige weitere Male auszuführen. Der jeweils geänderte Argumentwert wird in den Algorithmus für die logistische Regression im Code eingefügt. Dies führt jeweils zu unterschiedlichen Ergebnissen.

## <a name="review-the-run-history-in-detail"></a>Ausführliches Prüfen des Ausführungsverlaufs
In Azure Machine Learning Workbench wird jede Skriptausführung als Datensatz des Ausführungsverlaufs erfasst. Wenn Sie die Ansicht **Ausführungen** öffnen, können Sie den Ausführungsverlauf eines bestimmten Skripts anzeigen.

1. Wählen Sie zum Öffnen der Liste mit den **Ausführungen** in der linken Symbolleiste die Schaltfläche **Ausführungen** (Uhrsymbol). Wählen Sie anschließend **iris_sklearn.py**, um das **Ausführungsdashboard** von `iris_sklearn.py` anzuzeigen.

   ![Ausführen der Ansicht](media/tutorial-classifying-iris/run_view.png)

2. Die Registerkarte **Run Dashboard** (Ausführungsdashboard) wird geöffnet. Überprüfen Sie die in mehreren Ausführungen erfassten Statistiken. Die Graphen werden oben in der Registerkarte gerendert. Jede Ausführung verfügt über eine fortlaufende Nummer, und die Ausführungsdetails sind in der Tabelle unten auf dem Bildschirm aufgeführt.

   ![Ausführungsdashboard](media/tutorial-classifying-iris/run_dashboard.png)

3. Filtern Sie die Tabelle, und wählen Sie dann einen beliebigen Graphen aus, um für jede Ausführung Status, Dauer, Genauigkeit und Regularisierungsrate anzuzeigen. 

4. Wählen Sie zwei oder drei Ausführungen in der Tabelle **Ausführungen** aus, und wählen Sie anschließend die Schaltfläche **Vergleichen**, um einen Bereich mit ausführlichen Vergleichsdaten zu öffnen. Überprüfen Sie die Gegenüberstellung der Ausführungen. Wählen Sie oben links im Bereich **Vergleich** die Zurück-Schaltfläche **Ausführungsliste**, um wieder zurück zum **Ausführungsdashboard** zu wechseln.

5. Wählen Sie eine einzelne Ausführung aus, um die Ausführungsdetailansicht anzuzeigen. Beachten Sie, dass die Statistiken für die ausgewählte Ausführung im Abschnitt **Run Properties** (Ausführungseigenschaften) aufgeführt sind. Die Dateien, die in den Ausgabeordner geschrieben werden, sind im Abschnitt **Ausgaben** aufgeführt, und Sie können sie dort herunterladen.

   ![Ausführungsdetails](media/tutorial-classifying-iris/run_details.png)

   Die beiden Diagramme, d.h. die Konfusionsmatrix und die ROC-Kurve für mehrere Klassen, werden im Abschnitt **Visualisierungen** gerendert. Alle Protokolldateien werden zudem im Abschnitt **Protokolle** angezeigt.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Ausführen von Skripts in der lokalen Docker-Umgebung

Mit Machine Learning können Sie zusätzliche Ausführungsumgebungen wie Docker leicht konfigurieren und Ihr Skript in diesen Umgebungen ausführen. 

>[!IMPORTANT]
>Für diesen Schritt muss ein Docker-Modul lokal installiert sein und gestartet werden. Weitere Informationen finden Sie der Anleitung zur Docker-Installation.

1. Wählen Sie im linken Bereich das Symbol **Ordner**, um die Liste **Dateien** für Ihr Projekt zu öffnen. Erweitern Sie den Ordner `aml_config`. 

2. Es sind mehrere Umgebungen vorkonfiguriert, beispielsweise **docker-python**, **docker-spark** und **Lokal**. 

   Jeder Umgebung sind zwei Dateien zugewiesen, z.B. `docker-python.compute` und `docker-python.runconfig`. Wenn Sie die einzelnen Dateien öffnen, sehen Sie, dass bestimmte Optionen im Text-Editor konfiguriert werden können.  

   Wählen Sie zum Bereinigen in den Registerkarten für geöffnete Text-Editoren die Option **Schließen** (**x**).

3. Führen Sie das Skript **iris_sklearn.py** in der Umgebung **docker-python** aus: 

   - Klicken Sie in der linken Symbolleiste auf das **Uhr**-Symbol, um den Bereich **Ausführungen** zu öffnen. Wählen Sie **All Runs** (Alle Ausführungen). 
   - Wählen Sie oben auf der Registerkarte **All Runs** (Alle Ausführungen) anstelle der Standardumgebung **Lokal** die Umgebung **docker-python** als Zielumgebung aus. 
   - Wählen Sie als Nächstes auf der rechten Seite **iris_sklearn.py** als auszuführendes Skript aus. 
   - Lassen Sie das Feld **Argumente** leer, da das Skript einen Standardwert angibt. 
   - Wählen Sie die Schaltfläche **Ausführen**.

4. Sie sehen, dass ein neuer Auftrag gestartet wird. Er wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet.

   Wenn Sie das Skript erstmalig für die Docker-Umgebung ausführen, dauert die Ausführung einige Minuten länger. 

   Im Hintergrund wird von Azure Machine Learning Workbench eine neue Docker-Datei erstellt. 
   In der neuen Datei wird auf das Docker-Basisimage verwiesen, das in der Datei `docker.compute` angegeben ist, sowie auf die davon abhängigen Python-Pakete in der Datei `conda_dependencies.yml`. 
   
   Mit dem Docker-Modul werden die folgenden Aufgaben durchgeführt:

    - Herunterladen des Basisimage aus Azure
    - Installieren der Python-Pakete, die in der Datei `conda_dependencies.yml` angegeben sind
    - Starten eines Docker-Containers
    - Kopieren von bzw. Verweisen auf die lokale Kopie des Projektordners (je nach Ausführungskonfiguration)      
    - Ausführen des Skripts `iris_sklearn.py`

   Am Ende sollte genau das gleiche Ergebnis wie beim Angeben von **Lokal** als Ziel angezeigt werden.

5. Als Nächstes testen wir Spark. Das Docker-Basisimage enthält eine vorinstallierte und konfigurierte Spark-Instanz. Daher können Sie ein PySpark-Skript darin ausführen. Dies ist eine einfache Möglichkeit zum Entwickeln und Testen Ihres Spark-Programms, ohne Spark mit viel Zeitaufwand selbst installieren und konfigurieren zu müssen. 

   Öffnen Sie die Datei `iris_spark.py`. Mit diesem Skript wird die Datendatei `iris.csv` geladen und das Iris-Dataset mithilfe des Algorithmus für die logistische Regression aus der Spark Machine Learning-Bibliothek klassifiziert. Ändern Sie nun die Ausführungsumgebung in **docker-spark** und das Skript in **iris_spark.py**, und führen Sie es anschließend erneut aus. Dieser Prozess dauert etwas länger, da eine Spark-Sitzung erstellt und im Docker-Container gestartet werden muss. Sie können auch sehen, dass sich der stdout-Text von dem stdout-Text von `iris_spark.py` unterscheidet.

6. Führen Sie einige weitere Ausführungen aus, und testen Sie unterschiedliche Argumente. 

7. Öffnen Sie die Datei `iris_spark.py`, um das Modell für die logistische Regression anzuzeigen, das mit der Spark Machine Learning-Bibliothek erstellt wurde. 

8. Interagieren Sie mit dem Bereich **Aufträge**, führen Sie einen Vorgang für die Listenansicht des Verlaufs aus, und zeigen Sie eine Detailansicht Ihrer Ausführungen in verschiedenen Ausführungsumgebungen an.

## <a name="execute-scripts-in-the-azure-machine-learning-cli-window"></a>Ausführen von Skripts im Azure Machine Learning-CLI-Fenster

1. Wählen Sie in Azure Machine Learning Workbench das Befehlszeilenfenster, wählen Sie das Menü **Datei**, und wählen Sie dann die Option **Eingabeaufforderung öffnen**. Die Eingabeaufforderung wird im Projektordner mit der Aufforderung `C:\Temp\myIris\>` geöffnet.

   >[!IMPORTANT]
   >Sie müssen das Befehlszeilenfenster (über Workbench gestartet) verwenden, um die folgenden Schritte ausführen zu können.

2. Verwenden Sie die Eingabeaufforderung, um sich an Azure anzumelden. 

   Die Workbench-Anwendung und die CLI verwenden unabhängige Anmeldeinformationscaches bei der Authentifizierung von Azure-Ressourcen. Diesen Vorgang müssen Sie nur einmal durchführen, bis das zwischengespeicherte Token abläuft. Mit dem Befehl **az account list** wird die Liste der für Ihre Anmeldung verfügbaren Abonnements zurückgegeben. Verwenden Sie den ID-Wert aus dem gewünschten Abonnement, wenn mehr als ein Abonnement vorhanden ist. Legen Sie dieses Abonnement als Standardkonto für die Verwendung mit dem Befehl **az account set -s** fest, und geben Sie dann den ID-Wert des Abonnements an. Bestätigen Sie die Einstellung dann mit dem Befehl **account show**.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Geben Sie nach Abschluss der Authentifizierung und der Festlegung des aktuellen Azure-Abonnementkontexts im CLI-Fenster die folgenden Befehle ein, um **matplotlib** zu installieren, und übermitteln Sie das Python-Skript anschließend als auszuführendes Experiment.

   ```azurecli
   REM you don't need to run this command if you have installed matplotlib locally from the previous steps
   pip install matplotlib
   
   REM kicks off an execution of the iris_sklearn.py file against the local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Überprüfen Sie die Ausgabe. Sie verfügen über die gleiche Ausgabe und die gleichen Ergebnisse, wie bei Verwendung von Workbench zum Ausführen des Skripts. 

5. Führen Sie dasselbe Skript in der Docker-Ausführungsumgebung erneut aus, wenn Docker auf Ihrem Computer installiert ist.

   ```azurecli
   REM executes iris_sklearn.py in the local Docker container Python environment
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM executes iris_spark.py in the local Docker container Spark environment
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Wählen Sie in Workbench im linken Bereich das Symbol **Ordner** aus, um die Projektdateien aufzulisten, und öffnen Sie das Python-Skript mit dem Namen **run.py**. 

   Dieses Skript ist nützlich, um eine Schleife durch verschiedene Regularisierungsraten durchzuführen. Führen Sie das Experiment mehrfach mit diesen Raten aus. Mit diesem Skript wird der Auftrag `iris_sklearn.py` mit der Regularisierungsrate `10.0` (extrem hoher Wert) gestartet. Über das Skript wird die Rate während der folgenden Ausführung halbiert usw., bis der Mindestwert `0.005` erreicht ist. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Führen Sie die folgenden Befehle aus, um das Skript **run.py** über die Befehlszeile zu öffnen:

   ```cmd
   REM submits iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Wenn `run.py` abgeschlossen ist, wird in Workbench in der Listenansicht des Ausführungsverlaufs ein Graph angezeigt.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Ausführen von Skripts in einem Docker-Container auf einem Remotecomputer
Um Ihr Skript in einem Docker-Container auf einem Linux-Remotecomputer auszuführen, müssen Sie über SSH-Zugriff (Benutzername und Kennwort) für den Remotecomputer verfügen. Zudem muss auf dem Remotecomputer ein Docker-Modul installiert sein und ausgeführt werden. Am einfachsten erhalten Sie einen solchen Linux-Computer, indem Sie in Azure einen [Ubuntu-basierten virtuellen Data Science-Computer (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) erstellen. 

>[!NOTE] 
>Die CentOS-basierte DSVM wird *nicht* unterstützt.

1. Nach der Erstellung der VM können Sie sie als Ausführungsumgebung anfügen, wenn Sie ein Paar mit einer `.runconfig`- und einer `.compute`-Datei generieren. Verwenden Sie den folgenden Befehl, um die Dateien zu generieren. Wir nennen die neue Umgebung `myvm`.
 
   ```azurecli
   REM creates an myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >Die IP-Adresse kann auch ein öffentlich adressierbarer vollqualifizierter Domänenname (FQDN) sein, z.B. `vm-name.southcentralus.cloudapp.azure.com`. Es ist eine bewährte Methode, Ihrer DSVM einen FQDN hinzuzufügen und hier anstelle einer IP-Adresse zu verwenden. Diese Methode ist sinnvoll, da Sie die VM später ggf. einmal deaktivieren, um Kosten zu sparen. Darüber hinaus kann sich beim nächsten Start des virtuellen Computers die IP-Adresse geändert haben.

   Führen Sie nun den folgenden Befehl aus, um das Docker-Image auf dem virtuellen Computer zu erstellen und diesen für die Ausführung der Skripts einzurichten:
   
   ```azurecli
   REM prepares the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >Sie können zudem den Wert für `PrepareEnvironment` in `myvm.runconfig` vom Standardwert `false` in `true` ändern. Durch diese Änderung wird der Docker-Container bei der ersten Ausführung automatisch vorbereitet.

2. Bearbeiten Sie die generierte Datei `myvm.runconfig` unter `aml_config`, und ändern Sie das Standardframework `PySpark` in `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Es sollte auch funktionieren, wenn Sie die Frameworkeinstellung PySpark beibehalten. Die beschriebene Vorgehensweise ist aber effizienter, falls zum Ausführen des Python-Skripts eigentlich keine Spark-Sitzung erforderlich ist.

3. Führen Sie den gleichen Befehl wie zuvor im CLI-Fenster aus, aber mit der Ausnahme, dass dieses Mal _myvm_ als Ziel angegeben wird:
   ```azurecli
   REM executes iris_sklearn.py in a remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Der Befehl wird genau wie in einer Umgebung vom Typ `docker-python` ausgeführt, aber jetzt erfolgt die Ausführung auf dem virtuellen Linux-Remotecomputer. Im CLI-Fenster werden die gleichen Ausgabeinformationen angezeigt.

4. Wir probieren nun die Verwendung von Spark im Container aus. Öffnen Sie den Datei-Explorer. Sie können diesen Vorgang auch im CLI-Fenster durchführen, wenn Sie mit den allgemeinen Befehlen zur Dateibearbeitung vertraut sind. Erstellen Sie eine Kopie der Datei `myvm.runconfig`, und geben Sie ihr den Namen `myvm-spark.runconfig`. Bearbeiten Sie die neue Datei, und ändern Sie die Einstellung `Framework` von `Python` in `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Nehmen Sie an der Datei `myvm.compute` keine Änderungen vor. Dasselbe Docker-Image auf demselben virtuellen Computer wird für die Spark-Ausführung verwendet. In der neuen Datei `myvm-spark.runconfig` verweist das Feld `target` über ihren Namen `myvm` auf dieselbe Datei `myvm.compute`.

5. Geben Sie den folgenden Befehl ein, um ihn in der Spark-Instanz im Docker-Remotecontainer auszuführen:
   ```azureli
   REM executes iris_spark.py in a Spark instance on a remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Ausführen des Skripts in einem HDInsight-Cluster
Sie können dieses Skript auch in einem tatsächlichen Spark-Cluster ausführen. 

1. Wenn Sie Zugriff auf einen Spark für Azure HDInsight-Cluster haben, generieren Sie wie hier gezeigt einen Befehl für die HDInsight-Laufzeitkonfiguration. Geben Sie den Namen des HDInsight-Clusters sowie Ihren Benutzernamen und Ihr Kennwort für HDInsight als Parameter an. Verwenden Sie den folgenden Befehl:

   ```azurecli
   REM creates a compute target that points to a HDInsight cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepares the HDInsight cluster
   az ml experiment prepare -c myhdi
   ```

   Der FQDN des Clusterhauptknotens lautet normalerweise `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >Der `username` ist der SSH-Benutzername für den Cluster. Der Standardwert ist `sshuser`, wenn Sie ihn während der HDInsight-Einrichtung nicht ändern. Der Wert lautet nicht `admin`. Dies ist der andere Benutzer, der während der Einrichtung erstellt wurde, um Zugriff auf die Administratorwebsite des Clusters zu ermöglichen. 

2. Mit dem folgenden Befehl wird das Skript im HDInsight-Cluster ausgeführt:

   ```azurecli
   REM executes iris_spark on the HDInsight cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Wenn Sie den Befehl für einen HDInsight-Remotecluster ausführen, können Sie auch die Details zur YARN-Auftragsausführung (Yet Another Resource Negotiator) unter `https://<cluster_name>.azurehdinsight.net/yarnui` anzeigen, indem Sie das Benutzerkonto `admin` verwenden.


## <a name="next-steps"></a>Nächste Schritte
In diesem zweiten Teil der dreiteiligen Tutorialreihe wurde beschrieben, wie Sie Azure Machine Learning-Dienste für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Verwenden von Azure Machine Learning Workbench
> * Öffnen von Skripts und Überprüfen von Code
> * Ausführen von Skripts in einer lokalen Umgebung
> * Prüfen des Ausführungsverlaufs
> * Ausführen von Skripts in einer lokalen Docker-Umgebung
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer Docker-Remoteumgebung
> * Ausführen von Skripts in einer HDInsight-Cloudumgebung

Sie können nun mit dem dritten Teil der Reihe fortfahren. Sie haben das logistische Regressionsmodell erstellt und können es nun als Echtzeit-Webdienst bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen eines Modells](tutorial-classifying-iris-part-3.md)

---
title: "Erstellen eines Modells für Azure Machine Learning-Dienste (Vorschau) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschau) verwendet werden können. Dies ist Teil 2 zum Thema Experimentieren."
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
ms.openlocfilehash: 976407daee45e2f3a8360c1316227cc3399ad43e
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="classifying-iris-part-2-build-a-model"></a>Klassifizieren von Iris, Teil 2: Erstellen eines Modells
Azure Machine Learning-Dienste (Vorschau) sind eine integrierte End-to-End-Lösung für den Bereich Data Science und Advanced Analytics, mit der professionelle Datenanalysten Daten vorbereiten, Experimente entwickeln und Modelle in der Cloud bereitstellen können.

Dieses Tutorial ist der zweite Teil einer dreiteiligen Reihe. In diesem Teil des Tutorials verwenden Sie Azure Machine Learning-Dienste (Vorschauversion), um Folgendes zu lernen:

> [!div class="checklist"]
> * Arbeiten in Azure Machine Learning Workbench
> * Öffnen von Skripts und Überprüfen von Code
> * Ausführen von Skripts in einer lokalen Umgebung
> * Überprüfen des Ausführungsverlaufs
> * Ausführen von Skripts in einer lokalen Docker-Umgebung
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer Docker-Remoteumgebung
> * Ausführen von Skripts in einer HDInsight-Cloudumgebung

In diesem Tutorial wird das zeitlose Schwertlilien-DataSet ([Iris flower dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set)) verwendet, um die Dinge einfach zu halten. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für macOS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
Arbeiten Sie zunächst den ersten Teil dieser Tutorialreihe durch. Führen Sie vor dem vorliegenden Tutorial die Schritte im [Tutorial zum Vorbereiten von Daten](tutorial-classifying-iris-part-1.md) aus, um Azure Machine Learning-Ressourcen zu erstellen und die Azure Machine Learning Workbench-Anwendung zu installieren.

Sie können optional mit der Ausführung von Skripts für einen lokalen Docker-Container experimentieren. Hierzu muss auf Ihrem Windows- oder macOS-Computer ein lokal installiertes und gestartetes Docker-Modul vorhanden sein. (Die Community Edition ist ausreichend.) Weitere Informationen finden Sie in der [Docker-Installationsanleitung](https://docs.docker.com/engine/installation/).

Wenn Sie mit der Ausführung des Skripts in einem Docker-Container auf einem virtuellen Azure-Remotecomputer oder in einem HDInsight Spark-Cluster experimentieren möchten, befolgen Sie die [Anweisungen zum Erstellen eines Ubuntu-basierten virtuellen Data Science-Azure-Computers oder HDI-Clusters](how-to-create-dsvm-hdi.md).

## <a name="review-irissklearnpy-and-configuration-files"></a>Überprüfen der Datei „iris_sklearn.py“ und der Konfigurationsdatei
1. Starten Sie die Anwendung **Azure Machine Learning Workbench**, und öffnen Sie das Projekt **myIris**, das Sie im vorherigen Teil der Tutorialreihe erstellt haben.

2. Klicken Sie nach dem Öffnen des Projekts in Azure Machine Learning Workbench auf der linken Symbolleiste auf die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

3. Wenn Sie die Datei **iris_sklearn.py** auswählen, wird der Python-Code auf einer neuen Registerkarte des Text-Editors geöffnet.

   ![Öffnen der Datei](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Der Code, den Sie sehen, entspricht möglicherweise nicht genau dem oben abgebildeten Codebeispiel, da dieses Beispielprojekt regelmäßig aktualisiert wird.

4. Sehen Sie sich den Code des Python-Skripts an, um sich mit dem Codierungsstil vertraut zu machen. Das Skript führt folgende Aufgaben aus:

   - Lädt das Datenvorbereitungspaket **iris.dprep** zum Erstellen eines [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Wir verwenden das zum Beispielprojekt gehörende Datenvorbereitungspaket `iris.dprep`, das identisch sein sollte mit der Datei `iris-1.dprep`, die Sie in Teil 1 dieses Tutorials erstellt haben.

   - Fügt zufällige Funktionen hinzu, um die Lösung des Problems zu erschweren. (Zufälligkeit ist erforderlich, da Iris ein kleines DataSet ist, das mit nahezu 100%iger Genauigkeit einfach klassifiziert werden kann.)

   - Verwendet die Bibliothek [scikit-learn](http://scikit-learn.org/stable/index.html) für maschinelles Lernen zum Erstellen eines einfachen logistischen Regressionsmodells. 

   - Serialisiert das Modell mithilfe der Bibliothek [pickle](https://docs.python.org/2/library/pickle.html) in eine Datei im Ordner `outputs`, lädt es dann und deserialisiert es wieder in den Arbeitsspeicher.

   - Verwendet das deserialisierte Modell zur Vorhersage für einen neuen Datensatz. 

   - Zeichnet zwei Diagramme – Konfusionsmatrix und mehrklassige ROC-Kurve –mithilfe der Bibliothek [matplotlib](https://matplotlib.org/) und speichert sie im Ordner `outputs`.

   - Das Objekt `run_logger` wird im gesamten Datensatz verwendet, um die Regularisierungsrate und die Modellgenauigkeit in Protokollen zu erfassen. Die Protokolle werden automatisch im Ausführungsverlauf dargestellt.


## <a name="execute-irissklearnpy-script-in-local-environment"></a>Ausführen des Skripts „iris_sklearn.py“ in einer lokalen Umgebung

Zunächst bereiten wir das Skript **iris_sklearn.py** für die erste Ausführung vor. Für dieses Skript sind die Pakete „scikit-learn“ und „matplotlib“ erforderlich. **scikit-learn** wurde von Azure ML Workbench bereits installiert. **matplotlib** muss hingegen installiert werden. 

1. Klicken Sie in Azure Machine Learning Workbench auf das Menü **Datei**, und wählen Sie **Eingabeaufforderung öffnen** aus, um die Eingabeaufforderung zu starten. Dieses Befehlszeilenschnittstellenfenster bezeichnen wir als Azure Machine Learning Workbench-CLI-Fenster oder kurz als CLI-Fenster.

2. Geben Sie im CLI-Fenster den folgenden Befehl ein, um das Python-Paket **matplotlib** zu installieren. Die Installation sollte in weniger als einer Minute abgeschlossen sein.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Wenn Sie den Befehl `pip install` oben überspringen, wird der Code in `iris_sklearn.py` zwar erfolgreich ausgeführt, die Diagramme für die Konfusionsmatrix und die mehrklassige ROC-Kurve, wie sie in den Verlaufsvisualisierungen dargestellt sind, werden jedoch nicht erstellt.

3. Kehren Sie zum Fenster der Workbench-Anwendung zurück. 

4. Klicken Sie oben links auf der Registerkarte **iris_sklearn.py** neben dem Speichersymbol auf die Dropdownliste, um die Option für die **Ausführungskonfiguration** auszuwählen.  Wählen Sie **Lokal** als Ausführungsumgebung und `iris_sklearn.py` als auszuführendes Skript aus.

5. Geben Sie dann rechts auf derselben Registerkarte im Feld **Argumente** den Wert `0.01` ein. 

   ![Abbildung](media/tutorial-classifying-iris/run_control.png)

6. Klicken Sie auf die Schaltfläche **Ausführen**. Ein Auftrag wird sofort geplant. Der Auftrag wird im Bereich **Aufträge** rechts im Workbench-Fenster aufgelistet. 

7. Nach kurzer Zeit wechselt der Status des Auftrags von **Wird übermittelt** zu **Wird ausgeführt** und kurz danach schließlich zu **Abgeschlossen**.

   ![Ausführen von „sklearn“](media/tutorial-classifying-iris/run_sklearn.png)

8. Klicken Sie im Bereich „Aufträge“ auf das Wort **Abgeschlossen** im Auftragsstatustext. Im daraufhin geöffneten Popupfenster wird der Standardausgabetext (stdout) des ausgeführten Skripts angezeigt. Um den stdout-Text zu schließen, klicken Sie oben rechts im Popupfenster auf die Schaltfläche **X**.

9. Klicken Sie im gleichen Auftragsstatus im Bereich „Aufträge“ direkt über dem Status **Abgeschlossen** und der Startzeit auf den blauen Text **iris_sklearn.py [n]** (_n_ ist die Ausführungsanzahl). Auf der daraufhin geöffneten Seite **Run Properties** (Ausführungseigenschaften) werden Informationen zu den Ausführungseigenschaften sowie **Ausgabedateien**, eventuelle **Visualisierungen** und **Protokolle** der jeweiligen Ausführung angezeigt. 

   Nach Abschluss der Ausführung werden im Popupfenster die folgenden Ergebnisse angezeigt:

   >[!NOTE]
   >Da wir zuvor zufällige Funktionen in den Trainingssatz eingefügt haben, weichen Ihre genauen Ergebnisse etwas davon ab.

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
   Confusion matrix and ROC curve plotted. See them in Run History details page.
   ```

10. Schließen Sie die Registerkarte **Run Properties** (Ausführungseigenschaften), und kehren Sie zur Registerkarte **iris_sklearn.py** zurück. 

11. Führen Sie zusätzliche Ausführungen aus. 

    Geben Sie im Feld **Argumente** eine Reihe verschiedener numerischer Werte von `0.001` bis `10` ein. Klicken Sie auf **Ausführen**, um den Code einige weitere Male auszuführen. Der jeweils geänderte Argumentwert wird in den Logistic Regression-Algorithmus im Code eingefügt. Dies führt jeweils zu unterschiedlichen Ergebnissen.

## <a name="review-run-history-in-detail"></a>Überprüfen des Ausführungsverlaufs im Detail
In Azure Machine Learning Workbench wird jede Skriptausführung als Datensatz des Ausführungsverlaufs erfasst. Sie können den Ausführungsverlauf eines bestimmten Skripts durch Öffnen der Ansicht **Ausführungen** anzeigen.

1. Klicken Sie auf die Schaltfläche **Ausführungen** (Uhrsymbol) auf der linken Symbolleiste, um die Liste der **Ausführungen** zu öffnen. Klicken Sie dann auf **iris_sklearn.py**, um das **Ausführungsdashboard** von `iris_sklearn.py` anzuzeigen.

   ![Abbildung](media/tutorial-classifying-iris/run_view.png)

2. Die Registerkarte **Run Dashboard** (Ausführungsdashboard) wird geöffnet. Überprüfen Sie die in mehreren Ausführungen erfassten Statistiken. Diagramme werden oben auf der Registerkarte dargestellt. Und jede nummerierte Ausführung ist mit Details in der Tabelle unten auf der Seite aufgeführt.

   ![Abbildung](media/tutorial-classifying-iris/run_dashboard.png)

3. Filtern Sie die Tabelle, und klicken Sie interaktiv in die Diagramme, um Status, Dauer, Genauigkeit und Regularisierungsrate der einzelnen Ausführungen anzuzeigen. 

4. Wählen Sie zwei oder drei Ausführungen in der Tabelle **Ausführungen** aus, und klicken Sie auf die Schaltfläche **Vergleichen**, um eine Seite mit einem ausführlichen Vergleich zu öffnen. Überprüfen Sie die Gegenüberstellung der Ausführungen. Klicken Sie auf die Zurück-Schaltfläche **Ausführungsliste** oben links auf der Vergleichsseite, um wieder zum **Ausführungsdashboard** zurückzukehren.

5. Klicken Sie auf eine einzelne Ausführung, um die Ausführungsdetailansicht anzuzeigen. Beachten Sie, dass die Statistiken für die ausgewählte Ausführung im Abschnitt _Run Properties_ (Ausführungseigenschaften) aufgeführt sind. Die in die Ausgabeordner geschriebenen Dateien sind im Abschnitt **Ausgabe** aufgeführt und können heruntergeladen werden.

   ![Abbildung](media/tutorial-classifying-iris/run_details.png)

   Die beiden Diagramme, d.h. die Konfusionsmatrix und die mehrklassige ROC-Kurve, werden im Abschnitt **Visualisierungen** dargestellt. Alle Protokolldateien werden zudem im Abschnitt **Protokolle** angezeigt.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Ausführen von Skripts in der lokalen Docker-Umgebung

Mit Azure ML können Sie einfach zusätzliche Ausführungsumgebungen wie Docker konfigurieren und Ihr Skript in diesen Umgebungen ausführen. 

>[!IMPORTANT]
>Für diesen Schritt muss das Docker-Modul lokal installiert sein und gestartet werden. Weitere Details finden Sie in der Installationsanweisung.

1. Wählen Sie auf der linken Symbolleiste das Ordnersymbol aus, um die Liste **Dateien** für Ihr Projekt zu öffnen. Erweitern Sie den Ordner `aml_config`. 

2. Beachten Sie, dass mehrere Umgebungen vorkonfiguriert sind, beispielsweise **docker-python**, **docker-spark** und **Lokal**. 

   Jeder Umgebung sind zwei Dateien zugewiesen, z.B. `docker-python.compute` und `docker-python.runconfig`. Wenn Sie die verschiedenen Dateitypen öffnen, können Sie feststellen, dass bestimmte Optionen im Text-Editor konfiguriert werden können.  

   Schließen („X“) Sie die Registerkarten, um alle geöffneten Text-Editoren zu bereinigen.

3. Führen Sie das Skript **iris_sklearn.py** in der Umgebung **docker-python** aus. 

   - Klicken Sie auf der linken Symbolleiste auf das Uhrsymbol, um den Bereich **Ausführungen** zu öffnen. Klicken Sie auf **All Runs** (Alle Ausführungen). 
   - Wählen Sie oben auf der Registerkarte **All Runs** (Alle Ausführungen) anstelle der Standardumgebung **Lokal** die Umgebung **docker-python** als Zielumgebung aus. 
   - Wählen Sie dann rechts davon **iris_sklearn.py** als auszuführendes Skript aus. 
   - Lassen Sie das Feld **Argumente** leer, da das Skript einen Standardwert angibt. 
   - Klicken Sie auf die Schaltfläche **Ausführen**.

4. Sie können nun sehen, dass im Bereich **Aufträge** rechts im Workbench-Fenster ein neuer Auftrag gestartet wird.

   Wenn Sie das Skript erstmalig für die Docker-Umgebung ausführen, dauert die Ausführung einige Minuten länger. 

   Azure Machine Learning Workbench erstellt im Hintergrund eine neue Docker-Datei, die auf das in der Datei `docker.compute` angegebene Docker-Basisimage und die in der Datei `conda_dependencies.yml` angegebenen Python-Abhängigkeitspakete verweist. Das Docker-Modul lädt dann das Basisimage von Azure herunter, installiert die in der Datei `conda_dependencies.yml` angegebenen Python-Pakete und startet dann einen Docker-Container. Anschließend wird die lokale Kopie des Projektordners kopiert (oder abhängig von der Ausführungskonfiguration auf diesen verwiesen) und dann das Skript `iris_sklearn.py` ausgeführt. Am Ende sollte genau das gleiche Ergebnis angezeigt werden, wie wenn **Lokal** als Ziel angegeben wird.

5. Testen wir nun Spark. Das Docker-Basisimage enthält eine vorinstallierte und konfigurierte Spark-Instanz. Daher können Sie ein PySpark-Skript darin ausführen. Dies ist eine einfache Möglichkeit zum Entwickeln und Testen Ihres Spark-Programms, ohne Spark zeitaufwendig selbst installieren und konfigurieren zu müssen. 

   Öffnen Sie die Datei `iris_spark.py` . Mit diesem Skript wird die Datendatei `iris.csv` geladen und das Iris-DataSet mithilfe des Logistic Regression-Algorithmus aus der Spark ML-Bibliothek klassifiziert. Ändern Sie nun die Ausführungsumgebung in **docker-spark** und das Skript in **iris_spark.py**, und führen Sie das Skript erneut aus. Dies dauert etwas länger, da eine Spark-Sitzung erstellt und im Docker-Container gestartet werden muss. Sie können auch sehen, dass sich der stdout-Text von dem stdout-Text von `iris_spark.py` unterscheidet.

6. Führen Sie einige weitere Ausführungen aus, und testen Sie unterschiedliche Argumente. 

7. Öffnen Sie die Datei `iris_spark.py`, um das einfache logistische Regressionsmodell anzuzeigen, das mit der Spark ML-Bibliothek erstellt wurde. 

8. Interagieren Sie mit dem Bereich **Aufträge**, der Listenansicht des Ausführungsverlaufs und der Ausführungsdetailansicht Ihrer Ausführungen in verschiedenen Ausführungsumgebungen.

## <a name="execute-scripts-in-the-azure-ml-cli-window"></a>Ausführen von Skripts im Azure ML-CLI-Fenster

1. Starten Sie mithilfe von Azure Machine Learning Workbench das Befehlszeilenfenster, indem Sie auf das Menü **Datei** und dann auf **Eingabeaufforderung öffnen** klicken. Die Eingabeaufforderung wird im Projektordner mit der Aufforderung `C:\Temp\myIris\>` geöffnet.

   >[!Important]
   >Sie müssen das Befehlszeilenfenster (über Workbench gestartet) verwenden, um die folgenden Schritte ausführen zu können:

2. Melden Sie sich über die Eingabeaufforderung (CLI) bei Azure an. 

   Die Workbench-Anwendung und die CLI verwenden unabhängige Anmeldeinformationscaches bei der Authentifizierung von Azure-Ressourcen. Diesen Vorgang müssen Sie nur einmal durchführen, bis das zwischengespeicherte Token abläuft. Mit dem Befehl **az account list** wird die Liste der für Ihre Anmeldung verfügbaren Abonnements zurückgegeben. Wenn mehrere vorhanden sind, verwenden Sie den ID-Wert des gewünschten Abonnements, und legen Sie dieses als Standardkonto für die Verwendung mit dem Befehl **az set account -s** fest, indem Sie den ID-Wert des Abonnements angeben. Bestätigen Sie die Einstellung dann mit dem Befehl „account show“.

   ```azurecli
   REM login using aka.ms/devicelogin site.
   az login
   
   REM list all Azure subscriptions you have access to. 
   az account list -o table
   
   REM set the current Azure subscription to the one you want to use.
   az set account -s <subscriptionId>
   
   REM verify your current subscription is set correctly
   az account show
   ```

3. Nach der Authentifizierung und der Festlegung des aktuellen Azure-Abonnementkontexts geben Sie im CLI-Fenster die folgenden Befehle ein, um „matplotlib“ zu installieren und das Python-Skript als auszuführendes Experiment zu übermitteln.

   ```azurecli
   REM You don't need to do this if you have installed matplotlib locally from the previous steps.
   pip install matplotlib
   
   REM Kick off an execution of the iris_sklearn.py file against local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Überprüfen Sie die Ausgabe. Beachten Sie, dass Ausgabe und Ergebnis identisch sind mit der Ausgabe und dem Ergebnis, als Sie weiter oben in diesem Tutorial das Skript über Workbench ausgeführt haben. 

5. Führen Sie dasselbe Skript in der Docker-Ausführungsumgebung aus, wenn Docker auf Ihrem Computer installiert ist.

   ```azurecli
   REM Execute iris_sklearn.py in local Docker container Python environment.
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM Execute iris_spark.py in local Docker container Spark environment.
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Klicken Sie in Azure Machine Learning Workbench auf das Ordnersymbol auf der linken Symbolleiste, um die Projektdateien aufzulisten, und öffnen Sie das Python-Skript **run.py**. 

   Dieses Skript ist nützlich, um verschiedene Regularisierungsraten zu durchlaufen und das Experiment mehrere Male mit diesen Raten auszuführen. Mit diesem Skript wird ein Auftrag vom Typ `iris_sklearn.py` mit einer Regularisierungsrate von `10.0` gestartet (ein übertrieben hoher Wert) und die Rate in der folgenden Ausführung um die Hälfte reduziert usw., bis die Rate nicht kleiner als `0.005` ist. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Führen Sie die folgenden Befehle aus, um das Skript **run.py** über die Befehlszeile zu starten:

   ```cmd
   REM Submit iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Wenn `run.py` abgeschlossen ist, wird in Azure Machine Learning Workbench in der Listenansicht des Ausführungsverlaufs ein Diagramm angezeigt.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Ausführen von Skripts in einem Docker-Container auf einem Remotecomputer
Um Ihr Skript in einem Docker-Container auf einem Linux-Remotecomputer auszuführen, müssen Sie über SSH-Zugriff (Benutzername und Kennwort) für den Remotecomputer verfügen. Zudem muss auf dem Remotecomputer das Docker-Modul installiert sein und ausgeführt werden. Am einfachsten erhalten Sie einen solchen Linux-Computer, indem Sie einen [Ubuntu-basierten virtuellen Data Science-Computer (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) in Azure erstellen. (Beachten Sie, dass die CentOS-basierte DSVM nicht unterstützt wird.) 

1. Nach dem Erstellen des virtuellen Computers können Sie ihn als Ausführungsumgebung hinzufügen, indem Sie mit dem folgenden Befehl das Dateipaar `.runconfig` und `.compute` generieren. Wir nennen die neue Umgebung `myvm`.
 
   ```azurecli
   REM create myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >Der IP-Adressbereich kann auch ein öffentlich adressierbarer vollqualifizierter Domänenname (FQDN) sein, z.B. `vm-name.southcentralus.cloudapp.azure.com`. Es empfiehlt sich, Ihrem virtuellen Data Science-Computer einen FQDN hinzuzufügen und diesen hier anstelle der IP-Adresse zu verwenden, da Sie den virtuellen Computer möglicherweise zu einem bestimmten Zeitpunkt ausschalten, um Kosten zu sparen. Darüber hinaus kann sich beim nächsten Start des virtuellen Computers die IP-Adresse geändert haben.

   Führen Sie nun den folgenden Befehl aus, um das Docker-Image im virtuellen Computer zu erstellen, um diesen für die Ausführung der Skripts einzurichten.
   
   ```azurecli
   REM prepare the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >Sie können zudem den Wert von `PrepareEnvironment` in `myvm.runconfig` vom Standardwert `false` in `true` ändern. Dadurch wird der Docker-Container bei der ersten Ausführung automatisch vorbereitet.

2. Bearbeiten Sie die generierte Datei `myvm.runconfig` unter `aml_config`, und ändern Sie das Standardframework `PySpark` in `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Auch die belassene Frameworkeinstellung „PySpark“ sollte problemlos funktionieren. Sie ist jedoch etwas ineffizient, wenn zum Ausführen des Python-Skripts eigentlich keine Spark-Sitzung erforderlich ist.

3. Führen Sie den gleichen Befehl wie zuvor im CLI-Fenster aus, mit der Ausnahme, dass dieses Mal _myvm_ als Ziel angegeben wird:
   ```azurecli
   REM execute iris_sklearn.py in remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Der Befehl wird genau wie in einer Umgebung vom Typ `docker-python` ausgeführt, außer dass die Ausführung auf dem virtuellen Linux-Remotecomputer erfolgt. Im CLI-Fenster werden die gleichen Ausgabeinformationen angezeigt.

4. Testen wir Spark im Container. Öffnen Sie den Datei-Explorer. (Sie können diesen Vorgang auch im CLI-Fenster durchführen, wenn Sie mit den allgemeinen Befehlen zur Dateibearbeitung vertraut sind.) Erstellen Sie eine Kopie der Datei `myvm.runconfig`, und geben Sie ihr den Namen `myvm-spark.runconfig`. Bearbeiten Sie die neue Datei, und ändern Sie die Einstellung `Framework` von `Python` in `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Nehmen Sie an der Datei `myvm.compute` keine Änderungen vor. Dasselbe Docker-Image auf demselben virtuellen Computer wird für die Spark-Ausführung verwendet. In der neuen Datei `myvy-spark.runconfig` verweist das Feld `target` über ihren Namen `myvm` auf dieselbe Datei `myvm.compute`.

5. Geben Sie den folgenden Befehl ein, um ihn in der Spark-Instanz im Docker-Remotecontainer auszuführen:
   ```azureli
   REM execute iris_spark.py in Spark instance on remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Ausführen des Skripts in einem HDInsight-Cluster
Sie können dieses Skript auch in einem tatsächlichen Spark-Cluster ausführen. 

1. Wenn Sie Zugriff auf einen Spark für Azure HDInsight-Cluster haben, generieren Sie wie gezeigt einen Befehl für die HDI-Ausführungskonfiguration. Geben Sie den Namen des HDInsight-Clusters sowie Ihren Benutzernamen und Ihr Kennwort für HDInsight als Parameter an. Verwenden Sie den folgenden Befehl:

   ```azurecli
   REM create a compute target that points to a HDI cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepare the HDI cluster
   az ml experiment prepare -c myhdi
   ```

   Der vollqualifizierte Domänenname (FQDN) des Clusterhauptknotens lautet normalerweise `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >Der `username` ist der SSH-Benutzername für den Cluster. Der Standardwert ist `sshuser`, wenn Sie ihn während der HDI-Bereitstellung nicht ändern. Es ist nicht `admin`. Dies ist der andere Benutzer, der während der Bereitstellung erstellt wurde, um Zugriff auf die Administratorwebsite des Clusters zu ermöglichen. 

2. Wenn Sie den folgenden Befehl ausführen, wird das Skript im HDInsight-Cluster ausgeführt:

   ```azurecli
   REM execute iris_spark on the HDI cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Wenn Sie den Befehl für einen HDI-Remotecluster ausführen, können Sie unter Verwendung des Benutzerkontos `admin` unter `https://<cluster_name>.azurehdinsight.net/yarnui` auch die Details zur YARN-Auftragsausführung anzeigen.


## <a name="next-steps"></a>Nächste Schritte
In diesem zweiten Teil der dreiteiligen Tutorialreihe wurde beschrieben, wie Sie Azure Machine Learning-Dienste für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Arbeiten in Azure Machine Learning Workbench
> * Öffnen von Skripts und Überprüfen von Code
> * Ausführen von Skripts in einer lokalen Umgebung
> * Überprüfen des Ausführungsverlaufs
> * Ausführen von Skripts in einer lokalen Docker-Umgebung
> * Ausführen von Skripts in einem lokalen Azure CLI-Fenster
> * Ausführen von Skripts in einer Docker-Remoteumgebung
> * Ausführen von Skripts in einer HDInsight-Cloudumgebung

Sie können nun mit dem dritten Teil der Reihe fortfahren. Wir haben das logistische Regressionsmodell erstellt und können es nun als Echtzeit-Webdienst bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen eines Modells](tutorial-classifying-iris-part-3.md)

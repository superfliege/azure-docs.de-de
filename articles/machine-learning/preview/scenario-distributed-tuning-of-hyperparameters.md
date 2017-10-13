---
title: Verteilte Optimierung von Hyperparametern mit Azure Machine Learning Workbench | Microsoft-Dokumentation
description: "In diesem Szenario wird die verteilte Optimierung von Hyperparametern unter Verwendung von Azure Machine Learning Workbench erläutert."
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.topic: article
ms.author: dmpechyo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ms.openlocfilehash: 643cea5cc134a2eb25a0dec4abefd9edca726332
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Verteilte Optimierung von Hyperparametern mit Azure Machine Learning Workbench

In diesem Szenario wird beschrieben, wie Sie mit Azure Machine Learning Workbench Hyperparameter von Machine Learning-Algorithmen, die die scikit-learn-API implementieren, mit horizontaler Hochskalierung optimieren. Dabei wird erläutert, wie ein Docker-Remotecontainer und ein Spark-Cluster als Ausführungs-Back-End zum Optimieren von Hyperparametern konfiguriert und verwendet werden.

## <a name="link-of-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys
Im Folgenden finden Sie den Link zum öffentlichen GitHub-Repository: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall

Viele Machine Learning-Algorithmen verfügen über einen oder mehrere Knöpfe, die als Hyperparameter bezeichnet werden. Diese Knöpfe ermöglichen das Optimieren von Algorithmen, um eine Optimierung ihrer Leistung für künftige Daten zu erzielen, gemessen anhand von benutzerdefinierten Metriken (z.B. Genauigkeit, AUC, RMSE). Datenanalysten müssen Wert für Hyperparameter angeben, wenn sie ein Modell mit Trainingsdaten erstellen und bevor die Zukunftstestdaten angezeigt werden. Wie können die Werte von Hyperparametern auf Grundlage der bekannten Trainingsdaten festgelegt werden, sodass das Modell bei den unbekannten Testdaten eine gute Leistung liefert? 

Ein gängiges Verfahren für das Optimieren von Hyperparametern ist eine *Rastersuche*, kombiniert mit der *Kreuzvalidierung*. Die Kreuzvalidierung ist ein Verfahren, bei dem bewertet wird, wie gut ein Modell, das mit einem Trainingsset trainiert wurde, für das Testset prognostiziert werden kann. Bei diesem Verfahren wird das Dataset in K Teilmengen unterteilt, und der Algorithmus wird in K Durchläufen trainiert (im Rahmen eines Roundrobin). Dies gilt für alle Teilmengen, mit Ausnahme einer Teilmenge, die als verbleibende Teilmenge bezeichnet wird. Wir berechnen den Durchschnittswert der Metriken von K Modellen für K verbleibende Teilmengen. Dieser Durchschnittswert, der *kreuzvalidierte Leistungsschätzwert*, hängt von den Werten der Hyperparameter ab, die beim Erstellen von K Modellen verwendet wurden. Beim Optimieren von Hyperparametern durchsuchen wir den Raum der möglichen Hyperparameterwerte, um diejenigen zu ermitteln, bei denen der Kreuzvalidierungs-Leistungsschätzwert optimiert wird. Die Rastersuche ist ein gängiges Suchverfahren, bei dem der Raum der möglichen Werte mehrerer Hyperparameter ein Kreuzprodukt der Gruppen von möglichen Werten einzelner Hyperparameter ist. 

Die Rastersuche unter Verwendung der Kreuzvalidierung kann sehr zeitaufwendig sein. Weist ein Algorithmus fünf Hyperparameter mit jeweils fünf möglichen Werten auf, und wir verwenden K=5 Teilmengen, müssen wir für eine komplette Rastersuche 5<sup>6</sup>=15625 Modelle trainieren. Glücklicherweise ist die Rastersuche mit Kreuzvalidierung ein komplett paralleles Verfahren, und alle diese Modelle können parallel trainiert werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
* Eine installierte Kopie der [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) nach dem [Schnellstarthandbuch zum Installieren und Erstellen](./quickstart-installation.md), um die Workbench zu installieren und Konten zu erstellen.
* In diesem Szenario wird davon ausgegangen, dass Sie die Azure ML Workbench mit einem lokal installierten Docker-Modul unter Windows 10 oder macOS ausführen. 
* Um das Szenario mit einem Docker-Remotecontainer auszuführen, stellen Sie die Ubuntu Data Science Virtual Machine (DSVM) bereit. Befolgen Sie dazu die entsprechenden [Anweisungen](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-provision-vm). Es wird empfohlen, einen virtuellen Computer mit mindestens 8 Kernen und 28 GB Arbeitsspeicher zu verwenden. D4-Instanzen von virtuellen Computern weisen eine solche Kapazität auf. 
* Zum Ausführen dieses Szenarios mit einem Spark-Cluster stellen Sie einen HDInsight-Cluster bereit; befolgen Sie dazu die entsprechenden [Anweisungen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters). Es wird empfohlen, einen Cluster mit mindestens sechs Workerknoten und mindestens 8 Kernen und 28 GB Arbeitsspeicher im Header- und Workerknoten zu verwenden. D4-Instanzen von virtuellen Computern weisen eine solche Kapazität auf. Zum Maximieren der Leistung des Clusters empfehlen wir, die Parameter „spark.executor.instances“, „spark.executor.cores“ und „spark.executor.memory“ entsprechend den [Anweisungen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-resource-manager) zu ändern und die Definitionen im Abschnitt „custom spark defaults“ zu ändern.

     **Problembehandlung**: Für Ihr Azure-Abonnement gilt möglicherweise ein Kontingent hinsichtlich der Kerne, die verwendet werden können. Das Azure-Portal lässt keine Erstellung eines Clusters zu, dessen Gesamtzahl von Kernen das Kontingent überschreitet. Ermitteln Sie das Kontingent, indem Sie im Azure-Portal in den Abschnitt „Abonnements“ wechseln und dort auf das Abonnement zum Bereitstellen eines Clusters und anschließend auf **Nutzung + Kontingente** klicken. Im Allgemeinen sind Kontingente nach Azure-Regionen definiert, und Sie können den Spark-Cluster in einer Region bereitstellen, in dem eine ausreichende Anzahl freier Kerne zur Verfügung steht. 

* Erstellen Sie ein Azure-Speicherkonto, mit dem das Dataset gespeichert wird. Befolgen Sie die [Anweisungen](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account) zum Erstellen eines Speicherkontos.

## <a name="data-description"></a>Datenbeschreibung

Wir verwenden das Dataset [TalkingData](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Dieses Dataset enthält Ereignisse aus den Apps in Mobiltelefonen. Das Ziel besteht darin, Geschlecht und Alterskategorie des Mobiltelefonnutzers zu ermitteln; Grundlage sind die Art des Telefons und die vom Nutzer kürzlich generierten Ereignisse.  

## <a name="scenario-structure"></a>Struktur des Szenarios
Für dieses Szenario sind mehrere Ordner im GitHub-Repository vorhanden. Code und Konfigurationsdateien befinden sich im Ordner **Code**, die gesamte Dokumentation im Ordner **Docs** und sämtliche Bilder im Ordner **Images**. Der Stammordner enthält eine README-Datei mit einer kurzen Zusammenfassung dieses Szenarios.

### <a name="getting-started"></a>Erste Schritte
Klicken Sie auf das Symbol für die Azure Machine Learning Workbench, um die Azure Machine Learning Workbench auszuführen, und erstellen Sie ein Projekt auf der Grundlage der Vorlage „Distributed Tuning of Hyperparameters“ (Verteilte Optimierung von Hyperparametern). Ausführliche Anweisungen zum Erstellen eines neuen Projekts finden Sie im [Schnellstart zum Installieren und Erstellen](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Konfigurations- und Ausführungsumgebungen
Wir veranschaulichen, wie Sie den Code in einem Docker-Remotecontainer und in einem Spark-Cluster ausführen. Dabei beginnen wir mit der Beschreibung der Einstellungen, die beiden Umgebungen gemeinsam sind. 

Wir verwenden die Pakete [scikit-learn](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost) und [azure-storage](https://pypi.python.org/pypi/azure-storage), die nicht im Docker-Standardcontainer der Azure Machine Learning Workbench bereitgestellt werden. Für das Paket „azure-storage“ müssen die Pakete [cryptography](https://pypi.python.org/pypi/cryptography) und [azure](https://pypi.python.org/pypi/azure) installiert werden. Um diese Pakete im Docker-Container und in den Knoten des Spark-Clusters zu installieren, ändern wir die Datei „conda_dependencies.yml“:

    name: project_environment
    channels:
      - conda-forge
    dependencies:
      - python=3.5.2
      - scikit-learn
      - xgboost
      - pip:
        - cryptography
        - azure
        - azure-storage

Die geänderte Datei „conda\_dependencies.yml“ ist im Verzeichnis „aml_config“ des Tutorials gespeichert. 

In den nächsten Schritten verbinden wir die Ausführungsumgebung mit dem Azure-Konto. Öffnen Sie ein Befehlszeilenfenster (CLI), indem Sie oben links in der AML Workbench im Menü „Datei“ auf „Eingabeaufforderung öffnen“ klicken. Führen Sie in der CLI Folgendes aus:

    az login

Die folgende Meldung wird ausgegeben:

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Wechseln Sie zu dieser Webseite, geben Sie den Code ein, und melden Sie sich bei Ihrem Azure-Konto an. Führen Sie nach diesem Schritt Folgendes in der CLI aus:

    az account list -o table

Suchen Sie anschließend nach der Abonnement-ID des Azure-Abonnements, das Ihr Konto für den AML Workbench-Arbeitsbereich enthält. Führen Sie schließlich Folgendes in der CLI aus:

    az account set -s <subscription ID>

um die Verbindung mit Ihrem Azure-Abonnement fertigzustellen.

In den nächsten zwei Abschnitten wird beschrieben, wie die Konfiguration des Docker-Remotecontainers und des Spark-Clusters abgeschlossen wird.

#### <a name="configuration-of-remote-docker-container"></a>Konfiguration des Docker-Remotecontainers

 Führen Sie zum Einrichten eines Docker-Remotecontainers Folgendes in der CLI aus:

    az ml computetarget attach --name dsvm --address <IP address> --username <username> --password <password> --type remotedocker

mit der IP-Adresse, dem Benutzernamen und dem Kennwort in DSVM. Die IP-Adresse des DSVM kann im Abschnitt „Übersicht“ Ihrer DSVM-Seite im Azure-Portal ermittelt werden:

![VM-IP](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Konfiguration des Spark-Clusters

Führen Sie zum Einrichten der Spark-Umgebung Folgendes in der CLI aus:

    az ml computetarget attach --name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> --type cluster

mit dem Namen des Clusters, dem SSH-Benutzernamen und -Kennwort des Clusters. Der Standardwert des SSH-Benutzernamens lautet `sshuser`, es sei denn, Sie haben ihn während des Bereitstellens des Clusters geändert. Den Namen des Clusters finden Sie im Abschnitt „Eigenschaften“ der Clusterseite im Azure-Portal:

![Clustername](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

Wir verwenden das Paket „spark-sklearn“, um Spark als Ausführungsumgebung für die verteilte Optimierung von Hyperparametern einzurichten. Wir haben die Datei „spark_dependencies.yml“ geändert, um dieses Paket zu installieren, wenn die Spark-Ausführungsumgebung verwendet wird:

    configuration: {}
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

Die geänderte Datei „spark\_dependencies.yml“ ist im Verzeichnis „aml_config“ des Tutorials gespeichert. 

### <a name="data-ingestion"></a>Datenerfassung
Bei dem Code in diesem Szenario wird davon ausgegangen, dass die Daten in Azure Blob Storage gespeichert sind. Wir zeigen anfangs, wie die Daten von der Kaggle-Website auf Ihren Computer heruntergeladen und in den Blobspeicher hochgeladen werden. Anschließend wird erläutert, wie die Daten aus dem Blobspeicher gelesen werden. 

Wechseln Sie zum Herunterladen von Daten von Kaggle zur [Dataset-Seite](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data), und klicken Sie auf die Schaltfläche „Download“. Sie werden aufgefordert, sich bei Kaggle anzumelden. Nach erfolgter Anmeldung werden Sie zur Dataset-Seite zurückgeleitet. Laden Sie nun die ersten sieben Dateien in der linken Spalte herunter, indem Sie sie auswählen und auf die Schaltfläche „Download“ klicken. Die Gesamtgröße der heruntergeladenen Dateien beträgt 289 MB. Erstellen Sie zum Hochladen dieser Dateien in den Blobspeicher den Blobspeichercontainer „dataset“ in Ihrem Speicherkonto. Hierzu können Sie zur Azure-Seite Ihres Speicherkontos wechseln und auf „Blobs“ und anschließend auf „+Container“ klicken. Geben Sie als Name „dataset“ ein, und klicken Sie auf „OK“. Diese Schritte werden in den folgenden Screenshots veranschaulicht:

![Blob öffnen](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![Container öffnen](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Wählen Sie anschließend in der Liste den Container „dataset“ aus, und klicken Sie auf die Schaltfläche „Hochladen“. Das Azure-Portal lässt das gleichzeitige Hochladen mehrerer Dateien zu. Klicken Sie im Abschnitt „Blob hochladen“ auf die Ordner-Schaltfläche, wählen Sie alle Dateien im Dataset aus, klicken Sie auf „Öffnen“ und anschließend auf „Hochladen“. Diese Schritte werden im folgenden Screenshot veranschaulicht:

![Blob hochladen](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Das Hochladen der Dateien kann je nach vorhandener Internetverbindung einige Minuten dauern. 

In unserem Code verwenden wir das [Azure Storage-SDK](https://azure-storage.readthedocs.io/en/latest/) zum Herunterladen des Datasets aus dem Blobspeicher in die aktuelle Ausführungsumgebung. Der Download erfolgt in der load\_data()-Funktion in der Datei „load_data.py“. Zum Verwenden dieses Codes müssen Sie <ACCOUNT_NAME> und <ACCOUNT_KEY> durch den Namen und den Primärschlüssel Ihres Speicherkontos ersetzen, welches das Dataset hostet. Der Kontoname wird in der linken oberen Ecke der Azure-Seite Ihres Speicherkontos angezeigt. Wählen Sie zum Abrufen des Kontoschlüssels auf der Azure-Seite des Speicherkontos „Zugriffsschlüssel“ (siehe erster Screenshot im Abschnitt zur Datenerfassung) aus, und kopieren Sie die lange Zeichenfolge in die erste Spalte der Schlüsselspalte:
 
![Zugriffsschlüssel](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

Mit dem folgenden Code aus der load_data()-Funktion wird eine einzige Datei heruntergeladen:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

Beachten Sie, dass Sie die Datei „load_data.py“ nicht manuell ausführen müssen. Sie wird später von anderen Dateien aufgerufen.

### <a name="feature-engineering"></a>Featureentwicklung
Der Code zum Berechnen aller Features befindet sich in der Datei „feature\_engineering.py“. Sie müssen die Datei „feature_engineering.py“ nicht manuell ausführen. Sie wird später von anderen Dateien aufgerufen.

Wir erstellen mehrere Featuresätze:
* One-Hot-Codierung von Marke und Modell des Mobiltelefons (one\_hot\_brand_model-Funktion)
* Anteil der Ereignisse, die von Benutzern an den einzelnen Wochentagen generiert wurden (weekday\_hour_features-Funktion)
* Anteil der Ereignisse, die von Benutzern in den einzelnen Stunden generiert wurden (weekday\_hour_features-Funktion)
* Anteil der Ereignisse, die von Benutzern in den einzelnen Kombinationen aus Wochentag und Stunde generiert wurden (weekday\_hour_features-Funktion)
* Anteil der Ereignisse, die von Benutzern in den einzelnen Apps generiert wurden (one\_hot\_app_labels-Funktion)
* Anteil der Ereignisse, die von Benutzern in den einzelnen App-Bezeichnungen generiert wurden (one\_hot\_app_labels-Funktion)
* Anteil der Ereignisse, die von Benutzern in den einzelnen App-Kategorien generiert wurden (text\_category_features-Funktion)
* Indikatorfeatures für Kategorien von Apps, die in generierten Ereignissen verwendet wurden (one\_hot_category-Funktion)

Diese Features wurden inspiriert durch Kaggle-Kernel [A linear model on apps and labels](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels) (Ein lineares Modell für Apps und Bezeichnungen).

Bei der Berechnung dieser Funktionen wird eine erhebliche Menge an Arbeitsspeicher belegt. Anfänglich haben wir versucht, Features in der lokalen Umgebung mit 16 GB RAM zu berechnen. Wir konnten die ersten vier Featuresätze berechnen, beim fünften Featuresatz hingegen wurde der Fehler „Nicht genügend Arbeitsspeicher“ ausgegeben. Die Berechnung der ersten vier Featuresätze befindet sich in der Datei „singleVMsmall.py“, und sie kann in der lokalen Umgebung durch Ausführen von 

     az ml experiment submit -c local .\singleVMsmall.py   

im CLI-Fenster ausgeführt werden.

Da die lokale Umgebung zu klein zum Berechnen aller Featuresätze ist, wechseln wir zum Remote-DSVM, wo ein größerer Arbeitsspeicher verfügbar ist. Die Ausführung in DSVM erfolgt im Docker-Container, der von der AML Workbench verwaltet wird. Mit diesem DSVM können wir sämtliche Features berechnen, Modelle trainieren und Hyperparameter optimieren (siehe nächster Abschnitt). Die Datei „singleVM.py“ enthält den kompletten Code zum Berechnen und Modellieren von Features. Im nächsten Abschnitt wird veranschaulicht, wie „singleVM.py“ im Remote-DSVM ausgeführt wird. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Optimierung von Hyperparametern mit Remote-DSVM
Wir verwenden die [xgboost](https://anaconda.org/conda-forge/xgboost)-Implementierung [1] der stufenweisen Strukturverstärkung. Mit dem Paket [scikit-learn](http://scikit-learn.org/) werden Hyperparameter von „xgboost“ optimiert. Obwohl „xgboost“ kein Teil des Pakets „scikit-learn“ ist, implementiert es die scikit-learn-API und kann daher zusammen mit Funktionen zum Optimieren von Hyperparametern von „scikit-learn“ verwendet werden. 

„xgboost“ weist acht Hyperparameter auf:
* n_esitmators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* subsample
* Ziel: Eine Beschreibung dieser Hyperparameter finden Sie [hier](http://xgboost.readthedocs.io/en/latest/python/python_api.html#module-xgboost.sklearn) und [hier](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md). Anfänglich verwenden wir das Remote-DSVM und optimieren Hyperparameter aus einem kleinen Raster von möglichen Werten:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Dieses Raster weist vier Kombinationen von Werten der Hyperparameter auf. Wir wenden die 5-fache Kreuzvalidierung an und erhalten 4x5=20 Durchläufe von „xgboost“. Zum Messen der Leistung der Modelle verwenden wir die negative Log-Loss-Metrik. Mit dem folgenden Code werden die Werte der Hyperparameter im Raster bestimmt, bei denen der kreuzvalidierte negative Log-Loss maximiert ist. Der Code verwendet diese Werte auch zum Trainieren des endgültigen Modells für das komplette Trainingsset:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Nach dem Erstellen des Modells speichern wir die Ergebnisse der Hyperparameteroptimierung. Mit der Protokollierungs-API von AML Workbench speichern wir die besten Werte der Hyperparameter und den entsprechenden kreuzvalidierten Schätzwert des negativen Log-Loss:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

Zudem erstellen wir die Datei „sweeping_results.txt“ mit kreuzvalidierten negativen Log-Loss-Werten aller Kombinationen von Hyperparameterwerten im Raster:

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Diese Datei wird im speziellen Verzeichnis „./outputs“ gespeichert. Später wird erläutert, wie dieses heruntergeladen wird.  

 Vor dem erstmaligen Ausführen von „singleVM.py“ im Remote-DSVM erstellen wir dort einen Docker-Container durch Ausführen von 

    az ml experiment prepare -c dsvm

im CLI-Fenster. Das Erstellen des Docker-Containers kann einige Minuten dauern. Anschließend führen wir „singleVM.py“ in DSVM aus:

    az ml experiment submit -c dsvm .\singleVM.py

Die Ausführung dieses Befehls wird in 1 Stunde 38 Minuten abgeschlossen, wenn DSVM 8 Kerne und 28 GB Speicher aufweist. Die protokollierten Werte können im Fenster „Ausführungsverlauf“ von AML Workbench angezeigt werden:

![Ausführungsverlauf](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

In der Standardeinstellung werden im Fenster „Ausführungsverlauf“ Werte und Diagramme für die ersten 1-2 protokollierten Werte angezeigt. Wenn Sie die komplette Liste der ausgewählten Werte von Hyperparametern anzeigen möchten, klicken Sie auf das Symbol „Einstellungen“, das im letzten Screenshot mit dem roten Kreis gekennzeichnet ist, und wählen Sie die in der Tabelle anzuzeigenden Hyperparameter aus. Klicken Sie zum Auswählen der Diagramme, die im oberen Teil des Fensters „Ausführungsverlauf“ angezeigt werden sollen, auf das Symbol „Einstellungen“ (gekennzeichnet mit dem blauen Kreis), und wählen Sie die Diagramme in der Liste aus. 

Die ausgewählten Werte für Hyperparameter können auch im Fenster „Run Properties“ (Ausführungseigenschaften) untersucht werden: 

![Ausführungseigenschaften](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

In der oberen rechten Ecke des Fensters „Run Properties“ (Ausführungseigenschaften) befindet sich der Abschnitt „Ausgabedateien“ mit der Liste aller Dateien, die im Ordner „.\output“ in der Ausführungsumgebung erstellt wurden. Die Datei „sweeping\_results.txt“ kann dort heruntergeladen werden; wählen Sie die Datei aus, und klicken Sie auf die Schaltfläche „Herunterladen“. Die Datei „sweeping_results.txt“ sollte folgende Ausgabe haben:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Optimierung von Hyperparametern mit Spark-Cluster
Mit einem Spark-Cluster werden Hyperparameter mit horizontaler Hochskalierung optimiert, und ein größeres Raster wird verwendet. Das neue Raster ist

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Dieses Raster weist 16 Kombinationen von Werten der Hyperparameter auf. Da die fünffache Kreuzvalidierung angewendet wird, führen wir „xgboost“ 16x5=80 Mal aus.

Das Paket „scikit-learn“ bietet keine native Unterstützung für das Optimieren von Hyperparametern mit einem Spark-Cluster. Diese Lücke wird zum Glück durch das Paket [spark-sklearn](https://spark-packages.org/package/databricks/spark-sklearn) von Databricks geschlossen. Dieses Paket stellt die GridSearchCV-Funktion bereit, die nahezu die gleiche API wie die GridSearchCV-Funktion in „scikit-learn“ aufweist. Zum Verwenden von „spark-sklearn“ und Optimieren von Hyperparametern mit Spark müssen wir einen Spark-Kontext erstellen:

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Anschließend ersetzen wir 

    from sklearn.model_selection import GridSearchCV

with 

    from spark_sklearn import GridSearchCV

Zudem ersetzen wir den Aufruf von GridSearchCV aus „scikit-learn“ durch den aus „spark-sklearn“:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

Der endgültige Code zum Optimieren von Hyperparametern mit Spark befindet sich in der Datei „distributed\_sweep.py“. Der Unterschied zwischen „singleVM.py“ und „distributed_sweep.py“ besteht in der Definition des Rasters und vier Codezeilen. Beachten Sie außerdem, dass sich der Protokollierungscode aufgrund der AML Workbench-Dienste nicht ändert, wenn von der Remote-DSVM- zur Spark-Cluster-Ausführungsumgebung gewechselt wird.

Vor dem erstmaligen Ausführen von „distributed_sweep.py“ im Spark-Cluster müssen wir dort Python-Pakete installieren. Dies kann erreicht werden durch die Ausführung von 

    az ml experiment prepare -c spark

im CLI-Fenster. Diese Installation dauert einige Minuten. Nach dem Ausführen von „distributed_sweep.py“ im Spark-Cluster:

    az ml experiment submit -c spark .\distributed_sweep.py

Die Ausführung dieses Befehls wird in 1 Stunde 6 Minuten abgeschlossen, wenn der Spark-Cluster 6 Workerknoten mit 28 GB Speicher aufweist. Die Ergebnisse der Optimierung von Hyperparametern im Spark-Cluster (Protokolle, beste Werte von Hyperparametern und die Datei „sweeping_results.txt“) können in der Azure Machine Learning Workbench auf die gleiche Weise wie in der DSVM-Remoteausführung aufgerufen werden. 

### <a name="architecture-diagram"></a>Architekturdiagramm

In der folgenden Abbildung wird der End-to-End-Workflow veranschaulicht: ![Architektur](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Zusammenfassung 

In diesem Szenario haben wir erläutert, wie mit der Azure Machine Learning Workbench die Optimierung von Hyperparametern auf virtuellen Remotecomputern und im Spark-Remotecluster ausgeführt wird. Wir haben festgestellt, dass die Azure Machine Learning Workbench Tools zum einfachen Konfigurieren von Ausführungsumgebungen und zum Wechsel zwischen diesen Umgebungen bereitstellt. 

## <a name="references"></a>Referenzen

[1] T. Chen und C. Guestrin. [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754). KDD 2016.





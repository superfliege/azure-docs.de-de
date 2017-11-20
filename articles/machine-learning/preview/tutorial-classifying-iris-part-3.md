---
title: "Bereitstellen eines Modells für Azure Machine Learning-Dienste (Vorschauversion) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschau) verwendet werden können. Dies ist der dritte Teil, in dem das Bereitstellen des Modells beschrieben wird."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 11/14/2017
ms.openlocfilehash: 7fa7eb53876746f1934af8ca3428cfdacb56382d
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="classify-iris-part-3-deploy-a-model"></a>Klassifizieren von Iris, Teil 3: Bereitstellen eines Modells
Bei Azure Machine Learning-Diensten (Vorschauversion) handelt es sich um eine integrierte Data Science- und Advanced Analytics-End-to-End-Lösung für professionelle Data Scientists. Data Scientists können die Lösung nutzen, um Daten vorzubereiten, Experimente zu entwickeln und Modelle für die Cloud bereitzustellen.

Dieses Tutorial ist der dritte Teil einer dreiteiligen Reihe. In diesem Teil des Tutorials verwenden Sie Azure Machine Learning-Dienste (Vorschauversion), um Folgendes zu erfahren:

> [!div class="checklist"]
> * Ermitteln der Modelldatei
> * Generieren eines Bewertungsskripts und einer Schemadatei
> * Vorbereiten der Umgebung
> * Erstellen eines Echtzeit-Webdiensts
> * Ausführen des Echtzeit-Webdiensts
> * Untersuchen der Ausgabeblobdaten 

 In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower data set](https://en.wikipedia.org/wiki/iris_flower_data_set)) verwendet. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für Mac OS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
Arbeiten Sie die ersten beiden Teile dieser Tutorial-Reihe durch:

   * Führen Sie das [Tutorial zum Vorbereiten der Daten](tutorial-classifying-iris-part-1.md) aus, um Machine Learning-Ressourcen zu erstellen und die Azure Machine Learning Workbench-Anwendung zu installieren.

   * Führen Sie das [Tutorial zum Erstellen eines Modells](tutorial-classifying-iris-part-2.md) aus, um in Azure Machine Learning ein Modell für die logistische Regression zu erstellen.

Hierzu benötigen Sie ein lokal installiertes und ausgeführtes Docker-Modul. Alternativ können Sie als Bereitstellungsziel einen Azure Container Service-Cluster in Azure verwenden.

## <a name="download-the-model-pickle-file"></a>Herunterladen der pickle-Modelldatei
Im vorherigen Teil des Tutorials wurde das Skript **iris_sklearn.py** lokal in Machine Learning Workbench ausgeführt. Mit dieser Aktion wurde das Modell für die logistische Regression über das beliebte Python-Paket [pickle](https://docs.python.org/2/library/pickle.html) für die Objektserialisierung serialisiert. 

1. Starten Sie die Anwendung Machine Learning Workbench, und öffnen Sie anschließend das Projekt **myIris**, das Sie im vorherigen Teil der Tutorialreihe erstellt haben.

2. Wählen Sie nach dem Öffnen des Projekts im linken Bereich die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste in Ihrem Projektordner zu öffnen.

3. Wählen Sie die Datei **iris_sklearn.py** aus. Der Python-Code wird in Workbench in einer neuen Registerkarte des Text-Editors geöffnet.

4. Sehen Sie in der Datei **iris_sklearn.py** nach, um zu ermitteln, wo die pickle-Datei generiert wurde. Drücken Sie STRG+F, um das Dialogfeld **Suchen** zu öffnen, und suchen Sie im Python-Code nach dem Wort **pickle**.

   In diesem Codeausschnitt ist dargestellt, wie die pickle-Ausgabedatei generiert wurde. Die pickle-Ausgabedatei hat auf dem Datenträger den Namen **model.pkl**. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Suchen Sie in den Ausgabedateien eines vorherigen Ausführungslaufs nach der pickle-Modelldatei.
   
   Beim Ausführen des Skripts **iris_sklearn.py** wurde die Modelldatei unter dem Namen **model.pkl** in den Ordner **outputs** geschrieben. Dieser Ordner befindet sich in der Ausführungsumgebung, die Sie für die Ausführung des Skripts auswählen, und nicht in Ihrem lokalen Projektordner. 
   
   - Wählen Sie zum Suchen nach der Datei im linken Bereich die Schaltfläche **Ausführungen** (Uhrsymbol), um die Liste **All Runs** (Alle Ausführungen) zu öffnen.  
   - Die Registerkarte **All Runs** (Alle Ausführungen) wird geöffnet. Wählen Sie in der Tabelle mit den Ausführungen eine der letzten Ausführungen aus, für die das Ziel **local** und der Skriptname **iris_sklearn.py** lauten. 
   - Der Bereich **Run Properties** (Ausführungseigenschaften) wird geöffnet. Oben rechts im Bereich befindet sich der Abschnitt **Ausgaben**. 
   - Aktivieren Sie zum Herunterladen der pickle-Datei das Kontrollkästchen neben der Datei **model.pkl**, und wählen Sie anschließend die Schaltfläche **Herunterladen**. Speichern Sie sie im Stammverzeichnis Ihres Projektordners. Die Datei wird in den nächsten Schritten benötigt.

   ![Herunterladen der pickle-Datei](media/tutorial-classifying-iris/download_model.png)

   Weitere Informationen zum Ordner `outputs` finden Sie im Artikel [How to read and write large data files](how-to-read-write-files.md) (Lesen und Schreiben von großen Datendateien).

## <a name="get-the-scoring-script-and-schema-files"></a>Abrufen des Bewertungsskripts und von Schemadateien
Zum Bereitstellen des Webdiensts zusammen mit der Modelldatei benötigen Sie auch ein Bewertungsskript und optional ein Schema für die Webdienst-Eingabedaten. Mit dem Bewertungsskript wird die Datei **model.pkl** aus dem aktuellen Ordner geladen und zum Erzeugen einer neu vorhergesagten Iris-Klasse verwendet.  

1. Starten Sie die Anwendung Azure Machine Learning Workbench, und öffnen Sie anschließend das Projekt **myIris**, das Sie im vorherigen Teil der Tutorialreihe erstellt haben.

2. Wählen Sie nach dem Öffnen des Projekts im linken Bereich die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste in Ihrem Projektordner zu öffnen.

3. Wählen Sie die Datei **score_iris.py** aus. Das Python-Skript wird geöffnet. Diese Datei wird als Bewertungsdatei verwendet.

   ![Bewertungsdatei](media/tutorial-classifying-iris/model_data_collection.png)

4. Führen Sie das Skript aus, um die Schemadatei abzurufen. Wählen Sie in der Befehlsleiste die Umgebung **local** und das Skript **score_iris.py** und anschließend die Schaltfläche **Ausführen**. 

5. Mit diesem Skript wird im Abschnitt **outputs** eine JSON-Datei erstellt, mit der das für das Modell benötigte Eingabedatenschema erfasst wird.

6. Beachten Sie den Bereich **Aufträge** rechts im Bereich **Projektdashboard**. Warten Sie, bis für den aktuellen Auftrag **score_iris.py** in Grün der Status **Abgeschlossen** angezeigt wird. Wählen Sie anschließend den Hyperlink **score_iris.py [1]** für die letzte Auftragsausführung aus, um die Ausführungsdetails für die Ausführung **score_iris.py** anzuzeigen. 

7. Wählen Sie im Bereich **Run Properties** (Ausführungseigenschaften) im Abschnitt **Ausgaben** die neu erstellte Datei **service_schema.json** aus.  Aktivieren Sie das Kontrollkästchen neben dem Dateinamen, und wählen Sie anschließend **Herunterladen**. Speichern Sie die Datei im Stammverzeichnis Ihres Projekts.

8. Wechseln Sie zurück zur vorherigen Registerkarte, auf der Sie das Skript **score_iris.py** geöffnet haben. Mit der Datensammlung können Sie Modelleingaben und Vorhersagen für den Webdienst erfassen. Die folgenden Schritte sind für die Datensammlung von besonderem Interesse.

9. Sehen Sie sich den Code oben in der Dateiimportklasse **ModelDataCollector** an, da darin die Funktionalität für die Modelldatensammlung enthalten ist:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Die folgenden Codezeilen in der Funktion **init()** dienen zum Instanziieren von **ModelDataCollector**:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Die folgenden Codezeilen in der Funktion **run(input_df)** dienen zum Sammeln der Eingabe- und Vorhersagedaten:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

Als Nächstes können Sie Ihre Umgebung für die Operationalisierung des Modells vorbereiten.



## <a name="prepare-to-operationalize-locally"></a>Vorbereiten der lokalen Operationalisierung
Verwenden Sie für Docker-Container auf Ihrem lokalen Computer die Bereitstellung vom Typ _Lokaler Modus_.

Sie können _Lokaler Modus_ für Entwicklungs- und Testzwecke nutzen. Das Docker-Modul muss lokal ausgeführt werden, um die folgenden Schritte zum Operationalisieren des Modells auszuführen. Sie können das Flag `-h` am Ende von Befehlen verwenden, um Hilfe zu den Befehlen zu erhalten.

>[!NOTE]
>Falls Sie nicht über ein lokales Docker-Modul verfügen, können Sie trotzdem fortfahren, indem Sie in Azure einen Cluster für die Bereitstellung erstellen. Stellen Sie lediglich sicher, dass Sie den Cluster nach Abschluss des Tutorials löschen, damit keine laufenden Gebühren anfallen.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI).
   Wählen Sie in der Anwendung Azure Machine Learning Workbench im Menü **Datei** die Option **Eingabeaufforderung öffnen**.

   Die Eingabeaufforderung wird geöffnet und zeigt den aktuellen Speicherort Ihres Projektordners an: **c:\temp\myIris>**.

2. Stellen Sie sicher, dass der Azure-Ressourcenanbieter **Microsoft.ContainerRegistry** unter Ihrem Abonnement registriert ist. Sie müssen diesen Ressourcenanbieter registrieren, bevor Sie in Schritt 3 eine Umgebung erstellen können. Sie können überprüfen, ob die Registrierung bereits durchgeführt wurde, indem Sie den folgenden Befehl verwenden:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Es sollte in etwa folgende Ausgabe angezeigt werden: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   Wenn **Microsoft.ContainerRegistry** nicht registriert ist, können Sie dies mit dem folgenden Befehl durchführen:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   Die Registrierung kann einige Minuten dauern. Sie können den Status überprüfen, indem Sie den obigen Befehl **az provider list** oder den folgenden Befehl verwenden:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   In der dritten Zeile der Ausgabe wird **"registrationState": "Registering"** angezeigt. Warten Sie kurz ab, und wiederholen Sie dann den Befehl **show**, bis in der Ausgabe **"registrationState": "Registered"** angezeigt wird.

3. Erstellen Sie die Umgebung. Sie müssen diesen Schritt einmal pro Umgebung ausführen. Führen Sie ihn beispielsweise einmal für die Entwicklungsumgebung und einmal für die Produktionsumgebung aus. Verwenden Sie den _lokalen Modus_ für diese erste Umgebung. Sie können den Switch `-c` oder `--cluster` im folgenden Befehl später ausprobieren, um eine Umgebung im _Clustermodus_ einzurichten.

Beachten Sie, dass für den folgenden Befehl zum Einrichten der Zugriffstyp „Mitwirkender“ für das Abonnement erforderlich ist. Wenn Sie nicht über die erforderlichen Berechtigungen verfügen, benötigen Sie zumindest Zugriff als Mitwirkender auf die Ressourcengruppe, in der die Bereitstellung erfolgt. Hierzu müssen Sie den Ressourcengruppennamen mithilfe des Flags `-g` als Teil des Einrichtungsbefehls angeben. 

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Befolgen Sie die Anweisungen auf dem Bildschirm, um ein Speicherkonto zum Speichern von Docker-Images, eine Azure Container Registry zum Auflisten von Docker-Images und ein AppInsight-Konto zum Erfassen von Telemetriedaten bereitzustellen. Wenn Sie den Switch `-c` verwendet haben, wird auch ein Azure Container Service-Cluster erstellt.
   
   Der Clustername ist eine Möglichkeit, wie Sie die Umgebung identifizieren können. Der Standort sollte dem Standort des Kontos für die Modellverwaltung entsprechen, das Sie über das Azure-Portal erstellt haben.

4. Erstellen Sie ein Modellverwaltungskonto. (Dies ist eine einmalige Aufgabe.)  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Legen Sie das Modellverwaltungskonto fest.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Richten Sie die Umgebung ein.

   Verwenden Sie nach Abschluss der Einrichtung den folgenden Befehl, um die Umgebungsvariablen festzulegen, die zum Operationalisieren der Umgebung erforderlich sind. Verwenden Sie den gleichen Umgebungsnamen, den Sie zuvor in Schritt 4 verwendet haben. Verwenden Sie den gleichen Ressourcengruppennamen, der im Befehlsfenster ausgegeben wurde, nachdem der Setupprozess abgeschlossen war.

   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Geben Sie den folgenden Befehl ein, um zu überprüfen, ob Sie Ihre operationalisierte Umgebung für die lokale Webdienstbereitstellung richtig konfiguriert haben:

   ```azurecli
   az ml env show
   ```

Sie können nun den Echtzeit-Webdienst erstellen.

>[!NOTE]
>Für nachfolgende Webdienstbereitstellungen können Sie das Konto und die Umgebung für die Modellverwaltung wiederverwenden. Sie müssen sie nicht für jeden Webdienst erstellen. Einem Konto oder einer Umgebung können mehrere Webdienste zugeordnet sein.

## <a name="create-a-real-time-web-service-in-one-command"></a>Erstellen eines Echtzeit-Webdiensts mit einem Befehl
1. Verwenden Sie den folgenden Befehl, um einen Echtzeit-Webdienst zu erstellen:

   ```azurecli
   az ml service create realtime -f score_iris.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Bei diesem Befehl wird eine Webdienst-ID zur späteren Verwendung generiert.

   Die folgenden Switches werden für den Befehl **az ml service create realtime** verwendet:
   * `-n`: Der App-Name, der nur Kleinbuchstaben enthalten darf.
   * `-f`: Der Dateiname des Bewertungsskripts.
   * `--model-file`: Die Modelldatei. In diesem Fall ist dies die pickle-Datei „model.pkl“.
   * `-r`: Der Typ des Modells. In diesem Fall ist es ein Python-Modell.
   * `--collect-model-data true`: Dient zum Aktivieren der Datensammlung.

   >[!IMPORTANT]
   >Der Dienstname, bei dem es sich auch um den neuen Docker-Imagenamen handelt, darf nur Kleinbuchstaben enthalten. Andernfalls erhalten Sie eine Fehlermeldung. 

2. Wenn Sie den Befehl ausführen, werden das Modell und die Bewertungsdatei in das Speicherkonto hochgeladen, das Sie bei der Umgebungseinrichtung erstellt haben. Während des Bereitstellungsprozesses wird ein Docker-Image mit Ihrem Modell, dem Schema und der Bewertungsdatei erstellt und per Pushvorgang in die Azure Container Registry übertragen: **\<ACR-Name\>.azureacr.io/\<Imagename\>:\<Version\>**. 

   Der Befehl überträgt dieses Image lokal auf Ihren Computer und startet basierend auf diesem Image dann einen Docker-Container. Wenn Ihre Umgebung im Clustermodus konfiguriert ist, wird der Docker-Container stattdessen im Azure Cloud Services-Kubernetes-Cluster bereitgestellt.

   Im Rahmen der Bereitstellung wird auf Ihrem lokalen Computer ein HTTP-REST-Endpunkt für den Webdienst erstellt. Nach wenigen Minuten sollte der Befehl mit einer Erfolgsmeldung abgeschlossen und Ihr Webdienst betriebsbereit sein.

3. Verwenden Sie den Befehl **docker ps**, um den ausgeführten Docker-Container anzuzeigen:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Erstellen eines Echtzeit-Webdiensts mit separaten Befehlen
Alternativ zum obigen Befehl **az ml service create realtime** können Sie die Schritte auch separat ausführen. 

Registrieren Sie zuerst das Modell. Generieren Sie anschließend das Manifest, erstellen Sie das Docker-Image, und erstellen Sie den Webdienst. Bei diesem Schritt-für-Schritt-Ansatz können Sie bei jedem Schritt flexibler vorgehen. Außerdem können Sie die Entitäten wiederverwenden, die im vorherigen Schritt generiert wurden, und müssen die Entitäten nur bei Bedarf neu erstellen.

1. Registrieren Sie das Modell, indem Sie den Namen der pickle-Datei angeben.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Mit diesem Befehl wird eine Modell-ID generiert.

2. Erstellen Sie ein Manifest.

   Verwenden Sie zum Erstellen eines Manifests den folgenden Befehl, und geben Sie die Modell-ID aus dem vorherigen Schritt an:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
   ```
   Mit diesem Befehl wird eine Manifest-ID generiert.

3. Erstellen Sie ein Docker-Image.

   Verwenden Sie zum Erstellen eines Docker-Image den folgenden Befehl, und geben Sie den Manifest-ID-Wert aus dem vorherigen Schritt an:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   Mit diesem Befehl wird eine Docker-Image-ID generiert.
   
4. Erstellen Sie den Dienst.

   Verwenden Sie zum Erstellen eines Diensts den folgenden Befehl, und geben Sie die Image-ID aus dem vorherigen Schritt an:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Mit diesem Befehl wird eine Webdienst-ID generiert.

Sie können den Webdienst jetzt ausführen.

## <a name="run-the-real-time-web-service"></a>Ausführen des Echtzeit-Webdiensts

Verwenden Sie einen JSON-codierten Datensatz, der ein Array mit vier Zufallszahlen enthält, um den ausgeführten Webdienst **irisapp** zu testen:

1. Der Webdienst enthält Beispieldaten. Bei Ausführung im lokalen Modus können Sie den Befehl **az ml service show realtime** aufrufen. Mit diesem Aufruf wird ein Beispielbefehl für die Ausführung aufgerufen, der zum Testen des Diensts hilfreich ist. Außerdem wird mit diesem Aufruf die Bewertungs-URL abgerufen, mit der Sie den Dienst in Ihre eigene benutzerdefinierte App einbinden können:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Führen Sie den zurückgegebenen Befehl zum Ausführen des Diensts aus, um den Dienst zu testen:

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Die Ausgabe lautet **2**. Dies ist die vorhergesagte Klasse. (Sie erhalten unter Umständen ein anderes Ergebnis.) 

3. Zum Ausführen des Diensts außerhalb der CLI müssen Sie die Schlüssel für die Authentifizierung beschaffen:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Anzeigen der gesammelten Daten im Azure-Blobspeicher

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Suchen Sie nach Ihren Speicherkonten. Wählen Sie hierzu **Weitere Dienste**.

3. Geben Sie im Suchfeld **Speicherkonten** ein, und drücken Sie anschließend die **Eingabetaste**.

4. Wählen Sie im Suchfeld **Speicherkonten** die Ressource **Speicherkonto** für Ihre Umgebung aus. 

   > [!TIP]
   > Ermitteln Sie wie folgt, welches Speicherkonto verwendet wird:
   > 1. Öffnen Sie Azure Machine Learning Workbench.
   > 2. Wählen Sie das Projekt aus, an dem Sie arbeiten.
   > 3. Öffnen Sie im Menü **Datei** eine Befehlszeile.
   > 4. Geben Sie an der Eingabeaufforderung `az ml env show -v` ein, und überprüfen Sie den Wert von *storage_account*. Dies ist der Name Ihres Speicherkontos.

5. Wählen Sie nach dem Öffnen des Bereichs **Speicherkonto** in der Liste auf der linken Seite die Option **Container**. Suchen Sie nach dem Container mit dem Namen **modeldata**. 
 
   Falls keine Daten angezeigt werden, müssen Sie nach der ersten Webdienstanforderung unter Umständen bis zu zehn Minuten warten, bis die Daten in das Speicherkonto übertragen wurden und angezeigt werden.

   Die Daten fließen in Blobs mit dem folgenden Containerpfad:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Sie können diese Daten über den Azure-Blobspeicher nutzen. Hierfür gibt es viele verschiedene Tools (Microsoft-Software und Open-Source-Tools). Beispiele:

   - Azure Machine Learning: Öffnen Sie die CSV-Datei, indem Sie die CSV-Datei als Datenquelle hinzufügen. 
   - Excel: Öffnen Sie die täglichen CSV-Dateien als Tabelle.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Erstellen Sie Diagramme, für die Daten aus den CSV-Daten in Blobs genutzt werden.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): Laden Sie die CSV-Daten in eine Hive-Tabelle, und führen Sie SQL-Abfragen direkt für das Blob durch.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): Erstellen Sie einen Dataframe mit einem hohen Anteil von CSV-Daten.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Nächste Schritte
In diesem dritten Teil der dreiteiligen Tutorial-Reihe wurde beschrieben, wie Sie Azure Machine Learning-Dienste für folgende Zwecke verwenden:
> [!div class="checklist"]
> * Ermitteln der Modelldatei
> * Generieren eines Bewertungsskripts und einer Schemadatei
> * Vorbereiten der Umgebung
> * Erstellen eines Echtzeit-Webdiensts
> * Ausführen des Echtzeit-Webdiensts
> * Untersuchen der Ausgabeblobdaten 

Sie haben erfolgreich ein Trainingsskript in unterschiedlichen Computeumgebungen ausgeführt, ein Modell erstellt, das Modell serialisiert und es anschließend über einen Docker-basierten Webdienst operationalisiert. 

Nun können Sie die erweiterte Datenvorbereitung durchführen:
> [!div class="nextstepaction"]
> [Erweiterte Datenvorbereitung](tutorial-bikeshare-dataprep.md)

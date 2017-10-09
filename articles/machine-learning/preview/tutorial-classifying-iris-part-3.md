---
title: "Bereitstellen eines Modells für Azure Machine Learning-Dienste (Vorschauversion) | Microsoft-Dokumentation"
description: "Dieses Tutorial in voller Länge zeigt, wie End-to-End-Azure Machine Learning-Dienste (Vorschauversion) verwendet werden können. Dies ist Teil 3 zum Thema „Bereitstellungsmodell“."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/25/2017
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 0dfcc965d96949527b3e80285061bff320872621
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-3-deploy-a-model"></a>Klassifizieren von Schwertlilien, Teil 3: Bereitstellen eines Modells
Azure Machine Learning-Dienste (Vorschauversion) ist eine integrierte End-to-End Data Science- und erweiterte Analyselösung für professionelle Data Scientists zum Vorbereiten von Daten, Entwickeln von Experimenten und Bereitstellen von Modellen auf Cloudniveau.

Dieses Tutorial ist der dritte Teil einer dreiteiligen Reihe. In diesem Teil des Tutorials verwenden Sie Azure Machine Learning-Dienste (Vorschauversion), um Folgendes zu lernen:

> [!div class="checklist"]
> * Ermitteln der Modelldatei
> * Generieren eines Bewertungsskripts und einer Schemadatei
> * Vorbereiten der Umgebung
> * Erstellen eines Echtzeit-Webdiensts
> * Ausführen des Echtzeit-Webdiensts
> * Untersuchen der Ausgabeblobdaten 

 In diesem Tutorial wird das zeitlose Schwertlilien-Dataset ([Iris flower dataset](https://en.wikipedia.org/wiki/iris_flower_data_set)) verwendet, um die Dinge einfach zu halten. Die Screenshots sind Windows-spezifisch, aber die Benutzeroberfläche für macOS ist nahezu identisch.

## <a name="prerequisites"></a>Voraussetzungen
Arbeiten Sie die ersten beiden Teile dieser Tutorial-Reihe durch:
- Führen Sie zuerst das [Tutorial zum Vorbereiten der Daten](tutorial-classifying-iris-part-1.md) aus, um Azure Machine Learning-Ressourcen zu erstellen und die Azure Machine Learning Workbench-Anwendung zu installieren.
- Führen Sie anschließend das [Tutorial zum Erstellen eines Modells](tutorial-classifying-iris-part-2.md) aus, um in Azure Machine Learning ein Modell für die logistische Regression zu erstellen.

## <a name="download-the-model-pickle-file"></a>Herunterladen der pickle-Modelldatei
Im vorherigen Teil des Tutorials wurde das Skript `iris_sklearn.py` lokal in Azure Machine Learning Workbench ausgeführt. Mit dieser Aktion wurde das Modell für die logistische Regression über das beliebte Python-Paket **[pickle](https://docs.python.org/2/library/pickle.html)** für die Objektserialisierung serialisiert. 

1. Starten Sie die Anwendung **Azure Machine Learning Workbench**, und öffnen Sie das Projekt **myIris**, das Sie im vorherigen Teil der Tutorial-Reihe erstellt haben.

2. Klicken Sie nach dem Öffnen des Projekts in Azure Machine Learning Workbench in der linken Symbolleiste auf die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

3. Wenn Sie die Datei **iris_sklearn.py** auswählen, wird der Python-Code in Workbench in einer neuen Registerkarte des Text-Editors geöffnet.

4. Sehen Sie in der Datei **iris_sklearn.py** nach, um zu ermitteln, wo die pickle-Datei generiert wurde. Drücken Sie STRG+F, um das Suchdialogfeld zu öffnen, und suchen Sie dann im Python-Code nach dem Wort **pickle**.

   In diesem Codeausschnitt ist dargestellt, wie die pickle-Ausgabedatei generiert wurde. Beachten Sie, dass die pickle-Ausgabedatei auf dem Datenträger den Namen `model.pkl` hat. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Suchen Sie in den Ausgabedateien eines vorherigen Ausführungslaufs nach der pickle-Modelldatei.
   
   Beim Ausführen des Skripts **iris_sklearn.py** wurde die Modelldatei unter dem Namen `model.pkl` in den Ordner `outputs` geschrieben. Dieser Ordner befindet sich in der Ausführungsumgebung, die Sie für die Ausführung des Skripts auswählen, und nicht in Ihrem lokalen Projektordner. 
   
   - Verwenden Sie zum Ermitteln der Datei die Azure Machine Learning Workbench-Anwendung, und klicken Sie in der Symbolleiste auf der linken Seite auf die Schaltfläche **Ausführungen** (Uhrsymbol), um die Liste unter **All Runs** (Alle Ausführungen) zu öffnen.  
   - Die Registerkarte **All Runs** (Alle Ausführungen) wird geöffnet. Wählen Sie in der Tabelle mit den Ausführungen eine der letzten Ausführungen aus, für die das Ziel **local** und der Skriptname **iris_sklearn.py** lauten. 
   - Die Seite **Run Properties** (Ausführungseigenschaften) wird geöffnet. Oben rechts auf der Seite befindet sich der Abschnitt **Ausgaben**. 
   - Laden Sie die pickle-Datei herunter, indem Sie das Kontrollkästchen neben der Datei **model.pkl** aktivieren und auf die Schaltfläche **Herunterladen** klicken. Speichern Sie sie im Stammverzeichnis Ihres Projektordners. Sie benötigen die Datei für die weiteren Schritte.

   ![Herunterladen der pickle-Datei](media/tutorial-classifying-iris/download_model.png)

   Weitere Informationen zum Ordner `outputs` finden Sie im Artikel [How to read and write large data files](how-to-read-write-files.md) (Lesen und Schreiben von großen Datendateien).

## <a name="get-scoring-and-schema-files"></a>Abrufen von Bewertungs- und Schemadateien
Zum Bereitstellen des Webdiensts zusammen mit der Modelldatei benötigen Sie auch ein Bewertungsskript und optional ein Schema für die Webdienst-Eingabedaten. Mit dem Bewertungsskript wird die Datei `model.pkl` aus dem aktuellen Ordner geladen und zum Erzeugen einer neu vorhergesagten Iris-Klasse verwendet.  

1. Starten Sie die Anwendung **Azure Machine Learning Workbench**, und öffnen Sie das Projekt **myIris**, das Sie im vorherigen Teil der Tutorial-Reihe erstellt haben.

2. Klicken Sie nach dem Öffnen des Projekts in Azure Machine Learning Workbench in der linken Symbolleiste auf die Schaltfläche **Dateien** (Ordnersymbol), um die Dateiliste im Projektordner zu öffnen.

3. Wählen Sie die Datei **iris_score.py** aus. Das Python-Skript wird geöffnet. Diese Datei wird als Bewertungsdatei verwendet.

4. Führen Sie das Skript aus, um die Schemadatei abzurufen. Wählen Sie in der Befehlsleiste die Umgebung **local** und das Skript **iris_score.py** aus, und klicken Sie anschließend auf die Schaltfläche **Ausführen**. 

5. Mit diesem Skript wird im Ordner **outputs** eine JSON-Datei erstellt, mit der das für das Modell benötigte Eingabedatenschema erfasst wird.

   ![Bewertungsdatei](media/tutorial-classifying-iris/model_data_collection.png)

6. Warten Sie rechts im Machine Learning Workbench-Fenster im Bereich „Aufträge“, bis für den aktuellen **iris_score.py**-Auftrag der Status **Abgeschlossen** (grün) angezeigt wird. Klicken Sie anschließend auf den Hyperlink **iris_score.py [1]** für die aktuelle Auftragsausführung, um die Ausführungsdetails für die **iris_score.py**-Ausführung anzuzeigen. 

7. Wählen Sie auf der Seite „Run Properties“ (Ausführungseigenschaften) im Abschnitt **Ausgaben** die neu erstellte Datei **service_schema.json** aus. Speichern Sie die Datei im Stammverzeichnis Ihres Projekts.

8. Wechseln Sie zurück zu der Registerkarte, auf der Sie das Skript **iris_score.py** geöffnet haben. 

   Beachten Sie die Nutzung der Datensammlung, mit der Sie Modelleingaben und Vorhersagen für den Webdienst erfassen können. Die folgenden Punkte sind für die Datensammlung von besonderem Interesse.

9. Sehen Sie sich den Code oben in der Dateiimportklasse ModelDataCollector an, die die Funktionalität für die Modelldatensammlung enthält:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Die folgenden Codezeilen in der Funktion `init()` dienen zum Instanziieren von ModelDataCollector:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Die folgenden Codezeilen in der Funktion `run(input_df)` dienen zum Sammeln von Eingabe- und Vorhersagedaten:

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

1. Öffnen Sie die Befehlszeilenschnittstelle in Azure Machine Learning Workbench, und klicken Sie im Menü „Datei“ auf **Eingabeaufforderung öffnen**.

   Die Eingabeaufforderung wird geöffnet und zeigt den aktuellen Speicherort Ihres Projektordners an: `c:\temp\myIris>`.

2. Stellen Sie sicher, dass der Azure-Ressourcenanbieter `Microsoft.ContainerRegistry` in Ihrem Abonnement registriert ist. Sie müssen diesen Ressourcenanbieter registrieren, bevor Sie in Schritt 3 eine Umgebung erstellen können. Sie können überprüfen, ob die Registrierung bereits durchgeführt wurde, indem Sie den folgenden Befehl verwenden:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   Falls `Microsoft.ContainerRegistry` nicht registriert ist, können Sie dafür den folgenden Befehl verwenden:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   Die Registrierung kann einige Minuten dauern. Sie können den Status mit dem obigen Befehl `az provider list` oder mit dem folgenden Befehl prüfen:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

3. Erstellen Sie die Umgebung. Dieser Schritt muss einmal pro Umgebung ausgeführt werden, also beispielsweise jeweils für die Entwicklung und Produktion. Verwenden Sie den _lokalen Modus_ für diese erste Umgebung. (Sie können den Switch `-c` oder `--cluster` im folgenden Befehl später ausprobieren, um eine Umgebung im _Clustermodus_ einzurichten.)

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Befolgen Sie die Anweisungen auf dem Bildschirm, um ein Speicherkonto zum Speichern von Docker-Images, eine ACR-Instanz (Azure Container Registry) zum Auflisten von Docker-Images und ein AppInsight-Konto zum Erfassen von Telemetriedaten bereitzustellen. Wenn Sie den Switch `-c` verwendet haben, wird auch ein ACS-Cluster (Azure Container Service) erstellt.
   
   Der Clustername ist für Sie eine Möglichkeit zum Identifizieren der Umgebung, und der Standort sollte dem Standort des Kontos für die Modellverwaltung entsprechen, das Sie über das Azure-Portal erstellt haben.

4. Erstellen eines Kontos für die Modellverwaltung (einmalige Einrichtung)  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Festlegen des Kontos für die Modellverwaltung  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Richten Sie die Umgebung ein.
Legen Sie nach Abschluss der Einrichtung die Umgebungsvariablen, die für die Operationalisierung erforderlich sind, mit dem folgenden Befehl fest. Der Umgebungsname ist der Name, den Sie oben in Schritt 1 verwendet haben. Der Name der Ressourcengruppe war Teil der Ausgabe desselben Prozesses und wird im Befehlsfenster angezeigt, wenn der Einrichtungsprozess abgeschlossen ist.
   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

   Geben Sie den folgenden Befehl ein, um zu überprüfen, ob Sie Ihre Operationalisierungsumgebung für die lokale Webdienstbereitstellung richtig konfiguriert haben:

   ```azurecli
   az ml env show
   ```

Sie können nun den Echtzeit-Webdienst erstellen.

## <a name="create-a-real-time-web-service"></a>Erstellen eines Echtzeit-Webdiensts
Verwenden Sie den folgenden Befehl, um einen Echtzeit-Webdienst zu erstellen:

   ```azurecli
   az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Hierbei wird eine Webdienst-ID zur späteren Verwendung generiert.

   Hier sind die Switches für den Befehl `az ml service create realtime` angegeben:
   * -n: App-Name (nur Kleinbuchstaben)
   * -f: Dateiname des Bewertungsskripts
   * --model-file: Modelldatei (hier die pickle-Datei „model.pkl“)
   * -r: Typ des Modells (hier ein Python-Modell)
   * --collect-model-data true: Aktivierung der Datensammlung

   >[!IMPORTANT]
   >Der Dienstname (gleichzeitig der neue Docker-Imagename) darf nur Kleinbuchstaben enthalten. Andernfalls erhalten Sie einen Fehler. 

   Wenn Sie den Befehl ausführen, werden das Modell und die Bewertungsdatei in das Speicherkonto hochgeladen, das Sie oben im Rahmen der Umgebungseinrichtung erstellt haben. Während des Bereitstellungsprozesses wird ein Docker-Image mit Ihrem Modell, dem Schema und der Bewertungsdatei erstellt und per Pushvorgang in die Azure Container Registry (ACR) übertragen: `<ACR_name>.azureacr.io/<imagename>:<version>`. Anschließend wird dieses Image lokal auf Ihren Computer übertragen und basierend auf diesem Image ein Docker-Container gestartet. (Wenn Ihre Umgebung im Clustermodus konfiguriert ist, wird der Docker-Container stattdessen im ACS Kubernetes-Cluster bereitgestellt.)

   Im Rahmen der Bereitstellung wird auf Ihrem lokalen Computer ein HTTP-REST-Endpunkt für den Webdienst erstellt. Nach wenigen Minuten sollte der Befehl mit einer Erfolgsmeldung abgeschlossen und Ihr Webdienst betriebsbereit sein.

   Sie können den ausgeführten Docker-Container anzeigen, indem Sie den Befehl `docker ps` verwenden:
   ```azurecli
   docker ps
   ```
### <a name="alternative-route"></a>Alternative Route
Alternativ zum obigen Befehl `az ml service create realtime` können Sie die Schritte zum Durchführen der Modellregistrierung, Generieren des Manifests, Erstellen des Docker-Image und Erstellen des Webdiensts auch einzeln ausführen. Sie können bei jedem Schritt dann flexibler vorgehen und die im vorherigen Schritte generierten Entitäten wiederverwenden und nur bei Bedarf Entitäten neu erstellen. Hier ist angegeben, wie Sie dies erreichen:

1. Registrieren Sie das Modell, indem Sie den Namen der pickle-Datei angeben.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Eine Modell-ID wird generiert.

2. Erstellen des Manifests

   Verwenden Sie zum Erstellen eines Manifests diesen Befehl, und geben Sie die Modell-ID aus dem vorherigen Schritt an:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f iris_score.py -r python -i <model ID> -s service_schema.json
   ```
   Es wird eine Manifest-ID generiert.

3. Erstellen eines Docker-Image

   Verwenden Sie zum Erstellen eines Docker-Image diesen Befehl, und geben Sie den Manifest-ID-Wert aus dem vorherigen Schritt an:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   Es wird eine Docker-Image-ID generiert.
   
4. Erstellen des Diensts

   Verwenden Sie zum Erstellen eines Diensts den aufgeführten Befehl, und geben Sie die Image-ID aus dem vorherigen Schritt an:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Es wird eine Webdienst-ID generiert.

Sie können den Webdienst jetzt ausführen.

## <a name="run-the-real-time-web-service"></a>Ausführen des Echtzeit-Webdiensts

Testen Sie den ausgeführten `irisapp`-Webdienst, indem Sie darauf einen JSON-codierten Datensatz anwenden, der ein Array mit vier Zufallszahlen enthält.

1. Bei der Erstellung des Webdiensts wurden Beispieldaten verwendet. Bei der Ausführung im lokalen Modus können Sie den Befehl `az ml service show realtime` aufrufen, um einen Beispielbefehl für die Ausführung abzurufen, den Sie zum Testen des Diensts verwenden können. Hierbei erhalten Sie auch die Bewertungs-URL, mit der Sie den Dienst in Ihre eigene benutzerdefinierte App einbinden können:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Führen Sie den zurückgegebenen Befehl zum Ausführen des Diensts aus, um den Dienst zu testen.

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}
   ```
   Die Ausgabe lautet `"2"`. Dies ist die vorhergesagte Klasse. (Sie erhalten unter Umständen ein anderes Ergebnis.) 

3. Wenn Sie den Dienst außerhalb der CLI ausführen möchten, müssen Sie die Schlüssel für die Authentifizierung beschaffen:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Zeigen Sie die gesammelten Daten im Azure-Blobspeicher an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach Ihren Speicherkonten. Klicken Sie hierzu auf **Weitere Dienste**.

3. Geben Sie im Suchfeld den Suchbegriff **Speicherkonten** ein, und drücken Sie die **EINGABETASTE**.

4. Wählen Sie auf der Suchseite **Speicherkonten** die **Speicherkonto**-Ressource für Ihre Umgebung. 

   > [!TIP]
   > Ermitteln Sie wie folgt, welches Speicherkonto verwendet wird: Öffnen Sie Azure Machine Learning Workbench, wählen Sie das aktuelle Projekt aus, und öffnen Sie die Eingabeaufforderung über das Menü **Datei**. Geben Sie in der Eingabeaufforderung `az ml env show -v` ein, und überprüfen Sie den Wert von *storage_account*. Dies ist der Name des Speicherkontos.

5. Klicken Sie nach dem Öffnen der Seite **Speicherkonto** in der linken Liste auf den Eintrag **Container**. Suchen Sie nach dem Container mit dem Namen **modeldata**. 
 
   Falls keine Daten angezeigt werden, müssen Sie nach der ersten Webdienstanforderung unter Umständen bis zu 10 Minuten warten, bis die Daten unter dem Speicherkonto sichtbar werden.

   Die Daten fließen in Blobs mit dem folgenden Containerpfad:

   `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

6. Sie können diese Daten aus Azure-Blobs auf verschiedene Arten nutzen, indem Sie Microsoft-Software oder Open-Source-Tools verwenden. 

   Beispiele für die Nutzung der Ausgabeblobs:
   - Azure ML Workbench: Öffnen Sie die CSV-Datei in Azure ML Workbench, indem Sie sie als Datenquelle hinzufügen. 
   - Excel: Öffnen Sie die täglichen CSV-Dateien als Tabelle.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Erstellen Sie Diagramme, für die Daten aus den CSV-Daten in Blobs genutzt werden.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): Erstellen Sie einen Datenframe mit einem hohen Anteil von CSV-Daten.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): Laden Sie CSV-Daten in eine Hive-Tabelle, und führen Sie SQL-Abfragen direkt für ein Blob durch.

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



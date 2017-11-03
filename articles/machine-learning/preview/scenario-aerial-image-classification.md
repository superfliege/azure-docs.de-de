---
title: Klassifizierung von Luftbildern | Microsoft-Dokumentation
description: "Anweisungen für die Klassifizierung von Luftbildern in der Praxis"
author: mawah
ms.author: mawah
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
services: machine-learning
ms.date: 09/15/2017
ms.openlocfilehash: e6b673527d77550d4213e6d742156ccc525f6b44
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="aerial-image-classification"></a>Klassifizierung von Luftbildern

In diesem Beispiel wird veranschaulicht, wie Sie mit Azure Machine Learning Workbench verteilte Trainings- und Operationalisierungsfunktionen von Modellen zur Bildklassifizierung koordinieren. Das Paket [Microsoft Machine Learning für Apache Spark (MMLSpark)](https://github.com/Azure/mmlspark) wird verwendet, um Bilder mithilfe von vorab trainierten CNTK-Modellen mit Funktionen auszustatten und Klassifizierer mithilfe der abgeleiteten Funktionen zu trainieren. Anschließend werden die trainierten Modelle parallel auf große Bildzusammenstellungen in der Cloud angewendet. Diese Schritte werden in einem [Azure HDInsight Spark](https://azure.microsoft.com/en-us/services/hdinsight/apache-spark/)-Cluster ausgeführt. Dadurch können Sie die Geschwindigkeit der Trainings- und Operationalisierungsfunktionen durch Hinzufügen oder Entfernen von Workerknoten anpassen.

Die hier beschriebene Form des Lerntransfers besitzt wichtige Vorteile gegenüber dem erneuten Training oder der Feinabstimmung eines tiefen neuronalen Netzwerkes: Sie erfordert kein GPU-Computing, ist grundsätzlich schnell, beliebig skalierbar und erfordert weniger Parameter. Diese Methode eignet sich daher besonders, wenn nur wenige Trainingsbeispiele zur Verfügung stehen, wie z.B. bei benutzerdefinierten Anwendungsfällen. Viele Benutzer berichten von äußerst leistungsfähigen Modellen, die Sie mithilfe dieser Art von Lerntransfer erstellt haben. Dadurch werden tiefe neuronale Netzwerke vermieden, die von Grund auf trainiert werden müssen und daher wesentlich teurer sind.

Der in diesem Beispiel verwendete Spark-Cluster verfügt über 40 Workerknoten und kann für eine Gebühr von etwa $40/Stunde verwendet werden. Diese exemplarische Vorgehensweise dauert etwa zwei Stunden. Führen Sie nach Abschluss der Vorgehensweise die Bereinigungsanweisungen aus, um die erstellten Ressourcen zu entfernen und zu verhindern, dass weitere Gebühren anfallen.

## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Das öffentliche GitHub-Repository für dieses Szenario aus der Praxis enthält alles, was Sie für dieses Beispiel benötigen, einschließlich Codebeispielen:

[https://github.com/Azure/MachineLearningSamples-AerialImageClassification](https://github.com/Azure/MachineLearningSamples-AerialImageClassification)

## <a name="use-case-description"></a>Beschreibung der Anwendungsfälle

In diesem Szenario werden Machine Learning-Modelle trainiert, sodass sie Flächentypen anhand von Luftbildern im Maßstab 224 Meter x 224 Meter klassifizieren können. Klassifizierungsmodelle für die Flächennutzung werden verwendet, um die Entwicklung im Bereich Verstädterung, Abholzung, Verkleinerung von Feuchtgebieten und anderen wichtigen ökologischen Trends in regelmäßigen Abständen anhand von Luftbildern nachzuverfolgen. Es stehen Bildzusammenstellungen für Trainings- und Prüfungszwecke zur Verfügung mit Bildern aus dem US-amerikanischen National Agriculture Imagery Program sowie Flächennutzungskennzeichnungen der US-amerikanischen  National Land Cover Database. Die folgenden Bilder zeigen Beispiele der Flächennutzungsklassen:

![Beispielregionen für Flächennutzungskennzeichnungen](media/scenario-aerial-image-classification/example-labels.PNG)

Nachdem das Klassifizierungsmodell trainiert und geprüft wurde, wird es auf Luftbilder der Region Middlesex County in Massachusetts, USA (dem Sitz des New England Research & Development Center von Microsoft) angewendet. Dabei wird veranschaulicht, wie anhand dieser Modelle städtebauliche Entwicklungen untersucht werden können.

Datenanalysten erstellen häufig mehrere Modelle mit unterschiedlichen Methoden, um eine Bildklassifizierung mithilfe von Lerntransfer zu erstellen, und wählen anschließend das leistungsfähigste Modell aus. Mit Azure Machine Learning Workbench können Datenanalysten das Training in verschiedenen Compute-Umgebungen koordinieren, die Leistungen mehrerer Modelle nachverfolgen und vergleichen und schließlich ein ausgewähltes Modell auf große Datasets in der Cloud anwenden.

## <a name="scenario-structure"></a>Szenariostruktur

In diesem Beispiel sind die Bilddaten und vortrainierten Modelle in einem Azure-Speicherkonto abgelegt. Ein Azure HDInsight Spark-Cluster liest diese Dateien und erstellt mithilfe von MMLSpark ein Bildklassifizierungsmodell. Das trainierte Modell und seine Vorhersagen werden anschließend in das Speicherkonto geschrieben. Dort können sie mit einem lokal ausgeführten Jupyter-Notebook analysiert und angezeigt werden. Mit Azure Machine Learning Workbench wird die Remoteausführung von Skripts im Spark-Cluster koordiniert. Außerdem werden Genauigkeitsmetriken für mehrere Modelle nachverfolgt, die mit verschiedenen Methoden trainiert wurden. Damit können Benutzer das leistungsfähigste Modell auswählen.

![Schematische Darstellung der Luftbilderklassifizierung in der Praxis](media/scenario-aerial-image-classification/scenario-schematic.PNG)

In diesen schrittweisen Anleitungen erfahren Sie, wie ein Azure-Speicherkonto und ein Spark-Cluster erstellt und vorbereitet werden, einschließlich Datenübertragung und Installation der Abhängigkeit. Anschließend wird beschrieben, wie Trainingsaufträge gestartet und die Leistung der resultierenden Modelle verglichen werden. Schließlich wird veranschaulicht, wie das ausgewählte Modell auf eine große Bildzusammenstellung im Spark-Cluster angewendet wird und die Vorhersageergebnisse lokal analysiert werden.


## <a name="set-up-the-execution-environment"></a>Einrichten der Ausführungsumgebung

Befolgen Sie die folgenden Anweisungen, um die Ausführungsumgebung für dieses Beispiel einzurichten.

### <a name="prerequisites"></a>Voraussetzungen
- Ein [Azure-Konto](https://azure.microsoft.com/en-us/free/) (kostenlose Testversionen verfügbar).
    - In diesem Beispiel wird ein HDInsight Spark-Cluster mit 40 Workerknoten (insgesamt 168 Kerne) erstellt. Stellen Sie sicher, dass Ihr Konto über genügend verfügbare Kerne verfügt. Gehen Sie dazu im Azure-Portal auf die Registerkarte „Nutzung + Kontingente“ für Ihr Abonnement.
    - Wenn Sie weniger Kerne zur Verfügung haben, können Sie die HDInsight-Clustervorlage ändern und die Anzahl der bereitgestellten Worker reduzieren. Die Anweisungen hierzu finden Sie im Abschnitt „Erstellen des HDInsight Spark-Clusters“.
- [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md):
    - Befolgen Sie die Anleitung im [Schnellstart zum Installieren und Erstellen](quickstart-installation.md), um Azure Machine Learning Workbench zu installieren und Konten für die Modellverwaltung und zum Experimentieren zu erstellen.
- [AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy), ein kostenloses Hilfsprogramm zur Koordinierung der Dateiübertragung zwischen Azure-Speicherkonten:
    - Stellen Sie sicher, dass sich der Ordner mit der ausführbaren AzCopy-Datei in der PATH-Umgebungsvariable Ihres Systems befindet. (Anweisungen zum Ändern von Umgebungsvariablen finden Sie [hier](https://support.microsoft.com/en-us/help/310519/how-to-manage-environment-variables-in-windows-xp).) Dieses Beispiel wurde auf einem Windows 10-PC getestet. Sie sollten es auf jedem Windows-Computer, einschließlich virtuellen Azure Data Science-Computern, ausführen können. Wenn Sie dieses Beispiel unter macOS ausführen, sind möglicherweise kleinere Änderungen erforderlich (z.B. Änderungen an Dateipfaden).

### <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

Für dieses Beispiel benötigen Sie einen HDInsight Spark-Cluster und ein Azure-Speicherkonto, um die relevanten Dateien zu hosten. Zum Erstellen dieser Ressourcen in einer neuen Azure-Ressourcengruppe, gehen Sie folgendermaßen vor:

#### <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie im Bereich **Neues Projekt erstellen** die Informationen für das neue Projekt ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchbegriff „Klassifizierung von Luftbildern“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**
 
#### <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

1. Gehen Sie zu „Datei -> Eingabeaufforderung öffnen“, um in Ihrem Azure Machine Learning Workbench-Projekt eine Befehlszeilenschnittstelle (CLI) zu öffnen.
1. Melden Sie sich in der Befehlszeilenschnittstelle mithilfe des folgenden Befehls bei Ihrem Azure-Konto an:

    ```
    az login
    ```

    Sie werden aufgefordert, eine URL zu besuchen und einen bereitgestellten temporären Code einzugeben. Die Website fordert die Anmeldeinformationen für Ihr Azure-Konto an.
    
1. Nachdem die Anmeldung abgeschlossen ist, kehren Sie zur CLI zurück und führen den folgenden Befehl aus, um zu ermitteln, welche Azure-Abonnements für Ihr Azure-Konto verfügbar sind:

    ```
    az account list
    ```

    Dieser Befehl listet alle Abonnements auf, die Ihrem Azure-Konto zugeordnet sind. Suchen Sie nach der ID des Abonnements, das Sie verwenden möchten. Fügen Sie die Abonnement-ID wie angegeben in den folgenden Befehl ein, und legen Sie dann das aktive Abonnement durch Ausführen des folgenden Befehls fest:

    ```
    az account set --subscription [subscription ID]
    ```

1. Die in diesem Beispiel erstellten Azure-Ressourcen werden zusammen in einer Azure-Ressourcengruppe gespeichert. Wählen Sie einen eindeutigen Ressourcengruppennamen aus, und fügen Sie ihn wie angegeben ein. Führen Sie dann beide Befehle aus, um die Azure-Ressourcengruppe zu erstellen:

    ```
    set AZURE_RESOURCE_GROUP=[resource group name]
    az group create --location eastus --name %AZURE_RESOURCE_GROUP%
    ```

#### <a name="create-the-storage-account"></a>Erstellen des Speicherkontos

Wir erstellen nun das Speicherkonto, das die Projektdateien hostet, auf die HDInsight Spark zugreifen muss.

1. Wählen Sie einen eindeutigen Speicherkontonamen aus, und fügen Sie diesen wie angegeben in den folgenden Befehl `set` ein. Erstellen Sie dann ein Azure-Speicherkonto, indem Sie beide Befehle ausführen:

    ```
    set STORAGE_ACCOUNT_NAME=[storage account name]
    az storage account create --name %STORAGE_ACCOUNT_NAME% --resource-group %AZURE_RESOURCE_GROUP% --sku Standard_LRS
    ```

1. Geben Sie den folgenden Befehl aus, um die Speicherkontoschlüssel aufzulisten:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Notieren Sie sich den Wert von `key1` als Speicherschlüssel im folgenden Befehl, und führen Sie dann den Befehl zum Speichern des Werts aus.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Laden Sie die Datei `settings.cfg` aus dem Unterverzeichnis „Code“ des Azure Machine Learning Workbench-Projekts in Ihrem bevorzugten Text-Editor, und fügen Sie den Speicherkontonamen und Schlüssel wie angegeben ein. Die geänderten Zeilen in der Datei sollte etwa wie folgt aussehen:
    ```
    [Settings]
        # Credentials for the Azure Storage account
        storage_account_name = storacctname
        storage_account_key = kpI...88 chars total...Q==
    ```
    Speichern und schließen Sie die `settings.cfg`-Datei.
1. Wenn nicht bereits geschehen, laden Sie das Hilfsprogramm [AzCopy](http://aka.ms/downloadazcopy) herunter und installieren es. Stellen Sie sicher, dass sich die ausführbare AzCopy-Datei in Ihrem Systempfad befindet, indem Sie „AzCopy“ eingeben und die EINGABETASTE drücken, um die zugehörige Dokumentation anzuzeigen.
1. Geben Sie die folgenden Befehle aus, um alle Beispieldaten, vortrainierten Modelle und Modelltrainingsskripts in die entsprechenden Speicherorte in Ihrem Speicherkonto zu kopieren:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Gehen Sie davon aus, dass die Dateiübertragung bis zu 20 Minuten in Anspruch nimmt. Während Sie warten, können Sie mit dem folgenden Abschnitt fortfahren. Sie müssen möglicherweise eine weitere Befehlszeilenschnittstelle über Workbench öffnen und die temporären Variablen dort neu definieren.

#### <a name="create-the-hdinsight-spark-cluster"></a>Erstellen des HDInsight Spark-Clusters

Die empfohlene Methode zum Erstellen eines HDInsight-Clusters verwendet die Resource Manager-Vorlage des HDInsight Spark-Clusters, die im Unterordner „Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning“ dieses Projekts enthalten ist.

1. Die HDInsight Spark-Clustervorlage ist die Datei „template.json“ im Unterordner „Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning“ dieses Projekts. Die Vorlage erstellt standardmäßig einen Spark-Cluster mit 40 Workerknoten. Wenn Sie diese Anzahl anpassen müssen, öffnen Sie die Vorlage in Ihrem bevorzugten Text-Editor, und ersetzen Sie alle Instanzen von „40“ durch die Workerknotenanzahl Ihrer Wahl.
    - Wenn die Anzahl der ausgewählten Workerknoten klein ist, können ggf. Fehler des Typs „Nicht genügend Arbeitsspeicher“ auftreten. Um Speicherfehlern zu begegnen, können Sie die Trainings- und Operationalisierungsskripts für eine Teilmenge der verfügbaren Daten ausführen. Dies wird weiter unten in diesem Dokument beschrieben.
2. Wählen Sie einen eindeutigen Namen und ein Kennwort für den HDInsight-Cluster aus, und fügen Sie diese Angaben wie angegeben in den folgenden Befehl ein. Erstellen Sie dann den Cluster, indem Sie den folgenden Befehl ausgeben:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Die Bereitstellung des Clusters kann bis zu 30 Minuten (einschließlich Ausführen von Bereitstellung und Skriptaktionen) dauern.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Vorbereiten der Azure Machine Learning Workbench-Ausführungsumgebung

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Registrieren des HDInsight-Clusters als Azure Machine Learning Workbench-Computeziel

Nach dem Abschluss der Erstellung des HDInsight-Clusters registrieren Sie den Cluster wie folgt als Computeziel für Ihr Projekt:

1.  Geben Sie den folgenden Befehl in der Azure Machine Learning-Befehlszeilenschnittstelle aus:

    ```
    az ml computetarget attach --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD% -t cluster
    ```

    Dieser Befehl fügt dem Ordner `aml_config` des Projekts zwei Dateien (`myhdi.runconfig` und `myhdi.compute`) hinzu.

1. Öffnen Sie die Datei `myhdi.compute` in einem Text-Editor Ihrer Wahl. Ändern Sie die Zeile `yarnDeployMode: cluster` so, dass sie `yarnDeployMode: client` lautet, und speichern und schließen Sie die Datei dann.
1. Führen Sie den folgenden Befehl aus, um Ihre Umgebung für die Verwendung vorzubereiten:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Installieren lokaler Abhängigkeiten

Öffnen Sie eine CLI aus Azure Machine Learning Workbench, und installieren Sie die Abhängigkeiten, die für die lokale Ausführung benötigt werden, indem Sie den folgenden Befehl ausgeben:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn
```

## <a name="data-acquisition-and-understanding"></a>Datenerfassung und -auswertung

Dieses Szenario verwendet öffentlich verfügbare Luftbilddaten aus dem US-amerikanischen [National Agriculture Imagery Program](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) mit einer Auflösung von einem Meter. Wir haben Sammlungen von PNG-Dateien (224 Pixel x 224 Pixel) aus den ursprünglichen NAIP-Daten ausgeschnitten und nach den Flächennutzungskennzeichnungen aus der US-amerikanischen [National Land Cover-Datenbank](https://www.mrlc.gov/nlcd2011.php) sortiert. Ein Beispielbild mit der Bezeichnung „Developed“ wird in voller Größe angezeigt:

![Eine Beispielkachel für einen besiedelten Landstrich](media/scenario-aerial-image-classification/sample-tile-developed.png)

Klassenabgeglichene Sätze mit Bildern von ungefähr 44k und 11k werden für das Modelltraining bzw. die -validierung verwendet. Wir zeigen die Modellbereitstellung anhand einer Bildsammlung mit ungefähr 67k, die die Region Middlesex County (Massachusetts, USA) zeigt (den Sitz des New England Research and Development Center (NERD) von Microsoft). Weitere Informationen zum Aufbau dieser Bildsammlungen finden Sie unter [Git-Repository: Embarrassingly Parallel Image Classification (verblüffend parallele Bildklassifikation)](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Lage der Region Middlesex County (Massachusetts, USA)](media/scenario-aerial-image-classification/middlesex-ma.png)

Während des Setups wurden die in diesem Beispiel verwendeten Sammlungen von Luftbildaufnahmen in das Speicherkonto übertragen, das Sie erstellt haben. Die Trainings-, Überprüfungs- und Operationalisierungsbilder sind alle PNG-Dateien (224 Pixel x 224 Pixel) mit einer Auflösung von einem Pixel pro Quadratmeter. Die Trainings- und Überprüfungsbilder wurden in Unterordnern basierend auf ihrer Flächennutzungskennzeichnung organisiert. (Die Flächennutzungskennzeichnungen der Operationalisierungsbilder sind unbekannt und in vielen Fällen nicht eindeutig, einige dieser Bilder enthalten mehrere Flächenarten). Weitere Informationen zum Aufbau dieser Bildsammlungen finden Sie unter [Git-Repository: Embarrassingly Parallel Image Classification (verblüffend parallele Bildklassifikation)](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

So zeigen Sie Beispielbilder in Ihrem Azure-Speicherkonto an (optional):
1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie in der Suchleiste am oberen Rand des Bildschirms nach dem Namen Ihres Speicherkontos. Klicken Sie in den Suchergebnissen auf Ihr Speicherkonto.
2. Klicken Sie auf den Link „Blobs“ im Hauptbereich des Speicherkontos.
3. Klicken Sie auf den Container mit dem Namen „train“. Es sollte eine Liste von Verzeichnissen angezeigt werden, die gemäß der Flächennutzung benannt sind.
4. Klicken Sie auf eines dieser Verzeichnisse, um die Liste der darin enthaltenen Bilder zu laden.
5. Klicken Sie auf ein beliebiges Bild, und laden Sie es herunter, um das Bild anzuzeigen.
6. Falls gewünscht, klicken Sie auf die Container mit den Namen „test“ und „middlesexma2016“, um auch deren Inhalt anzuzeigen.

## <a name="modeling"></a>Modellierung

### <a name="training-models-with-mmlspark"></a>Trainieren von Modellen mit MMLSpark
Das Skript `run_mmlspark.py` im Unterordner „Code\02_Modeling“ des Workbench-Projekts dient zum Trainieren eines [MMLSpark](https://github.com/Azure/mmlspark)-Modells für die Bildklassifizierung. Das erste Skript versieht die Bilder der Trainingssammlung mithilfe eines Bildklassifizierungs-DNN, das mit dem ImageNet-Dataset (AlexNet oder ein ResNet mit 18 Schichten) vorab trainiert wurde, mit Funktionen. Das Skript verwendet dann die mit Funktionen versehenen Bilder zum Trainieren eines MMLSpark-Modells (einer zufälligen Gesamtstruktur oder eines logistischen Regressionsmodells), um die Bilder zu klassifizieren. Die Testbildsammlung wird dann mit Funktionen versehen und mit dem trainierten Modell bewertet. Die Genauigkeit der Vorhersagen des Modells für die Testsammlung wird berechnet und im Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench protokolliert. Schließlich werden das trainierte MMLSpark-Modell und seine Vorhersagen für die Testsammlung im Blobspeicher gespeichert.

Wählen Sie einen eindeutigen Ausgabemodellnamen für das trainierte Modell, einen vortrainierten Modelltyp und einen MMLSpark-Modelltyp aus. Fügen Sie Ihre Auswahl wie angegeben in die folgenden Befehlsvorlage ein, und beginnen Sie dann mit dem erneuten Training, indem Sie den folgenden Befehl in einer Azure ML-Befehlszeilenschnittstelle ausführen:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Ein zusätzlicher Parameter `--sample_frac` kann zum Trainieren und Testen des Modells mit einer Teilmenge der verfügbaren Daten verwendet werden. Mit einem kleinen Beispielbruchteil verringern sich die Laufzeit- und Arbeitsspeicheranforderungen zu Lasten der Genauigkeit des trainierten Modells. Wenn Sie weitere Informationen zu diesem und anderen Parametern benötigen, führen Sie `python Code\02_Modeling\run_mmlspark.py -h` aus.

Benutzer sollten dieses Skript mehrere Male mit verschiedenen Eingabeparametern ausführen. Die Leistung der sich ergebenden Modelle kann dann im Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench verglichen werden.

### <a name="comparing-model-performance-using-the-workbench-run-history-feature"></a>Vergleichen der Modellleistung mithilfe des Features „Ausführungsverlauf“ von Workbench

Nachdem Sie mindestens zwei Trainingsläufe dieses Typs ausgeführt haben, navigieren Sie durch Klicken auf das Uhrsymbol auf der linken Menüleiste in Workbench zum Feature „Ausführungsverlauf“. Wählen Sie `run_mmlspark.py` aus der Liste der Skripts auf der linken Seite aus. Es wird ein Bereich geladen, der die Genauigkeit der Testmenge für alle Ausführungen vergleicht. Um weitere Details anzuzeigen, scrollen Sie nach unten und klicken auf den Namen einer einzelnen Ausführung.

## <a name="deployment"></a>Bereitstellung

Zum Anwenden eines trainierten Modells auf Luftbilder der Region Middlesex County (Massachusetts, USA) mit Remoteausführung für HDInsight fügen Sie den Namen des gewünschten Modells in den folgenden Befehl ein und führen den Befehl dann aus:

```
az ml experiment submit -c myhdi Code\03_Deployment\batch_score_spark.py --config_filename Code/settings.cfg --output_model_name [trained model name chosen earlier]
```

Ein zusätzlicher Parameter `--sample_frac` kann zum Operationalisieren des Modells mit einer Teilmenge der verfügbaren Daten verwendet werden. Mit einem kleinen Beispielbruchteil verringern sich die Laufzeit- und Arbeitsspeicheranforderungen zu Lasten der Vollständigkeit der Vorhersage. Wenn Sie weitere Informationen zu diesem und anderen Parametern benötigen, führen Sie `python Code\03_Deployment\batch_score_spark -h` aus.

Dieses Skript schreibt die Vorhersagen des Modells in Ihr Speicherkonto. Die Vorhersagen können wie im nächsten Abschnitt beschrieben untersucht werden.

## <a name="visualization"></a>Visualisierung

Das Jupyter Notebook „Model prediction analysis“ im Unterordner „Code\04_Result_Analysis“ des Workbench-Projekts visualisiert die Vorhersagen eines Modells. Laden Sie das Notebook wie folgt, und führen Sie es aus:
1. Öffnen Sie das Projekt in Workbench, und klicken Sie auf das Ordnersymbol („Dateien“) im Menü auf der linken Seite, um die Verzeichnisliste zu laden.
2. Navigieren Sie zum Unterordner „Code\04_Result_Analysis“, und klicken Sie auf das Notebook mit dem Namen „Model prediction analysis“. Eine Vorschau des Notebooks sollte angezeigt werden.
3. Klicken Sie auf „Notebook-Server starten“ um das Notebook zu laden.
4. Geben Sie den Namen des Modells, dessen Ergebnisse analysiert werden sollen, wie angegeben in die erste Zelle ein.
5. Klicken Sie auf „Zelle“ > „Alle ausführen“, um alle Zellen im Notebook auszuführen.
6. Lesen Sie zusammen mit dem Notebook weitere Informationen, um mehr über die Analysen und Visualisierungen zu erfahren, die es bereitstellt.

## <a name="cleanup"></a>Cleanup
Nachdem Sie das Beispiel abgeschlossen haben, wird empfohlen, alle von Ihnen erstellten Ressourcen zu löschen, indem Sie den folgenden Befehl in der Azure-Befehlszeilenschnittstelle ausführen:

  ```
  az group delete --name %AZURE_RESOURCE_GROUP%
  ```

## <a name="references"></a>Referenzen

- [The Embarrassingly Parallel Image Classification Repository (Repository für verblüffend parallele Bildklassifikation)](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification)
   - Beschreibt die Dataseterstellung aus kostenlos verfügbarem Bildmaterial und Kennzeichnungen.
- [MMLSpark](https://github.com/Azure/mmlspark)-GitHub-Repository
   - Enthält weitere Beispiele für das Modelltraining und die -auswertung mit MMLSpark.

## <a name="conclusions"></a>Zusammenfassung

Mit Azure Machine Learning Workbench können Datenanalysten ihren Code auf einfache Weise auf Remotecomputezielen bereitstellen. In diesem Beispiel wurde lokaler Code für die Remoteausführung auf einem HDInsight-Cluster bereitgestellt. Über das Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench wurde die Leistung mehrerer Modelle nachverfolgt und das präziseste Modell identifiziert. Das Feature „Jupyter Notebooks“ von Workbench war bei der Visualisierung der Vorhersagen unserer Modelle in einer interaktiven, grafischen Umgebung hilfreich.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie sich ausführlicher mit diesem Beispiel beschäftigen möchten:
- Klicken Sie im Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench auf die Zahnradsymbole, um die anzuzeigenden Diagramme und Metriken auszuwählen.
- Untersuchen Sie die Beispielskripts für Anweisungen, die `run_logger` aufrufen. Stellen Sie sicher, dass Sie verstehen, wie jede Metrik aufgezeichnet wird.
- Untersuchen Sie die Beispielskripts für Anweisungen, die `blob_service` aufrufen. Stellen Sie sicher, dass Sie verstehen, wie trainierte Modelle und Vorhersagen in der Cloud gespeichert und aus ihr abgerufen werden.
- Untersuchen Sie den Inhalt der Container, die in Ihrem Blobspeicherkonto erstellt werden. Stellen Sie sicher, dass Sie verstehen, welches Skript oder welcher Befehl für das Erstellen jeder Gruppe von Dateien verantwortlich ist.
- Ändern Sie das Trainingsskript so,dass ein anderer Typ von MMLSpark-Modell trainiert wird, oder ändern Sie die Modellhyperparameter. Verwenden Sie das Feature „Ausführungsverlauf“, um zu ermitteln, ob Ihre Änderungen die Genauigkeit des Modells erhöht oder verringert haben.

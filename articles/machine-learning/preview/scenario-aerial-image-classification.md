---
title: Klassifizierung von Luftbildern | Microsoft-Dokumentation
description: "Anweisungen für die Klassifizierung von Luftbildern in der Praxis"
author: mawah
ms.author: mawah
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.topic: article
ms.service: machine-learning
services: machine-learning
ms.date: 10/27/2017
ms.openlocfilehash: 07e74c64e587cce99612cd5047516bf131943f2e
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="aerial-image-classification"></a>Klassifizierung von Luftbildern

In diesem Beispiel wird veranschaulicht, wie Sie mit Azure Machine Learning Workbench verteilte Trainings- und Operationalisierungsfunktionen von Modellen zur Bildklassifizierung koordinieren. Für das Training werden zwei Ansätze verwendet: (i) das Verfeinern eines tiefen neuronalen Netzwerks mithilfe eines [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/)-GPU-Clusters und (ii) das Verwenden des Pakets [Microsoft Machine Learning für Apache Spark (MMLSpark)](https://github.com/Azure/mmlspark), um Bilder mithilfe von vorab trainierten CNTK-Modellen mit Funktionen auszustatten und Klassifizierer mithilfe der abgeleiteten Funktionen zu trainieren. Die trainierten Modelle werden anschließend mithilfe eines [Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)-Clusters parallel auf große Bildzusammenstellungen in der Cloud angewendet. Dadurch wird das Skalieren der Geschwindigkeit des Trainings und der Operationalisierung ermöglicht, indem Workerknoten hinzugefügt oder entfernt werden.

In diesem Beispiel werden zwei Ansätze für den Lerntransfer hervorgehoben, die vorab trainierte Modelle nutzen, um die Kosten durch das Trainieren eines tiefen neuronalen Netzwerks von Grund auf zu vermeiden. Das erneute Trainieren eines tiefen neuronalen Netzwerks erfordert in der Regel GPU-Computing, führt jedoch zu höherer Genauigkeit, wenn der Trainingssatz ausreichend groß ist. Das Trainieren eines einfachen Klassifizierers durch mit Funktionen ausgestattete Bilder erfordert kein GPU-Computing, ist grundsätzlich schnell, beliebig skalierbar und erfordert weniger Parameter. Diese Methode eignet sich daher besonders, wenn nur wenige Trainingsbeispiele zur Verfügung stehen, wie z.B. bei benutzerdefinierten Anwendungsfällen. 

Der Spark-Cluster, der in diesem Beispiel verwendet wird, verfügt über 40 Workerknoten und kostet im Betrieb 40 $ pro Stunde. Die Batch AI-Clusterressourcen enthalten acht GPUs und kosten im Betrieb 10 $ pro Stunde. Diese exemplarische Vorgehensweise dauert etwa drei Stunden. Führen Sie nach Abschluss der Vorgehensweise die Bereinigungsanweisungen aus, um die erstellten Ressourcen zu entfernen und zu verhindern, dass weitere Gebühren anfallen.

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

In den [ausführlichen Anweisungen](https://github.com/MicrosoftDocs/azure-docs-pr/tree/release-ignite-aml-v2/articles/machine-learning/) erfahren Sie zunächst, wie ein Azure-Speicherkonto und ein Spark-Cluster erstellt und vorbereitet werden, einschließlich Datenübertragung und Installation der Abhängigkeit. Anschließend wird beschrieben, wie Trainingsaufträge gestartet und die Leistung der resultierenden Modelle verglichen werden. Schließlich wird veranschaulicht, wie das ausgewählte Modell auf eine große Bildzusammenstellung im Spark-Cluster angewendet wird und die Vorhersageergebnisse lokal analysiert werden.


## <a name="set-up-the-execution-environment"></a>Einrichten der Ausführungsumgebung

Befolgen Sie die folgenden Anweisungen, um die Ausführungsumgebung für dieses Beispiel einzurichten.

### <a name="prerequisites"></a>Voraussetzungen
- Ein [Azure-Konto](https://azure.microsoft.com/en-us/free/) (kostenlose Testversionen verfügbar).
    - Erstellen Sie einen HDInsight Spark-Cluster mit 40 Workerknoten (insgesamt 168 Kerne). Stellen Sie sicher, dass Ihr Konto über genügend verfügbare Kerne verfügt. Gehen Sie dazu im Azure-Portal auf die Registerkarte „Nutzung + Kontingente“ für Ihr Abonnement.
       - Wenn Sie weniger Kerne zur Verfügung haben, können Sie die HDInsight-Clustervorlage ändern und die Anzahl der bereitgestellten Worker reduzieren. Die Anweisungen hierzu finden Sie im Abschnitt „Erstellen des HDInsight Spark-Clusters“.
    - In diesem Beispiel wird ein Batch AI-Trainingscluster mit zwei NC6-VMs (1 GPU, 6 vCPU) erstellt. Stellen Sie sicher, dass Ihr Konto über genügend verfügbare Kerne in der Region „USA, Osten“ verfügt. Gehen Sie dazu im Azure-Portal auf die Registerkarte „Nutzung + Kontingente“ für Ihr Abonnement.
- [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md):
    - Befolgen Sie die Anleitung im [Schnellstart zum Installieren und Erstellen](quickstart-installation.md), um Azure Machine Learning Workbench zu installieren und Konten für die Modellverwaltung und zum Experimentieren zu erstellen.
- [Batch AI](https://github.com/Azure/BatchAI) Python SDK und Azure-CLI 2.0
    - Installieren Sie das Batch AI SDK und die Azure-CLI 2.0, indem Sie den Anweisungen im Abschnitt [Prerequisites (Voraussetzungen)](https://github.com/Azure/BatchAI/tree/master/recipes) befolgen.
        - In diesem Dokument verwendet Azure Machine Learning Workbench eine separate Verzweigung der Azure-CLI 2.0. Aus Gründen der Übersichtlichkeit wird die Workbenchversion der CLI als „eine CLI, die über die Azure Machine Learning Workbench gestartet wird“ und die allgemeine Releaseversion (die Batch AI enthält) als „Azure-CLI 2.0“ bezeichnet.
    - Erstellen Sie eine Azure Active Directory-Anwendung und einen Dienstprinzipal, indem Sie [diese Anweisungen](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials) befolgen. Notieren Sie sich die Client-ID, das Geheimnis und die Mandanten-ID.
- [AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy), ein kostenloses Hilfsprogramm zur Koordinierung der Dateiübertragung zwischen Azure-Speicherkonten:
    - Stellen Sie sicher, dass sich der Ordner mit der ausführbaren AzCopy-Datei in der PATH-Umgebungsvariable Ihres Systems befindet. (Anweisungen zum Ändern von Umgebungsvariablen finden Sie [hier](https://support.microsoft.com/en-us/help/310519/how-to-manage-environment-variables-in-windows-xp).)
- Einen SSH-Client, es wird [PuTTY](http://www.putty.org/) empfohlen.

Dieses Beispiel wurde auf einem Windows 10-PC getestet. Sie sollten es auf jedem Windows-Computer, einschließlich virtuellen Azure Data Science-Computern, ausführen können. Die Azure-CLI 2.0 wurde gemäß [diesen Anweisungen](https://github.com/Azure/azure-sdk-for-python/wiki/Contributing-to-the-tests#getting-azure-credentials) aus einer MSI-Datei installiert. Wenn Sie dieses Beispiel unter macOS ausführen, sind möglicherweise kleinere Änderungen erforderlich (z.B. Änderungen an Dateipfaden).

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

1. Nachdem Sie Ihr Projekt in Azure Machine Learning Workbench geladen haben, öffnen Sie eine Befehlszeilenschnittstelle (Command Line Interface, CLI), indem Sie auf „Datei -> Eingabeaufforderung öffnen“ klicken.
    Verwenden Sie diese Version der CLI für den Großteil des Tutorials. (Wo es erforderlich ist, werden Sie dazu aufgefordert, eine andere Version der CLI zu öffnen, um Batch AI-Ressourcen vorzubereiten.)

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

1. Führen Sie folgenden Befehl aus, um die Speicherkontoschlüssel aufzulisten:

    ```
    az storage account keys list --resource-group %AZURE_RESOURCE_GROUP% --account-name %STORAGE_ACCOUNT_NAME%
    ```

    Notieren Sie sich den Wert von `key1` als Speicherschlüssel im folgenden Befehl, und führen Sie dann den Befehl zum Speichern des Werts aus.
    ```
    set STORAGE_ACCOUNT_KEY=[storage account key]
    ```
1. Erstellen Sie mit dem folgenden Befehl eine Dateifreigabe mit dem Namen `baitshare` in Ihrem Speicherkonto:

    ```
    az storage share create --account-name %STORAGE_ACCOUNT_NAME% --account-key %STORAGE_ACCOUNT_KEY% --name baitshare
    ```
1. Laden Sie die Datei `settings.cfg` aus dem Unterverzeichnis „Code“ des Azure Machine Learning Workbench-Projekts in Ihrem bevorzugten Text-Editor, und fügen Sie den Speicherkontonamen und Schlüssel wie angegeben ein. Speichern und schließen Sie die `settings.cfg`-Datei.
1. Wenn nicht bereits geschehen, laden Sie das Hilfsprogramm [AzCopy](http://aka.ms/downloadazcopy) herunter und installieren es. Stellen Sie sicher, dass sich die ausführbare AzCopy-Datei in Ihrem Systempfad befindet, indem Sie „AzCopy“ eingeben und die EINGABETASTE drücken, um die zugehörige Dokumentation anzuzeigen.
1. Geben Sie die folgenden Befehle aus, um alle Beispieldaten, vortrainierten Modelle und Modelltrainingsskripts in die entsprechenden Speicherorte in Ihrem Speicherkonto zu kopieren:

    ```
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/test /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/test /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/train /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/train /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/middlesexma2016 /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/middlesexma2016 /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.blob.core.windows.net/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/pretrainedmodels /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/pretrainedmodels /DestKey:%STORAGE_ACCOUNT_KEY% /S
    AzCopy /Source:https://mawahsparktutorial.blob.core.windows.net/scripts /SourceSAS:"?sv=2017-04-17&ss=bf&srt=sco&sp=rwl&se=2037-08-25T22:02:55Z&st=2017-08-25T14:02:55Z&spr=https,http&sig=yyO6fyanu9ilAeW7TpkgbAqeTnrPR%2BpP1eh9TcpIXWw%3D" /Dest:https://%STORAGE_ACCOUNT_NAME%.file.core.windows.net/baitshare/scripts /DestKey:%STORAGE_ACCOUNT_KEY% /S
    ```

    Gehen Sie davon aus, dass die Dateiübertragung bis zu 20 Minuten in Anspruch nimmt. Während Sie warten, können Sie mit folgendem Abschnitt fortfahren: Sie müssen möglicherweise eine weitere Befehlszeilenschnittstelle über Workbench öffnen und die temporären Variablen dort neu definieren.

#### <a name="create-the-hdinsight-spark-cluster"></a>Erstellen des HDInsight Spark-Clusters

Die empfohlene Methode zum Erstellen eines HDInsight-Clusters verwendet die Resource Manager-Vorlage des HDInsight Spark-Clusters, die im Unterordner „Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning“ dieses Projekts enthalten ist.

1. Die HDInsight Spark-Clustervorlage ist die Datei „template.json“ im Unterordner „Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning“ dieses Projekts. Die Vorlage erstellt standardmäßig einen Spark-Cluster mit 40 Workerknoten. Wenn Sie diese Anzahl anpassen müssen, öffnen Sie die Vorlage in Ihrem bevorzugten Text-Editor, und ersetzen Sie alle Instanzen von „40“ durch die Workerknotenanzahl Ihrer Wahl.
    - Wenn die Anzahl der ausgewählten Workerknoten klein ist, können ggf. Fehler des Typs „Nicht genügend Arbeitsspeicher“ auftreten. Um Speicherfehlern zu begegnen, können Sie die Trainings- und Operationalisierungsskripts für eine Teilmenge der verfügbaren Daten ausführen. Dies wird weiter unten in diesem Dokument beschrieben.
2. Wählen Sie einen eindeutigen Namen und ein Kennwort für den HDInsight-Cluster aus, und fügen Sie diese Angaben wie angegeben in den folgenden Befehl ein: Erstellen Sie dann den Cluster, indem Sie die Befehle ausgeben:

    ```
    set HDINSIGHT_CLUSTER_NAME=[HDInsight cluster name]
    set HDINSIGHT_CLUSTER_PASSWORD=[HDInsight cluster password]
    az group deployment create --resource-group %AZURE_RESOURCE_GROUP% --name hdispark --template-file "Code\01_Data_Acquisition_and_Understanding\01_HDInsight_Spark_Provisioning\template.json" --parameters storageAccountName=%STORAGE_ACCOUNT_NAME%.blob.core.windows.net storageAccountKey=%STORAGE_ACCOUNT_KEY% clusterName=%HDINSIGHT_CLUSTER_NAME% clusterLoginPassword=%HDINSIGHT_CLUSTER_PASSWORD%
    ```

Die Bereitstellung des Clusters kann bis zu 30 Minuten (einschließlich Ausführen von Bereitstellung und Skriptaktionen) dauern.

### <a name="set-up-batch-ai-resources"></a>Einrichten von Batch AI-Ressourcen

Während Sie darauf warten, dass der Datentransfer Ihres Speicherkontos und die Bereitstellung des Spark-Clusters abgeschlossen werden, können Sie den Batch AI-Netzwerkdateiserver (Network File Server, NFS) und den GPU-Cluster vorbereiten. Öffnen Sie eine Azure-CLI 2.0-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

```
az --version 
```

Überprüfen Sie, ob `batchai` unter den installierten Modulen aufgelistet ist. Falls nicht, verwenden Sie möglicherweise eine unterschiedliche Befehlszeilenschnittstelle (z.B. eine, die über Workbench geöffnet wurde).

Überprüfen Sie anschließend, ob die Registrierung des Anbieters erfolgreich abgeschlossen wurde. (Die Registrierung des Anbieters kann bis zu 15 Minuten dauern und läuft möglicherweise noch, wenn Sie die [Batch AI-Setupanweisungen](https://github.com/Azure/BatchAI/tree/master/recipes) vor Kurzem abgeschlossen haben.) Überprüfen Sie, ob „Microsoft.Batch“ und „Microsoft.BatchAI“ mit dem Status „Registriert“ in der Ausgabe des folgenden Befehls angezeigt werden:

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Falls nicht, führen Sie die folgenden Befehle zur Registrierung des Anbieters aus, und warten Sie ungefähr 15 Minuten, bis die Registrierung abgeschlossen ist.
```
az provider register --namespace Microsoft.Batch
az provider register --namespace Microsoft.BatchAI
```

Ändern Sie die folgenden Befehle, um die in Klammern stehenden Ausdrücke durch die Werte zu ersetzen, die Sie zuvor bei der Erstellung der Ressourcengruppe und des Speicherkontos verwendet haben. Speichern Sie die Werte dann als Variablen, indem Sie die Befehle ausführen:
```
az account set --subscription [subscription ID]
set AZURE_RESOURCE_GROUP=[resource group name]
set STORAGE_ACCOUNT_NAME=[storage account name]
set STORAGE_ACCOUNT_KEY=[storage account key]
az configure --defaults location=eastus
az configure --defaults group=%AZURE_RESOURCE_GROUP%
```

Identifizieren Sie den Ordner, der Ihr Azure Machine Learning-Projekt (z.B. `C:\Users\<your username>\AzureML\aerialimageclassification`) enthält. Ersetzen Sie die in Klammern stehenden Werte durch den Dateipfad des Ordners (ohne abschließenden umgekehrten Schrägstrich), und führen Sie den Befehl aus:
```
set PATH_TO_PROJECT=[The filepath of your project's root directory]
```
Sie sind nun bereit dafür, die Batch AI-Ressourcen zu erstellen, die für dieses Tutorial erforderlich sind.

#### <a name="prepare-the-batch-ai-network-file-server"></a>Vorbereiten des Batch AI-Netzwerkdateiservers

Ihr Batch AI-Cluster greift auf Ihre Trainingsdaten auf einem Netzwerkdateiserver zu. Der Datenzugriff kann um ein Vielfaches schneller sein, wenn Sie ihn über einen NFS statt über Azure File Share oder Azure Blob Storage durchführen.

1. Führen Sie folgenden Befehl aus, um einen Netzwerkdateiserver zu erstellen:

    ```
    az batchai file-server create -n landuseclassifier -u demoUser -p Dem0Pa$$w0rd --vm-size Standard_D2_V2 --disk-count 1 --disk-size 1000 --storage-sku Premium_LRS
    ```

1. Überprüfen Sie den Bereitstellungsstatus Ihres Netzwerkdateiservers mithilfe des folgenden Befehls:

    ```
    az batchai file-server list
    ```

    Wenn der „provisioningState“ (Bereitstellungsstatus) des Netzwerkdateiservers namens „landuseclassifier“ als „erfolgreich“ angezeigt wird, ist dieser für die Verwendung bereit. Rechnen Sie damit, dass die Bereitstellung etwa fünf Minuten in Anspruch nimmt.
1. Suchen Sie die IP-Adresse Ihres NFS in der Ausgabe des vorherigen Befehls (die Eigenschaft „fileServerPublicIp“ unter „mountSettings“). Fügen Sie die IP-Adresse wie angegeben in den folgenden Befehl ein, und speichern Sie den Wert anschließend, indem Sie den Befehl ausführen:

    ```
    set AZURE_BATCH_AI_TRAINING_NFS_IP=[your NFS IP address]
    ```

1. Verwenden Sie Ihr bevorzugtes SSH-Tool (im folgenden Beispielbefehl wird [PuTTY](http://www.putty.org/) verwendet), um das `prep_nfs.sh`-Skript des Projekts auf dem NFS auszuführen, damit die Bildzusammenstellungen für das Training und die Überprüfung dorthin übertragen werden.

    ```
    putty -ssh demoUser@%AZURE_BATCH_AI_TRAINING_NFS_IP% -pw Dem0Pa$$w0rd -m %PATH_TO_PROJECT%\Code\01_Data_Acquisition_and_Understanding\02_Batch_AI_Training_Provisioning\prep_nfs.sh
    ```

    Es ist nicht ungewöhnlich, dass die Updates für den Datendownload und Extraktionsfortschritt so schnell über das Shell-Fenster scrollen, dass sie unlesbar sind.

Bei Bedarf können Sie bestätigen, dass die Datenübertragung wie geplant ausgeführt wurde, indem Sie sich mit Ihrem bevorzugten SSH-Tool beim Dateiserver anmelden und die Inhalte des `/mnt/data`-Verzeichnisses überprüfen. Sie sollten zwei Ordner vorfinden, „training_images“ und „validation_images“. Jeder davon enthält Unterordner, die nach den Flächennutzungskategorien benannt sind.  Die Trainings- und Überprüfungssätze sollten Bilder von ungefähr 44k und 11k enthalten.

#### <a name="create-a-batch-ai-cluster"></a>Erstellen eines Batch AI-Clusters

1. Erstellen Sie den Cluster, indem Sie folgenden Befehl ausgeben:

    ```
    set AZURE_BATCHAI_STORAGE_ACCOUNT=%STORAGE_ACCOUNT_NAME%
    set AZURE_BATCHAI_STORAGE_KEY=%STORAGE_ACCOUNT_KEY%
    az batchai cluster create -n landuseclassifier -u demoUser -p Dem0Pa$$w0rd --afs-name baitshare --nfs landuseclassifier --image UbuntuDSVM --vm-size STANDARD_NC6 --max 2 --min 2 
    ```

1. Verwenden Sie folgenden Befehl, um den Bereitstellungsstatus Ihres Clusters zu überprüfen:

    ```
    az batchai cluster list
    ```

    Wenn der Zuordnungsstatus für den Cluster namens „landuseclassifier“ sich von „Größe wird geändert“ zu „Bereit“ ändert, ist es möglich, Aufträge zu übermitteln. Aufträge werden jedoch nicht ausgeführt, bis alle virtuellen Computer im Cluster den Status „Wird vorbereitet“ verlassen haben. Wenn die Eigenschaft „Fehler“ des Clusters nicht NULL ist, ist ein Fehler während der Erstellung des Clusters aufgetreten, und er sollte nicht verwendet werden.

#### <a name="record-batch-ai-training-credentials"></a>Hinterlegen der Anmeldeinformationen für das Batch AI-Training

Während Sie darauf warten, dass die Zuordnung der Cluster abgeschlossen wird, öffnen Sie die `settings.cfg`-Datei aus dem Unterverzeichnis „Code“ dieses Projekts in Ihrem bevorzugten Text-Editor. Aktualisieren Sie folgende Variablen mit Ihren eigenen Anmeldeinformationen:
- `bait_subscription_id` (die 36 Zeichen lange ID Ihres Azure-Abonnements)
- `bait_aad_client_id` (die Anwendungs- bzw. Client-ID von Azure Active Directory, die im Abschnitt „Voraussetzungen“ erwähnt wurde)
- `bait_aad_secret` (das Anwendungsgeheimnis von Azure Active Directory, der im Abschnitt „Voraussetzungen“ erwähnt wurde)
- `bait_aad_tenant` (die Mandanten-ID von Azure Active Directory, die im Abschnitt „Voraussetzungen“ erwähnt wurde)
- `bait_region` (für dieses Dokument ist die einzige Option „USA, Osten“)
- `bait_resource_group_name` (die Ressourcengruppe, die Sie zuvor ausgewählt haben)

Sobald Sie diese Werte zugeordnet haben, sollten die geänderten Zeilen der Datei „settings.cfg“ folgendem Text ähneln:

```
[Settings]
    # Credentials for the Azure Storage account
    storage_account_name = yoursaname
    storage_account_key = kpIXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXQ==
    
    # Batch AI training credentials
    bait_subscription_id = 0caXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX9c3
    bait_aad_client_id = d0aXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX7f8
    bait_aad_secret = ygSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX6I=
    bait_aad_tenant = 72fXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXb47
    bait_region = eastus
    bait_resource_group_name = yourrgname
```

Speichern und schließen Sie `settings.cfg`.

Sie können nun das CLI-Fenster schließen, in dem Sie die Befehle für das Erstellen der Batch AI-Ressourcen ausgeführt haben. Alle weiteren Schritte in diesem Tutorial verwenden eine CLI, die über Azure Machine Learning Workbench gestartet wird.

### <a name="prepare-the-azure-machine-learning-workbench-execution-environment"></a>Vorbereiten der Azure Machine Learning Workbench-Ausführungsumgebung

#### <a name="register-the-hdinsight-cluster-as-an-azure-machine-learning-workbench-compute-target"></a>Registrieren des HDInsight-Clusters als Azure Machine Learning Workbench-Computeziel

Nach dem Abschluss der Erstellung des HDInsight-Clusters registrieren Sie den Cluster wie folgt als Computeziel für Ihr Projekt:

1.  Geben Sie den folgenden Befehl in der Azure Machine Learning-Befehlszeilenschnittstelle aus:

    ```
    az ml computetarget attach --name myhdi --address %HDINSIGHT_CLUSTER_NAME%-ssh.azurehdinsight.net --username sshuser --password %HDINSIGHT_CLUSTER_PASSWORD% -t cluster
    ```

    Dieser Befehl fügt dem Ordner `aml_config` des Projekts zwei Dateien (`myhdi.runconfig` und `myhdi.compute`) hinzu.

1. Öffnen Sie die Datei `myhdi.compute` in einem Text-Editor Ihrer Wahl. Ändern Sie die Zeile `yarnDeployMode: cluster` so, dass sie `yarnDeployMode: client` lautet, und speichern und schließen Sie die Datei dann.
1. Führen Sie den folgenden Befehl aus, um Ihre HDInsight-Umgebung für die Verwendung vorzubereiten:
   ```
   az ml experiment prepare -c myhdi
   ```

#### <a name="install-local-dependencies"></a>Installieren lokaler Abhängigkeiten

Öffnen Sie eine CLI aus Azure Machine Learning Workbench, und installieren Sie die Abhängigkeiten, die für die lokale Ausführung benötigt werden, indem Sie den folgenden Befehl ausgeben:

```
pip install matplotlib azure-storage==0.36.0 pillow scikit-learn azure-mgmt-batchai
```

## <a name="data-acquisition-and-understanding"></a>Datenerfassung und -auswertung

Dieses Szenario verwendet öffentlich verfügbare Luftbilddaten aus dem US-amerikanischen [National Agriculture Imagery Program](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) mit einer Auflösung von einem Meter. Wir haben Sammlungen von PNG-Dateien (224 Pixel x 224 Pixel) aus den ursprünglichen NAIP-Daten ausgeschnitten und nach den Flächennutzungskennzeichnungen aus der US-amerikanischen [National Land Cover-Datenbank](https://www.mrlc.gov/nlcd2011.php) sortiert. Ein Beispielbild mit der Bezeichnung „Developed“ wird in voller Größe angezeigt:

![Eine Beispielkachel für einen besiedelten Landstrich](media/scenario-aerial-image-classification/sample-tile-developed.png)

Klassenabgeglichene Sätze mit Bildern von ungefähr 44k und 11k werden für das Modelltraining bzw. die -validierung verwendet. Wir zeigen die Modellbereitstellung anhand einer Bildsammlung mit ungefähr 67k, die die Region Middlesex County (Massachusetts, USA) zeigt (den Sitz des New England Research and Development Center (NERD) von Microsoft). Weitere Informationen zum Aufbau dieser Bildsammlungen finden Sie unter [Git-Repository: Embarrassingly Parallel Image Classification (verblüffend parallele Bildklassifikation)](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

![Lage der Region Middlesex County (Massachusetts, USA)](media/scenario-aerial-image-classification/middlesex-ma.png)

Während des Setups wurden die in diesem Beispiel verwendeten Sammlungen von Luftbildaufnahmen in das Speicherkonto übertragen, das Sie erstellt haben. Die Trainings-, Überprüfungs- und Operationalisierungsbilder sind alle PNG-Dateien (224 Pixel x 224 Pixel) mit einer Auflösung von einem Pixel pro Quadratmeter. Die Trainings- und Überprüfungsbilder wurden in Unterordnern basierend auf ihrer Flächennutzungskennzeichnung organisiert. (Die Flächennutzungskennzeichnungen der Operationalisierungsbilder sind unbekannt und in vielen Fällen nicht eindeutig, einige dieser Bilder enthalten mehrere Flächenarten). Weitere Informationen zum Aufbau dieser Bildsammlungen finden Sie unter [Git-Repository: Embarrassingly Parallel Image Classification (verblüffend parallele Bildklassifikation)](https://github.com/Azure/Embarrassingly-Parallel-Image-Classification).

So zeigen Sie Beispielbilder in Ihrem Azure-Speicherkonto an (optional):
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
1. Suchen Sie in der Suchleiste am oberen Rand des Bildschirms nach dem Namen Ihres Speicherkontos. Klicken Sie in den Suchergebnissen auf Ihr Speicherkonto.
2. Klicken Sie auf den Link „Blobs“ im Hauptbereich des Speicherkontos.
3. Klicken Sie auf den Container mit dem Namen „train“. Es sollte eine Liste von Verzeichnissen angezeigt werden, die gemäß der Flächennutzung benannt sind.
4. Klicken Sie auf eines dieser Verzeichnisse, um die Liste der darin enthaltenen Bilder zu laden.
5. Klicken Sie auf ein beliebiges Bild, und laden Sie es herunter, um das Bild anzuzeigen.
6. Falls gewünscht, klicken Sie auf die Container mit den Namen „test“ und „middlesexma2016“, um auch deren Inhalt anzuzeigen.

## <a name="modeling"></a>Modellierung

### <a name="training-models-with-azure-batch-ai"></a>Trainingsmodelle mit Azure Batch AI

Das `run_batch_ai.py`-Skript im Unterordner „Code\02_Modeling“ des Workbench-Projekts wird verwendet, um einen Batch AI-Trainingsauftrag auszugeben. Dieser Auftrag trainiert eine vom Benutzer ausgewählte Bildklassifizierungs-DNN erneut (AlexNet oder ResNet 18, die vorab mit ImageNet trainiert wurden). Die Tiefe des erneuten Trainings kann ebenfalls angegeben werden: Das erneute Trainieren der letzten Ebene des Netzwerks kann eine Überanpassung reduzieren, wenn wenige Trainingsbeispiele verfügbar sind, während die Feinabstimmung des gesamten Netzwerks (oder der vollständig verbundenen Ebenen für AlexNet) zu einer höheren Leistung des Modells führen kann, wenn der Trainingssatz ausreichend groß ist.

Wenn der Trainingsauftrag abgeschlossen ist, speichert dieses Skript das Modell (zusammen mit einer Datei, die die Zuordnung der Ausgaben von ganzen Zahlen und der Zeichenfolgenbezeichnungen des Modells beschreibt) und die Vorhersagen in Blob Storage. Die Protokolldatei der BAIT-Aufträge wird analysiert, um den zeitlichen Verlauf der Verbesserung der Fehlerrate während der Trainingsepochen zu extrahieren. Der zeitliche Verlauf der Verbesserung der Fehlerrate wird für die spätere Ansicht im Ausführungsverlauf von Azure Machine Learning Workbench protokolliert.

Wählen Sie einen Namen für Ihr trainiertes Modell aus, einen vorab trainierten Modelltyp und die Tiefe des erneuten Trainings. Fügen Sie Ihre Auswahl wie angegeben in den folgenden Befehl ein, und beginnen Sie dann mit dem erneuten Training, indem Sie den folgenden Befehl in einer Azure ML-Befehlszeilenschnittstelle ausführen:

```
az ml experiment submit -c local Code\02_Modeling\run_batch_ai.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --retraining_type {last_only,fully_connected,all} --num_epochs 10
```

Die Ausführung von Azure Machine Learning nimmt etwa eine halbe Stunde in Anspruch. Es wird empfohlen, einige ähnliche Befehle auszuführen (wodurch die Ausgabe des Modellnamens, des vorab trainierten Modelltyps und die Tiefe des erneuten Trainings geändert wird), um die Leistung der Modelle zu vergleichen, die mit unterschiedlichen Methoden trainiert wurden.

### <a name="training-models-with-mmlspark"></a>Trainieren von Modellen mit MMLSpark

Das Skript `run_mmlspark.py` im Unterordner „Code\02_Modeling“ des Workbench-Projekts dient zum Trainieren eines [MMLSpark](https://github.com/Azure/mmlspark)-Modells für die Bildklassifizierung. Das erste Skript versieht die Bilder der Trainingssammlung mithilfe eines Bildklassifizierungs-DNN, das mit dem ImageNet-Dataset (AlexNet oder ein ResNet mit 18 Schichten) vorab trainiert wurde, mit Funktionen. Das Skript verwendet dann die mit Funktionen versehenen Bilder zum Trainieren eines MMLSpark-Modells (einer zufälligen Gesamtstruktur oder eines logistischen Regressionsmodells), um die Bilder zu klassifizieren. Die Testbildsammlung wird dann mit Funktionen versehen und mit dem trainierten Modell bewertet. Die Genauigkeit der Vorhersagen des Modells für die Testsammlung wird berechnet und im Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench protokolliert. Schließlich werden das trainierte MMLSpark-Modell und seine Vorhersagen für die Testsammlung im Blobspeicher gespeichert.

Wählen Sie einen eindeutigen Ausgabemodellnamen für das trainierte Modell, einen vortrainierten Modelltyp und einen MMLSpark-Modelltyp aus. Fügen Sie Ihre Auswahl wie angegeben in die folgenden Befehlsvorlage ein, und beginnen Sie dann mit dem erneuten Training, indem Sie den folgenden Befehl in einer Azure ML-Befehlszeilenschnittstelle ausführen:

```
az ml experiment submit -c myhdi Code\02_Modeling\run_mmlspark.py --config_filename Code/settings.cfg --output_model_name [unique model name, alphanumeric characters only] --pretrained_model_type {alexnet,resnet18} --mmlspark_model_type {randomforest,logisticregression}
```

Ein zusätzlicher Parameter `--sample_frac` kann zum Trainieren und Testen des Modells mit einer Teilmenge der verfügbaren Daten verwendet werden. Mit einem kleinen Beispielbruchteil verringern sich die Laufzeit- und Arbeitsspeicheranforderungen zu Lasten der Genauigkeit des trainierten Modells. (Eine Ausführung mit `--sample_frac 0.1` nimmt beispielsweise etwa 20 Minuten in Anspruch.) Wenn Sie weitere Informationen zu diesem und anderen Parametern benötigen, führen Sie `python Code\02_Modeling\run_mmlspark.py -h` aus.

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

Mit Azure Machine Learning Workbench können Datenanalysten ihren Code auf einfache Weise auf Remotecomputezielen bereitstellen. In diesem Beispiel wurde der Code für das lokale MMLSpark-Training für die Remoteausführung auf einem HDInsight-Cluster bereitgestellt, und ein lokales Skript hat einen Trainingsauftrag auf einem Azure Batch AI-GPU-Cluster gestartet. Über das Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench wurde die Leistung mehrerer Modelle nachverfolgt und das präziseste Modell identifiziert. Das Feature „Jupyter Notebooks“ von Workbench war bei der Visualisierung der Vorhersagen unserer Modelle in einer interaktiven, grafischen Umgebung hilfreich.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie sich ausführlicher mit diesem Beispiel beschäftigen möchten:
- Klicken Sie im Feature „Ausführungsverlauf“ von Azure Machine Learning Workbench auf die Zahnradsymbole, um die anzuzeigenden Diagramme und Metriken auszuwählen.
- Untersuchen Sie die Beispielskripts für Anweisungen, die `run_logger` aufrufen. Stellen Sie sicher, dass Sie verstehen, wie jede Metrik aufgezeichnet wird.
- Untersuchen Sie die Beispielskripts für Anweisungen, die `blob_service` aufrufen. Stellen Sie sicher, dass Sie verstehen, wie trainierte Modelle und Vorhersagen in der Cloud gespeichert und aus ihr abgerufen werden.
- Untersuchen Sie den Inhalt der Container, die in Ihrem Blobspeicherkonto erstellt werden. Stellen Sie sicher, dass Sie verstehen, welches Skript oder welcher Befehl für das Erstellen jeder Gruppe von Dateien verantwortlich ist.
- Ändern Sie das Trainingsskript so,dass ein anderer Typ von MMLSpark-Modell trainiert wird, oder ändern Sie die Modellhyperparameter. Verwenden Sie das Feature „Ausführungsverlauf“, um zu ermitteln, ob Ihre Änderungen die Genauigkeit des Modells erhöht oder verringert haben.

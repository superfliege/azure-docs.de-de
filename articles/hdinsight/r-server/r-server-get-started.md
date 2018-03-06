---
title: "Erste Schritte mit R Server in HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein Apache Spark-System in einem HDInsight-Cluster mit R Server erstellen und ein R-Skript im Cluster übermitteln."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: aa7f2e6f44036738756391ecaa265c57c093c42c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="get-started-with-r-server-on-hdinsight"></a>Erste Schritte mit R Server in HDInsight

Azure HDInsight umfasst eine R Server-Option für die Integration in Ihren HDInsight-Cluster. Mit dieser Option können R-Skripts Spark und MapReduce verwenden, um verteilte Berechnungen auszuführen. In diesem Artikel erfahren Sie, wie Sie im HDInsight-Cluster eine R Server-Instanz erstellen. Anschließend wird beschrieben, wie Sie ein R-Skript ausführen, mit dem die Verwendung von Spark für verteilte R-Berechnungen veranschaulicht wird.


## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**: Bevor Sie mit diesem Tutorial beginnen, müssen Sie über ein Azure-Abonnement verfügen. Weitere Informationen finden Sie unter [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Beziehen der kostenlosen Testversion von Microsoft Azure).
* **Ein Secure Shell-Client (SSH)**: Ein SSH-Client wird verwendet, um Remoteverbindungen mit dem HDInsight-Cluster herzustellen und Befehle direkt im Cluster auszuführen. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).
* **SSH-Schlüssel (optional)**: Sie können das SSH-Konto für die Verbindungsherstellung mit dem Cluster mit einem Kennwort oder einem öffentlichen Schlüssel schützen. Die Verwendung eines Kennworts ist einfacher und ermöglicht Ihnen den Einstieg, ohne ein Paar mit einem öffentlichen und privaten Schlüssel zu erstellen. Die Verwendung eines Schlüssels ist jedoch sicherer.

  > [!NOTE]
  > Für die Schritte in diesem Artikel wird davon ausgegangen, dass Sie ein Kennwort verwenden.


## <a name="automate-cluster-creation"></a>Automatisieren der Clustererstellung

Sie können die Erstellung von HDInsight R Server-Instanzen automatisieren, indem Sie Azure Resource Manager-Vorlagen, das SDK und PowerShell verwenden.

* Informationen zur Erstellung einer R Server-Instanz mit einer Azure Resource Manager-Vorlage finden Sie unter [Deploy an R Server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Bereitstellen eines R Server-HDInsight-Clusters).
* Informationen zur Erstellung einer R Server-Instanz mit dem .NET SDK finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit dem .NET SDK](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).
* Informationen zur Bereitstellung von R Server mit PowerShell finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-a-cluster-by-using-the-azure-portal"></a>Erstellen eines Clusters mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Neu** > **Intelligence und Analyse** > **HDInsight**.

    ![Bild der Erstellung eines neuen Clusters](./media/r-server-get-started/newcluster.png)

3. Geben Sie auf der Oberfläche für die **Schnellerfassung** im Feld **Clustername** einen Namen für den Cluster ein. Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie über das Feld **Abonnement** das gewünschte Abonnement auswählen.

    ![Auswahl von Clustername und Abonnement](./media/r-server-get-started/clustername.png)

4. Wählen Sie **Clustertyp**, um den Bereich **Clusterkonfiguration** zu öffnen. Wählen Sie im Bereich **Clusterkonfiguration** die folgenden Optionen aus:

    * **Clustertyp**: Wählen Sie **R Server**.
    * **Version**: Wählen Sie die Version von R Server, die Sie im Cluster installieren möchten. Die derzeit verfügbare Version ist **R Server 9.1 (HDI 3.6)**. Versionshinweise zu den verfügbaren Versionen von R Server finden Sie unter [docs.microsoft.com](https://docs.microsoft.com/machine-learning-server/whats-new-in-r-server#r-server-91).
    * **RStudio Community Edition für R Server**: Diese browserbasierte IDE wird standardmäßig auf dem Edgeknoten installiert. Sie können das Kontrollkästchen auch deaktivieren, falls Sie die Installation nicht wünschen. Wenn Sie eine Installation durchführen, finden Sie die URL für den Zugriff auf die RStudio Server-Anmeldung in einem Bereich der Portalanwendung für Ihren Cluster, nachdem dieser erstellt wurde.
    * Behalten Sie für die anderen Optionen die Standardwerte bei, und verwenden Sie die Schaltfläche **Auswählen**, um den Clustertyp zu speichern.

        ![Screenshot: Bereich „Clustertyp“](./media/r-server-get-started/clustertypeconfig.png)

5. Geben Sie im Bereich **Grundlagen** in den Feldern **Benutzername für Clusteranmeldung** und **Kennwort für Clusteranmeldung** einen Benutzernamen bzw. ein Kennwort für den Cluster ein.

6. Geben Sie im Feld **SSH-Benutzername (Secure Shell)** den SSH-Benutzernamen an. SSH wird verwendet, um per SSH-Client eine Remoteverbindung mit dem Cluster herzustellen. Sie können den SSH-Benutzer in diesem Feld oder nach der Erstellung des Clusters eingeben (auf der Registerkarte **Konfiguration** für den Cluster).
   
   > [!NOTE] 
   > R Server ist so konfiguriert, dass „remoteuser“ als SSH-Benutzername erwartet wird. Wenn Sie einen anderen Benutzernamen auswählen, müssen Sie nach dem Erstellen des Clusters einen zusätzlichen Schritt ausführen.

7. Lassen Sie das Kontrollkästchen **Dasselbe Kennwort wie für die Clusteranmeldung verwenden** aktiviert, um **KENNWORT** als Authentifizierungstyp zu verwenden, sofern Sie nicht die Verwendung eines öffentlichen Schlüssels bevorzugen. Sie benötigen ein Paar aus einem öffentlichen und privaten Schlüssel, um über einen Remoteclient, z.B. R Tools für Visual Studio, RStudio oder eine andere Desktop-IDE, auf R Server im Cluster zuzugreifen. Wenn Sie die Community Edition von RStudio Server installieren, müssen Sie ein SSH-Kennwort auswählen.     

   Deaktivieren Sie die Option **Dasselbe Kennwort wie für die Clusteranmeldung verwenden**, um ein Paar aus einem öffentlichen und privaten Schlüssel zu erstellen und zu verwenden. Wählen Sie anschließend die Option **ÖFFENTLICHER SCHLÜSSEL**, und fahren Sie wie hier angegeben fort. Diese Anweisungen setzen voraus, dass Cygwin mit ssh-keygen o.ä. installiert ist.

   a. Generieren Sie auf Ihrem Laptop über die Befehlszeile ein Paar mit einem öffentlichen und privaten Schlüssel:

        ssh-keygen -t rsa -b 2048

   b. Befolgen Sie die Aufforderung zum Benennen einer Schlüsseldatei, und geben Sie dann eine Passphrase zur Erhöhung der Sicherheit ein. Ihr Bildschirm sollte etwa folgendermaßen aussehen:

      ![SSH-Befehlszeile in Windows](./media/r-server-get-started/sshcmdline.png)

      Mit diesem Befehl wird eine Datei mit einem privaten Schlüssel und eine Datei mit einem öffentlichen Schlüssel mit dem Namen „<Dateiname_privater_Schlüssel>.pub“ erstellt. In diesem Beispiel haben diese Dateien die Namen „furiosa“ und „furiosa.pub“:

      ![Beispielergebnis für den Befehl „dir“](./media/r-server-get-started/dir.png)

   c. Geben Sie die Datei mit dem öffentlichen Schlüssel (&#42;.pub) beim Zuweisen von Anmeldeinformationen für den HDI-Cluster an. Bestätigen Sie die Ressourcengruppe und die Region, und wählen Sie **Weiter**.

      ![Bereich für Anmeldeinformationen](./media/r-server-get-started/publickeyfile.png)  

   d. Ändern von Berechtigungen für die Datei mit dem privaten Schlüssel auf Ihrem Laptop:

        chmod 600 <private-key-filename>

   e. Verwenden der Datei mit dem privaten Schlüssel mit SSH für die Remoteanmeldung:

        ssh –i <private-key-filename> remoteuser@<hostname public ip>

      Oder verwenden Sie die Datei mit dem privaten Schlüssel als Teil der Definition Ihres Rechenkontexts in Hadoop Spark für R Server auf dem Client. Weitere Informationen finden Sie unter [Create a Compute Context for Spark](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark) (Erstellen eines Rechenkontexts für Spark).

8. Über die Schnellerfassung gelangen Sie zum Bereich **Speicher**. Wählen Sie darin die Einstellungen für das Speicherkonto aus, die für den primären Standort des HDFS-Dateisystems, das vom Cluster genutzt wird, verwendet werden sollen. Wählen Sie ein neues oder vorhandenes Azure-Speicherkonto oder ein vorhandenes Azure Data Lake Store-Konto aus.

    - Wenn Sie ein Azure-Speicherkonto auswählen, können Sie ein vorhandenes Konto angeben, indem Sie auf **Speicherkonto auswählen** klicken und dann das entsprechende Konto auswählen. Erstellen Sie ein neues Konto über den Link **Neu erstellen** im Abschnitt **Speicherkonto auswählen**.

      > [!NOTE]
      > Wenn Sie **Neu** auswählen, müssen Sie einen Namen für das neue Speicherkonto eingeben. Wenn der Name akzeptiert wurde, wird ein grünes Häkchen angezeigt.

      Als **Standardcontainer** wird standardmäßig der Name des Clusters verwendet. Ändern Sie diesen Standardwert nicht.

      Wenn Sie ein neues Speicherkonto ausgewählt haben, können Sie in der Eingabeaufforderung **Standort** wählen, um eine dazugehörige Region auszuwählen.  

         ![Bereich „Datenquelle“](./media/r-server-get-started/datastore.png)  

      > [!IMPORTANT]
      > Durch die Auswahl des Standorts für die Standarddatenquelle wird auch der Standort des HDInsight-Clusters festgelegt. Der Cluster und die Standarddatenquelle müssen sich in derselben Region befinden.

    - Wählen Sie das gewünschte Konto aus, wenn Sie ein vorhandenes Data Lake Store-Konto verwenden möchten. Fügen Sie Ihrem Cluster anschließend die Clusteridentität *ADD* hinzu, um den Zugriff auf den Speicher zuzulassen. Weitere Informationen zu diesem Prozess finden Sie unter [Erstellen von HDInsight-Clustern mit Azure Data Lake Store mithilfe des Azure-Portals](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

    Verwenden Sie die Schaltfläche **Auswählen** , um die Datenquellenkonfiguration zu speichern.


9. Anschließend wird der Bereich **Zusammenfassung** angezeigt, damit Sie Ihre gesamten Einstellungen überprüfen können. Hier können Sie Ihre Clustergröße ändern, um die Anzahl von Servern in Ihrem Cluster zu ändern. Sie können auch Skriptaktionen angeben, die Sie ausführen möchten. Wenn Sie wissen, dass Sie keinen größeren Cluster benötigen, können Sie für die Anzahl von Workerknoten den Standardwert **4** beibehalten. In diesem Bereich werden auch die geschätzten Kosten des Clusters angezeigt.

    ![Clusterzusammenfassung](./media/r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Bei Bedarf können Sie im Portal die Größe Ihres Clusters später ändern (**Cluster** > **Einstellungen** > **Cluster skalieren**), um die Anzahl von Workerknoten zu erhöhen oder zu verringern. Diese Größenänderung kann zum Herunterfahren des Clusters nützlich sein, wenn er nicht verwendet wird, oder zum Hinzufügen von Kapazität zur Erfüllung der Anforderungen größerer Aufgaben.
   >
   >

   Berücksichtigen Sie beim Ändern der Größe Ihres Clusters, der Datenknoten und der Edgeknoten die folgenden Faktoren:  

   * Die Leistung von verteilten R Server-Analysen in Spark ist proportional zur Anzahl der Workerknoten, wenn die Datenmenge groß ist.  

   * Die Leistung von R Server-Analysen ist linear zur Größe der analysierten Daten. Beispiel:   

     * Bei kleinen bis mittelgroßen Datenmengen ist die Leistung am besten, wenn die Daten in einem lokalen Rechenkontext auf dem Edgeknoten analysiert werden. Weitere Informationen zu den Szenarien, in denen der lokale und der Spark-Rechenkontext am besten funktionieren, finden Sie unter [Rechenkontextoptionen für R Server in HDInsight](r-server-compute-contexts.md).<br>
     * Wenn Sie sich beim Edgeknoten anmelden und Ihr R-Skript ausführen, werden alle Funktionen bis auf die ScaleR-rx-Funktionen *lokal* im Edgeknoten ausgeführt. Die Größe des Speichers und die Anzahl von Kernen auf dem Edgeknoten sollten daher mit einer entsprechenden Größe festgelegt werden. Dasselbe gilt, wenn Sie R Server in HDI als Remoterechenkontext auf Ihrem Laptop verwenden.

   Klicken Sie auf die Schaltfläche **Auswählen** , um die Konfiguration der Knotenpreise zu speichern.

   ![Bereich für Knotentarife](./media/r-server-get-started/pricingtier.png)

   Es ist auch ein Link zum **Herunterladen von Vorlage und Parametern** vorhanden. Wählen Sie diesen Link, um Skripts anzuzeigen, mit denen Sie die Erstellung eines Clusters mit der ausgewählten Konfiguration automatisieren können. Diese Skripts sind auch über den Azure-Portal-Eintrag Ihres Clusters verfügbar, nachdem dieser erstellt wurde.

   > [!NOTE]
   > Die Erstellung des Clusters dauert in der Regel ca. 20 Minuten. Sie können den Status des Erstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen** überprüfen.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Herstellen einer Verbindung mit RStudio Server

Wenn Sie sich für die Einbeziehung der RStudio Server Community Edition in Ihre Installation entschieden haben, können Sie mit zwei Methoden auf die RStudio-Anmeldung zugreifen:

- Verwenden Sie die folgende URL (bei der *CLUSTERNAME* für den Namen des erstellten Clusters steht):

    https://*CLUSTERNAME*.azurehdinsight.net/rstudio/

- Öffnen Sie den Eintrag für Ihren Cluster im Azure-Portal, rufen Sie den Quicklink für die **R Server-Dashboards** auf, und wählen Sie dann das **R Studio-Dashboard** aus:

  ![Zugreifen auf das RStudio-Dashboard](./media/r-server-get-started/rstudiodashboard1.png)

  ![Zugreifen auf das RStudio-Dashboard](./media/r-server-get-started/rstudiodashboard2.png)

> [!IMPORTANT]
> Unabhängig von der verwendeten Zugriffsmethode müssen Sie sich bei der ersten Anmeldung zweimal authentifizieren. Geben Sie bei der ersten Authentifizierung die Benutzer-ID und das Kennwort für den Clusteradministrator an. Geben Sie bei der zweiten Aufforderung die Benutzer-ID und das Kennwort für SSH an. Für die nachfolgenden Anmeldungen sind dann nur die Benutzer-ID und das Kennwort für SSH erforderlich.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Herstellen einer Verbindung mit dem R Server-Edgeknoten

Stellen Sie eine Verbindung mit dem R Server-Edgeknoten des HDInsight-Clusters her, indem Sie SSH mit diesem Befehl verwenden:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Sie finden die Adresse `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` im Azure-Portal. Wählen Sie Ihren Cluster und dann die Option **Alle Einstellungen** > **Apps** > **RServer** aus. Hierdurch werden die SSH-Endpunktinformationen für den Edgeknoten angezeigt.
>
> ![Abbildung mit dem SSH-Endpunkt für den Edgeknoten](./media/r-server-get-started/sshendpoint.png)
>
>

Wenn Sie zum Schützen Ihres SSH-Benutzerkontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie unter Umständen den Parameter `-i` nutzen, um den passenden privaten Schlüssel anzugeben. Beispiel: 

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

Nachdem die Verbindung hergestellt wurde, wird in etwa die folgende Eingabeaufforderung angezeigt:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Aktivieren mehrerer gleichzeitiger Benutzer

Sie können mehrere gleichzeitige Benutzern aktivieren, indem Sie mehr Benutzer für den Edgeknoten hinzufügen, auf dem die RStudio Community-Version ausgeführt wird.

Beim Erstellen eines HDInsight-Clusters müssen Sie zwei Benutzer angeben: einen HTTP-Benutzer und einen SSH-Benutzer.

![Eingeben von Anmeldeinformationen für den Clusterbenutzer und SSH-Benutzer](./media/r-server-get-started/concurrent-users-1.png)

- **Benutzername für Clusteranmeldung**: Ein HTTP-Benutzer für die Authentifizierung über das HDInsight-Gateway, das zum Schutz der von Ihnen erstellten HDInsight-Cluster verwendet wird. Mit diesem HTTP-Benutzer wird auf die Ambari- und die YARN-Benutzeroberfläche sowie auf andere UI-Komponenten zugegriffen.
- **SSH-Benutzername (Secure Shell)**: Ein SSH-Benutzer für den Zugriff auf den Cluster über Secure Shell. Dieser Benutzer ist ein Benutzer im Linux-System für alle Hauptknoten, Workerknoten und Edgeknoten. Somit können Sie SSH zum Zugreifen auf einen beliebigen Knoten in einem Remotecluster verwenden.

Die RStudio Server Community-Version, die im Cluster vom Typ „Microsoft R Server in HDInsight“ verwendet wird, akzeptiert für die Anmeldung nur den Linux-Benutzernamen und das dazugehörige Kennwort. Die Übergabe von Token wird nicht unterstützt. Wenn Sie einen neuen Cluster erstellt haben und RStudio für den Zugriff darauf verwenden möchten, müssen Sie sich zweimal anmelden.

1. Melden Sie sich mit den HTTP-Benutzeranmeldeinformationen über das HDInsight-Gateway an: 

    ![Eingeben von HTTP-Benutzeranmeldeinformationen](./media/r-server-get-started/concurrent-users-2a.png)

2. Verwenden Sie die SSH-Benutzeranmeldeinformationen für die Anmeldung an RStudio:
  
    ![Eingeben von SSH-Benutzeranmeldeinformationen](./media/r-server-get-started/concurrent-users-2b.png)

Derzeit können Sie bei der Bereitstellung eines HDInsight-Clusters nur ein SSH-Benutzerkonto erstellen. Um mehreren Benutzern den Zugriff auf Microsoft R Server in HDInsight-Clustern zu ermöglichen, müssen Sie im Linux-System zusätzliche Benutzer erstellen.

Da die RStudio Server Community-Version auf dem Edgeknoten des Clusters ausgeführt wird, müssen hier drei Schritte ausgeführt werden:

1. Anmelden beim Edgeknoten mit dem erstellten SSH-Benutzer
2. Hinzufügen weiterer Linux-Benutzer zum Edgeknoten
3. Verwenden der RStudio Community-Version mit dem erstellten Benutzer

### <a name="use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Anmelden beim Edgeknoten mit dem erstellten SSH-Benutzer

Laden Sie ein SSH-Tool (z.B. PuTTY) herunter, und verwenden Sie den vorhandenen SSH-Benutzer für die Anmeldung. Befolgen Sie dann die Anweisungen unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md), um auf den Edgeknoten zuzugreifen. Die Edgeknotenadresse für die R Server-Instanz im HDInsight-Cluster lautet **clustername-ed-ssh.azurehdinsight.net**.


### <a name="add-more-linux-users-to-the-edge-node"></a>Hinzufügen weiterer Linux-Benutzer zum Edgeknoten

Führen Sie diese Befehle aus, um einen Benutzer dem Edgeknoten hinzuzufügen:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Daraufhin sollte Folgendes zurückgegeben werden: 

![Ausgabe von sudo-Befehlen](./media/r-server-get-started/concurrent-users-3.png)

Wenn Sie zum Eingeben des aktuellen Kerberos-Kennworts aufgefordert werden, können Sie dies ignorieren, indem Sie einfach die EINGABETASTE drücken. Mit der Option `-m` des Befehls `useradd` wird angegeben, dass das System einen Basisordner für den Benutzer erstellt. Dieser Ordner ist für die RStudio Community-Version erforderlich.


### <a name="use-the-rstudio-community-version-with-the-created-user"></a>Verwenden der RStudio Community-Version mit dem erstellten Benutzer

Verwenden Sie den erstellten Benutzer zum Anmelden in RStudio:

![Anmeldeseite für RStudio](./media/r-server-get-started/concurrent-users-4.png)

Beachten Sie die Angabe in RStudio, dass Sie den neuen Benutzer (in diesem Fall beispielsweise **sshuser6**) für die Anmeldung am Cluster verwenden: 

![Angabe des neuen Benutzers in der Befehlsleiste von RStudio](./media/r-server-get-started/concurrent-users-5.png)

Sie können sich auch gleichzeitig über ein anderes Browserfenster mit den ursprünglichen Anmeldeinformationen (standardmäßig **sshuser**) anmelden.

Sie können einen Auftrag mithilfe von ScaleR-Funktionen senden. Hier ist ein Beispiel für die Befehle zum Ausführen eines Auftrags angegeben:

    # Set the HDFS (Azure Blob storage) location of example data
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storing data temporarily
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set the directory in bigDataDirRoot to load the data
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (Blob storage) file system
    hdfsFS <- RxHdfsFileSystem()

    # Create an info list for the airline data
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in the local system
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context
    mySparkCluster <- RxSpark()

    # Set the compute context
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary
    summary(modelSpark)


Beachten Sie, dass die übermittelten Aufträge auf der YARN-Benutzeroberfläche unter anderen Benutzernamen angezeigt werden:

![Liste mit den Benutzern auf der YARN-Benutzeroberfläche](./media/r-server-get-started/concurrent-users-6.png)

Beachten Sie außerdem, dass die neu hinzugefügten Benutzer nicht über Stammberechtigungen im Linux-System verfügen. Es gelten aber die gleichen Zugriffsberechtigungen auf alle Dateien im HDFS-Remotedateisystem und Blobspeicher.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>Verwenden der R-Konsole

1. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um die R-Konsole zu starten:  

        R

2. Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:
    
        R version 3.2.2 (2015-08-14) -- "Fire Safety"
        Copyright (C) 2015 The R Foundation for Statistical Computing
        Platform: x86_64-pc-linux-gnu (64-bit)

        R is free software and comes with ABSOLUTELY NO WARRANTY.
        You are welcome to redistribute it under certain conditions.
        Type 'license()' or 'licence()' for distribution details.

        Natural language support but running in an English locale

        R is a collaborative project with many contributors.
        Type 'contributors()' for more information and
        'citation()' on how to cite R or R packages in publications.

        Type 'demo()' for some demos, 'help()' for on-line help, or
        'help.start()' for an HTML browser interface to help.
        Type 'q()' to quit R.

        Microsoft R Server version 8.0: an enhanced distribution of R
        Microsoft packages Copyright (C) 2016 Microsoft Corporation

        Type 'readme()' for release notes.
        >

3. An der `>` -Eingabeaufforderung können Sie R-Code eingeben. Zu R Server gehören Pakete, mit denen Sie auf einfache Weise mit Hadoop interagieren und verteilte Berechnungen durchführen können. Verwenden Sie beispielsweise den folgenden Befehl, um das Stammverzeichnis des Standarddateisystems für den HDInsight-Cluster anzuzeigen:

        rxHadoopListFiles("/")

4. Sie können für die Adressierung auch den Blobspeicherstil verwenden:

        rxHadoopListFiles("wasb:///")


## <a name="use-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Verwenden von R Server in HDI in einer Remoteinstanz von Microsoft R Server oder Microsoft R Client

Es ist möglich, den Zugriff auf den HDI Hadoop Spark-Rechenkontext von einer auf einem Desktop- oder Laptopcomputer ausgeführten Remoteinstanz von Microsoft R Server oder Microsoft R Client einzurichten. Weitere Informationen finden Sie im Abschnitt „Using Microsoft R Server as a Hadoop Client“ (Verwenden von Microsoft R Server als Hadoop-Client) unter [Create a Compute Context for Spark](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios) (Erstellen eines Rechenkontexts für Spark). Geben Sie hierfür beim Definieren des RxSpark-Rechenkontexts auf Ihrem Laptop die folgenden Optionen an: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches und sshProfileScript. Hier sehen Sie ein Beispiel:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>Verwenden eines Rechenkontexts

Mit einem Rechenkontext können Sie steuern, ob die Berechnung lokal auf dem Edgeknoten ausgeführt oder auf die Knoten im HDInsight-Cluster verteilt wird.

1. Verwenden Sie über RStudio Server oder die R-Konsole (in einer SSH-Sitzung) den folgenden Code, um Beispieldaten in den Standardspeicher für HDInsight zu laden:

        # Set the HDFS (Blob storage) location of example data
        bigDataDirRoot <- "/example/data"

        # Create a local folder for storing data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set the directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Erstellen Sie einige Dateninformationen, und definieren Sie zwei Datenquellen, damit Sie mit den Daten arbeiten können:

        # Define the HDFS (Blob storage) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create an info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # Get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in HDFS
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in the local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # Formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Führen Sie mit dem lokalen Rechenkontext eine logistische Regression für die Daten aus:

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Es sollte eine Ausgabe, die mit Zeilen wie den folgenden endet, angezeigt werden:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Führen Sie die gleiche logistische Regression mit dem Spark-Kontext aus. Im Spark-Kontext wird die Verarbeitung auf alle Workerknoten im HDInsight-Cluster verteilt.

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Sie können auch MapReduce verwenden, um die Berechnung auf Clusterknoten zu verteilen. Weitere Informationen zum Rechenkontext finden Sie unter [Rechenkontextoptionen für R Server in HDInsight](r-server-compute-contexts.md).


## <a name="distribute-r-code-to-multiple-nodes"></a>Verteilen von R-Code auf mehrere Knoten

Mit R Server können Sie mithilfe von `rxExec` vorhandenen R-Code einfach nutzen und auf mehreren Knoten im Cluster ausführen. Diese Funktion ist nützlich für Parameterbereinigungen oder für Simulationen. Im folgenden Code wird die Verwendung von `rxExec` veranschaulicht:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Wenn Sie noch den Spark- oder MapReduce-Kontext verwenden, gibt dieser Befehl den `nodename`-Wert für die Workerknoten zurück, auf denen der Code `(Sys.info()["nodename"])` ausgeführt wird. In einem Cluster mit vier Knoten erwarten Sie beispielsweise in etwa folgende Ausgabe:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="access-data-in-hive-and-parquet"></a>Zugreifen auf Daten in Hive und Parquet

Ein Feature in R Server 9.1 bietet direkten Zugriff auf Daten in Hive und Parquet, um diese für ScaleR-Funktionen im Spark-Rechenkontext zu verwenden. Diese Funktionen sind über die neuen ScaleR-Datenquellenfunktionen mit den Bezeichnungen „RxHiveData“ und „RxParquetData“ verfügbar. Sie funktionieren aufgrund der Verwendung von Spark SQL zum direkten Laden von Daten in einen Spark DataFrame zur Analyse durch ScaleR.  

Der folgende Code enthält einige Beispiele für die Verwendung der neuen Funktionen:

    #Create a Spark compute context
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, clean up, and close the Spark session
    lsObj <- rxSparkListData() #Two data objects are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() #It should show an empty list
    rxSparkDisconnect(myHadoopCluster)


Weitere Informationen zu diesen neuen Funktionen finden Sie in der Onlinehilfe von R Server, indem Sie die Befehle `?RxHivedata` und `?RxParquetData` verwenden.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Installieren zusätzlicher R-Pakete auf dem Edgeknoten

Falls Sie zusätzliche R-Pakete auf dem Edgeknoten installieren möchten, können Sie `install.packages()` direkt in der R-Konsole verwenden, wenn Sie über SSH eine Verbindung mit dem Edgeknoten hergestellt haben. Wenn Sie jedoch R-Pakete auf den Workerknoten des Clusters installieren möchten, müssen Sie eine Skriptaktion verwenden.

Skriptaktionen sind Bash-Skripts, mit denen Konfigurationsänderungen am HDInsight-Cluster vorgenommen werden oder zusätzliche Software (etwa zusätzliche R-Pakete) installiert wird. Gehen Sie wie folgt vor, um zusätzliche Pakete mit einer Skriptaktion zu installieren:

> [!IMPORTANT]
> Sie können Skriptaktionen erst nach der Erstellung des Clusters verwenden, um weitere R-Pakete zu installieren. Verwenden Sie dieses Verfahren nicht bei der Clustererstellung, da für das Skript R Server vollständig installiert und konfiguriert sein muss.
>
>

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren R Server im HDInsight-Cluster aus.

2. Wählen Sie im Bereich **Einstellungen** die Option **Skriptaktionen** > **Neue übermitteln**.

   ![Abbildung des Bereichs „Skriptaktionen“](./media/r-server-get-started/scriptaction.png)

3. Geben Sie im Bereich **Skriptaktion übermitteln** die folgenden Informationen an:

   * **Name**: Ein Anzeigename zum Identifizieren dieses Skripts.

   * **Bash-Skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Hauptknoten**: Diese Option muss deaktiviert sein.

   * **Worker**: Diese Option muss deaktiviert sein.

   * **Zookeeper**: Diese Option muss deaktiviert sein.

   * **Edgeknoten**: Diese Option muss aktiviert sein.

   * **Parameter**: Die zu installierenden R-Pakete, z.B. `bitops stringr arules`.

   * **Persist this script** (Skript speichern): Diese Option muss aktiviert sein.  

   > [!NOTE]
   > Standardmäßig werden alle R-Pakete über eine Momentaufnahme des Microsoft R Application Network-Repositorys erstellt, die der installierten Version von R Server entspricht. Wenn Sie neuere Versionen der Pakete installieren möchten, besteht das Risiko von Inkompatibilitäten. Diese Art von Installation ist aber möglich, wenn Sie `useCRAN` als erstes Element der Paketliste angeben, z.B. `useCRAN bitops, stringr, arules`.  
   > 
   > Für einige R-Pakete werden zusätzliche Linux-Systembibliotheken benötigt. Um Ihnen die Arbeit zu erleichtern, haben wir die abhängigen Komponenten für die 100 beliebtesten R-Pakete für Sie vorinstalliert. Wenn für die R-Pakete, die Sie installieren, weitere Bibliotheken hinzugefügt werden müssen, ist es erforderlich, dass Sie das hier verwendete Basisskript herunterladen und Schritte zum Installieren der Systembibliotheken hinzufügen. Anschließend müssen Sie das geänderte Skript in einen öffentlichen Blobcontainer in Azure Storage hochladen und das geänderte Skript zum Installieren der Pakete verwenden.
   >
   > Weitere Informationen zum Entwickeln von Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen](../hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Hinzufügen einer Skriptaktion](./media/r-server-get-started/submitscriptaction.png)

4. Wählen Sie **Erstellen** aus, um das Skript auszuführen. Nach Abschluss der Skriptausführung sind die R-Pakete auf allen Workerknoten verfügbar.


## <a name="configure-microsoft-r-server-operationalization"></a>Konfigurieren der Microsoft R Server-Operationalisierung

Nach Abschluss der Datenmodellierung können Sie das Modell operationalisieren, um Vorhersagen zu treffen. Führen Sie zum Konfigurieren der Microsoft R Server-Operationalisierung die folgenden Schritte aus:

1. Verwenden Sie den Befehl `ssh` für den Edgeknoten, z.B.: 

       ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

2. Ändern Sie das Verzeichnis für die relevante Version, und verwenden Sie den Befehl `sudo dotnet` für die DLL-Datei. 

   Für Microsoft R Server 9.1:

       cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
       sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

   Für Microsoft R Server 9.0:

       cd /usr/lib64/microsoft-deployr/9.0.1
       sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Gehen Sie zum Konfigurieren der Microsoft R Server-Operationalisierung mit einem einzelnen Computer wie folgt vor:

   a. Wählen Sie `Configure R Server for Operationalization` aus.

   b. Wählen Sie `A. One-box (web + compute nodes)` aus.

   c. Geben Sie ein Kennwort für den Benutzer `admin` ein.

   ![One-Box-Operationalisierung](./media/r-server-get-started/admin-util-one-box-.png)

4. Optional können Sie wie folgt einen Diagnosetest durchführen:

   a. Wählen Sie `6. Run diagnostic tests` aus.

   b. Wählen Sie `A. Test configuration` aus.

   c. Geben Sie `admin` als Benutzername und das Kennwort aus dem vorherigen Konfigurationsschritt ein.

   d. Bestätigen Sie Folgendes: `Overall Health = pass`.

   e. Beenden Sie das Verwaltungshilfsprogramm.

   f. Beenden Sie SSH.

   ![Diagnose für Operationalisierung](./media/r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>Falls bei der Nutzung eines Webdiensts, der mit mrsdeploy-Funktionen in einem Spark-Rechenkontext erstellt wurde, lange Verzögerungen auftreten, müssen Sie unter Umständen einige fehlende Ordner hinzufügen. Wenn die Spark-Anwendung von einem Webdienst mit mrsdeploy-Funktionen aufgerufen wird, gehört sie zu einem Benutzer namens *rserve2*. So können Sie dieses Problem umgehen:

    #Create these required folders for user rserve2 in local and HDFS
    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    #Create a new Spark compute context 
    rxSparkConnect(reset = TRUE)


In dieser Phase ist die Konfiguration der Operationalisierung abgeschlossen. Nun können Sie das mrsdeploy-Paket auf R Client verwenden, um eine Verbindung mit der Operationalisierung auf dem Edgeknoten herzustellen. Anschließend können Sie mit der Nutzung der Features beginnen, z.B. [Remoteausführung](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) und [Webdienste](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Je nachdem, ob Ihr Cluster in einem virtuellen Netzwerk eingerichtet ist, müssen Sie über eine SSH-Anmeldung ein Tunneling für die Portweiterleitung einrichten.

### <a name="r-server-cluster-on-a-virtual-network"></a>R Server-Cluster in einem virtuellen Netzwerk

Stellen Sie sicher, dass Sie Datenverkehr über Port 12800 zum Edgeknoten zulassen. Auf diese Weise können Sie den Edgeknoten zum Herstellen der Verbindung mit dem Feature „Operationalisierung“ verwenden.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Wenn mit `remoteLogin()` keine Verbindung mit dem Edgeknoten hergestellt werden kann, dies per SSH aber möglich ist, sollten Sie überprüfen, ob die Regel zum Zulassen von Datenverkehr über Port 12800 richtig festgelegt ist. Wenn das Problem weiterhin besteht, können Sie es umgehen, indem Sie über SSH das Tunneling der Portweiterleitung einrichten. Anweisungen finden Sie im folgenden Abschnitt.

### <a name="r-server-cluster-not-set-up-on-a-virtual-network"></a>R Server-Cluster nicht in einem virtuellen Netzwerk eingerichtet

Wenn Ihr Cluster nicht in einem virtuellen Netzwerk eingerichtet wurde oder wenn Sie Probleme mit der Konnektivität über ein virtuelles Netzwerk haben, können Sie das Tunneling der SSH-Portweiterleitung nutzen:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Sie können dies auch in PuTTY einrichten:

![SSH-Verbindung in PuTTY](./media/r-server-get-started/putty.png)

Nachdem Ihre SSH-Sitzung aktiv ist, wird der Datenverkehr über die SSH-Sitzung vom Port 12800 des Computers zum Port 12800 des Edgeknotens weitergeleitet. Stellen Sie sicher, dass Sie `127.0.0.1:12800` in der `remoteLogin()`-Methode verwenden. Bei dieser Methode erfolgt eine Anmeldung bei der Operationalisierung des Edgeknotens über die Portweiterleitung.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Skalieren von Computeknoten für die Microsoft R Server-Operationalisierung auf HDInsight-Workerknoten

### <a name="decommission-the-worker-nodes"></a>Außerbetriebsetzen der Workerknoten

Microsoft R Server wird derzeit nicht über Yarn verwaltet. Wenn die Workerknoten nicht außer Betrieb gesetzt werden, funktioniert der Yarn ResourceManager nicht wie erwartet, da er die vom Server verwendeten Ressourcen nicht ermitteln kann. Um diese Situation zu vermeiden, empfehlen wir die Außerbetriebnahme der Workerknoten, bevor Sie die Serverknoten horizontal hochskalieren.

Gehen Sie wie folgt vor, um Workerknoten außer Betrieb zu setzen:

1. Melden Sie sich an der Ambari-Konsole des HDI-Clusters an, und wählen Sie die Registerkarte **Hosts**.
2. Wählen Sie die Workerknoten aus, die außer Betrieb gesetzt werden sollen, und wählen Sie dann **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Wartungsmodus aktivieren**. In der folgenden Abbildung haben wir beispielsweise „wn3“ und „wn4“ für die Außerbetriebnahme ausgewählt.  

   ![Screenshot: Befehle zum Aktivieren des Wartungsmodus](./media/r-server-get-started/get-started-operationalization.png)  

3. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** > **Außer Betrieb setzen**.
4. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** > **Außer Betrieb setzen**.
5. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** > **Beenden**.
6. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** > **Beenden**.
7. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Stop All Components** (Alle Komponenten beenden).
8. Heben Sie die Auswahl der Workerknoten auf, und wählen Sie die Hauptknoten aus.
9. Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Restart All Components** (Alle Komponenten neu starten).

### <a name="configure-compute-nodes-on-each-decommissioned-worker-node"></a>Konfigurieren von Computeknoten auf allen außer Betrieb gesetzten Workerknoten

1. Verwenden Sie SSH, um jeweils eine Verbindung mit den einzelnen außer Betrieb gesetzten Workerknoten herzustellen.
2. Führen Sie mit `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll` ein Verwaltungshilfsprogramm aus.
3. Geben Sie `1` ein, um die Option `Configure R Server for Operationalization` auszuwählen.
4. Geben Sie `c` ein, um die Option `C. Compute node` auszuwählen. In diesem Schritt wird der Computeknoten auf dem Workerknoten konfiguriert.
5. Beenden Sie das Verwaltungshilfsprogramm.

### <a name="add-compute-nodes-details-on-the-web-node"></a>Hinzufügen von Details für Computeknoten auf dem Webknoten

Nachdem alle außer Betrieb gesetzten Workerknoten für die Ausführung auf dem Computeknoten konfiguriert wurden, kehren Sie zum Edgeknoten zurück und fügen die IP-Adressen der außer Betrieb gesetzten Workerknoten der Konfiguration des Webknotens von Microsoft R Server hinzu:

1. Stellen Sie über SSH eine Verbindung mit dem Edgeknoten her.
2. Führen Sie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`aus.
3. Suchen Sie nach dem Abschnitt `URIs`, und fügen Sie die IP-Adress- und Portdetails der Workerknoten hinzu.

    ![Befehlszeile für den Edgeknoten](./media/r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, helfen Ihnen die [Voraussetzungen für die Zugriffssteuerung](../hdinsight-administer-use-portal-linux.md#create-clusters) weiter.


## <a name="next-steps"></a>Nächste Schritte

Als Nächstes sollten Sie sich damit vertraut machen, wie Sie einen HDInsight-Cluster erstellen, der R Server enthält. Außerdem ist es ratsam, dass Sie sich über die Grundlagen der Verwendung einer R-Konsole über eine SSH-Sitzung informieren. In den folgenden Themen werden andere Methoden zum Verwalten von und Arbeiten mit R Server unter HDInsight erläutert:

* [Rechenkontextoptionen für R Server in HDInsight](r-server-compute-contexts.md)
* [Azure Storage-Optionen für R Server in HDInsight](r-server-storage.md)

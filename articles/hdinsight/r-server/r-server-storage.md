---
title: Azure Storage-Lösungen für R Server in HDInsight – Azure | Microsoft-Dokumentation
description: Lernen Sie die verschiedenen Speicheroptionen kennen, die mit R Server in HDInsight zur Verfügung stehen.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: nitinme
ms.openlocfilehash: 23e32a913fb73d2207f7cf37ce6230e428fbe95c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31409464"
---
# <a name="azure-storage-solutions-for-r-server-on-azure-hdinsight"></a>Azure Storage-Lösungen für R Server in Azure HDInsight

R Server in HDInsight bietet eine Vielzahl an Speicherlösungen zum dauerhaften Speichern von Daten, Codes oder Objekten, die Ergebnisse aus der Analyse enthalten. Dazu zählen die folgenden Optionen:

- [Azure-Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/)
- [Azure File Storage](https://azure.microsoft.com/services/storage/files/)

Sie haben auch die Möglichkeit, mit Ihrem HDInsight-Cluster auf mehrere Azure-Speicherkonten oder -Container zuzugreifen. Azure File Storage ist eine komfortable Datenspeicheroption zur Verwendung auf dem Edgeknoten, mit der Sie eine Azure Storage-Dateifreigabe z.B. auf einem Linux-Dateisystem bereitstellen können. Die Dateifreigaben können mit Azure File jedoch von allen Systemen mit einem unterstützten Betriebssystem, z.B. Windows oder Linux, bereitgestellt und verwendet werden. 

Bei der Erstellung eines Hadoop-Clusters in HDInsight geben Sie ein **Azure Storage**-Konto oder einen **Data Lake Store** an. Ein spezieller Speichercontainer dieses Kontos enthält das Dateisystem für den von Ihnen erstellten Cluster (z.B. das Hadoop Distributed File System). Weitere Informationen und Anleitungen finden Sie unter:

- [Verwenden von Azure Storage mit HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Verwenden von Data Lake Store mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-r-server-cluster"></a>Verwenden von Azure Blob Storage-Konten mit R Server-Cluster

Wenn Sie bei der Erstellung Ihres R Server-Clusters mehr als ein Speicherkonto angegeben haben, wird in den folgenden Anweisungen erläutert, wie Sie ein sekundäres Konto für den Datenzugriff und die Vorgänge auf dem R Server-Cluster verwenden können. Nehmen wir die folgenden Speicherkonten und Container an: **storage1** und einen Standardcontainer namens **container1** sowie **storage2** mit **container2**.

> [!WARNING]
> Aus Leistungsgründen wird der HDInsight-Cluster in demselben Rechenzentrum wie das von Ihnen angegebene primäre Speicherkonto erstellt. Die Verwendung eines Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

### <a name="use-the-default-storage-with-r-server-on-hdinsight"></a>Verwenden des Standardspeichers mit R Server auf HDInsight

1. Stellen Sie mit einem SSH-Client eine Verbindung mit dem Edgeknoten Ihres Clusters her. Informationen zum Verwenden von SSH mit HDInsight-Clustern finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Kopieren Sie die Beispieldatei „mysamplefile.csv“in das Verzeichnis „/share“. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. Wechseln Sie zu R Studio oder einer anderen R-Konsole, und schreiben Sie R-Code, um den Namensknoten auf **default** und den Speicherort der Datei festzulegen, auf die Sie zugreifen möchten.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Alle Verzeichnis- und Dateiverweise zeigen auf das Speicherkonto `wasb://container1@storage1.blob.core.windows.net`. Dies ist das **Standardspeicherkonto**, das dem HDInsight-Cluster zugeordnet ist.

### <a name="use-the-additional-storage-with-r-server-on-hdinsight"></a>Verwenden des zusätzlichen Speichers mit R Server auf HDInsight

Angenommen, Sie möchten eine Datei mit dem Namen „mysamplefile1.csv“ verarbeiten, die im Verzeichnis „/private“ von **container2** in **storage2** enthalten ist.

Legen Sie den Namensknotenverweis im R-Code auf das Speicherkonto **storage2** fest.

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")

Alle Verzeichnis- und Dateiverweise zeigen jetzt auf das Speicherkonto `wasb://container2@storage2.blob.core.windows.net`. Dies ist der **Namensknoten**, den Sie angegeben haben.

Sie müssen das Verzeichnis „/user/RevoShare/<SSH username>“ unter **storage2** wie folgt konfigurieren:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-an-azure-data-lake-store-with-r-server-cluster"></a>Verwenden einer Azure Data Lake Store-Instanz mit R Server 

Zum Verwenden von Data Lake Store mit Ihrem HDInsight-Cluster müssen Sie Ihrem Cluster Zugriff auf jede Azure Data Lake Store-Instanz gewähren, die Sie verwenden möchten. Anleitungen zur Verwendung des Azure-Portals zum Erstellen eines HDInsight-Clusters mit einem Azure Data Lake-Speicherkonto als Standardspeicher oder als zusätzlichen Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Nun verwenden Sie den Speicher im R-Skript so ähnlich wie ein sekundäres Azure-Speicherkonto (wie im vorherigen Verfahren beschrieben).

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Hinzufügen von Clusterzugriff auf Ihre Azure Data Lake-Speicher
Sie greifen auf einen Data Lake-Speicher zu, indem Sie einen Azure AD-Dienstprinzipal (Azure Active Directory) verwenden, der Ihrem HDInsight-Cluster zugeordnet ist.

1. Wählen Sie beim Erstellen des HDInsight-Clusters auf der Registerkarte **Datenquelle** eine **Azure Active Directory-Identität für den Cluster** aus.

2. Wählen Sie im Dialogfeld **Azure Active Directory-Identität für den Cluster** unter **AD-Dienstprinzipal auswählen** die Option **Neu erstellen**.

Nachdem Sie dem Dienstprinzipal einen Namen gegeben und ein Kennwort dafür erstellt haben, klicken Sie auf **ADLS-Zugriff verwalten**, um den Dienstprinzipal Ihrer Data Lake Store-Instanz zuzuordnen.

Sie können nach der Clustererstellung Clusterzugriff auf ein oder mehrere Data Lake Store-Konten hinzufügen. Öffnen Sie den Azure-Portal-Eintrag für eine Data Lake Store-Instanz, und wechseln Sie zu **Daten-Explorer > Zugriff > Hinzufügen**. 

### <a name="how-to-access-the-data-lake-store-from-r-server-on-hdinsight"></a>Zugreifen auf Data Lake Store mit R Server auf HDInsight

Nachdem Sie Zugriff auf eine Data Lake Store-Instanz gewährt haben, können Sie den Speicher im R Server-Cluster auf HDInsight wie ein sekundäres Azure-Speicherkonto nutzen. Der einzige Unterschied ist, dass sich das Präfix **wasb://** wie folgt in **adl://** ändert:


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Die folgenden Befehle werden zum Konfigurieren des Data Lake Store-Kontos mit dem RevoShare-Verzeichnis und zum Hinzufügen der CSV-Beispieldatei aus dem vorherigen Beispiel verwendet:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server-on-hdinsight"></a>Verwenden von Azure File Storage mit R Server auf HDInsight

Es gibt auch eine komfortable Datenspeicheroption zur Verwendung auf dem Edgeknoten mit der Bezeichnung [Azure Files]((https://azure.microsoft.com/services/storage/files/). Hiermit können Sie eine Azure Storage-Dateifreigabe auf einem Linux-Dateisystem bereitstellen. Diese Option kann zum Speichern von Datendateien, R-Skripts und Ergebnisobjekten nützlich sein, die unter Umständen später benötigt werden, besonders wenn es sinnvoll ist, anstelle von HDFS das native Dateisystem auf dem Edgeknoten zu verwenden. 

Ein großer Vorteil von Azure Files besteht darin, dass die Dateifreigaben von allen Systemen mit einem unterstützten Betriebssystem, z.B. Windows oder Linux, bereitgestellt und verwendet werden können. Beispielsweise ist die Verwendung durch einen anderen HDInsight-Cluster, der Ihnen oder einem Mitglied Ihres Teams gehört, eine Azure VM oder sogar ein lokales System möglich. Weitere Informationen finden Sie unter 

- [Verwenden des Azure-Dateispeichers unter Linux](../../storage/files/storage-how-to-use-files-linux.md)
- [Verwenden von Azure File Storage unter Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Nächste Schritte

* [Einführung in R Server und Open-Source-R-Funktionen in HDInsight](r-server-overview.md)
* [Erste Schritte mit R Server in HDInsight](r-server-get-started.md)
* [Rechenkontextoptionen für R Server in HDInsight](r-server-compute-contexts.md)


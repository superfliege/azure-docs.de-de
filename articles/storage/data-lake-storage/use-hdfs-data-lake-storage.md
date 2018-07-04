---
title: Verwenden der HDFS-CLI mit Azure Data Lake Storage Gen2 (Vorschauversion)
description: Einführung in die HDFS-CLI für Data Lake Storage Gen2 (Vorschauversion)
services: storage
documentationcenter: ''
author: artemuwka
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 75fb07120c78c45d422ee5017eac0afcf0e80859
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060817"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Verwenden der HDFS-CLI mit Data Lake Storage Gen2

Azure Data Lake Storage Gen2 (Vorschauversion) ermöglicht einen mit einem [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) vergleichbaren Zugriff auf Daten und deren Verwaltung. Unabhängig davon, ob ein HDInsight-Cluster angehängt ist oder ein Apache Spark-Auftrag mithilfe von Azure Databricks ausgeführt wird, um Analysen für in Azure Data Lake Storage Gen2 gespeicherte Daten vorzunehmen, können Sie die Befehlszeilenschnittstelle (CLI) zum Abrufen und Bearbeiten der geladenen Daten verwenden. Im weiteren Verlauf des Artikels sind die Optionen angegeben, die Ihnen bereitstehen, während [das Azure Storage-Team an zusätzlicher Unterstützung für Azure Storage-Explorer und das Azure-Portal arbeitet](https://azure.microsoft.com/roadmap/). Viel Spaß!

## <a name="hdfs-cli-with-hdinsight"></a>HDFS-CLI mit HDInsight

HDInsight bietet Zugang zum verteilten Dateisystem, das lokal an die Rechenknoten angefügt ist. Auf dieses Dateisystem kann mithilfe der Shell zugegriffen werden, die direkt mit dem HDFS interagiert, sowie anderen Dateisystemen, die von Hadoop unterstützt werden. Nachfolgend sind die häufig verwendeten Befehle und Links zu nützlichen Ressourcen aufgeführt.

>[!IMPORTANT]
>Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden (Informationen zum [Löschen eines Clusters](../../hdinsight/hdinsight-delete-cluster.md)). In Azure Data Lake Storage Gen2 gespeicherte Daten bleiben jedoch auch nach dem Löschen eines HDInsight-Clusters erhalten.

So rufen Sie eine Liste von Dateien oder Verzeichnissen ab:

    hdfs dfs -ls <args>
So erstellen Sie ein Verzeichnis:

    hdfs dfs -mkdir [-p] <paths>
So löschen Sie eine Datei oder ein Verzeichnis:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Nehmen wir nun einen Hadoop-Cluster von HDInsight unter Linux als Beispiel. Zur Verwendung der HDFS-CLI müssen Sie zuerst den [Remotezugriff auf Dienste](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services) einrichten. Wenn Sie [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) auswählen, sieht der PowerShell-Beispielcode folgendermaßen aus:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

Die Verbindungszeichenfolge ist im Abschnitt „SSH + Clusteranmeldung“ auf dem Blatt für den HDInsight-Cluster im Azure-Portal zu finden. SSH-Anmeldeinformationen wurden zum Zeitpunkt der Clustererstellung angegeben.

Weitere Informationen zur HDFS-CLI finden Sie in der [offiziellen Dokumentation](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) und im [Handbuch zu HDFS-Berechtigungen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS-CLI mit Azure Databricks

Databricks bietet eine einfach zu verwendende Befehlszeilenschnittstelle, die auf der Databricks-REST-API basiert. Das Open-Source-Projekt wird auf [GitHub](https://github.com/databricks/databricks-cli) gehostet. Nachfolgend sind die häufig verwendeten Befehle aufgeführt.

So rufen Sie eine Liste von Dateien oder Verzeichnissen ab:

    dbfs ls [-l]
So erstellen Sie ein Verzeichnis:

    dbfs mkdirs
So löschen Sie eine Datei:

    dbfs rm [-r]

Eine weitere Möglichkeit zur Interaktion mit Databricks sind Notizbücher. Zwar hat ein Notizbuch eine primäre Sprache, doch können Sie Sprachen mischen, indem Sie den Magic-Sprachbefehl „%language“ am Anfang einer Zelle angeben. Insbesondere ermöglicht Ihnen „%sh“ das Ausführen von Shellcode in Ihrem Notizbuch ähnlich wie im HDInsight-Beispiel weiter oben in diesem Artikel.

So rufen Sie eine Liste von Dateien oder Verzeichnissen ab:

    %sh ls <args>
So erstellen Sie ein Verzeichnis:

    %sh mkdir [-p] <paths>
So löschen Sie eine Datei oder ein Verzeichnis:

    %sh rm [-skipTrash] URI [URI ...]

Nachdem Sie den Spark-Cluster in Azure Databricks gestartet haben, erstellen Sie ein neues Notizbuch. Ein Beispiel für das Notizbuchskript sieht folgendermaßen aus:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Weitere Informationen zur Databricks-Befehlszeilenschnittstelle finden Sie in der [offiziellen Dokumentation](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Weitere Informationen zu Notizbüchern finden Sie im Abschnitt [Notizbücher](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) der Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines HDInsight-Clusters mit Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
- [Verwenden eines Azure Data Lake Storage Gen2-Kontos in Azure Databricks](./quickstart-create-databricks-account.md) 
---
title: Verwenden der HDFS-CLI mit Azure Data Lake Storage Gen2
description: Einführung in die HDFS-CLI für Data Lake Storage Gen2
services: storage
author: artemuwka
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: artek
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: f1f4cb036f4df226d651f8f4d0f5c7492f453a0a
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269739"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Verwenden der HDFS-CLI mit Data Lake Storage Gen2

Azure Data Lake Storage Gen2 ermöglicht einen mit einem [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) vergleichbaren Zugriff auf Daten sowie deren Verwaltung. Unabhängig davon, ob ein HDInsight-Cluster angehängt ist oder ein Apache Spark-Auftrag mithilfe von Azure Databricks ausgeführt wird, um Analysen für in einem Azure Storage-Konto gespeicherte Daten vorzunehmen, können Sie die Befehlszeilenschnittstelle (CLI) zum Abrufen und Bearbeiten der geladenen Daten verwenden.

## <a name="hdfs-cli-with-hdinsight"></a>HDFS-CLI mit HDInsight

HDInsight bietet Zugang zum verteilten Dateisystem, das lokal an die Rechenknoten angefügt ist. Auf dieses Dateisystem kann mithilfe der Shell zugegriffen werden, die direkt mit dem HDFS interagiert, sowie anderen Dateisystemen, die von Hadoop unterstützt werden. Nachfolgend sind die häufig verwendeten Befehle und Links zu nützlichen Ressourcen aufgeführt.

>[!IMPORTANT]
>Die Abrechnung für einen HDInsight-Cluster beginnt, nachdem der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. Informationen zum Löschen eines Clusters finden Sie in unserem [Artikel zum Thema](../../hdinsight/hdinsight-delete-cluster.md). In einem Speicherkonto mit aktiviertem Data Lake Storage Gen2 gespeicherte Daten bleiben jedoch auch nach dem Löschen eines HDInsight-Clusters erhalten.

### <a name="create-a-file-system"></a>Erstellen eines Dateisystems

    hdfs dfs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/

* Ersetzen Sie den Platzhalter `<file-system-name>` durch den Namen, den Sie für Ihr Dateisystem verwenden möchten.

* Ersetzen Sie den Platzhalter `<storage-account-name>` durch den Namen Ihres Speicherkontos.

### <a name="get-a-list-of-files-or-directories"></a>Abrufen einer Liste mit Dateien oder Verzeichnissen

    hdfs dfs -ls <path>

Ersetzen Sie den Platzhalter `<path>` durch den URI des Dateisystems oder des Dateisystemordners.

Beispiel: `hdfs dfs -ls abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name`

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

    hdfs dfs -mkdir [-p] <path>

Ersetzen Sie den Platzhalter `<path>` durch den Namen des Stammdateisystems oder einen Ordner in Ihrem Dateisystem.

Beispiel: `hdfs dfs -mkdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/`

### <a name="delete-a-file-or-directory"></a>Löschen einer Datei oder eines Verzeichnisses

    hdfs dfs -rm <path>

Ersetzen Sie den Platzhalter `<path>` durch den URI der Datei oder des Ordners, die bzw. den Sie löschen möchten.

Beispiel: `hdfs dfs -rmdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name/my-file-name`

### <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>Verwenden der Hadoop Distributed File System-Befehlszeilenschnittstelle mit einem HDInsight Hadoop-Cluster unter Linux

Richten Sie zuerst den [Remotezugriff auf Dienste](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services) ein. Wenn Sie [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) auswählen, sieht der PowerShell-Beispielcode folgendermaßen aus:

```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
Die Verbindungszeichenfolge ist im Abschnitt „SSH + Clusteranmeldung“ auf dem Blatt für den HDInsight-Cluster im Azure-Portal zu finden. SSH-Anmeldeinformationen wurden zum Zeitpunkt der Clustererstellung angegeben.

Weitere Informationen zur HDFS-CLI finden Sie in der [offiziellen Dokumentation](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) und im [Handbuch zu HDFS-Berechtigungen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html). Weitere Informationen zu den Zugriffssteuerungslisten in Databricks finden Sie unter [Secrets CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#secrets-cli).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS-CLI mit Azure Databricks

Databricks bietet eine einfach zu verwendende Befehlszeilenschnittstelle, die auf der Databricks-REST-API basiert. Das Open-Source-Projekt wird auf [GitHub](https://github.com/databricks/databricks-cli) gehostet. Nachfolgend sind die häufig verwendeten Befehle aufgeführt.

### <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Fügen Sie die Anleitung hier ein.

### <a name="get-a-list-of-files-or-directories"></a>Abrufen einer Liste mit Dateien oder Verzeichnissen

    dbfs ls [-l]

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

    dbfs mkdirs

### <a name="delete-a-file"></a>Löschen von Dateien

    dbfs rm [-r]

Eine weitere Möglichkeit zur Interaktion mit Databricks sind Notizbücher. Zwar hat ein Notizbuch eine primäre Sprache, doch können Sie Sprachen mischen, indem Sie den Magic-Sprachbefehl „%language“ am Anfang einer Zelle angeben. Insbesondere ermöglicht Ihnen „%sh“ das Ausführen von Shellcode in Ihrem Notizbuch ähnlich wie im HDInsight-Beispiel weiter oben in diesem Artikel.

### <a name="get-a-list-of-files-or-directories"></a>Abrufen einer Liste mit Dateien oder Verzeichnissen

    %sh ls <args>

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

    %sh mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>Löschen einer Datei oder eines Verzeichnisses

    %sh rm [-skipTrash] URI [URI ...]

Nachdem Sie den Spark-Cluster in Azure Databricks gestartet haben, erstellen Sie ein neues Notizbuch. Ein Beispiel für das Notizbuchskript sieht folgendermaßen aus:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder
    #Get the ACL of the newly created directory.
    hdfs dfs -getfacl /samplefolder

Weitere Informationen zur Databricks-Befehlszeilenschnittstelle finden Sie in der [offiziellen Dokumentation](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Weitere Informationen zu Notizbüchern finden Sie im Abschnitt [Notizbücher](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) der Dokumentation.

## <a name="set-file-and-directory-level-permissions"></a>Festlegen von Berechtigungen auf Datei- und Verzeichnisebene

Sie können Zugriffsberechtigungen auf Datei- und Verzeichnisebene festlegen und abrufen. Hier sind einige Befehle als Einstiegshilfe angegeben. 

Weitere Informationen zu Berechtigungen auf Datei- und Verzeichnisebene für das Azure Data Lake Gen2-Dateisystem finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](storage-data-lake-storage-access-control.md).

### <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>Anzeigen der Zugriffssteuerungslisten (Access Control Lists, ACLs) von Dateien und Verzeichnissen

    hdfs dfs -getfacl [-R] <path>

Beispiel:

`hdfs dfs -getfacl -R /dir`

Siehe [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl).

### <a name="set-acls-of-files-and-directories"></a>Festlegen der Zugriffssteuerungslisten von Dateien und Verzeichnissen

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Beispiel:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

Siehe [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl).

### <a name="change-the-owner-of-files"></a>Ändern des Besitzers von Dateien

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Siehe [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown).

### <a name="change-group-association-of-files"></a>Ändern der Gruppenzuordnung von Dateien

    hdfs dfs -chgrp [-R] <group> <URI>

Siehe [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp).

### <a name="change-the-permissions-of-files"></a>Ändern der Berechtigungen von Dateien

    hdfs dfs -chmod [-R] <mode> <URI>

Siehe [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod).

Sie können die vollständige Liste mit Befehlen auf der Website [Apache Hadoop 2.4.1 File System Shell Guide](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) (Leitfaden zur Apache Hadoop 2.4.1-Dateisystemshell) anzeigen.

## <a name="next-steps"></a>Nächste Schritte

[Verwenden eines Azure Data Lake Storage Gen2-Kontos in Azure Databricks](./data-lake-storage-quickstart-create-databricks-account.md) 

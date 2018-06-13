---
title: Verwenden von MapReduce und Curl mit Hadoop in HDInsight – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie MapReduce-Aufträge mit Hadoop in HDInsight mithilfe von Curl remote ausführen.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: eeecdb6432c4ab13b051c9a9dba1e7f14ce40f91
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31400213"
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Ausführen von MapReduce-Aufträgen mit Hadoop in HDInsight mithilfe von REST

Erfahren Sie, wie mithilfe der WebHCat-REST-API MapReduce-Aufträge auf einem Hadoop-Cluster in HDInsight ausführen. Curl dient zum Veranschaulichen der Interaktion mit HDInsight über unformatierte HTTP-Anforderungen zum Ausführen von MapReduce-Aufträgen.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, nicht jedoch mit HDInsight, finden Sie weitere Informationen im Artikel [Was Sie über Linux-basiertes Hadoop in HDInsight wissen müssen](../hdinsight-hadoop-linux-information.md).


## <a id="prereq"></a>Voraussetzungen

* Hadoop in einem HDInsight-Cluster
* Windows PowerShell oder [cURL](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Ausführen eines MapReduce-Auftrags

> [!NOTE]
> Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen den Clusternamen als Teil des URI verwenden, der zum Senden der Anforderungen an den Server genutzt wird.
>
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Sie sollten Anforderungen immer über HTTPS stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Um die Clusteranmeldung für die Skripts in diesem Dokument festzulegen, verwenden Sie einen der folgenden Befehle:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Um den Namen des Clusters festzulegen, verwenden Sie einen der folgenden Befehle:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Sie erhalten eine Antwort, die in etwa dem folgenden JSON entspricht:

        {"status":"ok","version":"v1"}

    Folgende Parameter werden in diesem Befehl verwendet:

   * **-u**: Gibt den Benutzernamen und das Kennwort für die Authentifizierung der Anforderung an
   * **-G**: Gibt an, dass dieser Vorgang eine GET-Anforderung ist.

   Der Anfang des URI (**https://CLUSTERNAME.azurehdinsight.net/templeton/v1**) ist für alle Anforderungen gleich.

4. Verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu übermitteln.

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Das Ende des URIs (/mapreduce/jar) weist WebHCat darauf hin, dass diese Anforderung einen MapReduce-Auftrag aus einer Klasse in einer JAR-Datei startet. Folgende Parameter werden in diesem Befehl verwendet:

   * **-d**: `-G` wird nicht verwendet, daher verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.
    * **user.name**: Der Benutzer, der den Befehl ausführt
    * **jar**: Der Speicherort der JAR-Datei, die die auszuführende Klasse enthält
    * **class**: Die Klasse, die die MapReduce-Logik enthält
    * **arg**: Die Argumente, die an den MapReduce-Auftrag übergeben werden, in diesem Fall die Eingabetextdatei und das Verzeichnis für die Ausgabe

   Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann:

       job_1415651640909_0026

5. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    Wenn der Auftrag abgeschlossen ist, wird der Status `SUCCEEDED` angezeigt.

   > [!NOTE]
   > Diese Curl-Anforderung gibt ein JSON-Dokument mit Informationen zum Auftrag zurück. Mithilfe von „jq“ wird nur der Statuswert abgerufen.

6. Sobald der Status des Auftrags in `SUCCEEDED` wechselt, können Sie die Ergebnisse des Auftrags aus Azure Blob Storage abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Beispiel ist dies `/example/curl`. Diese Adresse speichert die Ausgabe des Auftrags unter `/example/curl` im Standardspeicher des Clusters.

Sie können diese Dateien mithilfe von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) auflisten und herunterladen. Weitere Informationen zur Verwendung von BLOBs über die Azure-Befehlszeilenschnittstelle finden Sie im Artikel [Verwenden der Azure CLI 2.0 mit Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs).

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce-Aufträgen in HDInsight:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

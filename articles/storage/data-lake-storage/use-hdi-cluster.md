---
title: Verwenden von Azure Data Lake Storage Gen2 Preview mit Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie Daten in Azure Data Lake Storage Gen2 Preview abfragen und Ergebnisse Ihrer Analyse speichern.
keywords: HDFS, strukturierte Daten, unstrukturierte Daten, Data Lake Store, Hadoop-Eingabe, Hadoop-Ausgabe, Hadoop-Speicher, HDFS-Eingabe, HDFS-Ausgabe, HDFS-Speicher, WASB Azure
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: jamesbak
manager: jahogg
ms.component: data-lake-storage-gen2
ms.service: storage
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: e4e72d3c5b6b9c96de6a8faafccfcc39fe3c37b8
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136688"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Verwenden von Azure Data Lake Storage Gen2 Preview mit Azure HDInsight-Clustern

Zum Analysieren von Daten im HDInsight-Cluster können Sie die Daten in einer beliebigen Kombination von Azure Storage, Azure Data Lake Storage Gen1 oder Azure Data Lake Storage Gen2 Preview speichern. Alle Speichervarianten ermöglichen das sichere Löschen von HDInsight-Clustern, die für Berechnungen verwendet werden, ohne dass Benutzerdaten verloren gehen.

Hadoop unterstützt eine Variante des Standarddateisystems. Das Standarddateisystem gibt ein Standardschema und eine Standardautorität vor. Es kann auch zur Auflösung relativer Pfade verwendet werden. Während der Erstellung des HDInsight-Clusters können Sie einen Blobcontainer in Azure Storage oder Azure Data Lake Storage als Standarddateisystem angeben. Mit HDInsight 3.5 können Sie (mit einigen Ausnahmen) entweder Azure Storage oder Azure Data Lake Store als Standarddateisystem auswählen.

In diesem Artikel erfahren Sie, wie Azure Data Lake Storage Gen2 mit HDInsight-Clustern funktioniert. Weitere Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Einrichten von HDInsight-Clustern mithilfe von Azure Data Lake Storage mit u.a. Hadoop, Spark und Kafka](quickstart-create-connect-hdi-cluster.md).

Azure Storage stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. In HDInsight kann Azure Data Lake Storage als Standarddateisystem für den Cluster verwendet werden. Über eine HDFS-Schnittstelle (Hadoop Distributed File System) können sämtliche Komponenten in HDInsight direkt mit Dateien in Azure Data Lake Storage arbeiten.

Die Verwendung des Standarddateisystems zum Speichern von Geschäftsdaten wird nicht empfohlen. Stattdessen empfiehlt es sich, das Standarddateisystem nach jeder Verwendung zu löschen, um Speicherkosten zu senken. Beachten Sie, dass der Standardcontainer Anwendungs- und Systemprotokolle enthält. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

Das Freigeben eines Dateisystems für mehrere Cluster wird nicht unterstützt.

## <a name="hdinsight-storage-architecture"></a>HDInsight-Speicherarchitektur

Das folgende Diagramm bietet einen zusammenfassenden Überblick über die HDInsight-Speicherarchitektur bei der Verwendung von Azure Storage:

![Hadoop-Cluster verwenden die HDFS-API, um auf strukturierte und unstrukturierte Daten im Blobspeicher zuzugreifen und diese zu speichern.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight Storage-Architektur")

HDInsight bietet Zugang zum verteilten Dateisystem, das lokal an die Rechenknoten angefügt ist. Auf dieses Dateisystem kann über den vollständig qualifizierten URI zugegriffen werden. Zum Beispiel:

    hdfs://<NAME_NODE_HOST>/<PATH>

Zusätzlich bietet HDInsight die Möglichkeit, auf die in Azure Data Lake Storage gespeicherten Daten zuzugreifen. Die Syntax ist:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<path>

Es folgen einige Aspekte beim Verwenden eines Azure Storage-Kontos mit HDInsight-Clustern.

* **Bei Dateien in Speicherkonten, die mit einem Cluster verbunden sind,** werden der Kontoname und Schlüssel dem Cluster während seiner Erstellung zugeordnet. Diese Konfiguration bietet Ihnen Vollzugriff auf Dateien im Dateisystem.

* **Öffentliche Dateien in Speicherkonten, die NICHT mit einem Cluster verbunden sind,** machen nur Leseberechtigungen für die Dateien im Dateisystem verfügbar.
  
  > [!NOTE]
  > Öffentliche Dateisysteme ermöglichen Ihnen, eine Liste aller im Dateisystem verfügbaren Dateien abzurufen und auf Metadaten zuzugreifen. Öffentliche Dateisysteme erlauben den Zugriff auf Dateien nur, wenn Sie die genaue URL kennen. Weitere Informationen finden Sie unter [Einschränken des Zugriffs auf Container und Blobs](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (die Regeln für Container und Blobs gelten auch für Dateien und das Dateisystem).
 
* **Private Dateisysteme in Speicherkonten, die NICHT mit einem Cluster verbunden sind**, erlauben nicht den Zugriff auf Dateien im Dateisystem, es sei denn, Sie legen das Speicherkonto fest, wenn Sie die WebHCat-Aufträge senden. Gründe für diese Einschränkung werden weiter unten in diesem Artikel erläutert.

Die bei der Erstellung definierten Speicherkonten und ihre Schlüssel werden in der Datei *%HADOOP_HOME%/conf/core-site.xml* auf den Clusterknoten gespeichert. Standardmäßig verwendet HDInsight die in der Datei *core-site.xml* definierten Speicherkonten. Sie können diese Einstellung mit [Ambari](../../hdinsight/hdinsight-hadoop-manage-ambari.md) ändern.

Verschiedene WebHCat-Aufträge, darunter Hive, MapReduce, Hadoop Streaming und Pig, können eine Beschreibung von Speicherkonten und Metadaten enthalten. (Dieser Ansatz funktioniert momentan nur für Pig und Speicherkonten, aber nicht für Metadaten.) Weitere Informationen finden Sie unter [Verwenden eines HDInsight-Clusters mit alternativen Speicherkonten und Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

## <a id="benefits"></a>Vorteile von Azure Storage

Der Leistungsaufwand, der damit verbunden ist, dass die Computecluster und Speicherressourcen nicht an demselben Ort vorliegen, wird dadurch verringert, dass die Computecluster nahe an den Speicherkontoressourcen in der Azure-Region erstellt werden. Hier macht das Hochgeschwindigkeitsnetzwerk den Zugriff auf die Daten im Azure-Speicher für die Serverknoten effizient.

Die Speicherung von Daten im Azure-Speicher anstatt im HDFS hat mehrere Vorteile:

* **Datenfreigabe und -wiederverwendung:** Die Daten im HDFS befinden sich innerhalb des Rechenclusters. Nur die Anwendungen, die Zugriff auf den Rechencluster haben, können die Daten über die HDFS-API verwenden. Auf die Dateien im Azure-Speicher kann entweder über die HDFS-APIs oder über die [Blob Storage-REST-APIs][blob-storage-restAPI] zugegriffen werden. Somit kann eine größere Menge von Anwendungen (darunter andere HDInsight-Cluster) und Tools verwendet werden, um die Daten zu produzieren und abzurufen.

* **Datenarchivierung:** Die Speicherung von Daten im Azure-Speicher sorgt dafür, dass die HDInsight-Cluster, die für Berechnungen verwendet werden, sicher gelöscht werden können, ohne Benutzerdaten zu verlieren.

* **Datenspeicherkosten:** Die langfristige Datenspeicherung im nativen Hadoop Distributed File System ist kostspieliger als die Datenspeicherung im Azure-Speicher, da die Kosten eines Computeclusters höher als die Kosten für Azure-Speicher sind. Da die Daten nicht für jede Erzeugung eines neues Rechenclusters neu geladen werden, sparen Sie auch Kosten für das Laden von Daten.

* **Elastische horizontale Skalierung:** Auch wenn HDFS ein horizontal skaliertes Dateisystem bietet, wird die Skalierung durch die Anzahl der Knoten bestimmt, die Sie für Ihren Cluster erstellen. Eine Änderung der Skalierung kann weitaus schwieriger werden, als auf die flexiblen Speicherkapazitäten zu vertrauen, die Ihnen der Azure-Speicher automatisch bietet.

* **Georeplikation:** Für die Daten in Ihrem Azure-Speicher kann die Georeplikation durchgeführt werden. Obwohl Sie durch diese Funktion von geographischer Wiederherstellung und Datenredundanz profitieren, wirkt sich die Unterstützung eines Ausfalls des georeplizierten Standorts schwer auf Ihre Leistung aus und kann zusätzliche Kosten nach sich ziehen. Daher sollten Sie die Georeplikation mit Bedacht und nur dann auswählen, wenn der Nutzen der Daten die zusätzlichen Kosten rechtfertigt.

* **Verwaltung des Datenlebenszyklus:** Alle Daten in einem beliebigen Dateisystem durchlaufenen einen eigenen Lebenszyklus. Daten sind anfangs sehr wertvoll und es wird häufig darauf zugegriffen, doch mit der Zeit verlieren sie an Wert, und es muss seltener darauf zugegriffen werden. Letztlich ist eine Archivierung oder Löschung erforderlich. Azure Storage bietet Richtlinien für die Verwaltung von Dateneinstufung und -lebenszyklus, gemäß derer die Daten entsprechend ihrer Lebenszyklusphase eingestuft werden.

Bestimmte MapReduce-Aufträge und -Pakete können zu Zwischenergebnissen führen, die Sie eher nicht im Azure-Speicher speichern möchten. In diesem Fall können Sie die Dateien auch im lokalen HDFS speichern. Tatsächlich verwendet HDInsight die Implementierung des nativen Hadoop Distributed File System (auch als DFS bezeichnet) für einige dieser Zwischenergebnisse in Hive-Jobs und anderen Prozessen.

> [!NOTE]
> Die meisten HDFS-Befehle (z.B. `ls`, `copyFromLocal` und `mkdir`) funktionieren weiterhin wie erwartet. Nur die für das DFS spezifischen Befehle wie `fschk` und `dfsadmin` zeigen in Azure Storage ein anderes Verhalten.

## <a name="create-an-data-lake-storage-file-system"></a>Erstellen eines Data Lake Storage-Dateisystems

Um das Dateisystem verwenden zu können, müssen Sie zuerst ein [Azure Storage-Konto][azure-storage-create] erstellen. Hierbei liegen Sie eine Azure-Region fest, in der das Speicherkonto erstellt wird. Cluster und Speicherkonto müssen sich in der gleichen Region befinden. Die SQL Server-Datenbanken für den Hive- und Oozie-Metastore müssen sich ebenfalls in der gleichen Region befinden.

Ein Blob gehört unabhängig davon, wo es sich befindet, stets zu einem Dateisystem in Ihrem Azure Data Lake Storage-Konto. 

Das standardmäßige Data Lake Storage-Dateisystem speichert clusterspezifische Informationen wie etwa Auftragsverlauf und Protokolle. Geben Sie ein standardmäßiges Data Lake Storage-Dateisystem nicht für mehrere HDInsight-Cluster frei. Dies kann zu einer Beschädigung des Auftragsverlaufs führen. Es wird empfohlen, unterschiedliche Dateisysteme für die einzelnen Cluster zu verwenden und freigegebene Daten nicht im Standardspeicherkonto, sondern in einem verknüpften Speicherkonto abzulegen, das in der Bereitstellung aller relevanten Cluster angegeben wird. Weitere Informationen zum Konfigurieren verknüpfter Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern][hdinsight-creation]. Sie können ein standardmäßiges Speicherdateisystem jedoch auch wiederverwenden, nachdem der ursprüngliche HDInsight-Cluster gelöscht wurde. Bei HBase-Clustern können Sie das HBase-Tabellenschema sowie die darin enthaltenen Daten beibehalten, indem Sie einen neuen HBase-Cluster mit dem Standardblobcontainer erstellen, der von einem gelöschten HBase-Cluster verwendet wurde.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Beim Erstellen eines HDInsight-Clusters über das Portal können Sie Optionen zum Angeben der Details zum Speicherkonto nutzen (siehe den folgenden Screenshot). Sie können auch angeben, ob dem Cluster ein zusätzliches Speicherkonto zugeordnet werden soll. Falls ja, können Sie eine der verfügbaren Speicheroptionen als zusätzlichen Speicher auswählen.

![HDInsight, Hadoop, Erstellung, Datenquelle](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

### <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

Nach dem [Installieren und Konfigurieren von Azure PowerShell][powershell-install] können Sie ein Speicherkonto und einen Container mit dem folgenden Code an der Azure PowerShell-Eingabeaufforderung erstellen:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Das Erstellen eines Containers ist gleichbedeutend mit dem Erstellen eines Dateisystems in Azure Data Lake Storage.

### <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

Wenn Sie [die Befehlszeilenschnittstelle von Azure (Azure-CLI) installiert und konfiguriert](../../cli-install-nodejs.md)haben, können Sie zur Erstellung eines Speicherkontos und Containers den folgenden Befehl verwenden.

```bash
az storage account create \
    --name <STORAGE_ACCOUNT_NAME> \
    --resource-group <RESOURCE_GROUP_NAME> \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --Enable-hierarchical-namespace true
```

> [!NOTE]
> Während der Public Preview von Data Lake Storage Gen2 wird nur `--sku Standard_LRS` unterstützt.

Sie werden zum Angeben der geografischen Region aufgefordert, in der das Speicherkonto erstellt wird. Erstellen Sie das Speicherkonto in der Region, in der der HDInsight-Cluster erstellt werden soll.

Nach der Erstellung des Speicherkontos können Sie den Schlüssel des Speicherkontos mit dem folgenden Befehl abrufen:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Zum Erstellen eines Containers verwenden Sie den folgenden Befehl:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Das Erstellen eines Containers ist gleichbedeutend mit dem Erstellen eines Dateisystems in Azure Data Lake Storage.

## <a name="address-files-in-azure-storage"></a>Zugreifen auf Dateien in Azure-Speicher

Das URI-Schema für den Zugriff auf Dateien in Azure-Speicher aus HDInsight lautet:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.widows.net/<PATH>

Das URI-Schema ermöglicht unverschlüsselten Zugriff (mit dem Präfix *abfs:*) wie auch mit SSL verschlüsselten Zugriff (mit *abfss*). Wir empfehlen die Verwendung von *abfss*, und zwar auch für den Zugriff auf Daten, die sich in derselben Azure-Region befinden.

* &lt;FILE_SYSTEM_NAME&gt; gibt den Pfad des Dateisystems in Azure Data Lake Storage an.
* &lt;ACCOUNT_NAME&gt; gibt den Namen des Azure Storage-Kontos an. Ein vollqualifizierter Domänenname (FQDN) ist erforderlich.

    Wenn weder Werte für &lt;FILE_SYSTEM_NAME&gt; noch für &lt;ACCOUNT_NAME&gt; angegeben wurden, wird das Standarddateisystem verwendet. Für die Dateien im Standarddateisystem können Sie relative oder absolute Pfade verwenden. Auf die Datei *hadoop-mapreduce-examples.jar*, die sich in HDInsight-Clustern befindet, kann z.B. mithilfe eines der folgenden Pfade verwiesen werden:
    
        abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> In HDInsight-Clustern der Version 2.1 und 1.6 lautet der Dateiname *hadoop-examples.jar*.

* &lt;PATH&gt; ist der Pfadname des Hadoop Distributed File System für die Datei oder das Verzeichnis.

> [!NOTE]
> Wenn Dateien außerhalb von HDInsight verwendet werden, wird das ABFS-Format von den meisten Hilfsprogrammen nicht erkannt. Diese erwartet vielmehr ein einfaches Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>Verwenden zusätzlicher Speicherkonten

Beim Erstellen eines HDInsight-Clusters geben Sie das Azure Storage-Konto an, dem Sie es zuordnen möchten. Zusätzlich zu diesem Speicherkonto können Sie während des Erstellungsprozesses oder nach der Erstellung eines Clusters weitere Speicherkonten aus dem gleichen oder einem anderen Azure-Abonnement hinzufügen. Informationen zum Hinzufügen zusätzlicher Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie Sie HDFS-kompatiblen Azure Storage mit HDInsight verwenden. Über diesen Ansatz können Sie skalierbare Datenerfassungslösungen mit langfristiger Archivierung aufbauen und HDInsight verwenden, um die Informationen innerhalb der gespeicherten strukturierten und unstrukturierten Daten freizugeben.

Weitere Informationen finden Sie unter

* [Der ABFS-Hadoop-Dateisystemtreiber für Azure Data Lake Storage Gen2](abfs-driver.md)
* [Einführung in Azure Data Lake Storage](introduction.md)
* [Einrichten von HDInsight-Clustern mithilfe von Azure Data Lake Storage mit u.a. Hadoop, Spark und Kafka](quickstart-create-connect-hdi-cluster.md)
* [Erfassen von Daten in Azure Data Lake Storage mit distcp](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png

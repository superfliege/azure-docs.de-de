---
title: 'Abfragen von Daten aus HDFS-kompatiblem Azure-Speicher: Azure HDInsight'
description: Es wird beschrieben, wie Sie Daten in Azure-Speicher und Azure Data Lake Storage abfragen, um die Ergebnisse Ihrer Analyse zu speichern.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 01/28/2019
ms.openlocfilehash: d88a05b03813eb0ec94a84f60bffb903e1344987
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361913"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Verwenden von Azure Storage mit Azure HDInsight-Clustern

Zum Analysieren von Daten im HDInsight-Cluster können Sie die Daten in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) oder beidem speichern. Beide Speichervarianten ermöglichen das sichere Löschen der HDInsight-Cluster, die für Berechnungen verwendet werden, ohne Benutzerdaten zu verlieren.

Apache Hadoop unterstützt eine Variante des Standarddateisystems. Das Standarddateisystem gibt ein Standardschema und eine Standardautorität vor. Es kann auch zur Auflösung relativer Pfade verwendet werden. Bei der Erstellung des HDInsight-Clusters können Sie einen Blobcontainer in Azure Storage als Standarddateisystem angeben. Mit HDInsight 3.6 können Sie Azure Storage oder Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen2 als Standarddateisystem auswählen (mit einigen Ausnahmen). Informationen zur Unterstützung von Data Lake Storage Gen1 als Standardspeicher und als verknüpfter Speicher finden Sie unter [Verfügbarkeit für HDInsight-Cluster](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

In diesem Artikel erfahren Sie, wie Azure Storage mit HDInsight-Clustern funktioniert. Informationen zur Funktionsweise von Data Lake Storage Gen1 mit HDInsight-Clustern finden Sie unter [Verwenden von Azure Data Lake Storage mit Azure HDInsight-Clustern](hdinsight-hadoop-use-data-lake-store.md). Weitere Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Erstellen von Apache Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Azure Storage stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. In HDInsight kann ein Blobcontainer in Azure Storage als Standarddateisystem für den Cluster verwendet werden. Über eine HDFS-Schnittstelle (Hadoop Distributed File System) können sämtliche Komponenten in HDInsight direkt mit strukturierten oder unstrukturierten Daten arbeiten, die als Blobs gespeichert sind.

> [!WARNING]  
> Beim Erstellen eines Azure Storage-Kontos stehen Ihnen mehrere Optionen zur Verfügung. Die folgende Tabelle enthält Informationen dazu, welche Optionen mit HDInsight unterstützt werden:

| Speicherkontotyp | Unterstützte Dienste | Unterstützte Leistungsstufen | Unterstützte Zugriffsebenen |
|----------------------|--------------------|-----------------------------|------------------------|
| Allgemein v2   | Blob               | Standard                    | Heiß, Kalt, Archiv\*    |
| Allgemein v1   | Blob               | Standard                    | –                    |
| Blob Storage         | Blob               | Standard                    | Heiß, Kalt, Archiv\*    |

Die Verwendung des Standard-Blobcontainers zum Speichern von Geschäftsdaten wird nicht empfohlen. Stattdessen empfiehlt es sich, den Standard-Blobcontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Der Standardcontainer enthält Anwendungs- und Systemprotokolle. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

Das Freigeben eines Blobcontainers als Standarddateisystem für mehrere Cluster wird nicht unterstützt.
 
 > [!NOTE]  
 > Die Zugriffsebene „Archiv“ ist eine Offlineebene, die eine Abrufwartezeit von mehreren Stunden aufweist, und wird für die Verwendung mit HDInsight nicht empfohlen. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier">Zugriffsebene „Archiv“</a>.

## <a name="hdinsight-storage-architecture"></a>HDInsight-Speicherarchitektur
Das folgende Diagramm bietet einen zusammenfassenden Überblick über die HDInsight-Speicherarchitektur bei der Verwendung von Azure Storage:

![Hadoop-Cluster verwenden die HDFS-API, um auf strukturierte und unstrukturierte Daten im Blobspeicher zuzugreifen und diese zu speichern.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage-Architektur")

HDInsight bietet Zugang zum verteilten Dateisystem, das lokal an die Rechenknoten angefügt ist. Auf dieses Dateisystem kann über den vollständig qualifizierten URI zugegriffen werden. Zum Beispiel:

    hdfs://<namenodehost>/<path>

Zusätzlich bietet HDInsight die Möglichkeit, auf die in Azure Storage gespeicherten Daten zuzugreifen. Die Syntax ist:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Hier sind einige Aspekte beim Verwenden eines Azure Storage-Kontos mit HDInsight-Clustern.

* **Container in Speicherkonten, die mit einem Cluster verbunden sind:** Da Kontoname und Schlüssel dem Cluster bei der Erstellung zugeordnet werden, haben Sie vollständigen Zugriff auf die Blobs in diesen Containern.

* **Öffentliche Container oder öffentliche Blobs in Speicherkonten, die NICHT mit einem Cluster verbunden sind:** Sie verfügen über Leserechte für die Blobs in den Containern.
  
  > [!NOTE]  
  > Öffentliche Container erlauben das Abrufen einer Liste aller im Container verfügbaren Blobs und der Container-Metadaten. Auf öffentliche Blobs haben Sie nur Zugriff, wenn Sie die exakte URL kennen. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources">Verwalten des Zugriffs auf Container und Blobs</a>.

* **Private Container in Speicherkonten, die NICHT mit einem Cluster verbunden sind:** Sie können nicht auf die Blobs in den Containern zugreifen, es sei denn, Sie definieren beim Senden der WebHCat-Aufträge das Speicherkonto. Dies wird weiter unten im Artikel erläutert.

Die bei der Erstellung definierten Speicherkonten und ihre Schlüssel werden in der Datei „%HADOOP_HOME%/conf/core-site.xml“ auf den Clusterknoten gespeichert. Standardmäßig verwendet HDInsight die in der Datei core-site.xml definierten Speicherkonten. Sie können diese Einstellung mit [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) ändern.

Verschiedene WebHCat-Aufträge, darunter Apache Hive, MapReduce, Apache Hadoop Streaming und Apache Pig, können eine Beschreibung von Speicherkonten und Metadaten enthalten.  (Dies funktioniert momentan nur für Pig und Speicherkonten, nicht für Metadaten.) Weitere Informationen finden Sie unter [Verwenden eines HDInsight-Clusters mit alternativen Speicherkonten und Metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blobs können für strukturierte und unstrukturierte Daten verwendet werden. In Blobcontainern werden Daten als Schlüssel-Wert-Paare gespeichert, und es gibt keine Verzeichnishierarchie. Allerdings kann im Schlüsselnamen der Schrägstrich (/) verwendet werden, damit es so aussieht, als wäre eine Datei in einer Verzeichnisstruktur gespeichert. Der Schlüssel eines Blobs kann z. B. *input/log1.txt* heißen. Das Verzeichnis *input* existiert zwar nicht, wegen des Schrägstrichs (/) im Schlüsselnamen sieht es jedoch so aus, als gäbe es einen Dateipfad.

## <a id="benefits"></a>Vorteile von Azure Storage
Der Leistungsaufwand, der damit verbunden ist, dass die Computecluster und Speicherressourcen nicht an demselben Ort vorliegen, wird dadurch verringert, dass die Computecluster nahe an den Speicherkontoressourcen in der Azure-Region erstellt werden. Hier macht das Hochgeschwindigkeitsnetzwerk den Zugriff auf die Daten im Azure-Speicher für die Serverknoten effizient.

Die Speicherung von Daten im Azure-Speicher anstatt im HDFS hat mehrere Vorteile:

* **Datenwiederverwendung und -freigabe:** Die Daten im HDFS befinden sich innerhalb des Computeclusters. Nur die Anwendungen, die Zugriff auf den Rechencluster haben, können die Daten über die HDFS-API verwenden. Auf die Dateien im Azure-Speicher kann entweder über die HDFS-APIs oder über die [Blob Storage-REST-APIs][blob-storage-restAPI] zugegriffen werden. Somit kann eine größere Menge von Anwendungen (darunter andere HDInsight-Cluster) und Tools verwendet werden, um die Daten zu produzieren und abzurufen.
* **Datenarchivierung:** Die Speicherung von Daten im Azure-Speicher sorgt dafür, dass die HDInsight-Cluster, die für Berechnungen verwendet werden, sicher gelöscht werden können, ohne Benutzerdaten zu verlieren.
* **Datenspeicherkosten:** Die langfristige Datenspeicherung in DFS ist kostspieliger als die Datenspeicherung im Azure-Speicher, da die Kosten eines Computeclusters höher als die Kosten für Azure-Speicher sind. Da die Daten nicht für jede Erzeugung eines neues Rechenclusters neu geladen werden, sparen Sie auch Kosten für das Laden von Daten.
* **Elastische horizontale Skalierung:** Auch wenn HDFS ein horizontal skaliertes Dateisystem bietet, wird die Skalierung durch die Anzahl der Knoten bestimmt, die Sie für Ihren Cluster erstellen. Eine Änderung der Skalierung kann weitaus schwieriger werden, als auf die flexiblen Speicherkapazitäten zu vertrauen, die Ihnen der Azure-Speicher automatisch bietet.
* **Georeplikation:** Für den Azure-Speicher kann eine Georeplikation durchgeführt werden. Obwohl Sie dadurch von geographischer Wiederherstellung und Datenredundanz profitieren, wirkt sich ein Ausfall des georeplizierten Standorts schwer auf Ihre Leistung aus und kann zusätzliche Kosten nach sich ziehen. Deshalb empfehlen wir, die Georeplikation mit Bedacht auszuwählen und nur dann anzuwenden, wenn der Wert der Daten die zusätzlichen Kosten rechtfertigt.

Bestimmte MapReduce-Aufträge und -Pakete können zu Zwischenergebnissen führen, die Sie eher nicht im Azure-Speicher speichern möchten. In diesem Fall können Sie die Dateien auch im lokalen HDFS speichern. Tatsächlich verwendet HDInsight DFS für einige dieser Zwischenergebnisse in Hive-Jobs und anderen Prozessen.

> [!NOTE]  
> Die meisten HDFS-Befehle (z. B. <b>ls</b>, <b>copyFromLocal</b> und <b>mkdir</b>) funktionieren weiterhin wie erwartet. Nur die für die native HDFS-Implementierung (als DFS bezeichnet) spezifischen Befehle wie <b>fschk</b> und <b>dfsadmin</b> zeigen im Azure-Speicher ein anderes Verhalten.


## <a name="create-blob-containers"></a>Erstellen eines Blob-Containers
Um Blobs zu verwenden, erstellen Sie zuerst ein Azure-[Speicherkonto][azure-storage-create]. Hierzu liegen Sie eine Azure-Region fest, in der das Speicherkonto erstellt wird. Cluster und Speicherkonto müssen sich in der gleichen Region befinden. Die SQL Server-Datenbanken für den Hive- und Apache Oozie-Metastore müssen sich ebenfalls in der gleichen Region befinden.

Ein Blob gehört unabhängig davon, wo es sich befindet, stets zu einem Container Ihres Azure-Speicherkontos. Dieser Container kann ein außerhalb von HDInsight erstelltes Blob sein, oder es handelt sich um einen Container, der für einen HDInsight-Cluster erstellt wird.

Der standardmäßige Blobcontainer speichert clusterspezifische Informationen wie etwa Auftragsverlauf und Protokolle. Geben Sie einen Standard-Blob-Container nicht für mehrere HDInsight-Cluster frei. Dies kann zu einer Beschädigung des Auftragsverlaufs führen. Es wird empfohlen, unterschiedliche Container für die einzelnen Cluster zu verwenden und freigegebene Daten nicht im Standardspeicherkonto, sondern in einem verknüpften Speicherkonto abzulegen, das in der Bereitstellung aller relevanten Cluster angegeben wird. Weitere Informationen zum Konfigurieren verknüpfter Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern][hdinsight-creation]. Einen Standardspeichercontainer können Sie jedoch auch wiederverwenden, wenn der ursprüngliche HDInsight-Cluster gelöscht wurde. Bei HBase-Clustern können Sie das HBase-Tabellenschema sowie die darin enthaltenen Daten sogar beibehalten, indem Sie einen neuen HBase-Cluster mit dem Standardblobcontainer erstellen, der von einem gelöschten HBase-Cluster verwendet wurde.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals
Beim Erstellen eines HDInsight-Clusters über das Portal können Sie Optionen zum Angeben der Details zum Speicherkonto nutzen (wie unten dargestellt). Sie können auch angeben, ob dem Cluster ein zusätzliches Speicherkonto zugeordnet werden soll. Wenn ja, können Sie Data Lake Storage oder ein anderes Azure Storage-Blob als zusätzlichen Speicher auswählen.

![HDInsight, Hadoop, Erstellung, Datenquelle](./media/hdinsight-hadoop-use-blob-storage/storage.png)

> [!WARNING]  
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.


### <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Nach dem [Installieren und Konfigurieren von Azure PowerShell][powershell-install] können Sie ein Speicherkonto und einen Container wie folgt an der Azure PowerShell-Eingabeaufforderung erstellen:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzAccount
    Select-AzSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-classic-cli"></a>Verwenden der klassischen Azure CLI

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Wenn Sie [die klassische Befehlszeilenschnittstelle von Azure (Azure-CLI) installiert und konfiguriert](../cli-install-nodejs.md) haben, können Sie zur Erstellung eines Speicherkontos und Containers den folgenden Befehl verwenden.

```cli
azure storage account create <storageaccountname> --type LRS
```

> [!NOTE]  
> Der Parameter `--type` gibt an, wie das Speicherkonto repliziert wird. Weitere Informationen finden Sie unter [Azure-Speicherreplikation](../storage/storage-redundancy.md). Verwenden Sie keinen ZRS, da ZRS keine Seitenblobs, Dateien, Tabellen oder Warteschlangen unterstützt.

Sie werden zum Angeben der geografischen Region aufgefordert, in der das Speicherkonto erstellt wird. Das Speicherkonto sollte in derselben Region erstellt werden, in der der HDInsight-Cluster erstellt werden soll.

Nach der Erstellung des Speicherkontos können Sie den Schlüssel des Speicherkontos mit dem folgenden Befehl abrufen:

```cli
azure storage account keys list <storageaccountname>
```

Zum Erstellen eines Containers verwenden Sie den folgenden Befehl:

```cli
azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>
```

## <a name="address-files-in-azure-storage"></a>Zugreifen auf Dateien in Azure-Speicher
Das URI-Schema für den Zugriff auf Dateien in Azure-Speicher aus HDInsight lautet:

```config
wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

Das URI-Schema bietet unverschlüsselten Zugriff (mit dem Präfix *wasb:*) wie auch SSL-verschlüsselten Zugriff (mit *wasbs*). Wir empfehlen die Verwendung von *wasbs* , und zwar auch für den Zugriff auf Daten, die sich in Azure in derselben Region befinden.

&lt;BlobStorageContainerName&gt; ist der Name des Blobcontainers im Azure-Speicher.
&lt;StorageAccountName&gt; ist der Name des Azure-Speicherkontos. Ein vollqualifizierter Domänenname (FQDN) ist erforderlich.

Wenn weder &lt;BlobStorageContainerName&gt; noch &lt;StorageAccountName&gt; angegeben ist, wird das Standarddateisystem verwendet. Für die Dateien im Standarddateisystem können Sie relative oder absolute Pfade verwenden. Auf die Datei *hadoop-mapreduce-examples.jar*, die sich in HDInsight-Clustern befindet, kann z. B. mit einem der folgenden Befehle verwiesen werden:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> In HDInsight-Clustern der Version 2.1 und 1.6 lautet der Dateiname <i>hadoop-examples.jar</i>.

&lt;path&gt; ist der HDFS-Pfadname der Datei oder des Verzeichnisses. Da es sich bei Containern im Azure-Speicher um Schlüssel-Wert-Paare handelt, wird kein echtes hierarchisches Dateisystem verwendet. Ein Schrägstrich (/) in einem Blobschlüssel wird als Verzeichnistrennzeichen interpretiert. Der Blob-Name für *hadoop-mapreduce-examples.jar* lautet z. B.:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> Wenn Blobs außerhalb von HDInsight verwendet werden, wird das WASB-Format von den meisten Dienstprogrammen nicht erkannt. Diese erwartet vielmehr ein grundlegendes Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar`.

## <a name="access-blobs"></a>Zugreifen auf Blobs

### <a name="access-blobs-using-azure-powershell"></a> Verwenden von Azure PowerShell

> [!NOTE]
> 
> In diesem Abschnitt wird anhand eines einfachen Beispiels gezeigt, wie Sie mit PowerShell-Befehlen auf in Blobs gespeicherte Daten zugreifen. Ein komplexeres, speziell auf HDInsight zugeschnittenes Beispiel, das mehr Funktionen verwendet, finden Sie unter [HDInsight-Tools](https://github.com/Blackmist/hdinsight-tools).

Verwenden Sie den folgenden Befehl, um die Blob-bezogenen Cmdlets aufzulisten:

```powershell 
Get-Command *blob*
```

![Liste der Blob-bezogenen PowerShell-Cmdlets.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Hochladen von Dateien

Siehe [Hochladen von Daten in HDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Herunterladen von Dateien

Mit dem folgenden Skript wird ein Blockblob in das aktuelle Verzeichnis heruntergeladen. Wechseln Sie vor der Ausführung des Skripts in ein Verzeichnis, für das Sie Schreibberechtigungen haben.

```powershell
$resourceGroupName = "<AzureResourceGroupName>"
$storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
$containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
$blob = "example/data/sample.log" # The name of the blob to be downloaded.

# Use Add-AzureAccount if you haven't connected to your Azure subscription
Connect-AzAccount 
Select-AzSubscription -SubscriptionID "<Your Azure Subscription ID>"

Write-Host "Create a context object ... " -ForegroundColor Green
$storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

Write-Host "Download the blob ..." -ForegroundColor Green
Get-AzStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

Write-Host "List the downloaded file ..." -ForegroundColor Green
cat "./$blob"
```

Bei Angabe des Ressourcengruppen- und Clusternamens können Sie den folgenden Code verwenden:

```powershell
$resourceGroupName = "<AzureResourceGroupName>"
$clusterName = "<HDInsightClusterName>"
$blob = "example/data/sample.log" # The name of the blob to be downloaded.

$cluster = Get-AzHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
$defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
$defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
$defaultStorageContainer = $cluster.DefaultStorageContainer
$storageContext = New-AzStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

Write-Host "Download the blob ..." -ForegroundColor Green
Get-AzStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force
```

#### <a name="delete-files"></a>Löschen von Dateien

```powershell
Remove-AzStorageBlob -Container $containerName -Context $storageContext -blob $blob
```

#### <a name="list-files"></a>Auflisten von Dateien

```powershell
Get-AzStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"
```

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Ausführen von Hive-Abfragen mit einem nicht definierten Speicherkonto

Dieses Beispiel zeigt, wie der Inhalt eines Ordners eines Speicherkontos aufgelistet wird, der beim Erstellungsprozess nicht definiert wurde.

```powershell
$clusterName = "<HDInsightClusterName>"

$undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
$undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

$undefinedStorageKey = Get-AzStorageKey $undefinedStorageAccount | %{ $_.Primary }

Use-AzHDInsightCluster $clusterName

$defines = @{}
$defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

Invoke-AzHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"
```

### <a name="use-azure-classic-cli"></a>Verwenden der klassischen Azure CLI

Verwenden Sie den folgenden Befehl, um die Blob-bezogenen Befehle aufzulisten:

```cli
azure storage blob
```

**Beispiel für die Verwendung der klassischen Azure-CLI zum Hochladen einer Datei**

```cli
azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Beispiel für die Verwendung der klassischen Azure-CLI zum Herunterladen einer Datei**

```cli
azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Beispiel für die Verwendung der klassischen Azure-CLI zum Löschen einer Datei**

```cli
azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Beispiel für die Verwendung der klassischen Azure-CLI zum Auflisten von Dateien**

```cli
azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>
```

## <a name="use-additional-storage-accounts"></a>Verwenden zusätzlicher Speicherkonten

Beim Erstellen eines HDInsight-Clusters geben Sie das Azure Storage-Konto an, dem Sie es zuordnen möchten. Zusätzlich zu diesem Speicherkonto können Sie während des Erstellungsprozesses oder nach der Erstellung eines Clusters weitere Speicherkonten aus dem gleichen oder einem anderen Azure-Abonnement hinzufügen. Informationen zum Hinzufügen zusätzlicher Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie Sie HDFS-kompatiblen Azure Storage mit HDInsight verwenden. Dadurch können Sie skalierbare Datenerfassungslösungen mit langfristiger Archivierung aufbauen und HDInsight verwenden, um die Informationen innerhalb der gespeicherten strukturierten und unstrukturierten Daten zu entsperren.

Weitere Informationen finden Sie unter

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Erste Schritte mit Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Apache Hive mit HDInsight][hdinsight-use-hive]
* [Verwenden von Apache Pig mit HDInsight][hdinsight-use-pig]
* [Verwenden von Azure Storage Shared Access Signatures zum Einschränken des Zugriffs auf Daten mit HDInsight][hdinsight-use-sas]
* [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](hdinsight-hadoop-use-data-lake-storage-gen2.md)

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

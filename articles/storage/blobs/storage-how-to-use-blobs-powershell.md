---
title: "Ausführen von Vorgängen für den Azure Blob Storage (Objektspeicher) mit PowerShell | Microsoft-Dokumentation"
description: "Tutorial: Ausführen von Vorgängen für den Azure Blob Storage (Objektspeicher) mit PowerShell"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 565bcba848de1c518b25ff4c55a9a47aaa45bfb4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="perform-azure-blob-storage-operations-with-azure-powershell"></a>Ausführen von Vorgängen für Azure Blob Storage mit Azure PowerShell

Der Azure-Blobspeicher ist ein Dienst zur Speicherung großer Mengen unstrukturierter Objektdaten, beispielsweise Text- oder Binärdaten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. In diesem Artikel werden grundlegende Vorgänge für den Azure Blob Storage wie das Hochladen, Herunterladen und Löschen von Blobs beschrieben. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Containers 
> * Hochladen von Blobs
> * Auflisten der Blobs in einem Container 
> * Herunterladen von Blobs
> * Kopieren von Blobs
> * Löschen von Blobs
> * Anzeigen und Festlegen von Metadaten und Eigenschaften eines Blobs
> * Verwalten von Sicherheitseinstellungen mit SAS

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Blobs werden immer in einen Container hochgeladen. Container ähneln Verzeichnissen auf Ihrem Computer und ermöglichen es Ihnen, Gruppen von Blobs so zu organisieren wie Dateien oder Ordner auf Ihrem Computer. Ein Speicherkonto kann über eine beliebige Anzahl von Containern verfügen. Begrenzt wird dieses nur durch den in Anspruch genommenen Speicherplatz (maximal 500 TB). 

Wenn Sie einen Container erstellen, können Sie die Zugriffsebene definieren. Mit dieser wird festgelegt, wer auf Blobs in diesem Container zugreifen kann. Blobs können beispielsweise privat sein (Zugriffsebene = `Off`). In diesem Fall kann niemand auf sie ohne eine SAS oder den Zugriffsschlüssel für das Speicherkonto zugreifen. Wenn Sie beim Erstellen des Containers die Zugriffsebene nicht angeben, ist diese standardmäßig privat.

Bilder in Ihrem Container sollten beispielsweise öffentlich zugänglich sein. Dies ist etwa dann der Fall, wenn gespeicherte Bilder auf Ihrer Website angezeigt werden sollen. Wenn Sie nun als Zugriffsebene für den Container `blob` festlegen, kann jeder Besucher der Website mithilfe einer URL, die auf das Blob in diesem Container verweist, auf das Blob zugreifen.

Zum Erstellen des Containers legen Sie zuerst den Containernamen fest. Anschließend erstellen Sie den Container und legen für die Berechtigungen den Wert „blob“ fest. Containernamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten. Weitere Regeln für das Benennen von Blobs und Containern finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Erstellen Sie nun einen neuen Container mit [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer). Legen Sie für die Zugriffsebene den öffentlichen Status fest. Der Containername im folgenden Beispiel lautet *howtoblobs*.

```powershell
$containerName = "howtoblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-into-a-container"></a>Hochladen von Blobs in einen Container

Der Azure Blob Storage unterstützt Block-, Anfüge- und Seitenblobs.  Seitenblobs sind VHD-Dateien, mit denen IaaS-VMs gesichert werden. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Dateien, die im Blob Storage gespeichert werden, sind allerdings Blockblobs. 

Rufen Sie zum Hochladen einer Datei in ein Blockblob zuerst einen Containerverweis und anschließend einen Verweis auf das Blockblob in diesem Container auf. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe von [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) Daten in das Blockblob hochladen. Wenn kein Blob vorhanden ist, wird durch diesen Vorgang eines erstellt. Andernfalls wird das vorhandene Blob überschrieben.

Im folgenden Beispiel wird das Hochladen eines Blobs in einen Container demonstriert. Legen Sie zuerst die Variablen fest, die auf dem lokalen Computer auf das Verzeichnis verweisen, in dem sich die Dateien befinden. Anschließend legen Sie eine Variable für den Namen der Datei fest, die hochgeladen werden soll. Dies ist hilfreich, wenn Sie den gleichen Vorgang wiederholt ausführen möchten. Laden Sie einige Dateien hoch, damit Ihnen beim Auflisten der Blobs im Container mehrere Einträge angezeigt werden.

In den folgenden Beispielen werden die Dateien „Image001.jpg“ und „Image002.png“ aus dem Verzeichnis D:\\_TestImages des lokalen Datenträgers in den erstellten Container hochgeladen.

```powershell
$localFileDirectory = "D:\_TestImages\"

$blobName = "Image001.jpg"
$localFile = $localFileDirectory + $blobName
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx 
```

Laden Sie eine weitere Datei hoch. 

```powershell
$blobName = "Image002.png"
$localFile = $localFileDirectory + $blobName 
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx
```

Laden Sie beliebig viele Dateien hoch, bevor Sie fortfahren.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Verwenden Sie [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) zusammen mit dem Blobnamen, der angezeigt werden sollen, um eine Liste der Blobs im Container aufzurufen.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie die Blobs auf Ihren lokalen Datenträger herunter. Legen Sie zuerst die Variable fest, die auf den lokalen Ordner verweist, in den Sie die Blobs herunterladen möchten. Legen Sie anschließend für jedes Blob, das heruntergeladen werden soll, den Namen fest, und rufen Sie [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) zum Herunterladen des Blobs auf.

Im folgenden Beispiel werden die Blobs in das Verzeichnis \\_TestImages\Downloads auf dem lokalen Datenträger kopiert. 

```powershell
# local directory to which to download the files
# example "D:\_TestImages\Downloads\"
$localTargetDirectory = "D:\_TestImages\Downloads\"

# download the first blob
$blobName = "Image001.jpg"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 

# download another blob
$blobName = "Image002.png"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 
```

## <a name="copy-blobs"></a>Kopieren von Blobs

Beim Kopieren von Blobs müssen mehrere Szenarios berücksichtigt werden. Sie haben erstens die Möglichkeit, das Blob umzubenennen und es in denselben Speicherort zu kopieren. Zweitens können Sie das Blob in ein separates Speicherkonto kopieren. Darüber hinaus können Sie drittens ein sehr großes Blob (z.B. eine VHD-Datei) in ein anderes Speicherkonto kopieren. Für jedes Szenario sind unterschiedliche Schritte erforderlich.

### <a name="simple-blob-copy"></a>Einfaches Kopieren eines Blobs

Sie können ein Blob in einem Container in ein neues Blob kopieren. Im folgenden Beispiel wird ein umbenanntes Blob in denselben Container kopiert. Sie können allerdings genauso einfach einen weiteren Container erstellen und das Blob hierhin kopieren. Mit [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) können Sie das Blob kopieren. Hierbei müssen Sie sowohl den Blobnamen auch den Containernamen von Quelle und Ziel angeben.

```powershell
$blobName = "Image002.png"
$newBlobName = "CopyOf_" + $blobName 

Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName `
  -DestBlob $newBlobName `
  -Context $ctx 

# get list of blobs to verify the new one has been created
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

### <a name="copy-a-blob-to-a-different-storage-account"></a>Kopieren eines Blobs in ein anderes Speicherkonto 

In einigen Fällen sollten Sie ein Blob in ein separates Speicherkonto kopieren. Dies ist z.B. dann sinnvoll, wenn Sie eine VHD-Datei, mit der eine VM gesichert wird, in ein anderes Speicherkonto kopieren, um eine Sicherungskopie der Datei zu erstellen. 

Richten Sie zuerst ein zweites Speicherkonto ein. Anschließen rufen Sie den Kontext ab, richten einen Container in diesem Speicherkonto ein und führen den Kopiervorgang aus. Der folgende Skriptteil ist fast identisch mit dem vorherigen Skript. Der Unterschied besteht darin, dass das zweite Speicherkonto anstelle des ersten verwendet wird.

```powershell
#create new storage acount, get context 
$storageAccount2Name = "blobstutorialtestcopy"
$storageAccount2 = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccount2Name `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx2 = $storageAccount2.Context

#create a container in the second storage account 
$containerName2 = "tutorialblobscopied"
New-AzureStorageContainer -Name $containerName2 `
  -Context $ctx2 `
  -Permission blob

# copy one of the blobs from the first storage account to the second one 
# specify the source and destination container, blob name, and context
Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -SrcContext $ctx `
  -DestContext $ctx2 

# list the blobs in the container in the second storage account
# to verify that the newly copied blob is there
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

### <a name="copying-very-large-blobs-asynchronously"></a>Asynchrones Kopieren sehr großer Blobs

Wenn Sie ein sehr großes Blob mit mehreren GB kopieren, können Sie den asynchronen Kopiervorgang verwenden. Hierzu starten Sie den Kopiervorgang und fragen regelmäßig den Status ab, bis der Vorgang beendet ist. Auf diese Weise vermeiden Sie eine Auslastung während des Kopiervorgangs.

Wenn Sie in einem Speicherkonto Kopien anfertigen, nimmt der Kopiervorgang sehr wenig Zeit in Anspruch. Wenn Sie innerhalb einer Region eine Kopie von einem Speicherkonto auf ein anderes übertragen, dauert der Kopiervorgang zwar etwas länger, ist aber immer noch effizient. Wenn die Quelle und das Ziel hingegen in separaten Bereichen sind, kann der Kopiervorgang je nach Distanz zwischen Quelle und Ziel und Dateigröße mehrere Stunden in Anspruch nehmen. 

Im folgenden Skript werden die gleichen Variablen wie im letzten Kopierbeispiel definiert. Der Unterschied besteht darin, dass in diesem Skript der Status des Kopiervorgangs erfasst und alle 10 Sekunden bis zum Abschluss des Vorgangs abgefragt wird. Mehre Iterationen bis zum Abschluss des Vorgangs treten nur auf, wenn Sie ein sehr großes Blob in Ihr Speicherkonto hochladen.

Verwenden Sie [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/get-azurestorageblobcopystate), um den Status des Kopiervorgangs abzufragen. 

```powershell
# start the blob copy, and assign the result to $blobResult
$blobResult = Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -Context $ctx `
  -DestContext $ctx2

# get the status of the blob copy
$status = $blobResult | Get-AzureStorageBlobCopyState 
# show the value of the status
$status 

# loop until it finishes copying, pausing for 10 seconds after each iteration
# it is finished when the status is no longer 'Pending'
while ($status.Status -eq "Pending") {
    $status = $blobResult | Get-AzureStorageBlobCopyState 
    $status
    Start-Sleep 10
}

# get the list of blobs to see the new file in the second storage account 
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

## <a name="delete-blobs"></a>Löschen von Blobs

Verwenden Sie zum Löschen von Blobs aus einem Speicherkonto [Remove-AzureStorageBlob](/powershell/module/azure.storage/Remove-AzureStorageBlob). 

```powershell
$blobName = "Image001.jpg"
Remove-AzureStorageBlob -Blob $blobName -Container $containerName -Context $ctx

# get list of blobs to see the one you deleted is gone
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

## <a name="read-and-write-a-blobs-properties-and-metadata"></a>Lesen und Schreiben von Eigenschaften und Metadaten eines Blobs

Damit Sie auf die Eigenschaften und Methoden eines zurückgegebenen **IListBlobItem**-Objekts zugreifen können, müssen Sie es in ein **CloudBlockBlob**-, **CloudPageBlob**- oder **CloudBlobDirectory**-Objekt umwandeln oder konvertieren. Wenn der Typ unbekannt ist, können Sie eine Typenüberprüfung durchführen, um zu bestimmen, in welchen Typ die Umwandlung erfolgen soll. Im folgenden Codebeispiel wird gezeigt, wie Sie die Eigenschaften eines Blobs, das zuvor hochgeladen wurde, mithilfe von [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) abrufen und ausgeben und das Ergebnis anschließend in ein CloudBlockBlob-Objekt umwandeln.

```powershell
# blob properties
# get a reference to the blob you uploaded -- this is an **IListBlobItem** -- then
# convert it to a CloudBlockBlob, giving you access to the properties 
# and methods of the blob 
$blobName = "Image001.jpg"
$blob = Get-AzureStorageBlob -Context $ctx `
   -Container $containerName `
   -Blob $blobName 

#convert $blob to a CloudBlockBlob object
$cloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $blob.ICloudBlob

# you can view the properties by typing in $cloudBlockBlob 
#   and hitting the period key; this brings up IntelliSense,
#   which shows you all of the available properties
Write-Host "blob type = " $cloudBlockBlob.BlobType
Write-Host "blob name = " $cloudBlockBlob.Name
Write-Host "blob uri = " $cloudBLockBlob.Uri
```

Füllen Sie durch den Aufruf von FetchAttributes die Systemeigenschaften auf, und lassen Sie sich diese anzeigen. Für einige Eigenschaften sind Schreibzugriffe zulässig. Dies bedeutet, dass Sie die Werte der Eigenschaften ändern können. Im folgenden Beispiel wird demonstriert, wie Sie den Inhaltstyp, der häufig auch als MIME-Typ bezeichnet wird, ändern können.

```powershell
# populate the system properties
$cloudBlockBlob.FetchAttributes()

# view some of the system properties
# contentType is commonly referred to as MIME type
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
Write-Host "size = " $cloudBlockBlob.Properties.Length 

# change the content type to image/png
$contentType = "image/jpg"
$cloudBlockBlob.Properties.ContentType = $contentType 
$cloudBlockBlob.SetProperties() 

# get the system properties again to show that the content type has changed
$cloudBlockBlob.FetchAttributes()
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
```

Für jedes Blob können Sie eigene Metadaten festlegen. Beispielsweise ist es möglich, den Namen des Benutzers, der das Blob hochgeladen hat, oder Datum und Uhrzeit des Uploads speichern. Die Metadaten bestehen aus Schlüssel-Wert-Paaren. Mit PowerShell können Sie diese Daten wie folgt ändern:

```powershell
# "filename" is the key, "original file name" is the value
$cloudBlockBlob.Metadata["filename"] = "original file name"

# "owner" is the key, "RobinS" is the value
$cloudBlockBlob.Metadata["owner"] = "RobinS"

# save the metadata and then display it
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata

# clear the metadata, save it, and show it
$cloudBlockBlob.Metadata.Clear()
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata
```

## <a name="managing-security-for-blobs"></a>Verwalten der Sicherheit für Blobs 

Standardmäßig sichert Azure Storage Ihre Daten, indem der Zugriff auf den Kontobesitzer beschränkt wird, der im Besitz der Zugriffsschlüssel für das Speicherkonto ist. Wenn Sie Blobdaten in Ihrem Speicherkonto freigeben müssen, ist es wichtig, dass die Sicherheit Ihrer Kontozugriffsschlüssel dadurch nicht beeinträchtigt wird. Um dies zu gewährleisten, können Sie eine SAS-URL verwenden. Hierbei handelt es sich um eine URL zu einer Ressource, die Abfrageparameter und ein Sicherheitstoken enthält, mit dem eine spezielle Berechtigungsebene für einen bestimmten Zeitraum festgelegt werden kann. Beispielsweise können Sie den Lesezugriff auf ein privates Blob auf fünf Minuten beschränken, sodass ein Benutzer es sich nur in diesem Zeitraum anzeigen lassen kann. 

### <a name="set-the-access-level-of-the-container-and-its-blobs-to-private"></a>Festlegen der privaten Zugriffsebene für Container und zugehörige Blobs

Legen Sie für die Zugriffsebene des Containers den Wert `Off` fest. Hierdurch wird kenntlich gemacht, dass der Zugriff nur mit einer SAS oder einem Kontoschlüssel erfolgen kann. Verwenden Sie hierzu [Set-AzureStorageContainerAcl](/powershell/module/azure.storage/Set-AzureStorageContainerAcl).

```powershell
Set-AzureStorageContainerAcl -Name $containerName -Context $ctx -Permission Off 
```

### <a name="test-private-access"></a>Testen des privaten Zugriffs

Erstellen Sie eine URL zu einem der Blobs ohne eine SAS, und versuchen Sie, sich das Blob anzeigen zu lassen. So stellen Sie sicher, dass Sie keinen Zugriff auf die Blobs im Container haben. Durch die Verwendung des HTTPS-Protokoll wird die URL im folgenden Format angezeigt:

    `https://storageaccountname.blob.core.windows.net/containername/blobname`

Im folgenden Beispiel wird demonstriert, wie Sie die Blob-URL erstellen.

```powershell
$blobUrl = "https://" `
  $storageAccountName ".blob.core.windows.net/" + `
  $containerName +  "/" $blobName

Write-Host "Blob URL = " $blobUrl 
```

Kopieren Sie die Blob-URL, und fügen Sie sie in ein privates Browserfenster ein. Da das Blob privat ist und Sie keine SAS hinzugefügt haben, wird ein Autorisierungsfehler angezeigt. 

### <a name="create-the-sas-uri"></a>Erstellen des SAS-URI

Erstellen Sie einen SAS-URI. Hierbei handelt es sich um den Link zum Blob einschließlich der Abfrageparameter und des Sicherheitstokens, die zusammen die SAS bilden. Fügen Sie diesen URI in ein privates Browserfenster ein. In diesem wird das Bild angezeigt. 

Erstellen Sie zunächst für den Start und Ablauf Angaben zu Datum und Uhrzeit, mit denen der Zugriff eingeschränkt wird. Im folgenden Beispiel wird ein Zeitfenster von zwei Minuten festgelegt. 

```powershell
# set the start time to the current date/time 
# set the end time to 2 minutes in the future
$StartTime = Get-Date
$EndTime = $StartTime.AddMinutes(2.0)
```

Erstellen Sie nun den SAS-URI mit [New-AzureStorageBlobSASToken](/powershell/module/azure.storage/new-azurestorageblobsastoken). Hierfür benötigen Sie den Container- und Blobnamen, den Speicherkontokontext, die zu vergebenden Berechtigungen (in diesem Fall für das Lesen, Schreiben und Löschen) sowie die Start- und Endzeit für den Zugriff. 

```powershell
$SASURI = New-AzureStorageBlobSASToken -Container $containerName `
    -Blob $blobName `
    -Context $ctx `
    -Permission "rwd" `
    -StartTime $StartTime `
    -ExpiryTime $EndTime `
    -FullUri

Write-Host "URL with SAS = " $SASURI
```

Kopieren Sie den SAS-URI, und fügen Sie ihn in ein privates Browserfenster ein. In diesem wird das Bild angezeigt.

Warten Sie, bis die URL abgelaufen ist (in diesem Beispiel zwei Minuten), und fügen die URL in ein anderes InPrivate-Browserfenster ein. Nun wird Ihnen eine Autorisierungsfehlermeldung angezeigt. Das Bild wird nicht geladen, da der SAS-URI abgelaufen ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie alle erstellten Ressourcen. Hierzu können Sie die Ressourcengruppe entfernen, wodurch alle Ressourcen innerhalb der Gruppe gelöscht werden. In diesem Fall werden alle Speicherkonten und die Ressourcengruppe selbst entfernt.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zur grundlegenden Verwaltung des Blob Storage erhalten. Folgende Schritte wurden erläutert:

> [!div class="checklist"]
> * Erstellen eines Containers 
> * Hochladen von Blobs
> * Auflisten der Blobs in einem Container 
> * Herunterladen von Blobs
> * Kopieren von Blobs
> * Löschen von Blobs
> * Löschen und Schreiben von Metadaten und Eigenschaften eines Blobs
> * Verwalten von Sicherheitseinstellungen mit SAS

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell Storage-Cmdlets
* [Storage PowerShell-Cmdlets](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure-Speicher-Explorer
* Beim [Microsoft Azure-Speicher-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
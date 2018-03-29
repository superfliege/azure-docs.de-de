---
title: Azure-Schnellstart – Hochladen, Herunterladen und Auflisten von Blobs in Azure Storage mit Azure PowerShell | Microsoft-Dokumentation
description: In diesem Schnellstart verwenden Sie Azure PowerShell, um ein Speicherkonto und einen Container zu erstellen. Anschließend verwenden Sie PowerShell, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: rogarana
ms.openlocfilehash: 29eb69f95680babfc1004d483a6a1c2bc44edda8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-azure-powershell"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit Azure PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Anleitung wird ausgeführt, wie Sie Dateien mit PowerShell zwischen der lokalen Festplatte und Azure Blob Storage übertragen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Blobs werden immer in einen Container hochgeladen. Sie können Gruppen von Blobs ähnlich wie Dateien in Ordnern auf Ihrem Computer organisieren.

Legen Sie den Containernamen fest, und erstellen Sie dann mit [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) den Container, wobei Sie die Berechtigungen auf „blob“ festlegen, um den öffentlichen Zugriff auf die Dateien zuzulassen. Der Containername in diesem Beispiel lautet *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Seitenblobs sind VHD-Dateien, mit denen IaaS-VMs gesichert werden. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Dateien, die im Blob Storage gespeichert werden, sind allerdings Blockblobs. 

Rufen Sie zum Hochladen einer Datei in ein Blockblob zuerst einen Containerverweis und anschließend einen Verweis auf das Blockblob in diesem Container auf. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe von [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) Daten in das Blockblob hochladen. Wenn noch kein Blob vorhanden ist, wird durch diesen Vorgang eines erstellt. Andernfalls wird das vorhandene Blob überschrieben.

In den folgenden Beispielen werden die Dateien „Image001.jpg“ und „Image002.png“ aus dem Ordner „D:\\_TestImages“ des lokalen Datenträgers in den erstellten Container hochgeladen.

```powershell
# upload a file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Laden Sie beliebig viele Dateien hoch, bevor Sie fortfahren.

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Verwenden Sie [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob), um eine Liste der Blobs im Container abzurufen. In diesem Beispiel werden nur die Namen der hochgeladenen Blobs angezeigt.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie die Blobs auf Ihren lokalen Datenträger herunter. Legen Sie für jedes Blob, das heruntergeladen werden soll, den Namen fest, und rufen Sie [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) zum Herunterladen des Blobs auf.

Im folgenden Beispiel werden die Blobs in das Verzeichnis „D:\\_TestImages\Downloads“ auf dem lokalen Datenträger heruntergeladen. 

```powershell
# download first blob
Get-AzureStorageBlobContent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
Get-AzureStorageBlobContent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 
```

## <a name="data-transfer-with-azcopy"></a>Übertragen von Daten mit AzCopy

Das Dienstprogramm [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ist eine weitere Option für eine leistungsstarke, skriptfähige Datenübertragung für Azure Storage. Sie können AzCopy zum Übertragen von Daten in und aus Blob-, Datei- und Tabellenspeicher verwenden.

Als kurzes Beispiel sehen Sie hier den AzCopy-Befehl für das Hochladen der Datei *myfile.txt* in den Container *mystoragecontainer* in einem PowerShell-Fenster.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie alle Objekte, die Sie erstellt haben. Am Einfachsten löschen Sie hierzu die Ressourcengruppe. Dabei werden auch alle in der Gruppe enthaltenen Ressourcen gelöscht. In diesem Fall werden das Speicherkonto und die Ressourcengruppe selbst entfernt.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie Dateien zwischen einer lokalen Festplatte und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Referenz zu Microsoft Azure PowerShell Storage-Cmdlets
* [Storage PowerShell-Cmdlets](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage-Explorer
* Beim [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
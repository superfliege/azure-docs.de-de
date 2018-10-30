---
title: 'Schnellstart: Verwalten von Azure-Dateifreigaben mit Azure PowerShell'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure-Dateifreigaben mit Azure PowerShell verwalten.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 16f557d48f8056d438d55fdd066395e7e36ed8a5
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945483"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Schnellstart: Erstellen und Verwalten einer Azure-Dateifreigabe mit Azure PowerShell 
In dieser Anleitung werden Schritt für Schritt die Grundlagen der Verwendung von [Azure-Dateifreigaben](storage-files-introduction.md) mit PowerShell beschrieben. Azure-Dateifreigaben sind genau wie andere Dateifreigaben, werden jedoch in der Cloud gespeichert und von der Azure-Plattform unterstützt. Azure-Dateifreigaben unterstützen das SMB-Protokoll nach Industriestandard und ermöglichen es, Dateien für mehrere Computer, Anwendungen und Instanzen freizugeben. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für diese Anleitung mindestens Version 5.1.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um zu ermitteln, welche Version des Azure PowerShell-Moduls Sie ausführen. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um sich bei Ihrem Azure-Konto anzumelden.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Falls Sie nicht bereits über eine Azure-Ressourcengruppe verfügen, können Sie mit dem [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)-Cmdlet eine neue erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region „East US“ (USA, Osten) erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen von Azure-Dateifreigaben oder anderen Speicherressourcen wie Blobs oder Warteschlangen verwenden können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze des Speicherkontos erreicht ist.

In diesem Beispiel wird mithilfe des Cmdlets [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) ein Speicherkonto erstellt. Das Speicherkonto wird mit dem Namen *mystorageaccount<random number>* versehen, und ein Verweis auf das Speicherkonto wird in der Variablen **$storageAcct** gespeichert. Da Speicherkontonamen eindeutig sein müssen, sollten Sie `Get-Random` verwenden, um zu diesem Zweck eine Zahl an den Namen anzufügen. 

```azurepowershell-interactive 
$storageAcct = New-AzureRmStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Jetzt können Sie Ihre erste Azure-Dateifreigabe erstellen. Für die Erstellung einer Dateifreigabe können Sie das [New-AzureStorageShare](/powershell/module/azurerm.storage/new-azurestorageshare)-Cmdlet verwenden. In diesem Beispiel wird eine Freigabe mit dem Namen `myshare` erstellt.

```azurepowershell-interactive
New-AzureStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten und dürfen nicht mit einem Bindestrich beginnen. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="use-your-azure-file-share"></a>Verwenden Ihrer Azure-Dateifreigabe
Azure Files bietet zwei Methoden für die Arbeit mit Dateien und Ordnern in Ihrer Azure-Dateifreigabe: das [SMB-Protokoll (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) nach Industriestandard und das [REST-Protokoll „File“](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Informationen zum Einbinden einer Dateifreigabe mit SMB finden Sie im folgenden Dokument für Ihr jeweiliges Betriebssystem:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Verwenden einer Azure-Dateifreigabe mit dem REST-Protokoll „File“ 
Das REST-Protokoll „File“ kann direkt verwendet werden (d.h. Sie können REST-HTTP-Aufrufe selbst erstellen). Gängiger ist jedoch die Verwendung des AzureRM PowerShell-Moduls, der [Azure-Befehlszeilenschnittstelle](storage-how-to-use-files-cli.md) oder eines Azure Storage SDK. Bei allen diesen Methoden steht ein praktischer Wrapper um das REST-Protokoll „File“ in der Skript-/Programmiersprache Ihrer Wahl zur Verfügung.  

In den meisten Fällen werden Sie ihre Azure-Dateifreigabe über das SMB-Protokoll nutzen, da Sie so die bereits vorhandenen Anwendungen und Tools verwenden können, die sie erwarten. Es gibt jedoch auch Gründe, die für die Verwendung der REST-API „File“ sprechen:

- Sie durchsuchen Ihre Dateifreigabe per Cloud Shell in PowerShell. (In diesem Fall können Dateifreigaben nicht über SMB eingebunden werden.)
- Sie müssen ein Skript oder eine Anwendung über einen Client ausführen, der keine SMB-Freigaben einbinden kann (beispielsweise ein lokaler Client, für den die Blockierung von Port 445 nicht aufgehoben wurde).
- Sie nutzen serverlose Ressourcen (beispielsweise [Azure Functions](../../azure-functions/functions-overview.md)). 

Die folgenden Beispiele zeigen, wie Sie mit dem AzureRM PowerShell-Modul Ihre Azure-Dateifreigabe mit dem REST-Protokoll „File“ ändern. 

#### <a name="create-directory"></a>Erstellen eines Verzeichnisses
Verwenden Sie das [New-AzureStorageDirectory](/powershell/module/azurerm.storage/new-azurestoragedirectory)-Cmdlet, um im Stammverzeichnis Ihrer Azure-Dateifreigabe ein neues Verzeichnis mit dem Namen *myDirectory* zu erstellen.

```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Hochladen einer Datei
Um das Hochladen einer Datei mit dem [Set-AzureStorageFileContent](/powershell/module/azure.storage/set-azurestoragefilecontent)-Cmdlet demonstrieren zu können, müssen wir zuerst auf Ihrem temporären Laufwerk der Cloud Shell von PowerShell eine Datei erstellen, die hochgeladen werden kann. 

In diesem Beispiel werden das aktuelle Datum und die Uhrzeit in eine neue Datei auf Ihrem temporären Laufwerk eingefügt, und anschließend wird die Datei auf die Dateifreigabe hochgeladen.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzureStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

Beim lokalen Ausführen von PowerShell sollten Sie `C:\Users\ContainerAdministrator\CloudDrive\` durch einen Pfad ersetzen, der auf Ihrem Computer vorhanden ist.

Nach dem Hochladen der Datei können Sie das [Get-AzureStorageFile](/powershell/module/Azure.Storage/Get-AzureStorageFile)-Cmdlet zum Überprüfen verwenden, um sicherzustellen, dass die Datei auf Ihre Azure-Dateifreigabe hochgeladen wurde. 

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Herunterladen einer Datei
Sie können das [Get-AzureStorageFileContent](/powershell/module/azure.storage/get-azurestoragefilecontent)-Cmdlet verwenden, um eine Kopie der Datei herunterzuladen, die Sie gerade auf das temporäre Laufwerk Ihrer Cloud Shell hochgeladen haben.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item 
    `-Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzureStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Nach dem Herunterladen der Datei können Sie `Get-ChildItem` verwenden, um zu prüfen, ob die Datei auf das temporäre Laufwerk der Cloud Shell von PowerShell heruntergeladen wurde.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

#### <a name="copy-files"></a>Kopieren von Dateien
Eine häufige Aufgabe besteht darin, Dateien von einer Dateifreigabe auf eine andere oder in bzw. aus einem Azure Blob-Speichercontainer zu kopieren. Zum Demonstrieren dieser Funktionalität können Sie eine neue Freigabe erstellen und die Datei, die Sie gerade hochgeladen haben, mit dem [Start-AzureStorageFileCopy](/powershell/module/azure.storage/start-azurestoragefilecopy)-Cmdlet auf diese neue Freigabe kopieren. 

```azurepowershell-interactive
New-AzureStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzureStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Wenn Sie die Dateien auf der neuen Freigabe auflisten, sollte die kopierte Datei angezeigt werden.

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Das `Start-AzureStorageFileCopy`-Cmdlet eignet sich zwar gut für das Verschieben von Ad-hoc-Dateien zwischen Azure-Dateifreigaben und Azure Blob Storage-Containern, aber für umfangreichere Verschiebungen (in Bezug auf die Anzahl oder Größe von Dateien) empfehlen wir die Verwendung von AzCopy. Informieren Sie sich über [AzCopy für Windows](../common/storage-use-azcopy.md) und [AzCopy für Linux](../common/storage-use-azcopy-linux.md). AzCopy muss lokal installiert werden und ist in Cloud Shell nicht verfügbar. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie fertig sind, können Sie die Ressourcengruppe und alle zugehörigen Ressourcen mit dem [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)-Cmdlet entfernen. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Alternativ hierzu können Sie Ressourcen einzeln entfernen:

- Zum Entfernen der Azure-Dateifreigaben, die wir für diese Schnellstartanleitung erstellt haben
```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
    Remove-AzureStorageShare -Context $storageAcct.Context -Name $_.Name
}
```

- Zum Entfernen des eigentlichen Speicherkontos (Es werden implizit die von uns erstellten Azure-Dateifreigaben sowie alle anderen Speicherressourcen entfernt, die Sie ggf. erstellt haben, z.B. ein Azure Blob Storage-Container.)
```azurepowershell-interactive
Remove-AzureRmStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was ist Azure Files?](storage-files-introduction.md)
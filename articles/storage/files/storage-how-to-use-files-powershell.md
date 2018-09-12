---
title: Verwalten von Azure-Dateifreigaben mit Azure PowerShell
description: Es wird beschrieben, wie Sie Azure-Dateifreigaben mit Azure PowerShell verwalten.
services: storage
author: wmgries
ms.service: storage
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 72f320279d23dc701f045ee6db1681a355f98808
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022285"
---
# <a name="managing-azure-file-shares-with-azure-powershell"></a>Verwalten von Azure-Dateifreigaben mit Azure PowerShell 
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können unter Windows, Linux und macOS bereitgestellt werden. In dieser Anleitung werden die Grundlagen der Arbeit mit Azure-Dateifreigaben mit PowerShell Schritt für Schritt beschrieben. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei 
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für diese Anleitung mindestens Version 5.1.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um zu ermitteln, welche Version des Azure PowerShell-Moduls Sie ausführen. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Falls Sie nicht bereits über eine Azure-Ressourcengruppe verfügen, können Sie mit dem [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)-Cmdlet eine neue erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region „East US“ (USA, Osten) erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen von Azure-Dateifreigaben oder anderen Speicherressourcen wie Blobs oder Warteschlangen verwenden können. Ein Speicherkonto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze des Speicherkontos erreicht ist.

In diesem Beispiel wird ein Speicherkonto mit dem Namen `mystorageacct<random number>` erstellt und ein Verweis auf das Speicherkonto in die Variable **$storageAcct** eingefügt. Da Speicherkontonamen eindeutig sein müssen, sollten Sie `Get-Random` verwenden, um zu diesem Zweck eine Zahl an den Namen anzufügen. 

```azurepowershell-interactive 
$storageAcct = New-AzureRmStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Jetzt können Sie Ihre erste Azure-Dateifreigabe erstellen. Für die Erstellung einer Dateifreigabe können Sie das [New-AzureStorageShare](https://docs.microsoft.com/powershell/module/servicemanagement/azure.storage/new-azurestorageshare)-Cmdlet verwenden. In diesem Beispiel wird eine Freigabe mit dem Namen `myshare` erstellt.

```azurepowershell-interactive
New-AzureStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

> [!Important]  
> Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten und dürfen nicht mit einem Bindestrich beginnen. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="work-with-the-contents-of-the-azure-file-share"></a>Bearbeiten des Inhalts der Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe mit SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie für die Arbeit mit Ihrer Azure-Dateifreigabe auch das Azure PowerShell-Modul verwenden. Dies ist vorteilhafter als das Bereitstellen der Dateifreigabe mit SMB, da alle mit PowerShell gesendeten Anforderungen mit der Datei-REST-API durchgeführt werden, sodass Sie Dateien und Verzeichnisse auf Ihrer Dateifreigabe wie folgt erstellen, ändern und löschen können:

- Cloud Shell von PowerShell (Bereitstellung von Dateifreigaben per SMB ist nicht möglich).
- Clients, die keine SMB-Freigaben bereitstellen können, z.B. lokale Clients, für die die Blockierung von Port 445 nicht aufgehoben wurde.
- Serverlose Szenarien, z.B. wie bei [Azure Functions](../../azure-functions/functions-overview.md). 


### <a name="create-directory"></a>Erstellen eines Verzeichnisses
Verwenden Sie das [New-AzureStorageDirectory](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragedirectory)-Cmdlet, um im Stammverzeichnis Ihrer Azure-Dateifreigabe ein neues Verzeichnis mit dem Namen *myDirectory* zu erstellen.


```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

### <a name="upload-a-file"></a>Hochladen einer Datei
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

### <a name="download-a-file"></a>Herunterladen einer Datei
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

### <a name="copy-files"></a>Kopieren von Dateien
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

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Eine weitere nützliche Aufgabe, die Sie mit einer Azure-Dateifreigabe durchführen können, ist die Erstellung von Freigabemomentaufnahmen. Mit einer Momentaufnahme wird für eine Azure-Dateifreigabe ein bestimmter Zeitpunkt beibehalten. Freigabemomentaufnahmen ähneln Betriebssystemtechnologien, mit denen Sie unter Umständen bereits vertraut sind:
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) für Windows-Dateisysteme wie NTFS und ReFS
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS 

Sie können eine Freigabemomentaufnahme für eine Freigabe erstellen, indem Sie die `Snapshot`-Methode im PowerShell-Objekt für eine Dateifreigabe verwenden, die mit dem [Get-AzureStorageShare](/powershell/module/azure.storage/get-azurestorageshare)-Cmdlet abgerufen wird. 

```azurepowershell-interactive
$share = Get-AzureStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Durchsuchen von Freigabemomentaufnahmen
Sie können den Inhalt der Freigabemomentaufnahme durchsuchen, indem Sie den Momentaufnahmenverweis (`$snapshot`) an den Parameter `-Share` des `Get-AzureStorageFile`-Cmdlets übergeben.

```azurepowershell-interactive
Get-AzureStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Auflisten von Freigabemomentaufnahmen
Sie können die Liste mit den Momentaufnahmen, die Sie für Ihre Freigabe erstellt haben, mit dem folgenden Befehl anzeigen.

```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Sie können eine Datei wiederherstellen, indem Sie den Befehl `Start-AzureStorageFileCopy` verwenden, den wir bereits genutzt haben. In dieser Schnellstartanleitung löschen wir zuerst unsere Datei `SampleUpload.txt`, die wir hochgeladen haben, damit wir sie aus der Momentaufnahme wiederherstellen können.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzureStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"

# Restore SampleUpload.txt from the share snapshot
Start-AzureStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme
Sie können eine Freigabemomentaufnahme löschen, indem Sie das [Remove-AzureStorageShare](/powershell/module/azure.storage/remove-azurestorageshare)-Cmdlet mit der Variablen verwenden, die den Verweis `$snapshot` auf den Parameter `-Share` enthält.

```azurepowershell-interactive
Remove-AzureStorageShare -Share $snapshot
```

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
- [Verwalten von Dateifreigaben mit dem Azure-Portal](storage-how-to-use-files-portal.md)
- [Verwalten von Dateifreigaben mit der Azure CLI](storage-how-to-use-files-cli.md)
- [Verwalten von Dateifreigaben mit Storage-Explorer](storage-how-to-use-files-storage-explorer.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)

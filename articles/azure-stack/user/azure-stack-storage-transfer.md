---
title: Tools für Azure Stack-Speicher | Microsoft-Dokumentation
description: Erfahren Sie mehr über Datenübertragungstools für Azure Stack-Speicher
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 3d9bd187a70e8b8292e9c47497c2c6b13764045d
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604725"
---
# <a name="use-data-transfer-tools-for-azure-stack-storage"></a>Verwenden von Datenübertragungstools für Azure Stack-Speicher

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Microsoft Azure Stack bietet eine Reihe von Speicherdiensten für Datenträger, Blobs, Tabellen und Warteschlangen sowie Funktionen zur Kontoverwaltung. Sie können eine Reihe von Azure-Speichertools verwenden, um Daten zu verwalten oder in bzw. aus Azure Stack-Speicher zu verschieben. Dieser Artikel enthält eine Übersicht über die verfügbaren Tools.

Ihre Anforderungen bestimmen, welche der folgenden Tools sich am besten für Sie eignen:

* [AzCopy](#azcopy)

    Ein speicherspezifisches Befehlszeilenprogramm, das Sie herunterladen können, um Daten zwischen Objekten innerhalb Ihres Speicherkontos oder zwischen Speicherkonten zu kopieren.

* [Azure PowerShell](#azure-powershell)

    Eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die speziell für die Systemverwaltung entwickelt wurde.

* [Azure-CLI](#azure-cli)

    Ein plattformübergreifendes Tool auf Open-Source-Basis, das eine Reihe von Befehlen zum Arbeiten mit der Azure- und Azure Stack-Plattform bietet.

* [Microsoft Storage-Explorer](#microsoft-azure-storage-explorer)

    Eine benutzerfreundliche eigenständige App mit einer Benutzeroberfläche.

Aufgrund der Speicherdienstunterschiede zwischen Azure und Azure Stack unterscheiden sich möglicherweise die besonderen Anforderungen der in den folgenden Abschnitten beschriebenen Tools. Einen Vergleich zwischen Azure Stack-Speicher und Azure-Speicher finden Sie unter [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md).

## <a name="azcopy"></a>AzCopy

AzCopy ist ein Befehlszeilenprogramm zum Kopieren von Daten in bzw. aus Microsoft Azure Blob und Table Storage. Hierzu werden einfache Befehle mit optimaler Leistung verwendet. Sie können Daten von einem Objekt zu einem anderen innerhalb Ihres Speicherkontos oder von einem Speicherkonto zu einem anderen kopieren.

### <a name="download-and-install-azcopy"></a>Herunterladen und Installieren von AzCopy

AzCopy ist in zwei Versionen verfügbar: AzCopy unter Windows und AzCopy unter Linux.

 - **AzCopy unter Windows**
    - Laden Sie die unterstützte Version von AzCopy für Azure Stack herunter. Sie können AzCopy unter Azure Stack wie unter Azure installieren und verwenden. Weitere Informationen finden Sie unter [Übertragen von Daten mit AzCopy unter Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).
        - Laden Sie für das Update 1802 oder neuere Versionen [AzCopy 7.1.0](https://aka.ms/azcopyforazurestack20170417) herunter.
        - Laden Sie für vorherige Versionen [AzCopy 5.0.0](https://aka.ms/azcopyforazurestack20170417) herunter.

 - **AzCopy unter Linux**

    - AzCopy unter Linux unterstützt das Azure Stack-Update 1802 und neuere Versionen. Sie können AzCopy unter Azure Stack wie unter Azure installieren und verwenden. Weitere Informationen finden Sie unter [Übertragen von Daten mit AzCopy unter Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="azcopy-command-examples-for-data-transfer"></a>AzCopy-Beispielbefehle für die Datenübertragung

Die folgenden Beispiele behandeln typische Szenarien für das Kopieren von Daten in und aus Azure Stack-Blobs. Weitere Informationen finden Sie unter [Übertragen von Daten mit AzCopy unter Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) bzw. unter [Übertragen von Daten mit AzCopy unter Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="download-all-blobs-to-a-local-disk"></a>Herunterladen aller Blobs auf einen lokalen Datenträger

**Windows**

````AzCopy
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
````

**Linux**

````AzCopy
azcopy \
    --source https://myaccount.blob.local.azurestack.external/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
````

### <a name="upload-single-file-to-virtual-directory"></a>Hochladen einer einzelnen Datei in ein virtuelles Verzeichnis

**Windows**

```AzCopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

**Linux**

````AzCopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.local.azurestack.external/mycontainer/vd/abc.txt \
    --dest-key <key>
````

### <a name="move-data-between-azure-and-azure-stack-storage"></a>Verschieben von Daten zwischen Azure- und Azure Stack-Speicher

Eine asynchrone Datenübertragung zwischen Azure-Speicher und Azure Stack wird nicht unterstützt. Sie müssen die Übertragung mit der Option **/SyncCopy** oder **--sync-copy** angeben.

**Windows**

````AzCopy
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
````

**Linux**

````AzCopy
azcopy \
    --source https://myaccount1.blob.local.azurestack.external/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --sync-copy
````

### <a name="azcopy-known-issues"></a>Bekannte Probleme mit AzCopy

 - AzCopy-Vorgänge für Dateispeicher sind nicht verfügbar, da Dateispeicher noch nicht in Azure Stack verfügbar ist.
 - Eine asynchrone Datenübertragung zwischen Azure-Speicher und Azure Stack wird nicht unterstützt. Sie können die Übertragung mit der Option **/SyncCopy** angeben, um die Daten zu kopieren.
 - Die Linux-Version von AzCopy unterstützt nur das Update 1802 und höhere Versionen. Außerdem unterstützt sie den Tabellenspeicherdienst nicht.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell ist ein Modul, das Cmdlets für die Verwaltung von Diensten in Azure und Azure Stack bereitstellt. Bei diesem Modul handelt es sich um eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die speziell für die Systemverwaltung entwickelt wurde.

### <a name="install-and-configure-powershell-for-azure-stack"></a>Installieren und Konfigurieren von PowerShell für die Verwendung mit Azure Stack

Für die Verwendung von Azure Stack sind mit Azure Stack kompatible Azure PowerShell-Module erforderlich. Weitere Informationen finden Sie unter [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Installieren von PowerShell für Azure Stack) und [Configure the Azure Stack user's PowerShell environment](azure-stack-powershell-configure-user.md) (Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers).

### <a name="powershell-sample-script-for-azure-stack"></a>PowerShell-Beispielskript für Azure Stack 

In diesem Beispiel wird davon ausgegangen, dass Sie die [Installation von PowerShell für Azure Stack](azure-stack-powershell-install.md) erfolgreich durchgeführt haben. Dieses Skript unterstützt Sie beim Abschluss der Konfiguration und fordert Sie zur Eingabe Ihrer Azure Stack-Mandantenanmeldeinformationen auf, um Ihr Konto der lokalen PowerShell-Umgebung hinzuzufügen. Als Nächstes legt das Skript das standardmäßige Azure-Abonnement fest, erstellt ein neues Speicherkonto in Azure, erstellt in dem neuen Speicherkonto einen neuen Container und lädt eine vorhandene Image-Datei (Blob) in diesen Container hoch. Nachdem das Skript alle Blobs in diesem Container aufgelistet hat, erstellt es ein neues Zielverzeichnis auf dem lokalen Computer und lädt dann die Image-Datei herunter.

1. Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).
2. Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).
3. Öffnen Sie **Windows PowerShell ISE**, wählen Sie **Als Administrator ausführen**, und klicken Sie auf **Datei** > **Neu**, um eine neue Skriptdatei zu erstellen.
4. Kopieren Sie das folgende Skript, und fügen Sie es in die neue Skriptdatei ein.
5. Aktualisieren Sie die Skriptvariablen auf Basis der Konfigurationseinstellungen.
   > [!NOTE]
   > Dieses Skript muss im Stammverzeichnis für **AzureStack_Tools** ausgeführt werden.

```PowerShell  
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environemnt name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudiance = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase.
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\downlaod" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudience

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Add-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder

# end
````

### <a name="powershell-known-issues"></a>PowerShell – Bekannte Probleme

Die kompatible Azure PowerShell-Modulversion für Azure Stack ist derzeit 1.3.0. Sie unterscheidet sich von der neuesten Version von Azure PowerShell. Dieser Unterschied wirkt sich auf den Betrieb der Speicherdienste aus:

* Das Format des Rückgabewerts von `Get-AzureRmStorageAccountKey` in Version 1.3.0 hat zwei Eigenschaften: `Key1` und `Key2`. Die aktuelle Azure-Version gibt dagegen ein Array mit allen Kontoschlüsseln zurück.

   ```
   # This command gets a specific key for a storage account, 
   # and works for Azure PowerShell version 1.4, and later versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Value[0]

   # This command gets a specific key for a storage account, 
   # and works for Azure PowerShell version 1.3.2, and previous versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Key1

   ```

   Weitere Informationen finden Sie unter [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey?view=azurermps-4.1.0).

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure CLI ist die Befehlszeilenumgebung von Azure und dient zum Verwalten von Azure-Ressourcen. Sie können sie unter macOS, Linux und Windows installieren und über die Befehlszeile ausführen.

Azure CLI ist für die Verwaltung von Azure-Ressourcen über die Befehlszeile sowie die Erstellung von Automatisierungsskripts für Azure Resource Manager optimiert. Sie bietet viele der im Azure Stack-Portal vorhandenen Funktionen, so z.B. umfangreiche Datenzugriffsfunktionen.

Azure Stack erfordert Azure CLI Version 2.0. Weitere Informationen zum Installieren und Konfigurieren von Azure CLI mit Azure Stack finden Sie unter [Install and configure CLI for the Azure Stack user's environment](azure-stack-version-profiles-azurecli2.md) (Installieren und Konfigurieren von CLI für die Azure Stack-Benutzerumgebung). Weitere Informationen zur Verwendung der Azure CLI 2.0 für einige Aufgaben in Verbindung mit Ressourcen in Ihrem Azure Stack-Speicherkonto finden Sie unter [Verwenden der Azure CLI 2.0 mit Azure Storage](../../storage/storage-azure-cli.md).

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure CLI-Beispielskript für Azure Stack

Wenn Sie die Installation und Konfiguration der CLI abgeschlossen haben, können Sie die folgenden Schritte ausprobieren, um mit einem kleinen Shell-Beispielskript für die Interaktion mit Azure Stack-Speicherressourcen zu arbeiten. Dieses Skript führt folgende Aktionen aus:

* Erstellen eines Blobcontainers in Ihrem Speicherkonto
* Hochladen einer vorhandenen Datei (als Blob) in den Container
* Auflisten aller Blobs im Container
* Herunterladen der Datei in ein von Ihnen angegebenes Ziel auf dem lokalen Computer

Stellen Sie vor der Ausführung dieses Skript sicher, dass Sie eine Verbindung mit der gewünschten Azure Stack-Instanz herstellen und sich dort anmelden können.

1. Öffnen Sie Ihren bevorzugten Texteditor, in den Sie das kopierte vorhergehende Skript einfügen.
2. Aktualisieren Sie die Skriptvariablen entsprechend Ihren Konfigurationseinstellungen.
3. Nachdem Sie die erforderlichen Variablen aktualisiert haben, speichern Sie das Skript, und beenden Sie den Editor. In den nächsten Schritten wird vorausgesetzt, dass Sie Ihr **my_storage_sample.sh** genannt haben.
4. Markieren Sie das Skript bei Bedarf als ausführbar:`chmod +x my_storage_sample.sh`
5. Führen Sie das Skript aus. Beispielsweise in Bash:`./my_storage_sample.sh`

```bash
#!/bin/bash
# A simple Azure Stack storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
````

## <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage-Explorer

Microsoft Azure Storage-Explorer ist eine eigenständige App von Microsoft. Mit ihr können Sie Daten aus Azure-Speicher und Azure Stack-Speicher einfach auf Computern unter Windows, macOS und Linux nutzen. Microsoft Azure Storage-Explorer bietet eine komfortable Verwaltung für Ihre Azure Stack-Speicherdaten.

* Weitere Informationen zum Konfigurieren von Azure Storage-Explorer für die Verwendung mit Azure Stack finden Sie unter [Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement](azure-stack-storage-connect-se.md).
* Weitere Informationen zum Microsoft Azure Storage-Explorer finden Sie unter [Erste Schritte mit dem Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md).

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement](azure-stack-storage-connect-se.md)
* [Erste Schritte mit dem Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md)
* [Einführung in Azure Storage](../../storage/common/storage-introduction.md)

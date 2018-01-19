---
title: 'Azure PowerShell-Skriptbeispiel: Migrieren von Blobs zwischen Speicherkonten mit AzCopy unter Windows | Microsoft-Dokumentation'
description: Dieses Beispiel kopiert mithilfe von AzCopy die Blobinhalte eines Azure Storage-Kontos in ein anderes Konto.
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 1/3/2018
ms.author: v-rogara
ms.openlocfilehash: 0902792b2367aa56285e7e074e184ffa35fca466
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Migrieren von Blobs zwischen Speicherkonten mit AzCopy unter Windows

Dieses Beispiel kopiert alle Blobobjekte aus einem vom Benutzer bereitgestellten Quellspeicherkonto in ein vom Benutzer bereitgestelltes Zielspeicherkonto. 

Dies erfolgt mithilfe des `Get-AzureStorageContainer`-Befehls, der alle Container in einem Speicherkonto auflistet. Danach gibt das Beispiel AzCopy-Befehle aus, um jeden Container aus dem Quellspeicherkonto in das Zielspeicherkonto zu kopieren. Im Fall eines Fehlers unternimmt das Beispiel so viele Wiederholungsversuche, wie in „$retryTimes“ festgelegt ist (der Standardwert ist 3 und kann mit dem Parameter `-RetryTimes` geändert werden). Wenn bei jedem Wiederholungsversuch ein Fehler auftritt, kann der Benutzer über den Parameter `-LastSuccessContainerName` den letzten erfolgreich kopierten Container angeben und das Skript erneut ausführen. Das Beispiel fährt dann an diesem Punkt mit dem Kopieren von Containern fort.

Für dieses Beispiel ist das Azure PowerShell-Speichermodul Version **4.0.2** oder höher erforderlich. Sie können Ihre Version mithilfe von `Get-Module -ListAvailable Azure.storage` überprüfen. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Dieses Beispiel erfordert auch die neueste Version von [AzCopy unter Windows](http://aka.ms/downloadazcopy). Das Standardinstallationsverzeichnis lautet `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`.

Dieses Beispiel verwendet den Namen und Schlüssel eines Quellspeicherkontos, den Namen und Schlüssel eines Zielspeicherkontos und den vollständigen Dateipfad der AzCopy.exe-Datei (falls die Installation nicht im Standardverzeichnis erfolgt ist).

Im Folgenden finden Sie Beispiele für die Eingabe für dieses Beispiel:

Wenn AzCopy im Standardverzeichnis installiert ist:
```PowerShell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
```

Wenn AzCopy nicht im Standardverzeichnis installiert ist:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```

Wenn ein Fehler auftritt und das Beispiel aus einem bestimmten Container erneut ausgeführt werden muss: 

`.\copyScript.ps1 -LastSuccessContainerName myContainerName`

## <a name="sample-script"></a>Beispielskript

```Powershell
# Run the script in a new open Powershell window, which has not run other cmdlets, or AzCopy performance could suffer .
# Install Azure PowerShell before runing the script: https://github.com/Azure/azure-powershell/releases
# Install AzCopy before runing the script: https://docs.microsoft.com/azure/storage/common/storage-use-azcopy
# Do not modify the Source or Destination accounts while the script is running

 param (
    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$srcStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account key.")]
    [String]$srcStorageAccountKey,
    
    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$DestStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account key.")]
    [String]$DestStorageAccountKey,

    [Parameter(Mandatory = $false, 
    HelpMessage= "Input the full filePath of the AzCopy.exe, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe")]
    [String]$AzCopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe",

    [Parameter(Mandatory = $false, 
    HelpMessage='Sets the number of retries in an event of failure. Set to 0 for no retry, set -1 for infinite retry.')]
    [Int32]$RetryTimes = 3,

    [Parameter(Mandatory = $false, 
    ValueFromPipeline = $true, 
    HelpMessage='Used for resume operation. When provided, the script will copy containers that are alphabetically after $LastSuccessContainerName')]
    [String]$LastSuccessContainerName = $nullz
 )

 if( (Get-Item $AzCopyPath).Exists)
 {

    $FileItemVersion = (Get-Item $AzCopyPath).VersionInfo
    $FilePath = ("{0}.{1}.{2}.{3}" -f  $FileItemVersion.FileMajorPart,  $FileItemVersion.FileMinorPart,  $FileItemVersion.FileBuildPart,  $FileItemVersion.FilePrivatePart)

    if([version] $FilePath -lt "7.0.0.2")
    {
        $AzCopyPath = Read-Host "Version of AzCopy found at default install directory is of a lower, unsupported version. Please input the full filePath of the AzCopy.exe that is version 7.0.0.2 or higher, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
    }
 }
 elseIf( (Get-Item $AzCopyPath).Exists -eq $false)
 {
    $AzCopyPath = Read-Host "Input the full filePath of the AzCopy.exe, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
 }

# Create and check Storage context
$Error.Clear()
$srcCtx = New-AzureStorageContext -StorageAccountName $srcStorageAccountName -StorageAccountKey $srcStorageAccountKey
if ($srcCtx -eq $null)
{
    Write-Error "Script could not create source Storage Context, possibly due to invalid StorageAccountName or StorageAccount Key terminating: $Error[0]";
    return;
}
$destCtx = New-AzureStorageContext -StorageAccountName $destStorageAccountName -StorageAccountKey $destStorageAccountKey
if ($destCtx -eq $null)
{
    Write-Error "Script could not create destination storage context, possibly due to invalid StorageAccountName or StorageAccount Key terminating: $Error[0]";
    return;
}

#Check Source and Destination Storage account Connection
$Error.Clear()
$Containers = Get-AzureStorageContainer -MaxCount 1 -Context $srcCtx
if ($Error.Count -gt 0)
{
    Write-Error "Script failed to connect to source Storage account, terminating: $Error[0]";
    return;
}
$Containers = Get-AzureStorageContainer -MaxCount 1 -Context $destCtx
if ($Error.Count -gt 0)
{
    Write-Error "Script failed to connect to destination Storage account, terminating: $Error[0]";
    return;
}

#Check AzCopy Path
if((Test-Path $AzCopyPath) -eq $false)
{
    Write-Error "Script is terminating since the provided AzCopyPath does not exist: $AzCopyPath ";
    return;
}
elseif((Get-Item $AzCopyPath).BaseName -ne "AzCopy" )
{
    Write-Error "Script is terminating since the provided AzCopyPath does not refer to the AzCopy exe: $AzCopyPath ";
    return;
}
elseif((Get-Item $AzCopyPath).BaseName -eq "AzCopy")
{
    $FileItemVersion = (Get-Item $AzCopyPath).VersionInfo
    $FilePath = ("{0}.{1}.{2}.{3}" -f  $FileItemVersion.FileMajorPart,  $FileItemVersion.FileMinorPart,  $FileItemVersion.FileBuildPart,  $FileItemVersion.FilePrivatePart)

    if([version] $FilePath -lt "7.0.0.2")
    {
        $AzCopyPath = Read-Host "Version of AzCopy found at provided path is of a lower, unsupported version. Please input the full filePath of the AzCopy.exe that is version 7.0.0.2 or higher, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
    }
}

$OutputLastSuccessContainer = $LastSuccessContainerName;
$HasReachedLastSuccessContainer = $false;

if ($LastSuccessContainerName -eq $null -or $LastSuccessContainerName -eq "")
{
    $HasReachedLastSuccessContainer = $true;
}

# For list container
$MaxReturn = 250
$Token = $Null

do{
    # List source containers
    $retry = 1
    $Error.Clear()
    $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx

    # If list container fail, retry it
    while(($Error.Count -gt 0) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
    {
        Write-Host "Retry List containers $retry"
        $Error.Clear()
        $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx
        $retry++
    }

    # If list container fail after retry, break script
    if ($Error.Count -gt 0){
        Write-Error "Terminating the script since listing source containers failed: $Error[0]";
        $CopyContainerFail = $true
        break;
    }
    $Token = $srcContainers[$srcContainers.Count -1].ContinuationToken;

    # Transfer containers one by one
    $CopyContainerFail = $false
    foreach ($container in $srcContainers)
    {
        if (!$HasReachedLastSuccessContainer)
        {
            if ($container.Name -eq $LastSuccessContainerName)
            {
                $HasReachedLastSuccessContainer = $true;
            }
            Write-Host "Skipping container copy: $($container.Name)"
            Continue;
        }

        Write-Host "Start copying container: $($container.Name)"
        $retry = 1

        # Get AzCopy command for transfer one container
        $destContainer = $destCtx.StorageAccount.CreateCloudBlobClient().GetContainerReference($container.Name)
        $azCopyCmd = [string]::Format("""{0}"" /source:{1} /dest:{2} /sourcekey:""{3}"" /destkey:""{4}"" /snapshot /y /s",$AzCopyPath, $container.CloudBlobContainer.Uri.AbsoluteUri, $destContainer.Uri.AbsoluteUri, $srcStorageAccountKey, $DestStorageAccountKey)
    
        # Execute the AzCopy command first time
        Write-Host "$azCopyCmd"
        $result = cmd /c $azCopyCmd
        foreach($s in $result)
        {
            Write-Host $s 
        }

        # If transfer failed, retry it
        while(($LASTEXITCODE -ne 0) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
        {
            Write-Host "Retry $retry : $azCopyCmd"
            $result = cmd /c $azCopyCmd
            foreach($s in $result)
            {
                Write-Host $s 
            }
            $retry++
        }

        # If tranfer failed after retry, print error
        if ($LASTEXITCODE -ne 0){
            Write-Error "Container copy failed: $($container.Name)";
            $CopyContainerFail = $true
            break;
        }
        else
        {
            Write-Host "Finished copying container: $($container.Name)"
            Write-Host ""
            $OutputLastSuccessContainer = $container.Name
        }
    }
    if($CopyContainerFail)
    {
        break;
    }
}
While ($Token -ne $Null)

# Summary the copy result
if ($CopyContainerFail)
{
    if(($OutputLastSuccessContainer -ne $null) -and ($OutputLastSuccessContainer -ne ""))
    {
        Write-Warning "To resume, rerun the script and append the parameter: ""-LastSuccessContainer $OutputLastSuccessContainer"""
        return $OutputLastSuccessContainer
    }
    else
    {
        Write-Warning "To resume, rerun the script."
        return $null
    }
}
else
{
    Write-Host "All Containers copied successfully."
    return ""
}
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um Daten aus einem Speicherkonto in ein anderes zu kopieren. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzureStorageContainer](/powershell/module/azure.storage/Get-AzureStorageContainer) | Gibt die Container zurück, die diesem Storage-Konto zugeordnet sind. |
| [New-AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Erstellt einen Azure Storage-Kontext. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für Speicher finden Sie in den [PowerShell-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-powershell.md).

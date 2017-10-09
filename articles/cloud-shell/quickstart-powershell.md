---
title: "Schnellstart für PowerShell in Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Schnellstart für PowerShell in Cloud Shell"
services: Azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: damaerte
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: fd1d340bc0408eaeb0b7b18235df109224eae5f5
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Schnellstart für PowerShell in Azure Cloud Shell

Dieses Dokument erläutert die Verwendung von PowerShell in Cloud Shell im [Azure-Portal](https://aka.ms/PSCloudPreview).

> [!NOTE]
> Ein Schnellstart zu [Bash in Azure Cloud Shell](quickstart.md) ist ebenfalls verfügbar.

## <a name="start-cloud-shell"></a>Starten von Cloud Shell

1. Klicken Sie im oberen Navigationsbereich des Azure-Portals auf die Schaltfläche **Cloud Shell**.

  ![](media/quickstart-powershell/shell-icon.png)

2. Wählen Sie die PowerShell-Umgebung aus der Dropdownliste aus, und Sie befindet sich auf dem Azure-Laufwerk `(Azure:)`.

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>Ausführen von PowerShell-Befehlen

Führen Sie reguläre PowerShell-Befehle wie in der Cloud Shell aus. Beispiel:

```Powershell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Navigieren in Azure-Ressourcen

 1. Listen Sie Ihre Abonnements auf.

    ``` Powershell
    PS Azure:\> dir
    ```

 2. Führen Sie `cd` aus, um zu Ihrem bevorzugten Abonnement zu gelangen.

    ``` Powershell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Zeigen Sie alle Azure-Ressourcen unter dem aktuellen Abonnement an.
 
    Geben Sie ein `dir`, um mehrere Ansichten der Azure-Ressourcen aufzulisten.
 
    ``` PowerShell
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
     ```

### <a name="allresources-view"></a>AllResources-Ansicht 
Geben Sie `dir` unter dem Verzeichnis `AllResources` ein, um Ihre Azure-Ressourcen anzuzeigen.
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>Untersuchen von Ressourcengruppen

 Sie können zum Verzeichnis `ResourceGroups` wechseln und innerhalb einer bestimmten Ressourcengruppe virtuelle Computer suchen.

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> Sie werden möglicherweise feststellen, dass die Cloud-Shell bei der zweiten Eingabe von `dir` in der Lage ist, die Elemente wesentlich schneller anzuzeigen.
> Das liegt daran, dass die untergeordneten Elemente im Arbeitsspeicher erstellt werden, um eine höhere Benutzerfreundlichkeit zu erreichen.
Sie können jedoch immer `dir -Force` verwenden, um aktuelle Daten abzurufen.

### <a name="navigate-storage-resources"></a>Navigieren in Speicherressourcen
    
Durch den Wechsel in den `StorageAccounts`-Ordner können Sie einfach zu Ihren Speicherressourcen navigieren.
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

Mit der Verbindungszeichenfolge können Sie den folgenden Befehl verwenden, um die Azure-Dateifreigabe einzubinden.
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

Weitere Informationen finden Sie unter [Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows][azmount].

Sie können auch wie folgt durch die Verzeichnisse unter der Azure-Dateifreigabe navigieren:

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>Interagieren mit virtuellen Computern

Sie finden all Ihre virtuellen Computer unter dem aktuellen Abonnement über das `VirtualMachines`-Verzeichnis.
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Aufrufen von PowerShell-Skripts zwischen virtuellen Remotecomputern

 > [!WARNING]
 > Weitere Informationen finden Sie unter [Problembehandlung bei der Remoteverwaltung von virtuellen Azure-Computern](troubleshooting.md#powershell-resolutions).

  Angenommen, Sie verfügen über einen virtuellen Computer, MyVM1, dann verwenden wir `Invoke-AzureRmVMCommand`, um einen PowerShell-Skriptblock auf dem Remotecomputer aufzurufen.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  Sie können auch zuerst zum Verzeichnis „virtualMachines“ navigieren und `Invoke-AzureRmVMCommand` wie folgt ausführen.

  ``` Powershell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  Ihnen wird daraufhin eine Ausgabe angezeigt, die in etwa wie folgt aussieht:

  ``` Powershell
  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Interaktives Anmelden auf einem virtuellen Remotecomputer

Mithilfe von `Enter-AzureRmVM` können Sie sich interaktiv auf einem virtuellen Computer anmelden, der in Azure ausgeführt wird.

  ``` Powershell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Sie können auch zuerst zum Verzeichnis `virtualMachines` navigieren und `Enter-AzureRmVM` wie folgt ausführen.

  ``` Powershell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>Erkunden von WebApps

Durch den Wechsel in den `WebApps`-Ordner können Sie einfach zu Ihren Speicherressourcen navigieren.

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps for example,
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -force
PS Azure:\MySubscriptionName\WebApps> dir -force

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

```

## <a name="list-available-commands"></a>Auflisten verfügbarer Befehle

Geben Sie unter dem `Azure`-Laufwerk `Get-AzureRmCommand` ein, um kontextabhängige Azure-Befehle abzurufen.

Alternativ können Sie immer `Get-Command *azurerm* -Module AzureRM.*` verwenden, um die verfügbaren Azure-Befehle zu ermitteln.

## <a name="install-custom-modules"></a>Installieren von benutzerdefinierten Modulen

Sie können `Install-Module` ausführen, um Module aus dem [PowerShell-Katalog][gallery] zu installieren.

## <a name="get-help"></a>Get-Help

Geben Sie `Get-Help` ein, um Informationen zu PowerShell in Azure Cloud Shell abzurufen.

``` Powershell
PS Azure:\> Get-Help
```

Für einen bestimmten Befehl können Sie weiterhin „Get-Help“ gefolgt von einem Cmdlet ausführen. Beispiel:

``` Powershell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-file-storage-to-store-your-data"></a>Verwenden von Azure File Storage zum Speichern Ihrer Daten

Sie können ein Skript erstellen, z. B. `helloworld.ps1`, und es in Ihrem clouddrive-Verzeichnis speichern, um es für verschiedene Shellsitzungen zu verwenden.

``` Powershell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Wenn Sie PowerShell das nächste Mal in Cloud Shell verwenden, ist die Datei `helloworld.ps1` im Ordner `CloudDrive` enthalten, der Ihre Azure-Dateifreigabe einbindet.

## <a name="use-custom-profile"></a>Verwenden benutzerdefinierter Profile

Sie können die PowerShell-Umgebung anpassen, indem Sie PowerShell-Profile – `profile.ps1` oder `Microsoft.PowerShell_profile.ps1` – erstellen. Speichern Sie es unter dem `CloudDrive`, damit es in jeder PowerShell-Sitzung geladen werden kann, wenn Sie Cloud Shell starten.

Informationen zum Erstellen von Profilen finden Sie unter [Informationen zu Profilen][profile].

## <a name="use-git"></a>Verwenden von Git

Wenn Sie in Cloud Shell ein Git-Repository klonen möchten, müssen Sie ein [persönliches Zugriffstoken][githubtoken] erstellen und es als Benutzername verwenden. Nachdem Sie über das Token verfügen, klonen Sie das Repository wie folgt:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Da Sitzungen in Cloud Shell nicht beibehalten werden, wenn Sie sich abmelden oder das Zeitlimit der Sitzung erreicht wurde, ist die Git-Konfigurationsdatei bei der nächsten Anmeldung nicht vorhanden. Damit Ihre Git-Konfiguration erhalten bleibt, müssen Sie Ihre .gitconfig-Datei in Ihrem `CloudDrive` speichern und sie kopieren oder einen Symlink erstellen, wenn die `CloudShell` gestartet wird. Verwenden Sie den folgenden Codeausschnitt aus Ihrer „profile.ps1“, um einen Symlink zum `CloudDrive` zu erstellen.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $Home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>Beenden Sie die Shell:

Geben Sie `exit` ein, um die Sitzung zu beenden.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/

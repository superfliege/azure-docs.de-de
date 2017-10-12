---
title: "Installieren und Konfigurieren von PowerShell für den Azure Stack-Schnellstart | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Installieren und Konfigurieren von PowerShell für Azure Stack."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: 6e5c420ed80127213e38849ac1999bba199e36c2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Einrichten von PowerShell in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Dieser Artikel bietet einen schnellen Einstieg in das Installieren und Konfigurieren der Azure Stack-Umgebung mit PowerShell. Das in diesem Artikel bereitgestellte Skript ist auf den **Azure Stack-Betreiber** beschränkt.

Dieser Artikel stellt eine verkürzte Version der Schritte dar, die in den Artikeln [Install PowerShell (Installieren von PowerShell)]( azure-stack-powershell-install.md), [Download tools (Herunterladen von Tools)]( azure-stack-powershell-download.md) und [Configure the Azure Stack operator's PowerShell environment (Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers)]( azure-stack-powershell-configure-admin.md). Indem Sie die Skripts in diesem Thema verwenden, können Sie PowerShell für Azure Stack-Umgebungen einrichten, die mit Azure Active Directory oder Active Directory-Verbunddienste bereitgestellt werden.  


## <a name="set-up-powershell-for-aad-based-deployments"></a>Einrichten von PowerShell für auf AAD basierende Bereitstellungen

Melden Sie sich bei Ihrem Azure Stack Development Kit oder bei einem externen, Windows-basierten Client an, wenn Sie über VPN verbunden sind. Öffnen Sie eine PowerShell ISE-Sitzung mit erhöhten Rechten, und führen Sie das folgende Skript aus. (Aktualisieren Sie die Variablen „TenantName“, „ArmEndpoint“ und „GraphAudience“ gemäß der Konfiguration Ihrer Umgebung.)

```powershell
# Specify Azure Active Directory tenant name
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable | `
  where-Object {$_.Name -like “Azure*”} | `
  Uninstall-Module

# Install PowerShell for Azure Stack
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.10 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module
cd \

invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module `
  .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
$GraphAudience = "<GraphAuidence endpoint for your environment>"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackAdmin" `
  -ArmEndpoint $ArmEndpoint

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience $GraphAudience

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $TenantName `
  -EnvironmentName AzureStackAdmin

# Sign-in to the operator's portal.
Login-AzureRmAccount `
  -EnvironmentName "AzureStackAdmin" `
  -TenantId $TenantID 

```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>Einrichten von PowerShell für auf AD FS basierende Bereitstellungen 

Melden Sie sich bei Ihrem Azure Stack Development Kit oder bei einem externen, Windows-basierten Client an, wenn Sie über VPN verbunden sind. Öffnen Sie eine PowerShell ISE-Sitzung mit erhöhten Rechten, und führen Sie das folgende Skript aus. (Aktualisieren Sie die Variablen „ArmEndpoint“ und „GraphAudience“ gemäß der Konfiguration Ihrer Umgebung.)

```powershell

# Set the module repository and the execution policy
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable | `
  where-Object {$_.Name -like “Azure*”} | `
  Uninstall-Module

# Install PowerShell for Azure Stack
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.10 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module
cd \

invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module `
  .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
$GraphAudience = "<GraphAuidence endpoint for your environment>"

# Configure the cloud administrator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackAdmin" `
  -ArmEndpoint $ArmEndpoint

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience $GraphAudience `
  -EnableAdfsAuthentication:$true

$TenantID = Get-AzsDirectoryTenantId `
  -ADFS `
  -EnvironmentName "AzureStackAdmin"

# Sign-in to the operator's portal.
Login-AzureRmAccount `
  -EnvironmentName "AzureStackAdmin" `
  -TenantId $TenantID 

```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Da Sie PowerShell nun konfiguriert haben, können Sie die Konfiguration testen, indem Sie eine Ressourcengruppe erstellen:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

Wenn die Ressourcengruppe erstellt wird, ist die Eigenschaft „Bereitstellungsstatus“ in der Cmdlet-Ausgabe auf „Erfolgreich“ festgelegt.

## <a name="next-steps"></a>Nächste Schritte

* [Install and configure CLI (Installieren und Konfigurieren der CLI)](azure-stack-connect-cli.md)

* [Entwickeln von Vorlagen](user/azure-stack-develop-templates.md)








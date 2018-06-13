---
title: Installieren und Konfigurieren von PowerShell für den Azure Stack-Schnellstart | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Installieren und Konfigurieren von PowerShell für Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 6996DFC1-5E05-423A-968F-A9427C24317C
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mabrigg
ms.openlocfilehash: 66598bda7ca1fcf5c6e05ab47232236b740177a6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075237"
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Einrichten von PowerShell in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Dieser Schnellstart unterstützt Sie beim Installieren und Konfigurieren einer Azure Stack-Umgebung mit PowerShell. Der Gültigkeitsbereich des in diesem Artikel bereitgestellten Skripts bezieht sich nur auf den **Azure Stack-Betreiber**.

Dieser Artikel stellt eine verkürzte Version der Schritte dar, die in den Artikeln [Installieren von PowerShell]( azure-stack-powershell-install.md), [Herunterladen von Tools]( azure-stack-powershell-download.md) und [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers]( azure-stack-powershell-configure-admin.md) beschrieben werden. Indem Sie die Skripts in diesem Thema verwenden, können Sie PowerShell für Azure Stack-Umgebungen einrichten, die mit Azure Active Directory oder Active Directory-Verbunddiensten (AD FS) bereitgestellt werden.  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Einrichten von PowerShell für auf Azure Active Directory basierenden Bereitstellungen

Melden Sie sich bei Ihrem Azure Stack Development Kit oder bei einem externen, Windows-basierten Client an, wenn Sie über VPN verbunden sind. Öffnen Sie eine PowerShell ISE-Sitzung mit erhöhten Rechten, und führen Sie dann das folgende Skript aus. Stellen Sie sicher, dass Sie die Variablen **TenantName**, **ArmEndpoint** und **GraphAudience** wie für Ihre Umgebungskonfiguration erforderlich aktualisieren:

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>Einrichten von PowerShell für auf AD FS basierende Bereitstellungen

Sie können das folgende Skript verwenden, wenn Sie Azure Stack bei vorhandener Internetverbindung betreiben. Wenn Sie Azure Stack jedoch ohne Internetverbindung betreiben, [installieren Sie PowerShell in einer nicht verbundenen Umgebung](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity). Die zum Konfigurieren von PowerShell verwendeten Cmdlets bleiben wie in diesem Skript gezeigt gleich. Melden Sie sich bei Ihrem Azure Stack Development Kit oder bei einem externen, Windows-basierten Client an, wenn Sie über VPN verbunden sind. Öffnen Sie eine PowerShell ISE-Sitzung mit erhöhten Rechten, und führen Sie dann das folgende Skript aus. Stellen Sie sicher, dass Sie die Variablen **ArmEndpoint** und **GraphAudience** wie für Ihre Umgebungskonfiguration erforderlich aktualisieren:

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Da Sie PowerShell nun konfiguriert haben, können Sie die Konfiguration testen, indem Sie eine Ressourcengruppe erstellen:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

> [!note]  
> Um eine Ressourcengruppe anzugeben, benötigen Sie eine Ressourcengruppe in Ihrem Abonnement. Weitere Informationen zu Abonnements finden Sie unter [Übersicht über Pläne, Angebote, Kontingente und Abonnements](azure-stack-plan-offer-quota-overview.md).

Nachdem die Ressourcengruppe erstellt wurde, ist die Eigenschaft **Bereitstellungsstatus** auf **Erfolgreich** festgelegt.

## <a name="next-steps"></a>Nächste Schritte

* [Install and configure CLI (Installieren und Konfigurieren der CLI)](azure-stack-connect-cli.md)

* [Entwickeln von Vorlagen](user/azure-stack-develop-templates.md)

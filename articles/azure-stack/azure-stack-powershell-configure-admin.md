---
title: Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die PowerShell-Umgebung des Azure Stack-Betreibers konfigurieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: mabrigg
ms.openlocfilehash: a8ab52de6c57e84bb2c90ce6bcf53ef1b92e30af
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können Azure Stack für die Verwendung von PowerShell zum Verwalten von Ressourcen konfigurieren, z.B. die Erstellung von Angeboten, Plänen, Kontingenten und Warnungen. In diesem Thema können Sie die Betreiberumgebung konfigurieren. Wenn Sie PowerShell für die Benutzerumgebung konfigurieren möchten, hilft Ihnen der Artikel [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](user/azure-stack-powershell-configure-user.md) weiter.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, wenn Sie [über VPN verbunden](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) sind: 

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  
* Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Konfigurieren Sie die Betreiberumgebung, und melden Sie sich bei Azure Stack an.

Konfigurieren Sie die Azure Stack-Betreiberumgebung mit PowerShell. Basierend auf der Art von Bereitstellung wird in Azure AD oder AD FS eines der folgenden Skripts ausgeführt: Ersetzen Sie die Azure AD-Werte für tenantName, GraphAudience-Endpunkt und ArmEndpoint durch Ihre eigene Umgebungskonfiguration.

### <a name="azure-active-directory-azure-ad-based-deployments"></a>Auf Azure Active Directory (Azure AD) basierende Bereitstellungen

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -AADTenantName "<mydirectorytenant>.onmicrosoft.com" -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````


### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Auf Active Directory-Verbunddienste (AD FS) basierende Bereitstellungen

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -ADFS -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Nachdem Sie nun alles eingerichtet haben, können Sie mit PowerShell Ressourcen in Azure Stack erstellen. Sie können beispielsweise eine Ressourcengruppe für eine Anwendung erstellen und einen virtuellen Computer hinzufügen. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe mit dem Namen „MyResourceGroup“ zu erstellen:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Nächste Schritte
* [Entwickeln von Vorlagen für Azure Stack](user/azure-stack-develop-templates.md)
* [Bereitstellen von Vorlagen mit PowerShell](user/azure-stack-deploy-template-powershell.md)

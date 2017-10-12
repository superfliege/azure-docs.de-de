---
title: Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die PowerShell-Umgebung des Azure Stack-Betreibers konfigurieren.
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
ms.openlocfilehash: 7e912dcbfd1c745df2a0fc8717a075c0476e8d60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Azure Stack-Betreiber können Sie die PowerShell-Umgebung des Azure Stack Development Kits konfigurieren. Nach der Konfiguration können Sie PowerShell verwenden, um Azure Stack-Ressourcen wie Warnungen, das Erstellen von Angeboten, Plänen, Kontingenten usw. zu verwalten. Dieses Thema ist auf die Nutzung mit den Umgebungen des Cloudbetreibers beschränkt. Wenn Sie PowerShell für die Benutzerumgebung einrichten möchten, finden Sie weitere Informationen im Thema [Configure the Azure Stack user's PowerShell environment (Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers)](user/azure-stack-powershell-configure-user.md). 

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, wenn Sie [über VPN verbunden](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) sind: 

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  
* Laden Sie die [Tools herunter, die zum Arbeiten mit Azure Stack erforderlich sind](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Konfigurieren Sie die Betreiberumgebung, und melden Sie sich bei Azure Stack an.

Führen Sie basierend auf dem Bereitstellungstyp (Azure AD oder AD FS) eines der folgenden Skripts aus, um die Azure Stack-Umgebung des Betreibers mit PowerShell zu konfigurieren. (Ersetzen Sie die AAD-Werte „tenantName“ und „ArmEndpoint“ und den Wert für den GraphAudience-Endpunkt gemäß Ihrer Umgebungskonfiguration.):

### <a name="azure-active-directory-aad-based-deployments"></a>Auf Azure Active Directory (AAD) basierende Bereitstellungen
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Auf Active Directory-Verbunddienste (AD FS) basierende Bereitstellungen
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

  # Get the Active Directory tenantId that is used to deploy Azure Stack     
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Nachdem nun alles eingerichtet ist, können Sie mit PowerShell Ressourcen in Azure Stack erstellen. Sie können beispielsweise eine Ressourcengruppe für eine Anwendung erstellen und einen virtuellen Computer hinzufügen. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe mit dem Namen „MyResourceGroup“ zu erstellen:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Nächste Schritte
* [Entwickeln von Vorlagen für Azure Stack](user/azure-stack-develop-templates.md)
* [Bereitstellen von Vorlagen mit PowerShell](user/azure-stack-deploy-template-powershell.md)
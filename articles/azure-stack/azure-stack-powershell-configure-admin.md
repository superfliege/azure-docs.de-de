---
title: Herstellen einer Verbindung mit Azure Stack über PowerShell als Operator | Microsoft Docs
description: Informationen zum Herstellen einer Verbindung mit Azure Stack über PowerShell als Operator
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 11/08/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: f5df27df5901d6568b30e1ae4f40cae8b3de4c86
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51298818"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Herstellen einer Verbindung mit Azure Stack über PowerShell als Operator

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können Azure Stack für die Verwendung von PowerShell zum Verwalten von Ressourcen konfigurieren, z.B. die Erstellung von Angeboten, Plänen, Kontingenten und Warnungen. In diesem Thema können Sie die Betreiberumgebung konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](.\asdk\asdk-connect.md#connect-with-rdp) oder auf einem Windows-basierten externen Client aus, sofern [eine VPN-Verbindung mit dem ASDK](.\asdk\asdk-connect.md#connect-with-vpn) besteht. 

 - Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  
 - Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Konfigurieren Sie die Betreiberumgebung, und melden Sie sich bei Azure Stack an.

Konfigurieren Sie die Azure Stack-Betreiberumgebung mit PowerShell. Führen Sie eines der folgenden Skripts aus: Ersetzen Sie die Azure AD-Werte für tenantName, GraphAudience-Endpunkt und ArmEndpoint durch Ihre eigene Umgebungskonfiguration.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRmEnvironment -Name "AzureStackAdmin" -ARMEndpoint $ArmEndpoint -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external


    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $tenantId
````

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Nachdem Sie nun alles eingerichtet haben, können Sie mit PowerShell Ressourcen in Azure Stack erstellen. Sie können beispielsweise eine Ressourcengruppe für eine Anwendung erstellen und einen virtuellen Computer hinzufügen. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe mit dem Namen **MyResourceGroup** zu erstellen.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Nächste Schritte

 - [Entwickeln von Vorlagen für Azure Stack](user/azure-stack-develop-templates.md)
 - [Bereitstellen von Vorlagen mit PowerShell](user/azure-stack-deploy-template-powershell.md)
---
title: Registrieren des ASDK bei Azure | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Azure Stack bei Azure registriert wird, um die Azure Marketplace-Syndikation und die Berichterstellung zur Verwendung zu aktivieren.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: eb1f939f76c3528f05a9002b6365359fb6599aa2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357723"
---
# <a name="azure-stack-registration"></a>Azure Stack-Registrierung
Sie können Ihre ASDK-Installation (Azure Stack Development Kit) bei Azure registrieren, um Marketplace-Elemente von Azure herunterzuladen und die Berichterstellung zu E-Commerce-Daten für Microsoft einzurichten. Die Registrierung ist für die uneingeschränkte Unterstützung aller Azure Stack-Funktionen (einschließlich Marketplace-Syndikation) erforderlich. Es wird empfohlen, eine Registrierung durchzuführen, da Sie hierüber wichtige Azure Stack-Funktionen wie die Marketplace-Syndikation und Berichterstellung zur Verwendung testen können. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung durch Benutzer des ASDK wird diesen jedoch nicht in Rechnung gestellt.

Wenn Sie Ihr ASDK nicht registrieren, wird Ihnen die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.

## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure 
Führen Sie diese Schritte aus, um das ASDK bei Azure zu registrieren.

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. Beim ASDK ist das der Development Kit-Hostcomputer. 

Bevor Sie die folgenden Schritte zur Registrierung des ASDK bei Azure durchführen, installieren Sie die mit Azure Stack kompatiblen PowerShell-Module, und laden Sie die Azure Stack-Tools herunter. Dies wird im Artikel [Konfiguration nach der Bereitstellung](asdk-post-deploy.md) beschrieben. 

1. Öffnen Sie eine PowerShell-Konsole als Administrator.  

2. Führen Sie die folgenden PowerShell-Befehle zum Registrieren Ihrer ASDK-Installation bei Azure aus (Sie müssen sich sowohl bei Ihrem Azure-Abonnement als auch bei der lokalen ASDK-Installation anmelden):

    ```PowerShell
    # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    Set-AzsRegistration `
        -PrivilegedEndpointCredential $CloudAdminCred `
        -PrivilegedEndpoint AzS-ERCS01 `
        -BillingModel Development

3. When the script completes, you should see this message: **Your environment is now registered and activated using the provided parameters.**

    ![](media/asdk-register/1.PNG) 

## Verify the registration was successful
Follow these steps to verify that the ASDK registration with Azure was successful.

1. Sign in to the [Azure Stack administration portal](https://adminportal.local.azurestack.external).

2. Click **Marketplace Management** > **Add from Azure**.

    ![](media/asdk-register/2.PNG) 

3. If you see a list of items available from Azure, your activation was successful.

    ![](media/asdk-register/3.PNG) 

## Next steps
[Add an Azure Stack marketplace item](asdk-marketplace-item.md)

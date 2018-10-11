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
ms.date: 09/20/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: d06ad47dc2962b249b4e7aef5667492e642be35e
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830122"
---
# <a name="azure-stack-registration"></a>Azure Stack-Registrierung
Sie können Ihre ASDK-Installation (Azure Stack Development Kit) bei Azure registrieren, um Marketplace-Elemente von Azure herunterzuladen und die Berichterstellung zu E-Commerce-Daten für Microsoft einzurichten. Die Registrierung ist für die uneingeschränkte Unterstützung aller Azure Stack-Funktionen (einschließlich Marketplace-Syndikation) erforderlich. Es wird empfohlen, eine Registrierung durchzuführen, da Sie hierüber wichtige Azure Stack-Funktionen wie die Marketplace-Syndikation und Berichterstellung zur Verwendung testen können. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung durch Benutzer des ASDK wird diesen jedoch nicht in Rechnung gestellt.

Wenn Sie Ihr ASDK nicht registrieren, wird Ihnen die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die folgenden Anweisungen zur Registrierung des ASDK bei Azure befolgen, installieren Sie die mit Azure Stack kompatiblen PowerShell-Module, und laden Sie die Azure Stack-Tools herunter. Dies wird im Artikel [Konfiguration nach der Bereitstellung](asdk-post-deploy.md) beschrieben.

Außerdem muss der PowerShell-Sprachmodus auf dem Computer, der zur Registrierung des ASDK bei Azure verwendet wird, auf **FullLanguageMode** festgelegt werden. Um zu überprüfen, ob der aktuelle Sprachmodus auf vollständig festgelegt ist, öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden PowerShell-Befehle aus:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Vergewissern Sie sich, dass in der Ausgabe **FullLanguageMode** zurückgegeben wird. Wenn ein anderer Sprachmodus zurückgegeben wird, muss die Registrierung auf einem anderen Computer ausgeführt oder der Sprachmodus auf **FullLanguageMode** festgelegt werden, damit Sie fortfahren können.

## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure
Führen Sie diese Schritte aus, um das ASDK bei Azure zu registrieren.

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. Beim ASDK ist das der Development Kit-Hostcomputer.

1. Öffnen Sie eine PowerShell-Konsole als Administrator.  

2. Führen Sie die folgenden PowerShell-Befehle zum Registrieren Ihrer ASDK-Installation bei Azure aus. Sie müssen sich sowohl bei Ihrem Azure-Abonnement als auch bei der lokalen ASDK-Installation anmelden. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, [können Sie hier ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.<br><br>Wenn Sie das Registrierungsskript auf mehreren Instanzen von Azure Stack mit der gleichen Azure-Abonnement-ID ausführen, legen Sie einen eindeutigen Namen für die Registrierung fest, wenn Sie das **Set-AzsRegistration**-Cmdlet ausführen. Der **RegistrationName**-Parameter hat den Standardwert **AzureStackRegistration**. Wenn Sie den gleichen Namen jedoch in mehreren Instanzen von Azure Stack verwenden, tritt beim Ausführen des Skripts ein Fehler auf.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    $UsageReporting = True # Set to False if you using a Capacity Billing Model
    Set-AzsRegistration `
        -PrivilegedEndpointCredential $CloudAdminCred `
        -PrivilegedEndpoint AzS-ERCS01 `
        -BillingModel Development `
        -RegistrationName $RegistrationName `
        -EnableUsageReporting $UsageReporting
    ```
3. Wenn das Skript abgeschlossen ist, sollte folgende Meldung angezeigt werden: **Your environment is now registered and activated using the provided parameters** (Ihre Umgebung ist nun mit den angegebenen Parametern registriert und aktiviert).

    ![Ihre Umgebung ist nun registriert.](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Überprüfen der erfolgreichen Ausführung der Registrierung
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob die Registrierung des ASDK bei Azure erfolgreich ausgeführt wurde.

1. Melden Sie sich beim [Azure Stack-Verwaltungsportal](https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Marketplace management** >  **Add from Azure** (Marketplace-Verwaltung > Aus Azure hinzufügen).

    ![](media/asdk-register/2.PNG)

3. Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen, war die Aktivierung erfolgreich.

    ![](media/asdk-register/3.PNG)

## <a name="move-a-registration-resource"></a>Verschieben einer Registrierungsressource
Das Verschieben einer Registrierungsressource zwischen Ressourcengruppen im gleichen Abonnement **wird unterstützt**. Weitere Informationen zum Verschieben von Ressourcen in eine neue Ressourcengruppe finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen eines Azure Stack-Marketplace-Elements](.\.\azure-stack-marketplace.md)

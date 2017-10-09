---
title: Registrieren von Azure Stack | Microsoft-Dokumentation
description: Registrieren Sie Azure Stack in Ihrem Azure-Abonnement.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 13ac0afb9ebfe568f1d8b1db2fedfd28cb05c3e7
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Registrieren Sie Azure Stack in Ihrem Azure-Abonnement.

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Für Azure Active Directory-Bereitstellungen können Sie [Azure Stack](azure-stack-poc.md) in Azure registrieren, um Marketplace-Elemente aus Azure herunterzuladen, und um das Senden von Berichten zu Geschäftsdaten an Microsoft einzurichten. 

> [!NOTE]
>Die Registrierung wird empfohlen, da sie dadurch wichtige Azure Stack-Funktionen testen können, wie z.B. die Marketplace-Syndikation oder Nutzungsberichte. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung von Benutzern des Azure Stack Development Kits wird diesen nicht in Rechnung gestellt.
>


## <a name="get-azure-subscription"></a>Abschließen eines Azure-Abonnements

Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Die ID erhalten Sie, indem Sie sich bei Azure anmelden und auf **Weitere Dienste** > **Abonnements** und dann auf das Abonnement klicken, das Sie verwenden möchten. Unter **Essentials** (Zusammenfassung) sehen Sie nun die **Abonnement-ID**. Abonnements der Government-Clouds von China, Deutschland und den USA werden aktuell nicht unterstützt.
- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist (Konten mit MSA/2FA werden unterstützt)
- Sie müssen wissen, welches Azure Active Directory-Verzeichnis für das Azure-Abonnement verwendet wird. Dieses Verzeichnis können Sie in Azure finden, indem Sie mit der Maus auf Ihr Profilbild zeigen, das sich in der oberen rechten Ecke im Azure-Portal befindet. 
- Sie müssen den Azure Stack-Ressourcenanbieter registriert haben (weitere Informationen finden Sie weiter unten im Abschnitt **Registrieren des Azure Stack-Ressourcenanbieters**).

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/en-us/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Registrieren des Azure Stack-Ressourcenanbieters in Azure
> [!NOTE] 
> Diesen Schritt müssen Sie nur ausführen, wenn Sie sich in einer Azure Stack-Umgebung befinden.
>

1. Starten Sie die PowerShell ISE als Administrator.
2. Melden Sie sich bei Ihrem Azure-Konto an, das ein Azure-Abonnement besitzt, bei dem der Parameter „-EnvironmentName“ auf „AzureCloud“ festgelegt ist.
3. Registrieren Sie den Azure-Ressourcenanbieter „Microsoft.AzureStack“.

Beispiel: 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack -Force
```


## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure

> [!NOTE]
>Diese Schritte müssen auf dem Hostcomputer durchgeführt werden.
>

1. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md). 
2. Kopieren Sie das [Skript „RegisterWithAzure.ps1“](https://go.microsoft.com/fwlink/?linkid=842959) in einen Ordner (z.B. C:\Temp).
3. Starten Sie PowerShell ISE als Administrator.    
4. Führen Sie das Skript „“ aus, nachdem Sie die folgenden Platzhalter ersetzt haben:
    - *YourAccountName* (IhrKontoname) ist der Besitzer des Azure-Abonnements
    - *YourID* (IhreID) ist die ID des Azure-Abonnements, das Sie zur Registrierung von Azure Stack verwenden möchten
    - *YourDirectory* (IhrVerzeichnis) ist der Name Ihres Azure AD-Mandanten, zu dem Ihr Azure-Abonnement gehört

    ```powershell
    RegisterWithAzure.ps1 -azureSubscriptionId YourID -azureDirectoryTenantName YourDirectory -azureAccountId YourAccountName
    ```
    
    Beispiel:
    
    ```powershell
    C:\temp\RegisterWithAzure.ps1 -azureSubscriptionId "5e0ae55d-0b7a-47a3-afbc-8b372650abd3" `
    -azureDirectoryTenantName "contoso.onmicrosoft.com" `
    -azureAccountId serviceadmin@contoso.onmicrosoft.com
    ```
    
5. Drücken Sie bei beiden Aufforderungen die EINGABETASTE.
6. Geben Sie im Popup-Anmeldefenster die Anmeldeinformationen Ihres Azure-Abonnements ein.

## <a name="verify-the-registration"></a>Überprüfen der Registrierung

1. Melden Sie sich im Administratorportal (https://adminportal.local.azurestack.external) an.
2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen).
3. Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich.

## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)



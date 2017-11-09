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
ms.date: 10/27/2017
ms.author: erikje
ms.openlocfilehash: 24cde66a132ae2e1ba0eb9b1564915746e5ca448
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Registrieren Sie Azure Stack in Ihrem Azure-Abonnement.

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können [Azure Stack](azure-stack-poc.md) in Azure registrieren, um Marketplace-Elemente aus Azure herunterzuladen und das Senden von Berichten zu Geschäftsdaten an Microsoft einzurichten. 

> [!NOTE]
>Die Registrierung wird empfohlen, da sie dadurch wichtige Azure Stack-Funktionen testen können, wie z.B. die Marketplace-Syndikation oder Nutzungsberichte. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung durch Benutzer des Azure Stack Development Kits wird diesen nicht in Rechnung gestellt.
>


## <a name="get-azure-subscription"></a>Abschließen eines Azure-Abonnements

Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Die ID erhalten Sie, indem Sie sich bei Azure anmelden und auf **Weitere Dienste** > **Abonnements** und dann auf das Abonnement klicken, das Sie verwenden möchten. Unter **Essentials** (Zusammenfassung) sehen Sie nun die **Abonnement-ID**. Abonnements der Government-Clouds von China, Deutschland und den USA werden aktuell nicht unterstützt.
- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist (Konten mit MSA/2FA werden unterstützt).
- Sie müssen wissen, welches Azure Active Directory-Verzeichnis für das Azure-Abonnement verwendet wird. Dieses Verzeichnis können Sie in Azure finden, indem Sie mit der Maus auf Ihr Profilbild zeigen, das sich in der oberen rechten Ecke im Azure-Portal befindet. 
- [Sie müssen den Azure Stack-Ressourcenanbieter registriert haben.](#register-azure-stack-resource-provider-in-azure)

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
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```


## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure

> [!NOTE]
>Diese Schritte müssen auf dem Hostcomputer durchgeführt werden.
>

1. [Installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md). 
2. Kopieren Sie das [Skript „RegisterWithAzure.psm1“](https://go.microsoft.com/fwlink/?linkid=842959) in einen Ordner (z.B. „C:\Temp“).
3. Starten Sie PowerShell ISE als Administrator, und importieren Sie das Modul RegisterWithAzure.    
4. Führen Sie das Modul Add-AzsRegistration aus dem Skript „RegisterWithAzure.psm1“ heraus aus. Ersetzen Sie die folgenden Platzhalter: 
    - *YourCloudAdminCredential* ist ein PowerShell-Objekt, das die Anmeldeinformationen für die lokale Domäne „Domäne\cloudadmin“ enthält (beim Development Kit ist dies „azurestack\cloudadmin“).
    - *YourAzureSubscriptionID* ist die ID des Azure-Abonnements, das Sie zur Registrierung von Azure Stack verwenden möchten.
    - *YourAzureDirectoryTenantName* ist der Name des Azure-Mandantenverzeichnisses, das Ihrem Azure-Abonnement zugeordnet ist. Die Registrierungsressource wird in diesem Verzeichnismandanten erstellt. 
    - *YourPrivilegedEndpoint* ist der Name des [privilegierten Endpunkts](azure-stack-privileged-endpoint.md).

    ```powershell
    Add-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -AzureDirectoryTenantName $YourAzureDirectoryTenantName  -AzureSubscriptionId $YourAzureSubscriptionId -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Development 
    ```
5. Geben Sie im Popup-Anmeldefenster die Anmeldeinformationen Ihres Azure-Abonnements ein.

## <a name="verify-the-registration"></a>Überprüfen der Registrierung

1. Melden Sie sich im Administratorportal (https://adminportal.local.azurestack.external) an.
2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen).
3. Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich.

> [!NOTE]
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)


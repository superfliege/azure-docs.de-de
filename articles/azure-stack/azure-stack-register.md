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
ms.date: 11/21/2017
ms.author: erikje
ms.openlocfilehash: 6ce8f86592ece59e338578be86c2cb673c35dbc1
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
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

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/en-us/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.


## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. Beim Azure Stack Development Kit wäre dies der Hostcomputer. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener.
>

1. Öffnen Sie eine PowerShell-Konsole als Administrator, und [installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).  

2. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet werden soll. Führen Sie dazu das Cmdlet `Add-AzureRmAccount` aus, und legen Sie den EnvironmentName-Parameter auf „AzureCloud“ fest. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden. 

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Registrieren Sie den Azure Stack-Ressourcenanbieter im Azure-Abonnement. Führen Sie zu diesem Zweck den folgenden Befehl aus:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Löschen Sie alle vorhandenen Versionen der Powershell-Module, die der Registrierung entsprechen, und [laden Sie die neueste Version von GitHub herunter](azure-stack-powershell-download.md).  

6. Navigieren Sie vom Verzeichnis „AzureStack-Tools-master“, das im vorherigen Schritt erstellt wurde, zum Ordner „Registration“, und importieren Sie das Modul „.\RegisterWithAzure.psm1“:  

   ```powershell 
   Import-Module .\RegisterWithAzure.psm1 
   ```

7. Führen Sie in der gleichen PowerShell-Sitzung das folgende Skript aus. Wenn Sie zum Eingeben von Anmeldeinformationen aufgefordert werden, geben Sie `azurestack\cloudadmin` als Benutzer an. Das Kennwort ist das gleiche, das Sie während der Bereitstellung für den lokalen Administrator verwendet haben.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Add-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -AzureSubscriptionId $AzureContext.Subscription.SubscriptionId `
       -AzureDirectoryTenantName $AzureContext.Tenant.TenantId `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development 
   ```

   | Parameter | Beschreibung |
   | -------- | ------------- |
   | CloudAdminCredential | Die Anmeldeinformationen für die Clouddomäne, die für den [Zugriff auf den privilegierten Endpunkt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) verwendet werden. Der Benutzername hat das Format „\<Azure Stack-Domäne\>\cloudadmin“. Beim Development Kit wird der Benutzername auf „azurestack\cloudadmin“ festgelegt. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten.|
   | AzureSubscriptionId | Das Azure-Abonnement, das Sie zur Registrierung von Azure Stack verwenden.|
   | AzureDirectoryTenantName | Name des Azure-Mandantenverzeichnisses, das Ihrem Azure-Abonnement zugeordnet ist. Die Registrierungsressource wird in diesem Verzeichnismandanten erstellt. |
   | PrivilegedEndpoint | Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Beim Development Kit ist der privilegierte Endpunkt auf dem virtuellen Computer „AzS-ERCS01“ gehostet. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten. Weitere Informationen finden Sie unter dem Thema [Verwenden des privilegierten Endpunkts](azure-stack-privileged-endpoint.md).|
   | BillingModel | Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Zulässige Werte für diesen Parameter sind „Capacity“, „PayAsYouUse“ und „Development“. Beim Development Kit wird dieser Wert auf „Development“ gesetzt. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten. |

8. Nach Abschluss des Skripts wird eine Meldung „Azure Stack aktivieren (dieser Schritt kann bis zu 10 Minuten dauern)“ angezeigt. 

## <a name="verify-the-registration"></a>Überprüfen der Registrierung

1. Melden Sie sich im Administratorportal (https://adminportal.local.azurestack.external) an.
2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen).
3. Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich.

> [!NOTE]
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)


---
title: Registrieren von Azure Stack | Microsoft-Dokumentation
description: Registrieren Sie Azure Stack in Ihrem Azure-Abonnement.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/27/2018
ms.author: jeffgilb
ms.openlocfilehash: 8d520e1917d63b56a2adf3823ddbb741721fe751
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Registrieren Sie Azure Stack in Ihrem Azure-Abonnement.

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können [Azure Stack](azure-stack-poc.md) in Azure registrieren, um Marketplace-Elemente aus Azure herunterzuladen und das Senden von Berichten zu Geschäftsdaten an Microsoft einzurichten.

> [!NOTE]
>Die Registrierung wird empfohlen, da sie dadurch wichtige Azure Stack-Funktionen testen können, wie z.B. die Marketplace-Syndikation oder Nutzungsberichte. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung durch Benutzer des Azure Stack Development Kits wird diesen nicht in Rechnung gestellt.


## <a name="get-azure-subscription"></a>Abschließen eines Azure-Abonnements

Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Die ID erhalten Sie, indem Sie sich bei Azure anmelden und auf **Weitere Dienste** > **Abonnements** und dann auf das Abonnement klicken, das Sie verwenden möchten. Unter **Essentials** (Zusammenfassung) sehen Sie nun die **Abonnement-ID**. Abonnements der Government-Clouds von China, Deutschland und den USA werden aktuell nicht unterstützt.
- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist (Konten mit MSA/2FA werden unterstützt).
- *Ab der Azure Stack-Updateversion 1712 (180106.1) nicht erforderlich*: Das Azure Active Directory für das Azure-Abonnement. Dieses Verzeichnis können Sie in Azure finden, indem Sie mit der Maus auf Ihr Profilbild zeigen, das sich in der oberen rechten Ecke im Azure-Portal befindet.

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/en-us/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.

## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure  
> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. Beim Azure Stack Development Kit wäre dies der Hostcomputer. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener.
>

1. Öffnen Sie eine PowerShell-Konsole als Administrator, und [installieren Sie PowerShell für Azure Stack](azure-stack-powershell-install.md).  

2. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet wird. Führen Sie zum Hinzufügen des Kontos das Cmdlet `Add-AzureRmAccount` aus, und legen Sie den Parameter „EnvironmentName“ auf **AzureCloud** fest. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Führen Sie den folgenden Befehl zum Registrieren des Azure Stack-Ressourcenanbieters in Ihrem Azure-Abonnement aus:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Löschen Sie alle vorhandenen Versionen der PowerShell-Module, die der Registrierung entsprechen, und [laden Sie die neueste Version von GitHub herunter](azure-stack-powershell-download.md).  

6. Navigieren Sie vom Verzeichnis „AzureStack-Tools-master“, das im vorherigen Schritt erstellt wurde, zum Ordner „Registration“, und importieren Sie das Modul „.\RegisterWithAzure.psm1“:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. Führen Sie in derselben PowerShell-Sitzung das folgende Skript aus: Wenn Sie zum Eingeben von Anmeldeinformationen aufgefordert werden, geben Sie `azurestack\cloudadmin` als Benutzer an. Das Kennwort ist das gleiche, das Sie während der Bereitstellung für den lokalen Administrator verwendet haben.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | Parameter | BESCHREIBUNG |  
   |--------|-------------|
   | CloudAdminCredential | Die Anmeldeinformationen für die Clouddomäne, die für den [Zugriff auf den privilegierten Endpunkt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) verwendet werden. Der Benutzername hat das Format **\<Azure Stack-Domäne\>\cloudadmin**. Beim Development Kit wird der Benutzername auf **azurestack\cloudadmin** festgelegt. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten.|  
   | PrivilegedEndpoint | Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Beim Development Kit ist der privilegierte Endpunkt auf dem virtuellen Computer „AzS-ERCS01“ gehostet. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten. Weitere Informationen finden Sie im Artikel [Verwenden des privilegierten Endpunkts](azure-stack-privileged-endpoint.md).|  
   | BillingModel | Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Zulässige Werte für diesen Parameter sind **Capacity**, **PayAsYouUse** und **Development**. Beim Development Kit wird dieser Wert auf **Development** festgelegt. Wenn Sie ein integriertes System verwenden, wenden Sie sich an Ihren Azure Stack-Bediener, um diesen Wert zu erhalten. |

8. Nach Abschluss des Skripts wird eine Meldung „Azure Stack aktivieren (dieser Schritt kann bis zu 10 Minuten dauern)“ angezeigt.




## <a name="verify-the-registration"></a>Überprüfen der Registrierung  

1. Melden Sie sich im Administratorportal (https://adminportal.local.azurestack.external) an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen).

3. Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich.

> [!NOTE]
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.


## <a name="modify-the-registration"></a>Ändern der Registrierung

### <a name="change-the-subscription-you-use"></a>Ändern des verwendeten Abonnements
Wenn Sie das von Ihnen verwendete Abonnement ändern möchten, müssen Sie zuerst das Cmdlet „Remove-AzsRegistration“ ausführen. Dann müssen sicherstellen, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind, und anschließend „Set-AzsRegistration“ mit allen geänderten Parametern ausführen.

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>Ändern des Abrechnungsmodells oder der Syndikationsfeatures
Wenn Sie das Abrechnungsmodell oder die Syndikationsfeatures für Ihre Installation ändern möchten, können Sie die Registrierungsfunktion aufrufen, um die neuen Werte festzulegen. Sie müssen die aktuelle Registrierung nicht zuvor entfernen.  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>Nicht mit Azure verbundene Registrierung
*Die Informationen in diesem Abschnitt gelten ab der Azure Stack-Updateversion 1712 (180106.1) und werden von früheren Versionen nicht unterstützt.*

Wenn Sie Azure Stack in einer nicht mit Azure verbundenen Umgebung registrieren, müssen Sie ein Registrierungstoken aus der Azure Stack-Umgebung abrufen und dieses Token dann auf einem Computer verwenden, der zur Registrierung eine Verbindung mit Azure herstellen kann.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Abrufen eines Registrierungstokens aus der Azure Stack-Umgebung
  1. Führen Sie zum Abrufen des Registrierungstokens Folgendes aus:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > Das Registrierungstoken wird in der Datei gespeichert, die für *$env:SystemDrive\RegistrationToken.txt* angegeben ist.

  2. Speichern Sie dieses Registrierungstoken für die Verwendung auf einem mit Azure verbundenen Computer.


### <a name="connect-to-azure-and-register"></a>Verbinden mit Azure und Registrieren
Führen Sie die Schritte für diesen Vorgang auf einem Computer aus, der eine Verbindung mit Azure herstellen kann.

  1. [Laden Sie von GitHub die neuesten PowerShell-Module herunter, die der Registrierung entsprechen](azure-stack-powershell-download.md).  

  2. Navigieren Sie vom Verzeichnis „AzureStack-Tools-master“, das im vorherigen Schritt erstellt wurde, zum Ordner „Registration“, und importieren Sie das Modul „.\RegisterWithAzure.psm1“:  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. Kopieren Sie [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) in einen Ordner, z.B. *C:\Temp*.

  4. Starten Sie PowerShell ISE als Administrator, und importieren Sie dann das Modul „RegisterWithAzure“.  

  5. Stellen Sie sicher, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind, den Sie zum Registrieren Ihrer Azure Stack-Umgebung verwenden möchten:  

     ```Powershell
        Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. Geben Sie das Registrierungstoken für die Registrierung bei Azure an:

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
    Optional können Sie mit dem Cmdlet „Get-Content“ auf eine Datei zeigen, die Ihr Registrierungstoken enthält:

    ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
    ```
> [!NOTE]  
> Speichern Sie den Namen der Registrierungsressource oder das Registrierungstoken zur späteren Bezugnahme.

### <a name="remove-a-registered-resource"></a>Entfernen einer registrierten Ressource
Wenn Sie die Registrierungsressource entfernen möchten, müssen Sie das Cmdlet „UnRegister-AzsEnvironment“ verwenden und entweder den Namen der Registrierungsressource oder das Registrierungstoken übergeben, das Sie für „Register-AzsEnvironment“ verwendet haben.
- **Name der Registrierungsressource:**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **Registrierungstoken:**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)

---
title: Registrieren von in Azure Stack integrierten Systemen in Azure | Microsoft-Dokumentation
description: Informationen zum Prozess der Registrierung in Azure für mit Azure verbundene Bereitstellungen von Azure Stack-Systemen mit mehreren Knoten.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 65525ffe33ddc100dd3066e7c2b52ef8a856fbc3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37933488"
---
# <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure

Wenn Sie [Azure Stack](azure-stack-poc.md) bei Azure registrieren, können Sie Marketplace-Elemente aus Azure herunterladen und das Senden von Berichten zu Geschäftsdaten an Microsoft einrichten. Nach der Registrierung von Azure Stack wird die Nutzung an Azure Commerce gemeldet und kann unter dem für die Registrierung verwendeten Abonnement angezeigt werden.

> [!IMPORTANT]  
> Die Registrierung ist für die uneingeschränkte Unterstützung aller Azure Stack-Funktionen (einschließlich Marketplace-Syndikation) erforderlich. Außerdem verstoßen Sie gegen die Lizenzbedingungen von Azure Stack, wenn Sie das nutzungsbasierte Abrechnungsmodell verwenden, ohne sich zu registrieren. Weitere Informationen zu den Azure Stack-Lizenzierungsmodellen finden Sie auf der Seite [Azure Stack erwerben](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Voraussetzungen

Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Die ID erhalten Sie, indem Sie sich bei Azure anmelden und auf **Weitere Dienste** > **Abonnements** und dann auf das Abonnement klicken, das Sie verwenden möchten. Unter **Zusammenfassung** sehen Sie nun die Abonnement-ID.

  > [!NOTE]
  > Cloudabonnements für Deutschland und US Government werden derzeit nicht unterstützt.

- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist (Konten mit MSA/2FA werden unterstützt).
- Sie müssen den Azure Stack-Ressourcenanbieter registriert haben. (Weitere Informationen finden Sie weiter unten im Abschnitt „Registrieren des Azure Stack-Ressourcenanbieters“.)

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.

### <a name="powershell-language-mode"></a>PowerShell-Sprachmodus

Zur erfolgreichen Registrierung von Azure Stack muss der PowerShell-Sprachmodus auf **FullLanguageMode** festgelegt werden.  Um zu überprüfen, ob der aktuelle Sprachmodus auf vollständig festgelegt ist, öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden PowerShell-Befehle aus:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Vergewissern Sie sich, dass in der Ausgabe **FullLanguageMode** zurückgegeben wird. Wenn ein anderer Sprachmodus zurückgegeben wird, muss die Registrierung auf einem anderen Computer ausgeführt oder der Sprachmodus auf **FullLanguageMode** festgelegt werden, damit Sie fortfahren können.

### <a name="bkmk_powershell"></a>Installieren von PowerShell für Azure Stack

Für die Registrierung bei Azure muss die neueste Version von PowerShell für Azure Stack verwendet werden.

Falls noch nicht geschehen, [installieren Sie PowerShell für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>Herunterladen der Azure Stack-Tools

Das GitHub-Repository mit den Azure Stack-Tools enthält PowerShell-Module, die Azure Stack-Funktionalität einschließlich Registrierungsfunktionalität unterstützen. Während des Registrierungsprozesses müssen Sie das PowerShell-Modul „RegisterWithAzure.psm1“ importieren und verwenden, das sich im Repository mit den Azure Stack-Tools befindet, um Ihre Azure Stack-Instanz bei Azure zu registrieren.

Um sicherzustellen, dass Sie die neueste Version verwenden, sollten Sie vor der Registrierung bei Azure alle vorhandenen Versionen der Azure Stack-Tools löschen und [die neueste Version von GitHub herunterladen](azure-stack-powershell-download.md).

## <a name="register-azure-stack-in-connected-environments"></a>Registrieren von Azure Stack in mit Azure verbundenen Umgebungen

In mit Azure verbundenen Umgebungen kann auf das Internet und Azure zugegriffen werden. Für diese Umgebungen müssen Sie den Azure Stack-Ressourcenanbieter bei Azure registrieren und dann Ihr Abrechnungsmodell konfigurieren.

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat.

### <a name="register-the-azure-stack-resource-provider"></a>Registrieren des Azure Stack-Ressourcenanbieters

Zum Registrieren des Azure Stack-Ressourcenanbieters bei Azure starten Sie PowerShell ISE als Administrator und verwenden die folgenden PowerShell-Befehle mit dem auf den entsprechenden Azure-Abonnementtyp festgelegten Parameter **EnvironmentName** (siehe die Parameter unten).

1. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet wird. Führen Sie zum Hinzufügen des Kontos das Cmdlet **Add-AzureRmAccount** aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Konto des globalen Azure-Administrators einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parameter | BESCHREIBUNG |  
   |-----|-----|
   | EnvironmentName | Der Umgebungsname des Azure-Cloudabonnements. Unterstützte Umgebungsnamen sind **AzureCloud** oder für ein Azure-Abonnement für China **AzureChinaCloud**.  |
   |  |  |

2. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Führen Sie den folgenden Befehl zum Registrieren des Azure Stack-Ressourcenanbieters in Ihrem Azure-Abonnement aus:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells

Befolgen Sie diese Schritte zum Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells.

1. Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1** mit PowerShell:

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Stellen Sie als Nächstes in derselben PowerShell-Sitzung sicher, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind. Dies ist das Azure-Konto, das verwendet wurde, um den oben genannten Azure Stack-Ressourcenanbieter zu registrieren. Führen Sie diesen PowerShell-Befehl aus:

   ```powershell
   Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>"
   ```

3. Führen Sie in der gleichen PowerShell-Sitzung das Cmdlet **Set-AzsRegistration** aus. Führen Sie diesen PowerShell-Befehl aus:  

   ```powershell
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse
   ```

  |Parameter|BESCHREIBUNG|
  |-----|-----|
  |PrivilegedEndpointCredential|Die Anmeldeinformationen, die für den [Zugriff auf den privilegierten Endpunkt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) verwendet werden. Der Benutzername hat das Format **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Weitere Informationen finden Sie im Artikel [Verwenden des privilegierten Endpunkts](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).|
  |BillingModel|Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Zulässige Werte für diesen Parameter sind „Capacity“, „PayAsYouUse“ und „Development“.|
  |  |  |

  Der Vorgang dauert 10 bis 15 Minuten. Wenn der Befehl abgeschlossen ist, sehen Sie die Meldung, dass **Ihre Umgebung nun registriert und mit den angegebenen Parametern aktiviert wurde**.

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registrieren von Azure Stack bei Azure unter Verwendung des kapazitätsbasierten Abrechnungsmodells

Befolgen Sie die gleichen Anweisungen wie bei der Registrierung mit dem nutzungsbasierten Abrechnungsmodell. Fügen Sie jedoch die Vertragsnummer hinzu, unter der Kapazität erworben wurde, und ändern Sie den Parameter **BillingModel** in **Capacity**. Alle anderen Parameter bleiben unverändert.

Führen Sie diesen PowerShell-Befehl aus:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registrieren von Azure Stack in nicht mit Azure verbundenen Umgebungen
Wenn Sie Azure Stack in einer nicht mit Azure verbundenen Umgebung (ohne Internetverbindung) registrieren, müssen Sie ein Registrierungstoken aus der Azure Stack-Umgebung abrufen und dieses Token dann auf einem Computer verwenden, der eine Verbindung mit Azure herstellen kann und auf dem [PowerShell für Azure Stack installiert ist](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Abrufen eines Registrierungstokens aus der Azure Stack-Umgebung

1. Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1**:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Führen Sie die folgenden PowerShell-Befehle zum Abrufen des Registrierungstokens aus:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > Das Registrierungstoken wird in der Datei gespeichert, die für *$FilePathForRegistrationToken* angegeben ist. Sie können den Dateipfad oder den Dateinamen nach eigenem Ermessen ändern.

3. Speichern Sie dieses Registrierungstoken für die Verwendung auf einem mit Azure verbundenen Computer. Sie können die Datei oder den Text aus „$FilePathForRegistrationToken“ kopieren.

### <a name="connect-to-azure-and-register"></a>Verbinden mit Azure und Registrieren

Führen Sie auf dem Computer, der mit dem Internet verbunden ist, die gleichen Schritte aus, um das Modul „RegisterWithAzure.psm1“ zu importieren und sich im richtigen Azure PowerShell-Kontext anzumelden. Rufen Sie dann „Register-AzsEnvironment“ auf, und geben Sie das Registrierungstoken für die Registrierung bei Azure an:

  ```Powershell  
  $registrationToken = "<Your Registration Token>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

Optional können Sie mit dem Cmdlet „Get-Content“ auf eine Datei zeigen, die Ihr Registrierungstoken enthält:

  ```Powershell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

  > [!NOTE]  
  > Speichern Sie den Namen der Registrierungsressource und das Registrierungstoken zur späteren Bezugnahme.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Abrufen eines Aktivierungsschlüssels aus der Azure-Registrierungsressource

Als Nächstes müssen Sie einen Aktivierungsschlüssel aus der Registrierungsressource abrufen, die in Azure während der Ausführung von „Register-AzsEnvironment“ erstellt wurde.

Führen Sie die folgenden PowerShell-Befehle zum Abrufen des Aktivierungsschlüssels aus:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > Der Aktivierungsschlüssel wird in der für *$KeyOutputFilePath* angegebenen Datei gespeichert. Sie können den Dateipfad oder den Dateinamen nach eigenem Ermessen ändern.

### <a name="create-an-activation-resource-in-azure-stack"></a>Erstellen einer Aktivierungsressource in Azure Stack

Kehren Sie mit der Datei oder dem Text aus dem Aktivierungsschlüssel, der von „Get-AzsActivationKey“ erstellt wurde, zur Azure Stack-Umgebung zurück. Als Nächstes erstellen Sie mit diesem Aktivierungsschlüssel eine Aktivierungsressource in Azure Stack. Führen Sie die folgenden PowerShell-Befehle zum Erstellen einer Aktivierungsressource aus:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

Optional können Sie mit dem Cmdlet „Get-Content“ auf eine Datei zeigen, die Ihr Registrierungstoken enthält:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Überprüfen der Azure Stack-Registrierung

Gehen Sie folgendermaßen vor, um sicherzustellen, dass Azure Stack erfolgreich in Azure registriert wurde.

1. Melden Sie sich beim Azure Stack-[Administratorportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal) an: https&#58;//adminportal.*&lt;region>.&lt;fqdn>*.
2. Wählen Sie **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung)  > **Add from Azure** (Aus Azure hinzufügen) aus.

Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich. Allerdings werden in nicht verbundenen Umgebungen Azure Marketplace-Elemente nicht im Azure Stack-Marketplace angezeigt.

> [!NOTE]
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.

## <a name="renew-or-change-registration"></a>Erneuern oder Ändern der Registrierung

### <a name="renew-or-change-registration-in-connected-environments"></a>Erneuern oder Ändern der Registrierung in verbundenen Umgebungen

Unter den folgenden Umständen müssen Sie Ihre Registrierung aktualisieren oder erneuern:

- Nachdem Sie Ihr kapazitätsbasiertes Jahresabonnement verlängert haben.
- Wenn Sie Ihr Abrechnungsmodell ändern.
- Wenn Sie Änderungen für die kapazitätsbasierte Abrechnung vornehmen (Knoten hinzufügen/entfernen).

#### <a name="change-the-subscription-you-use"></a>Ändern des verwendeten Abonnements

Wenn Sie das von Ihnen verwendete Abonnement ändern möchten, müssen Sie zuerst das Cmdlet **Remove-AzsRegistration** ausführen. Dann müssen Sie sicherstellen, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind, und anschließend **Set-AzsRegistration** mit allen geänderten Parametern ausführen:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Ändern des Abrechnungsmodells oder der Syndikationsfeatures

Wenn Sie das Abrechnungsmodell oder die Syndikationsfeatures für Ihre Installation ändern möchten, können Sie die Registrierungsfunktion aufrufen, um die neuen Werte festzulegen. Sie müssen die aktuelle Registrierung nicht zuvor entfernen:

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Erneuern oder Ändern der Registrierung in nicht verbundenen Umgebungen

Unter den folgenden Umständen müssen Sie Ihre Registrierung aktualisieren oder erneuern:

- Nachdem Sie Ihr kapazitätsbasiertes Jahresabonnement verlängert haben.
- Wenn Sie Ihr Abrechnungsmodell ändern.
- Wenn Sie Änderungen für die kapazitätsbasierte Abrechnung vornehmen (Knoten hinzufügen/entfernen).

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Entfernen der Aktivierungsressource aus Azure Stack

Sie müssen zuerst die Aktivierungsressource aus Azure Stack und dann die Registrierungsressource in Azure entfernen.  

Um die Aktivierungsressource in Azure Stack zu entfernen, führen Sie die folgenden PowerShell-Befehle in Ihrer Azure Stack-Umgebung aus:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Um die Registrierungsressource in Azure zu entfernen, stellen Sie als Nächstes sicher, dass Sie einen mit Azure verbundenen Computer nutzen. Melden Sie sich beim richtigen Azure PowerShell-Kontext an, und führen Sie die entsprechenden PowerShell-Befehle aus, wie unten beschrieben.

Sie können das Registrierungstoken verwenden, mit dem die Ressource erstellt wurde:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Alternativ können Sie den Registrierungsnamen nutzen:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Erneutes Registrieren in einem nicht verbundenen Szenario

Sie haben jetzt die Registrierung in einem nicht verbundenen Szenario vollständig aufgehoben und müssen die Schritte zum Registrieren einer Azure Stack-Umgebung in einem nicht verbundenen Szenario wiederholen.

## <a name="next-steps"></a>Nächste Schritte

[Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)

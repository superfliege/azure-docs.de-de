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
ms.date: 12/04/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 58dfb3f02b338d62fcfb10e4d8c1bc492cdacbda
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890550"
---
# <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure

Wenn Sie Azure Stack bei Azure registrieren, können Sie Marketplace-Elemente aus Azure herunterladen und das Senden von Berichten zu Geschäftsdaten an Microsoft einrichten. Nach der Registrierung von Azure Stack wird die Nutzung an Azure Commerce gemeldet und kann unter dem für die Registrierung verwendeten Abonnement angezeigt werden.

In diesem Artikel wird das Registrieren von in Azure Stack integrierten Systemen bei Azure beschrieben. Informationen zum Registrieren des ASDK bei Azure finden Sie in der ASDK-Dokumentation unter [Azure Stack-Registrierung](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-register).

> [!IMPORTANT]  
> Die Registrierung ist für die uneingeschränkte Unterstützung aller Azure Stack-Funktionen einschließlich des Angebots von Elementen im Marketplace erforderlich. Außerdem verstoßen Sie gegen die Lizenzbedingungen von Azure Stack, wenn Sie das nutzungsbasierte Abrechnungsmodell verwenden, ohne sich zu registrieren. Weitere Informationen zu den Azure Stack-Lizenzierungsmodellen finden Sie auf der Seite [Azure Stack erwerben](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Voraussetzungen

Vor der Registrierung müssen Sie folgende Aufgaben durchführen:

 - Überprüfen Ihrer Anmeldeinformationen
 - Festlegen des PowerShell-Sprachmodus
 - Installieren von PowerShell für Azure Stack
 - Herunterladen der Azure Stack-Tools
 - Bestimmen Ihres Registrierungsszenarios

### <a name="verify-your-credentials"></a>Überprüfen Ihrer Anmeldeinformationen

Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Nur Abonnements freigegebener EA-, CSP- oder CSP-Dienste werden für die Registrierung unterstützt. CSPs müssen entscheiden, ob ein [CSP- oder APSS-Abonnement verwendet werden soll](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription).<br><br>Um die ID abzurufen, melden Sie sich bei Azure an, und klicken Sie auf **Alle Dienste**. Wählen Sie dann unter der Kategorie **ALLGEMEIN** die Option **Abonnements** aus, und klicken Sie auf das gewünschte Abonnement. Unter **Zusammenfassung** finden Sie die Abonnement-ID.

  > [!Note]  
  > Cloudabonnements für Deutschland werden derzeit nicht unterstützt.

- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist.

- Das Benutzerkonto muss Zugriff auf das Azure-Abonnement besitzen und berechtigt sein, Identitätsanwendungen und Dienstprinzipale in dem Verzeichnis zu erstellen, das diesem Abonnement zugeordnet ist.

- Sie müssen den Azure Stack-Ressourcenanbieter registriert haben. (Weitere Informationen finden Sie weiter unten im Abschnitt „Registrieren des Azure Stack-Ressourcenanbieters“.)

Nach der Registrierung ist globale Azure Active Directory-Administratorberechtigung nicht erforderlich. Einige Vorgänge erfordern jedoch möglicherweise die Anmeldeinformationen für den globalen Administrator. Beispielsweise ein Ressourcenanbieter-Installationsskript oder ein neues Feature, dem eine Berechtigung erteilt werden muss. Sie können entweder die globalen Administratorrechte des Kontos vorübergehend wiederherstellen oder ein separates globales Administratorkonto verwenden, das Besitzer des *Standardanbieterabonnements* ist.

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.

### <a name="powershell-language-mode"></a>PowerShell-Sprachmodus

Zur erfolgreichen Registrierung von Azure Stack muss der PowerShell-Sprachmodus auf **FullLanguageMode** festgelegt werden.  Um zu überprüfen, ob der aktuelle Sprachmodus auf vollständig festgelegt ist, öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden PowerShell-Cmdlets aus:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Vergewissern Sie sich, dass in der Ausgabe **FullLanguageMode** zurückgegeben wird. Wenn ein anderer Sprachmodus zurückgegeben wird, muss die Registrierung auf einem anderen Computer ausgeführt oder der Sprachmodus auf **FullLanguageMode** festgelegt werden, damit Sie fortfahren können.

### <a name="install-powershell-for-azure-stack"></a>Installieren von PowerShell für Azure Stack

Für die Registrierung bei Azure muss die neueste Version von PowerShell für Azure Stack verwendet werden.

Siehe [Installieren von PowerShell für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install), falls die aktuelle Version noch nicht installiert ist.

### <a name="download-the-azure-stack-tools"></a>Herunterladen der Azure Stack-Tools

Das GitHub-Repository mit den Azure Stack-Tools enthält PowerShell-Module, die Azure Stack-Funktionalität einschließlich Registrierungsfunktionalität unterstützen. Während des Registrierungsprozesses müssen Sie das PowerShell-Modul **RegisterWithAzure.psm1** importieren und verwenden, das sich im Repository mit den Azure Stack-Tools befindet, um Ihre Azure Stack-Instanz bei Azure zu registrieren.

Um sicherzustellen, dass Sie die neueste Version verwenden, sollten Sie vor der Registrierung bei Azure alle vorhandenen Versionen der Azure Stack-Tools löschen und [die neueste Version von GitHub herunterladen](azure-stack-powershell-download.md).

### <a name="determine-your-registration-scenario"></a>Bestimmen Ihres Registrierungsszenarios

Ihre Azure Stack-Bereitstellung kann *verbunden* oder *nicht verbunden* sein.

 - **Verbunden**  
 Verbunden bedeutet, dass Sie die Azure Stack-Instanz so bereitgestellt haben, dass sie eine Verbindung mit dem Internet und mit Azure herstellen kann. Als Identitätsspeicher verwenden Sie entweder Azure Active Directory (Azure AD) oder Active Directory-Verbunddienste (AD FS). Bei einer verbundenen Bereitstellung können Sie zwischen zwei Abrechnungsmodellen wählen: nutzungsbasiert oder kapazitätsbasiert.
    - [Registrieren einer verbundenen Azure Stack-Instanz bei Azure unter Verwendung des **nutzungsbasierten** Abrechnungsmodells](#register-connected-with-pay-as-you-go-billing)
    - [Registrieren einer verbundenen Azure Stack-Instanz bei Azure unter Verwendung des **kapazitätsbasierten** Abrechnungsmodells](#register-connected-with-capacity-billing)

 - **Nicht verbunden**  
 Bei der Bereitstellungsoption ohne Verbindung mit Azure können Sie Azure Stack auch ohne Verbindung mit dem Internet bereitstellen und nutzen. Bei einer nicht mit Azure verbundenen Bereitstellung sind Sie jedoch auf einen AD FS-Identitätsspeicher und das kapazitätsbasierte Abrechnungsmodell beschränkt.
    - [Registrieren einer nicht verbundenen Azure Stack-Instanz unter Verwendung des **kapazitätsbasierten** Abrechnungsmodells](#register-disconnected-with-capacity-billing)

### <a name="determine-a-unique-registration-name-to-use"></a>Bestimmen eines eindeutigen zu verwendenden Registrierungsnamens 
Wenn Sie Azure Stack in Azure registrieren, müssen Sie einen eindeutige Registrierungsnamen angeben. Eine einfache Möglichkeit, Ihr Azure Stack-Abonnement mit einer Azure-Registrierung zu verknüpfen, ist die Verwendung der **Cloud-ID** Ihres Azure Stack. 

> [!NOTE]
> Azure Stack-Registrierungen, die das kapazitätsbasierte Abrechnungsmodell verwenden, müssen den eindeutigen Namen ändern, wenn sie sich nach Ablauf dieser jährlichen Abonnements erneut registrieren, es sei denn, Sie [löschen die abgelaufene Registrierung](azure-stack-registration.md#change-the-subscription-you-use) und führen die Registrierung bei Azure erneut aus.

Um die Cloud-ID für Ihre Azure Stack-Bereitstellung zu bestimmen, öffnen Sie PowerShell als Administrator auf einem Computer, der auf den privilegierten Endpunkt zugreifen kann, führen Sie die folgenden Befehle aus, und speichern Sie den **CloudID**-Wert: 

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: get-azurestackstampinformation 
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Registrieren einer verbundenen Instanz mit nutzungsbasierter Abrechnung

Befolgen Sie diese Schritte zum Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells.

> [!Note]  
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt (PEP) hat. Details zum PEP finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md).

In mit Azure verbundenen Umgebungen kann auf das Internet und Azure zugegriffen werden. Für diese Umgebungen müssen Sie den Azure Stack-Ressourcenanbieter bei Azure registrieren und dann Ihr Abrechnungsmodell konfigurieren.

1. Zum Registrieren des Azure Stack-Ressourcenanbieters bei Azure starten Sie PowerShell ISE als Administrator, und verwenden Sie die folgenden PowerShell-Cmdlets mit dem auf den entsprechenden Azure-Abonnementtyp festgelegten Parameter **EnvironmentName** (siehe die Parameter unten).

2. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet wird. Führen Sie zum Hinzufügen des Kontos das Cmdlet **Add-AzureRmAccount** aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parameter | BESCHREIBUNG |  
   |-----|-----|
   | EnvironmentName | Der Umgebungsname des Azure-Cloudabonnements. Unterstützte Umgebungsnamen sind **AzureCloud**, **AzureUSGovernment** oder für ein Azure-Abonnement für China **AzureChinaCloud**.  |

3. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Führen Sie den folgenden Befehl zum Registrieren des Azure Stack-Ressourcenanbieters in Ihrem Azure-Abonnement aus:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1** mit PowerShell:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Stellen Sie als Nächstes in derselben PowerShell-Sitzung sicher, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind. Dies ist das Azure-Konto, das verwendet wurde, um den oben genannten Azure Stack-Ressourcenanbieter zu registrieren. Führen Sie diesen PowerShell-Befehl aus:

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parameter | BESCHREIBUNG |  
   |-----|-----|
   | EnvironmentName | Der Umgebungsname des Azure-Cloudabonnements. Unterstützte Umgebungsnamen sind **AzureCloud**, **AzureUSGovernment** oder für ein Azure-Abonnement für China **AzureChinaCloud**.  |

7. Führen Sie in der gleichen PowerShell-Sitzung das Cmdlet **Set-AzsRegistration** aus. Führen Sie diesen PowerShell-Befehl aus:  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   Weitere Informationen zum Cmdlet „Set-AzsRegistration“ finden Sie in der [Referenz zur Registrierung](#registration-reference).

  Der Vorgang dauert 10 bis 15 Minuten. Wenn der Befehl abgeschlossen ist, sehen Sie die Meldung, dass **Ihre Umgebung nun registriert und mit den angegebenen Parametern aktiviert wurde**.

## <a name="register-connected-with-capacity-billing"></a>Registrieren einer verbundenen Instanz mit kapazitätsbasierter Abrechnung

Befolgen Sie diese Schritte zum Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells.

> [!Note]  
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt (PEP) hat. Details zum PEP finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md).

In mit Azure verbundenen Umgebungen kann auf das Internet und Azure zugegriffen werden. Für diese Umgebungen müssen Sie den Azure Stack-Ressourcenanbieter bei Azure registrieren und dann Ihr Abrechnungsmodell konfigurieren.

1. Zum Registrieren des Azure Stack-Ressourcenanbieters bei Azure starten Sie PowerShell ISE als Administrator, und verwenden Sie die folgenden PowerShell-Cmdlets mit dem auf den entsprechenden Azure-Abonnementtyp festgelegten Parameter **EnvironmentName** (siehe die Parameter unten).

2. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet wird. Führen Sie zum Hinzufügen des Kontos das Cmdlet **Add-AzureRmAccount** aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Azure-Konto einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parameter | BESCHREIBUNG |  
   |-----|-----|
   | EnvironmentName | Der Umgebungsname des Azure-Cloudabonnements. Unterstützte Umgebungsnamen sind **AzureCloud**, **AzureUSGovernment** oder für ein Azure-Abonnement für China **AzureChinaCloud**.  |

3. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Führen Sie den folgenden Befehl zum Registrieren des Azure Stack-Ressourcenanbieters in Ihrem Azure-Abonnement aus:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1** mit PowerShell:

  ```PowerShell  
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  $RegistrationName = "<unique-registration-name>"
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity `
      -RegistrationName $RegistrationName
  ```
   > [!Note]  
   > Sie können Nutzungsberichte mit dem UsageReportingEnabled-Parameter für das Cmdlet **Set-AzsRegistration** deaktivieren, indem Sie den Parameter auf FALSE festlegen. 
   
  Weitere Informationen zum Cmdlet „Set-AzsRegistration“ finden Sie in der [Referenz zur Registrierung](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Registrieren einer nicht verbundenen Instanz mit kapazitätsbasierter Abrechnung

Wenn Sie Azure Stack in einer nicht mit Azure verbundenen Umgebung (ohne Internetverbindung) registrieren, müssen Sie ein Registrierungstoken aus der Azure Stack-Umgebung abrufen und dieses Token dann auf einem Computer verwenden, der eine Verbindung mit Azure herstellen kann und auf dem [PowerShell für Azure Stack installiert ist](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Abrufen eines Registrierungstokens aus der Azure Stack-Umgebung

1. Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1**:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Führen Sie die folgenden PowerShell-Cmdlets zum Abrufen des Registrierungstokens aus:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Weitere Informationen zum Cmdlet „Get-AzsRegistrationToken“ finden Sie in der [Referenz zur Registrierung](#registration-reference).

   > [!Tip]  
   > Das Registrierungstoken wird in der Datei gespeichert, die für *$FilePathForRegistrationToken* angegeben ist. Sie können den Dateipfad oder den Dateinamen nach eigenem Ermessen ändern.

3. Speichern Sie dieses Registrierungstoken für die Verwendung auf einem mit Azure verbundenen Computer. Sie können die Datei oder den Text aus „$FilePathForRegistrationToken“ kopieren.

### <a name="connect-to-azure-and-register"></a>Verbinden mit Azure und Registrieren

Führen Sie auf dem Computer, der mit dem Internet verbunden ist, die gleichen Schritte aus, um das Modul „RegisterWithAzure.psm1“ zu importieren und sich im richtigen Azure PowerShell-Kontext anzumelden. Rufen Sie dann „Register-AzsEnvironment“ auf. Geben Sie das Registrierungstoken für die Registrierung bei Azure an. Wenn Sie mehrere Instanzen von Azure Stack mit der gleichen Azure-Abonnement-ID registrieren, legen Sie einen eindeutigen Namen für die Registrierung fest. Führen Sie das folgende Cmdlet aus:

  ```PowerShell  
  $registrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  -RegistrationName $RegistrationName
  ```

Optional können Sie mit dem Cmdlet „Get-Content“ auf eine Datei zeigen, die Ihr Registrierungstoken enthält:

  ```PowerShell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > Speichern Sie den Namen der Registrierungsressource und das Registrierungstoken zur späteren Bezugnahme.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Abrufen eines Aktivierungsschlüssels aus der Azure-Registrierungsressource

Als Nächstes müssen Sie einen Aktivierungsschlüssel aus der Registrierungsressource abrufen, die in Azure während der Ausführung von „Register-AzsEnvironment“ erstellt wurde.

Führen Sie die folgenden PowerShell-Cmdlets zum Abrufen des Aktivierungsschlüssels aus:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > Der Aktivierungsschlüssel wird in der für *$KeyOutputFilePath* angegebenen Datei gespeichert. Sie können den Dateipfad oder den Dateinamen nach eigenem Ermessen ändern.

### <a name="create-an-activation-resource-in-azure-stack"></a>Erstellen einer Aktivierungsressource in Azure Stack

Kehren Sie mit der Datei oder dem Text aus dem Aktivierungsschlüssel, der von „Get-AzsActivationKey“ erstellt wurde, zur Azure Stack-Umgebung zurück. Als Nächstes erstellen Sie mit diesem Aktivierungsschlüssel eine Aktivierungsressource in Azure Stack. Führen Sie die folgenden PowerShell-Cmdlets zum Erstellen einer Aktivierungsressource aus:  

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

1. Melden Sie sich beim Azure Stack-[Verwaltungsportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal) an: https&#58;//adminportal.*&lt;region>.&lt;fqdn>*.
2. Wählen Sie **Alle Dienste** aus, und wählen Sie dann unter der Kategorie **VERWALTUNG** die Option **Marketplace-Verwaltung** > **Aus Azure hinzufügen** aus.

Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich. Allerdings werden in nicht verbundenen Umgebungen Azure Marketplace-Elemente nicht im Azure Stack-Marketplace angezeigt.

> [!Note]  
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.

## <a name="renew-or-change-registration"></a>Erneuern oder Ändern der Registrierung

### <a name="renew-or-change-registration-in-connected-environments"></a>Erneuern oder Ändern der Registrierung in verbundenen Umgebungen

Unter den folgenden Umständen müssen Sie Ihre Registrierung aktualisieren oder erneuern:

- Nachdem Sie Ihr kapazitätsbasiertes Jahresabonnement verlängert haben.
- Wenn Sie Ihr Abrechnungsmodell ändern.
- Wenn Sie Änderungen für die kapazitätsbasierte Abrechnung vornehmen (Knoten hinzufügen/entfernen).

#### <a name="change-the-subscription-you-use"></a>Ändern des verwendeten Abonnements

Wenn Sie das von Ihnen verwendete Abonnement ändern möchten, müssen Sie zuerst das Cmdlet **Remove-AzsRegistration** ausführen. Dann müssen Sie sicherstellen, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind, und anschließend **Set-AzsRegistration** mit allen geänderten Parametern einschließlich des \<Abrechnungsmodells\> ausführen:

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Ändern des Abrechnungsmodells oder des Angebots von Features

Wenn Sie das Abrechnungsmodell oder das Angebot von Features für Ihre Installation ändern möchten, können Sie die Registrierungsfunktion aufrufen, um die neuen Werte festzulegen. Sie müssen die aktuelle Registrierung nicht zuvor entfernen:

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Erneuern oder Ändern der Registrierung in nicht verbundenen Umgebungen

Unter den folgenden Umständen müssen Sie Ihre Registrierung aktualisieren oder erneuern:

- Nachdem Sie Ihr kapazitätsbasiertes Jahresabonnement verlängert haben.
- Wenn Sie Ihr Abrechnungsmodell ändern.
- Wenn Sie Änderungen für die kapazitätsbasierte Abrechnung vornehmen (Knoten hinzufügen/entfernen).

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Entfernen der Aktivierungsressource aus Azure Stack

Sie müssen zuerst die Aktivierungsressource aus Azure Stack und dann die Registrierungsressource in Azure entfernen.  

Zum Entfernen der Aktivierungsressource in Azure Stack führen Sie die folgenden PowerShell-Cmdlets in Ihrer Azure Stack-Umgebung aus:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Zum Entfernen der Registrierungsressource in Azure stellen Sie als Nächstes sicher, dass Sie einen mit Azure verbundenen Computer nutzen. Melden Sie sich beim richtigen Azure PowerShell-Kontext an, und führen Sie die entsprechenden PowerShell-Cmdlets aus, wie unten beschrieben.

Sie können das Registrierungstoken verwenden, mit dem die Ressource erstellt wurde:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Alternativ können Sie den Registrierungsnamen nutzen:

  ```Powershell
  $registrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Erneutes Registrieren in einem nicht verbundenen Szenario

Sie haben jetzt die Registrierung in einem nicht verbundenen Szenario vollständig aufgehoben und müssen die Schritte zum Registrieren einer Azure Stack-Umgebung in einem nicht verbundenen Szenario wiederholen.

### <a name="disable-or-enable-usage-reporting"></a>Deaktivieren oder Aktivieren von Nutzungsberichten

Für Azure Stack-Umgebungen, die ein kapazitätsbasiertes Abrechnungsmodell verwenden, deaktivieren Sie Nutzungsberichte mit dem **UsageReportingEnabled**-Parameter im Cmdlet **Set-AzsRegistration** oder im Cmdlet **Get-AzsRegistrationToken**. Azure Stack erstellt standardmäßig Berichte zu Nutzungsmetriken. Operatoren, die ein kapazitätsbasiertes Modell verwenden oder eine nicht verbundene Umgebung unterstützen, müssen Nutzungsberichte deaktivieren.

#### <a name="with-a-connected-azure-stack"></a>Mit einer verbundenen Azure Stack-Instanz

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>Mit einer nicht verbundenen Azure Stack-Instanz

1. Führen Sie die folgenden PowerShell-Cmdlets aus, um das Registrierungstoken zu ändern:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > Das Registrierungstoken wird in der Datei gespeichert, die für *$FilePathForRegistrationToken* angegeben ist. Sie können den Dateipfad oder den Dateinamen nach eigenem Ermessen ändern.

2. Speichern Sie dieses Registrierungstoken für die Verwendung auf einem mit Azure verbundenen Computer. Sie können die Datei oder den Text aus „$FilePathForRegistrationToken“ kopieren.

## <a name="move-a-registration-resource"></a>Verschieben einer Registrierungsressource
Das Verschieben einer Registrierungsressource zwischen Ressourcengruppen im gleichen Abonnement **wird** für alle Umgebungen unterstützt. Das Verschieben einer Registrierungsressource zwischen Abonnements wird jedoch nur für CSPs unterstützt, wenn beide Abonnements zur gleichen Partner-ID aufgelöst werden. Weitere Informationen zum Verschieben von Ressourcen in eine neue Ressourcengruppe finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="registration-reference"></a>Referenz zur Registrierung

### <a name="set-azsregistration"></a>Set-AzsRegistration

Sie können „Set-AzsRegistration“ zum Registrieren von Azure Stack bei Azure und zum Aktivieren oder Deaktivieren des Angebots von Elementen im Marketplace sowie Nutzungsberichten verwenden.

Voraussetzungen für die Ausführung des Cmdlets:
- Ein globales Azure-Abonnement beliebigen Typs.
- Außerdem müssen Sie mit einem Konto bei Azure PowerShell angemeldet sein, das ein Besitzer oder Mitwirkender dieses Abonnements ist.

```PowerShell
    Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
   ```

| Parameter | Typ | BESCHREIBUNG |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | Die Anmeldeinformationen, die für den [Zugriff auf den privilegierten Endpunkt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) verwendet werden. Der Benutzername hat das Format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Zeichenfolge | Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Weitere Informationen finden Sie im Artikel [Verwenden des privilegierten Endpunkts](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). |
| AzureContext | PSObject |  |
| ResourceGroupName | Zeichenfolge |  |
| ResourceGroupLocation | Zeichenfolge |  |
| BillingModel | Zeichenfolge | Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Gültige Werte für diesen Parameter: „Capacity“, „PayAsYouUse“ und „Development“. |
| MarketplaceSyndicationEnabled | True/False | Bestimmt, ob das Marketplace-Verwaltungsfeature im Portal verfügbar ist. Legen Sie den Wert auf „true“ fest, wenn bei der Registrierung eine Internetverbindung verfügbar ist. Legen Sie den Wert auf „false“ fest, wenn die Registrierung in einer Umgebung ohne Verbindung erfolgt. In Umgebungen ohne Verbindung können Marketplace-Elemente mithilfe des [Offlinesyndikationstools](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) heruntergeladen werden. |
| UsageReportingEnabled | True/False | Azure Stack erstellt standardmäßig Berichte zu Nutzungsmetriken. Operatoren, die ein kapazitätsbasiertes Modell verwenden oder eine nicht verbundene Umgebung unterstützen, müssen Nutzungsberichte deaktivieren. Gültige Werte für diesen Parameter: True, False. |
| AgreementNumber | Zeichenfolge |  |
| RegistrationName | Zeichenfolge | Legen Sie einen eindeutigen Namen für die Registrierung fest, wenn Sie das Registrierungsskript auf mehreren Instanzen von Azure Stack mit der gleichen Azure-Abonnement-ID ausführen. Der Parameter hat den Standardwert **AzureStackRegistration**. Wenn Sie den gleichen Namen jedoch in mehreren Instanzen von Azure Stack verwenden, tritt beim Ausführen des Skripts ein Fehler auf. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Mit „Get-AzsRegistrationToken“ wird aus den Eingabeparametern ein Registrierungstoken generiert.

```PowerShell  
    Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parameter | Typ | BESCHREIBUNG |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | Die Anmeldeinformationen, die für den [Zugriff auf den privilegierten Endpunkt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) verwendet werden. Der Benutzername hat das Format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Zeichenfolge |  Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Weitere Informationen finden Sie im Artikel [Verwenden des privilegierten Endpunkts](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). |
| AzureContext | PSObject |  |
| ResourceGroupName | Zeichenfolge |  |
| ResourceGroupLocation | Zeichenfolge |  |
| BillingModel | Zeichenfolge | Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Gültige Werte für diesen Parameter: „Capacity“, „PayAsYouUse“ und „Development“. |
| MarketplaceSyndicationEnabled | True/False |  |
| UsageReportingEnabled | True/False | Azure Stack erstellt standardmäßig Berichte zu Nutzungsmetriken. Operatoren, die ein kapazitätsbasiertes Modell verwenden oder eine nicht verbundene Umgebung unterstützen, müssen Nutzungsberichte deaktivieren. Gültige Werte für diesen Parameter: True, False. |
| AgreementNumber | Zeichenfolge |  |


## <a name="next-steps"></a>Nächste Schritte

[Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)

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
ms.date: 03/26/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: 1dc3d9a96b9b27927cc8cc66b5e80987fba4f8ea
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure
Wenn Sie [Azure Stack](azure-stack-poc.md) bei Azure registrieren, können Sie Marketplace-Elemente aus Azure herunterladen und das Senden von Berichten zu Geschäftsdaten an Microsoft einrichten. Nach der Registrierung von Azure Stack wird die Nutzung an Azure Commerce gemeldet und kann unter dem für die Registrierung verwendeten Abonnement angezeigt werden. 

> [!IMPORTANT]
> Die Registrierung ist obligatorisch, wenn Sie das nutzungsbasierte Abrechnungsmodell wählen. Andernfalls verstoßen Sie gegen die Lizenzbedingungen der Azure Stack-Bereitstellung, da die Nutzung nicht gemeldet wird.

## <a name="prerequisites"></a>Voraussetzungen
Vor der Registrierung von Azure Stack in Azure müssen Sie folgende Bedingungen erfüllen:

- Sie müssen über eine Abonnement-ID eines Azure-Abonnements verfügen. Die ID erhalten Sie, indem Sie sich bei Azure anmelden und auf **Weitere Dienste** > **Abonnements** und dann auf das Abonnement klicken, das Sie verwenden möchten. Unter **Zusammenfassung** sehen Sie nun die Abonnement-ID. 

  > [!NOTE]
  > Cloudabonnements für China, Deutschland und US Government werden derzeit nicht unterstützt. 

- Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Abonnements ist (Konten mit MSA/2FA werden unterstützt)
- Sie müssen den Azure Stack-Ressourcenanbieter registriert haben. (Weitere Informationen finden Sie weiter unten im Abschnitt „Registrieren des Azure Stack-Ressourcenanbieters“.)

Wenn Ihr Azure-Abonnement diese Anforderungen nicht erfüllt, können Sie [hier kostenlos ein neues Azure-Konto erstellen](https://azure.microsoft.com/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.

### <a name="bkmk_powershell"></a>Installieren von PowerShell für Azure Stack
Für die Registrierung bei Azure muss die neueste Version von PowerShell für Azure Stack verwendet werden.

Falls noch nicht geschehen, [installieren Sie PowerShell für Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Herunterladen der Azure Stack-Tools
Das GitHub-Repository mit den Azure Stack-Tools enthält PowerShell-Module, die Azure Stack-Funktionalität einschließlich Registrierungsfunktionalität unterstützen. Während des Registrierungsprozesses müssen Sie das PowerShell-Modul „RegisterWithAzure.psm1“ importieren und verwenden, das sich im Repository mit den Azure Stack Tools befindet, um Ihre Azure Stack-Instanz bei Azure zu registrieren. 

Um sicherzustellen, dass Sie die neueste Version verwenden, sollten Sie vor der Registrierung bei Azure alle vorhandenen Versionen der Azure Stack-Tools löschen und [die neueste Version von GitHub herunterladen](azure-stack-powershell-download.md).

## <a name="register-azure-stack-in-connected-environments"></a>Registrieren von Azure Stack in mit Azure verbundenen Umgebungen
In mit Azure verbundenen Umgebungen kann auf das Internet und Azure zugegriffen werden. Für diese Umgebungen müssen Sie den Azure Stack-Ressourcenanbieter bei Azure registrieren und dann Ihr Abrechnungsmodell konfigurieren.

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. 

### <a name="register-the-azure-stack-resource-provider"></a>Registrieren des Azure Stack-Ressourcenanbieters
Um den Azure Stack-Ressourcenanbieter bei Azure zu registrieren, starten Sie PowerShell ISE als Administrator, und verwenden Sie die folgenden PowerShell-Befehle. Diese Befehle fordern Sie zu Folgendem auf:
- Anmelden als Besitzer des zu verwendenden Azure-Abonnements und Festlegen des Parameters **EnvironmentName** auf **AzureCloud**.
- Registrieren des Azure-Ressourcenanbieters **Microsoft.AzureStack**.

1. Fügen Sie das Azure-Konto hinzu, das zum Registrieren von Azure Stack verwendet wird. Führen Sie zum Hinzufügen des Kontos das Cmdlet **Add-AzureRmAccount** aus. Sie werden aufgefordert, Ihre Anmeldeinformationen für das Konto des globalen Azure-Administrators einzugeben. Je nach Konfiguration Ihres Kontos müssen Sie möglicherweise die zweistufige Authentifizierung verwenden.

   ```Powershell
      Add-AzureRmAccount -EnvironmentName AzureCloud
   ```

2. Wenn Sie über mehrere Abonnements verfügen, führen Sie den folgenden Befehl aus, um das zu verwendende Abonnement auszuwählen:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Führen Sie den folgenden Befehl zum Registrieren des Azure Stack-Ressourcenanbieters in Ihrem Azure-Abonnement aus:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells
Befolgen Sie diese Schritte zum Registrieren von Azure Stack bei Azure unter Verwendung des nutzungsbasierten Abrechnungsmodells.

Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1** mit PowerShell: 

Führen Sie diesen PowerShell-Befehl aus:

```powershell
Import-Module .\RegisterWithAzure.psm1
```
Führen Sie anschließend in der gleichen PowerShell-Sitzung das Cmdlet **Set-AzsRegistration** aus. Wenn Sie zum Eingeben von Anmeldeinformationen aufgefordert werden, geben Sie den Besitzer des Azure-Abonnements an.  

Führen Sie diesen PowerShell-Befehl aus:

```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials>  -Message "Enter the credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -BillingModel PayAsYouUse
```

|Parameter|Beschreibung|
|-----|-----|
|CloudAdminCredential|PowerShell-Objekt mit Anmeldeinformationen (Benutzername und Kennwort) für den Zugriff auf den privilegierten Endpunkt.|
|PrivilegedEndpoint|Eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen Funktionen wie die Protokollsammlung und andere Aufgaben nach der Bereitstellung zur Verfügung stellt. Weitere Informationen finden Sie im Artikel [Verwenden des privilegierten Endpunkts](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint#access-the-privileged-endpoint).|
|BillingModel|Das Abrechnungsmodell, das von Ihrem Abonnement verwendet wird. Zulässige Werte für diesen Parameter sind „Capacity“, „PayAsYouUse“ und „Development“.|

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registrieren von Azure Stack bei Azure unter Verwendung des kapazitätsbasierten Abrechnungsmodells
Befolgen Sie die gleichen Anweisungen wie bei der Registrierung mit dem nutzungsbasierten Abrechnungsmodell. Fügen Sie jedoch die Vertragsnummer hinzu, unter der Kapazität erworben wurde, und ändern Sie den Parameter `BillingModel` in **Capacity**. Alle anderen Parameter bleiben unverändert.

Führen Sie diesen PowerShell-Befehl aus:
```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials>  -Message "Enter the credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registrieren von Azure Stack in nicht mit Azure verbundenen Umgebungen 
*Die Informationen in diesem Abschnitt gelten ab der Azure Stack-Updateversion 1712 (180106.1) und werden von früheren Versionen nicht unterstützt.*

Wenn Sie Azure Stack in einer nicht mit Azure verbundenen Umgebung (ohne Internetverbindung) registrieren, müssen Sie ein Registrierungstoken aus der Azure Stack-Umgebung abrufen und dieses Token dann auf einem Computer verwenden, der eine Verbindung mit Azure herstellen kann und auf dem [PowerShell für Azure Stack installiert ist](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Abrufen eines Registrierungstokens aus der Azure Stack-Umgebung
  1. Führen Sie die folgenden PowerShell-Befehle zum Abrufen des Registrierungstokens aus:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > Das Registrierungstoken wird in der Datei gespeichert, die für *$env:SystemDrive\RegistrationToken.txt* angegeben ist.

  2. Speichern Sie dieses Registrierungstoken für die Verwendung auf einem mit Azure verbundenen Computer.


### <a name="connect-to-azure-and-register"></a>Verbinden mit Azure und Registrieren
Starten Sie PowerShell ISE als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim [Herunterladen der Azure Stack-Tools](#bkmk_tools) erstellt wurde. Importieren Sie das Modul **RegisterWithAzure.psm1**: 

Führen Sie diesen PowerShell-Befehl aus:
```powershell
Import-Module .\RegisterWithAzure.psm1
```
Geben Sie anschließend in derselben PowerShell-Sitzung das Registrierungstoken für die Registrierung bei Azure an:

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
> Speichern Sie den Namen der Registrierungsressource oder das Registrierungstoken zur späteren Bezugnahme.

## <a name="verify-azure-stack-registration"></a>Überprüfen der Azure Stack-Registrierung
Gehen Sie folgendermaßen vor, um sicherzustellen, dass Azure Stack erfolgreich in Azure registriert wurde.
1. Melden Sie sich beim Azure Stack-[Administratorportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal) an: https&#58;//adminportal.*&lt;region>.&lt;fqdn>*.
2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen).

Wenn Sie eine Liste der in Azure verfügbaren Elemente sehen (z.B. WordPress), war die Aktivierung erfolgreich.

> [!NOTE]
> Nachdem die Registrierung abgeschlossen ist, wird die aktive Warnung für die Nichtregistrierung nicht mehr angezeigt.

## <a name="renew-or-change-registration"></a>Erneuern oder Ändern der Registrierung
Unter den folgenden Umständen müssen Sie Ihre Registrierung aktualisieren oder erneuern:
- Nachdem Sie Ihr kapazitätsbasiertes Jahresabonnement verlängert haben.
- Wenn Sie Ihr Abrechnungsmodell ändern.
- Wenn Sie Änderungen für die kapazitätsbasierte Abrechnung vornehmen (Knoten hinzufügen/entfernen).

### <a name="change-the-subscription-you-use"></a>Ändern des verwendeten Abonnements
Wenn Sie das von Ihnen verwendete Abonnement ändern möchten, müssen Sie zuerst das Cmdlet **Remove-AzsRegistration** ausführen. Dann müssen Sie sicherstellen, dass Sie im richtigen Azure PowerShell-Kontext angemeldet sind, und anschließend **Set-AzsRegistration** mit allen geänderten Parametern ausführen:

```powershell
Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
Set-AzureRmContext -SubscriptionId $NewSubscriptionId
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

### <a name="change-the-billing-model-or-syndication-features"></a>Ändern des Abrechnungsmodells oder der Syndikationsfeatures
Wenn Sie das Abrechnungsmodell oder die Syndikationsfeatures für Ihre Installation ändern möchten, können Sie die Registrierungsfunktion aufrufen, um die neuen Werte festzulegen. Sie müssen die aktuelle Registrierung nicht zuvor entfernen: 

```powershell
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```
## <a name="next-steps"></a>Nächste Schritte

[Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)
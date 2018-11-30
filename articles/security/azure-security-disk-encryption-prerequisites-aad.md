---
title: Voraussetzungen für Azure Disk Encryption mit Azure AD-App (Vorgängerversion) | Microsoft-Dokumentation
description: Dieser Artikel enthält die Voraussetzungen für die Verwendung von Microsoft Azure Disk Encryption für IaaS-VMs.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 10/12/2018
ms.openlocfilehash: d81925589eefa0ea5851180c83db5bc3540aabda
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262685"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Voraussetzungen für Azure Disk Encryption (Vorgängerversion)

**Bei der neuen Version von Azure Disk Encryption muss kein Azure AD-Anwendungsparameter mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Sie müssen beim neuen Release während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen mit der neuen Version und ohne die Azure AD-Anwendungsparameter verschlüsselt werden. Eine Anleitung zum Aktivieren der VM-Datenträgerverschlüsselung mit der neuen Version finden Sie unter [Azure Disk Encryption – Voraussetzungen](azure-security-disk-encryption-prerequisites.md). Virtuelle Computer, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der AAD-Syntax gepflegt werden.**

 Dieser Artikel „Azure Disk Encryption – Voraussetzungen“ thematisiert, was Sie beachten müssen, bevor Sie Azure Disk Encryption verwenden können. Azure Disk Encryption ist in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) integriert und verwendet eine Azure AD-Anwendung zur Authentifizierung, um Verschlüsselungsschlüssel im Schlüsseltresor zu verwalten. Sie können auch [Azure PowerShell](/powershell/azure/overview) oder die [Azure CLI](/cli/azure/) verwenden, um Key Vault und die Azure AD-Anwendung einzurichten oder zu konfigurieren.

Bevor Sie Azure Disk Encryption auf Azure-IaaS-VMs für die unterstützten Szenarien aktivieren, die im Artikel [Azure Disk Encryption – Übersicht](azure-security-disk-encryption-overview.md) erörtert werden, beachten Sie die folgenden Voraussetzungen. 

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung, was zusätzliche Lizenz- oder Abonnementkosten nach sich ziehen kann. Sie müssen über ein gültiges aktives Azure-Abonnement verfügen, um in den unterstützten Regionen Ressourcen in Azure zu erstellen.


## <a name="bkmk_OSs"></a> Unterstützte Betriebssysteme
Azure Disk Encryption wird auf folgenden Betriebssystemen unterstützt:

- Windows Server-Versionen: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016.
    - Für Windows Server 2008 R2 müssen Sie .NET Framework 4.5 installieren, bevor Sie die Verschlüsselung in Azure aktivieren. Sie können die Installation über Windows Update durchführen mit dem optionalen Update Microsoft.NET Framework 4.5.2 für Windows Server 2008 R2 x64-basierte Systeme ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Windows-Clientversionen: Windows 8-Client und Windows 10-Client.
- Azure Disk Encryption wird nur für bestimmte auf dem Azure-Katalog basierenden Linux-Serverdistributionen und -Versionen unterstützt. Die Liste der derzeit unterstützten Versionen finden Sie unter [Häufig gestellte Fragen zu Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk Encryption setzt voraus, dass Ihr Schlüsseltresor und die VMs zur selben Azure-Region und zum selben Azure-Abonnement gehören. Bei einer Konfiguration der Ressourcen in unterschiedlichen Regionen kann Azure Disk Encryption nicht aktiviert werden.

## <a name="bkmk_LinuxPrereq"></a> Zusätzliche Voraussetzungen für Linux-IaaS-VMs 

- Azure Disk Encryption für Linux setzt 7 GB RAM auf dem virtuellen Computer voraus, um die Verschlüsselung von Betriebssystemdatenträgern für [unterstützte Images](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) zu aktivieren. Sobald die Verschlüsselung des Betriebssystemdatenträgers abgeschlossen ist, kann die VM so konfiguriert werden, dass sie mit weniger Speicherplatz läuft.
- Vor dem Aktivieren der Verschlüsselung müssen die zu verschlüsselnden Datenträger ordnungsgemäß in „/etc/fstab“ aufgelistet werden. Verwenden Sie für diesen Eintrag einen persistenten Blockgerätenamen, da Gerätenamen im Format „/dev/sdX“ beim Neustart, insbesondere nach Anwendung der Verschlüsselung, nicht zuverlässig mit demselben Datenträger verknüpft werden können. Weitere Informationen zu diesem Verhalten finden Sie unter [Behandeln von Problemen mit geänderten Gerätenamen von Linux-VMs](../virtual-machines/linux/troubleshoot-device-names-problems.md).
- Stellen Sie sicher, dass die Einstellungen für „/etc/fstab“ ordnungsgemäß für die Einbindung konfiguriert sind. Um diese Einstellungen zu konfigurieren, führen Sie den Befehl „mount -a“ aus, oder starten Sie die VM neu, und lösen Sie das Einbinden so erneut aus. Wenn der Vorgang abgeschlossen wurde, überprüfen Sie die Ausgabe des Befehls „lsblk“, um zu sehen, ob das gewünschte Laufwerk noch eingebunden ist. 
    - Wenn die Datei „/etc/fstab“ das Laufwerk vor der Aktivierung der Verschlüsselung nicht richtig einbindet, kann Azure Disk Encryption es nicht richtig einbinden.
    - Azure Disk Encryption verschiebt die Einbindungsinformationen aus „/etc/fstab“ in die eigene Konfigurationsdatei als Teil des Verschlüsselungsprozesses. Der Eintrag fehlt in „/etc/fstab“, nachdem die Verschlüsselung des Datenlaufwerks abgeschlossen wurde.
    -  Nach dem Neustart dauert es einige Zeit, bis Azure Disk Encryption die neu verschlüsselten Datenträger eingebunden hat. Sie sind nach einem Neustart nicht sofort verfügbar. Der Prozess benötigt Zeit, um die verschlüsselten Laufwerke zu starten, zu entsperren und dann einzubinden, bevor sie für andere Prozesse verfügbar sind. Dies kann je nach Systemeigenschaften mehr als eine Minute nach dem Neustart dauern.

Ein Beispiel für Befehle, die verwendet werden können, um Datenträger einzubinden und die notwendigen „/etc/fstab“-Einträge zu erstellen, finden Sie in den [Zeilen 197–205 dieser Skriptdatei](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L197-L205). 


## <a name="bkmk_GPO"></a> Netzwerk und Gruppenrichtlinie

**IaaS-VMs müssen die folgenden Anforderungen an die Netzwerkendpunktkonfiguration erfüllen, damit das Azure Disk Encryption-Feature mit der älteren AAD-Parametersyntax aktiviert werden kann:** 
  - Um ein Token für die Verbindungsherstellung mit Ihrem Schlüsseltresor zu erhalten, muss der virtuelle IaaS-Computer eine Verbindung mit dem Azure Active Directory-Endpunkt \[login.microsoftonline.com\] herstellen können.
  - Um die Verschlüsselungsschlüssel in Ihren Schlüsseltresor schreiben zu können, muss der virtuelle IaaS-Computer eine Verbindung mit dem Schlüsseltresorendpunkt herstellen können.
  - Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Speicherendpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit einem Azure Storage-Konto, das die VHD-Dateien hostet.
  -  Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen. Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](../key-vault/key-vault-access-behind-firewall.md).
  - Wenn TLS 1.0 unter Windows explizit deaktiviert wurde und die .NET-Version nicht auf 4.6 oder höher aktualisiert wurde, wird mit der folgenden Registrierungsänderung ADE auf die neuere TLS-Version aktualisiert:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Gruppenrichtlinie:**
 - Die Azure Disk Encryption-Lösung verwendet für virtuelle Windows-IaaS-Computer die externe BitLocker-Schlüsselschutzvorrichtung. Für VMs, die der Domäne beigetreten sind, sollten Sie keine Gruppenrichtlinien nutzen, mit denen TPM-Schutzvorrichtungen durchgesetzt werden. Informationen zur Gruppenrichtlinie „BitLocker ohne kompatibles TPM zulassen“ finden Sie unter [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup) (Referenz zur BitLocker-Gruppenrichtlinie).

-  Die BitLocker-Richtlinie für VMs mit Domänenbeitritt und benutzerdefinierten Gruppenrichtlinien muss die folgende Einstellung enthalten: [Speichern von BitLocker-Wiederherstellungsinformationen durch Benutzer konfigurieren -> 256-Bit-Wiederherstellungsschlüssel zulassen](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk Encryption schlägt fehl, wenn benutzerdefinierte Einstellungen für die Gruppenrichtlinie nicht mit BitLocker kompatibel sind. Auf Computern ohne korrekte Richtlinieneinstellung müssen Sie die neue Richtlinie anwenden und die Aktualisierung der neuen Richtlinie erzwingen (gpupdate.exe /force). Danach ist möglicherweise ein Neustart erforderlich.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) bietet eine Reihe von Cmdlets, die das [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-Modell für die Verwaltung von Azure-Ressourcen verwenden. Azure PowerShell kann mit [Azure Cloud Shell](../cloud-shell/overview.md) im Browser verwendet oder auf dem lokalen Computer installiert (siehe Anleitung unten) und in einer beliebigen PowerShell-Sitzung verwendet werden. Falls Sie die lokale Installation bereits durchgeführt haben, verwenden Sie die neueste Version des Azure PowerShell SDKs, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter.

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Installieren Sie Azure PowerShell für die Verwendung auf Ihrem lokalen Computer (optional):  
1. Folgen Sie den Anweisungen unter dem Link für Ihr Betriebssystem, und fahren Sie dann mit den weiteren Schritten fort.      
    - [Installieren und Konfigurieren von Azure PowerShell unter Windows](/powershell/azure/install-azurerm-ps) 
        - Installieren Sie PowerShellGet und Azure PowerShell, und laden Sie das AzureRM-Modul herunter. 
    - [Installieren und Konfigurieren von Azure PowerShell unter macOS und Linux](/powershell/azure/install-azurermps-maclinux)
        -  Installieren Sie PowerShell Core und Azure PowerShell für .NET Core, und laden Sie das Az-Modul herunter.
2. Installieren sie das [Azure Active Directory PowerShell-Modul](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module). 

     ```powershell
     Install-Module AzureAD
     ```

3. Überprüfen Sie, welche Modulversionen installiert sind.
      ```powershell
      Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. Melden Sie sich mithilfe des Cmdlets [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) bei Azure an.
     
     ```powershell
     Connect-AzureRmAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzureRmSubscription and 
     specify it with Set-AzureRmContext.  #>
     Get-AzureRmSubscription
     Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Stellen Sie über [Connect-AzureAD](/powershell/module/azuread/connect-azuread) eine Verbindung mit Azure AD her.
     
     ```powershell
     Connect-AzureAD
     ```

6. Weitere Informationen finden Sie bei Bedarf unter [Erste Schritte mit Azure PowerShell](/powershell/azure/get-started-azureps) und [Azure AD](/powershell/module/azuread).

## <a name="bkmk_CLI"></a> Azure CLI

Die [Azure CLI 2.0](/cli/azure) ist ein Befehlszeilentool zum Verwalten von Azure-Ressourcen. Sie wurde entwickelt, um Daten flexible abzufragen, Vorgänge mit langer Ausführungsdauer als nicht blockierende Prozesse zu unterstützen und das Erstellen von Skripts zu vereinfachen. Azure PowerShell kann mit [Azure Cloud Shell](../cloud-shell/overview.md) im Browser verwendet oder auf dem lokalen Computer installiert und in einer beliebigen PowerShell-Sitzung verwendet werden.

1. [Installieren Sie die Azure CLI](/cli/azure/install-azure-cli) für die Verwendung auf Ihrem lokalen Computer (optional):

2. Überprüfen Sie, welche Version installiert ist.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Melden Sie sich mit [az login](/cli/azure/authenticate-azure-cli) in Azure an.
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Weitere Informationen finden Sie bei Bedarf unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-azure-cli). 


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Vorbereitung für Key Vault und die Azure AD-App

Wenn Sie bereits mit den Key Vault- und Azure AD-Voraussetzungen für Azure Disk Encryption vertraut sind, können Sie das [PowerShell-Skript zur Überprüfung der Azure Disk Encryption-Voraussetzungen](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ) verwenden. Weitere Informationen zur Verwendung des Skripts zur Überprüfung der Voraussetzungen finden Sie in der [Schnellstartanleitung zum Verschlüsseln eines virtuellen Computers](quick-encrypt-vm-powershell.md) sowie im [Anhang zu Azure Disk Encryption](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Erstellen eines Schlüsseltresors 
2. Richten Sie eine Azure AD-Anwendung und einen Dienstprinzipal ein.
3. Legen Sie die Zugriffsrichtlinie für den Schlüsseltresor für die Azure AD-App fest.
4. Legen Sie die erweiterten Zugriffsrichtlinien für den Schlüsseltresor fest.
 
## <a name="bkmk_KeyVault"></a> Erstellen einer Key Vault-Instanz 
Azure Disk Encryption ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement steuern und verwalten können. Sie können für Azure Disk Encryption einen neuen Schlüsseltresor erstellen oder einen bereits vorhandenen verwenden. Weitere Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit Azure Key Vault](../key-vault/key-vault-get-started.md) und [Schützen eines Schlüsseltresors](../key-vault/key-vault-secure-your-key-vault.md). Sie können eine Resource Manager-Vorlage, Azure PowerShell oder die Azure CLI verwenden, um einen Schlüsseltresor zu erstellen. 


>[!WARNING]
>Um sicherzustellen, dass die Verschlüsselungsgeheimnisse die Regionsgrenzen nicht verlassen, müssen den Schlüsseltresor und die VMs für Azure Disk Encryption sich in derselben Region angeordnet sein. Erstellen und verwenden Sie einen Schlüsseltresor, der sich in derselben Region wie die zu verschlüsselnde VM befindet. 


### <a name="bkmk_KVPSH"></a> Erstellen eines Schlüsseltresors mit PowerShell

Sie können mit Azure PowerShell mit dem Cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) einen Schlüsseltresor erstellen. Weitere Cmdlets für Key Vault finden Sie unter [AzureRM.KeyVault](/powershell/module/azurerm.keyvault/). 

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH) her. 
2. Erstellen Sie ggf. mit [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) eine neue Ressourcengruppe.  Verwenden Sie [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation), um Standorte von Rechenzentren aufzulisten. 
     
     ```azurepowershell-interactive
     # Get-AzureRmLocation 
     New-AzureRmResourceGroup –Name 'MySecureRG' –Location 'East US'
     ```

3. Erstellen Sie mit [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) einen neuen Schlüsseltresor.
    
      ```azurepowershell-interactive
     New-AzureRmKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -Location 'East US'
     ```

4. Notieren Sie **Tresorname**, **Ressourcengruppenname**, **Ressourcen-ID**, **Vault-URI** und **Objekt-ID**, die zur späteren Verwendung beim Verschlüsseln der Datenträger zurückgegeben werden. 


### <a name="bkmk_KVCLI"></a> Erstellen eines Schlüsseltresors mit der Azure CLI
Sie können Ihren Schlüsseltresor mit der Azure CLI mit den [az keyvault](/cli/azure/keyvault#commands)-Befehlen verwalten. Verwenden Sie [az keyvault create](/cli/azure/keyvault#az-keyvault-create), um einen Schlüsseltresor zu erstellen.

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI) her.
2. Erstellen Sie ggf. mit [az group create](/cli/azure/group#az-group-create) eine neue Ressourcengruppe. Verwenden Sie [az account list-locations](/cli/azure/account#az-account-list), um Orte aufzulisten. 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MySecureRG" -l "East US"
     ```

3. Erstellen Sie mit [az keyvault create](/cli/azure/keyvault#az-keyvault-create) einen neuen Schlüsseltresor.
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MySecureRG" --location "East US"
     ```

4. Notieren Sie **Tresorname** (Name), **Ressourcengruppenname**, **Ressourcen-ID** (ID), **Vault-URI** und **Objekt-ID**, die zur späteren Verwendung zurückgegeben werden. 

### <a name="bkmk_KVRM"></a> Erstellen eines Schlüsseltresors mit einer Resource Manager-Vorlage

Sie können mit der [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) einen Schlüsseltresor erstellen.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **Bereitstellung in Azure**.
2. Wählen Sie Abonnement, Ressourcengruppe, Ressourcengruppenstandort, Schlüsseltresorname, Objekt-ID, rechtliche Bedingungen und Vereinbarung aus, und klicken Sie auf **Kaufen**. 


## <a name="bkmk_ADapp"></a> Einrichten einer Azure AD-App und eines Dienstprinzipals 
Wenn Sie die Verschlüsselung auf einem ausgeführten virtuellen Computer in Azure aktivieren müssen, werden die Verschlüsselungsschlüssel von Azure Disk Encryption generiert und in Ihren Schlüsseltresor geschrieben. Das Verwalten von Verschlüsselungsschlüsseln im Schlüsseltresor erfordert eine Azure AD-Authentifizierung. Erstellen Sie dafür eine Azure AD-Anwendung. Zu Authentifizierungszwecken können Sie entweder die auf einem geheimen Clientschlüssel basierende Authentifizierung oder die [auf einem Clientzertifikat basierende Azure AD-Authentifizierung](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) verwenden.


### <a name="bkmk_ADappPSH"></a> Einrichten einer Azure AD-App und eines Dienstprinzipals mit Azure PowerShell 
Für die folgenden Befehle benötigen Sie das [Azure AD PowerShell-Modul](/powershell/azure/active-directory/install-adv2). 

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH) her.
2. Verwenden Sie das PowerShell-Cmdlet [New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication), um eine Azure AD-Anwendung zu erstellen. Die Werte für „MyApplicationHomePage“ und „MyApplicationUri“ können beliebig sein.

     ```azurepowershell-interactive
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzureRmADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. „$azureAdApplication.ApplicationId“ ist die Azure AD-Client-ID, und „$aadClientSecret“ ist der geheime Clientschlüssel. Sie benötigen diese Angaben später zum Aktivieren von Azure Disk Encryption. Sorgen Sie für eine sichere Aufbewahrung des Azure AD-Clientgeheimnisses. Wenn Sie `$azureAdApplication.ApplicationId` ausführen, wird Ihnen die „ApplicationID“ angezeigt.


### <a name="bkmk_ADappCLI"></a> Einrichten einer Azure AD-App und eines Dienstprinzipals mit der Azure CLI

Sie können Ihre Dienstprinzipale mit der Azure CLI mit den [az ad sp](/cli/azure/ad/sp)-Befehlen verwalten. Weitere Informationen finden Sie unter [Erstellen eines Azure-Dienstprinzipals](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI) her.
2. Erstellen Sie einen neuen Dienstprinzipal.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  Die zurückgegeben App-ID ist die Azure AD-Client-ID, die in anderen Befehlen verwendet wird. Sie ist außerdem der Dienstprinzipalname für „az keyvault set-policy“. Das Kennwort ist der geheime Clientschlüssel, mit dem Sie später Azure Disk Encryption aktivieren werden. Sorgen Sie für eine sichere Aufbewahrung des Azure AD-Clientgeheimnisses.
 
### <a name="bkmk_ADappRM"></a> Einrichten einer Azure AD-App und eines Dienstprinzipals über das Azure-Portal
Im Artikel [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md) finden Sie eine Anleitung, wie Sie eine Azure AD-Anwendung erstellen. Jeder unten aufgeführte Schritt führt Sie direkt zum entsprechenden Artikelabschnitt. 

1. [Überprüfen Sie die erforderlichen Berechtigungen.](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Erstellen Sie eine Azure Active Directory-Anwendung.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Sie können einen beliebigen Namen und eine beliebige Anmelde-URL verwenden, wenn Sie die Anwendung erstellen.
3. [Rufen Sie die Anwendungs-ID und den Authentifizierungsschlüssel ab.](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key) 
     - Der Authentifizierungsschlüssel ist der geheime Clientschlüssel und wird als „AadClientSecret“ für „Set-AzureRmVMDiskEncryptionExtension“ verwendet. 
        - Der Authentifizierungsschlüssel wird von der Anwendung als Anmeldeinformation für die Anmeldung in Azure AD verwendet. Im Azure-Portal heißt dieses Geheimnis „Schlüssel“, hat jedoch keinen Bezug zu Schlüsseltresoren. Schützen Sie dieses Geheimnis entsprechend. 
     - Die Anwendungs-ID wird später als „AadClientId“ für „Set-AzureRmVMDiskEncryptionExtension“ und als „ServicePrincipalName“ für „Set-AzureRmKeyVaultAccessPolicy“ verwendet. 

## <a name="bkmk_KVAP"></a> Festlegen der Zugriffsrichtlinie für den Schlüsseltresor für die Azure AD-App
Um Verschlüsselungsgeheimnisse in einen bestimmten Schlüsseltresor zu schreiben, benötigt Azure Disk Encryption die Client-ID und den geheimen Clientschlüssel der Azure Active Directory-Anwendung, berechtigt ist, Geheimnisse in den Schlüsseltresor zu schreiben. 

> [!NOTE]
> Azure Disk Encryption erfordert das Konfigurieren der folgenden Zugriffsrichtlinien für Ihre Azure AD-Clientanwendung: Berechtigungen _WrapKey_ und _Set_.

### <a name="bkmk_KVAPPSH"></a> Festlegen der Zugriffsrichtlinie für den Schlüsseltresor für die Azure AD-App mit Azure PowerShell
Ihre Azure AD-Anwendung benötigt Rechte zum Zugreifen auf die Schlüssel oder geheimen Schlüssel im Tresor. Verwenden Sie das Cmdlet [Set-AzureKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), um der Anwendung Berechtigungen zu erteilen. Verwenden Sie die Client-ID (die bei der Anwendungsregistrierung generiert wurde) als _–ServicePrincipalName_-Parameterwert. Weitere Informationen finden Sie im Blogbeitrag [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) (Azure Key Vault – Schritt für Schritt). 

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH) her.
2. Legen Sie die Zugriffsrichtlinie für den Schlüsseltresor für die AD-Anwendung mit PowerShell fest.

     ```azurepowershell-interactive
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $rgname = 'MySecureRG'
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
     ```

### <a name="bkmk_KVAPCLI"></a> Festlegen der Zugriffsrichtlinie für den Schlüsseltresor für die Azure AD-App mit der Azure CLI
Verwenden Sie [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy), um die Zugriffsrichtlinie festzulegen. Weitere Informationen finden Sie unter [Verwalten von Key Vault mit der CLI 2.0](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Stellen Sie ggf. eine [Verbindung mit Ihrem Azure-Abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI) her.
2. Erteilen Sie dem über die Azure CLI erstellten Dienstprinzipal Zugriff, damit er Geheimnisse abrufen und Schlüssel mit dem folgenden Befehl packen kann:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a> Festlegen der Zugriffsrichtlinie für den Schlüsseltresor für die Azure AD-App im Portal

1. Öffnen Sie die Ressourcengruppe mit Ihrem Schlüsseltresor.
2. Wählen Sie Ihren Schlüsseltresor aus, öffnen Sie **Zugriffsrichtlinien**, und klicken Sie dann auf **Neue hinzufügen**.
3. Suchen Sie unter **Prinzipal auswählen** nach der Azure AD-Anwendung, die Sie erstellt haben, und wählen Sie sie aus. 
4. Aktivieren Sie für **Schlüsselberechtigungen** unter **Kryptografische Vorgänge** die Option **Schlüssel packen**.
5. Aktivieren Sie für **Berechtigungen für Geheimnis** unter **Verwaltungsvorgänge für Geheimnisse** die Option **Festlegen**.
6. Klicken Sie auf **OK**, um die Richtlinie zu speichern. 

![Azure Key Vault: „Kryptografische Vorgänge“ und „Schlüssel packen“](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![„Berechtigungen für Geheimnis“ in Azure Key Vault auf „Festlegen“ ](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a> Festlegen von erweiterten Zugriffsrichtlinien für Schlüsseltresore
Die Azure-Plattform benötigt Zugriff auf die Verschlüsselungsschlüssel oder geheimen Schlüssel in Ihrem Schlüsseltresor, um sie für den virtuellen Computer zur Verfügung zu stellen, damit die Volumes gestartet und entschlüsselt werden können. Aktivieren Sie die Datenträgerverschlüsselung im Schlüsseltresor, damit Bereitstellungen nicht fehlschlagen.  

### <a name="bkmk_KVperPSH"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore mit Azure PowerShell
 Verwenden Sie das PowerShell-Cmdlet für Schlüsseltresore [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), um die Datenträgerverschlüsselung für den Schlüsseltresor zu aktivieren.

  - **Aktivieren von Key Vault für die Datenträgerverschlüsselung:** „EnabledForDiskEncryption“ ist für die Verwendung von Azure Disk Encryption erforderlich.
      
     ```azurepowershell-interactive 
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDiskEncryption
     ```

  - **Aktivieren von Key Vault für die Bereitstellung (falls erforderlich):** Der Ressourcenanbieter „Microsoft.Compute“ wird aktiviert, um Geheimnisse aus diesem Schlüsseltresor abzurufen, wenn dieser Schlüsseltresor bei der Ressourcenerstellung (z. B. beim Erstellen einer VM) referenziert wird.

     ```azurepowershell-interactive
      Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDeployment
     ```

  - **Aktivieren von Key Vault für die Vorlagenbereitstellung (falls erforderlich):** Azure Resource Manager kann aus diesem Schlüsseltresor Geheimnisse abrufen, wenn dieser Schlüsseltresor in einer Vorlagenbereitstellung referenziert wird.

     ```azurepowershell-interactive             
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForTemplateDeployment`
     ```

### <a name="bkmk_KVperCLI"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore mit der Azure CLI
Verwenden Sie [az keyvault update](/cli/azure/keyvault#az-keyvault-update), um die Datenträgerverschlüsselung für den Schlüsseltresor zu aktivieren. 

 - **Aktivieren von Key Vault für die Datenträgerverschlüsselung:** „Enabled-for-disk-encryption“ ist erforderlich. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-disk-encryption "true"
     ```  

 - **Aktivieren von Key Vault für die Bereitstellung (falls erforderlich):** VMs können Zertifikate aus dem Schlüsseltresor abrufen, die als Geheimnis gespeichert sind.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-deployment "true"
     ``` 

 - **Aktivieren von Key Vault für die Vorlagenbereitstellung (falls erforderlich):** Der Resource Manager kann Geheimnisse aus dem Tresor abrufen.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Festlegen der erweiterten Zugriffsrichtlinien für Schlüsseltresore im Azure-Portal

1. Wählen Sie Ihren Schlüsseltresor aus, navigieren Sie zu **Zugriffsrichtlinien**, und **Klicken Sie, um erweiterte Zugriffsrichtlinien anzuzeigen**.
2. Wählen Sie das Feld **Zugriff auf Azure Disk Encryption für Volumeverschlüsselung aktivieren** aus.
3. Wählen Sie ggf. **Zugriff auf Azure Virtual Machines für Bereitstellung aktivieren** und/oder **Zugriff auf Azure Resource Manager für Vorlagenbereitstellung aktivieren** aus. 
4. Klicken Sie auf **Speichern**.

![Erweiterte Zugriffsrichtlinien für Schlüsseltresore in Azure](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Einrichten eines Schlüssels zur Schlüsselverschlüsselung (optional)
Wenn Sie Verschlüsselungsschlüssel mit einem Schlüssel für die Schlüsselverschlüsselung zusätzlich schützen möchten, fügen Sie Ihrem Schlüsseltresor einen Schlüsselverschlüsselungsschlüssel hinzu. Verwenden Sie das Cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), um im Schlüsseltresor einen Schlüsselverschlüsselungsschlüssel zu erstellen. Sie können den KEK auch aus Ihrem lokalen Hardwaresicherheitsmodul (HSM) für die Schlüsselverwaltung importieren. Weitere Informationen finden Sie in der [Key Vault](../key-vault/key-vault-hsm-protected-keys.md)-Dokumentation. Wenn ein Schlüsselverschlüsselungsschlüssel angegeben wird, verwendet Azure Disk Encryption diesen, um Verschlüsselungsgeheimnisse vor dem Schreiben in Key Vault zu umschließen. 

* Ihre URLs für das Geheimnis des Schlüsseltresors und den Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) müssen mit einer Versionsangabe versehen sein. Azure erzwingt diese Einschränkung der Versionsverwaltung. Gültige URLs für Geheimnisse und KEKs finden Sie in den folgenden Beispielen:

  * Beispiel für eine gültige Geheimnis-URL: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Beispiel für eine gültige KEK-URL: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Die Angabe von Portnummern als Teil von URLs für Schlüsseltresorgeheimnisse und KEK-URLs wird von Azure Disk Encryption nicht unterstützt. Beispiele für nicht unterstützte und unterstützte Schlüsseltresor-URLs finden Sie hier:

  * Unzulässige Schlüsseltresor-URL: *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Zulässige Schlüsseltresor-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Festlegen eines Schlüssels für die Schlüsselverschlüsselung mit Azure PowerShell 
Bevor Sie das PowerShell-Skript verwenden, sollten Sie mit den Voraussetzungen für Azure Disk Encryption vertraut sein, um die Schritte im Skript zu verstehen. Das Beispielskript muss ggf. an Ihre Umgebung angepasst werden. Dieses Skript erstellt alle Voraussetzungen für Azure Disk Encryption und verschlüsselt eine vorhandene IaaS-VM, indem es den Datenträgerschlüssel mit einem Schlüssel für die Schlüsselverschlüsselung umschließt. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MySecureRG', and 'MySecureVault' with your values.
     # Use Get-AzureRmLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzureRmResourceGroup
     
     $Loc = 'MyLocation';
     $rgname = 'MySecureRG';
     $KeyVaultName = 'MySecureVault'; 
     New-AzureRmResourceGroup –Name $rgname –Location $Loc;
     New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc;
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $KeyVaultResourceId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption;
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname;
     
 #Step 4: Create a new key in the key vault with the Add-AzureKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' with your value. 
     
     $VMName = 'MySecureVM';
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a> Zertifikatbasierte Authentifizierung (optional)
Wenn Sie Zertifikatauthentifizierung verwenden möchten, können Sie eine in Ihren Schlüsseltresor hochladen und für den Client bereitstellen. Bevor Sie das PowerShell-Skript verwenden, sollten Sie mit den Voraussetzungen für Azure Disk Encryption vertraut sein, um die Schritte im Skript zu verstehen. Das Beispielskript muss ggf. an Ihre Umgebung angepasst werden.

     
 ```powershell

 # Fill in "MySecureRG", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a> Zertifikatbasierte Authentifizierung und Schlüssel für die Schlüsselverschlüsselung (optional)

Wenn Sie die Zertifikatsauthentifizierung verwenden und den Schlüssel mit einem Schlüsselverschlüsselungsschlüssel umschließen möchten, verwenden Sie das folgende Skript als Beispiel. Bevor Sie das PowerShell-Skript verwenden, sollten Sie mit allen genannten Voraussetzungen für Azure Disk Encryption vertraut sein, um die Schritte im Skript zu verstehen. Das Beispielskript muss ggf. an Ihre Umgebung angepasst werden.

> [!IMPORTANT]
> Die zertifikatbasierte Azure AD-Authentifizierung wird auf Linux-VMs aktuell nicht unterstützt.



     
 ```powershell
# Fill in 'MySecureRG', 'MySecureVault', and 'MyLocation' (if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Windows](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Linux](azure-security-disk-encryption-linux-aad.md)

---
title: Erste Schritte mit Azure Key Vault | Microsoft Docs
description: Verwenden Sie dieses Tutorials für den Einstieg in den Azure-Schlüsseltresor, um einen geschützten Container in Azure zu erstellen, in dem Sie kryptografischen Schlüssel und geheime Schlüssel in Azure speichern und verwalten.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2018
ms.author: barclayn
ms.openlocfilehash: 2d959eb9c4f55624d0d0915dd5dea4c62ba1f8e5
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505712"
---
# <a name="get-started-with-azure-key-vault"></a>Erste Schritte mit dem Azure-Schlüsseltresor
Dieser Artikel hilft Ihnen bei den ersten Schritten mit Azure Key Vault mit PowerShell und führt Sie durch die folgenden Aktivitäten:
- Erstellen eines festgeschriebenen Containers (Tresors) in Azure
- Verwenden von KeyVault zum Speichern und Verwalten von Kryptografieschlüsseln und Geheimnissen in Azure
- Verwenden dieses Schlüssels oder Kennworts in einer Anwendung

Azure-Tresorschlüssel ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie auf der Seite [Preisübersicht für Schlüsseltresor](https://azure.microsoft.com/pricing/details/key-vault/).

> [!NOTE]
> Dieser Artikel enthält keine Anweisungen zum Erstellen einer Azure-Anwendung. Sie können die [Azure Key Vault-Beispielanwendung](https://www.microsoft.com/download/details.aspx?id=45343) für diese Schritte verwenden.

Anleitungen für die plattformübergreifende Befehlszeilenschnittstelle finden Sie in [diesem entsprechenden Tutorial](key-vault-manage-with-cli2.md).

## <a name="requirements"></a>Requirements (Anforderungen)
Vergewissern Sie sich vor der weiteren Lektüre des Artikels, dass Sie über Folgendes verfügen:

- **Ein Azure-Abonnement**. Falls Sie über kein Abonnement verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)registrieren.
- **Azure PowerShell**, **mindestens Version 1.1.0**. Um Azure PowerShell zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Wenn Sie Azure PowerShell bereits installiert haben und die Version nicht kennen, geben Sie über die Azure PowerShell-Konsole `(Get-Module azure -ListAvailable).Version` ein. Wenn Sie Azure PowerShell in den Versionen 0.9.1 bis 0.9.8 installiert haben, können Sie diese Tutorial mit einigen kleineren Änderungen verwenden. Sie müssen z. B. den Befehl `Switch-AzureMode AzureResourceManager` verwenden, und einige der Befehle des Azure-Schlüsseltresors wurden geändert. Eine Liste der Schlüsseltresor-Cmdlets für die Versionen 0.9.1 bis 0.9.8 finden Sie unter [Cmdlets für den Azure-Schlüsseltresor](/powershell/module/azurerm.keyvault/#key_vault).
- **Eine Anwendung, die zur Verwendung von Key Vault konfiguriert werden kann**. Eine Beispielanwendung erhalten Sie im [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Anweisungen finden Sie in der zugehörigen **Infodatei**.

>[!NOTE]
In diesem Artikel werden grundlegende Kenntnisse von PowerShell und Azure vorausgesetzt. Weitere Informationen zu PowerShell finden Sie unter [Erste Schritte mit Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Um detaillierte Hilfe zu einem Cmdlet aus dem Tutorial zu erhalten, verwenden Sie das **Get-Help** -Cmdlet.

```powershell-interactive
Get-Help <cmdlet-name> -Detailed
```
    
Geben Sie beispielsweise Folgendes ein, um Hilfe zum Cmdlet **Connect-AzureRmAccount** zu erhalten:

```PowerShell
Get-Help Connect-AzureRmAccount -Detailed
```

Lesen Sie ggf. auch die folgenden Artikel, um sich mit dem Azure Resource Manager-Bereitstellungsmodell in Azure PowerShell vertraut zu machen:

* [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview)
* [Verwenden von Azure PowerShell mit dem Ressourcen-Manager](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Verbindungsherstellung mit Ihren Abonnements
Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:  

```PowerShell
Connect-AzureRmAccount
```

>[!NOTE]
 Verwenden Sie den „-Environment“-Parameter, falls Sie eine bestimmte Azure-Instanz verwenden. Beispiel:  
 ```powershell
 Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Azure PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind, und verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen und zur Verwendung für den Azure-Schlüsseltresor ein spezifisches Abonnement verwenden möchten, geben Sie den folgenden Befehl ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzureRmSubscription
```

Geben Sie Folgendes ein, um das zu verwendende Abonnement anzugeben:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

Weitere Informationen zum Konfigurieren von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

## <a id="resource"></a>Erstellen einer neuen Ressourcengruppe
Wenn Sie den Azure-Ressourcen-Manager verwenden, werden alle zugehörigen Ressourcen in einer Ressourcengruppe erstellt. Im Rahmen dieses Tutorials erstellen wir eine neue Ressourcengruppe mit dem Namen **ContosoResourceGroup** :

```powershell
New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East US'
```

## <a id="vault"></a>Erstellen eines Schlüsseltresors
Verwenden Sie das Cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault), um einen Schlüsseltresor zu erstellen. Dieses Cmdlet verfügt über drei erforderliche Parameter: einen für den **Ressourcengruppennamen**, einen für den **Schlüsseltresornamen** und einen für den **geografischen Standort**.

Angenommen, Sie verwenden folgende Werte:
- Tresorname: **ContosoKeyVault**
- Ressourcengruppename: **ContosoResourceGroup**
- Standort: **USA, Osten**

In diesem Fall geben Sie Folgendes ein:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```
![Ausgabe nach Abschluss des Befehls zur Erstellung des Schlüsseltresors](./media/key-vault-get-started/output-after-creating-keyvault.png)

Die Ausgabe dieses Cmdlets zeigt die Eigenschaften des Schlüsseltresors, den Sie erstellt haben. Die zwei wichtigsten Eigenschaften sind diese:

* **Tresorname**: In diesem Beispiel ist dies **ContosoKeyVault**. Sie verwenden diesen Namen für andere Schlüsseltresor-Cmdlets.
* **Tresor-URI:** https://contosokeyvault.vault.azure.net/ in diesem Beispiel. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

Ihr Azure-Konto ist jetzt autorisiert, Vorgänge in diesem Schlüsseltresor durchzuführen. Bisher sind Sie der einzige Benutzer mit dieser Berechtigung.

> [!NOTE]
> Beim Erstellen des neuen Schlüsseltresors wird möglicherweise der Fehler **Das Abonnement ist nicht für die Verwendung des Namespace „Microsoft.KeyVault“ registriert** angezeigt. Falls diese Meldung angezeigt wird, führen Sie `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` aus. Nachdem die Registrierung erfolgreich abgeschlossen wurde, können Sie den Befehl „New-AzureRmKeyVault“ erneut ausführen. Weitere Informationen finden Sie unter [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Hinzufügen eines Schlüssels oder geheimen Schlüssels zum Schlüsseltresor
Möglicherweise müssen Sie auf verschiedene Arten mit Key Vault und Schlüsseln oder Geheimnissen interagieren.

### <a name="azure-key-vault-generates-a-software-protected-key"></a>Azure Key Vault generiert einen softwaregeschützten Schlüssel.

Wenn Azure Key Vault einen softwaregeschützten Schlüssel für Sie erstellen soll, verwenden Sie das Cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), und geben Sie Folgendes ein:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'
```
Geben Sie zum Anzeigen des URI für diesen Schlüsseltyp Folgendes ein:
```powershell
$key.id
```

Sie können anhand des URI auf einen Schlüssel verweisen, den Sie erstellt oder in Azure Key Vault hochgeladen haben. Verwenden Sie **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**, um die aktuelle Version zu erhalten, oder **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**, um genau diese Version zu erhalten.  

### <a name="importing-an-existing-pfx-file-into-azure-key-vault"></a>Importieren einer vorhandenen PFX-Datei in Azure Key Vault

Falls vorhandene Schlüssel in einer PFX-Datei gespeichert sind, die Sie in Azure Key Vault hochladen möchten, unterscheiden sich die Schritte. Beispiel: 
- Sie haben einen vorhandenen softwaregeschützten Schlüssel in einer PFX-Datei.
- Der Name der PFX-Datei lautet „softkey.pfx“. 
- Die Datei ist auf dem Laufwerk C gespeichert.

In diesem Fall können Sie Folgendes eingeben:

```powershell
$securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force  // This stores the password 123 in the variable $securepfxpwd
```

Geben Sie anschließend den folgenden Code ein, um den Schlüssel aus der PFX-Datei zu importieren, die den Schlüssel mithilfe von Software im Schlüsseltresordienst schützt:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoImportedPFX' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd
```

Geben Sie zur Anzeige des URI für diesen Schlüssel Folgendes ein:

```powershell
$Key.id
```
Geben Sie Folgendes ein, um Ihren Schlüssel anzuzeigen: 

```powershell
Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'
```
Wenn Sie die Eigenschaften der PFX-Datei im Portal anzeigen möchten, sieht die Anzeige in etwa wie in der folgenden Abbildung dargestellt aus.

![Anzeige eines Zertifikats im Portal](./media/key-vault-get-started/imported-pfx.png)
### <a name="to-add-a-secret-to-azure-key-vault"></a>So fügen Sie Azure Key Vault ein Geheimnis hinzu

Um Azure Key Vault ein Geheimnis hinzuzufügen (ein Kennwort namens „SQLPassword“ mit dem Wert „Pa$$w0rd“), konvertieren Sie zunächst den Wert „Pa$$w0rd“ in eine sichere Zeichenfolge, indem Sie Folgendes eingeben:

```powershell    
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Geben Sie anschließend Folgendes ein:

```powershell
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```


Jetzt können Sie mit dem zugehörigen URI auf das Kennwort verweisen, das Sie dem Azure-Schlüsseltresor hinzugefügt haben. Verwenden Sie **https://ContosoVault.vault.azure.net/secrets/SQLPassword**, um immer die aktuelle Version zu erhalten, oder **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**, um genau diese Version zu erhalten.

Geben Sie zur Anzeige des URI für diesen geheimen Schlüssel Folgendes ein:

```powershell
$secret.Id
```
Geben Sie zum Anzeigen Ihres Geheimnisses Folgendes ein: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`. Alternativ können Sie das Geheimnis im Portal anzeigen.

![secret](./media/key-vault-get-started/secret-value.png)

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:
```powershell
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```
Jetzt sind Schlüsseltresor und Schlüssel oder geheimer Schlüssel bereit für die Verwendung durch die Anwendung. Sie müssen die Anwendungen zur Verwendung der Schlüssel bzw. geheimen Schlüssel autorisieren.  

## <a id="register"></a>Registrieren einer Anwendung mit Azure Active Directory
Dieser Schritt wird üblicherweise durch einen Entwickler auf einem separaten Computer durchgeführt. Dieser Schritt ist nicht auf Azure Key Vault beschränkt. Ausführliche Anweisungen zum Registrieren einer Anwendung in Azure Active Directory finden Sie im Artikel [Integrieren von Anwendungen in Azure Active Directory](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) oder [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md)

> [!IMPORTANT]
> Zum Abschließen dieses Tutorials müssen sich Ihr Konto, der Schlüsseltresor und die in diesem Schritt registrierte Anwendung im selben Azure-Verzeichnis befinden.


Anwendungen, die einen Schlüsseltresor verwenden, müssen sich mithilfe eines Azure Active Directory-Tokens authentifizieren. Hierzu muss der Besitzer der Anwendung die Anwendung zunächst in Azure Active Directory registrieren. Zum Abschluss der Registrierung erhält der Anwendungsbesitzer die folgenden Werte:

- Eine **Anwendungs-ID** 
- Einen **Authentifizierungsschlüssel** (auch bezeichnet als gemeinsames Geheimnis) 

Die Anwendung muss beide dieser Werte in Azure Active Directory vorlegen, um ein Token zu erhalten.  Wie die Anwendung konfiguriert wird, um dies zu erreichen, richtet sich nach der Anwendung. Bei der [Key Vault-Beispielanwendung](https://www.microsoft.com/download/details.aspx?id=45343) legt der Anwendungsbesitzer diese Werte in der Datei „app.config“ fest.


So registrieren Sie die Anwendungen in Azure Active Directory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Bereich auf **App-Registrierungen**. Falls keine App-Registrierungen angezeigt werden, klicken Sie auf **Weitere Dienste**, und suchen Sie nach der Registrierung.  
>[!NOTE]
Sie müssen dasselbe Verzeichnis auswählen, in dem auch das Azure-Abonnement enthalten ist, mit dem Sie Ihren Schlüsseltresor erstellt haben. 
3. Klicken Sie auf **Registrierung einer neuen Anwendung**.
4. Geben Sie auf dem Blatt **Erstellen** einen Namen für die Anwendung ein, wählen Sie dann **WEBANWENDUNG UND/ODER WEB-API** (Standardeinstellung) aus, und geben Sie die **ANMELDE-URL** für die Webanwendung ein. Falls Sie diese Information gerade nicht zur Hand haben, können Sie einen Fantasiewert angeben (beispielsweise http://test1.contoso.com). Hierbei spielt es keine Rolle, ob diese Websites wirklich vorhanden sind. 

    ![Registrierung einer neuen Anwendung](./media/key-vault-get-started/new-application-registration.png)
    >[!WARNING]
    Vergewissern Sie sich, dass Sie **WEBANWENDUNG UND/ODER WEB-API** ausgewählt haben. Andernfalls wird die Option **Schlüssel** nicht unter „Einstellungen“ angezeigt.

5. Klicken Sie auf die Schaltfläche **Erstellen** .
6. Nach Abschluss der App-Registrierung können Sie die Liste der registrierten Apps anzeigen. Suchen Sie nach der App, die Sie gerade registriert haben, und klicken Sie darauf.
7. Klicken Sie auf das Blatt **Registrierte App**, und kopieren Sie die **Anwendungs-ID**.
8. Klicken Sie auf **Alle Einstellungen**.
9. Klicken Sie auf dem Blatt **Einstellungen** auf **Schlüssel**.
9. Geben Sie eine Beschreibung in das Feld **Schlüsselbeschreibung** ein, wählen Sie eine Dauer aus, und klicken Sie dann auf **SPEICHERN**. Die Seite wird aktualisiert und zeigt jetzt einen Schlüsselwert. 
10. Sie verwenden die **Anwendungs-ID** und die Informationen zum **Schlüssel** im nächsten Schritt, um Berechtigungen für den Tresor festzulegen.

## <a id="authorize"></a>Autorisieren der Anwendung zum Verwenden des Schlüssels oder geheimen Schlüssels
Die Anwendung kann auf zwei Arten für den Zugriff auf den Schlüssel oder das Geheimnis im Tresor autorisiert werden.

### <a name="using-powershell"></a>Verwenden von PowerShell
Wenn Sie PowerShell verwenden möchten, verwenden Sie das Cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).

Wenn Ihr Tresorname beispielsweise **ContosoKeyVault** lautet, die Anwendung, die Sie autorisieren möchten, über die Client-ID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed verfügt und Sie die Anwendung zum Entschlüsseln und Anmelden mit Schlüsseln in Ihrem Tresor autorisieren möchten, führen Sie Folgendes aus:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Wenn Sie dieselbe Anwendung so autorisieren möchten, dass sie geheime Schlüssel im Tresor liest, führen Sie Folgendes aus:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get
```
### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals
So ändern Sie die Autorisierung einer Anwendung für die Verwendung von Schlüsseln oder Geheimnissen:
1. Klicken Sie auf dem Ressourcenblatt von Key Vault auf **Zugriffsrichtlinien**.
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche „+ Neu hinzufügen“.
3. Klicken Sie auf **Prinzipal auswählen**, und wählen Sie die zuvor erstellte Anwendung aus.
4. Wählen Sie in der Dropdownliste **Schlüsselberechtigungen** die Berechtigungen „Entschlüsseln“ und „Signieren“ aus, um die Anwendung für die Entschlüsselung und Signierung mit Schlüsseln aus Ihrem Tresor zu autorisieren.
5. Wählen Sie in der Dropdownliste **Berechtigungen für Geheimnis** die Option „Abrufen“ aus, um der Anwendung das Lesen von Geheimnissen aus dem Tresor zu ermöglichen.

## <a id="HSM"></a>Verwenden eines Hardwaresicherheitsmoduls (HSM)
Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Wenn diese Anforderung auf Sie nicht zutrifft, überspringen Sie diesen Abschnitt, und wechseln Sie zu [Löschen des Schlüsseltresors und zugeordneter Schlüssel und geheimer Schlüssel](#delete).

Zum Erstellen dieser HSM-geschützten Schlüssel müssen Sie die [Azure Key Vault Premium-Dienstebene verwenden, um HSM-geschützte Schlüssel zu unterstützen](https://azure.microsoft.com/pricing/free-trial/). Darüber hinaus steht diese Funktion nicht für Azure China zur Verfügung.

Wenn Sie den Schlüsseltresor erstellen, fügen Sie den Parameter **-SKU** hinzu:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US' -SKU 'Premium'
```


Sie können diesem Schlüsseltresor softwaregeschützte Schlüssel (wie weiter oben gezeigt) und HSM-geschützte Schlüssel hinzufügen. Legen Sie den Parameter **-Destination** auf "HSM" fest, um einen HSM-geschützten Schlüssel zu erstellen:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'
```

Sie können mit dem folgenden Befehl einen Schlüssel aus einer PFX-Datei auf Ihrem Computer importieren. Dieser Befehl importiert den Schlüssel in HSMs im Schlüsseltresordienst:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'
```

Der nächste Befehl importiert ein BYOK-Paket (Bring Your Own Key). In diesem Fall können Sie Ihren Schlüssel im lokalen HSM generieren und ihn in HSMs im Schlüsseltresordienst übertragen, ohne dass der Schlüssel die HSM-Grenzen verlässt:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'
```

Ausführlichere Informationen zum Generieren dieses BYOK-Pakets finden Sie unter [Generieren und Übertragen von HSM-geschützten Schlüsseln für den Azure-Schlüsseltresor](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Löschen des Schlüsseltresors und der zugeordneten Schlüssel und geheimen Schlüssel
Wenn Sie den Schlüsseltresor und die darin enthaltenen Schlüssel und geheimen Schlüssel nicht mehr benötigen, können Sie den Schlüsseltresor mit dem Cmdlet [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) löschen:

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'
```

Alternativ können Sie eine gesamte Azure-Ressourcengruppe löschen, die den Schlüsseltresor und alle weiteren Ressourcen enthält, die Sie in diese Gruppe eingeschlossen haben:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'
```

## <a id="other"></a>Weitere Azure PowerShell-Cmdlets
Die folgenden weiteren Befehle sind möglicherweise ebenfalls für das Verwalten eines Azure-Schlüsseltresors von Nutzen:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Dieser Befehl ruft eine tabellarische Anzeige aller Schlüssel und ausgewählten Eigenschaften ab.
- `$Keys[0]`: Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel an.
- `Get-AzureKeyVaultSecret`: Dieser Befehl ruft eine tabellarische Anzeige der Namen aller geheimen Schlüssel und ausgewählten Eigenschaften ab.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Beispiel dazu, wie ein bestimmter Schlüssel entfernt wird.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Beispiel dazu, wie ein bestimmter geheimer Schlüssel entfernt wird.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über den Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-whatis.md)
- Informationen zur Verwendung des Schlüsseltresors finden Sie unter [Azure-Schlüsseltresor-Protokollierung](key-vault-logging.md).
- Ein weiterführendes Tutorial zur Verwendung des Azure-Schlüsseltresors in einer Webanwendung finden Sie unter [Verwenden des Azure-Schlüsseltresors aus einer Webanwendung](key-vault-use-from-web-application.md).
- Eine Referenz zur Programmierung finden Sie im [Entwicklerhandbuch für den Azure-Schlüsseltresor](key-vault-developers-guide.md).
- Eine Liste mit den aktuellen Azure PowerShell-Cmdlets für den Azure-Schlüsseltresor (Azure Key Vault) finden Sie unter [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault) (Cmdlets für den Azure-Schlüsseltresor).

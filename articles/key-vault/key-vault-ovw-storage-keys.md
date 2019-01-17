---
ms.assetid: ''
title: Per Azure Key Vault verwaltetes Speicherkonto – CLI
description: Speicherkontoschlüssel bieten eine nahtlose Integration zwischen Azure Key Vault und dem schlüsselbasierten Zugriff auf das Azure Storage-Konto.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 3ee0d19c174490d558a8ff06d3f5e038ffff211f
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064439"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Per Azure Key Vault verwaltetes Speicherkonto – CLI

> [!NOTE]
> [Azure Storage unterstützt jetzt die AAD-Autorisierung](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Wir empfehlen die Verwendung von Azure Active Directory für die Authentifizierung und Autorisierung für Azure Storage, da sich die Benutzer keine Gedanken um die Rotation ihrer Speicherkontenschlüssel machen müssen.

- Azure Key Vault verwaltet Schlüssel eines Azure Storage-Kontos.
    - Intern kann Azure Key Vault die Schlüssel mit einem Azure-Speicherkonto auflisten (synchronisieren).    
    - Azure Key Vault generiert (rotiert) die Schlüssel in regelmäßigen Abständen.
    - Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben.
    - Azure Key Vault verwaltet Schlüssel von Speicherkonten und klassischen Speicherkonten.

<a name="prerequisites"></a>Voraussetzungen
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Installieren der Azure CLI   
2. [Erstellen eines Speicherkontos](https://azure.microsoft.com/services/storage/)
    - Führen Sie die Schritte in diesem [Dokument](https://docs.microsoft.com/azure/storage/) aus, um ein Speicherkonto zu erstellen.  
    - **Benennungsrichtlinien:** Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Schritt-für-Schritt-Anweisungen zur Verwendung von Key Vault zum Verwalten von Speicherkontoschlüsseln
--------------------------------------------------------------------------------
In den unten stehenden Anweisungen weisen wir Key Vault als Dienst zu, der Operatorberechtigungen für Ihr Speicherkonto haben soll.

> [!NOTE]
> Beachten Sie: Sobald Sie die mit Azure Key Vault verwalteten Speicherkontoschlüssel eingerichtet haben, sollten sie **NUR** noch über Key Vault geändert werden. Verwaltete Speicherkontoschlüssel bedeuten, dass Key Vault das Wechseln des Speicherkontoschlüssels verwaltet.

1. Führen Sie nach dem Erstellen eines Speicherkontos den folgenden Befehl aus, um die Ressourcen-ID des Speicherkontos zu erhalten, das Sie verwalten möchten.

    ```
    az storage account show -n storageaccountname (Copy ID field out of the result of this command)
    ```
    
2. Rufen Sie die Anwendungs-ID des Dienstprinzipals von Azure Key Vault ab. 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Weisen Sie die Speicherschlüsseloperator-Rolle zur Azure Key Vault-Identität zu.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ApplicationIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Erstellen Sie ein per Key Vault verwaltetes Speicherkonto.     <br /><br />
   Unten legen wir einen Zeitraum von 90 Tagen für die erneute Generierung fest. Nach 90 Tagen generiert Key Vault „key1“ neu und wechselt für den aktiven Schlüssel von „key2“ zu „key1“.
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    Falls der Benutzer das Speicherkonto nicht erstellt hat und keine Berechtigungen für das Speicherkonto hat, stellen die folgenden Schritte die Berechtigungen für Ihr Konto ein, um sicherzustellen, dass Sie alle Speicherberechtigungen im Key Vault verwalten können.
 > [!NOTE] 
    Für den Fall, dass der Benutzer keine Berechtigungen für das Speicherkonto besitzt, rufen wir zuerst die Objekt-ID des Benutzers ab.

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```
### <a name="relavant-azure-cli-cmdlets"></a>Relevante Azure CLI-Cmdlets
- [Azure CLI-Storage-Cmdlets](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

### <a name="relevant-powershell-cmdlets"></a>Relevante PowerShell-Cmdlets

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Weitere Informationen

- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-Teamblog](https://blogs.technet.microsoft.com/kv/)

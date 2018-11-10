---
ms.assetid: ''
title: Azure Key Vault-Speicherkontoschlüssel
description: Speicherkontoschlüssel bieten eine nahtlose Integration zwischen Azure Key Vault und dem schlüsselbasierten Zugriff auf das Azure Storage-Konto.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 02fffe7c4a3acff6ce6d68046eee4286003b1766
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232221"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Speicherkontoschlüssel

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
      
<a name="step-by-step-instructions"></a>Schrittweise Anleitung
-------------------------
In den unten stehenden Anweisungen weisen wir Key Vault als Dienst zu, der Operatorberechtigungen für Ihr Speicherkonto haben soll.

1. Führen Sie nach dem Erstellen eines Speicherkontos den folgenden Befehl aus, um die Ressourcen-ID des Speicherkontos zu erhalten, das Sie verwalten möchten.

    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
    
2. Rufen Sie die Anwendungs-ID des Dienstprinzipals von Azure Key Vault ab. 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Weisen Sie die Speicherschlüsseloperator-Rolle zur Azure Key Vault-Identität zu.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
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

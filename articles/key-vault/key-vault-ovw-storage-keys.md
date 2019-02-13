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
ms.openlocfilehash: 152e1e5892e3a72286205c2f5bf4e18b2a2bcbf7
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814842"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Per Azure Key Vault verwaltetes Speicherkonto – CLI

> [!NOTE]
> [Die Azure-Speicherintegration in Azure Active Directory (Azure AD) befindet sich jetzt in der Vorschau](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Wir empfehlen die Verwendung von Azure AD für die Authentifizierung und Autorisierung, da es wie Azure Key Vault einen auf OAuth2-Token basierenden Zugriff auf Azure-Speicher bietet. Dies ermöglicht Ihnen Folgendes:
> - Authentifizieren Sie Ihre Clientanwendung mithilfe einer Anwendungs- oder Benutzeridentität, statt Speicherkontoanmeldeinformationen angeben zu müssen. 
> - Verwenden Sie für die Ausführung in Azure eine [von Azure AD verwaltete Identität](/azure/active-directory/managed-identities-azure-resources/). Verwaltete Identitäten machen die Clientauthentifizierung und das Speichern von Anmeldeinformationen in oder mit Ihrer Anwendung überflüssig.
> - Verwenden Sie die rollenbasierte Zugriffssteuerung (RBAC) zum Verwalten der Autorisierung (wird auch von Key Vault unterstützt).

- Azure Key Vault verwaltet Schlüssel eines Azure Storage-Kontos.
    - Intern kann Azure Key Vault die Schlüssel mit einem Azure-Speicherkonto auflisten (synchronisieren).    
    - Azure Key Vault generiert (rotiert) die Schlüssel in regelmäßigen Abständen.
    - Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben.
    - Azure Key Vault verwaltet Schlüssel von Speicherkonten und klassischen Speicherkonten.
    
> [!IMPORTANT]
> Ein Azure AD-Mandant stellt jede registrierte Anwendung mit einem **[Dienstprinzipal](/azure/active-directory/develop/developer-glossary#service-principal-object)** bereit, der als Anwendungsidentität fungiert. Anhand der Anwendungs-ID des Dienstprinzipals kann über die rollenbasierte Zugriffssteuerung (RBAC) Zugriff auf andere Ressourcen erteilt werden. Key Vault ist eine Microsoft-Anwendung, deshalb ist sie in allen Azure AD-Mandanten über dieselbe Anwendungs-ID in jeder Azure-Cloud vorab registriert:
> - Azure AD-Mandanten in der Azure Government-Cloud verwenden die Anwendungs-ID `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure AD-Mandanten in der öffentlichen Azure-Cloud und allen weiteren verwenden die Anwendungs-ID `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

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
    az storage account show -n storageaccountname 
    ```
    Kopieren Sie das ID-Feld aus dem Ergebnis des oben aufgeführten Befehls.
    
2. Rufen Sie die Objekt-ID des Azure Key Vault-Dienstprinzipals ab, indem Sie folgenden Befehl ausführen.

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Nachdem die Ausführung dieses Befehls erfolgreich abgeschlossen wurde, suchen Sie die Objekt-ID im Ergebnis.
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Weisen Sie die Speicherschlüsseloperator-Rolle zur Azure Key Vault-Identität zu.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Erstellen Sie ein per Key Vault verwaltetes Speicherkonto.     <br /><br />
   Unten legen wir einen Zeitraum von 90 Tagen für die erneute Generierung fest. Nach 90 Tagen generiert Key Vault „key1“ neu und wechselt für den aktiven Schlüssel von „key2“ zu „key1“. „Key1“ wird jetzt als aktiver Schlüssel markiert. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    Falls der Benutzer das Speicherkonto nicht erstellt hat und keine Berechtigungen für das Speicherkonto hat, stellen die folgenden Schritte die Berechtigungen für Ihr Konto ein, um sicherzustellen, dass Sie alle Speicherberechtigungen im Key Vault verwalten können.
    
 > [!NOTE] 
 > Für den Fall, dass der Benutzer keine Berechtigungen für das Speicherkonto besitzt, rufen wir zuerst die Objekt-ID des Benutzers ab.


    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    
    ```
    
## <a name="how-to-access-your-storage-account-with-sas-tokens"></a>Zugreifen auf Ihr Speicherkonto mit SAS-Token

In diesem Abschnitt wird erläutert, wie Sie Vorgänge für Ihr Speicherkonto durchführen können, indem Sie [SAS-Token](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) aus Key Vault abrufen.

Im folgenden Abschnitt wird gezeigt, wie Sie Ihren in Key Vault gespeicherten Speicherkontenschlüssel abrufen und damit eine SAS-Definition (Shared Access Signature) für Ihr Speicherkonto erstellen können.

> [!NOTE] 
  Es gibt 3 Möglichkeiten, sich bei Key Vault zu authentifizieren, wie Sie in den [Grundlegenden Konzepten](key-vault-whatis.md#basic-concepts) lesen können.
> - Verwenden der verwalteten Dienstidentität (dringend empfohlen)
> - Verwenden eines Dienstprinzipals und eines Zertifikats 
> - Verwenden eines Dienstprinzipals und eines Kennworts (NICHT empfohlen)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Wenn Ihr SAS-Token in Kürze abläuft, würden Sie das SAS-Token erneut aus Key Vault abrufen und den Code aktualisieren.

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


### <a name="relavant-azure-cli-cmdlets"></a>Relevante Azure CLI-Cmdlets
[Azure CLI-Storage-Cmdlets](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

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

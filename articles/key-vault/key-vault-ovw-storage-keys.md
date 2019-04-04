---
ms.assetid: ''
title: Per Azure Key Vault verwaltetes Speicherkonto – CLI
description: Speicherkontoschlüssel bieten eine nahtlose Integration zwischen Azure Key Vault und dem schlüsselbasierten Zugriff auf das Azure Storage-Konto.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: c2107e501affd5e3dd22e0fbc83d078b51d414a5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57841139"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Per Azure Key Vault verwaltetes Speicherkonto – CLI

> [!NOTE]
> [Die Azure-Speicherintegration in Azure Active Directory (Azure AD) befindet sich jetzt in der Vorschau](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Wir empfehlen die Verwendung von Azure AD für die Authentifizierung und Autorisierung, da es wie Azure Key Vault einen auf OAuth2-Token basierenden Zugriff auf Azure-Speicher bietet. Dies ermöglicht Ihnen Folgendes:
> - Authentifizieren Sie Ihre Clientanwendung mithilfe einer Anwendungs- oder Benutzeridentität, statt Speicherkontoanmeldeinformationen angeben zu müssen. 
> - Verwenden Sie für die Ausführung in Azure eine [von Azure AD verwaltete Identität](/azure/active-directory/managed-identities-azure-resources/). Verwaltete Identitäten machen die Clientauthentifizierung und das Speichern von Anmeldeinformationen in oder mit Ihrer Anwendung überflüssig.
> - Verwenden Sie die rollenbasierte Zugriffssteuerung (RBAC) zum Verwalten der Autorisierung (wird auch von Key Vault unterstützt).

Ein [Azure-Speicherkonto](/azure/storage/storage-create-storage-account) verwendet Anmeldeinformationen, die sich aus einem Kontonamen und einem Schlüssel zusammensetzen. Der Schlüssel wird automatisch generiert und dient eher als „Kennwort“ denn als kryptografischer Schlüssel. Key Vault kann diese Speicherkontoschlüssel verwalten, indem sie als [Key Vault-Geheimnisse](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets) gespeichert werden. 

## <a name="overview"></a>Übersicht

Das Feature für ein in Key Vault verwaltetes Speicherkonto führt verschiedene Verwaltungsfunktionen in Ihrem Auftrag aus:

- Auflisten (Synchronisieren) der Schlüssel mit einem Azure-Speicherkonto
- Erneutes Generieren (Rotieren) der Schlüssel in regelmäßigen Abständen
- Verwalten der Schlüssel sowohl für Speicherkonten als auch für klassische Speicherkonten
- Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben.

Beachten Sie bei Verwendung des Features für verwaltete Speicherkontoschlüssel Folgendes:

- **Lassen Sie Ihre Speicherkontoschlüssel nur durch Key Vault verwalten.** Versuchen Sie nicht, die Schlüssel selbst zu verwalten, da dies zu Konflikten mit den Key Vault-Prozessen führt.
- **Lassen Sie Speicherkontoschlüssel nicht durch mehrere Key Vault-Objekte verwalten.**
- **Generieren Sie Ihre Speicherkontoschlüssel nicht manuell neu.** Es wird empfohlen, die Schlüssel über Key Vault neu zu generieren.
- Key Vault mit der Verwaltung Ihres Speicherkonto zu beauftragen, kann vorerst von einem Benutzerprinzipal und nicht von einem Dienstprinzipal durchgeführt werden.

Das folgende Beispiel zeigt, wie Sie Key Vault das Verwalten Ihrer Speicherkontoschlüssel ermöglichen.

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
Konzeptionell werden die folgenden Schritte befolgt:
- Wir rufen zuerst ein (bereits vorhandenes) Speicherkonto ab.
- Dann rufen wir einen (bereits vorhandenen) Schlüsseltresor ab.
- Anschließend fügen wir dem Tresor ein von KeyVault verwaltetes Speicherkonto hinzu, wobei Key1 als aktiver Schlüssel festgelegt, der nach 180 Tagen erneuert werden muss.
- Schließlich legen wir mit Key1 einen Speicherkontext für das angegebene Speicherkonto fest.

In den unten stehenden Anweisungen weisen wir Key Vault als Dienst zu, der Operatorberechtigungen für Ihr Speicherkonto haben soll.

> [!NOTE]
> Beachten Sie: Sobald Sie die mit Azure Key Vault verwalteten Speicherkontoschlüssel eingerichtet haben, sollten sie **NUR** noch über Key Vault geändert werden. Verwaltete Speicherkontoschlüssel bedeuten, dass Key Vault das Wechseln des Speicherkontoschlüssels verwaltet.

> [!IMPORTANT]
> Ein Azure AD-Mandant stellt jede registrierte Anwendung mit einem **[Dienstprinzipal](/azure/active-directory/develop/developer-glossary#service-principal-object)** bereit, der als Anwendungsidentität fungiert. Anhand der Anwendungs-ID des Dienstprinzipals kann über die rollenbasierte Zugriffssteuerung (RBAC) Zugriff auf andere Ressourcen erteilt werden. Key Vault ist eine Microsoft-Anwendung, deshalb ist sie in allen Azure AD-Mandanten über dieselbe Anwendungs-ID in jeder Azure-Cloud vorab registriert:
> - Azure AD-Mandanten in der Azure Government-Cloud verwenden die Anwendungs-ID `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure AD-Mandanten in der öffentlichen Azure-Cloud und allen weiteren verwenden die Anwendungs-ID `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.


1. Führen Sie nach dem Erstellen eines Speicherkontos den folgenden Befehl aus, um die Ressourcen-ID des Speicherkontos zu erhalten, das Sie verwalten möchten.

    ```
    az storage account show -n storageaccountname 
    ```
    Kopieren Sie das ID-Feld aus dem Ergebnis des oben aufgeführten Befehls, das wie folgt aussieht.
    ```
    /subscriptions/0xxxxxx-4310-48d9-b5ca-0xxxxxxxxxx/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    
2. Weisen Sie Key Vault die RBAC-Rolle „Speicherkonto-Schlüsseloperator-Dienstrolle“ zu und beschränken Sie den Zugriffsbereich auf Ihr Speicherkonto. Für ein klassisches Speicherkonto verwenden Sie „Klassische Speicherkonto-Schlüsseloperator-Dienstrolle“.
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    „93c27d83-f79b-4cb2-8dd4-4aa716542e74“ ist die Objekt-ID für Key Vault in der Public Cloud. Informationen zur Objekt-ID für Key Vault in nationalen Clouds finden Sie oben im Abschnitt „Wichtig“.
    
3. Erstellen Sie ein per Key Vault verwaltetes Speicherkonto.     <br /><br />
   Unten legen wir einen Zeitraum von 90 Tagen für die erneute Generierung fest. Nach 90 Tagen generiert Key Vault „key1“ neu und wechselt für den aktiven Schlüssel von „key2“ zu „key1“. „Key1“ wird jetzt als aktiver Schlüssel markiert. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Schritt-für-Schritt-Anweisungen zur Verwendung von Key Vault zum Erstellen und Generieren von SAS-Token
--------------------------------------------------------------------------------
Sie können Key Vault auch zum Generieren von SAS-Token (Shared Access Signature) auffordern. Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Mit einer SAS können Sie Clients Zugriff auf Ressourcen unter Ihrem Speicherkonto gewähren, ohne dafür Kontoschlüssel weitergeben zu müssen. Der wichtigste Aspekt bei der Verwendung von Shared Access Signatures in Ihren Anwendungen: Bei einer SAS handelt es sich um eine sichere Methode zur Freigabe Ihrer Speicherressurcen, ohne dass Sie Kompromisse bei der Sicherheit der Kontoschlüssel eingehen müssen.

Nachdem die oben aufgeführten Schritte erfolgt sind, können Sie die folgenden Befehle ausführen, um Key Vault aufzufordern, SAS-Token für Sie zu generieren. 

Über die nachstehenden Schritte werden die folgenden Aufgaben erledigt:
- In Ihrem Tresor „<VaultName>“ wird im von KeyVault verwalteten Speicherkonto „<YourStorageAccountName>“ eine SAS-Kontodefinition mit dem Namen „<YourSASDefinitionName>“ festgelegt. 
- Es wird ein Konto-SAS-Token für den Blob-, Datei-, Tabellen- und Warteschlangendienst und für die Ressourcentypen „Dienst“, „Container“ und Objekt mit allen Berechtigungen über HTTPS und mit dem angegebenen Start- und Enddatum erstellt.
- Im Tresor wird eine von KeyVault verwaltete Speicher-SAS-Definition festgelegt, und zwar mit dem Vorlagen-URI als oben erstelltem SAS-Token sowie mit dem SAS-Typ „Konto“ und einer Gültigkeit von n Tagen.
- Das tatsächliche Zugriffstoken wird aus dem KeyVault-Geheimnis abgerufen, das der SAS-Definition entspricht.

1. In diesem Schritt erstellen wir eine SAS-Definition. Sobald diese SAS-Definition erstellt wurde, können Sie Key Vault auffordern, weitere SAS-Token für Sie zu generieren. Dieser Vorgang erfordert die Berechtigung „storage/setsas“.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
Weitere Hilfe zum obigen Vorgang finden Sie [hier](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas).

Wenn dieser Vorgang erfolgreich ausgeführt wurde, sollte eine Ausgabe ähnlich wie unten dargestellt angezeigt werden. Kopieren Sie Folgendes:

```console
   "se=2020-01-01&sp=***"
```

1. In diesem Schritt verwenden wir die oben generierte Ausgabe ($sasToken), um eine SAS-Definition zu erstellen. Weitere Dokumentation finden Sie [hier](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters).   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > Für den Fall, dass der Benutzer keine Berechtigungen für das Speicherkonto besitzt, rufen wir zuerst die Objekt-ID des Benutzers ab.

 ```
 az ad user show --upn-or-object-id "developer@contoso.com"

 az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
 ```
    
## <a name="fetch-sas-tokens-in-code"></a>Abrufen von SAS-Token in Code

In diesem Abschnitt wird erläutert, wie Sie Vorgänge für Ihr Speicherkonto durchführen können, indem Sie [SAS-Token](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) aus Key Vault abrufen.

Im folgenden Abschnitt wird das Abrufen von SAS-Token veranschaulicht, nachdem eine SAS-Definition wie oben gezeigt erstellt wurde.

> [!NOTE]
>   Es gibt 3 Möglichkeiten, sich bei Key Vault zu authentifizieren, wie Sie in den [Grundlegenden Konzepten](key-vault-whatis.md#basic-concepts) lesen können.
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

### <a name="relevant-azure-cli-commands"></a>Entsprechende Azure CLI-Befehle

[CLI-Befehle für Azure Storage](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>Weitere Informationen

- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-Teamblog](https://blogs.technet.microsoft.com/kv/)

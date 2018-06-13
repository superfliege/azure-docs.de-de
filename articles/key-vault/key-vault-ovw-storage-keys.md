---
ms.assetid: ''
title: Azure Key Vault-Speicherkontoschlüssel
description: Speicherkontoschlüssel bieten eine nahtlose Integration zwischen Azure Key Vault und dem schlüsselbasierten Zugriff auf das Azure Storage-Konto.
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 4f42a47a6d934bf0538efccbcf7f057fd28e2c03
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179587"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Speicherkontoschlüssel

Vor Azure Key Vault-Speicherkontoschlüsseln mussten Entwickler ihre eigenen Azure-Speicherkontoschlüssel (ASA, Azure Storage Account) verwalten und sie manuell oder durch externe Automatisierung rotieren. Key Vault-Speicherkontoschlüssel werden jetzt als [Key Vault-Geheimnisse](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) für die Authentifizierung bei einem Azure Storage-Konto implementiert.

Das Hauptfeature ASA (Azure Storage Account, Azure-Speicherkonto) verwaltet die Geheimnisrotation für Sie. Es macht außerdem Ihren direkten Kontakt mit einem ASA-Schlüssel überflüssig, indem es Shared Access Signatures (SAS) als Methode anbietet.

Allgemeinere Informationen zu Azure-Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/storage-create-storage-account) .

## <a name="supporting-interfaces"></a>Unterstützende Schnittstellen

Sie finden eine vollständige Liste sowie Links zu unseren Programmierungs- und Skriptschnittstellen in [Programmieren mit dem Schlüsseltresor](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Was Key Vault verwaltet

Key Vault führt mehrere interne Verwaltungsfunktionen in Ihrem Auftrag aus, wenn Sie verwaltete Speicherkontoschlüssel verwenden.

- Azure Key Vault verwaltet Schlüssel eines Azure Storage-Kontos.
    - Intern kann Azure Key Vault die Schlüssel mit einem Azure-Speicherkonto auflisten (synchronisieren).
    - Azure Key Vault generiert (rotiert) die Schlüssel in regelmäßigen Abständen.
    - Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben.
    - Azure Key Vault verwaltet Schlüssel von Speicherkonten und klassischen Speicherkonten.
- Azure Key Vault ermöglicht Ihnen, dem Tresor-/Objektbesitzer, SAS-Definitionen (Konto oder SAS-Dienst) zu erstellen.
    - Der mithilfe der SAS-Definition erstellte SAS-Wert wird über den REST-URI-Pfad als Geheimnis zurückgegeben. Weitere Informationen finden Sie unter [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations) (Azure Key Vault-Speicherkontobetrieb).

## <a name="naming-guidance"></a>Benennungsrichtlinien

- Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.
- Ein SAS-Definitionsname muss 1-102 Zeichen lang sein und darf nur 0-9, a-Z und A-Z enthalten.

## <a name="developer-experience"></a>Entwicklerumgebung

### <a name="before-azure-key-vault-storage-keys"></a>Vor Azure Key Vault-Speicherschlüsseln

Entwickler mussten die folgenden Methoden auf einen Speicherkontoschlüssel anwenden, um auf Azure-Speicher zuzugreifen.
1. Speichern Sie die Verbindungszeichenfolge oder das SAS-Token in den Azure AppService-Anwendungseinstellungen oder einem anderen Speicher.
1. Rufen Sie beim Anwendungsstart die Verbindungszeichenfolge oder das SAS-Token ab.
1. Erstellen Sie [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) für die Interaktion mit dem Speicher.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Ab Azure Key Vault-Speicherschlüsseln

Entwickler erstellen ein [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)-Element und nutzen es, um das SAS-Token für ihren Speicher abzurufen. Anschließend erstellen Sie mit dem Token das [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount)-Element.

```cs
// Create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();

// Use the blobClientWithSas
...

// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Anleitung für Entwickler

- Lassen Sie Ihre ASA-Schlüssel nur durch Key Vault verwalten. Versuchen Sie nicht, sie selbst zu verwalten, da Sie mit den Prozessen von Key Vault in Konflikt geraten.
- Lassen Sie ASA-Schlüssel nicht durch mehrere Key Vault-Objekte verwalten.
- Wenn Sie Ihre ASA-Schlüssel manuell erneut generieren müssen, sollten Sie sie über Key Vault erneut generieren.

## <a name="getting-started"></a>Erste Schritte

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Setup für rollenbasierte Zugriffssteuerungsberechtigungen (RBAC)

Die Azure Key Vault-Anwendungsidentität benötigt die Berechtigungen zum *Auflisten* und *Regenerieren* von Schlüsseln für ein Speicherkonto. Richten Sie diese Berechtigungen mithilfe der folgenden Schritte ein:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure Key Vault Identity
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Beispiel

Im folgenden Beispiel wird die Erstellung eines durch Key Vault verwalteten Azure-Speicherkontos und der zugehörigen SAS-Definitionen (Shared Access Signature) veranschaulicht.

### <a name="prerequisite"></a>Voraussetzung

Stellen Sie sicher, dass Sie das [Setup für rollenbasierte Zugriffssteuerungsberechtigungen (RBAC)](#setup-for-role-based-access-control-rbac-permissions) durchgeführt haben.

### <a name="setup"></a>Einrichtung

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Legen Sie als Nächstes die Berechtigungen für **Ihr Konto** fest, um sicherzustellen, dass Sie alle Speicherberechtigungen in Key Vault verwalten können. Im folgenden Beispiel lautet das Azure-Konto _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Erstellen eines per Key Vault verwalteten Speicherkontos

Erstellen Sie jetzt ein verwaltetes Speicherkonto in Azure Key Vault, und verwenden Sie einen Zugriffsschlüssel aus Ihrem Speicherkonto, um die SAS-Token zu erstellen.
- `-ActiveKeyName` verwendet „key2“ zum Generieren der SAS-Token.
- `-AccountName` wird verwendet, um Ihr verwaltetes Speicherkonto zu identifizieren. Hier speichern wir der Einfachheit halber den Namen des Speicherkontos, aber es kann jeder beliebige Name sein.
- Mit `-DisableAutoRegenerateKey` wird angegeben, dass die Speicherkontoschlüssel nicht generiert werden.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Erneute Schlüsselgenerierung

Wenn Sie möchten, dass Key Vault die Zugriffsschlüssel Ihres Speichers regelmäßig neu generiert, können Sie einen Zeitraum für die erneute Generierung festlegen. Unten legen wir einen Zeitraum von drei Tagen für die erneute Generierung fest. Nach drei Tagen generiert Key Vault „key1“ neu und wechselt für den aktiven Schlüssel von „key2“ zu „key1“.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Festlegen von SAS-Definitionen

Die Konto-SAS gewährt Zugriff auf den Blobdienst mit verschiedenen Berechtigungen.
Legen Sie für Ihr verwaltetes Speicherkonto die SAS-Definitionen in Key Vault fest.
- `-AccountName` ist der Name des verwalteten Speicherkontos in Key Vault.
- `-Name` ist der Bezeichner für das SAS-Token in Ihrem Speicher.
- `-ValidityPeriod` legt das Ablaufdatum für das generierte SAS-Token fest.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>Abrufen von SAS-Token

Rufen Sie die entsprechenden SAS-Token ab, und führen Sie Aufrufe an den Speicher aus. `-SecretName` wird aus der Eingabe der Parameter `AccountName` und `Name` erstellt, wenn Sie [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition) ausgeführt haben.

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Erstellen des Speichers

Beachten Sie, dass der Zugriff mit *$readSasToken* nicht erfolgreich ist, aber dass *$writeSasToken* verwendet werden kann.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt" -Context $context2
```

Sie können auf den Speicherblobinhalt mit dem SAS-Token mit Schreibzugriff zugreifen.

### <a name="relevant-powershell-cmdlets"></a>Relevante PowerShell-Cmdlets

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Onboarding für Speicherkonto

Beispiel: Als Key Vault-Objektbesitzer fügen Sie Azure Key Vault zur Integration eines Speicherkontos ein Speicherkontoobjekt hinzu.

Während des Onboardings muss Key Vault sicherstellen, dass die Identität des integrierten Kontos über die Speicherschlüssel *list* und *regenerate* verfügt. Zur Überprüfung dieser Berechtigungen ruft Key Vault ein OBO-Token (On Behalf Of, im Auftrag von) vom Authentifizierungsdienst ab, legt die Zielgruppe auf Azure Resource Manager fest und führt einen Aufruf des *list*-Schlüssels an den Azure Storage-Dienst durch. Wenn beim *list*-Aufruf ein Fehler auftritt, tritt bei der Key Vault-Objekterstellung ein Fehler mit dem HTTP-Statuscode *Forbidden* auf. Die auf diese Weise aufgelisteten Schlüssel werden mit Ihrer Schlüsseltresorentitäts-Speicherung zwischengespeichert.

Key Vault muss sicherstellen, dass die Identität über *regenerate*-Berechtigungen verfügt, bevor Key Vault den Besitz der erneuten Generierung Ihrer Schlüssel übernehmen kann. So wird überprüft, ob sowohl die Identität, über das OBO-Token, als auch die Key Vault-Erstanbieteridentität über diese Berechtigungen verfügt:

- Key Vault listet RBAC-Berechtigungen für die Speicherkontoressource auf.
- Key Vault überprüft die Antwort über den Abgleich von Aktionen und Nichtaktionen mit regulären Ausdrücken.

Einige unterstützende Beispiele finden Sie unter [Key Vault – Beispiele für verwaltete Speicherkontoschlüssel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Wenn die Identität nicht über die *regenerate*-Berechtigung oder die Key Vault-Erstanbieteridentität nicht über die Berechtigung *list* oder *regenerate* verfügt, tritt bei der Onboardinganforderung ein Fehler auf, und ein entsprechender Fehlercode und eine Nachricht werden zurückgegeben.

Das OBO-Token funktioniert nur bei Verwendung von nativen Erstanbieter-Clientanwendungen von PowerShell oder CLI.

## <a name="other-applications"></a>Andere Anwendungen

- Mit Key Vault-Speicherkontoschlüsseln erstellte SAS-Token bieten sogar noch stärker kontrollierten Zugriff auf ein Azure-Speicherkonto. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Weitere Informationen

- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-Teamblog](https://blogs.technet.microsoft.com/kv/)

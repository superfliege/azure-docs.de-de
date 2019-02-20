---
title: 'PowerShell und Befehlszeilenschnittstelle: Aktivieren von TDE für SQL – mit Azure Key Vault – BYOK (Bring Your Own Key) – Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Erfahren Sie, wie eine Azure SQL-Datenbank- und Data Warehouse-Instanz mithilfe von PowerShell oder CLI für die Verwendung von Transparent Data Encryption (TDE) zur Verschlüsselung ruhender Daten konfiguriert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: f1cb99799e3aa5c0b37643112f8644d1aabfd666
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56108091"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell und Befehlszeilenschnittstelle: Aktivieren von Transparent Data Encryption mithilfe eines vom Kunden verwalteten Azure Key Vault-Schlüssels

In diesem Artikel wird die Verwendung eines Schlüssels aus Azure Key Vault für Transparent Data Encryption (TDE) auf einer SQL-Datenbank- oder Data Warehouse-Instanz erläutert. Weitere Informationen zur Integration von TDE mit Azure Key Vault und BYOK-Unterstützung (Bring Your Own Key) finden Sie unter [TDE mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Voraussetzungen für PowerShell

- Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
- [Empfohlen, aber optional] Sie sollten über ein Hardwaresicherheitsmodul (HSM) oder einen lokalen Schlüsselspeicher zum Erstellen einer lokalen Kopie des Schlüsselmaterials der TDE-Schutzvorrichtung verfügen.
- Azure PowerShell Version 4.2.0.x oder höher muss installiert sein und ausgeführt werden. 
- Erstellen Sie eine Azure Key Vault-Instanz und einen Schlüssel zur Verwendung für TDE.
   - [PowerShell-Anweisungen aus Key Vault](../key-vault/key-vault-overview.md)
   - [Anweisungen zur Verwendung eines Hardwaresicherheitsmodells (HSM) und Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
 - Der Schlüsseltresor muss die folgende Eigenschaft aufweisen, um für TDE verwendet werden zu können:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
   - [Verwenden des vorläufigen Löschens in Key Vault mit PowerShell](../key-vault/key-vault-soft-delete-powershell.md) 
- Der Schlüssel muss die folgenden Attribute aufweisen, um für TDE verwendet werden zu können:
   - Kein Ablaufdatum
   - Nicht deaktiviert
   - Fähigkeit zum Ausführen des *get*-, *wrap key*- und *unwrap key*-Vorgangs

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Schritt 1: Zuweisen einer Azure AD-Identität zu einem Server 

Wenn bereits ein Server vorhanden ist, fügen Sie ihm mit dem folgenden Code eine Azure AD-Identität hinzu:

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Wenn Sie einen Server erstellen, verwenden Sie das Cmdlet [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) mit dem Tag „-Identity“, um während der Servererstellung eine Azure AD-Identität hinzuzufügen:

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Schritt 2: Gewähren von Key Vault-Berechtigungen für Ihren Server

Verwenden Sie das Cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy), um Ihrem Server Zugriff auf den Schlüsseltresor zu gewähren, bevor Sie einen Schlüssel darin für TDE verwenden.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Schritt 3: Hinzufügen eines Key Vault-Schlüssels zum Server und Festlegen der TDE-Schutzvorrichtung

- Fügen Sie dem Server mit dem Cmdlet [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) den Schlüssel aus Key Vault hinzu.
- Legen Sie den Schlüssel mit dem Cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) als TDE-Schutzvorrichtung für alle Serverressourcen fest.
- Vergewissern Sie sich mit dem Cmdlet [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector), dass die TDE-Schutzvorrichtung wie vorgesehen konfiguriert wurde.

> [!Note]
> Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.
> 

>[!Tip]
>Beispiel für die KeyId-Eigenschaft in Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Schritt 4: Aktivieren von TDE 

Aktivieren Sie TDE mit dem Cmdlet [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption).

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Nun ist TDE für die Datenbank oder Data Warehouse-Instanz mit einem Verschlüsselungsschlüssel in Key Vault aktiviert.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Schritt 5: Überprüfen des Verschlüsselungsstatus und der Verschlüsselungsaktivität

Rufen Sie den Verschlüsselungsstatus mit dem Cmdlet [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) ab, und überprüfen Sie den Verschlüsselungsfortschritt für eine Datenbank oder Data Warehouse-Instanz mit dem Cmdlet [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity).

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Weitere nützliche PowerShell-Cmdlets

- Mit dem Cmdlet [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) deaktivieren Sie TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Mit dem Cmdlet [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) geben Sie die Liste der Key Vault-Schlüssel zurück, die dem Server hinzugefügt wurden.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Mit dem Cmdlet [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) entfernen Sie einen Key Vault-Schlüssel vom Server.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie Folgendes, wenn ein Problem auftritt:
- Wenn der Schlüsseltresor nicht gefunden werden kann, stellen Sie mithilfe des Cmdlets [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) sicher, dass Sie sich im richtigen Abonnement befinden.

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Wenn der neue Schlüssel dem Server nicht hinzugefügt oder nicht als TDE-Schutzvorrichtung aktualisiert werden kann, überprüfen Sie Folgendes:
   - Der Schlüssel darf kein Ablaufdatum haben.
   - Für den Schlüssel müssen die *get*-, *wrap key*- und *unwrap key*-Vorgänge aktiviert sein.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die TDE-Schutzvorrichtung eines Servers rotieren, um Sicherheitsanforderungen zu erfüllen: [Rotieren der Transparent Data Encryption-Schutzvorrichtung mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Im Fall eines Sicherheitsrisikos können Sie hier nachsehen, wie Sie eine möglicherweise kompromittierte TDE-Schutzvorrichtung entfernen: [Entfernen eines möglicherweise kompromittierten Schlüssels](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Voraussetzungen für CLI

- Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
- [Empfohlen, aber optional] Sie sollten über ein Hardwaresicherheitsmodul (HSM) oder einen lokalen Schlüsselspeicher zum Erstellen einer lokalen Kopie des Schlüsselmaterials der TDE-Schutzvorrichtung verfügen.
- Befehlszeilenschnittstellen-Version 2.0 oder höher. Informationen zum Installieren der aktuellen Version und Verbindungsaufbau mit Ihrem Azure-Abonnement, finden Sie unter [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Erstellen Sie eine Azure Key Vault-Instanz und einen Schlüssel zur Verwendung für TDE.
   - [Verwalten von Key Vault mit CLI 2.0](../key-vault/key-vault-manage-with-cli2.md)
   - [Anweisungen zur Verwendung eines Hardwaresicherheitsmodells (HSM) und Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
 - Der Schlüsseltresor muss die folgende Eigenschaft aufweisen, um für TDE verwendet werden zu können:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
   - [Verwenden des vorläufigen Löschens in Key Vault mit der CLI](../key-vault/key-vault-soft-delete-cli.md) 
- Der Schlüssel muss die folgenden Attribute aufweisen, um für TDE verwendet werden zu können:
   - Kein Ablaufdatum
   - Nicht deaktiviert
   - Fähigkeit zum Ausführen des *get*-, *wrap key*- und *unwrap key*-Vorgangs
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>Schritt 1: Erstellen eines Servers und Zuweisen einer Azure AD-Identität zum Server
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -i 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Schritt 2: Gewähren von Key Vault-Berechtigungen für Ihren Server
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Schritt 3: Hinzufügen eines Key Vault-Schlüssels zum Server und Festlegen der TDE-Schutzvorrichtung
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.
> 

>[!Tip]
>Beispiel für die KeyId-Eigenschaft in Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>Schritt 4: Aktivieren von TDE 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

Nun ist TDE für die Datenbank oder Data Warehouse-Instanz mit einem Verschlüsselungsschlüssel in Key Vault aktiviert.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Schritt 5: Überprüfen des Verschlüsselungsstatus und der Verschlüsselungsaktivität

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

## <a name="sql-cli-references"></a>SQL-CLI-Referenz

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 


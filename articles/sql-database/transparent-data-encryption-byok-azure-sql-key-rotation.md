---
title: 'PowerShell: Rotieren einer TDE-Schutzvorrichtung – Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Erfahren Sie, wie die Transparent Data Encryption-Schutzvorrichtung (TDE) für einen Azure SQL-Server rotiert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: becczhang
ms.author: ryzhang26
ms.reviewer: vanto
manager: jhubbard
ms.date: 08/07/2017
ms.openlocfilehash: 365cb020bb6b2786a89a98e008d1b62c23c1c0e7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166587"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>Rotieren der Transparent Data Encryption (TDE)-Schutzvorrichtung mithilfe von PowerShell 

Dieser Artikel beschreibt die Schlüsselrotation für einen Azure SQL-Server mithilfe einer TDE-Schutzvorrichtung aus Azure Key Vault. Das Rotieren der TDE-Schutzvorrichtung eines Azure SQL-Servers bedeutet, dass zu einem neuen asymmetrischen Schlüssel gewechselt wird, der die Datenbanken auf dem Server schützt. Die Schlüsselrotation ist ein Onlinevorgang, der in wenigen Sekunden abgeschlossen sein sollte, da nur der Datenverschlüsselungsschlüssel der Datenbank entschlüsselt und wieder verschlüsselt wird, nicht die gesamte Datenbank.

Dieser Leitfaden erläutert zwei Optionen zum Rotieren der TDE-Schutzvorrichtung auf dem Server.

> [!NOTE]
> Ein angehaltener SQL Data Warehouse-Dienst muss vor den Schlüsselrotationen fortgesetzt werden.
>

> [!IMPORTANT]
> **Löschen Sie keine** vorherigen Versionen des Schlüssels nach einem Rollover.  Bei einem Rollover von Schlüsseln sind einige Daten noch immer mit den vorherigen Schlüsseln verschlüsselt, z.B. ältere Datenbanksicherungen. 
>

## <a name="prerequisites"></a>Voraussetzungen

- In diesem Leitfaden wird davon ausgegangen, dass Sie bereits einen Schlüssel aus Azure Key Vault als TDE-Schutzvorrichtung für eine Azure SQL-Datenbank- oder Data Warehouse-Instanz verwenden. Siehe [Transparent Data Encryption mit BYOK-Unterstützung](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell Version 3.7.0.x oder höher muss installiert sein und ausgeführt werden. 
- [Empfohlen, aber optional] Erstellen Sie das Schlüsselmaterial für die TDE-Schutzvorrichtung zunächst in einem Hardwaresicherheitsmodul (HSM) oder einem lokalen Schlüsselspeicher, und importieren Sie das Schlüsselmaterial in Azure Key Vault. Weitere Informationen erhalten Sie, wenn Sie die [Anweisungen zur Verwendung eines Hardwaresicherheitsmodells (HSM) und Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) befolgen.

## <a name="option-1-auto-rotation"></a>Option 1: Automatische Rotation

Generieren Sie eine neue Version des vorhandenen Schlüssels der TDE-Schutzvorrichtung in Key Vault und zwar unter demselben Schlüsselnamen und im selben Schlüsseltresor. Der Azure SQL-Dienst verwendet die neue Version innerhalb von 24 Stunden. 

So erstellen Sie eine neue Version der TDE-Schutzvorrichtung mithilfe des Cmdlets [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey):

   ```powershell
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>
   ```

## <a name="option-2-manual-rotation"></a>Option 2: Manuelle Rotation

Bei dieser Option wird mithilfe der Cmdlets [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) und [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) ein komplett neuer Schlüssel hinzugefügt, der unter einem neuen Schlüsselnamen oder sogar in einem anderen Schlüsseltresor gespeichert werden kann. 

>[!NOTE]
>Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Weitere nützliche PowerShell-Cmdlets

- Zum Ändern der TDE-Schutzvorrichtung vom durch Microsoft verwalteten Modus in den BYOK-Modus verwenden Sie das Cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Zum Ändern der TDE-Schutzvorrichtung vom BYOK-Modus zum durch Microsoft verwalteten Modus verwenden Sie das Cmdlet [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Nächste Schritte

- Im Falle eines Sicherheitsrisikos, erfahren Sie unter [Entfernen eines möglicherweise kompromittierten Schlüssels](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) wie Sie eine möglicherweise kompromittierten TDE-Schutzvorrichtung entfernen können. 

- Erste Schritte mit der Bring Your Own Key-Unterstützung für TDE: [Aktivieren von TDE für das Verwenden Ihres eigenen Schlüssels aus Key Vault mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)

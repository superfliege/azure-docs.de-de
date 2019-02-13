---
title: 'PowerShell: Entfernen einer TDE-Schutzvorrichtung – Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Anleitung zum Reagieren auf eine möglicherweise kompromittierte TDE-Schutzvorrichtung für eine Azure SQL-Datenbank- oder Data Warehouse-Instanz, die TDE mit Bring Your Own Key (BYOK)-Unterstützung verwendet
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
ms.date: 10/12/2018
ms.openlocfilehash: 8ffda7fd1b987e34dc0e8157b535ccef65571247
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567892"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Entfernen einer Transparent Data Encryption (TDE)-Schutzvorrichtung mithilfe von PowerShell

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
- Azure PowerShell Version 4.2.0.x oder höher muss installiert sein und ausgeführt werden. 
- In diesem Leitfaden wird davon ausgegangen, dass Sie bereits einen Schlüssel aus Azure Key Vault als TDE-Schutzvorrichtung für eine Azure SQL-Datenbank- oder Data Warehouse-Instanz verwenden. Weitere Informationen finden Sie unter [Transparent Data Encryption mit BYOK-Unterstützung](transparent-data-encryption-byok-azure-sql.md).

## <a name="overview"></a>Übersicht

In dieser Anleitung wird beschrieben, wie auf eine möglicherweise kompromittierte TDE-Schutzvorrichtung für eine Azure SQL-Datenbank- oder Data Warehouse-Instanz zu reagieren ist, die TDE mit Bring Your Own Key (BYOK)-Unterstützung verwendet. Weitere Informationen zu BYOK-Unterstützung für TDE finden Sie auf der [Übersichtsseite](transparent-data-encryption-byok-azure-sql.md). 

Die folgenden Verfahren sollten nur in Ausnahmefällen oder in Testumgebungen durchgeführt werden. Lesen Sie den Leitfaden sorgfältig, da das Löschen von aktiv verwendeten TDE-Schutzvorrichtungen aus Azure Key Vault zu **Datenverlust** führen kann. 

Wenn der Verdacht besteht, dass ein Schlüssel kompromittiert ist, d.h ein Dienst oder Benutzer unautorisierten Zugriff auf den Schlüssel hat, empfiehlt es sich, den Schlüssel zu löschen.

Denken Sie daran: Sobald die TDE-Schutzvorrichtung aus Key Vault gelöscht ist, **werden alle Verbindungen mit den verschlüsselten Datenbanken unter dem Server blockiert, und diese Datenbanken werden offline geschaltet und innerhalb von 24 Stunden gelöscht**. Auf alte Sicherungen, die mit dem kompromittierten Schlüssel verschlüsselt wurden, kann nicht mehr zugegriffen werden.

In diesem Leitfaden werden zwei Ansätze behandelt, die jeweils vom gewünschten Ergebnis nach der Reaktion auf Vorfälle abhängen:

- Auf Azure SQL-Datenbank- bzw. Data Warehouse-Instanzen soll weiterhin **Zugriff** möglich sein
- Auf Azure SQL-Datenbank- bzw. Data Warehouse-Instanzen soll **kein Zugriff** mehr möglich sein

## <a name="to-keep-the-encrypted-resources-accessible"></a>Zugriff auf die verschlüsselten Ressourcen weiterhin ermöglichen

1. Erstellen Sie einen [neuen Schlüssel in Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Stellen Sie sicher, dass der neue Schlüssel in einem anderen Schlüsseltresor als der möglicherweise kompromittierten TDE-Schutzvorrichtung erstellt wird, da die Zugriffskontrolle auf Tresorebene bereitgestellt wird. 
2. Fügen Sie den neuen Schlüssel mithilfe der Cmdlets [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) und [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) dem Server hinzu, und aktualisieren Sie ihn als neue TDE-Schutzvorrichtung des Servers.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   # Set the key as the TDE protector for all resources under the server
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Stellen Sie mithilfe des Cmdlets [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) sicher, dass der Server und alle Replikate auf die neue TDE-Schutzvorrichtung aktualisiert wurden. 

   >[!NOTE]
   > Es kann einige Minuten dauern, bis sich die neue TDE-Schutzvorrichtung auf alle Datenbanken und sekundären Datenbanken auf dem Server verteilt.

   ```powershell
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Führen Sie eine [Sicherung des neuen Schlüssels](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey) in Key Vault durch.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```

5. Löschen Sie den kompromittierten Schlüssel mithilfe des Cmdlets [Remove-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/remove-azurekeyvaultkey) aus Key Vault. 

   ```powershell
   Remove-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```

6. Zum künftigen Wiederherstellen eines Schlüssels in Key Vault verwenden Sie das Cmdlet [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey):
   ```powershell
   Restore-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```

## <a name="to-make-the-encrypted-resources-inaccessible"></a>Zugriff auf die verschlüsselten Ressourcen verweigern

1. Löschen Sie die Datenbanken, die durch den möglicherweise kompromittierten Schlüssel verschlüsselt sind.

   Die Datenbank- und Protokolldateien werden automatisch gesichert, sodass eine Point-in-Time-Wiederherstellung der Datenbank jederzeit durchgeführt werden kann (solange Sie den Schlüssel bereitstellen). Die Datenbanken müssen gelöscht werden, bevor eine aktive TDE-Schutzvorrichtung gelöscht wird, um einen möglichen Datenverlust von bis zu zehn Minuten der letzten Transaktionen zu verhindern. 
2. Sichern Sie das Schlüsselmaterial der TDE-Schutzvorrichtung in Key Vault.
3. Entfernen Sie den möglicherweise kompromittierten Schlüssel aus Key Vault.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die TDE-Schutzvorrichtung eines Servers rotieren, um Sicherheitsanforderungen zu erfüllen: [Rotieren der Transparent Data Encryption-Schutzvorrichtung mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- Erste Schritte mit der BYOK-Unterstützung (Bring Your Own Key) für TDE: [Aktivieren von TDE mit BYOK aus Key Vault mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)

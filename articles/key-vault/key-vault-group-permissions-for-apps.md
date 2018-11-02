---
title: Gewähren der Berechtigung zum Zugreifen auf einen Azure Key Vault für viele Anwendungen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie vielen Anwendungen die Berechtigung zum Zugreifen auf einen Schlüsseltresor gewähren.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: ambapat
ms.openlocfilehash: 4ad6a18f9937fcc7d24bebc3ac197e23990ff59e
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309245"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Gewähren des Zugriffs auf einen Schlüsseltresor für mehrere Anwendungen

Mithilfe der Zugriffssteuerungsrichtlinie kann mehreren Anwendungen der Zugriff auf einen Schlüsseltresor gewährt werden. Eine Zugriffssteuerungsrichtlinie kann bis zu 1024 Anwendungen unterstützen und wird wie folgt konfiguriert:

1. Erstellen Sie eine Azure Active Directory-Sicherheitsgruppe. 
2. Fügen Sie alle zugehörigen Dienstprinzipale der Anwendungen zur Sicherheitsgruppe hinzu.
3. Gewähren Sie der Sicherheitsgruppe Zugriff auf Ihre Key Vault-Instanz.

Hier sind die erforderlichen Komponenten angegeben:
* [Installieren Sie das Azure Active Directory V2-PowerShell-Modul](https://www.powershellgallery.com/packages/AzureAD).
* [Installieren Sie Azure PowerShell](/powershell/azure/overview).
* Zum Ausführen der folgenden Befehle benötigen Sie Berechtigungen zum Erstellen/Bearbeiten von Gruppen im Azure Active Directory-Mandanten. Falls Sie nicht über die entsprechenden Berechtigungen verfügen, müssen Sie sich ggf. an Ihren Azure Active Directory-Administrator wenden. Unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md) finden Sie nähere Informationen zur Richtlinie für Berechtigungen zum Zugriff auf Schlüsseltresore.

Führen Sie als Nächstes die folgenden Befehle in PowerShell aus:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Wenn Sie einer Gruppe von Anwendungen einen anderen Berechtigungssatz gewähren müssen, können Sie für diese Anwendungen eine separate Azure Active Directory-Sicherheitsgruppe erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Schützen einer Key Vault-Instanz](key-vault-secure-your-key-vault.md).

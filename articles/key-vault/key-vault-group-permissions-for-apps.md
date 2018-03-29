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
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: ddeaf184138bd48d324799ddb45248b0a0ee8eeb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Gewähren der Berechtigung zum Zugreifen auf einen Schlüsseltresor für viele Anwendungen

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>F: Ich verwende mehrere Anwendungen (mehr als 16), für die der Zugriff auf einen Schlüsseltresor ermöglicht werden muss. Wie kann ich dies erreichen, obwohl für Key Vault nur 16 Einträge für die Zugriffssteuerung zulässig sind?

Die Richtlinie für die Key Vault-Zugriffssteuerung unterstützt nur 16 Einträge. Sie können aber eine Azure Active Directory-Sicherheitsgruppe erstellen. Fügen Sie dieser Sicherheitsgruppe alle zugeordneten Dienstprinzipale hinzu, und gewähren Sie dieser Sicherheitsgruppe dann Zugriff auf Key Vault.

Hier sind die erforderlichen Komponenten angegeben:
* [Installieren Sie das Azure Active Directory V2-PowerShell-Modul](https://www.powershellgallery.com/packages/AzureAD).
* [Installieren Sie Azure PowerShell](/powershell/azure/overview).
* Zum Ausführen der folgenden Befehle benötigen Sie Berechtigungen zum Erstellen/Bearbeiten von Gruppen im Azure Active Directory-Mandanten. Falls Sie nicht über die entsprechenden Berechtigungen verfügen, müssen Sie sich ggf. an Ihren Azure Active Directory-Administrator wenden.

Führen Sie als Nächstes die folgenden Befehle in PowerShell aus.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionsToKeys all –PermissionsToSecrets all –PermissionsToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Wenn Sie einer Gruppe von Anwendungen einen anderen Berechtigungssatz gewähren müssen, können Sie für diese Anwendungen eine separate Azure Active Directory-Sicherheitsgruppe erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Schützen einer Key Vault-Instanz](key-vault-secure-your-key-vault.md).

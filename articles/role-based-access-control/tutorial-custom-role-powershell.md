---
title: 'Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell | Microsoft-Dokumentation'
description: Erste Schritte beim Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/12/2018
ms.author: rolyon
ms.openlocfilehash: f49f6f03b6d9f1c51cada58ae782bbc364fc9d66
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427286"
---
# <a name="tutorial-create-a-custom-role-using-azure-powershell"></a>Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell

Wenn die [integrierten Rollen](built-in-roles.md) den Anforderungen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen benutzerdefinierten Rollen erstellen. In diesem Tutorial erstellen Sie mithilfe von Azure PowerShell eine benutzerdefinierte Rolle namens „Reader Support Tickets“. Die benutzerdefinierte Rolle ermöglicht dem Benutzer, alle Inhalte des Abonnement anzuzeigen und auch Supporttickets zu erstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer benutzerdefinierten Rolle
> * Auflisten benutzerdefinierter Rollen 
> * Aktualisieren einer benutzerdefinierten Rolle
> * Löschen einer benutzerdefinierten Rolle

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Lernprogramm abzuschließen, benötigen Sie:

- Berechtigungen zum Erstellen von benutzerdefinierten Rollen, etwa [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator)
- [Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) (lokal installiert)

## <a name="sign-in-to-azure-powershell"></a>Anmelden bei Azure PowerShell

Melden Sie sich bei [Azure PowerShell](/powershell/azure/authenticate-azureps) an.

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Zum Erstellen einer benutzerdefinierten Rolle beginnen Sie am besten mit einer integrierten Rolle, bearbeiten sie und erstellen dann eine neue Rolle.

1. Verwenden Sie in PowerShell den Befehl [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation), um die Liste der Vorgänge für den Ressourcenanbieter „Microsoft.Support“ abzurufen. Es ist hilfreich, die Vorgänge zu kennen, die zum Erstellen der Berechtigungen verfügbar sind. Eine Liste der verfügbaren Vorgänge finden Sie auch unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md#microsoftsupport).

    ```azurepowershell
    Get-AzureRMProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize
    ```
    
    ```Output
    Operation                              Description
    ---------                              -----------
    Microsoft.Support/register/action      Registers to Support Resource Provider
    Microsoft.Support/supportTickets/read  Gets Support Ticket details (including status, severity, contact ...
    Microsoft.Support/supportTickets/write Creates or Updates a Support Ticket. You can create a Support Tic...
    ```

1. Verwenden Sie den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um die Rolle [Leser](built-in-roles.md#reader) im JSON-Format auszugeben.

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json
    ```

1. Öffnen Sie die Datei **ReaderSupportRole.json** in einem Editor.

    Die JSON-Ausgabe sieht wie folgt aus. Informationen zu den verschiedenen Eigenschaften finden Sie unter [Erstellen von benutzerdefinierten Rollen in Azure](custom-roles.md).

    ```json
    {
        "Name":  "Reader",
        "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "IsCustom":  false,
        "Description":  "Lets you view everything, but not make any changes.",
        "Actions":  [
                        "*/read"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/"
                             ]
    }
    ```
    
1. Bearbeiten Sie die JSON-Datei, um den Vorgang `"Microsoft.Support/*"` zur Eigenschaft `Actions` hinzuzufügen. Achten Sie darauf, ein Komma nach dem Lesevorgang einzufügen. Diese Aktion ermöglicht dem Benutzer das Erstellen von Supporttickets.

1. Rufen Sie die ID Ihres Abonnements mithilfe des Befehls [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) ab.

    ```azurepowershell
    Get-AzureRmSubscription
    ```

1. Fügen Sie in `AssignableScopes` Ihre Abonnement-ID im folgenden Format ein: `"/subscriptions/00000000-0000-0000-0000-000000000000"`.

    Sie müssen explizite Abonnement-IDs hinzufügen. Andernfalls können Sie die Rolle nicht in Ihr Abonnement importieren.

1. Löschen Sie die Zeile mit der Eigenschaft `Id`, und ändern Sie die Eigenschaft `IsCustom` in `true`.

1. Ändern Sie die Eigenschaften `Name` und `Description` in „Reader Support Tickets“ und „View everything in the subscription and also open support tickets“.

    Ihre JSON-Datei sollte folgendermaßen aussehen:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
    
1. Zum Erstellen der neuen benutzerdefinierten Rolle verwenden Sie den Befehl [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) und geben die JSON-Rollendefinitionsdatei an.

    ```azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
        
    Die neue benutzerdefinierte Rolle ist jetzt im Azure-Portal verfügbar und kann wie integrierte Rollen Benutzern, Gruppen oder Dienstprinzipalen zugewiesen werden.

## <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 

- Verwenden Sie zum Auflisten aller benutzerdefinierten Rollen den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

    ```azurepowershell
    Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
    ```

    ```Output
    Name                   IsCustom
    ----                   --------
    Reader Support Tickets     True
    ```
    
    Sie können die benutzerdefinierte Rolle auch im Azure-Portal anzeigen.

    ![Screenshot der in das Azure-Portal importierten benutzerdefinierten Rolle](./media/tutorial-custom-role-powershell/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Um die benutzerdefinierte Rolle zu aktualisieren, können Sie die JSON-Datei aktualisieren oder das Objekt `PSRoleDefinition` verwenden.

1. Verwenden Sie zum Aktualisieren der JSON-Datei den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um die benutzerdefinierte Rolle im JSON-Format auszugeben.

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader Support Tickets" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole2.json
    ```

1. Öffnen Sie die Datei in einem Editor.

1. Fügen Sie in `Actions` den Vorgang zum Erstellen und Verwalten von Ressourcengruppenbereitstellungen (`"Microsoft.Resources/deployments/*"`) hinzu.

    Ihre aktualisierte JSON-Datei sollte wie folgt aussehen:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "Id":  "22222222-2222-2222-2222-222222222222",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*",
                        "Microsoft.Resources/deployments/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
        
1. Führen Sie zum Aktualisieren der benutzerdefinierten Rolle den Befehl [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) aus, und geben Sie die aktualisierte JSON-Datei an.

    ```azurepowershell
    Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole2.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```

1. Wenn Sie zum Aktualisieren der benutzerdefinierten Rolle das Objekt `PSRoleDefintion` verwenden möchten, führen Sie zuerst den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) aus, um die Rolle abzurufen.

    ```azurepowershell
    $role = Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```
    
1. Rufen Sie die Methode `Add` auf, um den Vorgang zum Lesen der Diagnoseeinstellungen hinzuzufügen.

    ```azurepowershell
    $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*/read")
    ```

1. Aktualisieren Sie die Rolle mithilfe von [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition).

    ```azurepowershell
    Set-AzureRmRoleDefinition -Role $role
    ```
    
    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*,
                       Microsoft.Insights/diagnosticSettings/*/read}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
    
## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

1. Verwenden Sie zum Abrufen der ID der benutzerdefinierten Rolle den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

    ```azurepowershell
    Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```

1. Verwenden Sie den Befehl [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition), und geben Sie die Rollen-ID an, um die benutzerdefinierte Rolle zu löschen.

    ```azurepowershell
    Remove-AzureRmRoleDefinition -Id "22222222-2222-2222-2222-222222222222"
    ```

    ```Output
    Confirm
    Are you sure you want to remove role definition with id '22222222-2222-2222-2222-222222222222'.
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```

1. Wenn Sie zum Bestätigen aufgefordert werden, geben Sie **Y** ein.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen benutzerdefinierter Rollen mithilfe von PowerShell](custom-roles-powershell.md)

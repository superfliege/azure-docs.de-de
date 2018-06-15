---
title: Verwalten der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) mit Azure PowerShell | Microsoft-Dokumentation
description: So verwalten Sie RBAC mit Azure PowerShell einschließlich der Auflistung von Rollen, der Zuweisung von Rollen und dem Löschen von Rollenzuweisungen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 00646187da1f93c01c3a57b50905239afd5e2bc8
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266797"
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](role-assignments-powershell.md)
> * [Azure-CLI](role-assignments-cli.md)
> * [REST-API](role-assignments-rest.md)

Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) definieren Sie den Zugriff für Benutzer, Gruppen und Dienstprinzipale durch Zuweisen von Rollen in einem bestimmten Bereich. In diesem Artikel wird die Verwaltung des Zugriffs mithilfe von Azure PowerShell beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie PowerShell verwenden können, um die RBAC zu verwalten, benötigen Sie ein der folgenden Komponenten:

* [PowerShell in Azure Cloud Shell](/azure/cloud-shell/overview)
* [Azure PowerShell 5.1.0 oder höher](/powershell/azure/install-azurerm-ps)

## <a name="list-roles"></a>Auflisten der Rollen

### <a name="list-all-available-roles"></a>Auflisten aller verfügbaren Rollen

Zum Auflisten von verfügbaren RBAC-Rollen für die Zuweisung und Überprüfung der Vorgänge, auf die sie Zugriff gewähren, verwenden Sie [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-specific-role"></a>Auflisten einer bestimmten Rolle

Verwenden Sie zum Auflisten einer bestimmten Rolle [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name>
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
AssignableScopes : {/}
```

### <a name="list-a-specific-role-in-json-format"></a>Auflisten einer bestimmten Rolle im JSON-Format

Verwenden Sie zum Auflisten einer bestimmten Rolle im JSON-Format [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | ConvertTo-Json

{
    "Name":  "Contributor",
    "Id":  "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "IsCustom":  false,
    "Description":  "Lets you manage everything except access to resources.",
    "Actions":  [
                    "*"
                ],
    "NotActions":  [
                       "Microsoft.Authorization/*/Delete",
                       "Microsoft.Authorization/*/Write",
                       "Microsoft.Authorization/elevateAccess/Action"
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

### <a name="list-actions-of-a-role"></a>Auflisten der Aktionen einer Rolle

Verwenden Sie zum Auflisten der Aktionen für eine bestimmte Rolle [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action}
```

```azurepowershell
(Get-AzureRmRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="see-who-has-access"></a>Sehen, wer Zugriff hat

Verwenden Sie zum Auflisten von RBAC-Zugriffszuweisungen [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

### <a name="list-role-assignments-at-a-specific-scope"></a>Auflistung der Rollenzuweisungen für einen bestimmten Bereich

Sie können alle Rollenzuweisungen für ein angegebenes Abonnement, eine angegebene Ressourcengruppe oder eine Ressource sehen. Um beispielsweise alle aktiven Zuweisungen für eine Ressourcengruppe zu sehen, verwenden Sie [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -ResourceGroupName pharma-sales-projectforecast | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Alain Charon
RoleDefinitionName : Backup Operator
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Alain Charon
RoleDefinitionName : Virtual Machine Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

### <a name="list-roles-assigned-to-a-user"></a>Zugewiesenen Rollen eines Benutzers auflisten

Verwenden Sie zum Auflisten aller Rollen, die einem bestimmten Benutzer zugewiesen sind, [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

Um alle Rollen, die einem bestimmten Benutzer zugewiesen sind, und die Rollen aufzulisten, die den Gruppen des Benutzers zugewiesen sind, verwenden Sie [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email> -ExpandPrincipalGroups
```

```Example
Get-AzureRmRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Auflisten der klassischen Rollenzuweisungen für die Dienstadministrator- und Co-Administrator-Rolle

Zum Auflisten der Zugriffszuweisungen für die klassischen Abonnementadministratoren und -Co-Administratoren verwenden Sie [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment):

```azurepowershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>Gewähren von Zugriff

### <a name="search-for-object-ids"></a>Suchen nach Objekt-IDs

Um eine Rolle zuzuweisen, müssen Sie sowohl das Objekt (Benutzer, Gruppe oder Anwendung) als auch den Bereich identifizieren.

Falls Sie die Abonnement-ID nicht kennen, können Sie diese im Blatt **Abonnements** im Azure-Portal finden, oder Sie verwenden [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

So rufen Sie die Objekt-ID für eine Azure AD-Gruppe mit [Get-AzureRmADGroup](/powershell/module/azurerm.resources/get-azurermadgroup) ab:

```azurepowershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

So rufen Sie die Objekt-ID für einen Azure AD-Dienstprinzipal oder eine Azure AD-Anwendung mit [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal) ab:

```azurepowershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Zuweisen einer Rolle zu einer Anwendung im Abonnementkontext

So gewähren Sie einer Anwendung mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff im Abonnementkontext:

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>
```

```Example
PS C:\> New-AzureRmRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Zuweisen einer Rolle zu einem Benutzer im Ressourcengruppenkontext

So gewähren Sie einem Benutzer mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff im Ressourcengruppenkontext:

```azurepowershell
New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```

```Example
PS C:\> New-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Zuweisen einer Rolle zu einer Gruppe im Ressourcenkontext

So gewähren Sie einer Gruppe mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff im Ressourcenkontext:

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzureRmRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

## <a name="remove-access"></a>Zugriff entfernen

So entfernen Sie mit [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) den Zugriff für Benutzer, Gruppen und Anwendungen:

```azurepowershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

```Example
PS C:\> Remove-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast
```

## <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 

Zum Auflisten der Rollen, die in einem Bereich für die Zuweisung verfügbar sind, wählen Sie den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

Im folgenden Beispiel werden alle Rollen aufgelistet, die für die Zuweisung im ausgewählten Abonnement verfügbar sind.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Im folgenden Beispiel werden nur die benutzerdefinierten Rollen aufgelistet, die für die Zuweisung im ausgewählten Abonnement verfügbar sind.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Wenn sich das ausgewählte Abonnement nicht in der `AssignableScopes` der Rolle befindet, wird die benutzerdefinierte Rolle nicht aufgeführt.

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Um eine benutzerdefinierte Rolle zu erstellen, verwenden Sie den Befehl [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition). Es gibt zwei Methoden zum Strukturieren der Rolle: mithilfe eines `PSRoleDefinition`-Objekts oder einer JSON-Vorlage. 

### <a name="get-operations-for-a-resource-provider"></a>Abrufen von Vorgängen für Ressourcenanbieter

Wenn Sie benutzerdefinierte Rollen neu erstellen, ist es wichtig, alle möglichen Vorgänge der Ressourcenanbieter zu kennen.
Sie können die Liste der [Ressourcenanbietervorgänge](resource-provider-operations.md) ansehen oder den Befehl [Get AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) ausführen, um diese Informationen zu erhalten.
Wenn Sie beispielsweise alle verfügbaren Vorgänge für virtuelle Computer überprüfen möchten, verwenden Sie folgenden Befehl:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-role-with-psroledefinition-object"></a>Erstellen einer Rolle mit dem „PSRoleDefinition“-Objekt

Wenn Sie PowerShell verwenden, um eine benutzerdefinierte Rolle zu erstellen, können Sie eine der [integrierten Rollen](built-in-roles.md) als Ausgangspunkt verwenden oder von Grund auf beginnen. Das erste Beispiel in diesem Abschnitt beginnt mit einer integrierten Rolle. Diese wird dann mit weiteren Berechtigungen angepasst. Bearbeiten Sie die Attribute, und fügen Sie die gewünschten `Actions`, `NotActions` oder `AssignableScopes` hinzu. Speichern Sie die Änderungen anschließend als neue Rolle.

Das folgende Beispiel beginnt mit der integrierten Rolle [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor), um eine benutzerdefinierte Rolle namens *Virtual Machine Operator* zu erstellen. Die neue Rolle gewährt Zugriff auf alle Lesevorgänge der Ressourcenanbieter *Microsoft.Compute*, *Microsoft.Storage* und *Microsoft.Network* sowie zum Starten, Neustarten und Überwachen virtueller Computer. Die benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

Das folgende Beispiel zeigt eine weitere Möglichkeit, um die benutzerdefinierte Rolle *Virtual Machine Operator* zu erstellen. Es beginnt mit der Erstellung eines neuen „PSRoleDefinition“-Objekts. Die Aktionsvorgänge werden in der `perms`-Variable angegeben, und auf die `Actions`-Eigenschaft festgelegt. Die `NotActions`-Eigenschaft wird festgelegt, indem die `NotActions` aus der integrierten Rolle [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) gelesen werden. Da [Virtual Machine Contributor](built-in-roles.md#virtual-machine-contributor) über keine `NotActions` verfügt, ist diese Zeile nicht erforderlich, aber sie zeigt, wie Informationen aus einer anderen Rolle abgerufen werden können.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-role-with-json-template"></a>Erstellen der Rolle mit einer JSON-Vorlage

Eine JSON-Vorlage kann als Quelldefinition für die benutzerdefinierte Rolle verwendet werden. Im folgenden Beispiel wird eine benutzerdefinierte Rolle erstellt, die Lesezugriff auf Speicher- und Computeressourcen und Zugriff auf den Support ermöglicht. Außerdem wird diese Rolle zwei Abonnements hinzugefügt. Erstellen Sie eine neue Datei `C:\CustomRoles\customrole1.json` anhand des folgenden Beispiels. Die ID muss bei der Ersterstellung der Rolle auf `null` festgelegt werden, da eine neue ID automatisch generiert wird. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Um die Rolle den Abonnements hinzuzufügen, führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Ändern einer benutzerdefinierten Rolle

Ähnlich wie beim Erstellen einer benutzerdefinierten Rolle können Sie eine vorhandene benutzerdefinierte Rolle mithilfe des `PSRoleDefinition`-Objekts oder einer JSON-Vorlage ändern.

### <a name="modify-role-with-psroledefinition-object"></a>Ändern der Rolle mit „PSRoleDefinition““-Objekt

Zum Ändern einer benutzerdefinierten Rolle verwenden Sie zunächst den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um die Rollendefinition abzurufen. Nehmen Sie zweitens die gewünschten Änderungen an der Rollendefinitionsdatei vor. Verwenden Sie abschließend den Befehl [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition), um die geänderte Rollendefinition zu speichern.

Im folgenden Beispiel wird der Vorgang `Microsoft.Insights/diagnosticSettings/*` zur benutzerdefinierten Rolle *Virtual Machine Operator* hinzugefügt.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Im folgenden Beispiel wird ein Azure-Abonnement den zuweisbaren Bereichen der benutzerdefinierten Rolle *Virtual Machine Operator* hinzugefügt.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="modify-role-with-json-template"></a>Ändern der Rolle mit einer JSON-Vorlage

Mithilfe der vorherigen JSON-Vorlage können Sie eine vorhandene benutzerdefinierte Rolle über das Hinzufügen oder Entfernen von Aktionen mühelos ändern. Aktualisieren Sie die JSON-Vorlage, indem Sie „Microsoft.Network“ die Aktion „Read“ hinzufügen (siehe unten). Die Definitionen in der Vorlage werden nicht kumulativ auf eine vorhandene Definition angewendet. Dies bedeutet, dass die Rolle entsprechend Ihren Angaben in der Vorlage angezeigt wird. Außerdem müssen Sie das Feld „ID“ mit der ID der Rolle aktualisieren. Wenn Sie nicht sicher sind, wie dieser Wert lautet, können Sie diese Informationen mit dem Cmdlet [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) abrufen.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Führen Sie zum Aktualisieren der vorhandenen Rolle den folgenden PowerShell-Befehl aus:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

Um eine benutzerdefinierte Rolle zu löschen, verwenden Sie den Befehl [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition).

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator* entfernt.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="see-also"></a>Weitere Informationen

* [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

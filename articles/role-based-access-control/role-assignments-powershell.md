---
title: Verwalten des Zugriffs mit der RBAC und Azure PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und Azure PowerShell verwalten. Dazu gehören das Auflisten, Erteilen und Entfernen des Zugriffs.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/14/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 31fa2b670b6492b7496c147ef72688c3c0121fa5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429717"
---
# <a name="manage-access-using-rbac-and-azure-powershell"></a>Verwalten des Zugriffs mit der RBAC und Azure PowerShell

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung](overview.md) (Role-Based Access Control, RBAC) verwaltet. In diesem Artikel wird beschrieben, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mit der RBAC und Azure PowerShell verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Für die Zugriffsverwaltung benötigen Sie Folgendes:

* [PowerShell in Azure Cloud Shell](/azure/cloud-shell/overview)
* [Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps)

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

## <a name="list-access"></a>Auflisten des Zugriffs

Zum Auflisten des Zugriffs in RBAC führen Sie die Rollenzuweisungen auf.

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

### <a name="list-role-assignments-for-a-user"></a>Liste von Rollenzuweisungen für einen Benutzer

Verwenden Sie zum Auflisten aller Rollen, die einem bestimmten Benutzer zugewiesen sind, [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <email, userprincipalname>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

Um alle Rollen, die einem bestimmten Benutzer zugewiesen sind, und die Rollen aufzulisten, die den Gruppen des Benutzers zugewiesen sind, verwenden Sie [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <email, userprincipalname> -ExpandPrincipalGroups
```

```Example
Get-AzureRmRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

### <a name="list-role-assignments-for-classic-service-administrator-and-co-administrators"></a>Auflisten der Rollenzuweisungen für klassische Abonnementadministratoren und -Co-Administratoren

Zum Auflisten der Rollenzuweisungen für den Administrator und die Co-Administratoren für das klassische Abonnement verwenden Sie [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>Gewähren von Zugriff

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung.

### <a name="search-for-object-ids"></a>Suchen nach Objekt-IDs

Um eine Rolle zuzuweisen, müssen Sie sowohl das Objekt (Benutzer, Gruppe oder Anwendung) als auch den Bereich identifizieren.

Falls Sie die Abonnement-ID nicht kennen, können Sie diese im Blatt **Abonnements** im Azure-Portal finden, oder Sie verwenden [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

So rufen Sie die Objekt-ID für eine Azure AD-Gruppe mit [Get-AzureRmADGroup](/powershell/module/azurerm.resources/get-azurermadgroup) ab:

```azurepowershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

Die Objekt-ID für einen Azure AD-Dienstprinzipal oder eine Azure AD-Anwendung können Sie mit [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal) abrufen.

```azurepowershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="create-a-role-assignment-for-an-application-at-a-subscription-scope"></a>Erstellen einer Rollenzuweisung für eine Anwendung im Abonnementbereich

Einer Anwendung können Sie mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff auf der Ebene der Abonnements gewähren.

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope /subscriptions/<subscription id>
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

### <a name="create-a-role-assignment-for-a-user-at-a-resource-group-scope"></a>Erstellen einer Rollenzuweisung für einen Benutzer in einem Ressourcengruppenbereich

Einem Benutzer können Sie mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff auf der Ebene der Ressourcengruppe gewähren.

```azurepowershell
New-AzureRmRoleAssignment -SignInName <email, userprincipalname> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
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

### <a name="create-a-role-assignment-for-a-group-at-a-resource-scope"></a>Erstellen einer Rollenzuweisung für eine Gruppe in einem Ressourcenbereich

Einer Gruppe können Sie mit [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) Zugriff auf Ressourcenebene gewähren.

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

In der RBAC entfernen Sie mit [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) eine Rollenzuweisung, um den Zugriff zu entfernen.

```azurepowershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

```Example
PS C:\> Remove-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Gewähren des Zugriffs für eine Gruppe mit der RBAC und mit Azure PowerShell](tutorial-role-assignments-group-powershell.md)
- [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell](tutorial-custom-role-powershell.md)
- [Verwalten von Ressourcen mit Azure PowerShell](../azure-resource-manager/powershell-azure-resource-manager.md)

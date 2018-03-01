---
title: Verwalten der rollenbasierten Zugriffssteuerung (RBAC) mit der Azure-CLI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) mit der Azure-Befehlszeilenschnittstelle verwalten, indem Sie Rollen und Rollenaktionen auflisten und Rollen im Kontext von Abonnements und Anwendungen zuweisen.
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 5c099a7fd8848c2934603ec9b2db8947885226f9
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
> * [REST-API](role-based-access-control-manage-access-rest.md)


Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) definieren Sie den Zugriff für Benutzer, Gruppen und Dienstprinzipale durch Zuweisen von Rollen in einem bestimmten Kontext. In diesem Artikel wird beschrieben, wie Sie den Zugriff mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwaltung von RBAC mithilfe der Azure-Befehlszeilenschnittstelle müssen folgende Voraussetzungen erfüllt sein:

* [Azure CLI 2.0](/cli/azure/overview). Sie können sie in Ihrem Browser mit [Azure Cloud Shell](../cloud-shell/overview.md) verwenden oder unter macOS, Linux und Windows [installieren](/cli/azure/install-azure-cli) und über die Befehlszeile ausführen.

## <a name="list-roles"></a>Auflisten der Rollen

### <a name="list-role-definitions"></a>Auflisten der Rollendefinitionen

Zum Auflisten aller verfügbaren Rollendefinitionen verwenden Sie [az role definition list](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list
```

Im folgenden Beispiel werden der Name und die Beschreibung aller verfügbaren Rollendefinitionen aufgelistet:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Im folgenden Beispiel werden alle integrierten Rollendefinitionen aufgelistet:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description, "type":.properties.type}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "type": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role-definition"></a>Auflisten der Aktionen einer Rollendefinition

Zum Auflisten der Aktionen einer Rollendefinition verwenden Sie [az role definition list](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list --name <role_name>
```

Im folgenden Beispiel wird die Rollendefinition für *Contributor* (Mitwirkender) aufgelistet:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "properties": {
      "additionalProperties": {
        "createdBy": null,
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedBy": null,
        "updatedOn": "2016-12-14T02:04:45.1393855Z"
      },
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything except access to resources.",
      "permissions": [
        {
          "actions": [
            "*"
          ],
          "notActions": [
            "Microsoft.Authorization/*/Delete",
            "Microsoft.Authorization/*/Write",
            "Microsoft.Authorization/elevateAccess/Action"
          ]
        }
      ],
      "roleName": "Contributor",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Im folgenden Beispiel werden *actions* (Aktionen) und *notActions* (Keine Aktionen) der Rolle *Contributor* (Mitwirkender) aufgelistet:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.properties.permissions[0].actions, "notActions":.properties.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Im folgenden Beispiel werden die Aktionen der Rolle *Virtual Machine Contributor* (Mitwirkender von virtuellen Computern) aufgelistet:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .properties.permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Auflisten des Zugriffs

### <a name="list-role-assignments-for-a-user"></a>Auflisten von Rollenzuweisungen für einen Benutzer

Zum Auflisten der Rollenzuweisungen für einen bestimmten Benutzer verwenden Sie [az role assignment list](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --assignee <assignee>
```

Standardmäßig werden nur Zuweisungen im Abonnementkontext angezeigt. Zum Anzeigen von Zuweisungen im Ressourcen- oder Gruppenkontext verwenden Sie `--all`.

Im folgenden Beispiel werden die Rollenzuweisungen aufgelistet, die dem Benutzer *patlong@contoso.com* direkt zugewiesen sind:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.properties.principalName, "roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Auflisten von Rollenzuweisungen für eine Ressourcengruppe

Zum Auflisten der Rollenzuweisungen, die für eine Ressourcengruppe vorhanden sind, verwenden Sie [az role assignment list](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Im folgenden Beispiel werden die Rollenzuweisungen für die Ressourcengruppe *pharma-sales-projectforecast* aufgelistet:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="assign-access"></a>Zuweisen des Zugriffs

### <a name="assign-a-role-to-a-user"></a>Zuweisen einer Rolle zu einem Benutzer

Zum Zuweisen einer Rolle zu einem Benutzer im Ressourcengruppenkontext verwenden Sie [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Im folgenden Beispiel wird dem Benutzer *patlong@contoso.com* im Ressourcengruppenkontext *pharma-sales-projectforecast* die Rolle *Virtual Machine Contributor* zugewiesen:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="assign-a-role-to-a-group"></a>Zuweisen einer Rolle zu einer Gruppe

Zum Zuweisen einer Rolle zu einer Gruppe verwenden Sie [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Im folgenden Beispiel wird der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Abonnementkontext die Rolle *Reader* zugewiesen. Zum Abrufen der ID der Gruppe können Sie [az ad group list](/cli/azure/ad/group#az_ad_group_list) oder [az ad group show](/cli/azure/ad/group#az_ad_group_show) verwenden.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Im folgenden Beispiel wird die Rolle *Virtual Machine Contributor* der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Ressourcenkontext für ein virtuelles Netzwerk mit dem Namen *pharma-sales-project-network* zugewiesen:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="assign-a-role-to-an-application"></a>Zuweisen einer Rolle zu einer Anwendung

Zum Zuweisen einer Rolle zu einer Anwendung verwenden Sie [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Im folgenden Beispiel wird einer Anwendung mit der Objekt-ID 44444444-4444-4444-4444-444444444444 im Ressourcengruppenkontext *pharma-sales-projectforecast* die Rolle *Virtual Machine Contributor* zugewiesen. Zum Abrufen der Objekt-ID der Anwendung können Sie [az ad app list](/cli/azure/ad/app#az_ad_app_list) oder [az ad app show](/cli/azure/ad/app#az_ad_app_show) verwenden.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Entfernen des Zugriffs

### <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

Zum Entfernen einer Rollenzuweisung verwenden Sie [az role assignment delete](/cli/azure/role/assignment#az_role_assignment_delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Im folgenden Beispiel wird die Zuweisung der Rolle *Virtual Machine Contributor* von Benutzer *patlong@contoso.com* für die Ressourcengruppe *pharma-sales-projectforecast* entfernt:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Im folgenden Beispiel wird die Rolle *Reader* von der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Abonnementkontext entfernt. Zum Abrufen der ID der Gruppe können Sie [az ad group list](/cli/azure/ad/group#az_ad_group_list) oder [az ad group show](/cli/azure/ad/group#az_ad_group_show) verwenden.

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="custom-roles"></a>Benutzerdefinierte Rollen

### <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 

Zum Auflisten der Rollen, die in einem Kontext für die Zuweisung verfügbar sind, verwenden Sie [az role definition list](/cli/azure/role/definition#az_role_definition_list).

In den beiden folgenden Beispielen werden alle benutzerdefinierten Rollen im aktuellen Abonnement aufgelistet:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.properties.roleName, "type":.properties.type}'
```

```azurecli
az role definition list --output json | jq '.[] | if .properties.type == "CustomRole" then {"roleName":.properties.roleName, "type":.properties.type} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Operator Role",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}

...
```

### <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Zum Erstellen einer benutzerdefinierten Rolle verwenden Sie [az role definition create](/cli/azure/role/definition#az_role_definition_create). Bei der Rollendefinition kann es sich um eine JSON-Beschreibung oder einen Pfad zu einer Datei mit einer JSON-Beschreibung handeln.

```azurecli
az role definition create --role-definition <role_definition>
```

Im folgenden Beispiel wird eine benutzerdefinierte Rolle mit dem Namen *Virtual Machine Operator* erstellt. Diese benutzerdefinierte Rolle weist den Zugriff auf alle Lesevorgänge der Ressourcenanbieter *Microsoft.Compute*, *Microsoft.Storage* und *Microsoft.Network* sowie den Zugriff zum Starten, Neustarten und Überwachen virtueller Computer zu. Diese benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden. In diesem Beispiel wird eine JSON-Datei als Eingabe genutzt.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

### <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Zum Aktualisieren einer benutzerdefinierten Rolle verwenden Sie zuerst [az role definition list](/cli/azure/role/definition#az_role_definition_list), um die Rollendefinition abzurufen. Nehmen Sie zweitens die gewünschten Änderungen an der Rollendefinitionsdatei vor. Verwenden Sie zum Schluss [az role definition update](/cli/azure/role/definition#az_role_definition_update), um die aktualisierte Rollendefinition zu speichern.

```azurecli
az role definition update --role-definition <role_definition>
```

Im folgenden Beispiel wird der Vorgang *Microsoft.Insights/diagnosticSettings/* zu *Actions* der benutzerdefinierten Rolle *Virtual Machine Operator* hinzugefügt.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

### <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

Zum Löschen einer benutzerdefinierten Rolle verwenden Sie [az role definition delete](/cli/azure/role/definition#az_role_definition_delete). Zum Angeben der zu löschenden Rolle verwenden Sie den Rollennamen oder die Rollen-ID. Zum Ermitteln der Rollen-ID verwenden Sie [az role definition list](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator* gelöscht:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]


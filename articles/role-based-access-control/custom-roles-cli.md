---
title: Erstellen benutzerdefinierter Rollen mithilfe der Azure CLI | Microsoft-Dokumentation
description: Informationen zum Erstellen benutzerdefinierter Rollen für rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) mithilfe der Azure CLI. Dies umfasst das Auflisten, Erstellen, Aktualisieren und Löschen von benutzerdefinierten Rollen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b1df50c73497e3f5ad64a7ba7210079871b155e0
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36324175"
---
# <a name="create-custom-roles-using-azure-cli"></a>Erstellen benutzerdefinierter Rollen mithilfe der Azure CLI

Wenn die [integrierten Rollen](built-in-roles.md) den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen benutzerdefinierten Rollen erstellen. In diesem Artikel wird das Erstellen und Verwalten benutzerdefinierter Rollen mithilfe der Azure CLI beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen von benutzerdefinierten Rollen in Azure benötigen Sie:

- Berechtigungen zum Erstellen benutzerdefinierter Rollen, etwa [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator)
- [Azure CLI](/cli/azure/install-azure-cli) (lokal installiert)

## <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 

Zum Auflisten der benutzerdefinierten Rollen, die zur Zuweisung verfügbar sind, verwenden Sie [az role definition list](/cli/azure/role/definition#az-role-definition-list). In den folgenden Beispielen werden alle benutzerdefinierten Rollen im aktuellen Abonnement aufgelistet.

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Zum Erstellen einer benutzerdefinierten Rolle verwenden Sie [az role definition create](/cli/azure/role/definition#az-role-definition-create). Bei der Rollendefinition kann es sich um eine JSON-Beschreibung oder einen Pfad zu einer Datei mit einer JSON-Beschreibung handeln.

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

## <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Zum Aktualisieren einer benutzerdefinierten Rolle verwenden Sie zuerst [az role definition list](/cli/azure/role/definition#az-role-definition-list), um die Rollendefinition abzurufen. Nehmen Sie zweitens die gewünschten Änderungen an der Rollendefinitionsdatei vor. Verwenden Sie zum Schluss [az role definition update](/cli/azure/role/definition#az-role-definition-update), um die aktualisierte Rollendefinition zu speichern.

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

## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

Zum Löschen einer benutzerdefinierten Rolle verwenden Sie [az role definition delete](/cli/azure/role/definition#az-role-definition-delete). Zum Angeben der zu löschenden Rolle verwenden Sie den Rollennamen oder die Rollen-ID. Zum Ermitteln der Rollen-ID verwenden Sie [az role definition list](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator* gelöscht.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe der Azure-Befehlszeilenschnittstelle](tutorial-custom-role-cli.md)
- [Benutzerdefinierte Rollen in Azure](custom-roles.md)
- [Vorgänge für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md)
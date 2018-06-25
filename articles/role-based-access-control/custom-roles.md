---
title: Erstellen benutzerdefinierter Rollen für Azure RBAC | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie benutzerdefinierte Rollen mit der rollenbasierten Zugriffssteuerung in Azure für eine präzisere Identitätsverwaltung in Ihrem Azure-Abonnement definieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3baf616e448f1f6d5292161ae125502d72141940
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266593"
---
# <a name="create-custom-roles-in-azure"></a>Erstellen von benutzerdefinierten Rollen in Azure

Wenn die [integrierten Rollen](built-in-roles.md) nicht Ihre spezifischen Zugriffsanforderungen erfüllen, können Sie eigene benutzerdefinierte Rollen erstellen. Genau wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Dienstprinzipalen auf Abonnement-, Ressourcengruppen- und Ressourcenebene zugewiesen werden. Benutzerdefinierte Rollen werden in einem Azure AD-Mandanten (Azure Active Directory) gespeichert und können für mehrere Abonnements genutzt werden. Jeder Mandant kann bis zu 2.000 benutzerdefinierte Rollen enthalten. Benutzerdefinierte Rollen können mit Azure PowerShell, der Azure CLI oder der REST-API erstellt werden.

In diesem Artikel wird ein Beispiel für die ersten Schritte beim Erstellen von benutzerdefinierten Rollen mithilfe von PowerShell und der Azure CLI beschrieben.

## <a name="create-a-custom-role-to-open-support-requests-using-powershell"></a>Erstellen einer benutzerdefinierten Rolle zum Öffnen von Supportanfragen mithilfe von PowerShell

Zum Erstellen einer benutzerdefinierten Rolle können Sie mit einer integrierten Rolle beginnen, sie bearbeiten und dann eine neue Rolle erstellen. In diesem Beispiel ist die integrierte [Leserolle](built-in-roles.md#reader) auf die Erstellung einer benutzerdefinierten Rolle mit dem Namen „Leserolle mit Zugriff auf Supporttickets“ zugeschnitten. Sie ermöglicht dem Benutzer, alle Inhalte des Abonnement anzuzeigen und auch Supportanfragen zu erstellen.

> [!NOTE]
> Die beiden einzigen integrierten Rollen, die einem Benutzer das Erstellen von Supportanfragen ermöglichen, sind [Besitzer](built-in-roles.md#owner) und [Mitwirkender](built-in-roles.md#contributor). Damit ein Benutzer Supportanfragen eröffnen kann, muss dem Benutzer eine Rolle für den Abonnementbereich zugewiesen werden, da alle Supportanfragen basierend auf einem Azure-Abonnement erstellt werden.

Verwenden Sie in PowerShell den Befehl [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um die Rolle [Leser](built-in-roles.md#reader) im JSON-Format zu exportieren.

```azurepowershell
Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json
```

Die JSON-Ausgabe für die [Leserolle](built-in-roles.md#reader) sieht wie folgt aus. Eine typische Rolle besteht aus drei Hauptabschnitten: `Actions`, `NotActions` und `AssignableScopes`. Im Abschnitt `Actions` sind alle zulässigen Vorgänge für die Rolle aufgelistet. Um Vorgänge von `Actions` ausschließen, fügen Sie sie zu `NotActions` hinzu. Die effektiven Berechtigungen werden durch Subtrahieren der `NotActions`-Vorgänge von den `Actions`-Vorgängen berechnet.

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
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Als Nächstes bearbeiten Sie die JSON-Ausgabe, um Ihre benutzerdefinierte Rolle zu erstellen. In diesem Fall muss für das Erstellen von Supporttickets der Vorgang `Microsoft.Support/*` hinzugefügt werden. Jeder Vorgang wird über einen Ressourcenanbieter verfügbar gemacht. Zum Abrufen einer Liste der Vorgänge für einen Ressourcenanbieter können Sie den Befehl [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) verwenden, oder sehen Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md) nach.

Es ist zwingend erforderlich, dass die Rolle die expliziten ID der Abonnements enthält, in denen sie verwendet wird. Die Abonnement-IDs sind unter `AssignableScopes` aufgelistet. Andernfalls können Sie die Rolle nicht in Ihr Abonnement importieren.

Schließlich müssen Sie die `IsCustom`-Eigenschaft auf `true` festlegen, um anzugeben, dass dies eine benutzerdefinierte Rolle ist.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/subscriptions/11111111-1111-1111-1111-111111111111"
                         ]
}
```

Zum Erstellen der neuen benutzerdefinierten Rolle verwenden Sie den Befehl [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) und stellen die aktualisierte JSON-Rollendefinitionsdatei bereit.

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\rbacrole2.json"
```

Nachdem Sie [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) ausgeführt haben, ist die neue benutzerdefinierte Rolle im Azure-Portal verfügbar und kann Benutzern zugewiesen werden.

![Screenshot der in das Azure-Portal importierten benutzerdefinierten Rolle](./media/custom-roles/18.png)

![Screenshot der Zuweisung der benutzerdefinierten importierten Rolle an einen Benutzer im gleichen Verzeichnis](./media/custom-roles/19.png)

![Screenshot der Berechtigungen für die benutzerdefinierte importierte Rolle](./media/custom-roles/20.png)

Benutzer mit dieser benutzerdefinierten Rolle können jetzt Supportanfragen erstellen.

![Screenshot der Erstellung von Supportanfragen durch benutzerdefinierte Rolle](./media/custom-roles/21.png)

Benutzer mit dieser benutzerdefinierten Rolle können keine weiteren Aktionen wie z.B. das Erstellen von virtuellen Computern oder Ressourcengruppen ausführen.

![Screenshot einer benutzerdefinierten Rolle, die keine virtuellen Computer erstellen kann](./media/custom-roles/22.png)

![Screenshot einer benutzerdefinierten Rolle, die keine neuen RG erstellen kann](./media/custom-roles/23.png)

## <a name="create-a-custom-role-to-open-support-requests-using-azure-cli"></a>Erstellen einer benutzerdefinierten Rolle zum Öffnen von Supportanfragen mithilfe der Azure CLI

Die Schritte zum Erstellen einer benutzerdefinierten Rolle mithilfe der Azure CLI sind ähnlich wie bei der Verwendung von PowerShell, mit dem Unterschied, dass die JSON-Ausgabe anders ist.

In diesem Beispiel können Sie mit der integrierten Rolle [Leser](built-in-roles.md#reader) beginnen. Zum Auflisten der Aktionen der [Leserrolle](built-in-roles.md#reader) verwenden Sie den Befehl [az role definition list](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition list --name "Reader" --output json
```

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": [],
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Erstellen Sie eine JSON-Datei mit dem folgenden Format. Der Vorgang `Microsoft.Support/*` wurde in den `Actions`-Abschnitten hinzugefügt, sodass dieser Benutzer Supportanfragen erstellen kann und weiterhin die Leserolle innehat. Sie müssen die ID des Abonnements, in dem diese Rolle verwendet wird, im Abschnitt `AssignableScopes` hinzufügen.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes": [
                            "/subscriptions/11111111-1111-1111-1111-111111111111"
                        ]
}
```

Zum Erstellen der neuen benutzerdefinierten Rolle verwenden Sie den Befehl [az role definition create](/cli/azure/role/definition#az_role_definition_create).

```azurecli
az role definition create --role-definition ~/roles/rbacrole1.json
```

Die neue benutzerdefinierte Rolle ist jetzt im Azure-Portal verfügbar, und der Verwendungsprozess dieser Rolle ist der gleiche wie im vorhergehenden PowerShell-Abschnitt.

![Screenshot des Azure-Portals mit der benutzerdefinierten, mithilfe der CLI 1.0 erstellten Rolle](./media/custom-roles/26.png)


## <a name="see-also"></a>Weitere Informationen
- [Grundlegendes zu Rollendefinitionen](role-definitions.md)
- [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](role-assignments-powershell.md)
- [Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle](role-assignments-cli.md)
- [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](role-assignments-rest.md)

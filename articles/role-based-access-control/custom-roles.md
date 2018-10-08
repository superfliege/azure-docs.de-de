---
title: Benutzerdefinierte Rollen in Azure | Microsoft-Dokumentation
description: Informationen zum Definieren benutzerdefinierter Rollen in der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) für die präzise Verwaltung des Zugriffs auf Ressourcen in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 281c426170985d43401a13988218126ea3951634
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405613"
---
# <a name="custom-roles-in-azure"></a>Benutzerdefinierte Rollen in Azure

Wenn die [integrierten Rollen](built-in-roles.md) den Anforderungen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen benutzerdefinierten Rollen erstellen. Genau wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Dienstprinzipalen auf Abonnement-, Ressourcengruppen- und Ressourcenebene zugewiesen werden. Benutzerdefinierte Rollen werden in einem Azure AD-Verzeichnis (Azure Active Directory) gespeichert und können für mehrere Abonnements genutzt werden. Jedes Verzeichnis kann bis zu 2.000 benutzerdefinierte Rollen enthalten. Benutzerdefinierte Rollen können mit Azure PowerShell, der Azure CLI oder der REST-API erstellt werden.

## <a name="custom-role-example"></a>Beispiel einer benutzerdefinierten Rolle

Das folgende Beispiel zeigt, wie eine benutzerdefinierte Rolle im JSON-Format angezeigt aussieht. Diese benutzerdefinierte Rolle kann zum Überwachen und Neustarten virtueller Computer verwendet werden.

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
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
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

Wenn Sie eine benutzerdefinierte Rolle erstellt haben, wird sie im Azure-Portal mit einem orangefarbenen Ressourcensymbol angezeigt.

![Symbol der benutzerdefinierten Rolle](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Schritte zum Erstellen einer benutzerdefinierten Rolle

1. Bestimmen der Berechtigungen, die Sie benötigen

    Wenn Sie eine benutzerdefinierte Rolle erstellen, müssen Sie die Ressourcenanbietervorgänge kennen, die zum Definieren der Berechtigungen verfügbar sind. Um die Liste der Vorgänge anzuzeigen, können Sie den Befehl [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) oder [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list) verwenden.
    Um die Berechtigungen für die benutzerdefinierte Rolle anzugeben, fügen Sie die Vorgänge der `Actions`- oder `NotActions`-Eigenschaft der [Rollendefinition](role-definitions.md) hinzu. Wenn Sie über Datenvorgänge verfügen, fügen Sie diese der `DataActions`- oder `NotDataActions`-Eigenschaft hinzu.

2. Erstellen der benutzerdefinierten Rolle

    Sie können Azure PowerShell oder Azure CLI zum Erstellen der benutzerdefinierten Rolle verwenden. In der Regel beginnen Sie mit einer vorhandenen integrierten Rolle und ändern sie Ihren Anforderungen entsprechend. Dann erstellen Sie die benutzerdefinierte Rolle mit dem Befehl [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) bzw. [az role definition create](/cli/azure/role/definition#az-role-definition-create). Um eine benutzerdefinierte Rolle zu erstellen, benötigen Sie die `Microsoft.Authorization/roleDefinitions/write`-Berechtigung für alle `AssignableScopes`, wie z.B. [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator).

3. Testen der benutzerdefinierten Rolle

    Sobald Sie über Ihre benutzerdefinierte Rolle verfügen, müssen Sie sie testen, um sicherzustellen, dass sie wie erwartet funktioniert. Wenn Sie später Anpassungen vornehmen müssen, können Sie die benutzerdefinierte Rolle aktualisieren.

Ein ausführliches Tutorial zum Erstellen einer benutzerdefinierten Rolle finden Sie unter [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell](tutorial-custom-role-powershell.md) oder [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe der Azure-Befehlszeilenschnittstelle](tutorial-custom-role-cli.md).

## <a name="custom-role-properties"></a>Eigenschaften der benutzerdefinierten Rolle

Eine benutzerdefinierte Rolle hat die folgenden Eigenschaften.

| Eigenschaft | Erforderlich | Typ | BESCHREIBUNG |
| --- | --- | --- | --- |
| `Name` | JA | Zeichenfolge | Der Anzeigename der benutzerdefinierten Rolle. Obwohl es sich bei einer Rollendefinition um eine Ressource auf Abonnementebene handelt, kann eine solche Definition dennoch in mehreren Abonnements verwendet werden, die dasselbe Azure AD-Verzeichnis gemeinsam nutzen. Der Anzeigename muss im Bereich des Azure AD-Verzeichnisses eindeutig sein. Er kann Buchstaben, Ziffern, Leerzeichen und Sonderzeichen enthalten. Die maximale Anzahl von Zeichen ist 128. |
| `Id` | JA | Zeichenfolge | Die eindeutige ID der benutzerdefinierten Rolle. Für Azure PowerShell und Azure CLI wird diese ID automatisch generiert, wenn Sie eine neue Rolle erstellen. |
| `IsCustom` | JA | Zeichenfolge | Gibt an, ob dies eine benutzerdefinierte Rolle ist. Legen Sie für benutzerdefinierte Rollen `true` fest. |
| `Description` | JA | Zeichenfolge | Die Beschreibung der benutzerdefinierten Rolle. Er kann Buchstaben, Ziffern, Leerzeichen und Sonderzeichen enthalten. Die maximale Anzahl von Zeichen ist 1.024. |
| `Actions` | JA | String[] | Ein Array von Zeichenfolgen, das die Verwaltungsvorgänge angibt, deren Ausführung die Rolle zulässt. Weitere Informationen finden Sie unter [Aktionen](role-definitions.md#actions). |
| `NotActions` | Nein  | String[] | Ein Array von Zeichenfolgen, das die Verwaltungsvorgänge angibt, die von den zulässigen `Actions` ausgeschlossen sind. Weitere Informationen finden Sie unter [NotActions](role-definitions.md#notactions). |
| `DataActions` | Nein  | String[] | Ein Array von Zeichenfolgen, das die Datenvorgänge angibt, deren Ausführung für Ihre Daten innerhalb des Objekts die Rolle zulässt. Weitere Informationen finden Sie unter [DataActions (Vorschau)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Nein  | String[] | Ein Array von Zeichenfolgen, das die Datenvorgänge angibt, die von den zulässigen `DataActions` ausgeschlossen sind. Weitere Informationen finden Sie unter [NotDataActions (Vorschau)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | JA | String[] | Ein Array von Zeichenfolgen, das die Bereiche angibt, in denen die benutzerdefinierte Rolle zur Zuweisung verfügbar ist. Derzeit kann es nicht auf den Stammbereich (`"/"`) oder eine Verwaltungsgruppenbereich festgelegt werden. Weitere Informationen finden Sie unter [AssignableScopes](role-definitions.md#assignablescopes) und [Organisieren von Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/index.md#custom-rbac-role-definition-and-assignment). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Rollen zum Erstellen, Löschen, Aktualisieren oder Anzeigen einer benutzerdefinierten Rolle

Wie integrierte Rollen legt die `AssignableScopes`-Eigenschaft die Bereiche fest, in denen die Rolle zur Zuweisung verfügbar ist. Die `AssignableScopes`-Eigenschaft für eine benutzerdefinierte Rolle steuert auch, wer zum Erstellen, Löschen, Aktualisieren oder Anzeigen der benutzerdefinierten Rolle berechtigt ist.

| Aufgabe | Vorgang | BESCHREIBUNG |
| --- | --- | --- |
| Erstellen/Löschen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/write` | Benutzer, die für alle `AssignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen für die Verwendung in diesen Bereichen erstellen (oder löschen). Hierzu zählen etwa [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Abonnements, Ressourcengruppen und Ressourcen. |
| Aktualisieren einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/write` | Benutzer, die für alle `AssignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen in diesen Bereichen aktualisieren. Hierzu zählen etwa [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Abonnements, Ressourcengruppen und Ressourcen. |
| Anzeigen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/read` | Benutzer, die in einem Bereich zu diesem Vorgang berechtigt sind, können die benutzerdefinierten Rollen anzeigen, die für die Zuweisung in diesem Bereich verfügbar sind. Benutzerdefinierte Rollen können bei allen integrierten Rollen für die Zuweisung verfügbar sein. |

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen benutzerdefinierter Rollen mit Azure PowerShell](custom-roles-powershell.md)
- [Erstellen benutzerdefinierter Rollen mithilfe der Azure CLI](custom-roles-cli.md)
- [Grundlegendes zu Rollendefinitionen](role-definitions.md)

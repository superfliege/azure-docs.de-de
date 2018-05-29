---
title: Grundlegendes zu Rollendefinitionen in Azure RBAC | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zu Rollendefinitionen in der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) sowie zum Definieren benutzerdefinierter Rollen für die präzise Zugriffsverwaltung von Ressourcen in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: ''
ms.openlocfilehash: 7a9e257d445ff7dadfe27d1c75cde6f58a393397
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161813"
---
# <a name="understand-role-definitions"></a>Grundlegendes zu Rollendefinitionen

Wenn Sie die Funktionsweise eine Rolle nachvollziehen oder eine eigene [benutzerdefinierte Rolle](custom-roles.md) erstellen möchten, ist es hilfreich zu verstehen, wie Rollen definiert werden. Dieser Artikel geht ausführlich auf Rollendefinitionen ein und enthält einige Beispiele.

## <a name="role-definition-structure"></a>Struktur einer Rollendefinition

Eine *Rollendefinition* ist eine Sammlung von Berechtigungen. Gelegentlich wird sie auch einfach als *Rolle* bezeichnet. Eine Rollendefinition listet die ausführbaren Vorgänge wie etwa Lesen, Schreiben und Löschen auf. Sie kann auch die nicht ausführbaren Vorgänge auflisten. Eine Rollendefinition hat folgende Struktur:

```
assignableScopes []
description
id
name
permissions []
  actions []
  notActions []
roleName
roleType
type
```

Vorgänge werden mit Zeichenfolgen im folgenden Format angegeben:

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

Der Teil `{action}` einer Vorgangszeichenfolge gibt die Art der Vorgänge an, die Sie für einen Ressourcentyp ausführen können. So weist `{action}` beispielsweise folgende Teilzeichenfolgen auf:

| Aktionsteilzeichenfolge    | Beschreibung         |
| ------------------- | ------------------- |
| `*` | Das Platzhalterzeichen gewährt Zugriff auf alle Vorgänge, die der Zeichenfolge entsprechen. |
| `read` | Ermöglicht Lesevorgänge (GET). |
| `write` | Ermöglicht Schreibvorgänge (PUT, POST und PATCH). |
| `delete` | Ermöglicht Löschvorgänge (DELETE). |

Hier sehen Sie die Rollendefinition [Mitwirkender](built-in-roles.md#contributor) im JSON-Format. Der Platzhaltervorgang (`*`) unter `actions` gibt an, dass der Prinzipal, der dieser Rolle zugewiesen ist, alle Aktionen ausführen und somit alles verwalten kann. Dies schließt auch Aktionen ein, die später definiert werden, wenn Azure neue Ressourcentypen hinzufügt. Die Vorgänge unter `notActions` werden von `actions` subtrahiert. Im Falle der Rolle [Mitwirkender](built-in-roles.md#contributor) sorgt `notActions` dafür, dass die Rolle weder den Zugriff auf Ressourcen verwalten noch Zugriff für Ressourcen zuweisen kann.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-operations"></a>Verwaltungsvorgänge

Die rollenbasierte Zugriffssteuerung für Verwaltungsvorgänge wird in den Abschnitten `actions` und `notActions` einer Rollendefinition angegeben. Im Anschluss finden Sie einige Beispiele für Verwaltungsvorgänge in Azure:

- Verwalten des Zugriffs auf ein Speicherkonto
- Erstellen, Aktualisieren oder Löschen eines Blobcontainers
- Löschen einer Ressourcengruppe und aller dazugehörigen Ressourcen

Verwaltungszugriff geht nicht auf Ihre Daten über. Diese Trennung verhindert, dass Rollen mit Platzhaltern (`*`) uneingeschränkten Zugriff auf Ihre Daten erhalten. Wenn einem Benutzer also beispielsweise die Rolle [Leser](built-in-roles.md#reader) für ein Abonnement zugewiesen ist, kann er das Speicherkonto anzeigen, aber nicht die zugrunde liegenden Daten.

## <a name="actions"></a>actions

Die Berechtigung `actions` gibt die Verwaltungsvorgänge an, auf die die Rolle Zugriff gewährt. Es handelt sich um eine Sammlung von Vorgangszeichenfolgen, mit denen sicherungsfähige Vorgänge von Azure-Ressourcenanbietern identifiziert werden. Im Anschluss finden Sie einige Beispiele für Verwaltungsvorgänge, die in `actions` verwendet werden können.

| Vorgangszeichenfolge    | Beschreibung         |
| ------------------- | ------------------- |
| `*/read` | Gewährt Zugriff auf Lesevorgänge für alle Ressourcentypen aller Azure-Ressourcenanbieter.|
| `Microsoft.Compute/*` | Gewährt Zugriff auf alle Vorgänge für alle Ressourcentypen im Microsoft.Compute-Ressourcenanbieter.|
| `Microsoft.Network/*/read` | Gewährt Zugriff auf Lesevorgänge für alle Ressourcentypen im Microsoft.Network-Ressourcenanbieter.|
| `Microsoft.Compute/virtualMachines/*` | Gewährt Zugriff auf alle Vorgänge virtueller Computer sowie auf die dazugehörigen untergeordneten Ressourcentypen.|
| `microsoft.web/sites/restart/Action` | Gewährt Zugriff zum Neustarten einer Web-App.|

## <a name="notactions"></a>notActions

Die Berechtigung `notActions` gibt die Verwaltungsvorgänge an, die von den zulässigen Aktionen (`actions`) ausgeschlossen sind. Verwenden Sie die Berechtigung `notActions`, wenn die Vorgänge, die Sie zulassen möchten, durch den Ausschluss unzulässiger Vorgänge leichter zu definieren sind. Zur Ermittlung des Zugriffs, der durch eine Rolle gewährt wird (effektive Berechtigungen), werden die `notActions`-Vorgänge von den `actions`-Vorgängen subtrahiert.

> [!NOTE]
> Wenn einem Benutzer eine Rolle zugewiesen wird, die einen Vorgang in `notActions` ausschließt, und dem Benutzer dann durch Zuweisen einer zweiten Rolle der Zugriff auf diesen Vorgang gewährt wird, kann der Benutzer den Vorgang durchführen. `notActions` ist keine Verweigerungsregel. Es ist lediglich eine bequeme Möglichkeit, eine Gruppe zulässiger Vorgänge zu erstellen, wenn bestimmte Vorgänge ausgeschlossen werden müssen.
>

## <a name="assignablescopes"></a>assignableScopes

Der Abschnitt `assignableScopes` gibt die Bereiche (Verwaltungsgruppen (derzeit in der Vorschauphase), Abonnements, Ressourcengruppen und Ressourcen) an, für die die Rolle zugewiesen werden kann. Sie können die Verfügbarkeit der Rolle für die Zuweisung auf die Abonnements oder Ressourcengruppen beschränken, für die dies erforderlich ist, um eine bessere Übersichtlichkeit der restlichen Abonnements oder Ressourcengruppen zu erzielen. Sie müssen mindestens eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressourcen-ID verwenden.

Bei integrierten Rollen ist `assignableScopes` auf den Stammbereich (`"/"`) festgelegt. Der Stammbereich gibt an, dass die Rolle für die Zuweisung in allen Bereichen verfügbar ist. Der Stammbereich kann nicht in eigenen benutzerdefinierten Rollen verwendet werden. Wenn Sie es trotzdem versuchen, tritt ein Autorisierungsfehler auf.

Beispiele für gültige zuweisbare Bereiche:

| Szenario | Beispiel |
|----------|---------|
| Rolle ist für die Zuweisung in einem einzelnen Abonnement verfügbar | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Rolle ist für die Zuweisung in zwei Abonnements verfügbar | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Rolle ist nur für die Zuweisung in der Netzwerkressourcengruppe verfügbar | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Rolle ist für die Zuweisung in allen Bereichen verfügbar | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>„assignableScopes“ und benutzerdefinierte Rollen

Der Abschnitt `assignableScopes` für eine benutzerdefinierte Rolle steuert auch, wer zum Erstellen, Löschen, Ändern oder Anzeigen der benutzerdefinierten Rolle berechtigt ist.

| Aufgabe | Vorgang | Beschreibung |
| --- | --- | --- |
| Erstellen/Löschen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/write` | Benutzer, die für alle `assignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen für die Verwendung in diesen Bereichen erstellen (oder löschen). Hierzu zählen etwa [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Abonnements, Ressourcengruppen und Ressourcen. |
| Ändern einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/write` | Benutzer, die für alle `assignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen in diesen Bereichen ändern. Hierzu zählen etwa [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Abonnements, Ressourcengruppen und Ressourcen. |
| Anzeigen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinition/read` | Benutzer, die in einem Bereich zu diesem Vorgang berechtigt sind, können die benutzerdefinierten Rollen anzeigen, die für die Zuweisung in diesem Bereich verfügbar sind. Benutzerdefinierte Rollen können bei allen integrierten Rollen für die Zuweisung verfügbar sein. |

## <a name="role-definition-examples"></a>Beispiele für Rollendefinitionen

Das folgende Beispiel zeigt die Azure CLI-Darstellung der Rollendefinition [Leser](built-in-roles.md#reader):

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
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

Das folgende Beispiel zeigt die Azure PowerShell-Darstellung einer benutzerdefinierten Rolle zum Überwachen und Neustarten virtueller Computer:

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
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>Weitere Informationen

* [Integrierte Rollen](built-in-roles.md)
* [Benutzerdefinierte Rollen](custom-roles.md)
* [Vorgänge für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md)

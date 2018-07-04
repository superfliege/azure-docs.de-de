---
title: Erstellen von benutzerdefinierten Rollen mithilfe der REST-API – Azure | Microsoft-Dokumentation
description: Informationen zum Erstellen benutzerdefinierter Rollen für rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) mithilfe der REST-API. Dies umfasst das Auflisten, Erstellen, Aktualisieren und Löschen von benutzerdefinierten Rollen.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 8267846fed30baf2c37dcddd453ae9ead9341da9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36323395"
---
# <a name="create-custom-roles-using-the-rest-api"></a>Erstellen benutzerdefinierter Rollen mithilfe der REST-API

Wenn die [integrierten Rollen](built-in-roles.md) den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen benutzerdefinierten Rollen erstellen. In diesem Artikel wird das Erstellen und Verwalten benutzerdefinierter Rollen mithilfe der REST-API beschrieben.

## <a name="list-roles"></a>Auflisten der Rollen

Verwenden Sie zum Auflisten aller Rollen oder Abrufen von Informationen zu einer einzelnen Rolle mit deren Anzeigenamen die REST-API [Rollendefinitionen – Liste](/rest/api/authorization/roledefinitions/list). Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleDefinitions/read` für den Bereich. Einigen [integrierten Rollen](built-in-roles.md) wird der Zugriff auf diesen Vorgang gewährt.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* mit dem Bereich, für den Sie die Rollen auflisten möchten.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{filter}* durch die Bedingung, die Sie zum Filtern der Rollenliste anwenden möchten.

    | Filter | BESCHREIBUNG |
    | --- | --- |
    | `$filter=atScopeAndBelow()` | Dient zum Auflisten der Rollen, die im angegebenen Bereich und in allen untergeordneten Bereichen verfügbar sind. |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Verwenden Sie die URL-codierte Form des genauen Anzeigenamens der Rolle. Beispiel: `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'`. |

### <a name="get-information-about-a-role"></a>Abrufen von Informationen zu einer Rolle

Verwenden Sie zum Abrufen von Informationen zu einer Rolle mit ihrem Rollendefinitionsbezeichner die REST-API [Rollendefinitionen – Abrufen](/rest/api/authorization/roledefinitions/get). Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleDefinitions/read` für den Bereich. Einigen [integrierten Rollen](built-in-roles.md) wird der Zugriff auf diesen Vorgang gewährt.

Informationen zum Abrufen von Informationen zu einer einzelnen Rolle über den Anzeigenamen finden Sie im vorherigen Abschnitt [Auflisten der Rollen](custom-roles-rest.md#list-roles).

1. Verwenden Sie die [Rollendefinitionen – Liste](/rest/api/authorization/roledefinitions/list)-REST-API zum Abrufen des GUID-Bezeichners für die Rolle. Für integrierte Rollen können Sie außerdem den Bezeichner von [integrierten Rollen](built-in-roles.md) erhalten.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* mit dem Bereich, für den Sie die Rollen auflisten möchten.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roledefinitionid}* durch den GUID-Bezeichner der Rollendefinition.

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Um eine benutzerdefinierte Rolle zu erstellen, verwenden Sie die [Rollendefinitionen – Erstellen oder Aktualisieren](/rest/api/authorization/roledefinitions/createorupdate)-REST-API. Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleDefinitions/write` für alle `assignableScopes`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang. 

1. Überprüfen Sie die Liste der [Ressourcenanbietervorgänge](resource-provider-operations.md), die zum Erstellen von Berechtigungen für die benutzerdefinierte Rolle verfügbar sind.

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für den benutzerdefinierten Rollenbezeichner verwendet wird. Das Format des Bezeichners ist: `00000000-0000-0000-0000-000000000000`

1. Beginnen Sie mit folgender Anforderung und folgendem Textkörper:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Ersetzen Sie im URI *{scope}* durch die ersten `assignableScopes` der benutzerdefinierten Rolle.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roleDefinitionId}* durch den GUID-Bezeichner der benutzerdefinierten Rolle.

1. Ersetzen Sie im Anforderungstext in der `assignableScopes`-Eigenschaft *{roleDefinitionId}* mit dem GUID-Bezeichner.

1. Ersetzen Sie *subscriptionid* durch Ihre Abonnement-ID.

1. Fügen Sie in der `actions`-Eigenschaft die Vorgänge hinzu, deren Ausführung die Rolle zulässt.

1. Fügen Sie in der `notActions`-Eigenschaft die Vorgänge hinzu, die von den zulässigen `actions` ausgeschlossen sind.

1. Geben Sie in den Eigenschaften `roleName` und `description` einen eindeutigen Rollennamen und eine Beschreibung an. Weitere Informationen zu den Eigenschaften finden Sie unter [Erstellen von benutzerdefinierten Rollen in Azure](custom-roles.md).

    Es folgt ein Beispiel für einen Anforderungstext:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
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
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Um eine benutzerdefinierte Rolle zu aktualisieren, verwenden Sie die [Rollendefinitionen – Erstellen oder Aktualisieren](/rest/api/authorization/roledefinitions/createorupdate)-REST-API. Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleDefinitions/write` für alle `assignableScopes`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang. 

1. Verwenden Sie die [Rollendefinitionen – Liste](/rest/api/authorization/roledefinitions/list)- oder [Rollendefinitionen – Abrufen](/rest/api/authorization/roledefinitions/get)-REST-API zum Abrufen von Informationen zu der benutzerdefinierten Rolle. Weitere Informationen finden Sie im vorherigen Abschnitt [Auflisten der Rollen](custom-roles-rest.md#list-roles).

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Ersetzen Sie im URI *{scope}* durch die ersten `assignableScopes` der benutzerdefinierten Rolle.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roleDefinitionId}* durch den GUID-Bezeichner der benutzerdefinierten Rolle.

1. Erstellen Sie basierend auf den Informationen über die benutzerdefinierte Rolle einen Anforderungstext mit folgendem Format:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Aktualisieren Sie den Anforderungstext mit den Änderungen, die Sie an der benutzerdefinierten Rolle vornehmen möchten.

    Im Folgenden sehen Sie ein Beispiel für einen Anforderungstext mit einer neu hinzugefügten Diagnoseeinstellungenaktion:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
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
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

Verwenden Sie zum Löschen einer benutzerdefinierten Rolle die [Rollendefinitionen – Löschen](/rest/api/authorization/roledefinitions/delete)-REST-API. Zum Aufrufen dieser API benötigen Sie Zugriff auf den `Microsoft.Authorization/roleDefinitions/delete`-Vorgang für alle `assignableScopes`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang. 

1. Verwenden Sie die [Rollendefinitionen – Liste](/rest/api/authorization/roledefinitions/list)- oder [Rollendefinitionen – Abrufen](/rest/api/authorization/roledefinitions/get)-REST-API zum Abrufen des GUID-Bezeichners der benutzerdefinierten Rolle. Weitere Informationen finden Sie im vorherigen Abschnitt [Auflisten der Rollen](custom-roles-rest.md#list-roles).

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* mit dem Bereich, in dem Sie die benutzerdefinierte Rolle löschen möchten.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roleDefinitionId}* durch den GUID-Bezeichner der benutzerdefinierten Rolle.

## <a name="next-steps"></a>Nächste Schritte

- [Benutzerdefinierte Rollen in Azure](custom-roles.md)
- [Verwalten des Zugriffs mithilfe der RBAC und der REST-API](role-assignments-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
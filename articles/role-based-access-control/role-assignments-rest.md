---
title: Verwalten des Zugriffs mithilfe der RBAC und der Azure-REST-API | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und der REST-API verwalten. Dazu gehören das Auflisten, Erteilen und Entfernen des Zugriffs.
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
ms.openlocfilehash: cfcb87fdff8105b25d4f7e63b775aaf9243d2a90
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317006"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>Verwalten des Zugriffs mithilfe der RBAC und der REST-API

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung](overview.md) (Role-Based Access Control, RBAC) verwaltet. In diesem Artikel wird beschrieben, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mit der RBAC und der REST-API verwalten.

## <a name="list-access"></a>Auflisten des Zugriffs

Zum Auflisten des Zugriffs in RBAC führen Sie die Rollenzuweisungen auf. Um Rollenzuweisungen aufzulisten, verwenden Sie eine der REST-APIs unter [Rollenzuweisungen – Auflisten](/rest/api/authorization/roleassignments/list). Um die Ergebnisse einzugrenzen, geben Sie einen Bereich und einen optionalen Filter an. Zum Aufrufen der API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/read` für den angegebenen Bereich. Einigen [integrierten Rollen](built-in-roles.md) wird der Zugriff auf diesen Vorgang gewährt.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, für den die Rollen aufgelistet werden sollen.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Rollenzuweisungen angewendet werden soll.

    | Filter | BESCHREIBUNG |
    | --- | --- |
    | `$filter=atScope()` | Listet nur die Rollenzuweisungen für den angegebenen Bereich auf, ohne die Rollenzuweisungen der Unterbereiche. |
    | `$filter=principalId%20eq%20'{objectId}'` | Listet die Rollenzuweisungen für bestimmte Benutzer, Gruppen oder Dienstprinzipale auf. |
    | `$filter=assignedTo('{objectId}')` | Listet Rollenzuweisungen für bestimmte Benutzer auf, einschließlich für von Gruppen geerbten Benutzer. |

## <a name="grant-access"></a>Gewähren von Zugriff

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung. Um eine Rollenzuweisung zu erstellen, verwenden Sie die REST-API unter [Rollenzuweisungen – Erstellen](/rest/api/authorization/roleassignments/create), und geben Sie das Sicherheitsprinzipal, die Rollendefinition und den Bereich an. Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/write`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

1. Verwenden Sie die REST API unter [Rollendefinitionen – Auflisten](/rest/api/authorization/roledefinitions/list) oder lesen Sie [Integrierte Rollen in Azure](built-in-roles.md), um den Bezeichner für die zuzuweisende Rollendefinition zu ermitteln.

1. Verwenden Sie ein GUID-Tool, um einen eindeutigen Bezeichner zu generieren, der für den Bezeichner der Rollenzuweisung verwendet wird. Das Format des Bezeichners ist: `00000000-0000-0000-0000-000000000000`

1. Beginnen Sie mit folgender Anforderung und folgendem Text:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich für die Rollenzuweisung.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roleAssignmentName}* durch den GUID-Bezeichner der Rollenzuweisung.

1. Ersetzen Sie innerhalb des Anforderungstexts *{subscriptionId}* durch Ihre Abonnement-ID.

1. Ersetzen Sie *{roleDefinitionId}* durch den Bezeichner der Rollendefinition.

1. Ersetzen Sie *{principalId}* durch den Objektbezeichner des Benutzers, der Gruppe oder des Dienstprinzipals, der bzw. die der Rolle zugewiesen wird.

## <a name="remove-access"></a>Zugriff entfernen

In RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen. Zum Entfernen einer Rollenzuweisung verwenden Sie die REST-API [Rollenzuweisungen – Löschen](/rest/api/authorization/roleassignments/delete). Zum Aufrufen dieser API benötigen Sie Zugriff auf den Vorgang `Microsoft.Authorization/roleAssignments/delete`. Von den integrierten Rollen verfügen nur [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) über Zugriff auf diesen Vorgang.

1. Rufen Sie den Bezeichner der Rollenzuweisung (GUID) ab. Dieser Bezeichner wird beim ersten Erstellen der Rollenzuweisung zurückgegeben oder kann durch Auflisten der Rollenzuweisungen ermittelt werden.

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Ersetzen Sie innerhalb des URIs *{scope}* durch den Bereich, um die Rollenzuweisung zu entfernen.

    | Umfang | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{roleAssignmentName}* durch den GUID-Bezeichner der Rollenzuweisung.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST-API-Referenz](/rest/api/azure/)
- [Erstellen benutzerdefinierter Rollen mithilfe der REST-API](custom-roles-rest.md)
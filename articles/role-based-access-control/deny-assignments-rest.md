---
title: Auflisten von Ablehnungszuweisungen mithilfe von RBAC und der REST-API – Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ablehnungszuweisungen für Benutzer, Gruppen und Anwendungen mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und der REST-API auflisten.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 44c1d3b18bb9bdc63247379fe3f277cb6542f2da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975583"
---
# <a name="list-deny-assignments-using-rbac-and-the-rest-api"></a>Auflisten von Ablehnungszuweisungen mithilfe von RBAC und der REST-API

Derzeit sind Ablehnungszuweisungen **schreibgeschützt** und können nur von Azure festgelegt werden. Auch wenn Sie keine eigenen Ablehnungszuweisungen erstellen können, ist es möglich, Ablehnungszuweisungen aufzulisten, da sie sich auf Ihre geltenden Berechtigungen auswirken können. In diesem Artikel wird beschrieben, wie Sie Ablehnungszuweisungen mithilfe von RBAC und der REST-API auflisten.

## <a name="list-a-single-deny-assignment"></a>Auflisten einer einzelnen Ablehnungszuweisung

1. Beginnen Sie mit der folgenden Anforderung:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. Ersetzen Sie innerhalb des URI *{scope}* durch den Bereich, für den die Ablehnungszuweisungen aufgelistet werden sollen.

    | Bereich | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{deny-assignment-id}* durch den Bezeichner für die Ablehnungszuweisung, die Sie abrufen möchten.

## <a name="list-multiple-deny-assignments"></a>Auflisten von mehreren Ablehnungszuweisungen

1. Beginnen Sie mit einer der folgenden Anforderungen:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Mit optionalen Parametern:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Ersetzen Sie innerhalb des URI *{scope}* durch den Bereich, für den die Ablehnungszuweisungen aufgelistet werden sollen.

    | Bereich | Typ |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Ressourcengruppe |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Ressource |

1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Ablehnungszuweisungen angewendet werden soll.

    | Filter | BESCHREIBUNG |
    | --- | --- |
    | (Kein Filter) | Alle Ablehnungszuweisungen des angegebenen Bereichs (auch ober- und unterhalb) werden aufgelistet. |
    | `$filter=atScope()` | Nur die Ablehnungszuweisungen für den angegebenen Bereich und oberhalb davon werden aufgelistet. Hierin sind nicht die Ablehnungszuweisungen von Unterbereichen enthalten. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Ablehnungszuweisungen mit dem angegebenen Namen werden aufgelistet. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Auflisten von Ablehnungszuweisungen im Stammbereich (/)

1. Erhöhen Sie Ihre Zugriffsrechte wie unter [Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory](elevate-access-global-admin.md) beschrieben.

1. Verwenden Sie die folgende Anforderung:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Ersetzen Sie *{filter}* durch die Bedingung, die zum Filtern der Liste mit den Ablehnungszuweisungen angewendet werden soll. Ein Filter ist erforderlich.

    | Filter | BESCHREIBUNG |
    | --- | --- |
    | `$filter=atScope()` | Nur Ablehnungszuweisungen für den Stammbereich werden aufgelistet. Hierin sind nicht die Ablehnungszuweisungen von Unterbereichen enthalten. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Ablehnungszuweisungen mit dem angegebenen Namen werden aufgelistet. |

1. Entfernen Sie die erhöhten Zugriffsrechte.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Ablehnungszuweisungen](deny-assignments.md)
- [Erhöhen der Zugriffsrechte für einen globalen Administrator in Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST-API-Referenz](/rest/api/azure/)

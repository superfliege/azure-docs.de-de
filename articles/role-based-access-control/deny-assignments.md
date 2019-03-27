---
title: Verstehen von Ablehnungszuweisungen für Azure-Ressourcen | Microsoft-Dokumentation
description: Informationen zu Ablehnungszuweisungen in der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: fff213c8d1a408bf96e385f2097a5ef30dcc05d2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57992091"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Verstehen von Ablehnungszuweisungen für Azure-Ressourcen

Ähnlich wie eine Rollenzuweisung verknüpft eine *Ablehnungszuweisung* in einem bestimmten Bereich einen Satz von Aktionen mit einem Benutzer, einer Gruppe oder einem Dienstprinzipal, um den Zugriff zu verweigern. Ablehnungszuweisungen hindern Benutzer an der Ausführung bestimmter Aktionen für Azure-Ressourcen, auch wenn ihnen über eine Rollenzuweisung Zugriff erteilt wird. Einige Ressourcenanbieter in Azure umfassen jetzt Ablehnungszuweisungen.

Ablehnungszuweisungen unterscheiden sich in gewisser Weise von Rollenzuweisungen. Ablehnungszuweisungen können Prinzipale ausschließen und die Vererbung an untergeordnete Bereiche verhindern. Ablehnungszuweisungen werden auch auf Zuweisungen für [klassische Abonnementadministratoren](rbac-and-directory-admin-roles.md) angewendet.

In diesem Artikel wird die Definition von Ablehnungszuweisungen beschrieben.

> [!NOTE]
> Zu diesem Zeitpunkt können Sie nur dann Ihre eigenen Ablehnungszuweisungen hinzufügen, wenn Sie Azure Blueprints verwenden. Weitere Informationen finden Sie unter [Schützen neuer Ressourcen mit Azure Blueprints-Ressourcensperren](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="deny-assignment-properties"></a>Eigenschaften von Ablehnungszuweisungen

 Eine Ablehnungszuweisungen hat folgende Eigenschaften:

> [!div class="mx-tableFixed"]
> | Eigenschaft | Erforderlich | Typ | BESCHREIBUNG |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Ja | Zeichenfolge | Der Anzeigename der Ablehnungszuweisung. Namen müssen für einen bestimmten Bereich eindeutig sein. |
> | `Description` | Nein  | Zeichenfolge | Die Beschreibung der Ablehnungszuweisung. |
> | `Permissions.Actions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotActions` | Nein  | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Permissions.DataActions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotDataActions` | Nein  | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Scope` | Nein  | Zeichenfolge | Eine Zeichenfolge, die den Bereich festlegt, für den die Ablehnungszuweisung gilt. |
> | `DoNotApplyToChildScopes` | Nein  | Boolescher Wert | Gibt an, ob die Ablehnungszuweisung für untergeordnete Bereiche gilt. Der Standardwert ist „false“. |
> | `Principals[i].Id` | Ja | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung gilt. Die Festlegung einer leeren GUID `00000000-0000-0000-0000-000000000000` repräsentiert alle Prinzipale. |
> | `Principals[i].Type` | Nein  | String[] | Ein Array von Objekttypen, das durch „Principals[i].Id“ dargestellt wird. Eine leere GUID `SystemDefined` repräsentiert alle Prinzipale. |
> | `ExcludePrincipals[i].Id` | Nein  | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung nicht gilt. |
> | `ExcludePrincipals[i].Type` | Nein  | String[] | Ein Array von Objekttypen, das durch „ExcludePrincipals[i].Id“ dargestellt wird. |
> | `IsSystemProtected` | Nein  | Boolescher Wert | Gibt an, ob diese Ablehnungszuweisung von Azure erstellt wurde und nicht bearbeitet oder gelöscht werden kann. Derzeit sind alle Ablehnungszuweisungen vom System geschützt. |

## <a name="system-defined-principal"></a>Systemseitig definierter Prinzipal

Zur Unterstützung von Ablehnungszuweisungen wurde der **systemseitig definierte Prinzipal** eingeführt. Dieser Prinzipal repräsentiert alle Benutzer, Gruppen, Dienstprinzipale und verwalteten Identitäten in einem Azure AD-Verzeichnis. Wenn die Prinzipal-ID eine aus Nullen bestehende GUID `00000000-0000-0000-0000-000000000000` ist und der Prinzipaltyp `SystemDefined` lautet, repräsentiert der Prinzipal alle Prinzipale. `SystemDefined` kann mit `ExcludePrincipals` kombiniert werden, um den Zugriff für alle Prinzipale mit Ausnahme einiger Benutzer zu verweigern. Für `SystemDefined` gelten die folgenden Einschränkungen:

- Kann nur in `Principals` verwendet werden, aber nicht in `ExcludePrincipals`.
- `Principals[i].Type` muss auf `SystemDefined` festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Auflisten von Ablehnungszuweisungen für Azure-Ressourcen mithilfe der REST-API](deny-assignments-rest.md)
* [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](role-definitions.md)

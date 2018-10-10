---
title: Grundlegendes zu Ablehnungszuweisungen in Azure RBAC | Microsoft-Dokumentation
description: Informationen zu Ablehnungszuweisungen in der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Ressourcen in Azure.
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
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 8ef3a2ec44c5eff80d3a50a6c56805667e164ba8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980167"
---
# <a name="understand-deny-assignments"></a>Grundlegendes zu Ablehnungszuweisungen

Ähnlich wie eine Rollenzuweisung bindet eine *Ablehnungszuweisung* in einem bestimmten Bereich einen Satz von Aktionen an einen Benutzer, eine Gruppe oder einen Dienstprinzipal, um Zugriff zu verweigern. Eine Ablehnungszuweisungen kann auch Prinzipale ausschließen und eine Vererbung an untergeordnete Bereiche verhindern. In diesem Aspekt unterscheidet sie sich von Rollenzuweisungen. Derzeit sind **Ablehnungszuweisung** schreibgeschützt und können nur von Azure festgelegt werden. In diesem Artikel wird die Definition von Ablehnungszuweisungen beschrieben.

## <a name="deny-assignment-properties"></a>Eigenschaften von Ablehnungszuweisungen

 Eine Ablehnungszuweisungen hat folgende Eigenschaften:

> [!div class="mx-tableFixed"]
> | Eigenschaft | Erforderlich | Typ | BESCHREIBUNG |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | JA | Zeichenfolge | Der Anzeigename der Ablehnungszuweisung. Namen müssen für einen bestimmten Bereich eindeutig sein. |
> | `Description` | Nein  | Zeichenfolge | Die Beschreibung der Ablehnungszuweisung. |
> | `Permissions.Actions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotActions` | Nein  | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Permissions.DataActions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotDataActions` | Nein  | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Scope` | Nein  | Zeichenfolge | Eine Zeichenfolge, die den Bereich festlegt, für den die Ablehnungszuweisung gilt. |
> | `DoNotApplyToChildScopes` | Nein  | Boolescher Wert | Gibt an, ob die Ablehnungszuweisung für untergeordnete Bereiche gilt. Der Standardwert ist „false“. |
> | `Principals[i].Id` | JA | String[] | Ein Array von Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe oder Dienstprinzipal), für die die Ablehnungszuweisung gilt. Eine leere GUID `00000000-0000-0000-0000-000000000000` stellt alle Benutzer dar. |
> | `Principals[i].Type` | Nein  | String[] | Ein Array von Objekttypen, das durch „Principals[i].Id“ dargestellt wird. Auf `Everyone` festlegen, um alle Benutzer darzustellen. |
> | `ExcludePrincipals[i].Id` | Nein  | String[] | Ein Array von Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe oder Dienstprinzipal), für die die Ablehnungszuweisung nicht gilt. |
> | `ExcludePrincipals[i].Type` | Nein  | String[] | Ein Array von Objekttypen, das durch „ExcludePrincipals[i].Id“ dargestellt wird. |
> | `IsSystemProtected` | Nein  | Boolescher Wert | Gibt an, ob diese Ablehnungszuweisung von Azure erstellt wurde und nicht bearbeitet oder gelöscht werden kann. Derzeit sind alle Ablehnungszuweisungen vom System geschützt. |

## <a name="everyone-principal"></a>Prinzipal „Jeder“

Zur Unterstützung von Ablehnungszuweisungen wurde der Prinzipal „Jeder“ eingeführt. Der Prinzipal „Jeder“ stellt alle Benutzer, Gruppen und Dienstprinzipale in einem Azure AD-Verzeichnis dar. Wenn die Prinzipal-ID ein NULL-GUID `00000000-0000-0000-0000-000000000000` und der Prinzipaltyp `Everyone` ist, stellt der Prinzipal alle Benutzer dar. Der Prinzipal „Jeder“ kann mit `ExcludePrincipals` kombiniert werden, um jeden mit Ausnahme einiger Benutzer zu verweigern. Der Prinzipal „Jeder“ weist folgende Einschränkungen auf:

- Kann nur in `Principals` verwendet werden, aber nicht in `ExcludePrincipals`.
- `Principals[i].Type` muss auf `Everyone` festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Auflisten von Ablehnungszuweisungen mithilfe von RBAC und der REST-API](deny-assignments-rest.md)
* [Grundlegendes zu Rollendefinitionen](role-definitions.md)

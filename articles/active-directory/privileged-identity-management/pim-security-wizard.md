---
title: Sicherheits-Assistent in PIM – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt den Sicherheits-Assistenten, der bei der ersten Verwendung von Azure AD Privileged Identity Management (PIM) angezeigt wird.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 178a4c5e978075f2a59b22a1cccf462138527964
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189078"
---
# <a name="security-wizard-in-pim"></a>Sicherheits-Assistent in PIM
Wenn Sie die erste Person sind, die Azure Privileged Identity Management (PIM) ausführt, wird Ihnen ein Assistent angezeigt. Der Assistent bietet Einblick in die Sicherheitsrisiken privilegierter Identitäten und informiert darüber, wie Sie diese Risiken mithilfe von PIM reduzieren. Sie müssen die vorhandenen Rollenzuweisungen im Assistenten nicht ändern, wenn Sie dies lieber später machen möchten.

## <a name="what-to-expect"></a>Ausblick
Bevor Ihre Organisation mit der Verwendung von PIM beginnt, sind alle Rollenzuweisungen permanent, d. h. die Benutzer besitzen diese Rollen ständig, auch wenn sie die zugehörigen Berechtigungen derzeit nicht benötigen.  Der erste Schritt des Assistenten bietet eine Liste der Rollen mit hohen Berechtigungen und zeigt an, wie viele Benutzer derzeit in diesen Rollen vorhanden sind. Sie können Details zu einer bestimmten Rolle anzeigen, um weitere Informationen zu den Benutzern zu erhalten und zu ermitteln, ob ein oder mehrere Benutzer unbekannt sind.

Im zweiten Schritt des Assistenten haben Sie die Möglichkeit, die Rollenzuweisungen des Administrators zu ändern.  

> [!WARNING]
> Es müssen unbedingt mindestens ein globaler Administrator sowie mehrere Administratoren für privilegierte Rollen mit einem Organisationskonto (keinem Microsoft-Konto) vorhanden sein. Wenn nur ein Administrator für privilegierte Rollen vorhanden ist, kann die Organisation PIM nicht mehr verwalten, sollte dieses Konto gelöscht werden.
> Verwenden Sie außerdem permanente Rollenzuweisungen, wenn ein Benutzer über ein Microsoft-Konto verfügt (ein Konto zum Anmelden bei Microsoft-Diensten wie Skype und Outlook.com). Wenn Sie planen, zur Aktivierung dieser Rolle MFA als erforderlich festzulegen, kann der Benutzer sie nicht übernehmen.
> 
> 

Nachdem Sie Änderungen vorgenommen haben, wird der Assistent nicht mehr angezeigt. Wenn Sie oder ein anderer Administrator für privilegierte Rollen PIM das nächste Mal verwenden, wird das PIM-Dashboard angezeigt.  

* Wenn Sie Benutzer zu Rollen hinzufügen oder aus Rollen entfernen oder permanente in berechtigte Zuweisungen ändern möchten, finden Sie weitere Informationen unter [Hinzufügen oder Entfernen einer Benutzerrolle](pim-how-to-add-role-to-user.md).
* Wenn Sie weiteren Benutzern Verwaltungszugriff auf PIM gewähren möchten, finden Sie weitere Informationen unter [Gewähren des Zugriffs zur Verwaltung von PIM](pim-how-to-give-access-to-pim.md)

## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in die Verwendung von PIM](pim-getting-started.md)
- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)
- [Gewähren von Zugriff für andere Administratoren zum Verwalten von PIM](pim-how-to-give-access-to-pim.md)

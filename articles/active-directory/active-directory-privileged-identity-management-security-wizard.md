---
title: Der Sicherheits-Assistent von Azure AD Privileged Identity Management
description: Bei der ersten Verwendung der Erweiterung Azure Active Directory Privileged Identity Management wird ein Sicherheits-Assistent angezeigt. Dieser Artikel beschreibt die Schritte zur Verwendung des Assistenten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: d380c8076fbe1a6c9e06acb6f583d719f43aaba4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448593"
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Verwenden des Sicherheits-Assistenten in Azure AD Privileged Identity Management 
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

* Wenn Sie Benutzer zu Rollen hinzufügen oder aus Rollen entfernen oder permanente in berechtigte Zuweisungen ändern möchten, finden Sie weitere Informationen unter [Hinzufügen oder Entfernen einer Benutzerrolle](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Wenn Sie weiteren Benutzern Verwaltungszugriff auf PIM gewähren möchten, finden Sie weitere Informationen unter [Gewähren des Zugriffs zur Verwaltung von PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


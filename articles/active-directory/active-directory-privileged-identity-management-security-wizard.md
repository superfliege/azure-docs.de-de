---
title: "Den Azure AD Privileged Identitätsmanagement-Assistenten"
description: Der ersten Verwendung die Azure Active Directory Privileged Identity Management-Erweiterung wird mit einem Security-Assistenten angezeigt werden. Dieser Artikel beschreibt die Schritte zur Verwendung des Assistenten.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 260d178f3d8158411b3ad266e3b0d15edbebc722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Verwenden des Assistenten in Azure AD Privileged Identitätsmanagement 
Wenn Sie die erste Person Azure Privileged Identity Management (PIM) für Ihre Organisation ausführen möchten, wird ein Assistent angezeigt. Der Assistent unterstützt Sie verstehen, die Sicherheitsrisiken der privilegierten Identitäten und wie PIM verwendet, um diese Risiken zu reduzieren. Sie müssen Änderungen an vorhandenen rollenzuweisungen im Assistenten vornehmen, wenn Sie später durchführen möchten.

## <a name="what-to-expect"></a>Was Sie erwartet
Vor dem Beginn Ihrer Organisation mithilfe von PIM werden alle rollenzuweisungen permanente: Benutzer stellen immer in diesen Rollen, selbst wenn sie gegenwärtig nicht über ihre Berechtigungen benötigen.  Der erste Schritt des Assistenten zeigt eine Liste mit hohen Berechtigungen von Rollen und wie viele Benutzer in diesen Rollen aktuell sind. Sie können einen Drilldown in einer bestimmten Rolle Weitere Informationen zum Benutzer wenn eine oder mehrere dieser Header nicht vertraut sind.

Der zweite Schritt des Assistenten haben Sie die Möglichkeit des Administrators rollenzuweisungen zu ändern.  

> [!WARNING]
> Es ist wichtig, dass Sie über mindestens ein globaler Administrator und mehr als eine privilegierte Rollenadministrator mit einem organisationskonto (kein Microsoft-Konto) verfügen. Ist nur eine privilegierte Rollenadministrator, wird die Organisation werden keine PIM verwalten, wenn das Konto gelöscht wird.
> Außerdem, dass rollenzuweisungen dauerhaft ausgeführt, wenn ein Benutzer verfügt über ein Microsoft-Konto (ein Konto, das sie verwenden, um zur Anmeldung bei Microsoft-Diensten wie Skype und Outlook.com). Wenn Sie planen, die MFA für die Aktivierung für diese Rolle erforderlich ist, wird dieser Benutzer gesperrt werden.
> 
> 

Nachdem Sie Änderungen vorgenommen haben, wird der Assistent nicht mehr angezeigt. Das nächste Mal von einem privilegierten Rolle Administrator verwenden PIM, sehen Sie das PIM-Dashboard.  

* Wenn Sie hinzufügen oder Entfernen von Benutzern aus Rollen aus, oder Ändern von Zuweisungen von permanenten in berechtigten möchten, erfahren Sie mehr zur [Vorgehensweise beim Hinzufügen oder Entfernen einer Benutzerrolle "](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Wenn Sie mehrere Benutzer Zugriff zum Verwalten von PIM erhalten möchten, erfahren Sie mehr zur [wie Zugriff auf die in PIM verwalten](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


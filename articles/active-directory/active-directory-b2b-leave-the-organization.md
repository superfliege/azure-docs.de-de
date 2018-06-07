---
title: Verlassen eine Organisation als Gastbenutzer – Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel zeigt, wie ein Azure AD B2B-Gastbenutzer eine Organisation über den Zugriffsbereich verlassen kann.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 942439da3e116415c77a28950df69d44744b2dd8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077705"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Verlassen einer Organisation als Gastbenutzer

Ein Azure Active Directory B2B-Gastbenutzer (Azure AD) kann eine Organisation jederzeit verlassen, wenn er die Apps dieser Organisation nicht mehr verwendet oder Zuordnungen verwalten muss. Ein Benutzer kann eine Organisation selbst verlassen, ohne sich an einen Administrator wenden zu müssen.

## <a name="leave-an-organization"></a>Verlassen einer Organisation

Um eine Organisation als Benutzer zu verlassen, der beim [Zugriffsbereich](https://myapps.microsoft.com) angemeldet ist, gehen Sie folgendermaßen vor:

1. Wenn Sie nicht bereits bei der Organisation angemeldet sind, die Sie verlassen möchten, wählen Sie Ihren Namen in der oberen rechten Ecke aus, und klicken Sie auf die Organisation, die Sie verlassen möchten.
2. Wählen Sie in der Ecke oben rechts Ihren Namen aus.
3. Wählen Sie neben **Organisationen** das Einstellungssymbol (Zahnrad) aus.
 
   ![Screenshot von den Benutzereinstellungen im Zugriffsbereich](media/active-directory-b2b-leave-the-organization/UserSettings.png) 

3. Suchen Sie unter **Organisationen** nach der Organisation, die Sie verlassen möchten, und wählen Sie **Organisation verlassen** aus.

   ![Screenshot der Option „Organisation verlassen“ auf der Benutzeroberfläche](media/active-directory-b2b-leave-the-organization/LeaveOrg.png)

4. Wenn Sie aufgefordert werden, Ihre Auswahl zu bestätigen, wählen Sie **Verlassen** aus. 

## <a name="account-removal"></a>Entfernen des Kontos

Wenn ein Benutzer eine Organisation verlässt, muss das Benutzerkonto im Verzeichnis „vorläufig gelöscht“ werden. Das Benutzerobjekt wird in Azure AD standardmäßig in den Bereich **Gelöschte Benutzer** verschoben, wird jedoch erst nach 30 Tagen dauerhaft gelöscht. Bei einer solchen vorläufigen Löschung kann der Administrator das Benutzerkonto (einschließlich Gruppen und Berechtigungen) wiederherstellen, wenn der Benutzer innerhalb eines Zeitraums von 30 Tagen eine Anfrage zur Kontowiederherstellung sendet.

Falls gewünscht, kann ein Mandantenadministrator das Konto innerhalb des Zeitraums von 30 Tagen jederzeit dauerhaft löschen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus.
2. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
3. Wählen Sie **Gelöschte Benutzer** aus.
4. Aktivieren Sie das Kontrollkästchen neben einem gelöschten Benutzer, und wählen Sie dann **Endgültig löschen** aus.

Wenn Sie einen Benutzer dauerhaft löschen, so ist diese Aktion unwiderruflich.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Azure AD B2B finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)




---
title: "Zurücksetzen von Kennwörtern in Azure Active Directory | Microsoft-Dokumentation"
description: "Vom Administrator initiierte Kennwortzurücksetzung für einen Benutzer in Azure Active Directory"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: 
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: bea082081e3f3f52ba78188903a9536fe9de9392
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Zurücksetzen des Kennworts für einen Benutzer in Azure Active Directory

Administratoren müssen möglicherweise das Kennwort eines Benutzers in Fällen zurücksetzen, in denen er es vergessen hat, ausgesperrt wurde oder in anderen Szenarien. Die folgenden Schritte begleiten Sie durch das Zurücksetzen des Kennworts eines Benutzers.

## <a name="how-to-reset-the-password-for-a-user"></a>Wie Sie das Kennwort für einen Benutzer zurücksetzen können

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit einem Konto an, das über Verzeichnisberechtigungen zum Zurücksetzen von Benutzerkennwörtern verfügt.
2. Wählen Sie **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer**.
3. Wählen Sie den Benutzer, dessen Kennwort Sie zurücksetzen möchten.
2. Wählen Sie für den ausgewählten Benutzer die Option **Kennwort zurücksetzen** aus.

    ![Zurücksetzen des Kennworts für einen Benutzer aus dem Profil des Benutzers in Azure AD](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. Wählen Sie unter **Kennwort zurücksetzen** die Option **Kennwort zurücksetzen** aus.
7. Ein temporäres Kennwort wird angezeigt, das Sie dann dem Benutzer bereitstellen können. Der Benutzer wird dann aufgefordert, sein Kennwort bei der nächsten Anmeldung zu ändern. 

   > [!NOTE]
   > Dieses temporäre Kennwort unterliegt keiner Ablaufzeit, sodass es gültig ist, bis sich der Benutzer wieder anmeldet und es dann ändern muss. 

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen eines Benutzers](active-directory-users-create-azure-portal.md)
* [Zuweisen eines Benutzers zu Administratorrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
* [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](active-directory-users-delete-user-azure-portal.md)

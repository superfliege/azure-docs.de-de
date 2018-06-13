---
title: Hinzufügen von B2B-Zusammenarbeitsbenutzern über das Azure-Portal – Azure Active Directory | Microsoft-Dokumentation
description: Hier wird gezeigt, wie ein Administrator seinem Verzeichnis unter Verwendung von Azure AD B2B-Zusammenarbeit (Azure Active Directory) Gastbenutzer aus einer Partnerorganisation hinzufügen kann.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 9d0565468d953c83ca5fee864b3079fbfee9bbf1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076764"
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal

Als globaler Administrator oder Benutzer mit einer der eingeschränkten Administratorverzeichnisrollen können Sie B2B-Zusammenarbeitsbenutzer über das Azure-Portal einladen. Gastbenutzer können zu einem Verzeichnis, zu einer Gruppe oder zu einer Anwendung eingeladen werden. Nachdem Sie einen Benutzer mithilfe einer der verfügbaren Methoden eingeladen haben, wird das Konto des eingeladenen Benutzers in Azure Active Directory (Azure AD) mit dem Benutzertyp *Gast* hinzugefügt. Der Gastbenutzer muss daraufhin seine Einladung annehmen, um auf Ressourcen zugreifen zu können.

Nachdem Sie dem Verzeichnis einen Gastbenutzer hinzugefügt haben, können Sie dem Gastbenutzer einen direkten Link zu einer freigegebenen App senden, oder der Gastbenutzer kann auf die Einlösungs-URL in der Einladungs-E-Mail klicken. Weitere Informationen zum Annahmevorgang finden Sie unter [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md).

> [!IMPORTANT]
> Zum Hinzufügen der URL für die Datenschutzbestimmungen Ihres Unternehmens sollten Sie die Schritte unter [How-to: Add your organization's privacy info in Azure Active Directory](https://aka.ms/adprivacystatement) (Hinzufügen der Datenschutzinformationen Ihrer Organisation in Azure Active Directory) ausführen. Als Bestandteil der erstmaligen Annahme einer Einladung muss ein eingeladener Benutzer Ihren Datenschutzbestimmungen zustimmen, um fortfahren zu können. 

## <a name="add-guest-users-to-the-directory"></a>Hinzufügen von Gastbenutzern zum Verzeichnis

Gehen Sie wie folgt vor, um B2B-Zusammenarbeitsbenutzer dem Verzeichnis hinzuzufügen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Klicken Sie unter **Verwalten** auf **Benutzer und Gruppen** > **Alle Benutzer**.
4. Klicken Sie auf **Neuer Gastbenutzer**.

   ![Position von „Neuer Gastbenutzer“ auf der Benutzeroberfläche](./media/active-directory-b2b-admin-add-users/NewGuestUser-Directory.png) 
 
7. Geben Sie unter **Gast einladen** die E-Mail-Adresse des externen Benutzers ein. Geben Sie optional eine Begrüßungsnachricht ein. Beispiel: 

   ![Position von „Neuer Gastbenutzer“ auf der Benutzeroberfläche](./media/active-directory-b2b-admin-add-users/InviteGuest.png) 

8. Klicken Sie auf **Einladen**. Die Einladung wird daraufhin automatisch an den Gastbenutzer gesendet. Prüfen Sie den **Benachrichtigungsbereich** auf die Meldung **Benutzer erfolgreich eingeladen**. 
 
Nach dem Senden der Einladung wird das Benutzerkonto dem Verzeichnis automatisch als Gast hinzugefügt.


![B2B-Benutzer mit Benutzertyp „Gast“](./media/active-directory-b2b-admin-add-users/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Hinzufügen von Gastbenutzern zu einer Gruppe
Wenn Sie als Azure AD-Administrator einer Gruppe manuell B2B-Zusammenarbeitsbenutzer hinzufügen müssen, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Klicken Sie unter **Verwalten** auf **Benutzer und Gruppen** > **Alle Gruppen**.
4. Wählen Sie eine Gruppe aus, oder klicken Sie auf **Neue Gruppe**, um eine neue Gruppe zu erstellen. Es empfiehlt sich, in der Gruppenbeschreibung anzugeben, dass die Gruppe B2B-Gastbenutzer enthält.
5. Klicken Sie auf **Mitglieder** > **Mitglieder hinzufügen**. 
6. Führen Sie einen der folgenden Schritte aus:
   - Falls der Gastbenutzer bereits im Verzeichnis vorhanden ist, suchen Sie nach dem B2B-Benutzer. Wählen Sie den Benutzer aus, und klicken Sie auf **Auswählen**, um ihn der Gruppe hinzuzufügen.
   - Falls der Gastbenutzer noch nicht im Verzeichnis vorhanden ist, klicken Sie auf **Einladen**.
   ![Schaltfläche „Einladen“ zum Hinzufügen von Gastmitgliedern](./media/active-directory-b2b-admin-add-users/GroupInvite.png)
   
      Geben Sie unter **Gast einladen** die E-Mail-Adresse und optional eine persönliche Nachricht ein, und klicken Sie anschließend auf **Einladen**. Klicken Sie auf **Auswählen**, um den Benutzer der Gruppe hinzuzufügen.

      Die Einladung wird automatisch an den eingeladenen Benutzer gesendet. Prüfen Sie den **Benachrichtigungsbereich** auf eine Erfolgsmeldung für die **Benutzereinladung**. 

Für Azure AD B2B-Zusammenarbeit können auch dynamische Gruppen verwendet werden. Weitere Informationen finden Sie unter [Dynamische Gruppen und Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md).

## <a name="add-guest-users-to-an-application"></a>Hinzufügen von Gastbenutzern zu einer Anwendung

Gehen Sie als Azure AD-Administrator wie folgt vor, um B2B-Zusammenarbeitsbenutzer einer Anwendung hinzuzufügen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Klicken Sie unter **Verwalten** auf **Unternehmensanwendungen** > **Alle Anwendungen**.
4. Wählen Sie die Anwendung aus, der Sie Gastbenutzer hinzufügen möchten.
5. Klicken Sie unter **Verwalten** auf **Benutzer und Gruppen**.
6. Klicken Sie auf **Benutzer hinzufügen**.
7. Klicken Sie unter **Zuweisung hinzufügen** auf **Benutzer und Gruppen**.
8. Führen Sie einen der folgenden Schritte aus:
   - Falls der Gastbenutzer bereits im Verzeichnis vorhanden ist, suchen Sie nach dem B2B-Benutzer. Wählen Sie den Benutzer aus, und klicken Sie auf **Auswählen**, um ihn der App hinzuzufügen.
   - Falls der Gastbenutzer noch nicht im Verzeichnis vorhanden ist, klicken Sie auf **Einladen**.
   ![Schaltfläche „Einladen“ zum Hinzufügen von Gastmitgliedern](./media/active-directory-b2b-admin-add-users/AppInviteUsers.png)
   
      Geben Sie unter **Gast einladen** die E-Mail-Adresse und optional eine persönliche Nachricht ein, und klicken Sie anschließend auf **Einladen**. Klicken Sie auf **Auswählen**, um den Benutzer der App hinzuzufügen.

      Die Einladung wird automatisch an den eingeladenen Benutzer gesendet. Prüfen Sie den **Benachrichtigungsbereich** auf eine Erfolgsmeldung für die **Benutzereinladung**.

9. Klicken Sie unter **Zuweisung hinzufügen** auf **Rolle auswählen**, wählen Sie eine Rolle aus, die auf den ausgewählten Benutzer angewendet werden soll (sofern zutreffend), und klicken Sie anschließend auf **OK**.
10. Klicken Sie auf **Zuweisen**.
 
## <a name="resend-invitations-to-guest-users"></a>Erneutes Senden von Einladungen an Gastbenutzer

Wenn ein Gastbenutzer seine Einladung noch nicht angenommen hat, können Sie die Einladungs-E-Mail erneut senden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Klicken Sie unter **Verwalten** auf **Benutzer und Gruppen**.
4. Wählen Sie **Alle Benutzer**.
5. Wählen Sie das Benutzerkonto aus.
6. Klicken Sie unter **Verwalten** auf **Profil**.
7. Sofern der Benutzer die Einladung noch nicht angenommen hat, steht die Option **Einladung erneut senden** zur Verfügung. Klicken Sie auf diese Schaltfläche, um die Einladung erneut zu senden.

   ![Option „Einladung erneut senden“ im Benutzerprofil](./media/active-directory-b2b-admin-add-users/Resend-Invitation.png)

> [!NOTE]
> Wenn Sie eine Einladung erneut senden, in der der Benutzer ursprünglich zu einer bestimmten App weitergeleitet wurde, gelangt der Benutzer über den Link in der neuen Einladung stattdessen zur obersten Ebene des Zugriffsbereichs.

## <a name="next-steps"></a>Nächste Schritte

- Unter [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer zu Azure Active Directory hinzu?](active-directory-b2b-iw-add-users.md) erfahren Sie, wie Benutzer ohne Azure AD-Administratorrolle B2B-Gastbenutzer hinzufügen können.
- Informationen zur Einladungs-E-Mail finden Sie unter [Die Elemente der Einladungs-E-Mail für die B2B-Zusammenarbeit – Azure Active Directory](active-directory-b2b-invitation-email.md).


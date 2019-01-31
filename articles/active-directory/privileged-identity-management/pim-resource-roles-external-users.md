---
title: Einladen externer Benutzer und Zuweisen von Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure AD Privileged Identity Management (PIM) externe Benutzer einladen und Azure-Ressourcenrollen zuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: f3cfb792b9c9befcbc2ee869ef5b31903e5c10f9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188608"
---
# <a name="invite-external-users-and-assign-azure-resource-roles-in-pim"></a>Einladen externer Benutzer und Zuweisen von Azure-Ressourcenrollen in PIM

Bei Azure Active Directory (Azure AD) Business-to-Business (B2B) handelt es sich um eine Gruppe von Funktionen in Azure AD, über die Organisationen unter Verwendung eines beliebigen Kontos mit externen Benutzern und Anbietern zusammenarbeiten können. Wenn Sie B2B mit Azure AD Privileged Identity Management (PIM) kombinieren, gelten Ihre Konformitäts- und Governanceanforderungen weiterhin für externe Benutzer. Beispielsweise können Sie folgende PIM-Funktionen für Azure-Ressourcen für externe Benutzer verwenden:

- Zuweisen des Zugriffs auf bestimmte Azure-Ressourcen
- Aktivieren des Just-in-Time-Zugriffs
- Angeben der Zuweisungsdauer und des Enddatums
- Erzwingen von MFA für aktive Zuweisung oder Aktivierung
- Ausführen von Zugriffsüberprüfungen
- Verwenden von Warnungen und Überwachungsprotokollen

In diesem Artikel wird beschrieben, wie Sie mithilfe von PIM externe Benutzer in Ihr Verzeichnis einladen und ihren Zugriff auf Azure-Ressourcen verwalten.

## <a name="when-would-you-invite-external-users"></a>Wann laden Sie externe Benutzer ein?

Einige Beispielszenarien, in denen Sie möglicherweise externe Benutzer in Ihr Verzeichnis einladen:

- Zugriff für einen externen selbstständigen Anbieter, der nur über ein E-Mail-Konto verfügt, auf Ihre Azure-Ressourcen für ein Projekt
- Zugriff für einen externen Partner in einer großen Organisation, die lokale Active Directory-Verbunddienste (AD FS) verwendet, auf Ihre Kostenmanagementanwendung
- Temporärer Zugriff für Supporttechniker außerhalb Ihrer Organisation (z.B. Microsoft-Support) auf Ihre Azure-Ressource zur Behebung von Problemen

## <a name="how-does-external-collaboration-using-b2b-work"></a>Funktionsweise der externen Kollaboration über B2B

Bei Verwendung von B2B können Sie einen externen Benutzer in Ihr Verzeichnis einladen. Der externe Benutzer scheint in Ihrem Verzeichnis vorhanden zu sein, ihm sind jedoch keine Anmeldeinformationen zugeordnet. Wenn ein externer Benutzer authentifiziert werden soll, muss er in seinem Stammverzeichnis und nicht in Ihrem Verzeichnis authentifiziert werden. Das bedeutet, dass der externe Benutzer, wenn er keinen Zugriff mehr auf sein Stammverzeichnis hat, automatisch den Zugriff auf Ihr Verzeichnis verliert. Wenn der externe Benutzer beispielsweise seine Organisation verlässt, verliert er automatisch den Zugriff auf alle Ressourcen, die Sie in Ihrem Verzeichnis für ihn freigegeben haben, ohne dass Sie etwas tun müssen. Weitere Informationen zu B2B finden Sie unter [Was ist der Gastzugriff in Azure Active Directory-B2B?](../b2b/what-is-b2b.md)

![B2B und externer Benutzer](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-external-collaboration-settings"></a>Überprüfen der Einstellungen für externe Kollaboration

Um sicherzustellen, dass Sie externe Benutzer in Ihr Verzeichnis einladen können, sollten Sie Ihre Einstellungen für externe Kollaboration überprüfen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

1. Klicken Sie auf **Azure Active Directory** > **Benutzereinstellungen**.

1. Klicken Sie auf **Externe Einstellungen zur Kollaboration verwalten**.

    ![Einstellungen für externe Kollaboration](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Stellen Sie sicher, dass der Schalter **Administratoren und Benutzer mit der Rolle „Gasteinladender“ können einladen** auf **Ja** festgelegt ist.

## <a name="invite-an-external-user-and-assign-a-role"></a>Einladen eines externen Benutzers und Zuweisen einer Rolle

Mithilfe von PIM können Sie einen externen Benutzer einladen und ihn genau wie einen Mitgliedsbenutzer als berechtigt für eine Azure-Ressourcenrolle festlegen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) oder [Benutzerkontoadministrator](../users-groups-roles/directory-assign-admin-roles.md#user-account-administrator) ist.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

1. Verwenden Sie den **Ressourcenfilter** zum Filtern der Liste verwalteter Ressourcen.

1. Klicken Sie auf die Ressource, die Sie verwalten möchten, z.B. eine Ressource, eine Ressourcengruppe, ein Abonnement oder eine Verwaltungsgruppe.

    Sie sollten den Umfang ausschließlich auf die vom Benutzer benötigten Ressourcen beschränken.

1. Klicken Sie unter „Verwalten“ auf **Rollen**, um die Liste der Rollen für Azure-Ressourcen anzuzeigen.

    ![Azure-Ressourcenrollen](./media/pim-resource-roles-external-users/resources-roles.png)

1. Klicken Sie auf die minimale Rolle, die der Benutzer benötigt.

    ![Ausgewählte Rolle](./media/pim-resource-roles-external-users/selected-role.png)

1. Klicken Sie auf der Seite der Rolle auf **Mitglied hinzufügen**, um den Bereich „Neue Zuweisung“ zu öffnen.

1. Klicken Sie auf **Mitglied oder Gruppe auswählen**.

    ![Mitglied oder Gruppe auswählen](./media/pim-resource-roles-external-users/select-member-group.png)

1. Klicken Sie zum Einladen eines externen Benutzers auf **Einladen**.

    ![Gast einladen](./media/pim-resource-roles-external-users/invite-guest.png)

1. Nachdem Sie einen externen Benutzer angegeben haben, klicken Sie auf **Einladen**.

    Der externe Benutzer sollte als ausgewähltes Mitglied hinzugefügt werden.

1. Klicken Sie im Bereich „Mitglied oder Gruppe auswählen“ auf **Auswählen**.

1. Wählen Sie im Bereich „Mitgliedschaftseinstellungen“ den Zuweisungstyp und die Dauer aus.

    ![Mitgliedschaftseinstellungen](./media/pim-resource-roles-external-users/membership-settings.png)

1. Klicken Sie zum Abschließen der Zuweisung auf **Fertig** und dann auf **Hinzufügen**.

    Die Rollenzuweisung für den externen Benutzer wird in der Rollenliste angezeigt.

    ![Rollenzuweisung für externen Benutzer](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-an-external-user"></a>Aktivieren einer Rolle als externer Benutzer

Als externer Benutzer müssen Sie zunächst die Einladung in das Azure AD-Verzeichnis annehmen und möglicherweise Ihre Rolle aktivieren.

1. Öffnen Sie die E-Mail mit der Verzeichniseinladung. Die E-Mail sieht etwa wie folgt aus.

    ![E-Mail-Einladung](./media/pim-resource-roles-external-users/email-invite.png)

1. Klicken Sie auf den Link **Get Started** (Start) in der E-Mail.

1. Überprüfen Sie die Berechtigungen, und klicken Sie dann auf **Annehmen**.

    ![Berechtigungen überprüfen](./media/pim-resource-roles-external-users/invite-accept.png)

1. Möglicherweise werden Sie aufgefordert, den Nutzungsbedingungen zuzustimmen und anzugeben, ob Sie angemeldet bleiben möchten.

    Das Azure-Portal wird geöffnet. Wenn Sie nur für eine Rolle berechtigt sind, haben Sie keinen Zugriff auf Ressourcen.

1. Um Ihre Rolle zu aktivieren, öffnen Sie die E-Mail mit dem Link „Rolle aktivieren“. Die E-Mail sieht etwa wie folgt aus.

    ![E-Mail-Einladung](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Klicken Sie auf **Rolle aktivieren**, um Ihre berechtigten Rollen in PIM zu öffnen.

    ![Meine Rollen – Berechtigt](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Klicken Sie unter „Aktion“ auf den Link **Aktivieren**.

    Je nach den Rolleneinstellungen müssen Sie möglicherweise bestimmte Informationen angeben, um die Rolle zu aktivieren.

1. Nachdem Sie die Einstellungen für die Rolle angegeben haben, klicken Sie auf **Aktivieren**, um die Rolle zu aktivieren.

    ![Rolle aktivieren](./media/pim-resource-roles-external-users/activate-role.png)

    Sofern der Administrator Ihre Anforderung nicht genehmigen muss, sollten Sie nun Zugriff auf die angegebenen Ressourcen haben.

## <a name="view-activity-for-an-external-user"></a>Anzeigen der Aktivität für einen externen Benutzer

Genau wie ein Mitgliedsbenutzer können Sie Überwachungsprotokolle anzeigen, um die Aktivitäten von externen Benutzern nachzuverfolgen.

1. Öffnen Sie PIM als Administrator, und wählen Sie die freigegebene Ressource aus.

1. Klicken Sie auf **Ressourcenüberwachung**, um die Aktivität für diese Ressource anzuzeigen. Im folgenden Beispiel wird die Aktivität für eine Ressourcengruppe angezeigt.

    ![Ressourcenüberwachung](./media/pim-resource-roles-external-users/audit-resource.png)

1. Klicken Sie zum Anzeigen der Aktivität für den externen Benutzer auf **Azure Active Directory** > **Benutzer** > externer Benutzer.

1. Klicken Sie auf **Überwachungsprotokolle**, um die Überwachungsprotokolle für das Verzeichnis anzuzeigen. Bei Bedarf können Sie Filter angeben.

    ![Verzeichnisüberwachung](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)
- [Was ist der Gastzugriff in Azure Active Directory-B2B?](../b2b/what-is-b2b.md)

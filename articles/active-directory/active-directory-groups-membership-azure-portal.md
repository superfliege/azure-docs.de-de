---
title: "Verwalten der Gruppen, denen Ihre Gruppe angehört, in Azure Active Directory | Microsoft-Dokumentation"
description: "Gruppen können in Azure Active Directory andere Gruppen enthalten. Nachfolgend wird erläutert, wie Sie diese Mitgliedschaften verwalten."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 44911ec2c278f7af7b8ec4bd971bc97b342a8bf6
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Verwalten, welchen Gruppen eine Gruppe in Ihrem Azure Active Directory-Mandanten angehört
Gruppen können in Azure Active Directory andere Gruppen enthalten. Nachfolgend wird erläutert, wie Sie diese Mitgliedschaften verwalten.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Wie finde ich die Gruppen, in denen meine Gruppe Mitglied ist?
1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators für das Verzeichnis an.
2. Wählen Sie **Benutzer und Gruppen**.

   ![Abbildung zum Öffnen des Blatts „Benutzer und Gruppen“](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Wählen Sie **Alle Gruppen** aus.

   ![Abbildung zum Auswählen von Gruppen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Wählen Sie eine Gruppe aus.
2. Wählen Sie **Gruppenmitgliedschaften** aus.

   ![Abbildung zum Öffnen des Blatts „Gruppenmitgliedschaften“](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Um Ihre Gruppe als Mitglied einer anderen Gruppe hinzuzufügen, klicken Sie auf dem Blatt **Gruppe – Gruppenmitgliedschaften** auf den Befehl **Hinzufügen**.
2. Wählen Sie auf dem Blatt **Gruppe auswählen** eine Gruppe aus, und klicken Sie dann im unteren Bereich des Blatts auf den Befehl **Auswählen**. Sie können Ihre Gruppe in einem Arbeitsschritt immer nur einer Gruppe hinzufügen. Das Feld **Benutzer** filtert die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. In diesem Feld können keine Platzhalterzeichen verwendet werden.

   ![Hinzufügen einer Gruppenmitgliedschaft](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Um Ihre Gruppe als Mitglied einer anderen Gruppe zu entfernen, wählen Sie auf dem Blatt **Gruppe – Gruppenmitgliedschaften** eine Gruppe aus.
9. Wählen Sie den Befehl **Entfernen**, und bestätigen Sie Ihre Auswahl, wenn Sie dazu aufgefordert werden.

   ![Befehl zum Entfernen der Mitgliedschaft](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Klicken Sie auf **Speichern**, wenn Sie die Bearbeitung der Gruppenmitgliedschaften für Ihre Gruppe abgeschlossen haben.

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

---
title: Hinzufügen oder Entfernen von Gruppenbesitzern – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Anweisungen zum Hinzufügen oder Entfernen von Gruppenbesitzern in Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: 887f376eafadfb556ce69611195301b25b24bbd5
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104251"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Hinzufügen oder Entfernen von Gruppenbesitzern in Azure Active Directory
Azure AD-Gruppen (Azure Active Directory) befinden sich im Besitz von Gruppenbesitzern und werden von diesen verwaltet. Gruppenbesitzer werden von einem Ressourcenbesitzer (Administrator) zugewiesen, um eine Gruppe und deren Mitglieder zu verwalten. Die Gruppenbesitzer müssen der Gruppe nicht angehören. Nachdem ein Gruppenbesitzer zugewiesen wurde, können Besitzer nur durch einen Ressourcenbesitzer hinzugefügt oder entfernt werden.

In einigen Fällen können Sie sich als Administrator dafür entscheiden, keinen Gruppenbesitzer zuzuweisen. In diesem Fall werden Sie selbst der Gruppenbesitzer. Besitzer können ihrer Gruppe zudem weitere Besitzer zuweisen, sofern dies in den Gruppeneinstellungen nicht eingeschränkt wurde.

## <a name="add-an-owner-to-a-group"></a>Hinzufügen eines Besitzers zu einer Gruppe
Fügen Sie einer Gruppe mithilfe von Azure AD weitere Gruppenbesitzer hinzu.

### <a name="to-add-a-group-owner"></a>So fügen Sie einen Gruppenbesitzer hinzu
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Azure Active Directory**, klicken Sie auf **Gruppen**, und wählen Sie anschließend die Gruppe aus, der Sie einen Besitzer hinzufügen möchten. (In diesem Beispiel wird *MDM policy - West* verwendet.)

3. Klicken Sie auf der Übersichtsseite für **MDM policy - West** auf **Besitzer**.

    ![Übersichtsseite für „MDM policy - West“ mit hervorgehobener Option „Besitzer“](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. Klicken Sie auf der Seite **MDM policy - West - Besitzer** auf **Besitzer hinzufügen**, suchen Sie nach dem Benutzer, der als neuer Gruppenbesitzer fungieren soll, wählen Sie ihn aus, und klicken Sie anschließend auf **Auswählen**.

    ![Seite „MDM policy - West - Besitzer“ mit hervorgehobener Option „Besitzer hinzufügen“](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Wenn Sie nach dem Auswählen des neuen Besitzers die Seite **Besitzer** aktualisieren, sehen Sie, dass der Name der Liste mit den Besitzern hinzugefügt wurde.

## <a name="remove-an-owner-from-a-group"></a>Entfernen eines Besitzers aus einer Gruppe
Entfernen Sie einen Besitzer mithilfe von Azure AD aus einer Gruppe.

### <a name="to-remove-an-owner"></a>So entfernen Sie einen Besitzer
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Azure Active Directory** > **Gruppen** und anschließend die Gruppe aus, aus der Sie einen Besitzer löschen möchten. (In diesem Beispiel wird *MDM policy - West* (MDM-Richtlinie – Westen) verwendet.)

3. Klicken Sie auf der Übersichtsseite für **MDM policy - West** auf **Besitzer**.

    ![Übersichtsseite für „MDM policy - West“ mit hervorgehobener Option „Besitzer“](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. Wählen Sie auf der Seite **MDM policy - West - Besitzer** den Benutzer aus, den Sie als Gruppenbesitzer entfernen möchten, klicken Sie auf der Informationsseite des Benutzers auf **Entfernen**, und klicken Sie anschließend auf **Ja**, um Ihre Entscheidung zu bestätigen.

    ![Informationsseite des Benutzers mit hervorgehobener Option „Entfernen“](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Wenn Sie nach dem Entfernen des Besitzers zur Seite **Besitzer** zurückkehren, sehen Sie, dass der Name aus der Liste mit den Besitzern entfernt wurde.

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

- [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](../users-groups-roles/groups-settings-cmdlets.md)

- [Verwenden von Gruppen zum Zuweisen des Zugriffs auf eine integrierte SaaS-App](../users-groups-roles/groups-saasapps.md)

- [Integrieren lokaler Identitäten in Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

- [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](../users-groups-roles/groups-settings-v2-cmdlets.md)

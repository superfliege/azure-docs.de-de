---
title: Verwalten von Benutzern in Azure Blockchain Workbench
description: Erfahren Sie, wie Sie Benutzer in Azure Blockchain Workbench verwalten.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: c17a406a0fd6f1172599ac0379c6afc139403148
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Verwalten von Benutzern in Azure Blockchain Workbench

Azure Blockchain Workbench umfasst eine Benutzerverwaltung für Personen und Organisationen, die Teil Ihres Konsortiums sind.

## <a name="prerequisites"></a>Voraussetzungen

Eine Blockchain Workbench-Bereitstellung ist erforderlich. Weitere Informationen zu Details dieser Bereitstellung finden Sie unter [Azure Blockchain Workbench-Bereitstellung](blockchain-workbench-deploy.md).

## <a name="add-azure-ad-users"></a>Hinzufügen von Azure AD-Benutzern

Azure Blockchain Workbench verwendet Azure Active Directory (Azure AD) für Authentifizierung, Zugriffssteuerung und Rollen. Benutzer im Blockchain Workbench-Azure AD-Mandanten können sich authentifizieren und Blockchain Workbench nutzen. Fügen Sie der Anwendungsrolle „Administrator“ Benutzer, damit diese interagieren und Aktionen ausführen können.

Blockchain Workbench-Benutzer müssen im Azure AD-Mandanten vorhanden sein, bevor Sie ihnen Anwendungen und Rollen zuweisen können. Um Benutzer zu Azure AD hinzuzufügen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.  Wählen Sie in der oberen rechten Ecke Ihr Konto aus, und wechseln Sie zu dem Azure AD-Mandanten, der mit Blockchain Workbench verknüpft ist.
3.  Wählen Sie **Azure Active Directory > Benutzer** aus. Sie sehen eine Liste der Benutzer in Ihrem Verzeichnis.
4.  Um Benutzer zum Verzeichnis hinzuzufügen, wählen Sie **Neuer Benutzer** aus. Für externe Benutzer wählen Sie **Neuer Gastbenutzer** aus.

    ![Neuer Benutzer](media/blockchain-workbench-manage-users/add-ad-user.png)

5.  Füllen Sie die erforderlichen Felder für den neuen Benutzer aus. Klicken Sie auf **Erstellen**.

Weitere Details zur Verwaltung von Benutzern in Azure AD finden Sie in der [Azure AD](../active-directory/add-users-azure-active-directory.md)-Dokumentation.

## <a name="manage-blockchain-workbench-administrators"></a>Verwalten von Blockchain Workbench-Administratoren

Nachdem Benutzer in das Verzeichnis aufgenommen wurden, müssen Sie im nächsten Schritt auswählen, welche Benutzer die Blockchain Workbench-Administratoren sind. Benutzer in der Gruppe **Administratoren** sind der **Administratoranwendungsrolle** in Blockchain Workbench zugeordnet. Administratoren können Benutzer hinzufügen oder entfernen, Benutzer bestimmten Szenarien zuweisen und neue Anwendungen erstellen.

So fügen Sie Benutzer zur Gruppe **Administratoren** im Azure AD-Verzeichnis hinzu:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.  Vergewissern Sie sich, dass Sie sich im mit Blockchain Workbench verbundenen Azure AD-Mandanten befinden, indem Sie Ihr Konto in der oberen rechten Ecke auswählen.
3.  Wählen Sie **Azure Active Directory > Unternehmensanwendungen** aus.
4.  Wählen Sie die Azure AD-Clientanwendung für Blockchain Workbench aus.
    
    ![Alle Registrierungen von Unternehmensanwendungen](media/blockchain-workbench-manage-users/select-blockchain-client-app.png)

5.  Wählen Sie **Benutzer und Gruppen > Benutzer hinzufügen** aus.
6.  Wählen Sie unter **Zuweisung hinzufügen** die Option **Benutzer** aus. Wählen oder suchen Sie den Benutzer, den Sie als Administrator hinzufügen möchten. Klicken Sie auf **Auswählen**, wenn Sie fertig sind.

    ![Hinzufügen einer Zuweisung](media/blockchain-workbench-manage-users/add-user-assignment.png)

9.  Überprüfen Sie, ob **Rolle** auf **Administrator** festgelegt ist.
10. Wählen Sie **Zuweisen** aus. Die hinzugefügten Benutzer werden in der Liste mit der zugewiesenen Administratorrolle angezeigt.

    ![Client-App-Benutzer in Blockchain](media/blockchain-workbench-manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Verwalten von Blockchain Workbench-Mitgliedern

Verwenden Sie die Blockchain Workbench-Anwendung, um Benutzer und Organisationen zu verwalten, die Teil Ihres Konsortiums sind. Sie können Benutzer zu Anwendungen und Rollen hinzufügen oder entfernen.

[Öffnen Sie Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) in Ihrem Browser, und melden Sie sich als Administrator an.

![Blockchain Workbench](media/blockchain-workbench-manage-users/blockchain-workbench-applications.png)

### <a name="managing-members-in-applications"></a>Verwalten von Mitgliedern in Anwendungen

Jeder Anwendung werden Mitglieder hinzugefügt. Mitgliedern können eine oder mehrere Anwendungsrollen zugewiesen sein, um Verträge zu initiieren oder Aktionen auszuführen.

Um Mitglieder zu einer Anwendung hinzuzufügen, wählen Sie eine Anwendungskachel im Bereich **Anwendungen**.

![Auswählen einer Anwendung](media/blockchain-workbench-manage-users/blockchain-workbench-select-application.png)

> [!NOTE]
> Die Anzahl der Mitglieder, die der ausgewählten Anwendung zugewiesen sind, wird in der Mitgliederkachel angezeigt.

#### <a name="add-member-to-application"></a>Hinzufügen eines Mitglieds zur Anwendung

1. Wählen Sie die Mitgliederkachel, um eine Liste der aktuellen Mitglieder anzuzeigen.
2. Wählen Sie **Mitglieder hinzufügen** aus.

    ![Hinzufügen von Mitgliedern](media/blockchain-workbench-manage-users/application-add-members.png)

3. Suchen Sie nach dem Namen des Benutzers.  Es werden nur Azure AD-Benutzer aufgelistet, die im Blockchain Workbench-Mandanten vorhanden sind. Wenn der Benutzer nicht gefunden wird, müssen Sie [Azure AD-Benutzer hinzufügen](#add-azure-ad-users).

    ![Hinzufügen von Mitgliedern](media/blockchain-workbench-manage-users/find-user.png)

4. Wählen Sie in der Dropdownliste eine **Rolle** aus.

    ![Auswählen von Rollenmitgliedern](media/blockchain-workbench-manage-users/application-select-role.png)

5. Wählen Sie **Hinzufügen**, um das Mitglied mit der zugehörigen Rolle zur Anwendung hinzuzufügen.

#### <a name="remove-member-from-application"></a>Entfernen eines Mitglieds aus der Anwendung

1. Wählen Sie die Mitgliederkachel, um eine Liste der aktuellen Mitglieder anzuzeigen.
2. Wählen Sie für den Benutzer, den Sie entfernen möchten, in der Dropdownliste für Rolle die Option **Entfernen** aus.

    ![Entfernen eines Mitglieds](media/blockchain-workbench-manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Ändern oder Hinzufügen einer Rolle

1. Wählen Sie die Mitgliederkachel, um eine Liste der aktuellen Mitglieder anzuzeigen.
2. Klicken Sie für den Benutzer, den Sie ändern möchten, in die Dropdownliste, und wählen Sie die neue Rolle aus.

    ![Ändern einer Rolle](media/blockchain-workbench-manage-users/application-change-role.png)

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Blockchainanwendung in Azure Blockchain Workbench](blockchain-workbench-create-app.md)

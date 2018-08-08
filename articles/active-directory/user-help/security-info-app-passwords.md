---
title: Einrichten von App-Kennwörtern mit Sicherheitsinformationen – Azure Active Directory | Microsoft-Dokumentation
description: Richten Sie automatisch generierte Kennwörter (App-Kennwörter) für die Verwendung mit jeder Nicht-Browser-App separat von einem normalen Kennwort mit Sicherheitsinformationen ein.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 56d207dc47b9e055782568f1c4a4cedc4d19a288
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348017"
---
# <a name="manage-app-passwords-using-security-info-preview"></a>Verwalten von App-Kennwörtern mit Sicherheitsinformationen (Vorschau)

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Bestimmte Nicht-Browser-Apps wie Outlook 2010 unterstützen keine zweistufige Überprüfung. Aus diesem Unterstützungsmangel resultiert, dass die App bei zweistufiger Überprüfung nicht funktioniert. Um dieses Problem zu umgehen, können Sie ein automatisch generiertes Kennwort für die Verwendung mit jeder Nicht-Browser-App separat von Ihrem normalen Kennwort erstellen.

Wenn Sie App-Kennwörter verwenden, müssen Sie unbedingt Folgendes beachten:

- App-Kennwörter werden automatisch generiert und nur einmal pro Anwendung eingegeben.

- Pro Benutzer können maximal 40 Kennwörter festgelegt werden. Wenn Sie versuchen, eines über diesen Grenzwert hinaus zu erstellen, werden Sie aufgefordert, ein vorhandenes Kennwort zu löschen, bevor Sie ein neues erstellen dürfen.

- Verwenden Sie ein App-Kennwort pro Gerät, nicht pro App. Erstellen Sie z.B. ein einziges Kennwort für alle Apps auf Ihrem Laptop und dann ein weiteres einzelnes Kennwort für alle Apps auf Ihrem Desktop.

    >[!Note]
    >Office 2013-Clients (einschließlich Outlook) unterstützen neue Authentifizierungsprotokolle und können für die zweistufige Überprüfung verwendet werden. Diese Unterstützung bedeutet, dass Sie nach Aktivierung der zweistufigen Überprüfung keine App-Kennwörter für Office 2013-Clients mehr benötigen. Weitere Informationen finden Sie im Artikel [Funktionsweise der modernen Authentifizierung in Office 2013- und Office 2016-Client-Apps](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517).

## <a name="create-and-delete-app-passwords-using-security-info"></a>Erstellen und Löschen von App-Kennwörtern mit Sicherheitsinformationen

Wenn Sie die zweistufige Überprüfung mit Ihrem Geschäfts-, Schul- oder Unikonto verwenden, und Ihr Administrator die Sicherheitsinformationen aktiviert hat, können Sie Ihre App-Kennwörter über das „Meine Apps“-Portal erstellen und löschen.

Wenn Ihr Administrator die Sicherheitsinformationen nicht aktiviert hat, müssen Sie die Anweisungen und Informationen im Abschnitt [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](multi-factor-authentication-end-user-app-passwords.md) befolgen.

### <a name="to-create-app-passwords-using-the-my-apps-portal"></a>So erstellen Sie App-Kennwörter über das „Meine Apps“-Portal

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an.

2. Rufen Sie „myapps.microsoft.com“ auf, und klicken Sie oben rechts auf der Seite auf Ihren Namen und dann auf **Profil**.

3. Klicken Sie im Bereich **Konto verwalten** auf **Sicherheitsinformationen bearbeiten**.

    ![Profilbildschirm mit hervorgehobenem Link „Sicherheitsinformation bearbeiten“](media/security-info/security-info-profile.png)

4. Wählen Sie im Bildschirm **Schützen Sie Ihr Konto** die Option **Sicherheitsinformationen hinzufügen** aus.

    ![Sicherheitsinformationenbildschirm mit vorhandenen, bearbeitbaren Informationen](media/security-info/security-info-edit-add-info.png)

5. Wählen Sie im Bildschirm **Sicherheitsinformationen hinzufügen** die Option **App-Kennwort** aus.

6. Geben Sie im Bildschirm **App-Kennwort erstellen** einen Namen für Ihr App-Kennwort ein, und wählen Sie dann **Weiter** aus.

    ![Bildschirm zur Eingabe des Namens Ihres App-Kennworts](media/security-info/security-info-name-app-password.png)

7. Wählen Sie **Kopieren** aus, um das Kennwort in die Zwischenablage zu kopieren, und wählen Sie dann **Weiter** aus.

    ![Bildschirm mit App-Kennwort für das Kopieren](media/security-info/security-info-create-app-password.png)
    
8. Stellen Sie sicher, dass das App-Kennwort auf dem Bildschirm **Schützen Sie Ihr Konto** angezeigt wird.

    ![„Schützen Sie Ihr Konto“ mit App-Kennwort](media/security-info/security-info-keep-secure-app-password.png)

### <a name="to-delete-app-passwords-using-the-my-apps-portal"></a>So löschen Sie App-Kennwörter über das „Meine Apps“-Portal

1. Wählen Sie im Bildschirm **Schützen Sie Ihr Konto** die Option **X** neben dem zu löschenden App-Kennwort aus.

    ![„Schützen Sie Ihr Konto“, App-Kennwort löschen](media/security-info/security-info-keep-secure-delete-app-password.png)

2. Wählen Sie im Bildschirm **Anwendungskennwort löschen** die Option **Löschen** aus.

    ![Bildschirm „Anwendungskennwort löschen“](media/security-info/security-info-keep-secure-delete-app-password2.png)

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Ihre Sicherheitsinformationen aktualisieren müssen, befolgen Sie die Anweisungen im Artikel [Verwalten Ihrer Sicherheitsinformationen (Vorschau)](security-info-manage-settings.md).

- Weitere allgemeine Informationen über Sicherheitsinformationen und darüber, was Sie tun können, finden Sie in der [Übersicht über die Sicherheitsinformation (Vorschau)](user-help-security-info-overview.md). 
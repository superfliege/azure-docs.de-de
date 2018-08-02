---
title: Benutzerstatus in Microsoft Azure Multi-Factor Authentication
description: Hier erhalten Sie Informationen zu den verschiedenen Benutzerstatus in Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 6bd07439d4c6b1ccb5919fbfb286f714bac3b628
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158895"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer

Es gibt zwei Ansätze, um die zweistufige Überprüfung zu erzwingen. Die erste Option besteht darin, jeden einzelnen Benutzer für Azure Multi-Factor Authentication (MFA) zu aktivieren. Wenn Benutzer einzeln aktiviert werden, führen sie die zweistufige Überprüfung bei jeder Anmeldung durch (bis auf einige Ausnahmen, beispielsweise wenn sie sich von vertrauenswürdigen IP-Adressen aus anmelden oder wenn die Funktion _Gespeicherte Geräte_ aktiviert ist). Die zweite Möglichkeit besteht darin, eine Richtlinie für bedingten Zugriff einzurichten, die unter bestimmten Umständen eine zweistufige Überprüfung erfordert.

> [!TIP]
> Wählen Sie eine der folgenden Methoden, um die zweistufige Überprüfung zu erzwingen. Wählen Sie jedoch nicht beide Methoden aus. Durch die Aktivierung eines Benutzers für Azure Multi-Factor Authentication werden alle Richtlinien für bedingten Zugriff außer Kraft gesetzt.

## <a name="choose-how-to-enable"></a>Auswählen der Art und Weise der Aktivierung

**Aktiviert durch Ändern des Benutzerstatus**: Dies ist die herkömmliche Methode, die zweistufige Überprüfung zu erfordern, und wird in diesem Artikel erläutert. Dies funktioniert sowohl für Azure MFA in der Cloud als auch für Azure MFA Server. Diese Methode erfordert, dass Benutzer **jedes Mal**, wenn sie sich anmelden, die zweistufige Überprüfung durchführen, und überschreibt Richtlinien für bedingten Zugriff.

Aktiviert durch Richtlinie für bedingten Zugriff: Dies ist die flexibelste Möglichkeit, die zweistufige Überprüfung für Ihre Benutzer zu aktivieren. Die Aktivierung mit der Richtlinie für bedingten Zugriff funktioniert jedoch nur bei Azure MFA in der Cloud und ist eine Premium-Funktion von Azure AD. Weitere Informationen zu dieser Methode finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](howto-mfa-getstarted.md).

Aktiviert durch Azure AD Identity Protection: Diese Methode nutzt die Risikorichtlinie von Azure AD Identity Protection, um die zweistufige Überprüfung ausschließlich auf Basis des Anmelderisikos für alle Cloudanwendungen zu erzwingen. Diese Methode erfordert die Azure Active Directory P2-Lizenzierung. Weitere Informationen zu dieser Methode finden Sie unter [Azure Active Directory Identity Protection](../active-directory-identityprotection.md#risky-sign-ins).

> [!Note]
> Weitere Informationen zu Lizenzen und Preisen finden Sie auf den Preisgestaltungsseiten von [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) und [mehrstufiger Authentifizierung](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Aktivieren von Azure MFA durch Ändern des Benutzerstatus

Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

| Status | BESCHREIBUNG | Nicht-Browser-Apps betroffen | Browser-Apps betroffen | Moderne Authentifizierung betroffen |
|:---:|:---:|:---:|:--:|:--:|
| Deaktiviert |Der Standardstatus eines neuen Benutzers, der nicht für Azure MFA registriert ist. |Nein  |Nein  |Nein  |
| Aktiviert |Der Prozess der Registrierung für Azure MFA für den Benutzer wurde begonnen, aber noch nicht abgeschlossen. Der Benutzer wird aufgefordert, sich bei der nächsten Anmeldung zu registrieren. |Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. | Ja. Nachdem die Sitzung abläuft, ist eine Azure MFA-Registrierung erforderlich.| Ja. Nachdem das Zugriffstoken abläuft, ist eine Azure MFA-Registrierung erforderlich. |
| Erzwungen |Der Benutzer wurde registriert und hat den Registrierungsprozess für Azure MFA abgeschlossen. |Ja. Für Apps sind App-Kennwörter erforderlich. |Ja. Azure MFA ist bei der Anmeldung erforderlich. | Ja. Azure MFA ist bei der Anmeldung erforderlich. |

Der Status eines Benutzers gibt an, ob ein Administrator den Benutzer für Azure MFA registriert hat und ob der Registrierungsprozess abgeschlossen ist.

Der Anfangsstatus aller Benutzer lautet *Deaktiviert*. Wenn Sie Benutzer für Azure MFA registrieren, ändert sich der Status in *Aktiviert*. Wenn aktivierte Benutzer sich anmelden und den Registrierungsprozess abschließen, ändert sich ihr Status in *Erzwungen*.  

### <a name="view-the-status-for-a-user"></a>Anzeigen des Status eines Benutzers

Führen Sie die folgenden Schritte aus, um auf die Seite zuzugreifen, auf der Sie Benutzerstatus anzeigen und verwalten können:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer**.
3. Wählen Sie **Multi-Factor Authentication** aus.
   ![Auswählen von Multi-Factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Es wird eine neue Seite geöffnet, auf der die Benutzerstatus angezeigt werden.
   ![Azure Multi-Factor Authentication-Benutzerstatus – Screenshot](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Ändern des Status eines Benutzers

1. Führen Sie die vorstehenden Schritte aus, um zur Seite mit den Azure Multi-Factor Authentication-**Benutzern** zu gelangen.
2. Suchen Sie den Benutzer, den Sie für Azure MFA aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern.
   ![Benutzer suchen – Screenshot](./media/howto-mfa-userstates/enable1.png)
3. Aktivieren Sie das Kontrollkästchen neben dem gewünschten Namen.
4. Wählen Sie auf der rechten Seite unter **QuickSteps** die Option **Aktivieren** oder **Deaktivieren** aus.
   ![Ausgewählten Benutzer aktivieren – Screenshot](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > Wenn sich Benutzer für Azure MFA registrieren, wird der Benutzerstatus *Aktiviert* automatisch in *Erzwungen* geändert. Ändern Sie den Benutzerstatus keinesfalls manuell in *Erzwungen*.

5. Bestätigen Sie Ihre Auswahl im Popupfenster, das geöffnet wird.

Benachrichtigen Sie die Benutzer per E-Mail, nachdem Sie die Benutzer aktiviert haben. Teilen Sie ihnen mit, dass sie zur Registrierung aufgefordert werden, wenn sie sich beim nächsten Mal anmelden. Und wenn Ihre Organisation auch nicht auf Browsern basierende Apps verwendet, die die moderne Authentifizierung nicht unterstützen, müssen die Benutzer App-Kennwörter erstellen. Sie können auch einen Link zum [Azure MFA-Leitfaden für Endbenutzer](../user-help/multi-factor-authentication-end-user.md) in die E-Mail einfügen, um den Benutzern den Einstieg zu erleichtern.

### <a name="use-powershell"></a>Verwenden von PowerShell

Zum Ändern des Benutzerstatus mit [Azure AD PowerShell](/powershell/azure/overview) ändern Sie `$st.State`. Es gibt drei mögliche Status:

* Aktiviert
* Erzwungen
* Deaktiviert  

Ändern Sie nicht direkt den Status *Erzwungen* für Benutzer. Wenn Sie dies tun, funktionieren nicht auf Browsern basierende Apps nicht mehr, weil der Benutzer die Azure MFA-Registrierung nicht durchlaufen und ein [App-Kennwort](howto-mfa-mfasettings.md#app-passwords) erhalten hat.

PowerShell ist eine gute Wahl, wenn Sie Benutzer massenweise aktivieren müssen. Erstellen Sie ein PowerShell-Skript, das eine Schleife durch eine Liste von Benutzern durchführt und sie aktiviert:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Das folgende Skript ist ein Beispiel:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="next-steps"></a>Nächste Schritte

Wie Sie zusätzliche Einstellungen wie vertrauenswürdige IP-Adressen, benutzerdefinierte Sprachnachrichten und Betrugswarnungen konfigurieren, erfahren Sie im Artikel [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md).

Informationen zum Verwalten von Benutzereinstellungen für die Microsoft Azure Multi-Factor Authentication finden Sie im Artikel [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](howto-mfa-userdevicesettings.md).

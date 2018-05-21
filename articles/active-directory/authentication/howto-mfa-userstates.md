---
title: Benutzerstatus in Microsoft Azure Multi-Factor Authentication
description: Hier erhalten Sie Informationen zu den verschiedenen Benutzerstatus in Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: b809097e50a17178da12fdb424eba08dc8e0c4cb
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer oder eine Gruppe

Es gibt zwei Ansätze, um die zweistufige Überprüfung zu erzwingen. Die erste Option besteht darin, jeden einzelnen Benutzer für Azure Multi-Factor Authentication (MFA) zu aktivieren. Wenn Benutzer einzeln aktiviert werden, führen sie die zweistufige Überprüfung bei jeder Anmeldung durch (bis auf einige Ausnahmen, beispielsweise wenn sie sich von vertrauenswürdigen IP-Adressen aus anmelden oder wenn die Funktion _Gespeicherte Geräte_ aktiviert ist). Die zweite Möglichkeit besteht darin, eine Richtlinie für bedingten Zugriff einzurichten, die unter bestimmten Umständen eine zweistufige Überprüfung erfordert.

>[!TIP] 
>Wählen Sie eine der folgenden Methoden, um die zweistufige Überprüfung zu erzwingen. Wählen Sie jedoch nicht beide Methoden aus. Durch die Aktivierung eines Benutzers für Azure Multi-Factor Authentication werden alle Richtlinien für bedingten Zugriff außer Kraft gesetzt.

## <a name="which-option-is-right-for-you"></a>Welche Option ist die richtige für Sie?

Die **Aktivierung von Azure Multi-Factor Authentication durch Ändern der Benutzerstatus** ist der herkömmliche Ansatz, um die zweistufige Überprüfung zu erzwingen. Dies funktioniert sowohl für Azure MFA in der Cloud als auch für Azure MFA Server. Alle aktivierten Benutzer führen die zweistufige Überprüfung bei jeder Anmeldung aus. Durch die Aktivierung eines Benutzers werden alle Richtlinien für bedingten Zugriff außer Kraft gesetzt, die diesen Benutzer betreffen könnten. 

Die **Aktivierung von Azure Multi-Factor Authentication mit einer Richtlinie für bedingten Zugriff** ist ein flexiblerer Ansatz, um die zweistufige Überprüfung zu erzwingen. Er funktioniert jedoch nur bei Azure MFA in der Cloud. Zudem ist der _bedingte Zugriff_ eine [kostenpflichtige Funktion von Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Sie können Richtlinien für bedingten Zugriff erstellen, die sowohl für Gruppen als auch für einzelne Benutzer gelten. Gruppen, bei denen ein hohes Risiko besteht, können umfassendere Einschränkungen zugewiesen werden. Alternativ kann die zweistufige Überprüfung nur für Cloud-Apps mit hohem Risiko erzwungen und für Apps mit niedrigem Risiko übersprungen werden. 

Bei beiden Optionen werden Benutzer aufgefordert, sich für Azure Multi-Factor Authentication zu registrieren, wenn sie sich zum ersten Mal nach der Aktivierung der Anforderungen anmelden. Beide Optionen funktionieren auch mit den konfigurierbaren [Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Aktivieren von Azure MFA durch Ändern des Benutzerstatus

Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

| Status | BESCHREIBUNG | Nicht-Browser-Apps betroffen | Browser-Apps betroffen | Moderne Authentifizierung betroffen |
|:---:|:---:|:---:|:--:|:--:|
| Deaktiviert |Der Standardstatus eines neuen Benutzers, der nicht für Azure MFA registriert ist. |Nein  |Nein  |Nein  |
| Aktiviert |Der Prozess der Registrierung für Azure MFA für den Benutzer wurde begonnen, aber noch nicht abgeschlossen. Der Benutzer wird aufgefordert, sich bei der nächsten Anmeldung zu registrieren. |Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. | Ja. Nachdem die Sitzung abläuft, ist eine Azure MFA-Registrierung erforderlich.| Ja. Nachdem das Zugriffstoken abläuft, ist eine Azure MFA-Registrierung erforderlich. |
| Erzwungen |Der Benutzer wurde registriert und hat den Registrierungsprozess für Azure MFA abgeschlossen. |Ja.  Für Apps sind App-Kennwörter erforderlich. |Ja. Azure MFA ist bei der Anmeldung erforderlich. | Ja. Azure MFA ist bei der Anmeldung erforderlich. |

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

   >[!TIP]
   >Wenn sich Benutzer für Azure MFA registrieren, wird der Benutzerstatus *Aktiviert* automatisch in *Erzwungen* geändert. Ändern Sie den Benutzerstatus keinesfalls manuell in *Erzwungen*. 

5. Bestätigen Sie Ihre Auswahl im Popupfenster, das geöffnet wird. 

Benachrichtigen Sie die Benutzer per E-Mail, nachdem Sie die Benutzer aktiviert haben. Teilen Sie ihnen mit, dass sie zur Registrierung aufgefordert werden, wenn sie sich beim nächsten Mal anmelden. Und wenn Ihre Organisation auch nicht auf Browsern basierende Apps verwendet, die die moderne Authentifizierung nicht unterstützen, müssen die Benutzer App-Kennwörter erstellen. Sie können auch einen Link zum [Azure MFA-Leitfaden für Endbenutzer](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md) in die E-Mail einfügen, um den Benutzern den Einstieg zu erleichtern.

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

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Aktivieren von Azure MFA mit einer Richtlinie für bedingten Zugriff

Der _bedingte Zugriff_ ist eine kostenpflichtige Funktion von Azure Active Directory mit zahlreichen Konfigurationsoptionen. In den folgenden Schritten werden Sie durch eine Methode zum Erstellen einer Richtlinie geführt. Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wählen Sie **Azure Active Directory** > **Bedingter Zugriff**.
3. Wählen Sie **Neue Richtlinie**.
4. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**. Verwenden Sie die Registerkarten **Einschließen** und **Ausschließen**, um anzugeben, welche Benutzer und Gruppen durch die Richtlinie verwaltet werden.
5. Klicken Sie unter **Zuweisungen** auf **Cloud-Apps**. Wählen Sie **Alle Cloud-Apps**, um alle einzuschließen.
6. Klicken Sie unter **Zugriffskontrollen** auf **Gewähren**. Wählen Sie **Mehrstufige Authentifizierung anfordern**.
7. Legen Sie **Richtlinie aktivieren** auf **Ein** fest, und wählen Sie dann **Speichern** aus.

Mit den anderen Optionen in der Richtlinie für bedingten Zugriff können Sie genau angeben, wann die zweistufige Überprüfung erforderlich ist. Beispielsweise können Sie eine Richtlinie für den folgenden Fall erstellen: Wenn Auftragnehmer von nicht vertrauenswürdigen Netzwerken aus über Geräte, die nicht in Domänen eingebunden sind, auf unsere Beschaffungs-App zugreifen, müssen sie die zweistufige Überprüfung durchführen. 

## <a name="next-steps"></a>Nächste Schritte

- Erhalten Sie Tipps zu [Best Practices für den bedingten Zugriff](../active-directory-conditional-access-best-practices.md).

- Verwalten Sie Azure Multi-Factor Authentication-Einstellungen für [Ihre Benutzer und deren Geräte](howto-mfa-userdevicesettings.md).

---
title: "Azure AD Connect: Nahtloses einmaliges Anmelden – Schnellstart | Microsoft-Dokumentation"
description: In diesem Artikel wird beschrieben, wie Sie Ihre ersten Schritte mit dem nahtlosen einmaligen Anmelden von Azure Active Directory gehen.
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 8975a82c5573cc0c284e1fc76cd0ef2c19fbbd72
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure AD Connect: Nahtloses einmaliges Anmelden – Schnellstart

## <a name="how-to-deploy-seamless-sso"></a>Bereitstellen der nahtlosen einmaligen Anmeldung (SSO)

Mit dem nahtlosen einmaligen Anmelden von Azure Active Directory (Azure AD Seamless Single Sign-On) werden Benutzer automatisch im Unternehmensnetzwerk angemeldet, wenn Sie an ihren Unternehmens-Desktops arbeiten. Dieses Feature ermöglicht Ihren Benutzern einen einfachen Zugriff auf Ihre cloudbasierten Dienste, ohne dass zusätzliche lokale Komponenten erforderlich sind.

Um die nahtlose einmalige Anmeldung bereitzustellen, führen Sie die folgenden Schritte aus:

## <a name="step-1-check-prerequisites"></a>Schritt 1: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

1. Richten Sie Ihren Azure AD Connect-Server ein: Wenn Sie die [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) als Anmeldemethode verwenden, ist keine zusätzliche Überprüfung der Voraussetzungen erforderlich. Wenn die [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) Ihre Anmeldemethode ist und eine Firewall zwischen Azure AD Connect und Azure AD vorhanden ist, sollten Sie Folgendes sicherstellen:
- Sie verwenden die Version 1.1.644.0 oder höher von Azure AD Connect. 
- Wenn Ihre Firewall oder ihr Proxy DNS-Whitelisting zulässt, beschränken Sie Verbindungen auf URLs von **\*.msappproxy.net** über den Port 443 mittels Whitelist. Ist dies nicht der Fall, aktivieren Sie den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden. Diese Voraussetzung gilt nur, wenn Sie das Feature aktivieren, nicht für tatsächliche Benutzeranmeldungen.

    >[!NOTE]
    >Die Azure AD Connect-Versionen 1.1.557.0, 1.1.558.0, 1.1.561.0 und 1.1.614.0 weisen ein Problem in Bezug auf die Kennworthashsynchronisierung auf. Wenn Sie die Kennworthashsynchronisierung _nicht_ zusammen mit der Passthrough-Authentifizierung verwenden möchten, finden Sie weitere Informationen dazu in den [Versionshinweisen zu Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

2. Für jede AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie die nahtlose einmalige Anmeldung aktivieren möchten, benötigen Sie die Anmeldeinformationen des Domänenadministrators.

## <a name="step-2-enable-the-feature"></a>Schritt 2: Aktivieren des Features

Das nahtlose einmalige Anmelden kann mithilfe von [Azure AD Connect](active-directory-aadconnect.md) aktiviert werden.

Wenn Sie Azure AD Connect neu installieren, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) aus. Aktivieren Sie auf der Seite „Benutzeranmeldung“ die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso8.png)

Wenn Sie bereits eine Installation von Azure AD Connect haben, wählen Sie in Azure AD Connect „Change user sign-in page“ (Anmeldeseite „Benutzer wechseln“), und klicken Sie auf „Weiter“. Aktivieren Sie dann die Option „Einmaliges Anmelden aktivieren“.

![Azure AD Connect – Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fahren Sie mit dem Assistenten fort, bis Sie zur Seite „Enable single sign on“ (Einmaliges Anmelden aktivieren) gelangen. Stellen Sie für jede AD-Gesamtstruktur, die Sie mit Azure AD (über Azure AD Connect) synchronisieren und für deren Benutzer Sie die nahtlose einmalige Anmeldung aktivieren möchten, die Anmeldeinformationen des Domänenadministrators bereit. 

Nach Abschluss des Assistenten ist das nahtlose einmalige Anmelden für Ihren Mandanten aktiviert.

>[!NOTE]
> Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert, werden aber für die Aktivierung des Features verwendet.

Befolgen Sie diese Anweisungen, um zu überprüfen, ob die nahtlose SSO ordnungsgemäß aktiviert ist:

1. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an.
2. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.
3. Wählen Sie **Azure AD Connect** aus.
4. Überprüfen Sie, dass die Funktion **Nahtloses einmaliges Anmelden** **aktiviert** ist.

![Azure-Portal – Blatt „Azure AD Connect“](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Schritt 3: Ausrollen des Features

Zum Durchführen des Rollouts für das Feature müssen Sie den Intranetzoneneinstellungen Ihrer Benutzer mithilfe der Gruppenrichtlinie in Active Directory die folgenden Azure AD-URLs hinzufügen:

- https://autologon.microsoftazuread-sso.com
- https://aadg.windows.net.nsatc.net

Darüber hinaus müssen Sie eine Richtlinieneinstellung für eine Intranetzone (mithilfe der Gruppenrichtlinie) namens „Aktualisierungen der Statusleiste per Skript zulassen“ aktivieren.

>[!NOTE]
> Die folgenden Anweisungen funktionieren nur für Internet Explorer und Google Chrome unter Windows (wenn ein Satz von URLs vertrauenswürdiger Websites wie für Internet Explorer freigegeben wird). Lesen Sie im nächsten Abschnitt die Anweisungen zum Einrichten von Mozilla Firefox und Google Chrome auf Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Warum müssen Sie Einstellungen von Benutzern für Intranetzonen ändern?

Der Browser berechnet standardmäßig anhand der URL automatisch die richtige Zone (Internet oder Intranet). Beispielsweise ist http://contoso/ der Intranetzone und http://intranet.contoso.com/ der Internetzone zugeordnet (da die URL einen Punkt enthält). Browser senden keine Kerberos-Tickets an Cloudendpunkte wie die beiden Azure AD-URLs, sofern die jeweilige URL nicht als Teil der Intranetzone des Browsers definiert ist.

### <a name="detailed-steps"></a>Ausführliche Schritte

1. Öffnen Sie das Gruppenrichtlinienverwaltungstool.
2. Bearbeiten Sie die Gruppenrichtlinie, die auf einige oder alle Benutzer angewendet wird. In diesem Beispiel verwenden wir die **Default Domain Policy** (Standarddomänenrichtlinie).
3. Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite**, und wählen Sie die Option **Liste der Site zu Zonenzuweisungen**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso6.png)
4. Aktivieren Sie die Richtlinie, und geben Sie in das Dialogfeld die folgenden Werte (Azure AD-URLs, in denen Kerberos-Tickets weitergeleitet werden) und Daten ein (*1* gibt die Intranetzone an).

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Wenn Sie das nahtlose einmalige Anmelden z.B. für einige Benutzer verbieten möchten, weil sich diese Benutzer beispielsweise auf freigegebenen Kiosken anmelden, legen Sie die vorherigen Werte auf *4* fest. Diese Aktion fügt der Zone eingeschränkter Sites die Azure AD-URLs hinzu, und löst für das nahtlose einmalige Anmelden ständig einen Fehler aus.

5. Klicken **Sie** zweimal nacheinander auf **OK**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso7.png)
6. Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite\Intranetzone**, und wählen Sie die Option **Aktualisierungen der Statusleiste per Skript zulassen** aus.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso11.png)
7. Aktivieren Sie die Richtlinieneinstellung, und klicken Sie auf **OK**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Überlegungen zum Browser

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox führt nicht automatisch die Kerberos-Authentifizierung aus. Jeder Benutzer muss den Firefox-Einstellungen manuell die Azure AD-URLs mithilfe der folgenden Schritte hinzufügen:
1. Führen Sie Firefox aus, und geben Sie in die Adressleiste `about:config` ein. Schließen Sie alle Benachrichtigungen, die Sie sehen.
2. Suchen Sie nach der Einstellung **network.negotiate-auth.trusted-uris**. In dieser Einstellung werden in Firefox die vertrauenswürdigen Sites für die Kerberos-Authentifizierung aufgeführt.
3. Klicken Sie mit der rechten Maustaste, und wählen Sie dann „Ändern“ aus.
4. Geben Sie „https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net“ in das Feld ein.
5. Klicken Sie auf „OK“, und öffnen Sie den Browser erneut.

#### <a name="safari-on-mac-os"></a>Safari unter Mac OS

Stellen Sie sicher, dass der Computer mit Mac OS in AD eingebunden ist. Eine entsprechende Anleitung finden Sie [hier](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome unter Mac OS

Informationen dazu, wie Sie in Google Chrome unter Mac OS und anderen nicht Windows-basierten Plattformen die Azure AD-URLs für die integrierte Authentifizierung auf eine Whitelist setzen, finden Sie in [diesem Artikel](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist).

Das Ausrollen der Azure AD-URLs für Firefox und Google Chrome unter macOS mithilfe von Active Directory-Gruppenrichtlinienerweiterungen von Drittanbietern kann in diesem Artikel nicht behandelt werden.

#### <a name="known-browser-limitations"></a>Bekannte Browsereinschränkungen

Das nahtlose einmalige Anmelden funktioniert in Firefox- und Edge-Browsern nicht im privaten Modus. Dies gilt auch für Internet Explorer, wenn der Browser im erweiterten Schutzmodus ausgeführt wird.

>[!IMPORTANT]
>Wir haben vor Kurzem ein Rollback für den Support für Edge ausgeführt, um von Kunden gemeldete Probleme zu untersuchen.

## <a name="step-4-test-the-feature"></a>Schritt 4: Testen des Features

Um das Feature für einen bestimmten Benutzer zu testen, stellen Sie sicher, dass _alle_ folgenden Bedingungen erfüllt werden:
  - Ihr Benutzer meldet sich auf einem Gerät des Unternehmens an.
  - Das Gerät ist zuvor Ihrer Active Directory(AD)-Domäne beigetreten.
  - Es muss eine direkte Verbindung zwischen dem Gerät und Ihrem Domänencontroller (DC) bestehen, entweder über das Unternehmensnetzwerk (Kabel- oder Funknetzwerk) oder per Remotezugriff, z.B. über eine VPN-Verbindung.
  - Sie haben [das Feature für diesen Benutzer mithilfe von Gruppenrichtlinien ausgerollt](##step-3-roll-out-the-feature).

So testen Sie das Szenario, wenn der Benutzer nur den Benutzernamen eingibt, jedoch kein Kennwort:
- Melden Sie sich unter *https://myapps.microsoft.com/* in einer neuen, privaten Browsersitzung an.

So testen Sie das Szenario, wenn der Benutzer weder den Benutzernamen noch das Kennwort eingeben muss: 
- Melden Sie sich unter *https://myapps.microsoft.com/contoso.onmicrosoft.com* in einer neuen, privaten Browsersitzung an. Ersetzen Sie „*contoso*“ durch den Namen Ihres Mandanten.
- Melden Sie sich alternativ unter *https://myapps.microsoft.com/contoso.com* in einer neuen, privaten Browsersitzung an. Ersetzen Sie „*contoso.com*“ durch eine überprüfte Domäne (keine Verbunddomäne) in Ihrem Mandanten.

## <a name="step-5-roll-over-keys"></a>Schritt 5: Durchführen des Rollovers für Schlüssel

In Schritt 2 erstellt Azure AD Connect Computerkonten (die Azure AD repräsentieren) in allen AD-Gesamtstrukturen, für die Sie das nahtlose einmalige Anmelden aktiviert haben. Ausführliche Informationen hierzu finden Sie [hier](active-directory-aadconnect-sso-how-it-works.md). Zur Erhöhung der Sicherheit wird empfohlen, für die Kerberos-Entschlüsselungsschlüssel dieser Computerkonten regelmäßig ein Rollover durchzuführen. Die Anweisungen für das Rollover finden Sie [hier](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Sie müssen diesen Schritt nicht _sofort_ nach der Aktivierung des Features ausführen. Führen Sie für die Kerberos-Entschlüsselungsschlüssel mindestens alle 30 Tage ein Rollover durch.

## <a name="next-steps"></a>Nächste Schritte

- [Technische Einzelheiten](active-directory-aadconnect-sso-how-it-works.md): Funktionsweise dieses Features verstehen
- [Häufig gestellte Fragen](active-directory-aadconnect-sso-faq.md): Antworten auf häufig gestellte Fragen
- [Problembehandlung](active-directory-aadconnect-troubleshoot-sso.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Verfassen neuer Featureanforderungen

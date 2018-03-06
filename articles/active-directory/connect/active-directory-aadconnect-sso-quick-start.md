---
title: "Azure AD Connect: Nahtloses einmaliges Anmelden – Schnellstart | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie Ihre ersten Schritte für das nahtlose einmalige Anmelden von Azure Active Directory ausführen."
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: billmath
ms.openlocfilehash: 58ca992f9fcf9a03d917f0dc250a292c4d5f49e5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure AD Connect: Nahtloses einmaliges Anmelden – Schnellstart

## <a name="deploy-seamless-single-sign-on"></a>Bereitstellen des nahtlosen einmaligen Anmeldens

Mit dem nahtlosen einmaligen Anmelden von Azure Active Directory (Azure AD Seamless Single Sign-On) werden Benutzer automatisch angemeldet, wenn sie an ihren mit dem Unternehmensnetzwerk verbundenen Unternehmens-Desktops arbeiten. Nahtloses SSO ermöglicht Ihren Benutzern einen einfachen Zugriff auf Ihre cloudbasierten Anwendungen, ohne dass zusätzliche lokale Komponenten erforderlich sind.

Um die nahtlose einmalige Anmeldung bereitzustellen, führen Sie die folgenden Schritte aus:

## <a name="step-1-check-the-prerequisites"></a>Schritt 1: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

* **Richten Sie Ihren Azure AD Connect-Server ein:** Wenn Sie die [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) als Anmeldemethode verwenden, ist keine zusätzliche Überprüfung der Voraussetzungen erforderlich. Wenn die [Kennworthashsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) Ihre Anmeldemethode ist und eine Firewall zwischen Azure AD Connect und Azure AD vorhanden ist, sollten Sie Folgendes sicherstellen:
   - Sie verwenden Azure AD Connect 1.1.644.0 oder eine höhere Version. 
   - Wenn Ihre Firewall oder ihr Proxy DNS-Whitelisting zulässt, beschränken Sie mittels Whitelist Verbindungen mit URLs von **\*.msappproxy.net** über den Port 443. Aktivieren Sie andernfalls den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden. Diese Voraussetzung gilt nur, wenn Sie das Feature aktivieren. Sie ist für tatsächliche Benutzeranmeldungen nicht erforderlich.

    >[!NOTE]
    >Die Azure AD Connect-Versionen 1.1.557.0, 1.1.558.0, 1.1.561.0 und 1.1.614.0 weisen ein Problem in Bezug auf die Kennworthashsynchronisierung auf. Wenn Sie die Kennworthashsynchronisierung _nicht_ zusammen mit der Passthrough-Authentifizierung verwenden möchten, finden Sie weitere Informationen dazu in den [Versionshinweisen zu Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

* **Richten Sie Anmeldeinformationen des Domänenadministrators ein:**: Sie benötigen Anmeldeinformationen des Domänenadministrators für jede Active Directory-Gesamtstruktur, die:
    * Sie über Azure AD Connect mit Azure AD synchronisieren.
    * Benutzer enthält, für die Sie nahtloses SSO aktivieren möchten.

## <a name="step-2-enable-the-feature"></a>Schritt 2: Aktivieren des Features

Aktivieren Sie nahtloses SSO über [Azure AD Connect](active-directory-aadconnect.md).

Wenn Sie Azure AD Connect neu installieren, wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md) aus. Aktivieren Sie auf der Seite **Benutzeranmeldung** die Option **Einmaliges Anmelden aktivieren**.

![Azure AD Connect: Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso8.png)

Wenn Sie bereits eine Installation von Azure AD Connect haben, wählen Sie in Azure AD Connect die Seite **Benutzeranmeldung ändern**, und klicken Sie auf **Weiter**.

![Azure AD Connect: Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fahren Sie mit dem Assistenten fort, bis Sie zur Seite **Einmaliges Anmelden aktivieren** gelangen. Geben Sie Anmeldeinformationen des Domänenadministrators für jede Active Directory-Gesamtstruktur an, die:
    * Sie über Azure AD Connect mit Azure AD synchronisieren.
    * Benutzer enthält, für die Sie nahtloses SSO aktivieren möchten.

Nach Abschluss des Assistenten ist das nahtlose einmalige Anmelden für Ihren Mandanten aktiviert.

>[!NOTE]
> Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur für die Aktivierung des Features verwendet.

Befolgen Sie diese Anweisungen, um zu überprüfen, ob die nahtlose SSO ordnungsgemäß aktiviert ist:

1. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Verwaltungscenter](https://aad.portal.azure.com) an.
2. Wählen Sie im linken Bereich die Option **Azure Active Directory** aus.
3. Wählen Sie **Azure AD Connect** aus.
4. Überprüfen Sie, ob **Aktiviert** für **Nahtloses einmaliges Anmelden** angezeigt wird.

![Azure-Portal: Bereich „Azure AD Connect“](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Schritt 3: Ausrollen des Features

Um das Rollout für das Feature durchzuführen, müssen Sie den Intranetzoneneinstellungen Ihrer Benutzer mithilfe der Gruppenrichtlinie in Active Directory die folgende Azure AD-URL hinzufügen:

- https://autologon.microsoftazuread-sso.com


Darüber hinaus müssen Sie mithilfe der Gruppenrichtlinie eine Richtlinieneinstellung für eine Intranetzone namens **Aktualisierungen der Statusleiste per Skript zulassen** aktivieren. 

>[!NOTE]
> Die folgenden Anweisungen funktionieren nur für Internet Explorer und Google Chrome unter Windows (wenn ein Satz von URLs vertrauenswürdiger Websites wie für Internet Explorer freigegeben wird). Lesen Sie im nächsten Abschnitt die Anweisungen zum Einrichten von Mozilla Firefox und Google Chrome auf Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Warum müssen Sie Einstellungen von Benutzern für Intranetzonen ändern?

Der Browser berechnet standardmäßig anhand der URL automatisch die richtige Zone (Internet oder Intranet). Beispielsweise ist http://contoso/ der Intranetzone und http://intranet.contoso.com/ der Internetzone zugeordnet (da die URL einen Punkt enthält). Browser senden keine Kerberos-Tickets an Cloudendpunkte wie die Azure AD-URL, sofern Sie die URL nicht explizit zur Intranetzone des Browsers hinzufügen.

### <a name="detailed-steps"></a>Ausführliche Schritte

1. Öffnen Sie das Tool Gruppenrichtlinienverwaltungs-Editor.
2. Bearbeiten Sie die Gruppenrichtlinie, die auf einige oder alle Benutzer angewendet wird. In diesem Beispiel wird **Standardrichtlinie der Domäne** verwendet.
3. Navigieren Sie zu **Benutzerkonfiguration** > **Verwaltungsvorlagen** > **Windows-Komponenten** > **Internet Explorer** > **Internetsystemsteuerung** > **Seite „Sicherheit“**. Wählen Sie dann **Liste der Site zu Zonenzuweisungen**.
    ![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso6.png)
4. Aktivieren Sie die Richtlinie, und geben Sie die folgenden Werte in das Dialogfeld ein:
   - **Wertname**: Die Azure AD-URL, an die die Kerberos-Tickets weitergeleitet werden.
   - **Wert** (Daten): **1** gibt die Intranetzone an.

    Das Ergebnis sieht wie folgt aus:

    Wert: https://autologon.microsoftazuread-sso.com
  
    Data 1

   >[!NOTE]
   > Wenn Sie das nahtlose einmalige Anmelden für einige Benutzer verbieten möchten (beispielsweise weil sich diese Benutzer bei freigegebenen Kiosken anmelden), legen Sie die vorherigen Werte auf **4** fest. Diese Aktion fügt die Azure AD-URL zur Zone eingeschränkter Sites hinzu und löst für das nahtlose einmalige Anmelden ständig einen Fehler aus.
   >

5. Klicken Sie auf **OK** und anschließend erneut auf **OK**.

    ![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso7.png)

6. Navigieren Sie zu **Benutzerkonfiguration** > **Verwaltungsvorlagen** > **Windows-Komponenten** > **Internet Explorer** > **Internetsystemsteuerung** > **Seite „Sicherheit“** > **Intranetzone**. Wählen Sie dann **Aktualisierungen der Statusleiste per Skript zulassen**.

    ![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso11.png)

7. Aktivieren Sie die Richtlinieneinstellung, und klicken Sie dann auf **OK**.

    ![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Überlegungen zum Browser

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (alle Plattformen)

Mozilla Firefox verwendet nicht automatisch die Kerberos-Authentifizierung. Jeder Benutzer muss den Firefox-Einstellungen manuell die Azure AD-URL mithilfe der folgenden Schritte hinzufügen:
1. Führen Sie Firefox aus, und geben Sie in die Adressleiste `about:config` ein. Schließen Sie alle Benachrichtigungen, die Sie sehen.
2. Suchen Sie nach der Einstellung **network.negotiate-auth.trusted-uris**. In dieser Einstellung werden in Firefox die vertrauenswürdigen Sites für die Kerberos-Authentifizierung aufgeführt.
3. Klicken Sie mit der rechten Maustaste, und wählen Sie dann **Ändern** aus.
4. Geben Sie https://autologon.microsoftazuread-sso.com in das Feld ein.
5. Klicken Sie auf **OK**, und öffnen Sie den Browser erneut.

#### <a name="safari-mac-os"></a>Safari (Mac OS)

Stellen Sie sicher, dass der Computer mit Mac OS in Azure AD eingebunden ist. Anweisungen zum Verknüpfen mit Azure AD finden Sie unter [Best Practices for Integrating OS X with Active Directory](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf) (Bewährte Methoden für die Integration von OS X in Active Directory).

#### <a name="google-chrome-all-platforms"></a>Google Chrome (alle Plattformen)

Wenn Sie die Richtlinieneinstellungen [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) oder [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) in Ihrer Umgebung außer Kraft gesetzt haben, stellen Sie sicher, dass Sie ihnen auch die URL von Azure AD (https://autologon.microsoftazuread-sso.com) hinzufügen.

#### <a name="google-chrome-mac-os-only"></a>Google Chrome (nur Mac OS)

Informationen dazu, wie Sie in Google Chrome unter Mac OS und anderen Nicht-Windows-Plattformen die Azure AD-URL für die integrierte Authentifizierung auf eine Whitelist setzen, finden Sie unter [The Chromium Project Policy List](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) (Richtlinienliste von The Chromium Project).

Das Rollout der Azure AD-URL für Firefox und Google Chrome unter Mac mithilfe von Active Directory-Gruppenrichtlinienerweiterungen von Drittanbietern kann in diesem Artikel nicht behandelt werden.

#### <a name="known-browser-limitations"></a>Bekannte Browsereinschränkungen

Das nahtlose einmalige Anmelden funktioniert in Firefox- und Edge-Browsern nicht im privaten Modus. Dies gilt auch für Internet Explorer, wenn der Browser im erweiterten geschützten Modus ausgeführt wird.

## <a name="step-4-test-the-feature"></a>Schritt 4: Testen des Features

Um das Feature für einen bestimmten Benutzer zu testen, stellen Sie sicher, dass alle folgenden Bedingungen erfüllt werden:
  - Der Benutzer meldet sich auf einem Gerät des Unternehmens an.
  - Das Gerät ist mit Ihrer Active Directory-Domäne verknüpft.
  - Es muss eine direkte Verbindung zwischen dem Gerät und Ihrem Domänencontroller (DC) bestehen, entweder über das Unternehmensnetzwerk (Kabel- oder Funknetzwerk) oder per Remotezugriff, z.B. über eine VPN-Verbindung.
  - Sie haben [das Feature für diesen Benutzer mithilfe von Gruppenrichtlinien ausgerollt](##step-3-roll-out-the-feature).

So testen Sie das Szenario, wenn der Benutzer nur den Benutzernamen eingibt, jedoch kein Kennwort:
   - Melden Sie sich unter https://myapps.microsoft.com/ in einer neuen, privaten Browsersitzung an.

Führen Sie zum Testen des Szenarios, in dem der Benutzer weder den Benutzernamen noch das Kennwort eingeben muss, einen der folgenden Schritte aus: 
   - Melden Sie sich unter https://myapps.microsoft.com/contoso.onmicrosoft.com in einer neuen, privaten Browsersitzung an. Ersetzen Sie *contoso* durch den Namen Ihres Mandanten.
   - Melden Sie sich alternativ unter https://myapps.microsoft.com/contoso.com in einer neuen, privaten Browsersitzung an. Ersetzen Sie *contoso.com* durch eine überprüfte Domäne (keine Verbunddomäne) in Ihrem Mandanten.

## <a name="step-5-roll-over-keys"></a>Schritt 5: Durchführen des Rollovers für Schlüssel

In Schritt 2 erstellt Azure AD Connect Computerkonten (die Azure AD repräsentieren) in allen Active Directory-Gesamtstrukturen, für die Sie das nahtlose einmalige Anmelden aktiviert haben. Weitere Informationen finden Sie unter [Azure Active Directory: Nahtloses einmaliges Anmelden: Technische Einblicke](active-directory-aadconnect-sso-how-it-works.md). Zur Erhöhung der Sicherheit wird empfohlen, für die Kerberos-Entschlüsselungsschlüssel dieser Computerkonten regelmäßig ein Rollover durchzuführen. Anweisungen zum Durchführen des Rollovers für Schlüssel finden Sie unter [Nahtloses einmaliges Anmelden mit Azure Active Directory: Häufig gestellte Fragen (FAQs)](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Sie müssen diesen Schritt nicht _sofort_ nach der Aktivierung des Features ausführen. Führen Sie für die Kerberos-Entschlüsselungsschlüssel mindestens alle 30 Tage ein Rollover durch.

## <a name="next-steps"></a>Nächste Schritte

- [Technische Einblicke:](active-directory-aadconnect-sso-how-it-works.md) Informationen zur Funktionsweise des nahtlosen einmaligen Anmeldens
- [Häufig gestellte Fragen:](active-directory-aadconnect-sso-faq.md) Antworten auf häufig gestellte Fragen zum nahtlosen einmaligen Anmelden
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-sso.md) Informationen zum Beheben von allgemeinen Problemen, die mit der Funktion für nahtloses einmaliges Anmelden auftreten können
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum

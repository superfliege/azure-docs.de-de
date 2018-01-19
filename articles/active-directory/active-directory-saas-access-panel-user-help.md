---
title: "Hilfe zum Portal „Meine Apps“ in Azure Active Directory | Microsoft-Dokumentation"
description: Hier finden Sie eine Anleitung zu allgemeinen Aufgaben im Zugriffsbereich.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 9bec51e1d49308baecc76143ec80902d2da418e8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>Hilfe zum Portal „Meine Apps“ in Azure Active Directory

Wahrscheinlich haben Sie diese Seite aufgerufen, weil Sie leider bei der Nutzung des Portals „Meine Apps“ auf ein Problem gestoßen sind. In manchen Fällen ist es nötig, den Helpdesk oder Ihren Administrator um Hilfe bei der Problemlösung zu bitten. Vielleicht finden Sie aber auch in den nachstehenden Abschnitten eine Lösung.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Ich habe Probleme bei der Anmeldung im Portal „Meine Apps“

Führen Sie Folgendes aus:

- Überprüfen Sie, ob Sie sich über die richtige URL anmelden: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Fügen Sie die URL in Ihrem Browser zu den vertrauenswürdigen Seiten hinzu.

- Vergewissern Sie sich, dass Sie Ihr Kennwort kennen und es nicht abgelaufen ist. Weitere Informationen zu Aktualisieren Ihres Kennwort finden Sie [hier](active-directory-passwords-update-your-own-password.md).

- Überprüfen Sie, ob Ihre Kontaktinformationen für die Authentifizierung korrekt sind und den Zugriff nicht blockieren. Weitere Informationen zum Einrichten der Authentifizierungsinformationen finden Sie [hier](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).

- Löschen Sie in Ihrem Browser die Cookies und versuchen Sie dann noch einmal, sich anzumelden.

Wenn beim Anmelden weiterhin Probleme auftreten, wenden Sie sich bitte an Ihren Administrator.


## <a name="how-do-i-update-my-password"></a>Wie kann ich mein Kennwort aktualisieren?

Falls Sie Ihr Kennwort ändern möchten, es vergessen oder nie von der IT-Abteilung erhalten haben oder aus Ihrem Konto ausgesperrt wurden, finden Sie unter [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md) weitere Informationen.

## <a name="how-do-i-register-for-password-reset"></a>Wie registrierte ich mich für die Zurücksetzung meines Kennworts?

Endbenutzer können mithilfe der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) selbstständig ihr Kennwort zurücksetzen oder ihr Konto entsperren. Um diese Funktion verwenden zu können, müssen Sie zunächst Authentifizierungsmethoden registrieren oder die vordefinierten Authentifizierungsmethoden bestätigen, die Ihr Administrator angegeben hat. Weitere Informationen finden Sie unter [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ich habe Probleme bei der Installation der Erweiterung zur sicheren Anmeldung bei „Meine Apps“.

Überprüfen Sie, ob Sie den erforderlichen Browser verwenden:

- Für das Portal benötigen Sie einen Browser der JavaScript unterstützt und in dem CSS aktiviert ist. Wenn Sie kennwortbasierte Apps für einmaliges Anmelden verwenden, muss die dazugehörige Browsererweiterung auch installiert werden. Diese Erweiterung wird automatisch heruntergeladen, wenn Sie eine Anwendung starten, die für kennwortbasierte Apps für einmaliges Anmelden konfiguriert ist.

- Die Erweiterung erfordert folgenden Browser:
    - Edge unter Windows 10 Anniversary Edition oder höher
    - Chrome unter Windows 7 oder höher und macOS X oder höher
    - Firefox 26.0 oder höher unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher
    - Internet Explorer 8, 9, 10, 11 unter Windows 7 oder höher (eingeschränkte Unterstützung)

Sie können die Erweiterung für Chrome und Firefox auch direkt über die folgenden Links herunterladen:

- [Chrome-Erweiterung](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Edge-Erweiterung](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Wenn nach der Installation Probleme auftreten, führen Sie die folgenden Schritte aus:

- Prüfen Sie in den Einstellungen Ihres Browsers, ob die Erweiterung auch wirklich installiert wurde.

- Starten Sie Ihren Browser neu und melden Sie sich im Portal „Meine Apps“ an.

- Löschen Sie über Ihren Browser die Cookies und melden Sie sich im Portal „Meine Apps“ an.
- Befolgen Sie die Anleitung [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting), um Zugriff auf ein Diagnosetool und Schritt-für-Schritt-Anweisungen zum Konfigurieren der Erweiterung für Internet Explorer zu erhalten.

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Wie verwende ich die Erweiterung zur sicheren Anmeldung bei „Meine Apps“?
Ändern der Standard-URL von „Meine Apps“ für die Erweiterung

Bei Verwendung einer anderen URL von „Meine Apps“ als https://myapps.microsoft.com müssen Sie die Standard-URL durch Ausführen der folgenden Schritte konfigurieren:
1. **Klicken Sie mit der rechten Maustaste** auf das Symbol für die Erweiterung, ohne bei der Erweiterung angemeldet zu sein.
2. Klicken Sie im Menü auf **Meine Apps-URL auswählen**.
3. **Wählen Sie** Ihre Standard-URL aus.
4. Klicken Sie auf das Symbol für die Erweiterung.
5. Melden Sie sich bei der Erweiterung an, indem Sie **Melden Sie sich an, um loszulegen** auswählen.

Direktes Anmelden bei einer App über den Browser
1. Melden Sie sich nach der Installation der Erweiterung bei der Erweiterung an, indem Sie **Melden Sie sich an, um loszulegen** auswählen.
2. Navigieren Sie zu der **Anmelde-URL** der App, bei der Sie sich anmelden möchten. Dies ist in der Regel die URL der App, die das Anmeldeformular anzeigt.
3. Der Status der Erweiterung sollte sich ändern, sodass Sie darüber informiert werden, dass ein Kennwort verfügbar ist. Klicken Sie auf das **Symbol für die Erweiterung**, um sich anzumelden.

Starten einer App über die Erweiterung
1. Melden Sie sich nach der Installation der Erweiterung bei der Erweiterung an, indem Sie **Melden Sie sich an, um loszulegen** auswählen.
2. Klicken Sie auf das Symbol für die Erweiterung, um das zugehörige Menü zu öffnen.
3. **Suchen** Sie nach einer App, die im Portal „Meine Apps“ zur Verfügung steht.
4. Klicken Sie in den **Suchergebnissen** auf die App, um sie zu starten.
5. In der Verknüpfungsliste **Zuletzt verwendet** werden auch die letzten drei gestarteten Apps angezeigt.

> [!NOTE]
> Diese Optionen sind nur für Edge, Chrome und Firefox verfügbar.

## <a name="how-do-i-add-a-new-app"></a>Wie füge ich eine neue App hinzu?

1.  Klicken Sie auf der Seite **Apps** auf **App hinzufügen**.

2.  Suchen Sie nach der App, die Sie hinzufügen möchten, und klicken Sie auf **Hinzufügen**.

**Hinweise:**

- Sie haben nur dann Zugriff auf diese Option, wenn Ihr Administrator dies für Ihr Konto aktiviert hat.

- Falls die App eine Berechtigung erfordert, ist unter Umständen eine Administratorgenehmigung erforderlich.


## <a name="how-do-i-manage-my-group-memberships"></a>Wie verwalte ich meine Gruppenmitgliedschaften?

1. Klicken Sie auf die Kachel **Gruppen**. 
2. Klicken Sie zum Erstellen einer Gruppe unter „Gruppen in meinem Besitz“ auf **Gruppe erstellen**, und folgen Sie den Anweisungen.
3. Um einer Gruppe beizutreten, klicken Sie unter „Gruppen in meinem Besitz“ auf **Gruppe beitreten**, und befolgen Sie die Anweisungen.

**Hinweise:**

- Sie haben nur dann Zugriff auf diese Option, wenn Ihr Administrator dies für Ihr Konto aktiviert hat.

- Zu einer Gruppe, in der Sie Mitglied sind, lassen sich Details anzeigen und Sie können aus ihr austreten.

- Bei Gruppen, die sich in Ihrem Besitz befinden, lassen sich Details anzeigen. Darüber hinaus lassen sich Mitglieder hinzufügen und entfernen und Sie können aus der Gruppe austreten.


## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung finden Sie unter [Probleme bei der Verwendung der Website mit dem Anwendungszugriffsbereich oder der mobilen Anwendung](active-directory-application-access-panel-content-map.md).


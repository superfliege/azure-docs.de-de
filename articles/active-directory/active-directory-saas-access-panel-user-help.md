---
title: Hilfe bei Zugriff und Verwendung des Portals „Meine Apps“ in Azure Active Directory | Microsoft-Dokumentation
description: Hier erhalten Sie Hilfe beim Anmelden beim Zugriffsbereich und beim Ausführen häufiger Aufgaben.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 285ee0c4e7f963c372c4f95face73864ba2a4f68
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-my-apps-portal"></a>Behandeln von Problemen bei Zugriff und Verwendung des Portals „Meine Apps“

Wenn Sie Probleme damit haben, sich beim Meine Apps-Portal anzumelden oder das Portal zu verwenden, versuchen Sie diese Problembehandlungstipps, bevor Sie sich an den Helpdesk oder Ihren Administrator wenden.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Ich habe Probleme bei der Anmeldung im Portal „Meine Apps“

Probieren Sie diese allgemeinen Tipps aus:

- Überprüfen Sie zuerst, ob Sie die richtige URL verwenden: [https://myapps.microsoft.com](https://myapps.microsoft.com).
- Fügen Sie die URL in Ihrem Browser zu den vertrauenswürdigen Seiten hinzu.
- Stellen Sie sicher, dass Ihr Kennwort richtig und nicht abgelaufen ist. Weitere Informationen finden Sie unter [Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos](active-directory-passwords-update-your-own-password.md).
- Überprüfen Sie, ob Ihre Kontaktinformationen für die Authentifizierung richtig sind und den Zugriff nicht blockieren. Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Löschen Sie in Ihrem Browser die Cookies und versuchen Sie dann noch einmal, sich anzumelden.

Wenn beim Anmelden weiterhin Probleme auftreten, wenden Sie sich an Ihren Administrator.


## <a name="i-seem-to-be-having-password-issues"></a>Ich habe anscheinend ein Problem mit dem Kennwort

Falls Sie Ihr Kennwort ändern möchten, es vergessen oder nie von der IT-Abteilung erhalten haben oder aus Ihrem Konto ausgesperrt wurden, finden Sie unter [Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos](active-directory-passwords-update-your-own-password.md) weitere Informationen.

## <a name="i-need-to-register-for-password-reset"></a>Ich muss mich für die Kennwortzurücksetzung registrieren

Sie können mithilfe der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) selbstständig Ihr Kennwort zurücksetzen oder Ihr Konto entsperren. Um diese Funktion verwenden zu können, müssen Sie zunächst Ihre Authentifizierungsmethoden registrieren oder die vordefinierten Authentifizierungsmethoden bestätigen, die Ihr Administrator als erforderlich angegeben hat. Weitere Informationen finden Sie unter [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ich habe Probleme bei der Installation der Erweiterung zur sicheren Anmeldung bei „Meine Apps“.

Das Meine Apps-Portal erfordert einen Browser, der JavaScript unterstützt und in dem CSS aktiviert ist. Wenn Sie kennwortbasierte Apps für einmaliges Anmelden verwenden, muss die dazugehörige Browsererweiterung auch installiert werden. Diese Erweiterung wird automatisch heruntergeladen, wenn Sie eine Anwendung starten, die für kennwortbasierte Apps mit einmaligem Anmelden konfiguriert ist.

Stellen Sie sicher, dass folgende Browseranforderungen erfüllt sind:
- **Microsoft Edge**: unter Windows 10 Anniversary Edition oder höher.
- **Chrome**: unter Windows 7 oder höher und Mac OS X oder höher.
- **Firefox 26.0 oder höher**: unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher.
- **Internet Explorer 8, 9, 10, 11**: unter Windows 7 oder höher (eingeschränkte Unterstützung).

Sie können die Erweiterung für Chrome und Edge auch direkt von den folgenden Websites herunterladen:

- [Chrome-Erweiterung](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Edge-Erweiterung](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Wenn Sie die Erweiterung installiert haben und dennoch weiterhin Probleme auftreten, versuchen Sie Folgendes:

- Prüfen Sie in den Browsereinstellungen für Erweiterungen, ob die Erweiterung aktiviert wurde.
- Starten Sie Ihren Browser neu, und melden Sie sich beim Portal „Meine Apps“ an.
- Löschen Sie die Cookies in Ihrem Browser, und melden Sie sich beim Portal „Meine Apps“ an.
- Unter [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) erhalten Sie Zugriff auf ein Diagnosetool sowie Schrittanleitungen zum Konfigurieren der Erweiterung für Internet Explorer.

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Verwenden der Erweiterung zur sicheren Anmeldung bei „Meine Apps“
* Wenn Sie eine andere Meine Apps-URL als `https://myapps.microsoft.com` verwenden, gehen Sie folgendermaßen vor, um Ihre Standard-URL zu konfigurieren:
   1. Klicken Sie mit der rechten Maustaste auf das Symbol für die Erweiterung, *ohne* bei der Erweiterung angemeldet zu sein.
   2. Wählen Sie im Menü den Eintrag **Meine Apps-URL**.
   3. Wählen Sie Ihre Standard-URL aus.
   4. Wählen Sie das Symbol für die Erweiterung aus.
   5. Um sich bei der Erweiterung anzumelden, wählen Sie **Melden Sie sich an, um loszulegen** aus.

* Um sich im Browser direkt bei einer App anzumelden, gehen Sie folgendermaßen vor:
   1. Melden Sie sich nach der Installation bei der Erweiterung an, indem Sie **Melden Sie sich an, um loszulegen** auswählen.
   2. Melden Sie sich mit der Anmelde-URL bei der App an.  
       Bei der Anmelde-URL handelt es sich üblicherweise um die URL, mit der das Anmeldeformular einer App angezeigt wird.
      Der Status der Erweiterung sollte sich ändern und Sie darüber informiert werden, dass ein Kennwort verfügbar ist.
   3. Wählen Sie das Symbol für die Erweiterung aus, um sich anzumelden.

* Gehen Sie wie folgt vor, um eine App aus der Erweiterung zu starten:
   1. Melden Sie sich nach der Installation bei der Erweiterung an, indem Sie **Melden Sie sich an, um loszulegen** auswählen.
   2. Wählen Sie das Symbol für die Erweiterung aus, um das zugehörige Menü zu öffnen.
   3. Suchen Sie nach einer App, die im Portal „Meine Apps“ zur Verfügung steht.
   4. Wählen Sie die App aus den Suchergebnissen aus.  
       In der Verknüpfungsliste **Zuletzt verwendet** werden die letzten drei verwendeten Apps angezeigt.

> [!NOTE]
> Diese Optionen sind nur für Microsoft Edge, Chrome und Firefox verfügbar.

## <a name="how-do-i-add-a-new-app"></a>Wie füge ich eine neue App hinzu?

1.  Wählen Sie auf der Seite **Apps** den Eintrag **App hinzufügen** aus.
2.  Suchen Sie nach der App, die Sie hinzufügen möchten, und wählen Sie **Hinzufügen** aus.

   > [!NOTE]
   > * Sie können nur dann auf diese Option zugreifen, wenn Ihr Administrator sie für Ihr Konto aktiviert hat.
   > * Wenn die App eine Berechtigung erfordert, müssen Sie unter Umständen auf eine Administratorgenehmigung warten.
   > 

## <a name="how-do-i-manage-my-group-memberships"></a>Wie verwalte ich meine Gruppenmitgliedschaften?

Wählen Sie die Kachel **Gruppen** aus, und führen Sie eine der folgenden Aktionen aus: 
* Wählen Sie zum Erstellen einer Gruppe unter **Gruppen in meinem Besitz** die Option **Gruppe erstellen** aus, und folgen Sie den Anweisungen.
* Um einer Gruppe beizutreten, klicken Sie unter **Meine Gruppenmitgliedschaften** auf **Gruppe beitreten**, und befolgen Sie die Anweisungen.

   > [!NOTE]
   > * Sie können nur dann auf diese Option zugreifen, wenn Ihr Administrator sie für Ihr Konto aktiviert hat.
   > * Wenn Sie Mitglied einer Gruppe sind, können Sie Details anzeigen und die Gruppe verlassen.
   > * Wenn Sie Besitzer einer Gruppe sind, können Sie Details anzeigen, Mitglieder hinzufügen/entfernen und die Gruppe verlassen.
   >


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie unter [Probleme bei der Verwendung der Website mit dem Anwendungszugriffsbereich oder der mobilen Anwendung](active-directory-application-access-panel-content-map.md).


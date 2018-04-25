---
title: Bereitstellungshandbuch zur Self-Service-Kennwortzurücksetzung – Azure Active Directory
description: Tipps für ein erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung von Azure AD
services: active-directory
keywords: ''
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: e34bf6ec106976c1c3aab0f2b5c4ebf4b6ccc54b
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung

Um einen reibungslosen Rollout der Funktion für die Self-Service-Kennwortzurücksetzung (SSPR) von Azure Active Directory (Azure AD) sicherzustellen, führen die meisten Kunden die folgenden Schritte aus:

1. [Aktivieren Sie die Kennwortzurücksetzung in Ihrem Verzeichnis.](quickstart-sspr.md)
2. [Konfigurieren Sie die lokalen Active Directory-Berechtigungen für das Kennwortrückschreiben](howto-sspr-writeback.md#active-directory-permissions).
3. [Konfigurieren Sie das Kennwortrückschreiben](howto-sspr-writeback.md#configure-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.
4. [Weisen Sie die erforderlichen Lizenzen zu, und überprüfen Sie sie](concept-sspr-licensing.md).
5. Entscheiden Sie, ob Sie den Rollout schrittweise durchführen möchten. Wenn Sie den SSPR-Rollout in Etappen durchführen möchten, können Sie den Zugriff auf eine Gruppe von Benutzern begrenzen und somit ein Pilotprogramm mit einer bestimmten Gruppe ausführen. Um einen Rollout für eine bestimmte Gruppe durchzuführen, legen Sie **Self-Service-Kennwortzurücksetzung aktiviert** auf **Ausgewählt** fest, und wählen Sie die Sicherheitsgruppe aus, für die die Kennwortzurücksetzung verfügbar sein soll. 
6. Füllen Sie die [Authentifizierungsdaten](howto-sspr-authenticationdata.md) auf, die die Benutzer zur Registrierung benötigen, beispielsweise die geschäftliche Telefonnummer, die Mobiltelefonnummer und eine alternative E-Mail-Adresse.
7. [Passen Sie die Azure AD-Benutzeroberfläche für die Anmeldung mit Ihrem Unternehmensbranding an](concept-sspr-customization.md).
8. Unterweisen Sie die Benutzer in der Verwendung von SSPR. Senden Sie ihnen Anweisungen zum Durchführen der Registrierung und Zurücksetzen ihrer Kennwörter.
9. Legen Sie fest, wann die Registrierung erzwungen werden soll. Sie können die Registrierung zu einem beliebigen Zeitpunkt erzwingen. Zudem können Sie festlegen, dass Benutzer ihre Authentifizierungsinformationen nach einem bestimmten Zeitraum erneut bestätigen müssen.
10. Verwenden Sie die Berichterstellungsfunktion. Im Laufe der Zeit können Sie die Registrierungen von Benutzern und die Verwendung mithilfe der [Berichterstellungsfunktion von Azure AD](howto-sspr-reporting.md) überprüfen.
11. Aktivieren Sie die Kennwortzurücksetzung. Wenn Sie bereit sind, aktivieren Sie die Kennwortzurücksetzung für alle Benutzer, indem Sie den Schalter **Self-Service-Kennwortzurücksetzung aktiviert** auf **Alle** festlegen. 

   > [!NOTE]
   > Wenn Sie diese Option von einer ausgewählten Gruppe in „Jeder“ ändern, werden die vorhandenen Authentifizierungsdaten nicht ungültig, die ein Benutzer im Rahmen einer Testgruppe registriert hat. Konfigurierte Benutzer, für die gültige Authentifizierungsdaten registriert wurden, können weiterhin verwendet werden.

12. [Ermöglichen Sie Windows 10-Benutzern das Zurücksetzen ihres Kennworts auf dem Anmeldebildschirm](tutorial-sspr-windows.md).

   > [!IMPORTANT]
   > Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](concept-sspr-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>E-Mail-basierte Einführung

Viele Kunden sind der Meinung, dass eine E-Mail-Kampagne mit einfachen Anweisungen der einfachste Weg ist, um Benutzer über die Nutzung von SSPR zu informieren. [Wir haben drei einfache E-Mails erstellt, die Sie beim Rollout als Vorlagen verwenden können](https://www.microsoft.com/download/details.aspx?id=56768):

* **Coming Soon** (In Kürze verfügbar): Diese E-Mail-Vorlage kann in den Wochen bzw. Tagen vor dem Rollout verwendet werden, um Benutzer darüber zu informieren, dass sie in Kürze aktiv werden müssen.
* **Available Now** (Jetzt verfügbar): Diese E-Mail-Vorlage kann am Tag der Programmeinführung verwendet werden, um Benutzer zum Registrieren und Bestätigen ihrer Authentifizierungsdaten zu bewegen. Wenn Benutzer sich jetzt registrieren, ist SSPR bei Bedarf für sie verfügbar.
* **Sign up Reminder** (Erinnerung an Registrierung): Diese E-Mail-Vorlage kann einige Tage bzw. Wochen nach der Bereitstellung verwendet werden, um Benutzer an das Registrieren und Bestätigen ihrer Authentifizierungsdaten zu erinnern.

![E-Mail][Email]

## <a name="create-your-own-password-portal"></a>Erstellen eines eigenen Kennwortportals

Viele Kunden entscheiden sich für das Hosten einer Webseite und das Erstellen eines DNS-Stammeintrags, beispielsweise https://passwords.contoso.com. Sie füllen diese Seite mit Links zu den folgenden Informationen auf:

* [Portal für die Azure AD-Kennwortzurücksetzung: https://aka.ms/sspr](https://aka.ms/sspr)
* [Registrierungsportal für die Azure AD-Kennwortzurücksetzung: https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Portal für die Azure AD-Kennwortänderung: https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Weitere organisationsspezifische Informationen

Sie können in allen E-Mails oder auf Flyern eine einprägsame Marken-URL angeben, unter der Benutzer sämtliche Informationen zur Nutzung der Dienste finden. Wir haben eine [Beispielseite für die Kennwortzurücksetzung](https://github.com/ajamess/password-reset-page) erstellt, die Sie verwenden und entsprechend den Anforderungen Ihrer Organisation anpassen können.

## <a name="use-enforced-registration"></a>Verwenden der erzwungenen Registrierung

Wenn Sie möchten, dass sich Ihre Benutzer für die Kennwortzurücksetzung registrieren, können Sie die Registrierung bei der Anmeldung über Azure AD erzwingen. Sie können diese Option im Bereich **Kennwortzurücksetzung** Ihres Verzeichnisses aktivieren, indem Sie auf der Registerkarte **Registrierung** die Option **Registrierung von Benutzern bei der Anmeldung verlangen?** aktivieren.

Administratoren können auch festlegen, dass sich Benutzer nach einem bestimmten Zeitraum erneut registrieren müssen. Sie können **Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen** auf 0 bis 730 Tage festlegen.

Nachdem Sie diese Option aktiviert haben, sehen Benutzer bei der Anmeldung eine Meldung mit dem Hinweis, dass ihr Administrator die Bestätigung ihrer Authentifizierungsinformationen verlangt.

## <a name="populate-authentication-data"></a>Auffüllen von Authentifizierungsdaten

Sie sollten [die Authentifizierungsdaten für Ihre Benutzer auffüllen](howto-sspr-authenticationdata.md). Auf diese Weise müssen sich Benutzer erst dann für die Kennwortzurücksetzung registrieren, wenn sie SSPR verwenden können. Solange Benutzer die Authentifizierungsdaten angeben, die der von Ihnen definierten Richtlinie für die Kennwortzurücksetzung entsprechen, können sie ihre Kennwörter zurücksetzen.

## <a name="disable-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung** > **Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine**.

## <a name="next-steps"></a>Nächste Schritte

* [Zurücksetzen oder Ändern des Kennworts](../active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](../active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](concept-sspr-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](howto-sspr-authenticationdata.md)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](concept-sspr-policy.md)
* [Übersicht über die Kennwortrückschreibung](howto-sspr-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](howto-sspr-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](concept-sspr-howitworks.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Passen Sie diese E-Mail-Vorlagen den Anforderungen Ihrer Organisation entsprechend an."

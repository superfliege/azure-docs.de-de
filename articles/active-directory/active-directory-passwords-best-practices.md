---
title: 'Rollout: Azure AD SSPR | Microsoft-Dokumentation'
description: "Tipps für ein erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung von Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0be1616f5df915e566dc73c15dbea2e53177aa1c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="how-to-successfully-rollout-self-service-password-reset"></a>Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung

Die meisten Kunden führen die hier angegebenen Schritte aus, um für ein reibungsloses Rollout der SSPR-Funktionalität (Self-Service Password Reset, Self-Service-Kennwortzurücksetzung) zu sorgen.

1. [Aktivieren Sie die Kennwortzurücksetzung in Ihrem Verzeichnis.](active-directory-passwords-getting-started.md)
2. [Konfigurieren Sie die lokalen AD-Berechtigungen für das Kennwortrückschreiben.](active-directory-passwords-writeback.md#active-directory-permissions)
3. [Konfigurieren Sie das Kennwortrückschreiben](active-directory-passwords-writeback.md#configuring-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.
4. [Weisen Sie die erforderlichen Lizenzen zu, und überprüfen Sie sie.](active-directory-passwords-licensing.md)
5. Wenn Sie das SSPR-Rollout in Etappen durchführen möchten, können Sie den Zugriff auf eine Gruppe von Benutzern begrenzen und somit ein Pilotprojekt mit einer bestimmten Gruppe verwenden. Ändern Sie hierzu die Umschaltoption **Self-Service-Kennwortzurücksetzung aktiviert** in **Selected** (Ausgewählte), und wählen Sie eine Sicherheitsgruppe aus, die für die Kennwortzurücksetzung aktiviert werden soll. 
6. Geben Sie [Authentifizierungsdaten](active-directory-passwords-data.md) für Ihre Benutzer an, etwa ihre geschäftliche Telefonnummer, die Mobilfunknummer und eine alternative E-Mail-Adresse.
7. [Passen Sie die Azure AD-Anmeldung mit Ihrem Unternehmensbranding an.](active-directory-passwords-customize.md)
8. Schulen Sie Ihre Benutzer in der Nutzung von SSPR, indem Sie ihnen eine Anleitung senden, in der die Registrierung und Zurücksetzung beschrieben ist.
9. Sie können einen beliebigen Zeitpunkt für das Erzwingen der Registrierung angeben und außerdem festlegen, dass Benutzer ihre Authentifizierungsinformationen nach einem bestimmten Zeitraum neu bestätigen müssen.
10. Sie können den Verlauf der Registrierung und Nutzung durch die Benutzer verfolgen, indem Sie die [Berichterstellungsfunktion von Azure AD](active-directory-passwords-reporting.md) verwenden.
11. Wenn Sie bereit sind, legen Sie den Umschalter **Self-Service-Kennwortzurücksetzung aktiviert** auf **Alle** fest. 

   > [!IMPORTANT]
   > Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>E-Mail-basierte Einführung

Viele Kunden sind der Meinung, dass eine E-Mail-Kampagne mit einfachen Anweisungen der einfachste Weg ist, um Benutzer über die Nutzung von SSPR zu informieren. [Wir haben drei einfache E-Mails erstellt, die Sie beim Rollout als Vorlagen verwenden können:](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* E-Mail-Vorlage **Coming Soon** (In Kürze verfügbar): Kann in den Wochen bzw. Tagen vor dem Rollout eingesetzt werden, um die Benutzer darüber zu informieren, dass bald eine Aufgabe erledigt werden muss.
* E-Mail-Vorlage **Available Now** (Jetzt verfügbar): Kann am Tag der Einführung eingesetzt werden, um Benutzer zum Registrieren und Bestätigen ihrer Authentifizierungsdaten zu bewegen, damit sie SSPR bei Bedarf nutzen können.
* E-Mail-Vorlage **Sign up Reminder** (Erinnerung an Registrierung): Kann einige Tage bzw. Wochen nach der Bereitstellung eingesetzt werden, um Benutzer an das Registrieren und Bestätigen ihrer Authentifizierungsdaten zu erinnern.

![E-Mail][Email]

## <a name="creating-your-own-password-portal"></a>Erstellen eines eigenen Kennwortportals

Viele der größeren Microsoft-Kunden entscheiden sich für das Hosten einer Webseite und das Erstellen eines DNS-Stammeintrags, z.B. „https://passwords.contoso.com“. Sie fügen auf dieser Seite Links hinzu, die zu folgenden Ressourcen führen: Azure AD-Kennwortzurücksetzung, Registrierung für die Kennwortzurücksetzung, Portale für die Kennwortänderung und andere organisationsbezogene Informationen. Auf diese Weise können Sie in der E-Mail-Korrespondenz oder auf ausgegebenen Flyern dann immer eine einheitliche URL angeben, unter der Benutzer alle Informationen zur Nutzung der Dienste finden.

* Kennwortzurücksetzungsportal: https://aka.ms/sspr
* Portal zur Registrierung für die Kennwortzurücksetzung: http://aka.ms/ssprsetup
* Portal für die Änderung des Kennworts: https://account.activedirectory.windowsazure.com/ChangePassword.aspx

Wir haben eine Beispielseite erstellt, die Sie gemäß den Anforderungen Ihrer Organisation anpassen können. Sie steht auf [GitHub](https://github.com/ajamess/password-reset-page) zum Download zur Verfügung.

## <a name="using-enforced-registration"></a>Verwenden der erzwungenen Registrierung

Wenn Sie möchten, dass sich Ihre Benutzer für die Kennwortzurücksetzung registrieren, können Sie die Registrierung bei der Anmeldung an Azure AD erzwingen. Sie können diese Option auf dem Blatt **Kennwortzurücksetzung** Ihres Verzeichnisses aktivieren, indem Sie auf der Registerkarte **Registrierung** die Option **Registrierung von Benutzern bei der Anmeldung verlangen?** aktivieren.

Administratoren können vorgeben, dass sich Benutzer nach einem bestimmten Zeitraum erneut registrieren müssen, indem sie die Option **Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen** auf einen Wert von 0 bis 730 Tagen festlegen.

Nachdem Sie diese Option aktiviert haben, wird Benutzern bei der Anmeldung eine Meldung mit dem Hinweis angezeigt, dass vom Administrator die Bestätigung ihrer Authentifizierungsinformationen vorgegeben wurde.

## <a name="populate-authentication-data"></a>Auffüllen von Authentifizierungsdaten

Wenn Sie [Authentifizierungsdaten für Ihre Benutzer vorab einfügen](active-directory-passwords-data.md), müssen sich Benutzer nicht für die Kennwortzurücksetzung registrieren, bevor sie SSPR nutzen können. Solange für die Benutzer Authentifizierungsdaten definiert sind, die der von Ihnen definierten Richtlinie für die Kennwortzurücksetzung entsprechen, können Benutzer ihre Kennwörter zurücksetzen.

## <a name="disabling-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung** > **Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Niemand**.

## <a name="next-steps"></a>Nächste Schritte

* [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Welche Daten werden von SSPR verwendet, und welche Daten sollten Sie für Ihre Benutzer angeben?](active-directory-passwords-data.md)
* [Welche Richtlinienoptionen stehen mit SSPR zur Verfügung?](active-directory-passwords-policy.md)
* [Übersicht über die Kennwortrückschreibung](active-directory-passwords-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](active-directory-passwords-reporting.md)
* [Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Passen Sie diese E-Mail-Vorlagen den Anforderungen Ihrer Organisation entsprechend an."
---
title: 'Schnellstart: Azure AD SSPR | Microsoft-Dokumentation'
description: "Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d97fad04a0aa549d0e3a182282f898302e8e41a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD

> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md).

Die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) ist für IT-Administratoren eine einfache Möglichkeit, Benutzern das Zurücksetzen oder Entsperren ihrer Kennwörter oder Konten zu erlauben. Das System verfügt über eine ausführliche Berichterstellung zur Nutzung des Systems durch Benutzer sowie über eine Benachrichtigungsfunktion, damit Sie über eine fehlerhafte oder missbräuchliche Nutzung informiert sind.

In dieser Anleitung wird vorausgesetzt, dass Sie bereits über einen funktionierenden Testmandanten oder einen lizenzierten Azure AD-Mandanten verfügen. Hilfe zur Einrichtung von Azure AD finden Sie im Artikel [Erste Schritte mit einem Azure-Abonnement](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Aktivieren der SSPR für Ihren Azure AD-Mandanten

1. Wählen Sie unter Ihrem vorhandenen Azure AD-Mandanten die Option **Kennwortzurücksetzung**.

2. Wählen Sie auf dem Bildschirm **Eigenschaften** unter „Self-Service-Kennwortzurücksetzung aktiviert“ eine der folgenden Optionen:
    * Niemand: Niemand kann die SSPR-Funktion nutzen.
    * Selected (Ausgewählte): Nur Mitglieder einer bestimmten Azure AD-Gruppe, die Sie ausgewählt haben, können die SSPR-Funktion nutzen. Es wird empfohlen, eine Gruppe von Benutzern zu definieren und diese Einstellung bei der Bereitstellung als Proof of Concept zu verwenden.
    * Alle: Alle Benutzer mit Konten unter Ihrem Azure AD-Mandanten können die SSPR-Funktion nutzen. Diese Option wird empfohlen, wenn Sie bereit sind, die Funktion nach Ausführung eines Proof of Concept im gesamten Mandanten bereitzustellen.

3. Wählen Sie in der Bildschirmansicht **Authentifizierungsmethoden** Folgendes:
    * Anzahl von Methoden, die zurückgesetzt werden müssen: Hierfür werden eine oder zwei Methoden unterstützt.
    * Für Benutzer verfügbare Methoden: Mindestens eine Methode ist erforderlich, aber es kann nicht schaden, wenn noch mehr Auswahl besteht.
        * Mit der Option **E-Mail** wird eine E-Mail mit einem Code an die E-Mail-Adresse des Benutzers gesendet, die für die Authentifizierung konfiguriert ist.
        * Mit der Option **Mobiltelefon** können Benutzer einen Anruf oder eine SMS mit einem Code unter der konfigurierten Mobiltelefonnummer erhalten.
        * Bei der Option **Bürotelefon** erhalten Benutzer einen Anruf mit einem Code unter der konfigurierten Bürotelefonnummer.
        * Unter **Sicherheitsfragen** besteht die folgende Auswahl:
            * Anzahl der für die Registrierung erforderlichen Fragen: Gibt die Mindestanzahl an, die für eine erfolgreiche Registrierung erforderlich ist. Ein Benutzer kann also auch mehr Fragen beantworten, um einen Pool mit verfügbaren Fragen zu erstellen. Sie können für diese Option einen Wert von 3 bis 5 festlegen, und der Wert muss größer oder gleich der Anzahl von Fragen sein, die für das Zurücksetzen erforderlich sind.
                * Sie können benutzerdefinierte Fragen hinzufügen, indem Sie beim Auswählen der Sicherheitsfragen auf die Schaltfläche „Benutzerdefiniert“ klicken.
            * Anzahl der für die Zurücksetzung erforderlichen Fragen: Sie können für diese Option einen Wert von 3 bis 5 Fragen festlegen, die richtig beantwortet werden müssen, bevor das Kennwort eines Benutzers zurückgesetzt oder entsperrt werden kann.
            
    ![Authentifizierung][Authentication]

4. EMPFOHLEN: Mit der Option **Anpassung** können Sie den Link „Wenden Sie sich an Ihren Administrator.“ so ändern, dass er auf eine von Ihnen definierte Seite oder E-Mail-Adresse verweist. Es wird empfohlen, diesen Link für Ihre Benutzer beispielsweise auf eine vertraute E-Mail-Adresse oder Website für den Support festzulegen.

5. OPTIONAL: Die Bildschirmansicht **Registrierung** enthält die folgenden Optionen für Administratoren:
    * Registrierung von Benutzern bei der Anmeldung verlangen
    * Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen

6. OPTIONAL: Die Bildschirmansicht **Benachrichtigung** enthält die folgenden Optionen für Administratoren:
    * Benutzer über Kennwortzurücksetzungen benachrichtigen?
    * Sollen alle Administratoren benachrichtigt werden, wenn andere Administratoren ihr Kennwort zurücksetzen?

**Sie haben SSPR jetzt für Ihren Azure AD-Mandanten konfiguriert**. Ihre Benutzer können nun ihr Kennwort ohne Eingreifen des Administrators anhand der Anweisungen unter [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md) und [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md) aktualisieren. Sie können hier aufhören, wenn Sie nur die Cloud verwenden, oder weitermachen, um die Synchronisierung von Kennwörtern in einer lokalen AD-Domäne zu konfigurieren.

> [!IMPORTANT]
> Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurieren der Synchronisierung mit der vorhandenen Identitätsquelle

Um die lokale Identitätssynchronisierung mit Azure AD zu aktivieren, müssen Sie [Azure AD Connect](./connect/active-directory-aadconnect.md) auf einem Server Ihrer Organisation installieren und konfigurieren. Mit dieser Anwendung wird die Synchronisierung von Benutzern und Gruppen von Ihrer vorhandenen Identitätsquelle mit Ihrem Azure AD-Mandanten durchgeführt.

* [Aktualisieren der Windows Azure Active Directory-Synchronisierung und Azure Active Directory-Synchronisierung](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurieren Sie das Kennwortrückschreiben](active-directory-passwords-writeback.md#configuring-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.

### <a name="on-premises-policy-change"></a>Änderung der lokalen Richtlinie

Wenn Sie Benutzer aus einer lokalen Active Directory-Domäne synchronisieren und Benutzern die sofortige Zurücksetzung ihrer Kennwörter ermöglichen möchten, ändern Sie die lokale Kennwortrichtlinie wie folgt:

**Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Kontorichtlinien** > **Kennwortrichtlinie**

**Minimales Kennwortalter** – 0 Tage

Diese Sicherheitseinstellung bestimmt den Zeitraum (in Tagen), für den ein Kennwort verwendet werden muss, bevor der Benutzer es ändern kann. Die Einstellung **0 Tage** ermöglicht Benutzern die Verwendung der SSPR, wenn ihre Kennwörter von den Supportteams geändert werden.

![Richtlinie][Policy]

## <a name="disabling-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung > Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine**.

### <a name="learn-more"></a>Weitere Informationen
Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [Wie führe ich ein erfolgreiches Rollout der SSPR durch?](active-directory-passwords-best-practices.md)
* [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Haben Sie eine Frage zur Lizenzierung?](active-directory-passwords-licensing.md)
* [Welche Daten werden von SSPR verwendet, und welche Daten sollten Sie für Ihre Benutzer angeben?](active-directory-passwords-data.md)
* [Welche Authentifizierungsmethoden sind für Benutzer verfügbar?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Welche Richtlinienoptionen stehen mit SSPR zur Verfügung?](active-directory-passwords-policy.md)
* [Worum handelt es sich beim Rückschreiben von Kennwörtern, und warum sollte ich mir Gedanken darüber machen?](active-directory-passwords-writeback.md)
* [Wie melde ich eine Aktivität bei SSPR?](active-directory-passwords-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Ich glaube, ein Fehler ist aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie die Self-Service-Kennwortzurücksetzung für Ihre Benutzer konfigurieren. Klicken Sie auf den Link unten zum Portal, um diese Schritte im Azure-Portal auszuführen.

> [!div class="nextstepaction"]
> [Aktivieren der Self-Service-Kennwortzurücksetzung](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Verfügbare Azure AD-Authentifizierungsmethoden und erforderliche Menge"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Lokale Kennwortgruppenrichtlinie festgelegt auf „0 Tage“"

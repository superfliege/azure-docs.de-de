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
ms.date: 11/16/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0320cb33dcab63ea394cf8e13e9aef5cc75951fa
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Schnelle Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD

> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen unter [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md) weiter.

Die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) ist für IT-Administratoren eine einfache Möglichkeit, Benutzern das Zurücksetzen oder Entsperren ihrer Kennwörter oder Konten zu erlauben. Das System verfügt über eine ausführliche Berichterstellung, bei der der Benutzerzugriff auf das System nachverfolgt wird, sowie über eine Benachrichtigungsfunktion, mit der Sie über eine fehlerhafte oder missbräuchliche Nutzung informiert werden.

In dieser Anleitung wird vorausgesetzt, dass Sie bereits über einen funktionierenden Testmandanten oder einen lizenzierten Azure AD-Mandanten (Azure Active Directory) verfügen. Hilfe zur Einrichtung von Azure AD finden Sie unter [Erste Schritte mit einem Azure-Abonnement](get-started-azure-ad.md).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Aktivieren der SSPR für Ihren Azure AD-Mandanten

1. Wählen Sie unter Ihrem vorhandenen Azure AD-Mandanten die Option **Kennwortzurücksetzung**.

2. Wählen Sie auf der Seite **Eigenschaften** unter **Self-Service-Kennwortzurücksetzung aktiviert** eine der folgenden Optionen:
    * **None** (Keine): Niemand kann die SSPR-Funktionalität nutzen.
    * **Selected** (Ausgewählte): Nur Mitglieder einer bestimmten Azure AD-Gruppe, die Sie ausgewählt haben, können die SSPR-Funktionalität nutzen. Es wird empfohlen, eine Gruppe mit Benutzern zu definieren und diese Einstellung zu verwenden, wenn Sie diese Funktionalität für einen Proof of Concept-Vorgang bereitstellen.
    * **All** (Alle): Alle Benutzer mit Konten unter Ihrem Azure AD-Mandanten können die SSPR-Funktionalität nutzen. Diese Einstellung wird empfohlen, wenn Sie nach Abschluss eines Proof of Concept-Vorgangs bereit sind, die Funktion im gesamten Mandanten bereitzustellen.

3. Wählen Sie auf der Seite **Authentifizierungsmethoden** Folgendes aus:
    * **Anzahl von Methoden, die zurückgesetzt werden müssen**: Hierfür werden eine oder zwei Methoden unterstützt.
    * **Für Benutzer verfügbare Methoden**: Mindestens eine Methode ist erforderlich, aber es kann nicht schaden, wenn noch mehr Auswahl besteht.
        * **E-Mail**: Mit dieser Option wird eine E-Mail mit einem Code an die E-Mail-Adresse des Benutzers gesendet, die für die Authentifizierung konfiguriert ist.
        * **Mobiltelefon**: Mit dieser Option können Benutzer einen Anruf oder eine SMS mit einem Code unter der konfigurierten Mobiltelefonnummer erhalten.
        * **Bürotelefon**: Mit dieser Option erhalten Benutzer einen Anruf mit einem Code unter der konfigurierten Bürotelefonnummer.
        * **Sicherheitsfragen**: Wählen Sie hier Folgendes aus:
            * **Anzahl der für die Registrierung erforderlichen Fragen**: Das Minimum für eine erfolgreiche Registrierung. Ein Benutzer kann auswählen, dass er mehr Fragen beantworten möchte, um einen Pool mit möglichen Fragen zu erstellen. Diese Option kann auf drei bis fünf Fragen festgelegt werden und muss größer oder gleich der Anzahl von Fragen sein, die zum Zurücksetzen des Kennworts erforderlich sind. Benutzer können benutzerdefinierte Fragen hinzufügen, indem sie beim Auswählen der Sicherheitsfragen die Schaltfläche **Benutzerdefiniert** wählen.
            * **Anzahl der für die Zurücksetzung erforderlichen Fragen**: Sie können für diese Option drei bis fünf Fragen festlegen, die richtig beantwortet werden müssen, bevor Sie zulassen, dass das Kennwort eines Benutzers zurückgesetzt oder entsperrt wird.
            
    ![Authentifizierung][Authentication]

4. Empfohlen: Mit der Option **Anpassung** können Sie den Link **Wenden Sie sich an Ihren Administrator.** so ändern, dass er auf eine von Ihnen definierte Seite oder E-Mail-Adresse verweist. Es wird empfohlen, diesen Link für Ihre Benutzer beispielsweise auf eine vertraute E-Mail-Adresse oder Website festzulegen, die von Ihren Benutzern bereits für Fragen an den Support verwendet wird.

5. Optional: Die Seite **Registrierung** enthält folgende Optionen für Administratoren:
    * Erzwingen der Registrierung für Benutzer bei der Anmeldung
    * Festlegen der Anzahl von Tagen, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen

6. Optional: Die Seite **Benachrichtigungen** enthält folgende Optionen für Administratoren:
    * Benachrichtigen von Benutzern über Kennwortzurücksetzungen
    * Benachrichtigen aller Administratoren, wenn andere Administratoren ihr Kennwort zurücksetzen

Sie haben SSPR jetzt für Ihren Azure AD-Mandanten konfiguriert. Ihre Benutzer können nun ihr Kennwort ohne Eingreifen des Administrators anhand der Anweisungen unter [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md) und [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md) aktualisieren. Sie können den Vorgang hier beenden, wenn Sie nur in der Cloud arbeiten. Oder Sie können mit dem nächsten Abschnitt fortfahren, um die Synchronisierung von Kennwörtern für eine lokale Active Directory-Domäne zu konfigurieren.

> [!IMPORTANT]
> Testen Sie SSPR nicht als Administrator, sondern als normaler Benutzer, da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-an-existing-identity-source"></a>Konfigurieren der Synchronisierung mit einer vorhandenen Identitätsquelle

Um die lokale Identitätssynchronisierung mit Azure AD zu aktivieren, müssen Sie [Azure AD Connect](./connect/active-directory-aadconnect.md) auf einem Server Ihrer Organisation installieren und konfigurieren. Mit dieser Anwendung wird die Synchronisierung von Benutzern und Gruppen von Ihrer vorhandenen Identitätsquelle mit Ihrem Azure AD-Mandanten durchgeführt. Weitere Informationen finden Sie unter:

* [Aktualisieren der Windows Azure Active Directory-Synchronisierung und Azure Active Directory-Synchronisierung](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](./connect/active-directory-aadconnect-get-started-express.md)
* [Konfigurieren Sie das Kennwortrückschreiben](active-directory-passwords-writeback.md#configure-password-writeback), um Kennwörter aus Azure AD wieder in Ihr lokales Verzeichnis zu schreiben.

### <a name="on-premises-policy-change"></a>Änderung der lokalen Richtlinie

Ändern Sie die lokale Kennwortrichtlinie wie folgt, wenn Sie Benutzer aus einer lokalen Active Directory-Domäne synchronisieren und Benutzern die sofortige Zurücksetzung ihrer Kennwörter ermöglichen möchten:

1. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Kontorichtlinien** > **Kennwortrichtlinie**.

2. Legen Sie **Minimales Kennwortalter** auf **0 Tage** fest.

Diese Sicherheitseinstellung bestimmt den Zeitraum in Tagen, für den ein Kennwort verwendet werden muss, bevor der Benutzer es ändern kann. Wenn Sie die Einstellung für die Mindestnutzungsdauer auf **0 Tage** festlegen, können Benutzer SSPR verwenden, wenn ihre Kennwörter von den Supportteams geändert werden.

![Richtlinie][Policy]

## <a name="disable-self-service-password-reset"></a>Deaktivieren der Self-Service-Kennwortzurücksetzung

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Kennwortzurücksetzung** > **Eigenschaften**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine**.

### <a name="learn-more"></a>Weitere Informationen
Die folgenden Artikel führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](active-directory-passwords-policy.md)
* [Übersicht über die Kennwortrückschreibung](active-directory-passwords-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](active-directory-passwords-reporting.md)
* [Ausführliche Informationen zur Self-Service-Kennwortzurücksetzung in Azure AD](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Häufig gestellte Fragen zur Kennwortverwaltung](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie die Self-Service-Kennwortzurücksetzung für Ihre Benutzer konfigurieren. Fahren Sie zum Ausführen dieser Schritte im Azure-Portal fort:

> [!div class="nextstepaction"]
> [Aktivieren der Self-Service-Kennwortzurücksetzung](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Verfügbare Azure AD-Authentifizierungsmethoden und erforderliche Menge"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Lokale Kennwortgruppenrichtlinie festgelegt auf „0 Tage“"


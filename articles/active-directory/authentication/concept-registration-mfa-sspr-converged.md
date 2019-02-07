---
title: Zusammengeführte Registrierung für Azure AD SSPR und MFA (öffentliche Vorschauversion)
description: Registrierung für die Azure AD Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: a3058eb3472b2bc9109ebc7b93b83b9f27091edd
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487590"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Zusammengeführte Registrierung für Self-Service-Kennwortzurücksetzung und Azure Multi-Factor Authentication (öffentliche Vorschauversion)

Bisher mussten Benutzer Authentifizierungsmethoden für Azure Multi-Factor Authentication (MFA) und Self-Service-Kennwortzurücksetzung (SSPR) in zwei verschiedenen Portalen registrieren. Viele Anwender waren verwirrt, dass ähnliche Methoden sowohl für Azure MFA als auch für SSPR verwendet wurden und diese nicht in beiden Portalen registriert werden konnten. Einige Benutzer konnten dann bei Bedarf weder Azure MFA noch SSPR verwenden, und riefen beim Helpdesk an. 

Jetzt können sich Benutzer einmalig registrieren und die Vorteile von sowohl Azure MFA als auch SSPR nutzen. Benutzer müssen ihre Authentifizierungsmethoden für diese Funktionen nicht zweimal registrieren.  

Bevor Sie diese neue Oberfläche für Ihre Organisation aktivieren, empfehlen wir Ihnen, diese Artikel und unsere [Benutzerdokumentation](https://aka.ms/securityinfoguide) zu lesen, um zu verstehen, welche Auswirkungen dies auf Ihre Benutzer haben wird. Sie können die Benutzerdokumentation verwenden, um Ihre Benutzer auf die neue Oberfläche vorzubereiten und so einen erfolgreichen Rollout sicherstellen.

|     |
| --- |
| Zusammengeführte Registrierung für Self-Service-Kennwortzurücksetzung und Azure Multi-Factor Authentication ist ein öffentliches Vorschaufeature von Azure Active Directory (Azure AD). Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Damit Ihre Benutzer die Authentifizierungsmethoden für die Azure Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung in einem einzigen Vorgang registrieren können, führen Sie die folgenden Schritte aus:

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an.
2. Navigieren Sie zu **Azure Active Directory** > **Benutzereinstellungen** > **Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.
3. Unter **Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden.** können Sie auswählen, ob Sie die Funktion für eine **Ausgewählte** Gruppe an Benutzern oder für **Alle** Benutzer aktivieren.

Benutzer können jetzt [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) aufrufen, um Ihre Authentifizierungsmethoden für sowohl MFA als auch SSPR zu registrieren. Wenn Sie weitere Informationen dazu benötigen, was Ihre Benutzer mit der neuen Oberfläche erwartet, lesen Sie die [Benutzerdokumentation](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Sobald Sie diese Oberfläche aktiviert haben, können Benutzer, die ihre Telefonnummer oder mobile App durch die neue Funktion registrieren oder bestätigen, diese für MFA und SSPR verwenden, wenn diese Methoden in den MFA- und SSPR-Richtlinien aktiviert sind. Wenn Sie diese Oberfläche dann deaktivieren, müssen Benutzer, die zur vorherigen SSPR-Registrierungsseite unter https:/aka.ms/ssprsetup navigieren, die mehrstufige Authentifizierung durchführen, bevor sie auf die Seite zugreifen können.  

## <a name="how-it-works"></a>So funktioniert's

Wenn ein Benutzer zuvor Authentifizierungsmethoden über die separate MFA- und SSPR-Registrierung registriert hat, muss er diese Informationen nicht erneut registrieren. Wenn Ihre Einstellungen jedoch erfordern, dass sich Benutzer für MFA oder SSPR registrieren müssen, werden sie beim Anmelden möglicherweise aufgefordert, ihre Sicherheitsinformationen zu überprüfen.

Wenn ein Benutzer eine Methode registriert hat, die für MFA verwendet werden kann, wird er aufgefordert, die mehrstufige Authentifizierung durchzuführen, bevor er auf die neue Oberfläche zugreifen kann.

Wenn Sie die Registrierung für MFA oder SSPR erzwungen haben und ein Benutzer noch nicht registriert ist, wird er bei der Anmeldung zur Registrierung aufgefordert.

Benutzern, die bei der Anmeldung aufgefordert werden, sich zu registrieren, wird folgendes angezeigt:

![Zusammengeführte Registrierung. Einrichten von Methoden als neuer Benutzer](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Dies wird nur angezeigt, wenn ein Benutzer bei der Anmeldung aufgefordert wird, sich zu registrieren. Benutzer, die direkt auf die Oberfläche auf https://aka.ms/setupsecurityinfo zugreifen, sehen eine andere Version der Oberfläche, die später in diesem Artikel beschrieben wird.

Die angezeigten Authentifizierungsmethoden ändern sich basierend auf den in Ihren MFA- und SSPR-Richtlinien aktivierten Methoden. Der Benutzer wird aufgefordert, die Mindestanzahl von Authentifizierungsmethoden zu registrieren, die erforderlich sind, um die MFA-Richtlinie, die SSPR-Richtlinie oder beides zu erfüllen. Wenn der Benutzer flexibel auswählen kann, welche Authentifizierungsmethoden er registriert, kann er auf **Sicherheitsinformationen auswählen** klicken, um andere Authentifizierungsmethoden auszuwählen.  

> [!NOTE]
> Wenn Sie das Empfangen einer Benachrichtigung in der mobilen App und das Erhalten eines Codes in der mobilen App aktivieren, können Benutzer, die die Microsoft Authenticator-App mithilfe einer Benachrichtigung registrieren, sowohl die Benachrichtigung als auch den Code zum Bestätigen ihrer Identität verwenden.

Im Gegensatz zur bisherigen MFA-Registrierungsoberfläche werden die Benutzer nicht aufgefordert, ein App-Kennwort zu registrieren, wenn sie die neue Registrierung durchführen. Stattdessen sollten sie den Schritten folgen, die in unserem Tutorial zu App-Kennwörtern aufgeführt sind, um App-Kennwörter in der neuen Oberfläche zu registrieren.  

Sobald ein Benutzer die Registrierung abgeschlossen hat, wird seine standardmäßige MFA-Methode automatisch eingestellt. Wenn der Benutzer eine Authentifikator-App registriert, wird die Standardmethode auf „App“ festgelegt. Wenn der Benutzer keine Authentifikator-App registriert, sondern nur seine Telefonnummer, wird die Standardmethode auf „Telefonanruf“ gesetzt. Benutzer können ihre Standardwerte ändern, indem sie https://aka.ms/setupsecurityinfo aufrufen und **Standard ändern**  auswählen.  

Wenn die Registrierung nicht erzwungen wird, können Benutzer unter https://aka.ms/setupsecurityinfo ihre eigenen Authentifizierungsmethoden verwalten. Wenn ein Benutzer vorher eine Methode registriert hat, die für MFA verwendet werden kann, wird er aufgefordert, die mehrstufige Authentifizierung durchzuführen, bevor er auf die Seite zugreifen kann.  

![Zusammengeführte Registrierung. Bearbeiten von Methoden als registrierter Benutzer](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Auf dieser Seite sehen Benutzer bereits registrierte Authentifizierungsmethoden und für sie registrierte Authentifizierungsmethoden, wie z.B. das Bürotelefon. Benutzer können ihre Authentifizierungsmethoden auch hinzufügen, bearbeiten oder löschen (mit Ausnahme von Bürotelefon).  

Überwachungsprotokolle für diese neue Oberfläche finden Sie in der Kategorie „Authentifizierungsmethode“ des Überwachungsprotokolls.  

## <a name="known-issues"></a>Bekannte Probleme

**Standardmäßige MFA-Methode wird auf „Telefonanruf“ festgelegt, wenn der Benutzer „Telefon“ per SMS registriert**

   * Einige Benutzer werden feststellen, dass ihre standardmäßige MFA-Methode auf „Telefonanruf“ festgelegt ist, nachdem sie ihre Telefonnummer per SMS registriert haben. Benutzer können dieses Problem beheben, indem sie ihre Standardmethode ändern. Die entsprechende Anleitung ist im Artikel [Verwalten der Sicherheitsinformation (Vorschauversion)](../user-help/security-info-manage-settings.md#change-your-info) enthalten.

**Benutzer können nicht auf die neue Registrierungsoberfläche zugreifen, nachdem der Administrator ihre Standardmethode deaktiviert**

   * Einige Benutzer können möglicherweise nicht auf die neue Registrierungsoberfläche zugreifen, wenn ihr Administrator ihre zuvor registrierte standardmäßige MFA-Methode deaktiviert hat. Hier ist ein Beispielszenario:
      1. Der Benutzer hat zuvor seine Telefonnummer registriert und seine Standardmethode auf „Telefonanruf“ festgelegt.
      2. Der Administrator deaktiviert „Telefonanruf“ als MFA-Methode für den Mandanten.
      3. Der Benutzer wird während der Anmeldung aufgefordert, sich zu registrieren, da er eine zusätzliche Methode registrieren muss, um die SSPR-Richtlinie des Mandanten zu erfüllen.
      4. Der Benutzer versucht sich zu registrieren, kann aber nicht auf die Seite zugreifen und steckt in einer Schleife fest, weil keine Standardmethode eingestellt ist.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über die Bereitstellung der Self-Service-Kennwortzurücksetzung in Azure AD](howto-sspr-deployment.md)

[Erfahren Sie mehr zum Erzwingen der mehrstufigen Authentifizierung beim Anmelden](howto-mfa-getstarted.md)

[Konfigurieren der Benutzerauthentifizierungsmethoden](https://aka.ms/securityinfoguide)

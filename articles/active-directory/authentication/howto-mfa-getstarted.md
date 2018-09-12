---
title: Erste Schritte mit Azure MFA in der Cloud
description: Erste Schritte mit Microsoft Azure Multi-Factor Authentication mit bedingtem Zugriff
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 0408b26e687dd31c408dbccc68f56e8198016c8f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43664787"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>Bereitstellen von cloudbasierter Azure Multi-Factor Authentication

Die ersten Schritte mit Azure Multi-Factor Authentication (Azure MFA) sind sehr einfach.

Stellen zu zuerst sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Ein globales Administratorkonto in Ihrem Azure AD-Mandanten. Wenn Sie Hilfe bei diesem Schritt benötigen, lesen Sie unseren Artikel [Erste Schritte mit Azure AD](../get-started-azure-ad.md).
* Den Benutzern sind die richtigen Lizenzen zugewiesen. Weitere Informationen finden Sie unter [Beziehen von Azure Multi-Factor Authentication](concept-mfa-licensing.md).

## <a name="choose-how-to-enable"></a>Auswählen der Art und Weise der Aktivierung

**Aktiviert mit der Richtlinie für bedingten Zugriff** – Diese Methode wird in diesem Artikel erläutert. Dies ist die flexibelste Möglichkeit, die zweistufige Überprüfung für Ihre Benutzer zu aktivieren. Die Aktivierung mit der Richtlinie für bedingten Zugriff funktioniert jedoch nur bei Azure MFA in der Cloud und ist eine Premium-Funktion von Azure AD.

Aktiviert durch Azure AD Identity Protection – Diese Methode nutzt die Risikorichtlinie von Azure AD Identity Protection, um die zweistufige Überprüfung ausschließlich auf Basis des Anmelderisikos für alle Cloudanwendungen zu erzwingen. Diese Methode erfordert die Azure Active Directory P2-Lizenzierung. Weitere Informationen zu dieser Methode finden Sie unter [Azure Active Directory Identity Protection](../identity-protection/overview.md#risky-sign-ins).

Aktiviert durch Ändern des Benutzerstatus – Dies ist die herkömmliche Methode, die zweistufige Überprüfung zu erfordern. Dies funktioniert sowohl für Azure MFA in der Cloud als auch für Azure MFA Server. Diese Methode erfordert, dass Benutzer **jedes Mal**, wenn sie sich anmelden, die zweistufige Überprüfung durchführen, und überschreibt Richtlinien für bedingten Zugriff. Weitere Informationen zu dieser Methode finden Sie in [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](howto-mfa-userstates.md).

> [!Note]
> Weitere Informationen zu Lizenzen und Preisen finden Sie auf den Preisgestaltungsseiten von [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) und [mehrstufiger Authentifizierung](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="choose-authentication-methods"></a>Auswählen der Authentifizierungsmethoden

Aktivieren Sie basierend auf den Anforderungen Ihrer Organisation mindestens eine Authentifizierungsmethode für Ihre Benutzer. Wir denken, dass die Microsoft Authenticator-App die beste Benutzererfahrung bietet, wenn sie für Benutzer aktiviert ist. Wenn Sie wissen möchten, welche Methoden verfügbar sind und wie diese festgelegt werden, lesen Sie den Artikel [Was sind Authentifizierungsmethoden](concept-authentication-methods.md).

## <a name="get-users-to-enroll"></a>Benutzer registrieren

Nachdem Sie die Richtlinie für bedingten Zugriff aktiviert haben, müssen sich Benutzer registrieren, wenn die beim nächsten Mal eine App verwenden, die von dieser Richtlinie geschützt ist. Wenn Sie eine Richtlinie aktivieren, die für alle Benutzer in allen Cloud-Apps MFA erfordert, könnte diese Aktion für Ihre Benutzer und Ihr Helpdesk zu Schwierigkeiten führen. Es wird empfohlen, die Benutzer aufzufordern, Authentifizierungsmethoden vorab über das Registrierungsportal unter [https://aka.ms/mfasetup](https://aka.ms/mfasetup) zu registrieren. Viele Organisationen stellen fest, dass die Erstellung von Postern, Tischkarten und E-Mail-Nachrichten die Akzeptanz fördert.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Aktivieren von Multi-Factor Authentication mit bedingtem Zugriff

Melden Sie sich mit dem globalen Administratorkonto am [Azure-Portal](https://portal.azure.com) an.

### <a name="choose-verification-options"></a>Auswählen der Überprüfungsoptionen

Bevor Sie die Azure Multi-Factor Authentication aktivieren, muss Ihre Organisation festlegen, welche Überprüfungsoptionen zugelassen werden. Für den Zweck dieser Übung aktivieren Sie „Auf Telefon anrufen“ und „Textnachricht an Telefon“, da es sich um generische Optionen handelt, die die meisten verwenden können. Weitere Informationen zu Authentifizierungsmethoden und deren Verwendung finden Sie im Artikel [Was sind Authentifizierungsmethoden?](concept-authentication-methods.md)

1. Navigieren Sie zu **Azure Active Directory**, **Benutzer**, **Multi-Factor Authentication**
   ![Zugreifen auf das Multi-Factor Authentication-Portal über das Blatt „Azure AD-Benutzer“ im Azure-Portal](media/howto-mfa-getstarted/users-mfa.png) 
2. Navigieren Sie in der neuen Registerkarte, die geöffnet wird, zu **Diensteinstellungen**
3. Aktivieren Sie unter **Überprüfungsoptionen** die folgenden Kontrollkästchen für Methoden, die den Benutzern zur Verfügung stehen sollen:
   * Auf Telefon anrufen
   * Textnachricht an Telefon

   ![Konfigurieren von Überprüfungsmethoden, in der Registerkarte „Diensteinstellung“ von Multi-Factor Authentication](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. Klicken Sie im Menü „Einstellungen“ auf **Speichern**
5. Schließen Sie die Registerkarte **Diensteinstellungen**.

### <a name="create-conditional-access-policy"></a>Erstellen der Richtlinie für den bedingten Zugriff

1. Melden Sie sich mit dem globalen Administratorkonto am [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory**  > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Geben Sie einen aussagekräftigen Namen für die Richtlinie an.
1. Unter **Benutzer und Gruppen**.
   * Wählen Sie auf der Registerkarte **Einschließen** das Optionsfeld **Alle Benutzer**.
   * EMPFOHLEN: Aktivieren Sie auf der Registerkarte **Ausschließen** das Kontrollkästchen für **Benutzer und Gruppen** und wählen Sie eine Gruppe, die für Ausschlüsse verwendet werden soll, wenn Benutzer keinen Zugriff auf ihre Authentifizierungsmethoden haben.
   * Klicken Sie auf **Fertig**.
1. Wählen Sie unter **Cloud-Apps** das Optionsfeld **Alle Cloud-Apps** aus.
   * OPTIONAL:  Wählen Sie auf der Registerkarte **Ausschließen** Cloud-Apps, für die Ihre Organisation keine MFA benötigt.
   * Klicken Sie auf **Fertig**.
1. Im Abschnitt **Bedingungen**.
   * OPTIONAL: Wenn Sie die Azure Identity Protection aktiviert haben, können Sie entscheiden, ob das Anmelderisiko als Teil der Richtlinie ausgewertet werden soll.
   * Optional: Wenn Sie vertrauenswürdige Speicherorte oder benannte Orte konfiguriert haben, können Sie angeben, ob diese in die Richtlinie ein- oder von dieser ausgeschlossen werden.
1. Stellen Sie unter **Gewähren** sicher, dass das Optionsfeld **Zugriff gewähren** ausgewählt ist.
    * Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication erforderlich**.
    * Klicken Sie auf **Auswählen**.
1. Überspringen Sie den Abschnitt **Sitzung**.
1. Legen Sie den Umschalter **Richtlinie aktivieren** auf **Ein** fest.
1. Klicken Sie auf **Erstellen**

![Erstellen Sie eine Richtlinie für bedingten Zugriff, um MFA für Azure-Portal-Benutzer in der Pilotgruppe zu aktivieren.](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

### <a name="test-azure-multi-factor-authentication"></a>Testen der Azure Multi-Factor Authentication

Um zu prüfen, ob Ihre Richtlinie für bedingten Zugriff funktioniert, testen Sie das Anmelden an einer Ressource, für die MFA nicht erforderlich ist, und dann am Azure-Portal mit MFA.

1. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Melden Sie sich mit dem Testbenutzer an, den Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben. Sie sollten nicht zur Durchführung des MFA-Vorgangs aufgefordert werden.
   * Schließen Sie das Browserfenster.
2. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://portal.azure.com](https://portal.azure.com).
   * Melden Sie sich mit dem Testbenutzer an, den Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben. Sie sollten jetzt aufgefordert werden, die Registrierung für Azure Multi-Factor Authentication durchzuführen und diese Art der Authentifizierung zu nutzen.
   * Schließen Sie das Browserfenster.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch, Sie haben Azure Multi-Factor Authentication in der Cloud eingerichtet.

Wie Sie zusätzliche Einstellungen wie vertrauenswürdige IP-Adressen, benutzerdefinierte Sprachnachrichten und Betrugswarnungen konfigurieren, erfahren Sie im Artikel [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md).

Informationen zum Verwalten von Benutzereinstellungen für die Microsoft Azure Multi-Factor Authentication finden Sie im Artikel [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](howto-mfa-userdevicesettings.md).

Lesen Sie außerdem, wie Sie die [zusammengeführte Registrierung für Azure Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung in Azure AD aktivieren](concept-registration-mfa-sspr-converged.md).

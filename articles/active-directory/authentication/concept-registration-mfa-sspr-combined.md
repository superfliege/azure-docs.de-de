---
title: Kombinierte Registrierung für Azure AD SSPR und MFA (Vorschauversion)
description: Registrierung für Azure AD Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung (Vorschauversion)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6399f5b208bc4d7182622f29bd522cfc9a088e9d
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224548"
---
# <a name="combined-security-information-registration-preview"></a>Kombinierte Registrierung von Sicherheitsinformationen (Vorschauversion)

Vor der kombinierten Registrierung registrierten Benutzer Authentifizierungsmethoden für Azure Multi-Factor Authentication (MFA) und die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) über zwei verschiedene Benutzeroberflächen. Benutzer waren verwirrt, dass ähnliche Methoden für Azure MFA und für SSPR verwendet wurden, dass sie sich jedoch für jede Funktion getrennt registrieren mussten. Mit der kombinierten Registrierung können sich Benutzer jetzt einmalig registrieren und die Vorteile von Azure MFA sowie von SSPR nutzen.

![Kombinierte Sicherheitsinformationen: „Mein Profil“ mit registrierten Sicherheitsinformationen für einen Benutzer, einschließlich Microsoft Authenticator und Telefon für einen Beispielbenutzer im Verzeichnis.](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Lesen Sie vor dem Aktivieren der neuen Funktion diese an Administratoren gerichtete Dokumentation und die benutzerorientierte Dokumentation, um sich mit der Funktionalität und den Auswirkungen dieser Funktion vertraut zu machen. Ziehen Sie für Ihre Schulungen die Benutzerdokumentation heran, um Ihre Benutzer auf die neue Oberfläche vorzubereiten und so einen erfolgreichen Rollout sicherzustellen.

|     |
| --- |
| Die kombinierte Registrierung von Sicherheitsinformationen für Azure Multi-Factor Authentication und die Azure AD-Self-Service-Kennwortzurücksetzung ist eine Funktion der öffentlichen Vorschau von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!IMPORTANT]
> Wenn Benutzer für die ursprüngliche Vorschauversion sowie für die erweiterte kombinierte Registrierung aktiviert sind, können sie die neue Oberfläche sehen. Benutzer, die für beide Funktionen aktiviert sind, sehen nur die neue Oberfläche „Mein Profil“. Die neue Oberfläche „Mein Profil“ hat das Erscheinungsbild der kombinierten Registrierung und bietet Benutzern eine einheitliche Oberfläche. Benutzer können „Mein Profil“ anzeigen, indem sie zu [https://myprofile.microsoft.com](https://myprofile.microsoft.com) navigieren.

Die Seite „Mein Profil“ ist entsprechend den aktuellen Spracheinstellungen auf dem Computer lokalisiert, über den auf die Seite zugegriffen wird. Microsoft speichert die aktuell verwendete Sprache im Browsercache, sodass nachfolgende Zugriffsversuche weiterhin in der zuletzt verwendeten Sprache angezeigt werden. Durch Löschen des Caches werden die Seiten neu angezeigt. Wenn Sie eine bestimmte Sprache festlegen möchten, wird durch Hinzufügen von `?lng=de-DE` am Ende der URL (`de-DE` wird dabei auf den entsprechenden Sprachcode festgelegt) erzwungen, dass die Seiten in dieser Sprache angezeigt werden.

![Oberfläche „Mein Profil“ mit Sicherheitsinformationen und der Möglichkeit für den Benutzer, SSPR oder andere zusätzliche Sicherheitsüberprüfungsmethoden einzurichten.](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-converged-registration"></a>Verfügbare Methoden in der kombinierten Registrierung

Derzeit werden in der kombinierten Registrierung folgende Methoden sowie folgende Aktionen für diese Methoden unterstützt:

|   | Register  | Change | Löschen |
| --- | --- | --- | --- |
| Microsoft Authenticator | Ja (max. 5) | Nein  | Ja |
| Andere Authenticator-App | Ja (max. 5) | Nein  | Ja |
| Hardwaretoken | Nein  | Nein  | Ja |
| Phone | Ja | Ja | Ja |
| Alternatives Telefon | Ja | Ja | Ja |
| Bürotelefon | Nein  | Nein  | Nein  |
| E-Mail | Ja | Ja | Ja |
| Sicherheitsfragen | Ja | Nein | Ja |
| App-Kennwörter | Ja | Nein | Ja |

> [!NOTE]
> App-Kennwörter sind nur für Benutzer verfügbar, für die MFA erzwungen wurde. App-Kennwörter sind nicht verfügbar für Benutzer, für die über eine Richtlinie für bedingten Zugriff MFA aktiviert wurde.

Benutzer können die folgenden Optionen als Standardmethode für MFA festlegen:

- Microsoft Authenticator – Benachrichtigung
- Authenticator-App oder Hardwaretoken – Code
- Telefonanruf
- Textnachricht

Da wir weiterhin weitere Authentifizierungsmethoden hinzufügen, z. B. Azure AD, werden dann auch diese Methoden bei der kombinierten Registrierung verfügbar sein.

## <a name="combined-registration-modes"></a>Modi der kombinierten Registrierung

Zwei Modi der kombinierten Registrierung sind verfügbar: Interruptmodus und Verwaltungsmodus.

Beim Interruptmodus handelt es sich um eine assistentenähnliche Oberfläche, die für Benutzer angezeigt wird, wenn sie ihre Sicherheitsinformationen bei der Anmeldung registrieren oder aktualisieren.

Der Verwaltungsmodus ist Teil des Benutzerprofils und ermöglicht Benutzern die Verwaltung ihrer Sicherheitsinformationen.

Wenn ein Benutzer zuvor eine Methode registriert hat, die für MFA verwendet werden kann, wird er in beiden Modi aufgefordert, MFA durchzuführen, bevor er auf seine Sicherheitsinformationen zugreifen kann.

### <a name="interrupt-mode"></a>Interruptmodus

Bei der kombinierten Registrierung werden sowohl MFA- als auch SSPR-Richtlinien eingehalten, wenn beide für Ihren Mandanten aktiviert sind. Über diese Richtlinien wird gesteuert, ob die Anmeldung eines Benutzer unterbrochen und dieser aufgefordert wird, die Registrierung durchzuführen, und welche Methoden für die Registrierung verfügbar sind.

Im Folgenden sind mehrere Szenarien aufgeführt, bei denen ein Benutzer aufgefordert werden kann, seine Sicherheitsinformationen zu registrieren oder zu aktualisieren:

* Durch Identity Protection erzwungene MFA-Registrierung: Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren MFA-Methoden und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
* Durch MFA pro Benutzer erzwungene MFA-Registrierung: Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren MFA-Methoden und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
* Durch Richtlinien für bedingten Zugriff oder andere Richtlinien erzwungene MFA-Registrierung: Benutzer werden beim Zugriff auf eine Ressource, für die MFA erforderlich ist, aufgefordert, die Registrierung durchzuführen. Die Benutzer registrieren MFA-Methoden und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
* Erzwungene SSPR-Registrierung: Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren nur SSPR-Methoden.
* Erzwungene SSPR-Aktualisierung: Benutzer werden in einem vom Administrator festgelegten Intervall aufgefordert, ihre Sicherheitsinformationen zu überprüfen. Die Benutzer prüfen ihre Informationen und können „Sieht gut aus“ auswählen oder bei Bedarf Änderungen vornehmen.

Wenn die Registrierung erzwungen wird, wird den Benutzern die Mindestanzahl der Methoden angezeigt (angefangen bei der sichersten bis zur am wenigsten sicheren Methode), die erforderlich sind, um den MFA- sowie den SSPR-Richtlinien zu entsprechen.

Beispiel:

* Ein Benutzer ist für SSPR aktiviert. Die SSPR-Richtlinie erfordert zwei Methoden zum Zurücksetzen und aktiviert den Code der mobilen App sowie E-Mail und Telefon.
   * Der Benutzer muss zwei Methoden registrieren.
      * Standardmäßig werden ihm die Authenticator-App und das Telefon angezeigt.
      * Der Benutzer hat die Möglichkeit, statt der Authenticator-App oder des Telefons die E-Mail-Adresse zu registrieren.

Im folgenden Flussdiagramm sind die Methoden beschrieben, die für einen Benutzer angezeigt werden, wenn seine Anmeldung unterbrochen und er aufgefordert wird, die Registrierung durchzuführen:

![Flussdiagramm für kombinierte Registrierung von Sicherheitsinformationen: Angabe der Anzahl der erforderlichen Methoden, wenn bei der Anmeldung weitere Informationen erforderlich sind. Dies kann sich ändern, wenn nur MFA oder nur SSPR erforderlich ist.](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Wenn Sie sowohl MFA als auch SSPR aktiviert haben, wird empfohlen, dass Sie die MFA-Registrierung erzwingen.

Wenn Benutzer über die SSPR-Richtlinie in regelmäßigen Abständen aufgefordert werden, ihre Sicherheitsinformationen zu überprüfen, wird die Anmeldung der Benutzer unterbrochen, und es werden alle registrierten Methoden angezeigt. Die Benutzer können „Sieht gut aus“ auswählen, wenn die Informationen auf dem neuesten Stand sind, oder „Informationen bearbeiten“, um Änderungen vorzunehmen.

### <a name="manage-mode"></a>Verwaltungsmodus

Benutzer haben Zugriff auf den Verwaltungsmodus, indem sie zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) navigieren oder unter „Mein Profil“ die Option „Sicherheitsinformation“ auswählen. Dort können Benutzer Methoden hinzufügen, vorhandene Methoden löschen oder ändern, die verwendete Standardmethode ändern und vieles mehr.

## <a name="key-usage-scenarios"></a>Wichtige Verwendungsszenarien

### <a name="set-up-security-info-during-sign-in"></a>Einrichten der Sicherheitsinformationen bei der Anmeldung

Ein Administrator hat die Registrierung erzwungen.

Ein Benutzer hat nicht alle erforderlichen Sicherheitsinformationen eingerichtet und navigiert zum Azure-Portal. Nach der Eingabe des Benutzernamens und Kennworts wird der Benutzer aufgefordert, die Sicherheitsinformationen einzurichten. Der Benutzer führt dann die im Assistenten angezeigten Schritte zum Einrichten der erforderlichen Sicherheitsinformationen aus. Der Benutzer kann auch andere Methoden als die standardmäßig angezeigten Methoden einrichten, wenn dies in den Einstellungen zulässig ist. Am Ende des Assistenten überprüft der Benutzer die eingerichteten Methoden und die Standardmethode für MFA. Um den Einrichtungsvorgang abzuschließen, bestätigt der Benutzer die Informationen und navigiert dann zum Azure-Portal.

### <a name="set-up-security-info-from-my-profile"></a>Einrichten der Sicherheitsinformationen unter „Mein Profil“

Ein Administrator hat die Registrierung nicht erzwungen.

Ein Benutzer, der noch nicht alle erforderlichen Sicherheitsinformationen eingerichtet hat, navigiert zu [https://myprofile.microsoft.com](https://myprofile.microsoft.com). Der Benutzer wählt dann im linken Navigationsbereich die Option **Sicherheitsinformation** aus. Dort gibt der Benutzer das Hinzufügen einer Methode an, wählt eine der verfügbaren Methoden aus und führt die Schritte zum Einrichten dieser Methode aus. Nach Abschluss des Vorgangs wird die eingerichtete Methode auf der Seite „Sicherheitsinformation“ angezeigt.

### <a name="delete-security-info-from-my-profile"></a>Löschen der Sicherheitsinformationen unter „Mein Profil“

Ein Benutzer, der zuvor mindestens eine Methode eingerichtet hat, navigiert zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo). Der Benutzer löscht eine der zuvor registrierten Methoden. Anschließend wird diese Methode nicht mehr auf der Seite „Sicherheitsinformation“ angezeigt.

### <a name="change-default-method-from-my-profile"></a>Ändern der Standardmethode unter „Mein Profil“

Ein Benutzer, der zuvor mindestens eine Methode eingerichtet hat, die für MFA verwendet werden kann, navigiert zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo). Der Benutzer ändert die aktuell verwendete Standardmethode in eine andere Standardmethode. Anschließend wird die neue Standardmethode auf der Seite „Sicherheitsinformation“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren der kombinierten Registrierung in Ihrem Mandanten](howto-registration-mfa-sspr-combined.md)

[Verfügbare Methoden für MFA und SSPR](concept-authentication-methods.md)

[Konfigurieren der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)

[Konfigurieren von Azure Multi-Factor Authentication](howto-mfa-getstarted.md)

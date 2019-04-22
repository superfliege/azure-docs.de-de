---
title: Erste Schritte mit der kombinierten Registrierung für Azure AD-SSPR und mehrstufige Authentifizierung (Vorschau) – Azure Active Directory
description: Aktivieren der kombinierten Registrierung für Azure AD Multi-Factor Authentication und die Self-Service-Kennwortzurücksetzung (Vorschauversion)
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
ms.openlocfilehash: 3baf2690ae07b87bb4d5dba30fcd20f62a1a4506
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280569"
---
# <a name="enable-combined-security-information-registration-preview"></a>Aktivieren der kombinierten Registrierung von Sicherheitsinformationen (Vorschauversion)

Lesen Sie vor dem Aktivieren der neuen Funktion den Artikel [Kombinierte Registrierung von Sicherheitsinformationen (Vorschauversion)](concept-registration-mfa-sspr-combined.md), um sich mit der Funktionalität und den Auswirkungen dieser Funktion vertraut zu machen.

![Verbesserte Oberfläche für die kombinierte Registrierung von Sicherheitsinformationen](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Die kombinierte Registrierung von Sicherheitsinformationen für Azure Multi-Factor Authentication und die Azure AD-Self-Service-Kennwortzurücksetzung (Azure Active Directory) ist eine Funktion der öffentlichen Vorschau von Azure AD. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="enable-combined-registration"></a>Aktivieren der kombinierten Registrierung

Führen Sie die folgenden Schritte aus, um die kombinierte Registrierung zu aktivieren:

1. Melden Sie sich als Benutzeradministrator oder globaler Administrator beim Azure-Portal an.
2. Navigieren Sie zu **Azure Active Directory** > **Benutzereinstellungen** > **Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.
3. Wählen Sie unter **Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden – Aktualisieren** aus, ob Sie die Funktion für eine **ausgewählte** Gruppe von Benutzern oder für **alle** Benutzer aktivieren möchten.

   ![Aktivieren der kombinierten Registrierung von Sicherheitsinformationen für alle Benutzer](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> Ab März 2019 sind die Telefonanrufoptionen für MFA- (Multi-Factor Authentication) und SSPR-Benutzer in kostenlosen bzw. Testversionen von Azure AD-Mandanten nicht mehr verfügbar. SMS-Nachrichten sind von dieser Änderung nicht betroffen. Für Benutzer in kostenpflichtigen Azure AD-Mandanten ist die Telefonanrufoption weiterhin verfügbar.

> [!NOTE]
> Nachdem Sie die kombinierte Registrierung aktiviert haben, können Benutzer, die ihre Telefonnummer oder mobile App durch die neue Funktion registrieren oder bestätigen, diese für die mehrstufige Authentifizierung und SSPR verwenden, wenn diese Methoden in den MFA- und SSPR-Richtlinien aktiviert sind. Wenn Sie diese Funktion dann deaktivieren, müssen Benutzer, die zur vorherigen SSPR-Registrierungsseite unter `https:/aka.ms/ssprsetup` navigieren, die mehrstufige Authentifizierung durchführen, bevor sie auf die Seite zugreifen können.

Wenn Sie die „Liste der Site zu Zonenzuweisungen“ in Internet Explorer konfiguriert haben, müssen sich die folgenden Websites in der gleichen Zone befinden:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Nächste Schritte

[Verfügbare Methoden für Multi-Factor Authentication und SSPR](concept-authentication-methods.md)

[Konfigurieren der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)

[Konfigurieren von Azure Multi-Factor Authentication](howto-mfa-getstarted.md)

[Problembehandlung für die kombinierte Registrierung von Sicherheitsinformationen](howto-registration-mfa-sspr-combined-troubleshoot.md)
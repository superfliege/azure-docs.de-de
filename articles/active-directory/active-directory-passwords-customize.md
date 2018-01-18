---
title: "Anpassung der Self-Service-Kennwortzurücksetzung – Azure Active Directory"
description: "Anpassungsoptionen für die Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 6d8a2f2106e57bdf84bc3bead70d379691b79742
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Anpassen der Azure AD-Funktionalität für die Self-Service-Kennwortzurücksetzung

IT-Experten, die die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) in Azure Active Directory (Azure AD) bereitstellen möchten, können die Bedienung an die Anforderungen ihrer Benutzer anpassen.

## <a name="customize-the-contact-your-administrator-link"></a>Anpassen des Links „Wenden Sie sich an Ihren Administrator“

Selbst wenn SSPR nicht aktiviert ist, gibt es für Benutzer im Portal für die Kennwortzurücksetzung den Link „Wenden Sie sich an Ihren Administrator“. Wählt ein Benutzer diesen Link aus, wird einer der folgenden Vorgänge ausgelöst:
   * Es wird eine E-Mail an Ihre Administratoren gesendet, in der diese gebeten werden, den Benutzer beim Ändern des Kennworts zu unterstützen. 
   * Die Benutzer werden an eine URL weitergeleitet, die Sie für Unterstützung angegeben haben. 

Es wird empfohlen, diesen Kontakt für Ihre Benutzer beispielsweise auf eine vertraute E-Mail-Adresse oder Website festzulegen, die von Ihren Benutzern bereits für Fragen an den Support verwendet wird.

![Kontakt][Contact]

Die Kontakt-E-Mail wird in der folgenden Reihenfolge an die folgenden Empfänger gesendet:

1. Wenn die Rolle **Kennwortadministrator** zugewiesen ist, werden die Administratoren mit dieser Rolle benachrichtigt.
2. Falls keine Kennwortadministratoren zugewiesen sind, werden die Administratoren mit der Rolle **Benutzeradministrator** benachrichtigt.
3. Wenn keine der obigen Rollen zugewiesen ist, werden die **globalen Administratoren** benachrichtigt.

In jedem Fall werden bis zu 100 Empfänger benachrichtigt.

Weitere Informationen zu den verschiedenen Administratorrollen und wie sie zugewiesen werden können finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>Deaktivieren von „Wenden Sie sich an Ihren Administrator“-E-Mails

Wenn Ihre Organisation nicht möchte, dass Administratoren über Anforderungen zur Kennwortzurücksetzung benachrichtigt werden, können Sie die folgende Konfiguration aktivieren:

* Aktivieren Sie die Self-Service-Kennwortzurücksetzung für alle Endbenutzer. Diese Option finden Sie unter **Zurücksetzen des Kennworts** > **Eigenschaften**.
  
  Wenn Sie nicht möchten, dass Benutzer ihre eigenen Kennwörter zurücksetzen, können Sie den Zugriff auf eine leere Gruppe beschränken. *Dies ist nicht zu empfehlen.*
* Passen Sie den Helpdesklink an, um eine Web-URL oder eine mailto:-Adresse bereitzustellen, die Benutzer verwenden können, um Unterstützung zu erhalten. Diese Option finden Sie unter **Zurücksetzen des Kennworts** > **Anpassung** > **Benutzerdefinierte Helpdesk-E-Mail oder -URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>Anpassen der AD FS-Anmeldeseite für SSPR

AD FS-Administratoren (Active Directory Federation Services) können einen Link zu ihrer Anmeldeseite hinzufügen, indem sie gemäß der Anleitung vorgehen, die im Artikel [Beschreibung der Anmeldeseite hinzufügen](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) zu finden ist.

Um der AD FS-Anmeldeseite einen Link hinzuzufügen, verwenden Sie den folgenden Befehl auf dem AD FS-Server. Benutzer können diese Seite verwenden, um den SSPR-Workflow einzugeben.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Anpassen des Aussehens und Verhaltens der Anmeldeseite und des Zugriffsbereichs

Sie können die Anmeldeseite anpassen. Sie können ein Logo hinzufügen, das zusammen mit dem Bild angezeigt wird, das zu Ihrem Unternehmensbranding gehört.

Die von Ihnen ausgewählten Grafiken werden in den folgenden Situationen angezeigt:

* Nachdem ein Benutzer seinen Benutzernamen eingegeben hat
* Wenn der Benutzer auf die angepasste URL zugreift:
    * Durch Übergeben des *whr*-Parameters an die Seite zum Zurücksetzen des Kennworts, z.B. „https://login.microsoftonline.com/?whr=contoso.com“
    * Durch Übergeben des *username*-Parameters an die Seite zum Zurücksetzen des Kennworts, z.B. „https://login.microsoftonline.com/?username=admin@contoso.com“

### <a name="graphics-details"></a>Grafikdetails

Verwenden Sie die folgenden Einstellungen, um die visuellen Merkmale der Anmeldeseite zu ändern. Wechseln Sie zu **Azure Active Directory** > **Unternehmensbranding** > **Unternehmensbranding bearbeiten**:

* Das Bild für die Anmeldeseite muss eine PNG- oder JPG-Datei mit 1420 x 1200 Pixel und einer maximalen Größe von 500 KB sein. Zum Erzielen der besten Ergebnisse empfiehlt sich eine Größe von etwa 200 KB.
* Die Hintergrundfarbe für die Anmeldeseite wird für Verbindungen mit langer Wartezeit verwendet, und sie muss im RGB-Hexadezimalformat vorliegen.
* Das Bannerbild muss eine PNG- oder JPG-Datei mit 60 x 280 Pixel und einer maximalen Größe von 10 KB sein.
* Das quadratische Logo (normales und dunkles Design) muss eine PNG- oder JPG-Datei mit 240 x 240 Pixel (änderbar) und einer maximalen Größe von 10 KB sein.

### <a name="sign-in-text-options"></a>Optionen für den Anmeldetext

Verwenden Sie die folgenden Einstellungen, um Text zur Anmeldeseite hinzuzufügen, der für Ihre Organisation relevant ist. Wechseln Sie zu **Azure Active Directory** > **Unternehmensbranding** > **Unternehmensbranding bearbeiten**:

* **Benutzernamenhinweis**: Ersetzt den Beispieltext von *someone@example.com* durch Text, der für Ihre Benutzer besser geeignet ist. Es empfiehlt sich, dass Sie den Standardhinweis belassen, wenn Sie interne und externe Benutzer unterstützen.
* **Text für die Anmeldeseite**: Kann bis zu maximal 256 Zeichen lang sein. Dieser Text wird überall angezeigt, wo sich Ihre Benutzer online anmelden, und wird in der Azure AD-Arbeitsbereichverknüpfung unter Windows 10 angezeigt. Verwenden Sie diesen Text für die Nutzungsbedingungen, Hinweise und Tipps für Ihre Benutzer. 

   >[!IMPORTANT]
   >Jeder kann die Anmeldeseite sehen. Stellen Sie hier also keine vertraulichen Informationen bereit.
   >

### <a name="the-keep-me-signed-in-disabled-setting"></a>Einstellung „Angemeldet bleiben“

Mit der Option **Angemeldet bleiben** können Benutzer angemeldet bleiben, wenn sie ihr Browserfenster schließen und erneut öffnen. Diese Option wirkt sich nicht auf die Lebensdauer der Sitzung aus. Wechseln Sie zu **Azure Active Directory** > **Unternehmensbranding** > **Unternehmensbranding bearbeiten**.

Einige Features von SharePoint Online und Office 2010 setzen voraus, dass dieses Kontrollkästchen von Benutzern aktiviert werden kann. Wenn Sie diese Option ausblenden, erhalten Benutzer möglicherweise zusätzliche und unerwartete Anmeldeaufforderungen.

### <a name="directory-name"></a>Verzeichnisname

Sie können das Attribut für den Verzeichnisnamen unter **Azure Active Directory** > **Eigenschaften** ändern. Sie können einen Anzeigenamen für Organisation anzeigen, der Im Portal und in automatisierter Kommunikation zu sehen ist. Diese Option wird insbesondere in automatisierten E-Mails in den folgenden Formen angezeigt:

* Der Anzeigename in einer E-Mail, beispielsweise „Microsoft im Namen von CONTOSO – Demo“
* Die Betreffzeile einer E-Mail, beispielsweise „E-Mail-Prüfnummer für CONTOSO-Demokonto“

## <a name="next-steps"></a>Nächste Schritte

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](active-directory-passwords-policy.md)
* [Übersicht über die Kennwortrückschreibung](active-directory-passwords-writeback.md)
* [Berichterstellungsoptionen für die Kennwortverwaltung von Azure AD](active-directory-passwords-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Beispiel-E-Mail zum Anfordern von Administratorunterstützung bei der Kennwortzurücksetzung"

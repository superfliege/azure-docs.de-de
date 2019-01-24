---
title: Webdienst der mobilen App für den Azure MFA-Server | Microsoft-Dokumentation
description: Konfigurieren Sie die MFA-Server zum Senden von Pushbenachrichtigungen an Benutzer mit der Microsoft Authenticator-App.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 26c227d96c3d951a5140b94a4597a4d76405fe63
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429207"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Aktivieren der Mobile App-Authentifizierung mit dem Azure Multi-Factor Authentication-Server

Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Überprüfungsoption. Anstelle eines automatischen Telefonanrufs oder einer SMS sendet Azure Multi-Factor Authentication bei der Benutzeranmeldung eine Pushbenachrichtigung an die Microsoft Authenticator-App auf dem Smartphone oder Tablet des Benutzers. Der Benutzer tippt in der App einfach auf **Überprüfen** (oder gibt eine PIN ein und tippt auf „Authentifizieren“), um sich anzumelden.

Die Verwendung einer mobilen App für die zweistufige Überprüfung empfiehlt sich, wenn kein zuverlässiges Mobilfunknetz zur Verfügung steht. Bei Verwendung als OAuth-Token-Generator benötigt die App keine Netzwerk- oder Internetverbindung.

> [!IMPORTANT]
> Wenn Sie Azure Multi-Factor Authentication-Server v8.x oder höher installiert haben, sind die meisten unten angegebenen Schritte nicht erforderlich. Die Vorgehensweise zum Einrichten der Authentifizierung mittels mobiler App wird unter [Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) beschrieben.

## <a name="requirements"></a>Requirements (Anforderungen)

Zum Verwenden der Microsoft Authenticator-App müssen Sie Azure Multi-Factor Authentication-Server v8.x oder höher ausführen.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server

1. Klicken Sie in der Konsole des Multi-Factor Authentication-Servers auf das Benutzerportal-Symbol. Wenn Benutzer ihre Authentifizierungsmethode selbst steuern dürfen, aktivieren Sie auf der Registerkarte „Einstellungen“ unter **Methodenauswahl durch Benutzer zulassen** die Option **Mobile App**. Wenn diese Funktion nicht aktiviert ist, müssen sich Endbenutzer an den Helpdesk wenden, um die Aktivierung für die mobile App abzuschließen.
2. Aktivieren Sie das Kontrollkästchen **Aktivierung der mobilen Anwendung durch Benutzer zulassen**.
3. Aktivieren Sie das Kontrollkästchen **Benutzerregistrierung zulassen**.
4. Klicken Sie auf das Symbol **Mobile App**.
5. Geben Sie im Feld **Kontoname** den Unternehmens- oder Organisationsnamen ein, der in der mobilen Anwendung für dieses Konto angezeigt werden soll.
   ![MFA-Server-Konfiguration – Einstellungen für die mobile App](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erweiterte Szenarien mit Azure Multi-Factor Authentication und Drittanbieter-VPNs](howto-mfaserver-nps-vpn.md)

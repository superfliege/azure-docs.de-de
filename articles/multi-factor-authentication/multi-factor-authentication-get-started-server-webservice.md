---
title: "Webdienst der mobilen App für den Azure MFA-Server | Microsoft-Dokumentation"
description: "Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Authentifizierungsoption.  Diese ermöglicht es, dass der MFA-Server Pushbenachrichtigungen an Benutzer sendet."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: bf758d1241f2a56eba4d5c92ace713d6e563df65
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Aktivieren der Mobile App-Authentifizierung mit dem Azure Multi-Factor Authentication-Server

Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Überprüfungsoption. Anstelle eines automatischen Telefonanrufs oder einer SMS sendet Azure Multi-Factor Authentication bei der Benutzeranmeldung eine Pushbenachrichtigung an die Microsoft Authenticator-App auf dem Smartphone oder Tablet des Benutzers. Der Benutzer tippt in der App einfach auf **Überprüfen** (oder gibt eine PIN ein und tippt auf „Authentifizieren“), um sich anzumelden.

Die Verwendung einer mobilen App für die Prüfung in zwei Schritten empfiehlt sich, wenn kein zuverlässiges Mobilfunknetz zur Verfügung steht. Bei Verwendung als OAuth-Token-Generator benötigt die App keine Netzwerk- oder Internetverbindung.

Abhängig von Ihrer Umgebung können Sie Verwaltungsrichtlinien für mobile Apps, um den Webdienst auf dem gleichen Server wie Azure Multi-Factor Authentication-Server oder einem anderen Internet verbundene Server bereitstellen möchten.

## <a name="requirements"></a>Anforderungen

Zur Verwendung der Microsoft Authenticator-App müssen folgende Voraussetzungen erfüllt sein, damit die App erfolgreich mit dem Webdienst der mobilen App kommunizieren kann:

* Azure Multi-Factor Authentication-Server ab Version 6.0
* Installieren Sie den Webdienst der mobilen App auf einem Webserver mit Internetverbindung, auf dem Microsoft® [Internetinformationsdienste (IIS) 7.x oder höher](http://www.iis.net/) ausgeführt wird.
* ASP.NET v4.0.30319 ist installiert, registriert und auf „Zulässig“ festgelegt.
* Erforderliche Rollendienste umfassen ASP.NET und IIS 6-Metabasiskompatibilität.
* Der Webdienst der mobilen App ist über eine öffentliche URL erreichbar.
* Der Webdienst der mobilen App ist durch ein SSL-Zertifikat geschützt.
* Installieren Sie das Azure Multi-Factor Authentication Webdienst-SDK in IIS 7.x oder höher auf dem **demselben Server wie der Azure Multi-Factor Authentication-Server**
* Das Azure Multi-Factor Authentication-Webdienst-SDK ist durch ein SSL-Zertifikat geschützt.
* Der Webdienst der mobilen App kann eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen.
* Der Webdienst der mobilen App kann sich beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos authentifizieren, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Dieses Dienstkonto und diese Gruppe sind in Active Directory vorhanden, wenn sich der Azure Multi-Factor Authentication-Server auf einem in die Domäne eingebundenen Server befindet. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.

## <a name="install-the-mobile-app-web-service"></a>Installieren des Webdiensts der mobilen App

Beachten Sie folgende Punkte, bevor Sie den Webdienst der mobilen App installieren:

* Sie benötigen ein Dienstkonto, die Teil von "PhoneFactor Admins" ist Gruppe. Dieses Konto kann identisch mit der für die Installation des Benutzerportals verwendet werden.
* Es ist hilfreich, auf dem mit dem Internet verbundenen Webserver einen Webbrowser zu öffnen und die URL des Webdienst-SDK aufzurufen, die in die Datei "Web.config" eingegeben wurde. Wenn der Webdienst erfolgreich im Browser aufgerufen werden kann, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort, die in die Datei "Web.config" eingegeben wurden, genau wie in der Datei angezeigt ein. Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.
* Wenn sich vor dem Webserver mit dem Webdienst der mobilen App ein Reverseproxy oder eine Firewall befindet und eine SSL-Abladung durchführt, können Sie die Datei „web.config“ des Webdiensts der mobilen App bearbeiten, damit der Webdienst der mobilen App HTTP anstelle von HTTPS verwenden kann. Für die Verbindung zwischen der mobilen Anwendung und der Firewall/dem Reverseproxy ist weiterhin SSL erforderlich. Fügen Sie dem Abschnitt \<appSettings\> den folgenden Schlüssel hinzu:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-web-service-sdk"></a>Installieren des Webdienst-SDK

In beiden Szenarien ist die Azure Multi-Factor Authentication Webdienst-SDK **nicht** bereits auf dem Azure Multi-Factor Authentication (MFA) Server installiert ist, führen Sie die Schritte, die folgen.

1. Öffnen Sie die Multi-Factor Authentication-Server-Verwaltungskonsole.
2. Wechseln Sie zu der **Webdienst-SDK** , und wählen Sie **Webdienst-SDK installieren**.
3. Führen Sie die Installation mit den Standardeinstellungen, es sei denn, Sie aus irgendeinem Grund geändert werden müssen.
4. Binden Sie ein SSL-Zertifikat an der Website in IIS.

Wenn Sie Fragen zum Konfigurieren eines SSL-Zertifikats auf einem IIS-Server haben, finden Sie im Artikel [zum Einrichten von SSL in IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Das Webdienst-SDK muss durch ein SSL-Zertifikat geschützt sein. Für diesen Zweck ist ein selbst signiertes Zertifikat ausreichend. Importieren Sie das Zertifikat in den Speicher "Vertrauenswürdige Stammzertifizierungsstellen" des lokalen Computerkontos auf dem Benutzerportal-Webserver, sodass sie das Zertifikat beim Initiieren der SSL-Verbindung vertraut.

![MFA-Server-Setup für Configuration Webdienst-SDK](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-the-service"></a>Installieren des Diensts

1. **Auf dem Server MFA**, navigieren Sie zu den Installationspfad.
2. Navigieren Sie zu dem Ordner, in denen Azure MFA-Server installiert ist, die Standardeinstellung ist **c:\Programme\Microsoft c:\programme\azure Multi-Factor Authentication**.
3. Suchen Sie die Installationsdatei **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Wenn der Server ist **nicht** Internetzugriff, kopieren Sie die Installationsdatei mit dem Internet verbundenen Server.
4. Wenn der MFA-Server ist **nicht** Internetzugriff zum Wechseln der **Internet verbundene Server**.
5. Führen Sie die **MultiFactorAuthenticationMobileAppWebServiceSetup64** installieren Sie die Datei als Administrator, falls gewünscht, ändern Sie den Standort aus, und ändern Sie das virtuelle Verzeichnis in einen kurzen Namen, wenn Sie möchten.
6. Nach Abschluss der Installation, navigieren Sie zu **C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (oder dem entsprechenden Verzeichnis basierend auf den Namen des virtuellen Verzeichnisses), und bearbeiten Sie die Datei "Web.config".

   * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** , und ändern Sie **Wert = ""** auf **Wert = "Domäne\Benutzer"** "Domäne\Benutzer" ist, in dem ein Dienstkonto, die Teil von "PhoneFactor Admins" ist Gruppe.
   * Suchen Sie den Schlüssel **"Das WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** , und ändern Sie **Wert = ""** auf **Wert = "Kennwort"** Kennwort ist das Kennwort für das Dienstkonto in der vorherigen Zeile eingegeben haben.
   * Suchen der **PfMobile App Web Service_pfwssdk_PfWsSdk** festlegen und ändern Sie den Wert von **Http://localhost:4898/PfWsSdk.asmx** auf die Webdienst-SDK-URL (Beispiel: https://mfa.contoso.com/ MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Speichern Sie die Datei "Web.config", und schließen Sie Editor.

   > [!NOTE]
   > Da für diese Verbindung SSL verwendet wird, müssen Sie das Webdienst-SDK von verweisen **vollständig qualifizierten Domänennamen (FQDN)** und **keine IP-Adresse**. Das SSL-Zertifikat würde ausgestellt wurden für den FQDN als auch die URL, mit den Namen des Zertifikats übereinstimmen muss.

7. Wenn die Website, der unter der Webdienst für Mobile Apps installiert wurde nicht durch ein öffentlich signiertes Zertifikat gebunden wurde, installieren Sie das Zertifikat auf dem Server, öffnen Sie IIS-Manager und binden Sie das Zertifikat an die Website.
8. Von einem beliebigen Computer einen Webbrowser öffnen, und navigieren Sie zu der URL, auf dem Webdienst für Mobile Apps installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server

Nachdem Sie den Webdienst der mobilen App installiert haben, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung mit dem Portal konfigurieren.

1. Klicken Sie in der Multi-Factor Authentication-Server-Konsole auf das Symbol "Benutzerportal". Wenn Benutzer ihre Authentifizierungsmethode selbst steuern dürfen, aktivieren Sie auf der Registerkarte „Einstellungen“ unter **Methodenauswahl durch Benutzer zulassen** die Option **Mobile App**. Ohne diese Funktion aktivieren müssen Endbenutzer wenden Sie sich an Ihrem Helpdesk, um die Aktivierung der mobilen Anwendung abzuschließen.
2. Aktivieren Sie das Kontrollkästchen **Aktivierung der mobilen Anwendung durch Benutzer zulassen**.
3. Aktivieren Sie das Kontrollkästchen **Benutzerregistrierung zulassen**.
4. Klicken Sie auf die **Mobile Anwendung** Symbol.
5. Geben Sie die URL, die mit dem virtuellen Verzeichnis, das erstellt wurde, bei der Installation von MultiFactorAuthenticationMobileAppWebServiceSetup64 verwendet wird (Beispiel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/) in das Feld **Mobile Anwendung Webdienst-URL:**.
6. Auffüllen der **Kontoname** Feld mit dem Unternehmen oder Ihre Organisation Namen in der mobilen Anwendung für dieses Konto angezeigt.
   ![MFA-Server-Konfigurationseinstellungen für Mobile Anwendung](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erweiterte Szenarien mit Azure Multi-Factor Authentication und Drittanbieter-VPNs](multi-factor-authentication-advanced-vpn-configurations.md)
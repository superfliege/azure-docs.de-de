---
title: "Benutzerportal für Azure MFA-Server | Microsoft-Dokumentation"
description: "Dies ist die Seite für Azure Multi-Factor Authentication, auf der die ersten Schritte mit Azure MFA und dem Benutzerportal beschrieben werden."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: a4eb403d3d21b7dbe63c2645b488a7bddb6d39fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="user-portal-for-the-azure-multi-factor-authentication-server"></a>Benutzerportal für Azure Multi-Factor Authentication-Server

Das Benutzerportal ist eine IIS-Website, die Benutzer in der Azure Multi-Factor Authentication (MFA) registrieren und ihre Konten pflegen können. Die Benutzer können ihre Telefonnummer oder PIN ändern oder die Überprüfung in zwei Schritten bei der nächsten Anmeldung umgehen.

Die Benutzer melden sich mit ihrem normalen Benutzernamen und Kennwort am Benutzerportal an und rufen dann entweder die Überprüfung in zwei Schritten auf oder beantworten Sicherheitsfragen, um die Authentifizierung abzuschließen. Wenn die Benutzerregistrierung zulässig ist, konfigurieren Benutzer ihre Telefonnummer und PIN, wenn sie sich zum ersten Mal beim Benutzerportal anmelden.

Benutzerportal-Administratoren kann eingerichtet und die Berechtigung zum Hinzufügen von neuer Benutzern und zum Aktualisieren vorhandener Benutzer gewährt werden.

Abhängig von Ihrer Umgebung können Sie das Benutzerportal auf dem gleichen Server wie Azure Multi-Factor Authentication-Server oder auf einem anderen Internet verbundene Server bereitstellen möchten.

![MFA-Benutzerportal](./media/multi-factor-authentication-get-started-portal/portal.png)

> [!NOTE]
> Das Benutzerportal ist nur mit Multi-Factor Authentication-Server verfügbar. Bei Verwendung von Multi-Factor Authentication in der Cloud finden Sie Ihren Benutzern die [Einrichtung Ihr Konto für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-first-time.md) oder [verwalten Sie Ihre Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md).

## <a name="install-the-web-service-sdk"></a>Installieren des Webdienst-SDK

In beiden Szenarien ist die Azure Multi-Factor Authentication Webdienst-SDK **nicht** bereits auf dem Azure Multi-Factor Authentication (MFA) Server installiert ist, führen Sie die Schritte, die folgen.

1. Öffnen Sie die Multi-Factor Authentication-Server-Verwaltungskonsole.
2. Wechseln Sie zu der **Webdienst-SDK** , und wählen Sie **Webdienst-SDK installieren**.
3. Führen Sie die Installation mit den Standardeinstellungen, es sei denn, Sie aus irgendeinem Grund geändert werden müssen.
4. Binden Sie ein SSL-Zertifikat an der Website in IIS.

Wenn Sie Fragen zum Konfigurieren eines SSL-Zertifikats auf einem IIS-Server haben, finden Sie im Artikel [zum Einrichten von SSL in IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Das Webdienst-SDK muss durch ein SSL-Zertifikat geschützt sein. Für diesen Zweck ist ein selbst signiertes Zertifikat ausreichend. Importieren Sie das Zertifikat in den Speicher "Vertrauenswürdige Stammzertifizierungsstellen" des lokalen Computerkontos auf dem Benutzerportal-Webserver, sodass sie das Zertifikat beim Initiieren der SSL-Verbindung vertraut.

![MFA-Server-Setup für Configuration Webdienst-SDK](./media/multi-factor-authentication-get-started-portal/sdk.png)

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Bereitstellen des Benutzerportals auf demselben Server wie für den Azure Multi-Factor Authentication-Server

Die folgenden erforderlichen Komponenten sind erforderlich, um das Benutzerportal installiert die **demselben Server** als Azure Multi-Factor Authentication-Server:

* IIS, einschließlich ASP.NET und IIS 6-Metadaten, die Basis-Kompatibilität (für IIS 7 oder höher)
* Ein Konto mit Administratorrechten für den Computer und die Domäne, falls zutreffend. Das Konto benötigt Berechtigungen zum Erstellen von Active Directory-Sicherheitsgruppen.
* Schützen Sie das Benutzerportal mit einem SSL-Zertifikat.
* Schützen Sie das Azure Multi-Factor Authentication-Webdienst-SDK mit einem SSL-Zertifikat.

Führen Sie die folgenden Schritte aus, um das Benutzerportal bereitzustellen:

1. Öffnen Sie die Azure Multi-Factor Authentication-Server-Verwaltungskonsole, klicken Sie auf die **Benutzerportal** Symbol im linken Menü klicken Sie dann auf **Benutzerportal installieren**.
2. Führen Sie die Installation mit den Standardeinstellungen, es sei denn, Sie aus irgendeinem Grund geändert werden müssen.
3. Binden Sie ein SSL-Zertifikat auf der Website in IIS

   > [!NOTE]
   > Dieses SSL-Zertifikat ist normalerweise ein öffentlich signiertes SSL-Zertifikat.

4. Von einem beliebigen Computer einen Webbrowser öffnen, und navigieren Sie zu der URL, in dem das Benutzerportal installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuth). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

![MFA-Server-Benutzerportals-installation](./media/multi-factor-authentication-get-started-portal/install.png)

Wenn Sie Fragen zum Konfigurieren eines SSL-Zertifikats auf einem IIS-Server haben, finden Sie im Artikel [zum Einrichten von SSL in IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="deploy-the-user-portal-on-a-separate-server"></a>Bereitstellen des Benutzerportals auf einem separaten Server

Wenn der Server, auf dem Azure Multi-Factor Authentication-Server ausgeführt wird, nicht über Internetzugriff verfügt, sollten Sie das Benutzerportal installieren, auf eine **separate, Internet verbundene Server**.

Falls Ihre Organisation die Microsoft Authenticator-App als eine der Überprüfungsmethoden nutzt und das Benutzerportal auf einem eigenen Server bereitgestellt werden soll, müssen die folgenden Voraussetzungen erfüllt sein:

* Verwenden Sie Version 6.0 des Azure Multi-Factor Authentication-Servers (oder höher).
* Installieren Sie das Benutzerportal auf einem Webserver mit Internetzugriff, auf dem Microsoft Internetinformationsdienste (IIS) 6.x oder höher ausgeführt wird.
* Stellen Sie bei Verwendung von IIS 6.x sicher, dass ASP.NET v2.0.50727 installiert, registriert und auf **Zulässig** festgelegt ist.
* Bei Verwendung von IIS 7.x oder höher, IIS, einschließlich der Standardauthentifizierung, ASP.NET und IIS 6-Metabasiskompatibilität.
* Schützen Sie das Benutzerportal mit einem SSL-Zertifikat.
* Schützen Sie das Azure Multi-Factor Authentication-Webdienst-SDK mit einem SSL-Zertifikat.
* Stellen Sie sicher, dass das Benutzerportal eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen kann.
* Stellen Sie sicher, dass das Benutzerportal beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos der Sicherheitsgruppe „PhoneFactor Admins“ authentifiziert werden kann. Dieses Dienstkonto und die Gruppe sollten in Active Directory vorhanden sein, wenn der Azure Multi-Factor Authentication-Server auf eine Domäne eingebundenen Server ausgeführt wird. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.

Wenn Sie das Benutzerportal auf einem anderen Server als dem Azure Multi-Factor Authentication-Server installieren möchten, sind folgende Schritte erforderlich:

1. **Auf dem Server MFA**, navigieren Sie zum Installationspfad (Beispiel: c:\Programme\Microsoft C:\programme\multi-Factor Authentication Server), und kopieren Sie die Datei **MultiFactorAuthenticationUserPortalSetup64** an einem Speicherort mit dem Internet verbundene Server zugegriffen werden kann, in dem Sie ihn installieren möchten.
2. **Auf dem Internet verbundenen Webserver**, führen Sie die Installationsdatei multifactorauthenticationuserportalsetup64 als Administrator, falls gewünscht, ändern Sie den Standort und das virtuelle Verzeichnis in einen kurzen Namen ändern, wenn Sie möchten.
3. Binden Sie ein SSL-Zertifikat an der Website in IIS.

   > [!NOTE]
   > Dieses SSL-Zertifikat ist normalerweise ein öffentlich signiertes SSL-Zertifikat.

4. Navigieren Sie zu **C:\inetpub\wwwroot\MultiFactorAuth**
5. Bearbeiten Sie die Datei "Web.config" im Editor

    * Suchen Sie den Schlüssel **"USE_WEB_SERVICE_SDK"** , und ändern Sie **Wert = "false"** zu **Wert = "true"**
    * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** , und ändern Sie **Wert = ""** auf **Wert = "Domäne\Benutzer"** "Domäne\Benutzer" ist, in dem ein Dienstkonto, die Teil von "PhoneFactor Admins" ist Gruppe.
    * Suchen Sie den Schlüssel **"Das WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** , und ändern Sie **Wert = ""** auf **Wert = "Kennwort"** Kennwort ist das Kennwort für das Dienstkonto in der vorherigen Zeile eingegeben haben.
    * Suchen Sie den Wert **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx** , und ändern Sie diese Platzhalter-URL in die Webdienst-SDK-URL wir in Schritt 2 installiert.
    * Speichern Sie die Datei "Web.config", und schließen Sie Editor.

6. Von einem beliebigen Computer einen Webbrowser öffnen, und navigieren Sie zu der URL, in dem das Benutzerportal installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuth). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

Wenn Sie Fragen zum Konfigurieren eines SSL-Zertifikats auf einem IIS-Server haben, finden Sie im Artikel [zum Einrichten von SSL in IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="configure-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren Sie benutzerportaleinstellungen im Azure Multi-Factor Authentication-Server

Nachdem das Benutzerportal installiert wurde, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung des Portals konfigurieren.

1. Klicken Sie in der Azure Multi-Factor Authentication-Server-Konsole auf die **Benutzerportal** Symbol. Geben Sie auf der Registerkarte „Einstellungen“ im Textfeld **Benutzerportal-URL** die URL des Benutzerportals ein. Sofern die E-Mail-Funktion aktiviert wurde, wird diese URL in E-Mails eingefügt, die an Benutzer gesendet werden, wenn diese in den Azure Multi-Factor Authentication-Server importiert werden.
2. Wählen Sie die Einstellungen aus, die Sie im Benutzerportal verwenden möchten. Wenn beispielsweise Benutzer ihre Authentifizierungsmethoden auswählen dürfen, sollten Sie sicherstellen, dass die Option **Methodenauswahl durch Benutzer zulassen** zusammen mit den zur Auswahl stehenden Methoden aktiviert ist.
3. Definieren, wer Administratoren sein sollte auf die **Administratoren** Registerkarte. Sie können eine präzise Administratorberechtigungen mithilfe der Kontrollkästchen und Dropdownlisten in den Feldern hinzufügen/bearbeiten erstellen.

Optionale Konfiguration:
- **Sicherheitsfragen** -definieren Sie genehmigt Sicherheitsfragen für Ihre Umgebung und die Sprache, die sie im angezeigt werden.
- **Weitergegebenen Sitzungen** -Portalintegration für Benutzer mit einer formularbasierten Website mithilfe von MFA konfigurieren.
- **"Vertrauenswürdige IPs"** -MFA bei der zu überspringenden authentifizieren aus einer Liste von IP-Adressen oder Adressbereiche vertrauenswürdigen Benutzern erlauben.

![Konfiguration der MFA Server-Benutzerportals](./media/multi-factor-authentication-get-started-portal/config.png)

Der Azure Multi-Factor Authentication-Server stellt mehrere Optionen für das Benutzerportal bereit. Die folgende Tabelle enthält eine Liste dieser Optionen sowie eine Erläuterung zu deren Verwendung.

| Benutzerportal – Einstellungen | Beschreibung |
|:--- |:--- |
| Benutzerportal-URL | Geben Sie die URL ein, unter der das Portal gehostet wird. |
| Primäre Authentifizierung | Geben Sie den Typ der Authentifizierung für die Anmeldung beim Portal an. Mögliche Optionen: Windows-, Radius- oder LDAP-Authentifizierung. |
| Benutzeranmeldung zulassen | Ermöglicht Benutzern die Eingabe von Benutzername und Kennwort auf der Anmeldeseite des Benutzerportals. Wenn diese Option nicht ausgewählt ist, sind die Felder abgeblendet. |
| Benutzerregistrierung zulassen | Ermöglicht dem Benutzer die Registrierung bei der Multi-Factor Authentication. Dazu wird der Benutzer auf einem Setupbildschirm zur Eingabe zusätzlicher Informationen wie beispielsweise einer Telefonnummer aufgefordert. Die Eingabeaufforderung für die Ersatzrufnummer ermöglicht dem Benutzer die Angabe einer sekundären Telefonnummer. Die Eingabeaufforderung für das Drittanbieter-OATH-Token ermöglicht dem Benutzer die Angabe eines entsprechenden Tokens. |
| Benutzern das Initiieren einer Einmalumgehung ermöglichen | Ermöglicht Benutzern das Initiieren einer Einmalumgehung. Wenn ein Benutzer diese Option richtet, wird es das nächste Mal wirksam, das der Benutzer anmeldet. Die Eingabeaufforderung für die Gültigkeit der Umgehung in Sekunden zeigt dem Benutzer ein Feld an, in dem er den Standardwert von 300 Sekunden ändern kann. Andernfalls gilt die Einmalumgehung nur für 300 Sekunden. |
| Benutzern die Auswahl der Methode ermöglichen | Ermöglicht Benutzern das Festlegen ihrer primären Kontaktmethode. Diese Methode kann Telefonanruf, SMS, mobile Anwendung oder oath-Token sein. |
| Benutzern die Auswahl der Sprache ermöglichen | Ermöglicht Benutzern das Ändern der Sprache, die für den Telefonanruf, die Textnachricht (SMS), die mobile App oder das OATH-Token verwendet wird. |
| Benutzern das Aktivieren der mobilen App ermöglichen | Ermöglicht Benutzern das Generieren eines Aktivierungscodes, um den Aktivierungsprozess der mit dem Server verwendeten mobilen App durchzuführen.  Sie können auch die Anzahl der Geräte festlegen (1 bis 10), auf denen Benutzer die App aktivieren können. |
| Sicherheitsfragen als Alternative verwenden | Ermöglicht die Verwendung von Sicherheitsfragen, falls die Überprüfung in zwei Schritten fehlschlägt. Sie können die Anzahl der Sicherheitsfragen festlegen, die richtig beantwortet werden müssen. |
| Benutzern das Zuordnen von Drittanbieter-OATH-Token ermöglichen | Ermöglicht Benutzern das Angeben eines Drittanbieter-OATH-Tokens. |
| OATH-Token als Alternative verwenden | Ermöglicht die Verwendung eines OATH-Tokens, falls die Überprüfung in zwei Schritten nicht erfolgreich ist. Sie können auch den Sitzungstimeout in Minuten angeben. |
| Aktivieren der Protokollierung | Ermöglicht die Protokollierung im Benutzerportal. Die Protokolldateien befinden sich unter: C:\Programme\Multi-Factor Authentication Server\Logs. |

Diese Einstellungen werden für den Benutzer im Portal sichtbar, nachdem sie aktiviert sind und sie die Anmeldung beim Benutzerportal angemeldet sind.

![Benutzerportal – Einstellungen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Self-Service-Registrierung

Wenn Sie möchten, dass Ihre Benutzer sich anmelden und registrieren, müssen Sie auf der Registerkarte „Einstellungen“ die Optionen **Benutzeranmeldung zulassen** und **Benutzerregistrierung zulassen** auswählen. Denken Sie daran, dass die ausgewählten Einstellungen die Benutzeranmeldung beeinflussen.

Wenn sich ein Benutzer beispielsweise zum ersten Mal beim Benutzerportal anmeldet, wird er auf die Seite zur Benutzereinrichtung von Azure Multi-Factor Authentication geleitet. Je nachdem, wie Sie Azure Multi-Factor Authentication konfiguriert haben, kann der Benutzer seine Authentifizierungsmethode auswählen.

Wenn sie die Überprüfungsmethode Telefonanruf wählen oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite den Benutzer zur Eingabe ihrer primäre Telefonnummer mit Durchwahl, falls zutreffend. Möglicherweise kann er auch eine Ersatzrufnummer eingeben.

![Registrieren Sie primärer und sekundärer Telefonnummern](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Wenn der Benutzer eine PIN verwenden, bei der Authentifizierung erforderlich ist, fordert die Seite, um eine PIN zu erstellen. Nach der Eingabe der Telefonnummer(n) und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt für Authentifizierung angerufen werden**. Azure Multi-Factor Authentication führt eine Überprüfung Telefonanruf primäre Telefonnummer des Benutzers. Der Benutzer muss den Anruf annehmen, ggf. seine PIN eingeben und # drücken, um zum nächsten Schritt der Selbstregistrierung zu gelangen.

Wenn der Benutzer die Methode zur nachrichtensignaturüberprüfung Textnachricht wählt oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite die Benutzer ihre Mobiltelefonnummer. Wenn der Benutzer eine PIN verwenden, bei der Authentifizierung erforderlich ist, wird der Seite auch diese Eingabe eine PIN aufgefordert.  Nach der Eingabe der Telefonnummer und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt per SMS authentifizieren**. Azure Multi-Factor Authentication führt eine Überprüfung SMS an Mobiltelefon des Benutzers. Der Benutzer empfängt die SMS mit einer Einmalkennung (One-Time Passcode, OTP) und sendet dann eine Antwort auf die Nachricht mit der Einmalkennung und seiner PIN (falls zutreffend).

![Benutzerportal-SMS](./media/multi-factor-authentication-get-started-portal/text.png)

Wenn der Benutzer die Mobile Anwendung Überprüfungsmethode auswählt, fragt die Seite Microsoft Authenticator-app auf ihrem Gerät installieren und einen Aktivierungscode zu generieren. Nach der Installation der App klickt der Benutzer auf die Schaltfläche „Aktivierungscode generieren“.

> [!NOTE]
> Zum Verwenden der Microsoft Authenticator-App muss der Benutzer Pushbenachrichtigungen für sein Gerät aktivieren.

Die Seite zeigt dann einen Aktivierungscode und eine URL zusammen mit einem Barcodebild an. Wenn der Benutzer eine PIN verwenden, bei der Authentifizierung erforderlich ist, werden die Seite darüber diese Eingabe eine PIN aufgefordert. Der Benutzer gibt den Aktivierungscode und die URL in die Microsoft Authenticator-App ein oder verwendet den Barcodescanner, um das Barcodebild zu scannen, und klickt auf die Schaltfläche „Aktivieren“.

Nach Abschluss der Aktivierung klickt der Benutzer auf die Schaltfläche **Jetzt authentifizieren**. Azure Multi-Factor Authentication führt eine Überprüfung auf mobile app des Benutzers. Der Benutzer muss seine PIN (falls zutreffend) eingeben die Schaltfläche „Authentifizieren“ in seiner mobilen App drücken, um mit dem nächsten Schritt der Selbstregistrierung fortzufahren.

Wenn die Administratoren den Azure Multi-Factor Authentication-Server für das Erfassen von Sicherheitsfragen und -antworten konfiguriert haben, wird der Benutzer auf die Seite mit den Sicherheitsfragen weitergeleitet. Der Benutzer muss vier Sicherheitsfragen auswählen und die entsprechenden Antworten bereitstellen.

![Benutzerportal-Sicherheitsfragen](./media/multi-factor-authentication-get-started-portal/secq.png)

Die Selbstregistrierung des Benutzers ist jetzt abgeschlossen, und der Benutzer wird am Benutzerportal angemeldet. Benutzer können erneut anmelden beim Benutzerportal zu einem beliebigen Zeitpunkt in der Zukunft ihre Telefonnummern, PINs, Authentifizierungsmethoden und Sicherheitsfragen ändern, wenn die Änderung ihrer Methoden von ihren Adminstratoren zugelassen wird.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen Sie den Azure Multi-Factor Authentication-Serverdienst Webdienst für Mobile Anwendungen](multi-factor-authentication-get-started-server-webservice.md)
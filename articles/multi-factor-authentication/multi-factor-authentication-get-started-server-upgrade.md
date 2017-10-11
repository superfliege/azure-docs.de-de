---
title: Upgrade von PhoneFactor auf Azure MFA-Server | Microsoft Docs
description: "Erste Schritte mit Azure MFA-Server bei einem upgrade von Phonefactor Agent ältere."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.openlocfilehash: 7ab7e693909f807781744ae53eed75d425096590
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Upgrade von PhoneFactor Agent auf Azure Multi-Factor Authentication-Server
Upgrade von PhoneFactor Agent v5.x oder älter auf Azure Multi-Factor Authentication-Server, den PhoneFactor-Agent deinstallieren und Komponenten zuerst zugewiesen. Anschließend können die Multi-Factor Authentication-Server und die zugehörigen Komponenten installiert werden.

## <a name="uninstall-the-phonefactor-agent"></a>Deinstallieren Sie den PhoneFactor-Agent

1. Sichern Sie zuerst die PhoneFactor-Datendatei aus. Der Standardinstallationspfad lautet c:\Programme\Microsoft Files\PhoneFactor\Data\Phonefactor.pfdata.

2. Wenn das Benutzerportal installiert ist:
  1. Navigieren Sie zum Installationsordner, und Sichern Sie die Datei "Web.config". Der Standardinstallationspfad lautet C:\inetpub\wwwroot\PhoneFactor.

  2. Wenn Sie das Portal benutzerdefinierte Designs hinzugefügt haben, Sichern Sie Ihren benutzerdefinierten Ordner unter dem Verzeichnis C:\inetpub\wwwroot\PhoneFactor\App_Themes.

  3. Deinstallieren Sie das Benutzerportal entweder über den PhoneFactor-Agent (nur verfügbar, wenn auf dem gleichen Server wie der PhoneFactor-Agent installiert) oder über Windows-Programme und Funktionen.

3. Wenn der Webdienst für Mobile Apps installiert ist:

  1. Wechseln Sie zum Installationsordner, und Sichern Sie die Datei "Web.config". Der Standardinstallationspfad lautet C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

  2. Deinstallieren Sie den Webdienst für Mobile Anwendungen Service über Windows-Programme und Funktionen.

4. Wenn das Webdienst-SDK installiert ist, deinstallieren Sie es entweder über den PhoneFactor-Agent oder über Windows-Programme und Funktionen.

5. Deinstallieren Sie den PhoneFactor Agent über die Windows-Programme und Funktionen.

## <a name="install-the-multi-factor-authentication-server"></a>Installieren Sie den Multi-Factor Authentication-Server

Der Installationspfad ist übernommen aus der Registrierung aus der vorherigen PhoneFactor-Agent-Installation, sodass am gleichen Speicherort (z. B. c:\Programme\Microsoft c:\programme\phonefactor) installiert werden soll. Neuinstallationen haben einen anderen standardmäßigen Installationspfad (z. B. c:\Programme\Microsoft C:\programme\multi-Factor Authentication Server). Vom vorherigen PhoneFactor Agent hinterlassene Datendatei sollte während der Installation aktualisiert werden, damit Ihre Benutzer und Einstellungen noch vorhanden sein sollten, nach der Installation der neuen Multi-Factor Authentication-Servers.

1. Wenn Sie aufgefordert werden, aktivieren Sie den Multi-Factor Authentication-Server, und stellen Sie sicher, dass er der richtigen Replikationsgruppe zugewiesen ist.

2. Wenn das Webdienst-SDK zuvor installiert war, installieren Sie das neue Webdienst-SDK über die Benutzeroberfläche des Multi-Factor Authentication-Server.

  Der Standardname für das virtuelle Verzeichnis lautet jetzt **"multifactorauthwebservicesdk"** anstelle von **PhoneFactorWebServiceSdk**. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation den neuen Standardnamen verwenden lassen, müssen Sie hingegen die URL in einer Anwendung zu ändern, die das Webdienst-SDK (z. B. das Benutzerportal und der Webdienst für Mobile App) verweisen, dass Sie am richtigen Speicherort verweist.

3. Wenn das Benutzerportal zuvor auf dem PhoneFactor Agent-Server installiert war, installieren Sie das neue Multi-Factor Authentication-Benutzerportal über die Benutzeroberfläche des Multi-Factor Authentication-Server.

  Der Standardname für das virtuelle Verzeichnis lautet jetzt **"multifactorauth"** anstelle von **PhoneFactor**. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation den neuen Standardnamen verwenden lassen, sollten Sie andernfalls klicken Sie auf das benutzerportalsymbol im Multi-Factor Authentication-Server und aktualisieren die URL des Benutzerportals auf der Registerkarte "Einstellungen".

4. Wenn das Benutzerportal und/oder der Webdienst für Mobile Apps zuvor auf einem anderen Server als der PhoneFactor-Agent installiert war:

  1. Wechseln Sie zum Installationsspeicherort (z. B. c:\Programme\Microsoft c:\programme\phonefactor), und kopieren Sie eine oder mehrere Installationsprogramme auf den anderen Server. Es gibt 32-Bit und 64-Bit-Installationsprogramme für das Benutzerportal und der Webdienst für Mobile Apps. Sie heißen MultiFactorAuthenticationUserPortalSetupXX.msi und MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

  2. Klicken Sie zum Installieren des Benutzerportals auf dem Webserver öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie MultiFactorAuthenticationUserPortalSetupXX.msi.

    Der Standardname für das virtuelle Verzeichnis lautet jetzt **"multifactorauth"** anstelle von **PhoneFactor**. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Wenn Sie die Installation den neuen Standardnamen verwenden lassen, sollten Sie andernfalls klicken Sie auf das benutzerportalsymbol im Multi-Factor Authentication-Server und aktualisieren die URL des Benutzerportals auf der Registerkarte "Einstellungen". Vorhandene Benutzer müssen über die neue URL informiert werden.

  3. Gehen Sie beim Benutzerportal Installationspfad (z. B. C:\inetpub\wwwroot\MultiFactorAuth), und bearbeiten Sie die Datei "Web.config". Kopieren Sie die Werte in den Abschnitten "appSettings" und "ApplicationSettings" aus der ursprünglichen Datei "Web.config", die in die neue "Web.config"-Datei vor dem Upgrade gesichert wurde. Wenn der neue Standardname virtuellen Verzeichnisses bei der Installation des Webdienst-SDK beibehalten wurde, ändern Sie die URL im Abschnitt ApplicationSettings so, dass auf den richtigen Speicherort verweist. Wenn in der vorherigen Datei web.config noch andere Standardwerte geändert wurden, gelten Sie die gleichen Änderungen auf die neue Datei "Web.config".

  4. Klicken Sie zum Installieren des Webdiensts für Mobile Apps auf dem Webserver öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie die multifactorauthenticationmobileappwebservicesetupxx.msi aus.

    Der Standardname für das virtuelle Verzeichnis lautet jetzt **MultiFactorAuthMobileAppWebService** anstelle von **PhoneFactorPhoneAppWebService**. Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Möglicherweise möchten einen kürzeren Namen zu erleichtern für Endbenutzer auf ihren mobilen Geräten einzugeben auswählen. Wenn Sie die Installation den neuen Standardnamen verwenden lassen, sollten Sie andernfalls klicken Sie auf das Symbol Mobile Anwendung im Multi-Factor Authentication-Server und aktualisieren die Mobile App Webdienst-URL.

  5. Wechseln Sie an den Webdienst für Mobile App Installationspfad (z. B. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService), und bearbeiten Sie die Datei "Web.config". Kopieren Sie die Werte in den Abschnitten "appSettings" und "ApplicationSettings" aus der ursprünglichen Datei "Web.config", die in die neue "Web.config"-Datei vor dem Upgrade gesichert wurde. Wenn der neue Standardname virtuellen Verzeichnisses bei der Installation des Webdienst-SDK beibehalten wurde, ändern Sie die URL im Abschnitt ApplicationSettings so, dass auf den richtigen Speicherort verweist. Wenn in der vorherigen Datei web.config noch andere Standardwerte geändert wurden, gelten Sie die gleichen Änderungen auf die neue Datei "Web.config".

## <a name="next-steps"></a>Nächste Schritte

- [Installieren des benutzerportals](multi-factor-authentication-get-started-portal.md) für Azure Multi-Factor Authentication-Server.

- [Konfigurieren der Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md) für Ihre Anwendungen. 

---
title: "Azure AD v2 JS SPA geführtes Setup - konfigurieren | Microsoft Docs"
description: "Informationen, wie JavaScript SPA-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: adff529bfdc40006666cc643d49a302d7f1d09ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
## <a name="register-your-application"></a>Anwendung registrieren

Es gibt mehrere Möglichkeiten, eine Anwendung erstellen, wählen Sie eines von ihnen:

### <a name="option-1-register-your-application-express-mode"></a>Option 1: Registrieren Sie Ihrer Anwendung (Express-Modus)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:

1.  Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option *Geführtes Setup* aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-register-your-application-advanced-mode"></a>Option 2: Registrieren Sie Ihrer Anwendung (Erweiterter Modus)

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option *Geführtes Setup* deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
5. Hinzufügen der `Redirect URL` , die die URL der Anwendung basierend auf dem Webserver entsprechen. Zum Festlegen / Abrufen der umleitungs-URL in Visual Studio und Python finden Sie unter den folgenden Abschnitten zu Anweisungen.
6. Klicken Sie unten auf der Seite auf *Speichern*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-Anweisungen zum Abrufen der Umleitungs-URL
> Befolgen Sie die Anweisungen zum Abrufen Ihrer umleitungs-URL ein:
> 1.    Wählen Sie im *Projektmappen-Explorer* das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie `F4`).
> 2.    Kopieren Sie den Wert von `URL` in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Wechseln Sie zurück zu den *App-Registrierungsportal* und fügen Sie den Wert als eine `Redirect URL` , und klicken Sie auf "Speichern"

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Festlegen der Umleitungs-URL für Python
> Für Python können Sie den Webserverport über die Befehlszeile festlegen. Diese ausführliche Anleitung verwendet den Port 8080. Sie können aber auch beliebige andere verfügbare Ports verwenden. Befolgen Sie in jedem Fall die nachstehenden Anweisungen zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> - Wechseln Sie zurück zu der *App-Registrierungsportal* und legen Sie `http://localhost:8080/` als eine `Redirect URL`, oder verwenden Sie `http://localhost:[port]/` bei Verwendung ein benutzerdefiniertes TCP-Ports (, in denen *[Port]* die benutzerdefinierte TCP-Portnummer ist), und klicken Sie auf "Speichern"


#### <a name="configure-your-javascript-spa"></a>Konfigurieren des JavaScript-SPA

1.  Erstellen Sie eine Datei namens `msalconfig.js` mit den Informationen für die Anwendungsregistrierung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`. Vergeben Sie den Namen `msalconfig.js`.
2.  Fügen Sie Ihrer Datei `msalconfig.js` den folgenden Code hinzu:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Ersetzen Sie <code>Enter_the_Application_Id_here</code> durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
</ol>

---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 038ea48bedeb31416627f99b38ebb083846747e4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842970"
---
## <a name="register-your-application"></a>Anwendung registrieren

Es gibt mehrere Möglichkeiten, eine Anwendung zu erstellen – bitte wählen Sie eine aus:

### <a name="option-1-register-your-application-express-mode"></a>Option 1: Registrieren Sie Ihre Anwendung (Expressmodus)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:

1.  Registrieren Sie Ihre Anwendung über das [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für **Angeleitetes Setup** aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-register-your-application-advanced-mode"></a>Option 2: Registrieren Sie Ihre Anwendung (Erweiterter Modus)

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3. Stellen Sie sicher, dass die Option für **Angeleitetes Setup** aktiviert ist.
4.  Klicken Sie auf **Plattform hinzufügen**, und wählen Sie dann die Option **Web**.
5. Fügen Sie die **Umleitungs-URL** hinzu, die der URL der Anwendung gemäß Ihrem Webserver entspricht. Informationen über Anweisungen zum Festlegen und Abrufen der Umleitungs-URL in Visual Studio und Node finden Sie in den folgenden Abschnitten.
6. Wählen Sie **Speichern**aus.

> #### <a name="setting-redirect-url-for-node"></a>Festlegen der Umleitungs-URL für Node
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird Port 30662 als Referenz verwendet. Sie können aber auch beliebige andere verfügbare Ports nutzen. Befolgen Sie in jedem Fall die nachstehenden Anweisungen zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> - Wechseln Sie zurück zum *Portal für die Anwendungsregistrierung*, und legen Sie `http://localhost:30662/` als `Redirect URL` fest, oder verwenden Sie `http://localhost:[port]/`, wenn Sie einen benutzerdefinierten TCP-Port einsetzen (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist), und klicken Sie auf „Speichern“.

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-Anleitung zum Abrufen der Umleitungs-URL
> Führen Sie diese Schritte aus, um die Umleitungs-URL zu erhalten:
> 1.    Wählen Sie im **Projektmappen-Explorer** das Projekt aus, und sehen Sie sich das **Eigenschaftenfenster** an. Wenn kein **Eigenschaftenfenster** angezeigt wird, können Sie **F4** drücken.
> 2.    Kopieren Sie den Wert aus **URL** in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Wechseln Sie zurück zum *Portal für die Anwendungsregistrierung*, und fügen Sie den Wert als **Umleitungs-URL** ein. Wählen Sie anschließend **Speichern**.


#### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

1.  Fügen Sie in der Datei `index.html`, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie in den Tags `<script></script>` oben im Text der Datei `index.html` den folgenden Code hinzu:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
<ol start="3">
<li>
Ersetzen Sie <code>Enter the application Id here</code> durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
</ol>

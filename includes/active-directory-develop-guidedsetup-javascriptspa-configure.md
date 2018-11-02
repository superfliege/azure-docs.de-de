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
ms.openlocfilehash: b3d46e10facdef26b36c910a5c23b40a415a2894
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988407"
---
## <a name="register-your-application"></a>Anwendung registrieren

Sie haben mehrere Möglichkeiten, eine Anwendung zu registrieren. Wählen Sie die Option aus, die Ihren Anforderungen am besten entspricht:
* [Expressmodus: Konfigurieren der App mithilfe des SPA-Schnellstarts](#option-1-register-your-application-express-mode)
* [Erweiterter Modus: Manuelles Konfigurieren der App-Einstellungen](#option-2-register-your-application-advanced-mode)

### <a name="option-1-register-your-application-express-mode"></a>Option 1: Registrieren Sie Ihre Anwendung (Expressmodus)

1. Melden Sie sich beim [Azure-Portal – App-Registrierung (Vorschau)](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) an, um eine Anwendung zu registrieren.
1. Geben Sie auf der Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
1. Wenn Sie so weit sind, klicken Sie auf **Registrieren**.
1. Befolgen Sie die Schnellstartanweisungen, um Ihre neue Anwendung mit einem Klick herunterzuladen und automatisch für Sie zu konfigurieren.

### <a name="option-2-register-your-application-advanced-mode"></a>Option 2: Registrieren Sie Ihre Anwendung (Erweiterter Modus)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um eine Anwendung zu registrieren.
1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus, und klicken Sie anschließend auf **App-Registrierungen (Vorschau) > Neue Registrierung**.
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
1. Wählen Sie im Abschnitt **Umleitungs-URI** die Plattform **Web** aus, und setzen Sie den Wert auf die URL der Anwendung basierend auf Ihrem Webserver. Informationen über Anweisungen zum Festlegen und Abrufen der Umleitungs-URL in Visual Studio und Node finden Sie in den folgenden Abschnitten.
1. Wenn Sie so weit sind, klicken Sie auf **Registrieren**.
1. Notieren Sie sich den Wert **Anwendungs-ID (Client)** auf der App-Seite **Übersicht**.
1. Für diesen Schnellstart muss der [Flow zur impliziten Genehmigung](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) aktiviert werden. Wählen Sie im linken Navigationsbereich der registrierten Anwendung **Authentifizierung** aus.
1. Aktivieren Sie unter **Erweiterte Einstellungen** unter **Implizite Genehmigung** die Kontrollkästchen **ID-Token** und **Zugriffstoken**. ID-Token und Zugriffstoken sind erforderlich, da diese App Benutzer anmelden und eine API aufrufen muss.
1. Wählen Sie **Speichern**aus.

> #### <a name="setting-the-redirect-url-for-node"></a>Festlegen der Umleitungs-URL für Node
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird der Port 30662 als Referenz genutzt, aber Sie können auch einen beliebigen anderen Port verwenden, der verfügbar ist. Befolgen Sie die unten angegebene Anleitung zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> - Wechseln Sie zurück zu *App-Registrierung*, und legen Sie `http://localhost:30662/` als `Redirect URL` fest, oder verwenden Sie `http://localhost:[port]/`, wenn Sie einen benutzerdefinierten TCP-Port einsetzen (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist).

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-Anleitung zum Abrufen der Umleitungs-URL
> Führen Sie diese Schritte aus, um die Umleitungs-URL zu erhalten:
> 1. Wählen Sie im **Projektmappen-Explorer** das Projekt aus, und sehen Sie sich das **Eigenschaftenfenster** an. Wenn kein **Eigenschaftenfenster** angezeigt wird, können Sie **F4** drücken.
> 2. Kopieren Sie den Wert aus **URL** in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Wechseln Sie zurück zu *App-Registrierung*, und legen Sie den Wert als **Umleitungs-URL** fest.

#### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

1. Fügen Sie in der Datei `index.html`, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie in den Tags `<script></script>` oben im Text der Datei `index.html` den folgenden Code hinzu:

    ```javascript
    var applicationConfig = {
        clientID: "[Enter the application Id here]",
        graphScopes: ["user.read"],
        graphEndpoint: "https://graph.microsoft.com/v1.0/me"
    };
    ```

<ol start="2">
<li>
Ersetzen Sie <code>Enter the application Id here</code> durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
</ol>

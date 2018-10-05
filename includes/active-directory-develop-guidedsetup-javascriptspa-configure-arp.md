---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: eead4c6a66a317c7404205415cbf04c442ffe8d1
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060611"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen von Registrierungsinformationen für die Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Umleitungs-URL der Registrierungsinformationen für die Anwendung konfigurieren und dann die Anwendungs-ID zu Ihrer JavaScript-Einzelseitenanwendung hinzufügen.

### <a name="configure-redirect-url"></a>Konfigurieren der Umleitungs-URL

Konfigurieren Sie das Feld `Redirect URL` mit der URL für Ihre Seite „index.html“ basierend auf Ihrem Webserver, und klicken Sie dann auf *Aktualisieren*.


> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-Anleitung zum Abrufen der Umleitungs-URL
> Führen Sie diese Schritte aus, um die Umleitungs-URL zu erhalten:
> 1.    Wählen Sie im **Projektmappen-Explorer** das Projekt aus, und sehen Sie sich das **Eigenschaftenfenster** an. Wenn kein **Eigenschaftenfenster** angezeigt wird, können Sie **F4** drücken.
> 2.    Kopieren Sie den Wert aus **URL** in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Fügen Sie den Wert oben auf dieser Seite als **Umleitungs-URL** ein, und klicken Sie dann auf **Aktualisieren**.

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Festlegen der Umleitungs-URL für Node
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird der Port 30662 als Referenz genutzt, aber Sie können auch einen beliebigen anderen Port verwenden, der verfügbar ist. Befolgen Sie die unten angegebene Anleitung zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> Legen Sie `http://localhost:30662/` oben auf dieser Seite als **Umleitungs-URL** fest, oder verwenden Sie `http://localhost:[port]/` bei einem benutzerdefinierten TCP-Port (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist). Klicken Sie anschließend auf **Aktualisieren**.

### <a name="configure-your-javascript-spa-application"></a>Konfigurieren Ihrer JavaScript-Einzelseitenanwendung

1.  Fügen Sie in der Datei `index.html`, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie in den Tags `<script></script>` oben im Text der Datei `index.html` den folgenden Code hinzu:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```

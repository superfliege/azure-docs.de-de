---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: da2477b19327273fe922ac81f909233cb4ef8f06
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59503274"
---
## <a name="setting-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

> Möchten Sie stattdessen das Projekt dieses Beispiels herunterladen?
> - [Laden Sie die Projektdateien für einen lokalen Webserver herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip), z.B. Node
>
> oder
> - [Laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> Fahren Sie dann mit dem Schritt [Konfiguration](#register-your-application) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Tutorials ist ein lokaler Webserver erforderlich, z.B. [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) oder eine IIS Express-Integration mit [Visual Studio 2017](https://www.visualstudio.com/downloads/).

Installieren Sie bei Verwendung von Node.js zum Ausführen des Projekts eine IDE, z. B. [Visual Studio Code](https://code.visualstudio.com/download), um die Projektdateien zu bearbeiten.

Die Anweisungen in diesem Leitfaden basieren auf Node.js und Visual Studio 2017. Sie können aber auch eine andere Entwicklungsumgebung oder einen anderen Webserver verwenden.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

> ### <a name="option-1-node-other-web-servers"></a>Option 1: Node/Andere Webserver
> Vergewissern Sie sich, dass Sie [Node.js](https://nodejs.org/en/download/) installiert haben, und führen Sie dann den folgenden Schritt aus:
> - Erstellen Sie einen Ordner zum Hosten Ihrer Anwendung.

<p><!-- -->

> ### <a name="option-2-visual-studio"></a>Option 2: Visual Studio
> Wenn Sie Visual Studio verwenden und ein neues Projekt erstellen, führen Sie die folgenden Schritte aus, um eine neue Visual Studio-Projektmappe zu erstellen:
> 1.    In Visual Studio:  **„Datei“ > „Neu“ > „Projekt“**
> 2.    Wählen Sie unter **Visual C#\Web** die Option **ASP.NET-Webanwendung (.NET Framework)**.
> 3.    Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **OK**.
> 4.    Wählen Sie unter **Neue ASP.NET-Webanwendung** die Option **Leer**.

## <a name="create-your-single-page-applications-ui"></a>Erstellen der Benutzeroberfläche für die Einzelseitenanwendung
1. Erstellen Sie die Datei `index.html` für Ihre JavaScript-SPA. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: **„Hinzufügen“ > „Neues Element“ > „HTML-Seite“**, und nennen Sie es „index.html“.

2. Fügen Sie Ihrer Seite den folgenden Code hinzu:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.4/js/msal.js"></script>
       <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

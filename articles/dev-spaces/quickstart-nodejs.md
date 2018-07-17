---
title: Erstellen eines Kubernetes-Entwicklungsbereichs in der Cloud | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: quickstart
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: d0cb1c113724af5d07abf75e6d3a45b54e5202dc
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950769"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-nodejs"></a>Schnellstart: Erstellen eines Kubernetes-Entwicklungsbereichs mit Azure Dev Spaces (Node.js)

In diesem Leitfaden lernen Sie Folgendes:

- Einrichten von Azure Dev Spaces mit einem verwalteten Kubernetes-Cluster in Azure
- Iteratives Entwickeln von Code in Containern mit VS Code und der Befehlszeile
- Debuggen von Code, der in Ihrem Cluster ausgeführt wird

> [!Note]
> **Sollten Sie einmal nicht weiterkommen**, lesen Sie den Abschnitt [Problembehandlung](troubleshooting.md), oder hinterlassen Sie einen Kommentar auf dieser Seite. Ein ausführlicheres Tutorial finden Sie [hier](get-started-nodejs.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Ein [Kubernetes-Cluster](https://ms.portal.azure.com/#create/microsoft.aks) mit Kubernetes 1.10.3 in der Region „USA, Osten“, „USA, Mitte“, „USA, Westen 2“, „Europa, Westen“, „Kanada, Mitte“ oder „Kanada, Osten“ mit aktiviertem **HTTP-Anwendungsrouting**.

  ![Achten Sie darauf, dass HTTP-Anwendungsrouting aktiviert ist.](media/common/Kubernetes-Create-Cluster-3.PNG)

- Visual Studio Code (kann [hier](https://code.visualstudio.com/download) heruntergeladen werden).

## <a name="set-up-azure-dev-spaces"></a>Einrichten von Azure Dev Spaces

1. Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) (mindestens Version 2.0.38).
1. Richten Sie Dev Spaces in Ihrem AKS-Cluster ein: `az aks use-dev-spaces -g MyResourceGroup -n MyAKS`
1. Laden Sie die [Azure Dev Spaces-Erweiterung](https://aka.ms/get-azds-code) für VS Code herunter. Klicken Sie auf „Installieren“, sobald Sie sich auf der Marketplace-Seite der Erweiterung befinden. Wiederholen Sie diesen Vorgang in VS Code.

## <a name="build-and-run-code-in-kubernetes"></a>Erstellen und Ausführen von Code in Kubernetes

1. Laden Sie den Beispielcode von GitHub herunter: [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) 
1. Wechseln zum Ordner „webfrontend“: `cd dev-spaces/samples/nodejs/getting-started/webfrontend`
1. Generieren Sie Diagrammressourcen für Docker und Helm: `azds prep --public`
1. Erstellen Sie Ihren Code in AKS, und führen Sie ihn aus. Führen Sie im Terminalfenster unter dem **webfrontend-Ordner** den folgenden Befehl aus: `azds up`.
1. Suchen Sie in der Konsolenausgabe nach Informationen zu der URL, die durch den Befehl `up` erstellt wurde. Sie weist folgendes Format auf: 

   `Service 'webfrontend' port 'http' is available at <url>` 

   Öffnen Sie diese URL in einem Browserfenster. Dort sollten Sie sehen, dass die Web-App geladen wird. Wenn der Container ausgeführt wird, wird die Ausgabe von `stdout` und `stderr` an das Terminalfenster gestreamt.
   
   > [!Note]
   > Beim ersten Ausführen kann es mehrere Minuten dauern, bis das öffentliche DNS bereit ist. Wenn die öffentliche URL nicht aufgelöst wird, können Sie die alternative URL http://localhost:<portnumber> verwenden, die in der Konsolenausgabe angezeigt wird. Bei Verwendung der localhost-URL kann es so aussehen, als ob der Container lokal ausgeführt wird, während er stattdessen unter AKS ausgeführt wird. Um die Interaktion mit dem Dienst auf Ihrem lokalen Computer zu erleichtern, erstellt Azure Dev Spaces einen temporären SSH-Tunnel zu dem in Azure ausgeführten Container. Sie können später zurückkehren und versuchen, die öffentliche URL zu verwenden, wenn der DNS-Eintrag bereit ist.

### <a name="update-a-content-file"></a>Aktualisieren einer Inhaltsdatei
Bei Azure Dev Spaces geht es nicht nur um die Ausführung von Code in Kubernetes: Mit diesem Dienst sollen Codeänderungen in einer Kubernetes-Umgebung in der Cloud schnell und iterativ sichtbar gemacht werden.

1. Navigieren Sie zur Datei `./public/index.html`, und ändern Sie die HTML. Legen Sie beispielsweise als Hintergrundfarbe der Seite einen Blauton fest:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

1. Speichern Sie die Datei . Kurz darauf wird im Terminalfenster eine Nachricht mit dem Hinweis angezeigt, dass eine Datei im ausgeführten Container aktualisiert wurde.
1. Aktualisieren Sie die Anzeige im Browser. Daraufhin sollte die Farbaktualisierung angezeigt werden.

Was ist passiert? Für Änderungen an Inhaltsdateien (etwa HTML und CSS) ist kein Neustart des Node.js-Prozesses erforderlich. Ein aktiver `azds up`-Befehl synchronisiert daher automatisch alle geänderten Inhaltsdateien direkt im ausgeführten Container in Azure, sodass Inhaltsänderungen schnell sichtbar werden.

### <a name="test-from-a-mobile-device"></a>Testen auf einem mobilen Gerät
Öffnen Sie die Web-App auf einem mobilen Gerät, indem Sie die öffentliche URL für „webfrontend“ verwenden. Es kann hilfreich sein, die URL zu kopieren und von Ihrem Desktopcomputer an das mobile Gerät zu senden, damit Sie die lange Adresse nicht eingeben müssen. Wenn die Web-App auf Ihrem mobilen Gerät geladen wird, werden Sie feststellen, dass die Benutzeroberfläche auf einem kleinen Gerät nicht richtig angezeigt wird.

Um dieses Problem zu beheben, fügen Sie ein `viewport`-META-Tag hinzu:
1. Öffnen Sie die Datei `./public/index.html`.
1. Fügen Sie im vorhandenen `head`-Element ein `viewport`-META-Tag hinzu:

    ```html
    <head>
        <!-- Add this line -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    ```

1. Speichern Sie die Datei .
1. Aktualisieren Sie den Browser auf dem Gerät. Die Web-App sollte nun ordnungsgemäß gerendert werden. 

Dies ist ein Beispiel dafür, dass Probleme mit einer App erst erkannt werden, wenn die App auf den zu verwendenden Geräten getestet wird. Mit Azure Dev Spaces können Sie schnell Code iterieren und Änderungen auf Zielgeräten überprüfen.

### <a name="update-a-code-file"></a>Aktualisieren einer Codedatei
Die Aktualisierung von serverseitigen Codedateien ist etwas aufwendiger, da eine Node.js-App neu gestartet werden muss.

1. Drücken Sie im Terminalfenster `Ctrl+C` (zum Beenden von `azds up`).
1. Öffnen Sie die Codedatei mit dem Namen `server.js`, und bearbeiten Sie die Begrüßungsmeldung des Diensts: 

    ```javascript
    res.send('Hello from webfrontend running in Azure!');
    ```

3. Speichern Sie die Datei .
1. Führen Sie `azds up` im Terminalfenster aus. 

Dadurch wird das Containerimage neu erstellt und das Helm-Diagramm erneut bereitgestellt. Laden Sie die Browserseite neu, um zu sehen, wie sich Ihre Codeänderungen auswirken.

Es gibt jedoch eine noch *schnellere Methode* für die Codeentwicklung. Sie wird im nächsten Abschnitt erläutert. 

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes

In diesem Abschnitt verwenden Sie VS Code zum direkten Debuggen des in Azure ausgeführten Containers. Außerdem erfahren Sie, wie Sie die Schleife zum Bearbeiten/Ausführen/Testen beschleunigen.

![](./media/common/edit-refresh-see.png)

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Initialisieren von Debugressourcen mit der VS Code-Erweiterung
Sie müssen zunächst Ihr Codeprojekt konfigurieren, damit VS Code mit dem Entwicklungsbereich in Azure kommunizieren kann. Die VS Code-Erweiterung für Azure Dev Spaces enthält einen Befehl zum Einrichten der Debugkonfiguration. 

Öffnen Sie die **Befehlspalette** (über das Menü **Ansicht | Befehlspalette**), verwenden Sie die automatische Vervollständigung für die Eingabe, und wählen Sie den Befehl `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces` aus.

Dadurch wird die Debugkonfiguration für Azure Dev Spaces unter dem `.vscode`-Ordner hinzugefügt.

![](./media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>Auswählen der AZDS-Debugkonfiguration
1. Klicken Sie zum Öffnen der Debugansicht auf der **Aktivitätsleiste** am Rand von VS Code auf das Symbol „Debuggen“.
1. Wählen Sie als aktive Debugkonfiguration **Launch Program (AZDS)** (Programm starten (AZDS)) aus.

![](media/get-started-node/debug-configuration-nodejs2.png)

> [!Note]
> Falls in der Befehlspalette keine Azure Dev Spaces-Befehle angezeigt werden, überprüfen Sie, ob die VS Code-Erweiterung für Azure Dev Spaces installiert wurde.

### <a name="debug-the-container-in-kubernetes"></a>Debuggen des Containers in Kubernetes
Drücken Sie **F5**, um Ihren Code in Kubernetes zu debuggen.

Wie beim Befehl `up` wird der Code mit dem Entwicklungsbereich synchronisiert, wenn Sie das Debuggen starten, und ein Container wird erstellt und in Kubernetes bereitgestellt. Dieses Mal ist der Debugger an den Remotecontainer angefügt.

> [!Tip]
> Auf der Statusleiste von VS Code wird eine klickbare URL angezeigt.

Legen Sie einen Breakpoint in einer serverseitigen Codedatei fest, beispielsweise in `app.get('/api'...` in `server.js`. Aktualisieren Sie die Browserseite, oder klicken Sie auf die Schaltfläche „Say It Again“ (Wiederholen). Dadurch sollte der Breakpoint erreicht werden, und Sie sollten den Code schrittweise durchlaufen können.

Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

### <a name="edit-code-and-refresh-the-debug-session"></a>Bearbeiten von Code und Aktualisieren der Debugsitzung
Nehmen Sie bei aktivem Debugger eine Codeänderung vor. Ändern Sie beispielsweise erneut die Begrüßungsmeldung:

```javascript
app.get('/api', function (req, res) {
    res.send('**** Hello from webfrontend running in Azure! ****');
});
```

Speichern Sie die Datei, und klicken Sie im Bereich für **Debugaktionen** auf die Schaltfläche **Aktualisieren**. 

![](media/get-started-node/debug-action-refresh-nodejs.png)

Das Neuerstellen und erneute Bereitstellen eines neuen Containerimages bei jeder vorgenommenen Codeänderung kann geraume Zeit in Anspruch nehmen. Daher startet Azure Dev Spaces den Node.js-Prozess zwischen den Debugsitzungen neu, um den Bearbeitungs-/Debugging-Kreislauf zu beschleunigen.

Aktualisieren Sie die Web-App im Browser, oder klicken Sie auf die Schaltfläche *Say It Again* (Wiederholen). Daraufhin sollte Ihre benutzerdefinierte Meldung auf der Benutzeroberfläche angezeigt werden.

### <a name="use-nodemon-to-develop-even-faster"></a>Noch schnellere Entwicklung mithilfe von Nodemon

Das Beispielprojekt `webfrontend` wurde für die Verwendung von [nodemon](https://nodemon.io/) konfiguriert – ein beliebtes Tool zur Beschleunigung der Node.js-Entwicklung, das vollständig mit Azure Dev Spaces kompatibel ist.

Probieren Sie die folgenden Schritte aus:
1. Beenden Sie den VS Code-Debugger.
1. Klicken Sie auf der **Aktivitätsleiste** am Rand von VS Code auf das Symbol „Debuggen“. 
1. Wählen Sie als aktive Debugkonfiguration **Attach (AZDS)** (Anfügen (AZDS)) aus.
1. Drücken Sie F5.

In dieser Konfiguration ist der Container für das Starten von *Nodemon* konfiguriert. Bei Codeänderungen startet *Nodemon* genau wie bei der lokalen Entwicklung den Node-Prozess automatisch neu. 
1. Bearbeiten Sie die Begrüßungsmeldung in `server.js` erneut, und speichern Sie die Datei.
1. Aktualisieren Sie den Browser, oder klicken Sie auf die Schaltfläche *Say It Again* (Wiederholen), um Ihre Änderungen sichtbar zu machen.

**Sie kennen jetzt eine Methode für die schnelle Codeiteration und das direkte Debuggen in Kubernetes!**

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Arbeiten mit mehreren Containern und Teamentwicklung](team-development-nodejs.md)
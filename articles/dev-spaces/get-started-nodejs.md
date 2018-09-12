---
title: Erstellen einer Kubernetes-Node.js-Entwicklungsumgebung in der Cloud mit VS Code | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: f441f18ab72485feca9356f7218a35b2c351dd40
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157893"
---
# <a name="get-started-on-azure-dev-spaces-with-nodejs"></a>Erste Schritte in Azure Dev Spaces mit Node.js

[!INCLUDE [](includes/learning-objectives.md)]

[!INCLUDE [](includes/see-troubleshooting.md)]

Sie können nun eine Kubernetes-basierte Entwicklungsumgebung in Azure erstellen.

[!INCLUDE [](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Installieren der Azure CLI
Bei Azure Dev Spaces ist der Einrichtungsaufwand für die lokalen Computer minimal. Der Großteil der Konfiguration Ihres Entwicklungsbereichs wird in der Cloud gespeichert und kann gemeinsam mit anderen Benutzern genutzt werden. Auf dem lokalen Computer kann Windows, Mac oder Linux ausgeführt werden. Für Linux werden die folgenden Distributionen unterstützt: Ubuntu (18.04, 16.04 und 14.04), Debian 8 und 9, RHEL 7, Fedora 26 und höhere Versionen, CentOS 7, openSUSE 42.2 und SLES 12.

Laden Sie zunächst die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) (Command-Line Interface, CLI) herunter, und führen Sie sie aus. 

> [!IMPORTANT]
> Falls die Azure CLI bereits installiert ist, vergewissern Sie sich, dass es sich dabei mindestens um Version 2.0.43 handelt.

[!INCLUDE [](includes/sign-into-azure.md)]

[!INCLUDE [](includes/use-dev-spaces.md)]

[!INCLUDE [](includes/install-vscode-extension.md)]

Während Sie auf die Erstellung des Clusters warten, können Sie mit dem Schreiben von Code beginnen.

## <a name="create-a-nodejs-container-in-kubernetes"></a>Erstellen eines Node.js-Containers in Kubernetes

In diesem Abschnitt erstellen Sie eine Node.js-Web-App und bereiten sie für die Ausführung in einem Container in Kubernetes vor.

### <a name="create-a-nodejs-web-app"></a>Erstellen einer Node.js-Web-App
Laden Sie Beispielcode von GitHub herunter. Navigieren Sie dazu zu https://github.com/Azure/dev-spaces, und klicken Sie auf **Klonen oder herunterladen**, um das GitHub-Repository in Ihre lokale Umgebung herunterzuladen. Der Code für diese Anleitung befindet sich in `samples/nodejs/getting-started/webfrontend`.

[!INCLUDE [](includes/azds-prep.md)]

[!INCLUDE [](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Aktualisieren einer Inhaltsdatei
Bei Azure Dev Spaces geht es nicht nur um die Ausführung von Code in Kubernetes: Mit diesem Dienst sollen Codeänderungen in einer Kubernetes-Umgebung in der Cloud schnell und iterativ sichtbar gemacht werden.

1. Navigieren Sie zur Datei `./public/index.html`, und ändern Sie die HTML. Legen Sie beispielsweise als Hintergrundfarbe der Seite einen Blauton fest:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

2. Speichern Sie die Datei . Kurz darauf wird im Terminalfenster eine Nachricht mit dem Hinweis angezeigt, dass eine Datei im ausgeführten Container aktualisiert wurde.
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

[!INCLUDE [](includes/debug-intro.md)]

[!INCLUDE [](includes/init-debug-assets-vscode.md)]

### <a name="select-the-azds-debug-configuration"></a>Auswählen der AZDS-Debugkonfiguration
1. Klicken Sie zum Öffnen der Debugansicht auf der **Aktivitätsleiste** am Rand von VS Code auf das Symbol „Debuggen“.
1. Wählen Sie als aktive Debugkonfiguration **Launch Program (AZDS)** (Programm starten (AZDS)) aus.

![](media/get-started-node/debug-configuration-nodejs2.png)

> [!Note]
> Falls in der Befehlspalette keine Azure Dev Spaces-Befehle angezeigt werden, überprüfen Sie, ob die [VS Code-Erweiterung für Azure Dev Spaces](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code) installiert wurde.

### <a name="debug-the-container-in-kubernetes"></a>Debuggen des Containers in Kubernetes
Drücken Sie **F5**, um Ihren Code in Kubernetes zu debuggen.

Wie beim Befehl `up` wird der Code mit der Entwicklungsumgebung synchronisiert, wenn Sie das Debuggen starten, und ein Container wird erstellt und in Kubernetes bereitgestellt. Dieses Mal ist der Debugger an den Remotecontainer angefügt.

[!INCLUDE [](includes/tip-vscode-status-bar-url.md)]

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
*Nodemon* ist ein gängiges Tool, das Node.js-Entwickler für die schnelle Entwicklung nutzen. Statt bei jeder serverseitigen Codeänderung den Node-Prozess neu zu starten, konfigurieren Entwickler das Node-Projekt häufig so, dass *Nodemon* Dateiänderungen überwacht und den Serverprozess automatisch neu startet. Bei Verwendung dieser Methode aktualisiert der Entwickler nach einer Codeänderung lediglich den Browser.

Mit Azure Dev Spaces können Sie viele Entwicklungsworkflows nutzen, die auch bei der lokalen Entwicklung zum Einsatz kommen. Zur Veranschaulichung wurde das Beispielprojekt `webfrontend` für die Verwendung von *Nodemon* (Entwicklerabhängigkeit in `package.json`) konfiguriert.

Probieren Sie die folgenden Schritte aus:
1. Beenden Sie den VS Code-Debugger.
1. Klicken Sie auf der **Aktivitätsleiste** am Rand von VS Code auf das Symbol „Debuggen“. 
1. Wählen Sie als aktive Debugkonfiguration **Attach (AZDS)** (Anfügen (AZDS)) aus.
1. Drücken Sie F5.

In dieser Konfiguration ist der Container für das Starten von *Nodemon* konfiguriert. Bei Codeänderungen startet *Nodemon* genau wie bei der lokalen Entwicklung den Node-Prozess automatisch neu. 
1. Bearbeiten Sie die Begrüßungsmeldung in `server.js` erneut, und speichern Sie die Datei.
1. Aktualisieren Sie den Browser, oder klicken Sie auf die Schaltfläche *Say It Again* (Wiederholen), um Ihre Änderungen sichtbar zu machen.

**Sie kennen jetzt eine Methode für die schnelle Codeiteration und das direkte Debuggen in Kubernetes!** Als Nächstes erfahren Sie, wie Sie einen zweiten Container erstellen und aufrufen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zur Teamentwicklung](team-development-nodejs.md)


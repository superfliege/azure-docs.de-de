---
title: 'Tutorial: Azure SignalR Service-Authentifizierung mit Azure Functions'
description: In diesem Tutorial erfahren Sie, wie Sie Azure SignalR-Dienstclients nutzen.
author: sffamily
ms.service: signalr
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/18/2018
ms.author: zhshang
ms.openlocfilehash: 0cd0dcaf200b1248204efc2d2c0011a94d3c41d3
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720967"
---
# <a name="tutorial-azure-signalr-service-authentication-with-azure-functions"></a>Tutorial: Azure SignalR Service-Authentifizierung mit Azure Functions

Hier finden Sie ein ausführliches Tutorial zum Erstellen eines Chatraums mit Authentifizierung und privaten Nachrichten unter Verwendung von Azure Functions, App Service-Authentifizierung und SignalR Service.

## <a name="introduction"></a>Einführung

### <a name="technologies-used"></a>Eingesetzte Technologien:

* [Azure Functions:](https://azure.microsoft.com/services/functions/?WT.mc_id=serverlesschatlab-tutorial-antchu) Back-End-API für das Authentifizieren von Benutzern und das Senden von Chatnachrichten
* [Azure SignalR Service:](https://azure.microsoft.com/services/signalr-service/?WT.mc_id=serverlesschatlab-tutorial-antchu) Übertragen neuer Nachrichten an verbundene Chatclients
* [Azure Storage:](https://azure.microsoft.com/services/storage/?WT.mc_id=serverlesschatlab-tutorial-antchu) Hosten der statischen Website für die Benutzeroberfläche des Chatclients

### <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial ist die folgende Software erforderlich:

* [Git-Client](https://git-scm.com/downloads)
* [Node.js](https://nodejs.org/en/download/) (Version 10.x)
* [.NET SDK](https://www.microsoft.com/net/download) (Version 2.x, erforderlich für Functions-Erweiterungen)
* [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) (Version 2)
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code) mit den folgenden Erweiterungen:
  * [Azure Functions:](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) Verwendung von Azure Functions in VS Code
  * [Live Server:](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) Lokales Bereitstellen von Webseiten für Tests

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich mit Ihren Anmeldeinformationen an.

## <a name="create-an-azure-signalr-service-instance"></a>Erstellen einer Azure SignalR Service-Instanz

Sie erstellen und testen die Azure Functions-App lokal. Die App greift auf eine SignalR Service-Instanz in Azure zu, die zuvor erstellt werden muss.

1. Klicken Sie auf die Schaltfläche **Ressource erstellen** (**+**), um eine neue Azure-Ressource zu erstellen.

1. Suchen Sie nach **SignalR Service**, und wählen Sie den Dienst aus. Klicken Sie auf **Create**.

    ![Neue SignalR Service-Instanz](media/signalr-authenticate-azure-functions/signalr-quickstart-new.png)

1. Geben Sie Folgendes ein:

    | NAME | Wert |
    |---|---|
    | Ressourcenname | Ein eindeutiger Name für die SignalR Service-Instanz |
    | Ressourcengruppe | Erstellen einer neuen Ressourcengruppe |
    | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
    | Preisstufe | Kostenlos |

1. Klicken Sie auf **Create**.

## <a name="initialize-the-function-app"></a>Initialisieren der Funktions-App

### <a name="create-a-new-azure-functions-project"></a>Erstellen eines neuen Azure Functions-Projekts

1. Verwenden Sie in einem neuen VS Code-Fenster `File > Open Folder` im Menü, um einen leeren Ordner an einem geeigneten Speicherort zu erstellen und zu öffnen. Dies wird der Hauptprojektordner für die Anwendung, die Sie erstellen werden.

1. Initialisieren Sie mithilfe der Azure Functions-Erweiterung in VS Code eine Funktions-App im Hauptprojektordner.
    1. Öffnen Sie dazu die Befehlspalette in Visual Studio Code, indem Sie im Menü die Optionen **Ansicht > Befehlspalette** auswählen (Tastenkombination: `Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).
    1. Suchen Sie den Befehl **Azure Functions: Neues Projekt erstellen** und wählen Sie ihn aus.
    1. Daraufhin sollte der Hauptprojektordner angezeigt werden. Wählen Sie ihn aus. (Oder navigieren Sie über „Durchsuchen“ zu diesem Ordner.)
    1. Wählen Sie in der Aufforderung zum Auswählen einer Sprache **JavaScript** aus.

    ![Erstellen einer Funktionen-App](media/signalr-authenticate-azure-functions/signalr-create-vscode-app.png)

### <a name="install-function-app-extensions"></a>Installieren der Funktions-App-Erweiterungen

In diesem Tutorial werden Azure Functions-Bindungen für die Interaktion mit Azure SignalR Service verwendet. Wie die meisten anderen Bindungen stehen die SignalR Service-Bindungen als Erweiterung zur Verfügung, die mithilfe der Azure Functions Core Tools-CLI installiert werden muss, damit sie verwendet werden kann.

1. Öffnen Sie in VS Code ein Terminal, indem Sie im Menü die Optionen **Ansicht > Integriertes Terminal** (STRG-\`) auswählen.

1. Stellen Sie sicher, dass der Hauptprojektordner das aktuelle Verzeichnis ist.

1. Installieren Sie die Erweiterung für die SignalR Service-Funktions-App.

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.SignalRService -v 1.0.0-preview1-10002
    ```

### <a name="configure-application-settings"></a>Konfigurieren von Anwendungseinstellungen

Beim lokalen Ausführen und Debuggen der Azure Functions-Runtime werden Anwendungseinstellungen aus **local.settings.json** gelesen. Aktualisieren Sie diese Datei mit der Verbindungszeichenfolge der zuvor erstellten SignalR Service-Instanz.

1. Wählen Sie in VS Code im Explorer-Bereich die Datei **local.settings.json** aus, um sie zu öffnen.

1. Ersetzen Sie den Inhalt der Datei durch Folgendes:

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureSignalRConnectionString": "<signalr-connection-string>",
            "WEBSITE_NODE_DEFAULT_VERSION": "10.0.0",
            "FUNCTIONS_WORKER_RUNTIME": "node"
        },
        "Host": {
            "LocalHttpPort": 7071,
            "CORS": "*"
        }
    }
    ```

    * Geben Sie die Azure SignalR Service-Verbindungszeichenfolge in eine Einstellung namens `AzureSignalRConnectionString` ein. Den Wert finden Sie im Azure-Portal in der Azure SignalR Service-Ressource auf der Seite **Schlüssel**. Sie können die primäre oder die sekundäre Verbindungszeichenfolge verwenden.
    * Die Einstellung `WEBSITE_NODE_DEFAULT_VERSION` wird nicht lokal verwendet, ist aber bei der Bereitstellung in Azure erforderlich.
    * Im Abschnitt `Host` sind der Port und die CORS-Einstellungen für den lokalen Functions-Host konfiguriert. (Diese Einstellung hat bei der Ausführung in Azure keine Auswirkung.)

    ![Abrufen des SignalR Service-Schlüssels](media/signalr-authenticate-azure-functions/signalr-get-key.png)

1. Speichern Sie die Datei .

    ![Aktualisieren der lokalen Einstellungen](media/signalr-authenticate-azure-functions/signalr-update-local-settings.png)

## <a name="create-a-function-to-authenticate-users-to-signalr-service"></a>Erstellen einer Funktion zum Authentifizieren von Benutzern bei SignalR Service

Wird die Chap-App zum ersten Mal im Browser geöffnet, sind gültige Verbindungsanmeldeinformationen für das Herstellen einer Verbindung mit Azure SignalR Service erforderlich. Sie erstellen in Ihrer Funktions-App eine über HTTP ausgelöste Funktion namens *SignalRInfo*, um diese Verbindungsinformationen zurückzugeben.

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Suchen Sie den Befehl  **Azure Functions: Funktion erstellen**, und wählen Sie ihn aus.

1. Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:

    | NAME | Wert |
    |---|---|
    | Funktions-App-Ordner | Wählen Sie den Hauptprojektordner aus. |
    | Vorlage | HTTP-Trigger |
    | NAME | SignalRInfo |
    | Autorisierungsstufe | Anonym |

    Ein Ordner mit dem Namen **SignalRInfo** wird erstellt, der die neue Funktion enthält.

1. Öffnen Sie **SignalRInfo/function.json**, um Bindungen für die Funktion zu konfigurieren. Ändern Sie den Inhalt der Datei wie unten angegeben. Dadurch wird eine Eingabebindung hinzugefügt, die gültige Anmeldeinformationen für einen Client generiert, um eine Verbindung mit einem Azure SignalR Service-Hub namens `chat` herzustellen.

    ```json
    {
        "disabled": false,
        "bindings": [
            {
                "authLevel": "anonymous",
                "type": "httpTrigger",
                "direction": "in",
                "name": "req"
            },
            {
                "type": "http",
                "direction": "out",
                "name": "res"
            },
            {
                "type": "signalRConnectionInfo",
                "name": "connectionInfo",
                "userId": "",
                "hubName": "chat",
                "direction": "in"
            }
        ]
    }
    ```

    Die Eigenschaft `userId` in der `signalRConnectionInfo`-Bindung wird zum Erstellen einer authentifizierten SignalR Service-Verbindung verwendet. Lassen Sie die Eigenschaft bei der lokalen Entwicklung leer. Sie verwenden sie bei der Bereitstellung der Funktions-App in Azure.

1. Öffnen Sie **SignalRInfo/index.js**, um den Textteil der Funktion anzuzeigen. Ändern Sie den Inhalt der Datei wie unten angegeben.

    ```javascript
    module.exports = function (context, req, connectionInfo) {
        context.res = { body: connectionInfo };
        context.done();
    };
    ```

    Diese Funktion übernimmt die SignalR-Verbindungsinformationen aus der Eingabebindung und gibt sie an den Client im HTTP-Antworttext zurück.

## <a name="create-a-function-to-send-chat-messages"></a>Erstellen einer Funktion zum Senden von Chatnachrichten

Die Web-App benötigt darüber hinaus eine HTTP-API zum Senden von Chatnachrichten. Sie erstellen eine über HTTP ausgelöste Funktion namens *SendMessage*, die mithilfe von SignalR Service Nachrichten an alle verbundenen Clients sendet.

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Suchen Sie den Befehl  **Azure Functions: Funktion erstellen**, und wählen Sie ihn aus.

1. Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:

    | NAME | Wert |
    |---|---|
    | Funktions-App-Ordner | Wählen Sie den Hauptprojektordner aus. |
    | Vorlage | HTTP-Trigger |
    | NAME | SendMessage |
    | Autorisierungsstufe | Anonym |

    Ein Ordner mit dem Namen **SendMessage** wird erstellt, der die neue Funktion enthält.

1. Öffnen Sie **SendMessage/function.json**, um Bindungen für die Funktion zu konfigurieren. Ändern Sie den Inhalt der Datei wie unten angegeben.
    ```json
    {
        "disabled": false,
        "bindings": [
            {
                "authLevel": "anonymous",
                "type": "httpTrigger",
                "direction": "in",
                "name": "req",
                "route": "messages",
                "methods": [
                    "post"
                ]
            },
            {
                "type": "http",
                "direction": "out",
                "name": "res"
            },
            {
                "type": "signalR",
                "name": "signalRMessages",
                "hubName": "chat",
                "direction": "out"
            }
        ]
    }
    ```
    Dadurch werden an der ursprünglichen Funktion zwei Änderungen vorgenommen:
    * Die Route wird in `messages` geändert und der HTTP-Trigger auf die **POST**-HTTP-Methode beschränkt.
    * Eine SignalR Service-Ausgabebindung wird hinzugefügt, die Nachrichten an alle Clients sendet, die mit einem SignalR Service-Hub namens `chat` verbunden sind.

1. Speichern Sie die Datei .

1. Öffnen Sie **SendMessage/index.js**, um den Textteil der Funktion anzuzeigen. Ändern Sie den Inhalt der Datei wie unten angegeben.

    ```javascript
    module.exports = function (context, req) {
        const message = req.body;
        message.sender = req.headers && req.headers['x-ms-client-principal-name'] || '';

        let recipientUserId = '';
        if (message.recipient) {
            recipientUserId = message.recipient;
            message.isPrivate = true;
        }

        context.bindings.signalRMessages = [{
            'userId': recipientUserId,
            'target': 'newMessage',
            'arguments': [ message ]
        }];
        context.done();
    };
    ```

    Diese Funktion übernimmt den Textteil aus der HTTP-Anforderung und sendet ihn an Clients, die mit SignalR Service verbunden sind. Dabei wird auf jedem Client eine Funktion namens `newMessage` aufgerufen.

    Die Funktion kann die Identität des Absenders lesen und den Wert *recipient* im Nachrichtentext akzeptieren, um zuzulassen, dass eine Nachricht privat an einen einzelnen Benutzer gesendet wird. Diese Funktionen werden später im Tutorial verwendet.

1. Speichern Sie die Datei .

## <a name="create-and-run-the-chat-client-web-user-interface"></a>Erstellen und Ausführen der Webbenutzeroberfläche des Chatclients

Die Benutzeroberfläche der Chatanwendung ist eine Single-Page-Webanwendung (Single Page Application, SPA), die mit dem JavaScript-Framework Vue erstellt wird. Sie wird separat von der Funktions-App gehostet. Lokal führen Sie die Webbenutzeroberfläche mit der VS Code-Erweiterung für Live Server aus.

1. Erstellen Sie in VS Code im Stamm des Hauptprojektordners einen neuen Ordner namens **Inhalt**.

1. Erstellen Sie im Ordner **Inhalt** eine neue Datei mit dem Namen **index.html**.

1. Kopieren Sie den Inhalt von **[index.html](https://raw.githubusercontent.com/Azure-Samples/signalr-service-quickstart-serverless-chat/master/docs/demo/chat-with-auth/index.html)**, und fügen Sie ihn ein.

1. Speichern Sie die Datei .

1. Drücken Sie **F5**, um die Funktions-App lokal auszuführen und einen Debugger anzufügen.

1. Öffnen Sie **index.html**, und starten Sie Live Server. Öffnen Sie dazu die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`), und wählen Sie **Live Server: Mit Live-Server öffnen** aus. Live Server öffnet die Anwendung in einem Browser.

1. Die Anwendung wird geöffnet. Geben Sie eine Nachricht ins Chatfeld ein, und drücken Sie die EINGABETASTE. Aktualisieren Sie die Anwendung, um neue Nachrichten anzuzeigen. Da keine Authentifizierung konfiguriert wurde, werden alle Nachrichten anonym gesendet.

## <a name="deploy-to-azure-and-enable-authentication"></a>Bereitstellen in Azure und Aktivieren der Authentifizierung

Sie haben die Funktions-App und die Chatanwendung lokal ausgeführt. Nun stellen Sie sie in Azure bereit und aktivieren Authentifizierung und private Nachrichten in der Anwendung.

### <a name="log-into-azure-with-vs-code"></a>Anmelden bei Azure mit VS Code

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Suchen Sie den Befehl  **Azure: Anmelden**, und wählen Sie ihn aus.

1. Folgen Sie den Anweisungen, um den Anmeldevorgang im Browser abzuschließen.

### <a name="configure-function-app-for-authentication"></a>Konfigurieren der Funktions-App für die Authentifizierung

Bisher funktioniert die Chat-App anonym. In Azure verwenden Sie zum Authentifizieren von Benutzern die [App Service-Authentifizierung](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization). Die Benutzer-ID oder der Benutzername des authentifizierten Benutzers kann an die *SignalRConnectionInfo*-Bindung übergeben werden, um Verbindungsinformationen zu generieren, die als der entsprechende Benutzer authentifiziert sind.

Beim Senden einer Nachricht kann die App entscheiden, ob sie an alle verbundenen Clients oder nur an die Clients gesendet werden soll, die für einen bestimmten Benutzer authentifiziert wurde.

1. Öffnen Sie in VS Code die Datei **SendMessage/function.json**.

1. Fügen Sie in der Eigenschaft *userId* der *SignalRConnectionInfo*-Bindung einen [Bindungsausdruck](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#binding-expressions-and-patterns) ein: `{headers.x-ms-client-principal-name}`. Damit wird der Wert auf den Benutzernamen des authentifizierten Benutzers festgelegt. Das Attribut sollte nun wie folgt aussehen:

    ```json
    {
        "type": "signalRConnectionInfo",
        "name": "connectionInfo",
        "userId": "{headers.x-ms-client-principal-name}",
        "hubName": "chat",
        "direction": "in"
    }
    ```

1. Speichern Sie die Datei .

### <a name="deploy-function-app"></a>Bereitstellen der Funktions-App

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`), und wählen Sie **Azure Functions: Für Funktions-App bereitstellen** aus.

1. Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:

    | NAME | Wert |
    |---|---|
    | Bereitzustellender Ordner | Wählen Sie den Hauptprojektordner aus. |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Funktionen-App | Wählen Sie **Create New Function App** (Neue Funktions-App erstellen) aus. |
    | Name der Funktions-App | Geben Sie einen eindeutigen Namen ein. |
    | Ressourcengruppe | Wählen Sie die gleiche Ressourcengruppe wie für die SignalR Service-Instanz aus. |
    | Speicherkonto | Wählen Sie **Neues Speicherkonto erstellen** aus. |
    | Speicherkontoname | Geben Sie einen eindeutigen Namen ein (3 bis 24 alphanumerische Zeichen). |
    | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |

    Eine neue Funktions-App wird in Azure erstellt, und die Bereitstellung beginnt. Warten Sie, bis die Bereitstellung abgeschlossen ist.

### <a name="upload-function-app-local-settings"></a>Hochladen lokaler Einstellungen für die Funktions-App

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Suchen Sie den Befehl  **Azure Functions: Lokale Einstellungen hochladen**, und wählen Sie ihn aus.

1. Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:

    | NAME | Wert |
    |---|---|
    | Datei für lokale Einstellungen | local.settings.json |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Funktionen-App | Wählen Sie die zuvor bereitgestellte Funktions-App aus. |
    | Name der Funktions-App | Geben Sie einen eindeutigen Namen ein. |

Lokale Einstellungen werden in die Funktions-App in Azure hochgeladen. Wählen Sie **Ja, alle** aus, wenn Sie zum Überschreiben vorhandener Einstellungen aufgefordert werden.

### <a name="enable-function-app-cross-origin-resource-sharing-cors"></a>Aktivieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS) für die Funktions-App

**local.settings.json** enthält zwar eine CORS-Einstellung, sie wird jedoch nicht an die Funktions-App in Azure weitergegeben. Sie müssen sie separat festlegen.

1. Öffnen Sie die VS Code-Befehlspalette (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Suchen Sie den Befehl  **Azure Functions: In Portal öffnen**, und wählen Sie ihn aus.

1. Wählen Sie den Abonnement- und Funktions-App-Namen aus, um die Funktions-App im Azure-Portal zu öffnen.

1. Wählen Sie im daraufhin geöffneten Portal auf der Registerkarte **Plattformfeatures** die Option **CORS** aus.

    ![Suchen von CORS](media/signalr-authenticate-azure-functions/signalr-find-cors.png)

1. Fügen Sie einen Eintrag mit dem Wert `*` hinzu.

1. Entfernen Sie alle anderen vorhandenen Einträge.

1. Klicken Sie auf **Speichern**, um die CORS-Einstellungen zu speichern.

    ![Festlegen von CORS](media/signalr-authenticate-azure-functions/signalr-set-cors.png)

> [!NOTE]
> In einer Anwendung aus der Praxis ist es sicherer, spezifische CORS-Einträge für jede Domäne anzugeben, die sie benötigt, statt CORS für alle Ursprünge (`*`) zuzulassen.

### <a name="update-the-web-app"></a>Aktualisieren der Web-App

1. Navigieren Sie im Azure-Portal zur Seite „Übersicht“ der Funktions-App.

1. Kopieren Sie die URL der Funktions-App.

    ![Abrufen der URL](media/signalr-authenticate-azure-functions/signalr-get-url.png)

1. Öffnen Sie in VS Code die Datei **index.html**, und ersetzen Sie den Wert `apiBaseUrl` durch die URL der Funktions-App.

1. Die Anwendung kann mit Authentifizierung über Azure Active Directory, Facebook, Twitter, Microsoft-Konten oder Google konfiguriert werden. Legen Sie den Wert `authProvider` fest, um den gewünschten Authentifizierungsanbieter auszuwählen.

1. Speichern Sie die Datei .

### <a name="deploy-the-web-application-to-blob-storage"></a>Bereitstellen der Webanwendung in Blobspeicher

Die Webanwendung wird mithilfe des Azure Blob Storage-Features für statische Websites gehostet.

1. Klicken Sie auf die Schaltfläche **Neu** (**+**), um eine neue Azure-Ressource zu erstellen.

1. Wählen Sie unter **Speicher** die Option **Storage-Konto** aus.

1. Geben Sie Folgendes ein:

    | NAME | Wert |
    |---|---|
    | NAME | Ein eindeutiger Name für das Blobspeicherkonto |
    | Kontoart | StorageV2 (universell v2) |
    | Standort | Wählen Sie die gleiche Region wie für Ihre anderen Ressourcen aus. |
    | Replikation | Lokal redundanter Speicher (LRS) |
    | Leistung | Standard |
    | Zugriffsebene | Heiß |
    | Ressourcengruppe | Wählen Sie die gleiche Ressourcengruppe wie für die anderen Ressourcen in diesem Tutorial aus. |

1. Klicken Sie auf **Create**.

    ![Erstellen des Speichers](media/signalr-authenticate-azure-functions/signalr-create-storage.png)

1. Öffnen Sie das Speicherkonto nach seiner Erstellung im Azure-Portal.

1. Wählen Sie im linken Navigationsbereich die Option **Statische Website (Vorschau)** aus.

1. Wählen Sie **Aktivieren** aus.

1. Geben Sie `index.html` als **Name des Indexdokuments** ein.

1. Klicken Sie auf **Speichern**.

    ![Konfigurieren von statischen Websites](media/signalr-authenticate-azure-functions/signalr-static-websites.png)

1. Klicken Sie auf der Seite auf den Link **$web**, um den Blobcontainer zu öffnen.

1. Klicken Sie auf **Hochladen**, und laden Sie die Datei **index.html** im Ordner **Inhalt** hoch.

1. Gehen Sie zurück zur Seite **Statische Website**. Notieren Sie sich den Wert für **Primärer Endpunkt**. Dies ist die URL der Webanwendung.

### <a name="enable-app-service-authentication"></a>Aktivieren der App Service-Authentifizierung

App Service-Authentifizierung unterstützt die Authentifizierung über Azure Active Directory, Facebook, Twitter, Microsoft-Konten und Google.

1. Lassen Sie die Funktions-App im Portal geöffnet. Navigieren Sie zur Registerkarte **Plattformfeatures**, und wählen Sie **Authentifizierung/Autorisierung**.

1. **Aktivieren** Sie die App Service-Authentifizierung.

1. Wählen Sie unter **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option „Mit {zuvor ausgewählter Authentifizierungsanbieter} anmelden“.

1. Geben Sie unter **Zulässige externe Umleitungs-URLs** die URL des primären Webendpunkts Ihres Speicherkontos ein, die Sie zuvor notiert haben.

1. Befolgen Sie zum Abschließen der Konfiguration die Anweisungen in der Dokumentation des ausgewählten Anmeldeanbieters.

    - [Azure Active Directory](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad)
    - [Facebook](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-facebook)
    - [Twitter](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-twitter)
    - [Microsoft-Konto](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-microsoft)
    - [Google](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-google)

### <a name="try-the-application"></a>Testen der Anwendung

1. Navigieren Sie in einem Browser zum primären Webendpunkt des Speicherkontos.

1. Wählen Sie **Anmeldung** aus, um sich mit dem ausgewählten Authentifizierungsanbieter zu authentifizieren.

1. Senden Sie öffentliche Nachrichten, indem Sie sie ins Hauptchatfeld eingeben.

1. Senden Sie private Nachrichten, indem Sie auf einen Benutzernamen im Chatverlauf klicken. Nur der ausgewählte Empfänger erhält diese Nachrichten.

Glückwunsch! Sie haben eine serverlose Echtzeit-Chat-App bereitgestellt.

![Demo](media/signalr-authenticate-azure-functions/signalr-serverless-chat.gif)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zum Bereinigen der im Rahmen dieses Tutorials erstellten Ressourcen löschen Sie die Ressourcengruppe über das Azure-Portal.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Azure Functions mit Azure SignalR Service nutzen. Lesen Sie mehr über das Erstellen von serverlosen Echtzeitanwendungen mit SignalR Service-Bindungen für Azure Functions.

> [!div class="nextstepaction"]
> [Erstellen von Echtzeit-Apps mit Azure Functions](signalr-overview-azure-functions.md)
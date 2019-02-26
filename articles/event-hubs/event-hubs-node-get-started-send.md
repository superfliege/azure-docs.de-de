---
title: 'Senden von Ereignissen unter Verwendung von Node.js: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Node.js-Anwendung, die Ereignisse aus Azure Event Hubs sendet.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: ec3182d11f1b2ffa31acd05fa1f2db695f3f2cf7
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447717"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Node.js geschriebenen Anwendung an einen Event Hub senden.

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort.

Gehen Sie wie im folgenden Artikel beschrieben vor, um die Verbindungszeichenfolge für den Event Hub-Namespace abzurufen: [Rufen Sie die Verbindungszeichenfolge ab](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Die Verbindungszeichenfolge wird im weiteren Verlauf dieses Tutorials benötigt.

## <a name="clone-the-sample-git-repository"></a>Klonen des Git-Beispielrepositorys
Klonen Sie das Git-Beispielrepository von [GitHub](https://github.com/Azure/azure-event-hubs-node) auf Ihren Computer. 

## <a name="install-nodejs-package"></a>Installieren des Node.js-Pakets
Installieren Sie das Node.js-Paket für Azure Event Hubs auf Ihrem Computer. 

```shell
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Klonen des Git-Repositorys
Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) von GitHub herunter, oder klonen Sie es. 

## <a name="send-events"></a>Senden von Ereignisse
Das von Ihnen geklonte SDK enthält mehrere Beispiele, die veranschaulichen, wie Sie mithilfe von Node.js Ereignisse an einen Event Hub senden. In diesem Schnellstart verwenden Sie das Beispiel **simpleSender.js**. Wenn Sie die empfangenen Ereignisse beobachten möchten, öffnen Sie ein weiteres Terminal, und empfangen Sie Ereignisse mithilfe des [Beispiels zum Empfangen](event-hubs-node-get-started-receive.md).

1. Öffnen Sie das Projekt in Visual Studio Code. 
2. Erstellen Sie im Ordner **client** eine Datei mit dem Namen **.env**. Kopieren Sie Beispielumgebungsvariablen aus der Datei **sample.env** im Stammordner, und fügen Sie sie ein.
3. Konfigurieren Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs sowie den Speicherendpunkt. Anweisungen zum Abrufen einer Verbindungszeichenfolge für einen Event Hub finden Sie unter [Abrufen der Verbindungszeichenfolge](event-hubs-create.md#create-an-event-hubs-namespace).
4. Navigieren Sie in der Azure CLI zum Ordnerpfad **client**. Führen Sie die folgenden Befehle aus, um Node-Pakete zu installieren und das Projekt zu erstellen:

    ```shell
    npm i
    npm run build
    ```
5. Starten Sie das Senden von Ereignissen mithilfe des folgenden Befehls: 

    ```shell
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes 
Sehen Sie sich den Beispielcode in der Datei „simpleSender.js“ zum Senden von Ereignissen an einen Event Hub an.

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

Denken Sie daran, die Umgebungsvariablen festzulegen, bevor Sie das Skript ausführen. Ihre Konfiguration kann entweder über die Befehlszeile (siehe folgendes Beispiel) oder mithilfe des [dotenv-Pakets](https://www.npmjs.com/package/dotenv#dotenv) erfolgen. 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie Nachrichten mithilfe von Node.js an einen Event Hub gesendet. Informationen zum Empfangen von Nachrichten von einem Event Hub mit Node.js finden Sie unter [Empfangen von Ereignissen von Event Hubs – Node.js](event-hubs-node-get-started-receive.md).

Sehen Sie sich andere Node.js-Beispiele für Event Hubs bei [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/) an.

---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs mithilfe von Node.js
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: 14ea98b9d31bee08b962e8b3801ed507472ba692
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49455792"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und unter [Event Hubs-Features im Überblick](event-hubs-features.md).

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Node.js geschriebenen Anwendung an einen Event Hub senden.

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen, und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort.

## <a name="clone-the-sample-git-repository"></a>Klonen des Git-Beispielrepositorys
Klonen Sie das Git-Beispielrepository von [Github](https://github.com/Azure/azure-event-hubs-node) auf Ihren Computer. 

## <a name="install-nodejs-package"></a>Installieren des Node.js-Pakets
Installieren Sie das Node.js-Paket für Azure Event Hubs auf Ihrem Computer. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Klonen des Git-Repositorys
Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) von GitHub herunter, oder klonen Sie es. 

## <a name="send-events"></a>Senden von Ereignisse
Das von Ihnen geklonte SDK enthält mehrere Beispiele, die veranschaulichen, wie Sie mithilfe von Node.js Ereignisse an einen Event Hub senden. In diesem Schnellstart verwenden Sie das Beispiel **simpleSender.js**. Wenn Sie die empfangenen Ereignisse beobachten möchten, öffnen Sie ein weiteres Terminal, und empfangen Sie Ereignisse mithilfe des [Beispiels zum Empfangen](event-hubs-node-get-started-receive.md).

1. Öffnen Sie das Projekt in Visual Studio Code. 
2. Erstellen Sie im Ordner **client** eine Datei mit dem Namen **.env**. Kopieren Sie Beispielumgebungsvariablen aus der Datei **sample.env** im Stammordner, und fügen Sie sie ein.
3. Konfigurieren Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs sowie den Speicherendpunkt. Die Verbindungszeichenfolge für Ihren Event Hub können Sie im Azure-Portal auf der Event Hub-Seite unter **RootManageSharedAccessKey** aus **Verbindungszeichenfolge – Primärschlüssel** kopieren. Ausführliche Schritte finden Sie unter [Abrufen der Verbindungszeichenfolge](event-hubs-create.md#create-an-event-hubs-namespace).
4. Navigieren Sie in der Azure CLI zum Ordnerpfad **client**. Führen Sie die folgenden Befehle aus, um Node-Pakete zu installieren und das Projekt zu erstellen:

    ```nodejs
    npm i
    npm run build
    ```
5. Starten Sie das Senden von Ereignissen mithilfe des folgenden Befehls: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes 
Nachfolgend sehen Sie den Beispielcode zum Senden von Ereignissen an einen Event Hub mithilfe von Node.js. Sie können manuell eine Datei „sampleSender.js“ erstellen und ausführen, um Ereignisse an einen Event Hub zu senden. 


```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

Denken Sie daran, die Umgebungsvariablen festzulegen, bevor Sie das Skript ausführen. Die Konfiguration kann entweder über die Befehlszeile (siehe folgendes Beispiel) oder mithilfe des [dotenv-Pakets](https://www.npmjs.com/package/dotenv#dotenv) erfolgen. 

```
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

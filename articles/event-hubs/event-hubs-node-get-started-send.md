---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs mithilfe von Node.js
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: 413f36a12dee135cc1a7dc99a34d8b7b2be6c46f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801062"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten.

Weitere Informationen zu Event Hubs finden Sie unter [Übersicht über Event Hubs](event-hubs-about.md).

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Node.js geschriebenen Anwendung an einen Event Hub senden. Um Ereignisse über das Event Processor Host-Paket von Node.js zu empfangen, lesen Sie [den entsprechenden Artikel zum Empfangen von Ereignissen](event-hubs-node-get-started-receive.md).

Code für diesen Schnellstart ist auf [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) verfügbar. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.

## <a name="create-a-namespace-and-event-hub"></a>Erstellen eines Namespace und eines Event Hubs
Zunächst erstellen Sie über das Azure-Portal einen Event Hubs-Namespace mit einem Event Hub. Besitzen Sie noch keinen Event Hub, können Sie diese Entitäten anhand der Anweisungen unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md) erstellen.

## <a name="clone-the-sample-git-repository"></a>Klonen des Git-Beispielrepositorys
Klonen Sie das Git-Beispielrepository von [Github](https://github.com/Azure/azure-event-hubs-node) auf Ihren Computer. 

## <a name="install-nodejs-package"></a>Installieren des Node.js-Pakets
Installieren Sie das Node.js-Paket für Azure Event Hubs auf Ihrem Computer. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Klonen des Git-Repositorys
Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) von GitHub herunter, oder klonen Sie es. 

## <a name="send-events"></a>Senden von Ereignissen
Das von Ihnen geklonte SDK enthält mehrere Beispiele, die veranschaulichen, wie Sie mithilfe von Node.js Ereignisse an einen Event Hub senden. In diesem Schnellstart verwenden Sie das Beispiel **simpleSender.js**. Wenn Sie die empfangenen Ereignisse beobachten möchten, öffnen Sie ein weiteres Terminal, und empfangen Sie Ereignisse mithilfe des [Beispiels zum Empfangen](event-hubs-node-get-started-receive.md).

1. Öffnen Sie das Projekt in Visual Studio Code. 
2. Erstellen Sie im Ordner **client** eine Datei mit dem Namen **.env**. Kopieren Sie Beispielumgebungsvariablen aus der Datei **sample.env** im Stammordner, und fügen Sie sie ein.
3. Konfigurieren Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs sowie den Speicherendpunkt. Die Verbindungszeichenfolge für Ihren Event Hub können Sie im Azure-Portal auf der Event Hub-Seite unter **RootManageSharedAccessKey** und **Verbindungszeichenfolge – Primärschlüssel** kopieren. Ausführliche Schritte finden Sie unter [Abrufen der Verbindungszeichenfolge](event-hubs-quickstart-portal.md#create-an-event-hubs-namespace).
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

Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

* [Empfangen von Ereignissen mithilfe von Node.js](event-hubs-node-get-started-receive.md)
* [Beispiele auf GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

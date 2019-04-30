---
title: 'Senden und Empfangen von Ereignissen unter Verwendung von Node.js: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Node.js-Anwendung, die Ereignisse aus Azure Event Hubs sendet.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: f03bfde8f7ea37989756ad47678369e94b831438
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677900"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Senden von Ereignissen an oder Empfangen von Ereignissen aus Event Hubs mithilfe von Node.js

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

In diesem Tutorial wird beschrieben, wie Sie Node.js-Anwendungen erstellen, die Ereignisse an einen Event Hub senden oder von diesem empfangen.

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter.
- Visual Studio Code (empfohlen) oder eine beliebige andere IDE.
- **Erstellen Sie einen Event Hubs-Namespace und einen Event Hub**. Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten in diesem Tutorial fort. Gehen Sie dann wie im folgenden Artikel beschrieben vor, um die Verbindungszeichenfolge für den Event Hub-Namespace abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Die Verbindungszeichenfolge wird im weiteren Verlauf dieses Tutorials benötigt.
- Klonen Sie das [GitHub-Beispielrepository](https://github.com/Azure/azure-event-hubs-node) auf Ihren Computer. 


## <a name="send-events"></a>Senden von Ereignisse
In diesem Abschnitt erfahren Sie, wie Sie eine Node.js-Anwendung zum Senden von Ereignissen an einen Event Hub erstellen. 

### <a name="install-nodejs-package"></a>Installieren des Node.js-Pakets
Installieren Sie das Node.js-Paket für Azure Event Hubs auf Ihrem Computer. 

```shell
npm install @azure/event-hubs
```

Wenn Sie das Git-Repository nicht wie in den Voraussetzungen erwähnt geklont haben, laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) aus GitHub. 

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


### <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes 
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

## <a name="receive-events"></a>Empfangen von Ereignissen
In diesem Tutorial wird gezeigt, wie Sie mithilfe eines Azure-[Ereignisprozessorhosts](event-hubs-event-processor-host.md) (EventProcessorHost) in einer Node.js-Anwendung Ereignisse von einem Event Hub empfangen. Der Ereignisprozessorhost ermöglicht den effizienten Empfang von Ereignissen von einem Event Hub. Zu diesem Zweck werden auf allen Partitionen in der Consumergruppe eines Event Hubs Empfänger erstellt. In einer Azure Storage Blob-Instanz werden die Metadaten empfangener Nachrichten in regelmäßigen Abständen mit Prüfpunkten versehen. Dadurch kann der Empfang von Nachrichten später an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

Der Code für diesen Schnellstart ist auf [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor) verfügbar.

### <a name="clone-the-git-repository"></a>Klonen des Git-Repositorys
Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) von GitHub herunter, oder klonen Sie es. 

### <a name="install-the-eventprocessorhost"></a>Installieren des Ereignisprozessorhosts
Installieren Sie den Ereignisprozessorhost für das Event Hubs-Modul. 

```shell
npm install @azure/event-processor-host
```

### <a name="receive-events-using-eventprocessorhost"></a>Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts
Das von Ihnen geklonte SDK enthält mehrere Beispiele, die veranschaulichen, wie Sie mithilfe von Node.js Ereignisse von einem Event Hub empfangen. In dieser Schnellstartanleitung verwenden Sie das Beispiel **singleEPH.js**. Wenn Sie die empfangenen Ereignisse überwachen möchten, öffnen Sie ein weiteres Terminal, und senden Sie Ereignisse mithilfe des [Beispiels zum Senden](event-hubs-node-get-started-send.md).

1. Öffnen Sie das Projekt in Visual Studio Code. 
2. Erstellen Sie im Ordner **processor** eine Datei mit dem Namen **.env**. Kopieren Sie Beispielumgebungsvariablen aus der Datei **sample.env** im Stammordner, und fügen Sie sie ein.
3. Konfigurieren Sie die Verbindungszeichenfolge und den Namen Ihres Event Hubs sowie den Speicherendpunkt. Die Verbindungszeichenfolge für Ihren Event Hub können Sie im Azure-Portal auf der Event Hub-Seite unter **RootManageSharedAccessKey** aus **Verbindungszeichenfolge – Primärschlüssel** kopieren. Ausführliche Schritte finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md#create-an-event-hubs-namespace).
4. Navigieren Sie in der Azure-Befehlszeilenschnittstelle zum Ordnerpfad **processor**. Führen Sie die folgenden Befehle aus, um Knotenpakete zu installieren und das Projekt zu erstellen:

    ```shell
    npm i
    npm run build
    ```
5. Führen Sie den folgenden Befehl aus, um Ereignisse mit Ihrem Ereignisprozessorhost zu empfangen:

    ```shell
    node dist/examples/singleEph.js
    ```

### <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes 
Nachfolgend sehen Sie den Beispielcode zum Empfangen von Ereignissen von einem Event Hub mithilfe von Node.js. Sie können manuell die Datei „sampleEph.js“ erstellen und ausführen, um Ereignisse von einem Event Hub zu empfangen. 

  ```javascript
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processor Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path,
        onEphError: (error) => {
          console.log("This handler will notify you of any internal errors that happen " +
          "during partition and lease management: %O", error);
        }
      }
    );
    let count = 0;
    // Message event handler
    const onMessage = async (context/*PartitionContext*/, data /*EventData*/) => {
      console.log(">>>>> Rx message from '%s': '%s'", context.partitionId, data.body);
      count++;
      // let us checkpoint every 100th message that is received across all the partitions.
      if (count % 100 === 0) {
        return await context.checkpoint();
      }
    };
    // Error event handler
    const onError = (error) => {
      console.log(">>>>> Received Error: %O", error);
    };
    // start the EPH
    await eph.start(onMessage, onError);
    // After some time let' say 2 minutes
    await delay(120000);
    // This will stop the EPH.
    await eph.stop();
  }
  
  main().catch((err) => {
    console.log(err);
  });
      
  ```

Legen Sie die Umgebungsvariablen fest, bevor Sie das Skript ausführen. Die Konfiguration kann entweder über die Befehlszeile (siehe folgendes Beispiel) oder mithilfe des [dotenv-Pakets](https://www.npmjs.com/package/dotenv#dotenv) erfolgen. 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

Weitere Beispiele finden Sie [hier](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Features und Terminologie in Azure Event Hubs](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
- Sehen Sie sich andere Node.js-Beispiele für Event Hubs bei [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/) an.

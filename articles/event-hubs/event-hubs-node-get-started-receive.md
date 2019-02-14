---
title: 'Empfangen von Ereignissen unter Verwendung von Node.js: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Node.js-Anwendung, die Ereignisse von Azure Event Hubs empfängt.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 55e4ce4a59a498c5c22fe6e701e3140aa65e7a10
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895243"
---
# <a name="receive-events-from-azure-event-hubs-using-nodejs"></a>Empfangen von Ereignissen von Azure Event Hubs mithilfe von Node.js

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten. Sie können auch Ereignisdaten in Azure Storage oder Azure Data Lake Store vor ihrer Verarbeitung erfassen.  

Weitere Informationen zu Event Hubs finden Sie in der [Übersicht über Event Hubs](event-hubs-about.md).

In diesem Tutorial wird gezeigt, wie Sie mithilfe eines Azure-[Ereignisprozessorhosts](event-hubs-event-processor-host.md) (EventProcessorHost) in einer Node.js-Anwendung Ereignisse von einem Event Hub empfangen. Der Ereignisprozessorhost ermöglicht den effizienten Empfang von Ereignissen von einem Event Hub. Zu diesem Zweck werden auf allen Partitionen in der Consumergruppe eines Event Hubs Empfänger erstellt. In einer Azure Storage Blob-Instanz werden die Metadaten empfangener Nachrichten in regelmäßigen Abständen mit Prüfpunkten versehen. Dadurch kann der Empfang von Nachrichten später an der Stelle fortgesetzt werden, an der Sie aufgehört haben.

Der Code für diesen Schnellstart ist auf [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor) verfügbar.

> [!NOTE]
>  Zum Senden von Ereignissen an Event Hubs mit Node.js lesen Sie den folgenden Artikel: [Senden von Ereignissen an Azure Event Hubs mithilfe von Node.js](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Node.js, Version 8.x oder höher. Laden Sie die neueste LTS-Version von [https://nodejs.org](https://nodejs.org) herunter. Verwenden Sie keine ältere LTS-Version von Node.js. 
- Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.

## <a name="create-a-namespace-and-event-hub"></a>Erstellen eines Namespace und eines Event Hubs
Zunächst erstellen Sie über das Azure-Portal einen Event Hubs-Namespace mit einem Event Hub. Besitzen Sie noch keinen, können Sie diese Entitäten anhand der Anweisungen unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md) erstellen.

## <a name="create-a-storage-account-and-container"></a>Erstellen eines Speicherkontos und Containers
Um den Ereignisprozessorhost verwenden zu können, benötigen Sie ein Azure Storage-Konto. Statusinformationen, etwa Leases auf Partitionen und Prüfpunkte im Ereignisstream, werden über einen Azure Storage-Container zwischen den Empfängern freigegeben. Befolgen Sie die Anweisungen [in diesem Artikel](../storage/common/storage-quickstart-create-account.md), um ein Azure Storage-Konto zu erstellen.

## <a name="clone-the-git-repository"></a>Klonen des Git-Repositorys
Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) von GitHub herunter, oder klonen Sie es. 

## <a name="install-the-eventprocessorhost"></a>Installieren des Ereignisprozessorhosts
Installieren Sie den Ereignisprozessorhost für das Event Hubs-Modul. 

```shell
npm install @azure/event-processor-host
```

## <a name="receive-events-using-eventprocessorhost"></a>Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts
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

## <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes 
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

In dieser Schnellstartanleitung haben Sie eine Node.js-Anwendung erstellt, die Nachrichten von einem Event Hub empfängt. Informationen dazu, wie Sie Ereignisse unter Verwendung von Node.js an einen Event Hub senden, finden Sie unter [Erste Schritte beim Senden von Nachrichten an Azure Event Hubs: Node.js](event-hubs-node-get-started-send.md).

<!-- Links -->
[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

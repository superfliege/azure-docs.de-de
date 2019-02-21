---
title: 'Senden von Ereignissen unter Verwendung von Go: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Go-Anwendung, die Ereignisse aus Azure Event Hubs sendet.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: c0583f67d2351d05f877d0ebc1f29cea9e52c5eb
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311986"
---
# <a name="send-events-to-event-hubs-using-go"></a>Senden von Ereignissen an Event Hubs mithilfe von Go

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Go geschriebenen Anwendung an einen Event Hub senden. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Eine lokale Go-Installation. Befolgen Sie bei Bedarf [diese Anweisungen](https://golang.org/doc/install).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub.

Fahren Sie nun mit den folgenden Schritten in diesem Tutorial fort.

## <a name="install-go-package"></a>Installieren des Go-Pakets

Rufen Sie das Go-Paket für Event Hubs mit `go get` oder `dep` ab. Beispiel: 

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Importieren von Paketen in eine Codedatei

Verwenden Sie das folgende Codebeispiel, um Go-Pakete zu importieren:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals

Erstellen Sie anhand der Anweisungen unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) einen neuen Dienstprinzipal. Speichern Sie die bereitgestellten Anmeldeinformationen in Ihrer Umgebung mit den folgenden Namen. Sowohl das Azure SDK für Go als auch die Event Hubs-Pakete sind vorkonfiguriert, um nach diesen Variablennamen zu suchen:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Erstellen Sie dann einen Autorisierungsanbieter für Ihren Event Hubs-Client, der diese Anmeldeinformationen verwendet:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Erstellen eines Event Hubs-Clients

Der folgende Code erstellt einen neuen Event Hubs-Client:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>Senden von Nachrichten

Verwenden Sie im folgenden Ausschnitt „1.“, um Nachrichten interaktiv von einem Terminal aus zu senden, oder „2.“, um Nachrichten innerhalb Ihres Programms zu senden:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!"))
```

## <a name="extras"></a>Extras

Rufen Sie die IDs der Partitionen in Ihrem Event Hub ab:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n", info.PartitionIDs)
```

Führen Sie die Anwendung aus, um Ereignisse an den Event Hub zu senden. 

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie Nachrichten mithilfe von Go an einen Event Hub gesendet. Informationen dazu, wie Sie Nachrichten unter Verwendung von Go von einem Event Hub empfangen, erhalten Sie unter [Empfangen von Ereignissen von Event Hubs mithilfe von Go](event-hubs-go-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

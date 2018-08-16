---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Go | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs mithilfe von Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: 40b3aa82c3e9e8ab9a30362c0a41998877655725
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005596"
---
# <a name="send-events-to-event-hubs-using-go"></a>Senden von Ereignissen an Event Hubs mithilfe von Go

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten.

Weitere Informationen zu Event Hubs finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Go geschriebenen Anwendung an einen Event Hub senden. Verwenden Sie das **Go eph**-Paket (Eventprozessorhost), um Ereignisse zu empfangen. Weitere Informationen dazu finden Sie in [diesem Artikel](event-hubs-go-get-started-receive-eph.md).

Der Code in diesem Tutorial stammt aus [ diesen GitHub-Beispielen](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), die weitere Informationen zur voll funktionsfähigen Anwendung einschließlich Importanweisungen und Variablendeklarationen enthalten.

Weitere Beispiele stehen Ihnen im [Event Hubs-Paketrepository](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Eine lokale Go-Installation. Befolgen Sie bei Bedarf [diese Anweisungen](https://golang.org/doc/install).
* Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.
* Einen vorhandenen Event Hubs-Namespace und einen vorhandenen Event Hub. Die Anweisungen [in diesem Artikel](event-hubs-create.md) zeigen Ihnen, wie Sie diese Entitäten erstellen.

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
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Extras

Rufen Sie die IDs der Partitionen in Ihrem Event Hub ab:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie auf den folgenden Seiten:

* [Empfangen von Ereignissen mithilfe von EventProcessorHost](event-hubs-go-get-started-receive-eph.md)
* [Übersicht über Event Hubs][Event Hubs overview]
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

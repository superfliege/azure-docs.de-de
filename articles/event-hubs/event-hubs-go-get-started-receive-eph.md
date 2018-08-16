---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von Go | Microsoft-Dokumentation
description: Erste Schritte beim Empfangen von Ereignissen von Event Hubs mithilfe von Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: eaea6adbaef7baf9bb1e617ba0a709cf14edf781
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005577"
---
# <a name="receive-events-from-event-hubs-using-go"></a>Empfangen von Ereignissen von Event Hubs mithilfe von Go

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten.

Weitere Informationen zu Event Hubs finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

Dieses Tutorial beschreibt, wie Sie Ereignisse von einem Event Hub in einer Go-Anwendung empfangen. Unter [Senden von Ereignissen mithilfe von Go](event-hubs-go-get-started-send.md) erfahren Sie mehr darüber.

Der Code in diesem Tutorial stammt aus [ diesen GitHub-Beispielen](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), die weitere Informationen zur voll funktionsfähigen Anwendung einschließlich Importanweisungen und Variablendeklarationen enthalten.

Weitere Beispiele stehen Ihnen im [Event Hubs-Paketrepository](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials benötigen Sie Folgendes:

* Eine lokale Go-Installation. Befolgen Sie bei Bedarf [diese Anweisungen](https://golang.org/doc/install).
* Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.
* Im Ziel-Event Hub müssen sich Nachrichten befinden, damit Nachrichten empfangen werden können. Weitere Informationen zum Senden von Nachrichten finden Sie in [diesem Tutorial](event-hubs-go-get-started-send.md).
* Einen vorhandenen Event Hub (siehe folgender Abschnitt).
* Ein vorhandenes Speicherkonto und einen vorhandenen Container (siehe übernächster Abschnitt).

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Dieses Tutorial beginnt mit einem vorhandenen Event Hubs-Namespace und Event Hub. Die Anweisungen [in diesem Artikel](event-hubs-create.md) zeigen Ihnen, wie Sie diese Entitäten erstellen.

### <a name="create-a-storage-account-and-container"></a>Erstellen eines Storage-Kontos und -Containers

Angaben zum Status, z.B. Leases auf Partitionen und Prüfpunkte im Ereignisstream, werden über einen Azure Storage-Container zwischen den Empfängern freigegeben. Sie können ein Speicherkonto und einen Container erstellen, indem Sie das Go SDK verwenden oder die Anweisungen unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md) befolgen.

Beispiele zum Erstellen von Storage-Artefakte mit dem Go SDK finden Sie im [Go-Beispielrepository](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) und im Beispiel für dieses Tutorial.

## <a name="receive-messages"></a>Empfangen von Nachrichten

Um Nachrichten zu empfangen, rufen Sie die Go-Pakete für Event Hubs mit `go get` oder `dep` ab:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

## <a name="import-packages-in-your-code-file"></a>Importieren von Paketen in eine Codedatei

Verwenden Sie das folgende Codebeispiel, um Go-Pakete zu importieren:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

## <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals

Erstellen Sie anhand der Anweisungen unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli) einen neuen Dienstprinzipal. Speichern Sie die bereitgestellten Anmeldeinformationen in Ihrer Umgebung mit den folgenden Namen. Sowohl das Azure SDK für Go als auch das Event Hubs-Paket sind vorkonfiguriert, um nach diesen Variablennamen zu suchen.

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

## <a name="get-metadata-struct"></a>Metadatenstruktur abrufen

Mit dem Azure SDK für Go können Sie eine Metadatenstruktur Ihrer Azure-Umgebung abrufen. Mithilfe dieser Struktur lassen sich in späteren Vorgängen die richtigen Endpunkte finden.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

## <a name="create-credential-helper"></a>Erstellen eines Anmeldeinformationen-Hilfsprogramms 

Erstellen Sie ein Anmeldeinformationen-Hilfsprogramm, das mithilfe der vorherigen Azure Active Directory-Anmeldeinformationen (AAD) Shared Access Signature-Anmeldeinformationen (SAS) für Storage erstellt. Der letzte Parameter weist diesen Konstruktor an, die gleichen Umgebungsvariablen wie zuvor zu verwenden:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

## <a name="create-checkpointer-and-leaser"></a>Erstellen eines Checkpointers und Leasers 

Erstellen Sie einen **Leaser**, der eine Partition an einen bestimmten Empfänger least, und einen **Checkpointer**, der Prüfpunkte für den Nachrichtenstream schreibt, damit andere Empfänger ab dem richtigen Offset lesen können.

Derzeit ist ein einzelnes **StorageLeaserCheckpointer**-Element verfügbar, das denselben Storage-Container für die Verwaltung von Leases und Checkpoints verwendet. Zusätzlich zu den Speicherkonten- und Containernamen benötigt **StorageLeaserCheckpointer** die im vorherigen Schritt erstellten Anmeldeinformationen und die Azure-Umgebungsstruktur, um korrekt auf den Container zuzugreifen.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

## <a name="construct-event-processor-host"></a>Erstellen eines Ereignisprozessorhosts

Nun sind alle Voraussetzungen erfüllt, um EventProcessorHost wie folgt zu erstellen. Dasselbe **StorageLeaserCheckpointer**-Element wird sowohl als Leaser als auch als Checkpointer verwendet, wie zuvor beschrieben:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

## <a name="create-handler"></a>Erstellen eines Handlers 

Erstellen Sie einen Handler, und registrieren Sie ihn beim Ereignisprozessorhost. Wenn der Host gestartet wird, wendet er diesen und alle anderen angegebenen Handler auf eingehende Nachrichten an:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

## <a name="receive-messages"></a>Empfangen von Nachrichten

Wenn alles eingerichtet ist, starten Sie den Ereignisprozessorhost entweder mit `Start(context)`, damit er permanent ausgeführt wird, oder mit `StartNonBlocking(context)`, damit er nur ausgeführt wird, wenn Nachrichten verfügbar sind.

In diesem Tutorial erfolgen der Start und die Ausführung wie folgt. Im GitHub-Beispiel finden Sie ein Beispiel für die Verwendung von `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="notes"></a>Notizen

In diesem Tutorial wird eine einzelne Instanz von **EventProcessorHost**verwendet. Um den Durchsatz und die Zuverlässigkeit zu erhöhen, führen Sie mehrere Instanzen von **EventProcessorHost** auf verschiedenen Systemen aus. Das Leasersystem stellt sicher, dass zu einem angegebenen Zeitpunkt nur ein Empfänger einer bestimmten Partition zugeordnet ist und Nachrichten von dieser empfängt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie auf diesen Seiten:

* [Senden von Ereignissen mithilfe von Go](event-hubs-go-get-started-send.md)
* [Übersicht über Event Hubs](event-hubs-about.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[kostenloses Konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

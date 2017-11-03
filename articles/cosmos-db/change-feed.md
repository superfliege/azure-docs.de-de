---
title: "Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB | Microsoft-Dokumentation"
description: "Verwenden Sie die Azure Cosmos DB-Unterstützung von Änderungsfeeds, um Änderungen in Dokumenten nachzuverfolgen, eine ereignisbasierte Verarbeitung (wie mit Triggern) durchzuführen und Caches und Analysesysteme auf dem neuesten Stand zu halten."
keywords: "Änderungsfeed"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 10/10/2017
ms.author: arramac
ms.openlocfilehash: 0971959fb168d92096531d1c081666cf301608cf
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB

[Azure Cosmos DB](../cosmos-db/introduction.md) ist eine schnelle und flexible global replizierte Datenbank, die sich ideal für IoT-, Spiele-, Verkaufs- und Vorgangsprotokollierungsanwendungen eignet. Ein allgemeines Entwurfsmuster in diesen Anwendungen ist die Verwendung von Datenänderungen, um zusätzliche Aktionen zu starten. Diese zusätzlichen Aktionen können z.B. folgende sein: 

* Auslösen einer Benachrichtigung oder eines Aufrufs einer API, wenn ein Dokument eingefügt oder geändert wird.
* Datenstromverarbeitung für IoT oder das Ausführen von Analysen.
* Zusätzliche Datenverschiebung durch Synchronisieren mit einem Cache, einer Suchmaschine oder einem Datawarehouse, oder Archivieren von Daten zu Cold Storage.

Die **Unterstützung von Änderungsfeeds** in Azure Cosmos DB ermöglicht Ihnen das Erstellen effizienter und skalierbarer Lösungen für jedes dieser Muster, wie in der folgenden Abbildung gezeigt:

![Verwenden des Azure Cosmos DB-Änderungsfeeds für Echtzeitanalysen und ereignisgesteuerte Computingszenarien](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Unterstützung von Änderungsfeeds wird für alle Datenmodelle und Container in Azure Cosmos DB bereitgestellt. Der Änderungsfeed wird allerdings mithilfe des DocumentDB-Clients gelesen und serialisiert Elemente in das JSON-Format. Aufgrund der JSON-Formatierung tritt bei MongoDB-Clients ein Konflikt zwischen BSON-formatierten Dokumenten und dem JSON-formatierten Änderungsfeed auf. 

## <a name="how-does-change-feed-work"></a>Wie funktioniert der Änderungsfeed?

Zur Unterstützung des Änderungsfeeds in Azure Cosmos DB wird gelauscht, ob in einer Azure Cosmos DB-Sammlung Änderungen auftreten. Anschließend wird die sortierte Liste von geänderten Dokumenten in der Reihenfolge ausgegeben, in der sie geändert wurden. Die Änderungen bleiben erhalten und können asynchron und inkrementell verarbeitet werden, und die Ausgabe kann über einen oder mehrere Consumer für eine parallele Verarbeitung verteilt werden. 

Sie können den Änderungsfeed auf drei verschiedene Arten lesen, wie weiter unten in diesem Artikel erläutert:

1.  [Verwenden von Azure Functions](#azure-functions)
2.  [Arbeiten mit der REST-API und den DocumentDB SDKs](#rest-apis)
3.  [Change Feed Processor-Bibliothek](#change-feed-processor)

Der Änderungsfeed ist für jeden Partitionsschlüsselbereich innerhalb der Dokumentsammlung verfügbar. Daher kann er für eine parallele Verarbeitung über einen oder mehrere Consumer verteilt werden, wie in der folgenden Abbildung gezeigt.

![Verteilte Verarbeitung des Azure Cosmos DB-Änderungsfeeds](./media/change-feed/changefeedvisual.png)

Weitere Details:
* Der Änderungsfeed ist standardmäßig für alle Konten aktiviert.
* Sie können Ihren [bereitgestellten Durchsatz](request-units.md) in Ihrer Schreibregion oder einer [Leseregion](distribute-data-globally.md) zum Lesen aus dem Änderungsfeed verwenden, so wie es bei allen anderen Vorgängen von Azure Cosmos DB möglich ist.
* Der Änderungsfeed enthält Einfüge- und Aktualisierungsvorgänge, die an Dokumenten in der Sammlung vorgenommen wurden. Sie können Löschvorgänge durch ein Kennzeichen für „vorläufiges Löschen“ erfassen, dass den gelöschten Text in Ihren Dokumenten ersetzt. Alternativ können Sie über die [TTL-Funktion](time-to-live.md) einen begrenzten Ablaufzeitraum für Ihre Dokumente festlegen, z.B. 24 Stunden. Verwenden Sie den Wert dieser Eigenschaft, um Löschvorgänge zu erfassen. Mit dieser Lösung müssen Sie Änderungen in einem kürzeren Zeitraum als der TTL-Ablaufzeitraum verarbeiten.
* Jede Änderung eines Dokuments kommt genau einmal im Änderungsfeed vor, und Clients verwalten ihre Prüfpunktausführungs-Logik. Die Änderungsfeed-Prozessorbibliothek bietet automatische Prüfpunktausführung und „Mindestens einmal“-Semantik.
* Nur die letzte Änderung für ein bestimmtes Dokument ist im Änderungsprotokoll enthalten. Zwischenzeitliche Änderungen sind möglicherweise nicht verfügbar.
* Der Änderungsfeed ist anhand der Reihenfolge der Änderungen innerhalb der einzelnen Partitionsschlüsselwerte sortiert. Es gibt keine festgelegte Reihenfolge über Partitionsschlüsselwerte hinweg.
* Änderungen können für jeden Zeitpunkt synchronisiert werden, d.h., es gibt keine feste Datenaufbewahrungsdauer, in der die Änderungen verfügbar sind.
* Änderungen sind in Abschnitten von Partitionsschlüsselbereichen verfügbar. Auf diese Weise können Änderungen aus umfangreichen Sammlungen parallel von mehreren Consumern/Servern verarbeitet werden.
* Anwendungen können mehrere Änderungsfeeds gleichzeitig für dieselbe Sammlung anfordern.

## <a name="use-cases-and-scenarios"></a>Anwendungsfälle und -szenarien

Der Änderungsfeed ermöglicht eine effiziente Verarbeitung großer Datasets mit vielen Schreibvorgängen und bietet eine Alternative zum Abfragen des gesamten Datasets, um Änderungen zu identifizieren. 

Sie können mit einem Änderungsfeed beispielsweise die folgenden Aufgaben effizient ausführen:

* Aktualisieren eines Caches, eines Suchindex oder eines Data Warehouse mit in Azure Cosmos DB gespeicherten Daten.
* Implementieren von Datentiering und -archivierung auf Anwendungsebene, d. h. Speichern von „aktiven Daten“ in Azure Cosmos DB und Auslagern von „inaktiven Daten“ in [Azure Blob Storage](../storage/common/storage-introduction.md) oder [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementieren von Batchanalysen der Daten unter Verwendung von [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Ausführen von Migrationen zu einem anderen Azure Cosmos DB-Konto mit einem anderen Partitionierungsschema ohne Ausfallzeit.
* Implementieren von [Lambda-Pipelines in Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) mit Azure Cosmos DB. Azure Cosmos DB bietet eine skalierbare Datenbanklösung, die die Erfassung und Abfrage verarbeiten kann, und implementiert Lambda-Architekturen mit geringen Gesamtkosten. 
* Empfangen und Speichern von Ereignisdaten von Geräten, Sensoren, Infrastrukturen und Anwendungen und Verarbeiten dieser Ereignisse in Echtzeit mit [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md) oder [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

Die folgende Abbildung zeigt, wie Lambda-Pipelines, die sowohl aufnehmen als auch abfragen, mithilfe von Azure Cosmos DB die Unterstützung von Änderungsfeeds nutzen können: 

![Azure Cosmos DB-basierte Lambda-Pipeline für die Erfassung und Abfrage](./media/change-feed/lambda.png)

In Ihren [serverlosen](http://azure.com/serverless) Web- und mobilen Apps können Sie auch Ereignisse wie Änderungen am Profil, an den Voreinstellungen oder am Speicherort des Kunden nachverfolgen, um bestimmte Aktionen wie das Senden von Pushbenachrichtigungen an deren Geräte mit [Azure Functions](#azure-functions) auszulösen. Wenn Sie Azure Cosmos DB zum Erstellen eines Spiels verwenden, können Sie den Änderungsfeed beispielsweise verwenden, um in Echtzeit Bestenlisten anhand der Ergebnisse von abgeschlossenen Spiele zu implementieren.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Verwenden von Azure Functions 

Wenn Sie Azure Functions verwenden, ist die einfachste Möglichkeit zum Herstellen einer Verbindung mit einem Azure Cosmos DB-Änderungsfeed das Hinzufügen eines Azure Cosmos DB-Triggers zu Ihrer Azure Functions-App. Wenn Sie einen Azure Cosmos DB-Trigger in einer Azure Functions-App erstellen, wählen Sie die Azure Cosmos DB-Sammlung aus, mit der eine Verbindung hergestellt werden soll, und die Funktion wird immer dann ausgelöst, wenn eine Änderung der Sammlung erfolgt ist. 

Trigger können im Azure Functions-Portal, Azure Cosmos DB-Portal oder programmgesteuert erstellt werden. Weitere Informationen finden Sie unter [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](serverless-computing-database.md).

<a id="rest-apis"></a>
## <a name="using-the-sdk"></a>Verwenden des SDK

Mit dem [DocumentDB SDK](documentdb-sdk-dotnet.md) für Azure Cosmos DB können Sie uneingeschränkt einen Änderungsfeed lesen und verwalten. Doch mit zunehmendem Leistungspotenzial wächst auch die Verantwortung. Wenn Sie Prüfpunkte verwalten, mit Dokumentsequenznummern umgehen und präzise Kontrolle über Partitionsschlüssel haben möchten, ist der Einsatz des SDK wohl der richtige Ansatz.

Dieser Abschnitt führt Sie durch das Arbeiten mit einem Änderungsfeed mithilfe des DocumentDB-SDK.

1. Beginnen Sie, indem Sie die folgenden Ressourcen aus appconfig lesen. Anweisungen zum Abrufen des Endpunkts und Autorisierungsschlüssels finden Sie unter [Aktualisieren der Verbindungszeichenfolge](create-documentdb-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. Erstellen Sie den Client wie folgt:

    ```csharp
    using (client = new DocumentClient(new Uri(endpointUrl), authorizationKey,
    new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    {
    }
    ```

3. Rufen Sie die Partitionsschlüsselbereiche ab:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. Rufen Sie ExecuteNextAsync für jeden Partitionsschlüsselbereich ab:

    ```csharp
    foreach (PartitionKeyRange pkRange in partitionKeyRanges){
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);
        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collectionUri,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = -1,
                // Set reading time: only show change feed results modified since StartTime
                StartTime = DateTime.Now - TimeSpan.FromSeconds(30)
            });
        while (query.HasMoreResults)
            {
                FeedResponse<dynamic> readChangesResponse = query.ExecuteNextAsync<dynamic>().Result;
    
                foreach (dynamic changedDocument in readChangesResponse)
                    {
                         Console.WriteLine("document: {0}", changedDocument);
                    }
                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
    }
    ```

Wenn Sie über mehrere Reader verfügen, können Sie mit **ChangeFeedOptions** die Leselast auf verschiedene Threads oder Clients verteilen.

Das war schon alles, und mit diesen wenigen Codezeilen können Sie beginnen, den Änderungsfeed zu lesen. Sie erhalten den vollständigen in diesem Artikel verwendeten Code im [azure-cosmos-db-DocumentFeed-GitHub-Repository](https://github.com/rsarosh/azure-cosmos-db-DocumentFeed).

Im Code von Schritt 4 oben enthält **ResponseContinuation** in der letzten Zeile die letzte logische Sequenznummer (Logical Sequence Number, LSN) des Dokuments, die Sie verwenden werden, wenn Sie das nächste Mal nach dieser Sequenznummer neue Dokumente lesen. Mithilfe von **StartTime** von **ChangeFeedOption** können Sie den Zeitraum erweitern, um die Dokumente zu erhalten. Wenn also **ResponseContinuation** NULL ist, Ihre **StartTime** jedoch zeitlich zurückliegt, erhalten Sie alle Dokumente, die seit **StartTime** geändert wurden. Wenn aber **ResponseContinuation** einen Wert enthält, erhalten Sie vom System alle Dokumente ab dieser LSN.

So verwaltet Ihr Prüfpunktarray die LSN für jede Partition. Aber wenn Sie nicht mit Partitionen, Prüfpunkten, LSN, Startzeit usw. arbeiten möchten, steht Ihnen als einfachere Option die Verwendung der Change Feed Processor-Bibliothek zur Verfügung.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Verwenden der Change Feed Processor-Bibliothek 

Mit der [Change Feed Processor-Bibliothek von Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed) können Sie die Ereignisverarbeitung mühelos auf mehrere Consumer verteilen. Diese Bibliothek vereinfacht das Partitionen übergreifende Lesen von Änderungen und die Parallelverarbeitung mehrerer Threads.

Der wichtigste Vorteil der Change Feed Processor-Bibliothek ist, dass Sie nicht jede Partition und jedes Fortsetzungstoken verwalten und nicht jede Sammlung manuell abrufen müssen.

Die Change Feed Processor-Bibliothek vereinfacht das Partitionen übergreifende Lesen von Änderungen und die Parallelverarbeitung mehrerer Threads.  Sie verwaltet automatisch das Partitionen übergreifende Lesen von Änderungen mit einem Leasemechanismus. Wenn Sie zwei Clients starten, die die Change Feed Processor-Bibliothek verwenden, teilen sie die Arbeit untereinander auf, wie die folgende Abbildung zeigt. Wenn Sie die Zahl der Clients erhöhen, teilen sie weiterhin die Arbeit untereinander auf.

![Verteilte Verarbeitung des Azure Cosmos DB-Änderungsfeeds](./media/change-feed/change-feed-output.png)

Der linke Client wurde zuerst gestartet und begann mit der Überwachung aller Partitionen, dann wurde der zweite Client gestartet, und der erste Client hat dem zweiten einige der Leases überlassen. Wie Sie sehen, ist dies eine gute Möglichkeit, die Arbeit zwischen verschiedenen Computern und Clients zu verteilen.

Beachten Sie: Wenn zwei serverlose Azure-Funktionen dieselbe Sammlung überwachen und die gleiche Lease verwenden, können diese zwei Funktionen möglicherweise unterschiedliche Dokumente erhalten, je nachdem, wie die Prozessorbibliothek über die Verarbeitung der Partitionen entscheidet.

### <a name="understanding-the-change-feed-processor-library"></a>Grundlegendes zur Change Feed Processor-Bibliothek

Es gibt vier Hauptkomponenten der Implementierung von Change Feed Processor: die überwachte Sammlung, die Lease-Sammlung, den Prozessor-Host und die Consumer. 

> [!WARNING]
> Das Erstellen einer Sammlung hat Auswirkungen auf die Preise, da Sie Durchsatz für die Anwendung zur Kommunikation mit Azure Cosmos DB reservieren. Weitere Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

**Überwachte Sammlung:** Die überwachte Sammlung sind die Daten, aus denen der Änderungsfeed generiert wird. Alle Einfügungen in die und Änderungen an der überwachten Sammlung werden in den Änderungsfeeds der Sammlung wiedergegeben. 

**Lease-Sammlung:** Die Lease-Sammlung koordiniert das Verarbeiten der Änderungsfeeds über mehrere Workerinstanzen. Eine separate Sammlung wird verwendet, um die Leases mit einer Lease pro Partition zu speichern. Es empfiehlt sich, diese Lease-Sammlung auf einem anderen Konto zu speichern, mit der Schreibregion näher an dem Ort, an dem Change Feed Processor ausgeführt wird. Ein Lease-Objekt beinhaltet die folgenden Attribute: 
* Besitzer: Gibt den Host an, der die Lease besitzt.
* Fortsetzung: Gibt die Position (Fortsetzungstoken) im Änderungsfeed für eine bestimmte Partition an.
* Zeitstempel: Zum Zeitpunkt der letzten Aktualisierung der Lease kann der Zeitstempel genutzt werden, um zu überprüfen, ob die Lease als abgelaufen betrachtet wird. 

**Prozessor-Host:** Jeder Host bestimmt, wie viele Partitionen verarbeitet werden, je nachdem, wie viele andere Instanzen des Hosts aktive Leases aufweisen. 
1.  Wenn ein Host gestartet wird, erhält er Leases, um die Workloads gleichmäßig auf alle Hosts zu verteilen. Ein Host erneuert in regelmäßigen Abständen Leases, damit die Leases aktiv bleiben. 
2.  Ein Host überprüft das letzte Fortsetzungstoken auf seiner Lease für jeden Lesezugriff. Um Parallelitätssicherheit zu gewährleisten, überprüft ein Host den ETag für jedes Lease-Update. Andere Prüfpunkt-Strategien werden ebenfalls unterstützt.  
3.  Nach dem Herunterfahren gibt ein Host alle Leases frei, speichert jedoch die Fortsetzungsinformationen, sodass das Lesen aus dem gespeicherten Prüfpunkt später fortgesetzt werden kann. 

Zu diesem Zeitpunkt darf die Anzahl der Hosts nicht die Anzahl der Partitionen (Leases) überschreiten.

**Consumer:** Consumer oder Worker sind Threads, welche die von jedem Host initiierte Change Feed-Verarbeitung ausführen. Jeder Prozessor-Host kann mehrere Consumer besitzen. Jeder Consumer liest den Änderungsfeed aus der Partition, der er zugewiesen wurde, und benachrichtigt seinen Host bei Änderungen und abgelaufenen Leases.

Um besser zu verstehen, wie diese vier Elemente von Change Feed Processor zusammenarbeiten, sehen wir uns ein Beispiel in der folgenden Abbildung an. Die überwachte Sammlung speichert Dokumente und verwendet „City“ als Partitionsschlüssel. Sie sehen, dass die blaue Partition Dokumente mit dem Feld „City“ aus „A-E“ usw. enthält. Es gibt zwei Hosts mit jeweils zwei Consumern, die aus den vier Partitionen gleichzeitig lesen. Die Pfeile zeigen die Consumer beim Lesen aus einer bestimmten Stelle im Änderungsfeed. In der ersten Partition stellt das dunklere Blau ungelesene Änderungen dar, während das Hellblau die bereits gelesenen Änderungen im Änderungsfeed darstellt. Die Hosts verwenden die Lease-Sammlung zum Speichern eines „Fortsetzungs“-Werts der aktuellen Leseposition für jeden Consumer. 

![Verwenden des Change Feed Processor-Hosts von Azure Cosmos DB](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Arbeiten mit der Change Feed Processor-Bibliothek

Installieren Sie vor der Installation des Change Feed Processor-NuGet-Pakets Folgendes: 

* Microsoft Azure DocumentDB, Version 1.13.1 oder höher 
* Newtonsoft.Json, Version 9.0.1 oder höher

Installieren Sie dann das [Microsoft.Azure.DocumentDB.ChangeFeedProcessor-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/), und fügen Sie es als Verweis hinzu.

So implementieren Sie die Change Feed Processor-Bibliothek:

1. Implementieren Sie ein **DocumentFeedObserver**-Objekt, das **IChangeFeedObserver** implementiert.

2. Implementieren Sie ein **DocumentFeedObserverFactory**-Objekt, das **IChangeFeedObserverFactory** implementiert.

3. Instanziieren Sie in der **CreateObserver**-Methode von **DocumentFeedObserverFacory** den **ChangeFeedObserver**, den Sie in Schritt 1 erstellt haben, und geben Sie ihn zurück.

    ```
    public IChangeFeedObserver CreateObserver()
    {
              DocumentFeedObserver newObserver = new DocumentFeedObserver(this.client, this.collectionInfo);
              return newObserver;
    }
    ```

4. Instanziieren Sie **DocumentObserverFactory**.

5. Instanziieren Sie einen **ChangeFeedEventHost**:

    ```csharp
    ChangeFeedEventHost host = new ChangeFeedEventHost(
                     hostName,
                     documentCollectionLocation,
                     leaseCollectionLocation,
                     feedOptions,
                     feedHostOptions);
    ```

6. Registrieren Sie die **DocumentFeedObserverFactory** bei dem Host.

Der Code für die Schritte 4 bis 6 ist: 

```
ChangeFeedOptions feedOptions = new ChangeFeedOptions();
feedOptions.StartFromBeginning = true;

ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();
 
// Customizing lease renewal interval to 15 seconds.
// Can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay
feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
 
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
{
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);
        await host.RegisterObserverFactoryAsync(docObserverFactory);
        await host.UnregisterObserversAsync();
}
```

Das ist alles. Nach diesen wenigen Schritten gehen Dokumente in die **DocumentFeedObserver ProcessChangesAsync**-Methode ein.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure Cosmos DB mit Azure Functions finden Sie unter [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](serverless-computing-database.md).

Weitere Informationen zur Verwendung der Change Feed Processor-Bibliothek bieten Ihnen folgende Ressourcen:

* [DocumentDB .NET Change Feed Processor-SDK: Download und Anmerkungen zur Version](documentdb-sdk-dotnet-changefeed.md) 
* [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Beispielcode zu den obigen Schritten 1 bis 6](https://github.com/rsarosh/Cosmos-ChangeFeedProcessor)
* [Zusätzliche Beispiele auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Weitere Informationen zur Verwendung des Änderungfeeds mittels SDK bieten Ihnen folgende Ressourcen:

* [Azure Cosmos DB .NET SDK: Download und Anmerkungen zum Release](documentdb-sdk-dotnet.md)

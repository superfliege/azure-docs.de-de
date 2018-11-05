---
title: Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB | Microsoft-Dokumentation
description: Verwenden Sie die Azure Cosmos DB-Unterstützung von Änderungsfeeds, um Änderungen in Dokumenten nachzuverfolgen, eine ereignisbasierte Verarbeitung (wie mit Triggern) durchzuführen und Caches und Analysesysteme auf dem neuesten Stand zu halten.
keywords: Änderungsfeed
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 09f827e8784fe2a97c587524d70baf76ae4458ba
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741860"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB

[Azure Cosmos DB](../cosmos-db/introduction.md) ist eine schnelle und flexible global replizierte Datenbank, die sich ideal für IoT-, Spiele-, Verkaufs- und Vorgangsprotokollierungsanwendungen eignet. Ein allgemeines Entwurfsmuster in diesen Anwendungen ist die Verwendung von Datenänderungen, um zusätzliche Aktionen zu starten. Diese zusätzlichen Aktionen können z.B. folgende sein: 

* Auslösen einer Benachrichtigung oder eines Aufrufs einer API, wenn ein Dokument eingefügt oder geändert wird.
* Datenstromverarbeitung für IoT oder das Ausführen von Analysen.
* Zusätzliche Datenverschiebung durch Synchronisieren mit einem Cache, einer Suchmaschine oder einem Datawarehouse, oder Archivieren von Daten zu Cold Storage.

Die **Unterstützung von Änderungsfeeds** in Azure Cosmos DB ermöglicht Ihnen das Erstellen effizienter und skalierbarer Lösungen für jedes dieser Muster, wie in der folgenden Abbildung gezeigt:

![Verwenden des Azure Cosmos DB-Änderungsfeeds für Echtzeitanalysen und ereignisgesteuerte Computingszenarien](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Unterstützung von Änderungsfeeds wird für alle Datenmodelle und Container in Azure Cosmos DB bereitgestellt. Der Änderungsfeed wird allerdings mithilfe des SQL-Clients gelesen und serialisiert Elemente in das JSON-Format. Aufgrund der JSON-Formatierung tritt bei MongoDB-Clients ein Konflikt zwischen BSON-formatierten Dokumenten und dem JSON-formatierten Änderungsfeed auf.

## <a name="how-does-change-feed-work"></a>Wie funktioniert der Änderungsfeed?

Zur Unterstützung des Änderungsfeeds in Azure Cosmos DB wird gelauscht, ob in einer Azure Cosmos DB-Sammlung Änderungen auftreten. Anschließend wird die sortierte Liste von geänderten Dokumenten in der Reihenfolge ausgegeben, in der sie geändert wurden. Die Änderungen bleiben erhalten und können asynchron und inkrementell verarbeitet werden, und die Ausgabe kann über einen oder mehrere Consumer für eine parallele Verarbeitung verteilt werden. 

Sie können den Änderungsfeed auf drei verschiedene Arten lesen, wie weiter unten in diesem Artikel erläutert:

*   [Verwenden von Azure Functions](#azure-functions)
*   [Arbeiten mit der REST-API und den DocumentDB SDKs](#sql-sdk)
*   [Verwenden der Change Feed Processor-Bibliothek von Azure Cosmos DB](#change-feed-processor)

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
* Mithilfe von „ChangeFeedOptions.StartTime“ können Sie einen anfänglichen Startpunkt angeben, um das Fortsetzungstoken zu suchen, das der angegebenen Uhrzeit entspricht. Der ContinuationToken-Wert, sofern angegeben, hat Vorrang vor den Werten „StartTime“ und „StartFromBeginning“. Die Genauigkeit von „ChangeFeedOptions.StartTime“ beträgt ca. 5 Sekunden. 

## <a name="use-cases-and-scenarios"></a>Anwendungsfälle und -szenarien

Der Änderungsfeed ermöglicht eine effiziente Verarbeitung großer Datasets mit vielen Schreibvorgängen und bietet eine Alternative zum Abfragen des gesamten Datasets, um Änderungen zu identifizieren. 

Sie können mit einem Änderungsfeed beispielsweise die folgenden Aufgaben effizient ausführen:

* Aktualisieren eines Caches, eines Suchindex oder eines Data Warehouse mit in Azure Cosmos DB gespeicherten Daten.
* Implementieren von Datentiering und -archivierung auf Anwendungsebene, d. h. Speichern von „aktiven Daten“ in Azure Cosmos DB und Auslagern von „inaktiven Daten“ in [Azure Blob Storage](../storage/common/storage-introduction.md) oder [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Ausführen von Migrationen zu einem anderen Azure Cosmos DB-Konto mit einem anderen Partitionierungsschema ohne Ausfallzeit.
* Implementieren von [Lambda-Pipelines in Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) mit Azure Cosmos DB. Azure Cosmos DB bietet eine skalierbare Datenbanklösung, die die Erfassung und Abfrage verarbeiten kann, und implementiert Lambda-Architekturen mit geringen Gesamtkosten. 
* Empfangen und Speichern von Ereignisdaten von Geräten, Sensoren, Infrastrukturen und Anwendungen und Verarbeiten dieser Ereignisse in Echtzeit mit [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md) oder [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

Die folgende Abbildung zeigt, wie Lambda-Pipelines, die sowohl aufnehmen als auch abfragen, mithilfe von Azure Cosmos DB die Unterstützung von Änderungsfeeds nutzen können: 

![Azure Cosmos DB-basierte Lambda-Pipeline für die Erfassung und Abfrage](./media/change-feed/lambda.png)

In Ihren [serverlosen](http://azure.com/serverless) Web- und mobilen Apps können Sie auch Ereignisse wie Änderungen am Profil, an den Voreinstellungen oder am Speicherort des Kunden nachverfolgen, um bestimmte Aktionen wie das Senden von Pushbenachrichtigungen an deren Geräte mit [Azure Functions](#azure-functions) auszulösen. Wenn Sie Azure Cosmos DB zum Erstellen eines Spiels verwenden, können Sie den Änderungsfeed beispielsweise verwenden, um in Echtzeit Bestenlisten anhand der Ergebnisse von abgeschlossenen Spiele zu implementieren.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Verwenden von Azure Functions 

Wenn Sie Azure Functions verwenden, ist die einfachste Möglichkeit zum Herstellen einer Verbindung mit einem Azure Cosmos DB-Änderungsfeed das Hinzufügen eines Azure Cosmos DB-Triggers zu Ihrer Azure Functions-App. Wenn Sie einen Azure Cosmos DB-Trigger in einer Azure Functions-App erstellen, wählen Sie die Azure Cosmos DB-Sammlung aus, mit der eine Verbindung hergestellt werden soll, und die Funktion wird immer dann ausgelöst, wenn eine Änderung der Sammlung erfolgt ist. 

Trigger können im Azure Functions-Portal, Azure Cosmos DB-Portal oder programmgesteuert erstellt werden. Weitere Informationen finden Sie unter [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>Verwenden des SDK

Mit dem [SQL SDK](sql-api-sdk-dotnet.md) für Azure Cosmos DB können Sie einen Änderungsfeed uneingeschränkt lesen und verwalten. Doch mit zunehmendem Leistungspotenzial wächst auch die Verantwortung. Wenn Sie Prüfpunkte verwalten, mit Dokumentsequenznummern umgehen und präzise Kontrolle über Partitionsschlüssel haben möchten, ist der Einsatz des SDK wohl der richtige Ansatz.

Dieser Abschnitt führt Sie durch das Arbeiten mit einem Änderungsfeed mithilfe des SQL SDK.

1. Beginnen Sie, indem Sie die folgenden Ressourcen aus appconfig lesen. Anweisungen zum Abrufen des Endpunkts und Autorisierungsschlüssels finden Sie unter [Aktualisieren der Verbindungszeichenfolge](create-sql-api-dotnet.md#update-your-connection-string).

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

> [!NOTE]
> Anstelle von `ChangeFeedOptions.PartitionKeyRangeId` können Sie `ChangeFeedOptions.PartitionKey` verwenden, um einen einzelnen Partitionsschlüssel anzugeben, für den Sie einen Änderungsfeed erhalten. Beispiel: `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Wenn Sie über mehrere Reader verfügen, können Sie mit **ChangeFeedOptions** die Leselast auf verschiedene Threads oder Clients verteilen.

Das war schon alles, und mit diesen wenigen Codezeilen können Sie beginnen, den Änderungsfeed zu lesen. Sie erhalten den vollständigen in diesem Artikel verwendeten Code im [GitHub-Repository](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

Im Code von Schritt 4 oben enthält **ResponseContinuation** in der letzten Zeile die letzte logische Sequenznummer (Logical Sequence Number, LSN) des Dokuments, die Sie verwenden werden, wenn Sie das nächste Mal nach dieser Sequenznummer neue Dokumente lesen. Mithilfe von **StartTime** von **ChangeFeedOption** können Sie den Zeitraum erweitern, um die Dokumente zu erhalten. Wenn also **ResponseContinuation** NULL ist, Ihre **StartTime** jedoch zeitlich zurückliegt, erhalten Sie alle Dokumente, die seit **StartTime** geändert wurden. Wenn aber **ResponseContinuation** einen Wert enthält, erhalten Sie vom System alle Dokumente ab dieser LSN.

So verwaltet Ihr Prüfpunktarray die LSN für jede Partition. Aber wenn Sie nicht mit Partitionen, Prüfpunkten, LSN, Startzeit usw. arbeiten möchten, steht Ihnen als einfachere Option die Verwendung der Change Feed Processor-Bibliothek zur Verfügung.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Verwenden der Change Feed Processor-Bibliothek 

Mit der [Change Feed Processor-Bibliothek von Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) können Sie die Ereignisverarbeitung mühelos auf mehrere Consumer verteilen. Diese Bibliothek vereinfacht das Partitionen übergreifende Lesen von Änderungen und die Parallelverarbeitung mehrerer Threads.

Der wichtigste Vorteil der Change Feed Processor-Bibliothek ist, dass Sie nicht jede Partition und jedes Fortsetzungstoken verwalten und nicht jede Sammlung manuell abrufen müssen.

Die Change Feed Processor-Bibliothek vereinfacht das Partitionen übergreifende Lesen von Änderungen und die Parallelverarbeitung mehrerer Threads.  Sie verwaltet automatisch das Partitionen übergreifende Lesen von Änderungen mit einem Leasemechanismus. Wenn Sie zwei Clients starten, die die Change Feed Processor-Bibliothek verwenden, teilen sie die Arbeit untereinander auf, wie die folgende Abbildung zeigt. Wenn Sie die Zahl der Clients erhöhen, teilen sie weiterhin die Arbeit untereinander auf.

![Verteilte Verarbeitung des Azure Cosmos DB-Änderungsfeeds](./media/change-feed/change-feed-output.png)

Der linke Client wurde zuerst gestartet und begann mit der Überwachung aller Partitionen, dann wurde der zweite Client gestartet, und der erste Client hat dem zweiten einige der Leases überlassen. Wie Sie sehen, ist dies eine gute Möglichkeit, die Arbeit zwischen verschiedenen Computern und Clients zu verteilen.

Beachten Sie: Wenn zwei serverlose Azure-Funktionen dieselbe Sammlung überwachen und die gleiche Lease verwenden, können diese zwei Funktionen möglicherweise unterschiedliche Dokumente erhalten, je nachdem, wie die Prozessorbibliothek über die Verarbeitung der Partitionen entscheidet.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Grundlegendes zur Change Feed Processor-Bibliothek

Es gibt vier Hauptkomponenten der Implementierung der Change Feed Processor-Bibliothek: die überwachte Sammlung, die Lease-Sammlung, den Prozessor-Host und die Consumer. 

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

* Microsoft.Azure.DocumentDB, aktuelle Version.
* Newtonsoft.Json, aktuelle Version

Installieren Sie dann das [Microsoft.Azure.DocumentDB.ChangeFeedProcessor-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/), und fügen Sie es als Verweis hinzu.

So implementieren Sie die Change Feed Processor-Bibliothek:

1. Implementieren Sie ein **DocumentFeedObserver**-Objekt, das **IChangeFeedObserver** implementiert.
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;
    using Microsoft.Azure.Documents.Client;

    /// <summary>
    /// This class implements the IChangeFeedObserver interface and is used to observe 
    /// changes on change feed. ChangeFeedEventHost will create as many instances of 
    /// this class as needed. 
    /// </summary>
    public class DocumentFeedObserver : IChangeFeedObserver
    {
    private static int totalDocs = 0;

        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserver" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        /// <param name="client"> Client connected to destination collection </param>
        /// <param name="destCollInfo"> Destination collection information </param>
        public DocumentFeedObserver()
        {
            
        }

        /// <summary>
        /// Called when change feed observer is opened; 
        /// this function prints out observer partition key id. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task OpenAsync(IChangeFeedObserverContext context)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Observer opened for partition Key Range: {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }

        /// <summary>
        /// Called when change feed observer is closed; 
        /// this function prints out observer partition key id and reason for shut down. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <param name="reason">Specifies the reason the observer is closed.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task CloseAsync(IChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("Observer closed, {0}", context.PartitionKeyRangeId);
            Console.WriteLine("Reason for shutdown, {0}", reason);
            return Task.CompletedTask;
        }

        public Task ProcessChangesAsync(IChangeFeedObserverContext context, IReadOnlyList<Document> docs, CancellationToken cancellationToken)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Change feed: PartitionId {0} total {1} doc(s)", context.PartitionKeyRangeId, Interlocked.Add(ref totalDocs, docs.Count));
            foreach (Document doc in docs)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(doc.Id.ToString());
            }

            return Task.CompletedTask;
        }
    }
    ```

2. Implementieren Sie ein **DocumentFeedObserverFactory**-Objekt, das **IChangeFeedObserverFactory** implementiert.
    ```csharp
     using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;

    /// <summary>
    /// Factory class to create instance of document feed observer. 
    /// </summary>
    public class DocumentFeedObserverFactory : IChangeFeedObserverFactory
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserverFactory" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        public DocumentFeedObserverFactory()
        {
        }

        /// <summary>
        /// Creates document observer instance with client and destination collection information
        /// </summary>
        /// <returns>DocumentFeedObserver with client and destination collection information</returns>
        public IChangeFeedObserver CreateObserver()
        {
            DocumentFeedObserver newObserver = new DocumentFeedObserver();
            return newObserver as IChangeFeedObserver;
        }
    }
    ```

3. Definieren Sie *CancellationTokenSource* und *ChangeFeedProcessorBuilder*.

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. Erstellen Sie **ChangeFeedProcessorBuilder** nach dem Definieren der relevanten Objekte. 

    ```csharp
            string hostName = Guid.NewGuid().ToString();
      
            // monitored collection info 
            DocumentCollectionInfo documentCollectionInfo = new DocumentCollectionInfo
            {
                Uri = new Uri(this.monitoredUri),
                MasterKey = this.monitoredSecretKey,
                DatabaseName = this.monitoredDbName,
                CollectionName = this.monitoredCollectionName
            };
            
            DocumentCollectionInfo leaseCollectionInfo = new DocumentCollectionInfo
                {
                    Uri = new Uri(this.leaseUri),
                    MasterKey = this.leaseSecretKey,
                    DatabaseName = this.leaseDbName,
                    CollectionName = this.leaseCollectionName
                };
            DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory();
       
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
            feedProcessorOptions.StartFromBeginning = true;

            this.builder
                .WithHostName(hostName)
                .WithFeedCollection(documentCollectionInfo)
                .WithLeaseCollection(leaseCollectionInfo)
                .WithProcessorOptions (feedProcessorOptions)
                .WithObserverFactory(new DocumentFeedObserverFactory());               
                //.WithObserver<DocumentFeedObserver>();  If no factory then just pass an observer

            var result =  await this.builder.BuildAsync();
            await result.StartAsync();
            Console.Read();
            await result.StopAsync();    
    ```

Das ist alles. Nach diesen wenigen Schritten werden Dokumente in der **DocumentFeedObserver ProcessChangesAsync**-Methode angezeigt.

Der oben angegebene Code dient der Veranschaulichung, um verschiedene Arten von Objekten und deren Interaktion zu zeigen. Sie müssen geeignete Variablen definieren und mit richtigen Werten initiieren. Sie erhalten den vollständigen in diesem Artikel verwendeten Code im [GitHub-Repository](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> Es sollte niemals ein Hauptschlüssel in Ihrem Code oder in der Konfigurationsdatei enthalten sein, wie es im Code oben gezeigt ist. Sehen Sie sich die Informationen zum [Verwenden von Key Vault zum Abrufen der Schlüssel](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/) an.


## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Auf welche verschiedenen Weisen kann ein Änderungsfeed gelesen werden? Und wann sind die einzelnen Methoden geeignet?

Es bestehen drei Optionen zum Lesen von Änderungsfeeds:

* **[Mithilfe des Azure Cosmos DB-SQL-API-.NET-SDKs](#sql-sdk)**
   
   Mit dieser Methode erhalten Sie ein geringes Maß an Kontrolle über den Änderungsfeed. Sie können den Prüfpunkt verwalten, auf einen bestimmten Partitionsschlüssel zugreifen usw. Wenn Sie über mehrere Reader verfügen, können Sie mit [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) die Leselast auf verschiedene Threads oder Clients verteilen. .

* **[Mithilfe der Change Feed Processor-Bibliothek von Azure Cosmos DB](#change-feed-processor)**

   Wenn Sie einen großen Teil der Komplexität von Änderungsfeeds auslagern möchten, können Sie die Change Feed Processor-Bibliothek verwenden. Diese Bibliothek blendet einen großen Teil der Komplexität aus, gibt Ihnen aber trotzdem volle Kontrolle über den Änderungsfeed. Diese Bibliothek folgt einem [Beobachtermuster](https://en.wikipedia.org/wiki/Observer_pattern), Ihre Verarbeitungsfunktion wird vom SDK aufgerufen. 

   Wenn Ihr Änderungsfeed einen hohen Durchsatz aufweist, können Sie zum Lesen des Änderungsfeeds mehrere Clients instanziieren. Da Sie die „Change Feed Processor-Bibliothek“ verwenden, wird die Last automatisch zwischen den verschiedenen Clients aufgeteilt. Sie brauchen gar nichts zu tun. Die gesamte Komplexität wird vom SDK behandelt. Wenn Sie aber ein eigenes Lastenausgleichsmodul verwenden möchten, können Sie IParitionLoadBalancingStrategy implementieren und eine benutzerdefinierte Partitionsstrategie verwenden. Implementieren von IPartitionProcessor – für benutzerdefinierte Verarbeitungsänderungen in einer Partition. Mit SDK können Sie zwar einen Partitionsbereich verarbeiten, aber wenn Sie einen bestimmten Partitionsschlüssel verarbeiten möchten, müssen Sie die SDK für SQL-API verwenden.

* **[Mithilfe von Azure Functions](#azure-functions)** 
   
   Die letzte Option, Azure Functions, ist die einfachste. Wir empfehlen die Verwendung dieser Option. Wenn Sie einen Azure Cosmos DB-Trigger in einer Azure Functions-App erstellen, wählen Sie die Azure Cosmos DB-Sammlung aus, mit der eine Verbindung hergestellt werden soll, und die Funktion wird immer dann ausgelöst, wenn eine Änderung der Sammlung erfolgt ist. Einen [Screencast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) zur Verwendung von Azure Functions mit Änderungsfeeds anzeigen

   Trigger können im Azure Functions-Portal, Azure Cosmos DB-Portal oder programmgesteuert erstellt werden. Visual Studio und VS Code bieten hervorragende Unterstützung zum Schreiben von Azure-Funktionen. Sie können den Code auf Ihrem Desktop schreiben und debuggen und dann die Funktion mit einem Klick bereitstellen. Weitere Informationen finden Sie im Artikel [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](serverless-computing-database.md).

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>Welche Sortierreihenfolge gilt für Dokumente im Änderungsfeed?

Dokumente im Änderungsfeed treten in der Reihenfolge des Zeitpunkts ihrer letzten Änderung auf. Diese Sortierreihenfolge ist nur pro Partition garantiert.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Was geschieht bei einem Konto, das mehrere Regionen überspannt, mit dem Änderungsfeed, wenn ein Failover der schreibenden Region eintritt? Tritt dann auch für den Änderungsfeed ein Failover ein? Wäre der Änderungsfeed immer noch zusammenhängend, oder würde der Failover zu einem Zurücksetzen des Änderungsfeeds führen?

Ja, der Änderungsfeed funktioniert übergreifend über den manuellen Failovervorgang hinweg und bleibt zusammenhängend.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Wie lange bleiben die geänderten Daten im Änderungsfeed erhalten, wenn ich die TTL-Eigenschaft (Time to Live) für das Dokument auf -1 festlege?

Der Änderungsfeed bleibt dauerhaft erhalten, ohne zeitliche Beschränkung. Wenn Daten nicht gelöscht werden, verbleiben sie im Änderungsfeed.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Wie kann ich Azure-Funktionen zum Lesen aus einer bestimmten Region konfigurieren, da der Änderungsfeed standardmäßig in allen Leseregionen verfügbar ist?

Derzeit ist es nicht möglich, Azure Functions für das Lesen aus einer bestimmten Region zu konfigurieren. Es besteht ein GitHub-Problem im Azure Functions-Repository beim Festlegen der bevorzugten Regionen von Azure Cosmos DB-Bindungen und -Triggern.

Azure Functions verwendet die Standard-Verbindungsrichtlinie. Sie können den Verbindungsmodus in Azure Functions konfigurieren, und standardmäßig erfolgt das Lesen aus der Schreibregion, daher sollte Azure Functions auch in der gleichen Region platziert werden.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Was ist die Standardgröße von Batches in Azure Functions?

100 Dokumente bei jedem Aufruf von Azure Functions. Diese Zahl kann jedoch in der JSON-Datei der Funktion konfiguriert werden. Hier finden Sie die vollständige [Liste der Konfigurationsoptionen](../azure-functions/functions-run-local.md). Wenn Sie lokal entwickeln, aktualisieren Sie die Anwendungseinstellungen in der Datei [local.settings.json](../azure-functions/functions-run-local.md).

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Ich überwache eine Sammlung und lese ihren Änderungsfeed, ich stelle aber fest, dass nicht alle eingefügten Dokumente enthalten sind, einige Dokumente fehlen. Was geht da vor?

Vergewissern sie sich, dass keine andere Funktion die gleiche Sammlung mit der gleichen Lease-Sammlung liest. Das ist mir passiert, und erst später habe ich festgestellt, dass die fehlenden Dokumente von meinen anderen Azure-Funktionen verarbeitet wurden, die ebenfalls die gleiche Lease-Dauer verwenden.

Wenn Sie mehrere Azure-Funktionen zum Lesen des gleichen Änderungsfeeds erstellen, müssen sie verschiedene Leasesammlungen oder die leasePrefix-Konfiguration verwenden, wenn sie die gleiche Sammlung freigeben möchten. Beim Einsatz der Change Feed Processor-Bibliothek können Sie allerdings mehrere Instanzen Ihrer Funktion starten, und das SDK übernimmt automatisch die Aufteilung der Dokumente auf die verschiedenen Instanzen.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Mein Dokument wird jede Sekunde aktualisiert, ich erhalte aber beim Lauschen am Änderungsfeed nicht alle Änderungen in Azure Functions.

Azure Functions fragt den Änderungsfeed alle 5 Sekunden ab, daher gehen alle Änderungen verloren, die zwischen den 5-Sekunden-Intervallen liegen. Azure Cosmos DB speichert alle 5 Sekunden nur eine Version, Sie erhalten also die fünfte Änderung am Dokument. Wenn Sie allerdings unter 5 Sekunden bleiben und den Änderungsfeed jede Sekunde abfragen möchten, können Sie die Abrufzeit „feedPollDelay“ konfigurieren. Weitere Informationen dazu finden Sie unter [Azure Cosmos DB-Bindungen](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Sie ist in Millisekunden mit dem Standardwert 5000 definiert. Intervalle unterhalb einer Sekunde sind möglich, aber nicht ratsam, da die CPU-Belastung dadurch deutlich ansteigt.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Ich habe ein Dokument in die Mongo-API-Sammlung eingefügt, aber wenn ich das Dokument im Änderungsfeed abrufe, hat es einen anderen ID-Wert. Was ist hier schiefgelaufen?

Ihre Sammlung ist die Mongo API-Sammlung. Beachten Sie aber, dass der Änderungsfeed mithilfe des SQL-Clients gelesen wird und Elemente in das JSON-Format serialisiert. Aufgrund der JSON-Formatierung tritt bei MongoDB-Clients ein Konflikt zwischen BSON-formatierten Dokumenten und dem JSON-formatierten Änderungsfeed auf. Was Sie sehen, ist die Darstellung eines BSON-Dokuments in JSON. Wenn Sie in einem Mongo-Konto binäre Attribute verwenden, werden sie nach JSON konvertiert.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Gibt es eine Möglichkeit, Änderungsfeeds nur für Aktualisierungen zu steuern, nicht aber für Einfügungen?

Bisher noch nicht, aber diese Funktionalität ist in Planung. Heute können Sie Dokumenten für Aktualisierungen eine schwache Markierung hinzufügen.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Gibt es eine Möglichkeit, Löschvorgänge in Änderungsfeeds aufzunehmen?

Aktuell protokollieren Änderungsfeeds keine Löschvorgänge. Änderungsfeeds werden fortlaufend verbessert, und diese Funktionalität ist in Planung. Heute können Sie einem Dokument eine schwache Markierung für einen Löschvorgang hinzufügen. Fügen Sie dem Dokument ein Attribut „deleted“ (gelöscht) hinzu, legen Sie es auf TRUE fest, und legen Sie außerdem eine TTL für das Dokument fest, damit es automatisch gelöscht werden kann.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Kann ich den Änderungsfeed für historische Dokumente lesen (z. B. Dokumente, die vor fünf Jahren hinzugefügt wurden)?

Ja, wenn das Dokument nicht gelöscht wurde, können Sie den Änderungsfeed bis zurück zum Ursprung Ihrer Sammlung lesen.

### <a name="can-i-read-change-feed-using-javascript"></a>Kann ich den Änderungsfeed mithilfe von JavaScript lesen?

Ja, die Node.js-SDK-Anfangsunterstützung für Änderungsfeeds wurde vor Kurzem hinzugefügt. Sie kann wie im folgenden Beispiel gezeigt verwendet werden; bitte aktualisieren Sie das documentdb-Modul auf die aktuelle Version, bevor Sie den Code ausführen:

```js

var DocumentDBClient = require('documentdb').DocumentClient;
const host = "https://your_host:443/";
const masterKey = "your_master_key==";
const databaseId = "db";
const collectionId = "c1";
const dbLink = 'dbs/' + databaseId;
const collLink = dbLink + '/colls/' + collectionId;
var client = new DocumentDBClient(host, { masterKey: masterKey });
let options = {
    a_im: "Incremental feed",
    accessCondition: {
        type: "IfNoneMatch",        // Use: - empty condition (or remove accessCondition entirely) to start from beginning.
        //      - '*' to start from current.
        //      - specific etag value to start from specific continuation.
        condition: ""
    }
};
 
var query = client.readDocuments(collLink, options);
query.executeNext((err, results, headers) =&gt; {
    // Now we have headers.etag, which can be used in next readDocuments in accessCondition option.
    console.log(results);
    console.log(headers.etag);
    console.log(results.length);
    options.accessCondition = { type: "IfNoneMatch", condition: headers.etag };
    var query = client.readDocuments(collLink, options);
    query.executeNext((err, results, headers) =&gt; {
        console.log("next one:", results[0]);
    });
});<span id="mce_SELREST_start" style="overflow:hidden;line-height:0;"></span>

```

### <a name="can-i-read-change-feed-using-java"></a>Kann ich den Änderungsfeed mithilfe von Java lesen?

Die Java-Bibliothek zum Lesen von Änderungsfeeds steht im [Github-Repository](https://github.com/Azure/azure-documentdb-changefeedprocessor-java) zur Verfügung. Aktuell liegt die Java-Bibliothek jedoch um einige Versionen hinter der .NET-Bibliothek zurück. Bald werden beide Bibliotheken jedoch synchron sein.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Kann ich „_etag“, „_lsn“ oder „_ts“, die ich als Antwort erhalte, für die interne Buchhaltung verwenden?

Das _etag-Format ist intern, und Sie sollten nicht darauf aufbauen (es nicht analysieren), da es jederzeit geändert werden kann.
„_ts“ ist der Zeitstempel der Änderung oder Erstellung. Sie können „_ts“ für chronologische Vergleiche verwenden.
„_lsn“ ist eine Batch-ID, die nur für den Änderungsfeed hinzugefügt wird, sie stellt die Transaktions-ID aus dem Speicher dar. Viele Dokumente können den gleichen _lsn-Wert aufweisen.
Dazu ist noch anzumerken, dass ETag in der FeedResponse sich von dem „_etag“ unterscheidet, das für das Dokument angezeigt wird. „_etag“ ist ein interner Bezeichner und wird für Parallelität eingesetzt, es ist eine Information über die Dokumentversion, während „ETag“ für die Sequenzierung des Feeds verwendet wird.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Verursacht das Lesen des Änderungsfeeds irgendwelche Zusatzkosten?

Ihnen werden die genutzten RUs in Rechnung gestellt, d.h., die Datenbewegungen in und aus Azure Cosmos DB-Sammlungen verbrauchen immer RU. Benutzern werden von der Lease-Sammlung verbrauchte RUs in Rechnung gestellt.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Können mehrere Azure-Funktionen den Änderungsfeed einer Sammlung lesen?

Ja. Mehrere Azure-Funktionen können den Änderungsfeed der gleichen Sammlung lesen. Allerdings muss für die Azure-Funktionen ein separates „leaseCollectionPrefix“ definiert sein.

### <a name="should-the-lease-collection-be-partitioned"></a>Sollte die Lease-Sammlung partitioniert sein?

Nein, die Lease-Sammlung kann fest sein. Partitionierte Lease-Sammlungen sind nicht erforderlich und werden derzeit nicht unterstützt.

### <a name="can-i-read-change-feed-from-spark"></a>Kann ich den Änderungsfeed von Spark lesen?

Ja, das ist möglich. Weitere Informationen finden Sie unter [Azure Cosmos DB-Spark-Connector](spark-connector.md). Hier finden Sie einen [Screencast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s), aus dem Sie ersehen können, wie ein Änderungsfeed als strukturierter Stream verarbeitet werden kann.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Angenommen, ich verarbeite einen Änderungsfeed mithilfe von Azure Functions, z. B. einen Stapel von 10 Dokumenten, und beim 7. Dokument wird ein Fehler ausgegeben. In diesem Fall werden die letzten drei Dokumente nicht verarbeitet. Wie kann ich in meinem nächsten Feed mit der Verarbeitung ab dem Dokument mit dem Fehler (d.h. dem 7. Dokument) beginnen?

Das empfohlene Muster zum Behandeln des Fehlers besteht darin, Ihren Code in einen try-catch-Block einzuschließen. Fangen Sie den Fehler ab, und stellen Sie das Dokument in eine Warteschlange (unzustellbare Nachrichten) ein. Definieren Sie anschließend Logik zum Verarbeiten der Dokumente, die den Fehler verursacht haben. Mit dieser Methode brauchen Sie bei einem Stapel aus 200 Dokumenten, von denen nur bei einem Dokument ein Fehler aufgetreten ist, nicht den gesamten Stapel wegzuwerfen.

Im Fehlerfall sollten Sie den Prüfpunkt nicht zum Anfang zurücksetzen, andernfalls erhalten Sie erneut diese Dokumente aus dem Änderungsfeed. Beachten Sie, dass der Änderungsfeed die letzte endgültige Momentaufnahme der Dokumente aufbewahrt, und Sie so die vorhergehende Momentaufnahme des Dokuments verlieren können. Der Änderungsfeed bewahrt nur eine letzte Version des Dokuments aus, und zwischenzeitlich kann das Dokument von anderen Prozessen verändert worden sein.

Sie werden während der Problembehebung an Ihrem Code bald keine Dokumente mehr in der Warteschlange für unzustellbare Nachrichten finden.
Azure Functions wird vom Änderungsfeedsystem automatisch aufgerufen, und der Prüfpunkt usw. wird intern von Azure Functions verwaltet. Wenn Sie einen Rollback des Prüfpunkts ausführen und jeden Aspekt davon steuern möchten, sollten Sie die Verwendung des Change Feed Processor-SDKs in Erwägung ziehen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure Cosmos DB mit Azure Functions finden Sie unter [Azure Cosmos DB: Serverloses Datenbank-Computing mithilfe von Azure Functions](serverless-computing-database.md).

Weitere Informationen zur Verwendung der Change Feed Processor-Bibliothek bieten Ihnen folgende Ressourcen:

* [DocumentDB .NET Change Feed Processor-SDK: Download und Anmerkungen zur Version](sql-api-sdk-dotnet-changefeed.md) 
* [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Beispielcode zu den obigen Schritten 1 bis 6](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Zusätzliche Beispiele auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Weitere Informationen zur Verwendung des Änderungfeeds mittels SDK bieten Ihnen folgende Ressourcen:

* [Azure Cosmos DB .NET SDK: Download und Anmerkungen zum Release](sql-api-sdk-dotnet.md)

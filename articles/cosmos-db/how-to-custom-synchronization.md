---
title: Implementieren der benutzerdefinierten Synchronisierung zur Optimierung der Verfügbarkeit und Leistung in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie zur Optimierung der Verfügbarkeit und Leistung in Azure Cosmos DB die benutzerdefinierte Synchronisierung implementieren.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: 9033a7502919c8dc05053048272f3fa62f81b31d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118833"
---
# <a name="how-to-implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Implementieren der benutzerdefinierten Synchronisierung zur Optimierung der Verfügbarkeit und Leistung

In Azure Cosmos DB stehen fünf klar definierte Konsistenzebenen zur Auswahl, mit denen Sie Konsistenz, Leistung und Verfügbarkeit optimal aufeinander abstimmen können. Eine hohe Konsistenz gewährleistet, dass Daten synchron repliziert und dauerhaft in jeder Region gespeichert werden, mit der das Cosmos-Konto konfiguriert ist. Diese Konfiguration bietet zwar ein Höchstmaß an Dauerhaftigkeit, dies geht allerdings zulasten der Leistung und der Verfügbarkeit. Wenn die Dauerhaftigkeit von Daten im Einklang mit den Anforderungen der Anwendung gesteuert/gelockert werden soll, ohne die Verfügbarkeit zu beeinträchtigen, kann eine benutzerdefinierte Synchronisierung auf der Anwendungsebene verwendet werden, um das gewünschte Maß an Dauerhaftigkeit zu erreichen.

Das folgende Diagramm veranschaulicht das Modell der benutzerdefinierten Synchronisierung:

![Benutzerdefinierte Synchronisierung](./media/how-to-custom-synchronization/custom-synchronization.png)

In diesem Szenario wird ein Cosmos-Container global über mehrere Regionen und Kontinente hinweg repliziert. In diesem Fall hat die Verwendung einer hohen Konsistenz für alle Regionen keine negativen Auswirkungen auf die Leistung. Um für die Daten ein höheres Maß an Dauerhaftigkeit zu gewährleisten, ohne die Wartezeit bei Schreibvorgängen zu erhöhen, kann die Anwendung zwei Clients mit dem gleichen Sitzungstoken verwenden.

Der erste Client kann Daten in der lokalen Region (beispielsweise „USA, Westen“) schreiben. Der zweite Client (beispielsweise in der Region „USA, Osten“) ist ein Leseclient und dient zur Gewährleistung der Synchronisierung. Durch die Weitergabe des Sitzungstokens von der Schreibantwort an den nachfolgenden Lesevorgang stellt der Lesevorgang die Synchronisierung von Schreibvorgängen in der Region „USA, Osten“ sicher. Azure Cosmos DB sorgt dafür, dass Schreibvorgänge von mindestens einer Region erkannt werden und im Falle eines regionalen Ausfalls erhalten bleiben, sollte die ursprüngliche Schreibregion nicht mehr verfügbar sein. In diesem Szenario wird jeder Schreibvorgang mit „USA, Osten“ synchronisiert, was die Wartezeit bei der Nutzung von hoher Konsistenz in allen Regionen verringert. In einem Multimasterszenario mit Schreibvorgängen in jeder Region kann dieses Modell durch eine parallele Synchronisierung mit mehreren Regionen erweitert werden.

## <a name="configure-the-clients"></a>Konfigurieren der Clients

Das folgende Beispiel zeigt die Datenzugriffsebene, die zwei Clients für diesen Zweck instanziiert:

```csharp
class MyDataAccessLayer
{
    private DocumentClient writeClient;
    private DocumentClient readClient;

    public async Task InitializeAsync(Uri accountEndpoint, string key)
    {
        ConnectionPolicy writeConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp,
            UseMultipleWriteLocations = true
        };
        writeConnectionPolicy.SetCurrentLocation(LocationNames.WestUS);

        ConnectionPolicy readConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        };
        readConnectionPolicy.SetCurrentLocation(LocationNames.EastUS);

        writeClient = new DocumentClient(accountEndpoint, key, writeConnectionPolicy);
        readClient = new DocumentClient(accountEndpoint, key, readConnectionPolicy, ConsistencyLevel.Session);

        await Task.WhenAll(new Task[]
        {
            writeClient.OpenAsync(),
            readClient.OpenAsync()
        });
    }
}
```

## <a name="implement-custom-synchronization"></a>Implementieren der benutzerdefinierten Synchronisierung

Nachdem die Clients initialisiert wurden, kann die Anwendung Schreibvorgänge in der lokalen Region (USA, Westen) ausführen und die Synchronisierung der Schreibvorgänge in der Region „USA, Osten“ erzwingen:

```csharp
class MyDataAccessLayer
{
    public async Task CreateItem(string containerLink, Document document)
    {
        ResourceResponse<Document> response = await writeClient.CreateDocumentAsync(
            containerLink, document);

        await readClient.ReadDocumentAsync(response.Resource.SelfLink,
            new RequestOptions { SessionToken = response.SessionToken });
    }
}
```

Dieses Modell kann durch eine parallele Synchronisierung mit mehreren Regionen erweitert werden.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über die globale Verteilung und Konsistenz in Azure Cosmos DB zu erfahren:

* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)

* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)

* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md)

* [Partitionierung und horizontale Skalierung in Azure Cosmos DB](partition-data.md)

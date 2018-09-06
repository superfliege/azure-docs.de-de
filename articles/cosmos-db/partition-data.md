---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3cc2794105eff196c3e1db02d664a89c9b37e318
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286984"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partitionieren und Skalieren in Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist ein weltweit verteilter Datenbankdienst mit mehreren Modellen, der Ihnen helfen soll, eine schnelle und vorhersagbare Leistung zu erzielen. Er lässt sich nahtlos zusammen mit Ihrer Anwendung skalieren. Dieser Artikel bietet eine Übersicht darüber, wie die Partitionierung für alle Datenmodelle in Azure Cosmos DB funktioniert. Außerdem wird beschrieben, wie Sie Azure Cosmos DB-Container konfigurieren, um Ihre Anwendungen effektiv zu skalieren.

## <a name="partitioning-in-azure-cosmos-db"></a>Partitionierung in Azure Cosmos DB
Azure Cosmos DB stellt Container für das Speichern von Daten bereit, die als Sammlungen (für Dokumente), Graphen oder Tabellen bezeichnet werden. Container sind logische Ressourcen und können eine oder mehrere physische Partitionen bzw. einen oder mehrere Server umfassen. Die Partitionenanzahl wird von Azure Cosmos DB auf Basis der Speichergröße und des bereitgestellten Durchsatzes für einen Container oder eine Gruppe von Containern bestimmt. 


### <a name="physical-partition"></a>Physische Partition

Bei einer *physischen* Partition wird eine feste Menge an reserviertem SSD-Speicher mit einer variablen Menge an Computeressourcen (CPU und Arbeitsspeicher) kombiniert. Jede physische Partition wird repliziert, um Hochverfügbarkeit zu erzielen. Jede Gruppe von Containern kann sich eine oder mehrere physische Partitionen teilen. Die Verwaltung der physischen Partitionen erfolgt vollständig über Azure Cosmos DB, Sie müssen also weder komplexen Code schreiben noch Ihre Partitionen verwalten. Für Azure Cosmos DB-Container gelten keine Obergrenzen hinsichtlich Speicherplatz und Durchsatz. Physische Partitionen sind ein internes Konzept von Azure Cosmos DB und vorübergehender Natur. Azure Cosmos DB skaliert automatisch die Anzahl der physischen Partitionen basierend auf Ihrer Workload. Daher sollten Sie Ihren Datenbankentwurf nicht an der Anzahl der physischen Partitionen ausrichten, sondern den richtigen Partitionsschlüssel auswählen, der die logischen Partitionen bestimmt. 

### <a name="logical-partition"></a>Logische Partition

Eine *logische* Partition ist eine Partition innerhalb einer physischen Partition, die alle Daten speichert, die mit einem einzelnen Partitionsschlüsselwert verknüpft sind. Mehrere logische Partitionen können in der gleichen physischen Partition zusammengeführt werden. Im der folgenden Abbildung verfügt ein einzelner Container über drei logische Partitionen. Jede logische Partition speichert die Daten für jeweils einen Partitionsschlüssel: LAX, AMS oder MEL. Die Größe der logischen LAX-, AMS- bzw. MEL-Partitionen darf die Höchstgrenze für logische Partitionen von 10 GB nicht überschreiten. 

![Ressourcenpartitionierung](./media/introduction/azure-cosmos-db-partitioning.png) 

Wenn ein Container die [Partitionierungsvoraussetzungen](#prerequisites) erfüllt, ist die Partitionierung für Ihre Anwendung vollständig transparent. Azure Cosmos DB übernimmt die Verteilung von Daten auf physische und logische Partitionen sowie das Routing von Abfrageanforderungen an die richtige Partition. 

## <a name="how-does-partitioning-work"></a>Wie funktioniert die Partitionierung?

Jedes Dokument muss zur eindeutigen Identifizierung über einen *Partitionsschlüssel* und einen *Zeilenschlüssel* verfügen. Der Partitionsschlüssel fungiert als logische Partition für Ihre Daten und bietet Azure Cosmos DB eine natürliche Grenze für die Verteilung von Daten über physische Partitionen hinweg. **Die Daten für eine einzelne logische Partition müssen sich innerhalb einer einzelnen physischen Partition befinden, wobei die Verwaltung der physischen Partition durch Azure Cosmos DB erfolgt**. 

So funktioniert die Partitionierung in Azure Cosmos DB:

* Sie stellen eine Gruppe von Azure Cosmos DB-Containern mit einem Durchsatz von **T** RU/s (Anforderungen pro Sekunde) bereit.  
* Im Hintergrund stellt Azure Cosmos DB die physischen Partitionen bereit, die zum Verarbeiten von **T** Anforderungen pro Sekunde erforderlich sind. Wenn **T** höher ist als der maximale Durchsatz pro physischer Partition **t**, stellt Azure Cosmos DB **N = T/t** physische Partitionen bereit. Der Wert des maximalen Durchsatzes pro Partition (t) wird von Azure Cosmos DB konfiguriert. Dieser Wert wird basierend auf dem gesamten bereitgestellten Durchsatz und der verwendeten Hardwarekonfiguration zugewiesen.  
* Azure Cosmos DB verteilt den Schlüsselbereich der Partitionsschlüsselhashes gleichmäßig auf die **N** physischen Partitionen. Die Anzahl der logischen Partitionen, die jede physische Partition hostet, ist also **1/N** * Anzahl der Partitionsschlüsselwerte.  
* Wenn eine physische Partition **p** ihren Speichergrenzwert erreicht, teilt Azure Cosmos DB **p** nahtlos in zwei neue physische Partitionen (**p1** und **p2**) auf. Dabei werden die Schlüsselwerte etwa im Verhältnis 50:50 auf die neuen physischen Partitionen aufgeteilt. Dieser Aufteilungsvorgang ist für Ihre Anwendung vollkommen unsichtbar. Wenn eine physische Partition ihr Speicherlimit erreicht und alle Daten in der physischen Partition zum selben logischen Partitionsschlüssel gehören, erfolgt keine Aufteilung. Dies liegt daran, dass sich alle Daten für einen einzelnen logischen Partitionsschlüssel in derselben physischen Partition befinden müssen. In diesem Fall sollte eine andere Partitionsschlüsselstrategie verwendet werden.  
* Wenn Sie einen höheren Durchsatz als **t*N** bereitstellen, teilt Azure Cosmos DB mindestens eine Ihrer physischen Partitionen auf, um den höheren Durchsatz zu unterstützen.

Die jeweilige Semantik für Partitionsschlüssel ist je nach Semantik der verschiedenen APIs geringfügig unterschiedlich, wie in der folgenden Tabelle dargestellt:

| API | Partitionsschlüssel | Zeilenschlüssel |
| --- | --- | --- |
| SQL | Benutzerdefinierter Partitionsschlüsselpfad | `id` (feststehend) | 
| MongoDB | Benutzerdefinierter Shardschlüssel  | `_id` (feststehend) | 
| Gremlin | Benutzerdefinierte Partitionsschlüsseleigenschaft | `id` (feststehend) | 
| Table | `PartitionKey` (feststehend) | `RowKey` (feststehend) | 

Azure Cosmos DB arbeitet mit hashbasierter Partitionierung. Wenn Sie ein Element schreiben, erstellt Azure Cosmos DB einen Hashwert für den Partitionsschlüsselwert und verwendet das Hashergebnis, um zu ermitteln, in welcher Partition das Element gespeichert werden soll. 

> [!NOTE]
> Azure Cosmos DB speichert alle Elemente mit dem gleichen Partitionsschlüssel in der gleichen physischen Partition. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Bewährte Methoden bei der Auswahl eines Partitionsschlüssels

Die Auswahl des Partitionsschlüssels ist eine wichtige Entscheidung, die Sie zur Entwurfszeit treffen müssen. Wählen Sie einen Eigenschaftsnamen mit einem breiten Wertespektrum und gleichmäßigen Zugriffsmustern. Es hat sich als bewährte Methode herausgestellt, einen Partitionsschlüssel mit einer großen Anzahl unterschiedlicher Werte (z.B. Hunderte bis Tausende) zu verwenden. Damit können Sie Ihre Workload gleichmäßig auf diese Werte verteilen. Ein idealer Partitionsschlüssel ist einer, der häufig als Filter in Ihren Abfragen erscheint und genügend Kardinalität besitzt, um sicherzustellen, dass Ihre Lösung skalierbar ist.

Wenn eine physische Partition ihr Speicherlimit erreicht und die Daten in der Partition den gleichen Partitionsschlüssel aufweisen, gibt Azure Cosmos DB die Meldung *„Maximale Größe des Partitionsschlüssels von 10 GB wurde erreicht“* zurück, und die Partition wird nicht aufgeteilt. Die Auswahl eines guten Partitionsschlüssels ist eine sehr wichtige Entscheidung. 

Wählen Sie einen Partitionsschlüssel aus, der die folgenden Bedingungen erfüllt:

* Die Speicherverteilung erfolgt gleichmäßig über alle Schlüssel.  
* Die Volumeverteilung von Anforderungen zu einem bestimmten Zeitpunkt erfolgt für alle Schlüssel gleichmäßig.  

  Sie sollten überprüfen, wie Ihre Daten auf Partitionen verteilt sind. Navigieren Sie zur Überprüfung der Datenverteilung im Portal zu Ihrem Azure Cosmos DB-Konto, und klicken Sie im Abschnitt **Überwachung** auf **Metrik**. Klicken Sie anschließend auf die Registerkarte **Speicher**, und sehen Sie sich die Verteilung Ihrer Daten auf verschiedene physische Partitionen an.

  ![Ressourcenpartitionierung](./media/partition-data/partitionkey-example.png)

  Die Abbildung links oben zeigt das Ergebnis eines ungeeigneten Partitionsschlüssels und die Abbildung rechts oben das Ergebnis der Auswahl eines geeigneten Partitionsschlüssels. Sie sehen, dass in der linken Abbildung die Daten nicht gleichmäßig auf die Partitionen verteilt sind. Sie sollten sich bemühen, einen Partitionsschlüssel auszuwählen, der Ihre Daten ähnlich der rechten Abbildung verteilt.

* Abfragen, die mit hoher Parallelität aufgerufen werden, können effizient weitergeleitet werden, indem der Partitionsschlüssels in das Filterprädikat eingeschlossen wird.  
* Das Auswählen eines Partitionsschlüssels mit höherer Kardinalität wird im Allgemeinen bevorzugt (weil in der Regel eine bessere Verteilung und Skalierbarkeit erreicht wird). Beispielsweise kann ein synthetischer Schlüssel durch Verketten von Werten aus mehreren Eigenschaften generiert werden, um die Kardinalität zu erhöhen.  

Wenn Sie einen Partitionsschlüssel unter Berücksichtigung der oben aufgeführten Überlegungen auswählen, müssen Sie sich keine Gedanken über die Anzahl der Partitionen oder den zugeordneten Durchsatz pro physischer Partition machen, da Azure Cosmos DB die Anzahl der physischen Partitionen horizontal hochskaliert und auch die einzelnen Partitionen bei Bedarf skalieren kann.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Voraussetzungen für die Partitionierung

Azure Cosmos DB-Container können mit fester oder unbegrenzter Größe im Azure-Portal erstellt werden. Container mit fester Größe weisen eine Obergrenze von 10 GB und 10.000 RUs/Sek. (Request Units, Anforderungseinheiten) auf. Um einen unbegrenzten Container zu erstellen, müssen Sie einen Partitionsschlüssel und Mindestdurchsatz von 1.000 RU/s angeben. Azure Cosmos DB-Container können auch so konfiguriert werden, dass sich die Container innerhalb einer Gruppe von Containern den Durchsatz teilen. Dabei muss jeder Container einen Partitionsschlüssel angeben und kann sich unbegrenzt vergrößern. Im Folgenden finden Sie die Voraussetzungen, die bei der Partitionierung und Skalierung berücksichtigt werden müssen:

* Wenn Sie einen Container (z.B. eine Sammlung, einen Graph-Container oder eine Tabelle) im Azure-Portal erstellen, wählen Sie die Option **Unbegrenzt** für die Speicherkapazität aus, um die Vorteile einer unbegrenzten Skalierung zu nutzen. Für physische Partitionen mit automatischer Aufteilung in **p1** und **p2** (siehe [Wie funktioniert die Partitionierung?](#how-does-partitioning-work)) muss der Container mit einem Durchsatz von mindestens 1.000 RUs/s erstellt werden (oder sich den Durchsatz innerhalb einer Gruppe von Containern teilen), und es muss ein Partitionsschlüssel angegeben werden. 

* Wenn Sie im Azure-Portal oder programmgesteuert einen Container erstellt haben und der anfängliche Durchsatz 1.000 RU/s oder mehr betrug und Sie einen Partitionsschlüssel angegeben haben, können Sie die Vorteile einer unbegrenzten Skalierung ohne Änderungen an Ihrem Container nutzen. Dies schließt Container des Typs **Fest** ein, wenn der anfängliche Container mit einem Durchsatz von mindestens 1.000 RU/s erstellt wurde und ein Partitionsschlüssel angegeben wird.

* Alle Container, die für die gemeinsame Nutzung von Durchsatz innerhalb einer Containergruppe konfiguriert sind, werden als **unbegrenzte** Container behandelt.

Wenn Sie einen Container des Typs **Fest** ohne Partitionsschlüssel oder mit einem Durchsatz von weniger als 1.000 RU/s erstellt haben, wird der Container nicht automatisch skaliert. Zum Migrieren der Daten aus einem festen Container zu einem unbegrenzten Container müssen Sie das [Datenmigrationstool](import-data.md) oder die [Änderungsfeedbibliothek](change-feed.md) verwenden. 

## <a name="partitioning-and-provisioned-throughput"></a>Partitionierung und bereitgestellter Durchsatz
Azure Cosmos DB ist auf vorhersagbare Leistung ausgelegt. Wenn Sie einen Container oder eine Gruppe von Containern erstellen, reservieren Sie den Durchsatz als *[Anforderungseinheiten](request-units.md) (Request Units, RUs) pro Sekunde*. Für jede Anforderung fällt eine Gebühr für Anforderungseinheiten an, die sich proportional zur vom Vorgang genutzten Menge an Systemressourcen wie CPU, Arbeitsspeicher und E/A verhält. Der Lesevorgang eines Dokuments der Größe 1 KB mit einer Sitzungskonsistenz beansprucht eine Anforderungseinheit. Ein Lesevorgang entspricht einer RU, unabhängig von der Anzahl der gespeicherten Elemente oder der Anzahl gleichzeitiger Anforderungen, die parallel ausgeführt werden. Größere Elemente erfordern mehr Anforderungseinheiten. Wenn Sie die Größe Ihrer Entitäten und die Anzahl der Lesevorgänge kennen, die Sie für Ihre Anwendung unterstützen müssen, können Sie genau die Menge an Durchsatz bereitstellen, die für die Anforderungen Ihrer Anwendung erforderlich ist. 

> [!NOTE]
> Um den bereitgestellten Durchsatz für einen Container oder eine Gruppe von Containern vollständig zu nutzen, müssen Sie einen Partitionsschlüssel auswählen, der es Ihnen ermöglicht, Anforderungen gleichmäßig auf alle unterschiedlichen Partitionsschlüsselwerte zu verteilen.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="create-partition-key"></a>Erstellen des Partitionsschlüssels 
Sie können das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden, um Container zu erstellen und jederzeit zu skalieren. Dieser Abschnitt zeigt, wie Container erstellt und der bereitgestellte Durchsatz und der Partitionsschlüssel mithilfe der einzelnen APIs festgelegt werden.


### <a name="sql-api"></a>SQL-API
Das folgende Beispiel zeigt, wie Sie mit der SQL-API einen Container (eine Sammlung) erstellen. 

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Sie können ein Element (Dokument) mit der `GET`-Methode in der REST-API oder mithilfe von `ReadDocumentAsync` in einem der SDKs lesen.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Weitere Informationen finden Sie unter [Partitionieren in Azure Cosmos DB mithilfe der SQL-API](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB-API
Bei der MongoDB-API können Sie eine Sammlung mit Shards mithilfe Ihrer bevorzugten Tools, Treiber oder SDKs erstellen. In diesem Beispiel verwenden wir die Mongo Shell zum Erstellen einer Sammlung.

In der Mongo Shell:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Ergebnisse:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tabelle-API

Um eine Tabelle mit der Tabellen-API zu erstellen, verwenden Sie die `CreateIfNotExists`-Methode. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Der bereitgestellte Durchsatz wird als Argument von `CreateIfNotExists` festgelegt. Der Partitionsschlüssel wird implizit als `PartitionKey`-Wert erstellt. 

Mit dem folgendem Code können Sie eine einzelne Entität abrufen:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Weitere Informationen finden Sie unter [Entwickeln mit der Tabellen-API](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>Gremlin-API

Mit der Gremlin-API können Sie über das Azure-Portal oder die Azure CLI einen Container erstellen, der einen Graphen darstellt. Da Azure Cosmos DB mehrere Modelle unterstützt, können Sie alternativ dazu auch eine der anderen APIs verwenden, um Ihren Graph-Container zu erstellen und zu skalieren.

Mit dem Partitionsschlüssel und der Partitions-ID in Gremlin können Sie jeden beliebigen Scheitelpunkt- oder Randwert lesen. Bei einem Graphen mit der Region „USA“ als Partitionsschlüssel und „Seattle“ als Zeilenschlüssel können Sie einen Scheitelpunkt beispielsweise mit folgender Syntax suchen:

```
g.V(['USA', 'Seattle'])
```

Sie können mithilfe des Partitions- und des Zeilenschlüssels auf einen Randwert verweisen.

```
g.E(['USA', 'I5'])
```

Weitere Informationen finden Sie unter [Verwenden eines partitionierten Graph in Azure Cosmos DB](graph-partitioning.md).

## <a name="form-partition-key-by-concatenating-multiple-fields"></a>Formen des Partitionsschlüssels durch Verketten mehrerer Felder

Sie können einen Partitionsschlüssel auch formen, indem Sie mehrere Eigenschaftenwerte zu einer einzigen künstlichen partitionKey-Eigenschaft des Elements verketten und auffüllen. Diese Schlüssel werden als synthetische Schlüssel bezeichnet.

Beispiel: Sie verfügen über ein Dokument, das folgendermaßen aussieht:

```json
{
"deviceId": "abc-123",
"date": 2018
}
```

Eine Möglichkeit besteht darin, die partitionKey-Eigenschaft auf „/deviceId“ oder „/date“ festzulegen, wenn Sie einen Partitionsschlüssel für Geräte-ID und Datum formen möchten. Verketten Sie diese beiden Werte zu einer künstlichen partitionKey-Eigenschaft, und legen Sie den Partitionsschlüssel auf „/partitionKey“ fest.

```json
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

In realen Szenarien verfügen Sie u.U. über Tausende von Dokumenten, daher sollten Sie die clientseitige Logik zum Verketten der Werte in einen synthetischen Schlüssel definieren, den synthetischen Schlüssel in die Dokumente einfügen und ihn dann dazu verwenden, den Partitionsschlüssel anzugeben.

<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Entwurf für die Skalierung
Um bei Azure Cosmos DB eine effektive Skalierung zu erzielen, müssen Sie beim Erstellen Ihres Containers einen sinnvollen Partitionsschlüssel auswählen. Bei der Auswahl eines guten Partitionsschlüssels müssen zwei wichtige Überlegungen berücksichtigt werden:

* **Abfragebegrenzungen und Transaktionen**. Sie sollten Ihren Partitionsschlüssel so wählen, dass Transaktionen vorgenommen werden können und zugleich die Skalierbarkeit der Lösung gegeben ist (durch Verteilung Ihrer Entitäten auf mehrere Partitionsschlüssel). An einem Ende der Skala könnten Sie denselben Partitionsschlüssel für alle Elemente festlegen, dies könnte aber die Skalierbarkeit Ihrer Lösung beeinträchtigen. Andererseits können Sie jedem Element einen eindeutigen Partitionsschlüssel zuweisen. Diese Auswahl ist hochgradig skalierbar, verhindert aber, dass Sie dokumentübergreifende Transaktionen über gespeicherte Prozeduren und Trigger verwenden können. Ein idealer Partitionsschlüssel ermöglicht die Verwendung von effizienten Abfragen und verfügt über eine ausreichende Kardinalität, um sicherzustellen, dass Ihre Lösung skalierbar ist. 
* **Keine Speicher- und Leistungsengpässe**. Es ist wichtig, eine Eigenschaft auszuwählen, die eine gleichmäßige Verteilung von Schreibvorgängen auf verschiedene unterschiedliche Werte ermöglicht. Anforderungen an den gleichen Partitionsschlüssel können den für eine Partition bereitgestellten Durchsatz nicht überschreiten und sind ratenbegrenzt. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu „Hotspots“ innerhalb Ihrer Anwendung führt. Da alle Daten für einen einzelnen Partitionsschlüssel innerhalb einer Partition gespeichert werden müssen, sollten Sie möglichst keine Partitionsschlüssel verwenden, die große Mengen an Daten für den gleichen Wert besitzen. 

Im Folgenden sehen wir uns einige Szenarien aus dem echten Leben sowie sinnvolle Partitionsschlüssel für jedes Szenario an:
* Wenn Sie ein Benutzerprofil-Back-End implementieren, ist die *Benutzer-ID* eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie IoT-Daten speichern (z.B. zum Gerätezustand), ist eine *Geräte-ID* eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie Azure Cosmos DB für die Protokollierung von Zeitreihendaten verwenden, ist der *Hostname* oder die *Prozess-ID* eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie über eine mehrinstanzenfähige Architektur verfügen, ist die *Mandanten-ID* eine gute Wahl für den Partitionsschlüssel.

In einigen Anwendungsfällen (z.B. bei IoT und Benutzerprofilen) kann der Partitionsschlüssel mit Ihrer *ID* (Dokumentenschlüssel) identisch sein. In anderen Fällen, wie etwa bei den Zeitreihendaten, kann sich der Partitionsschlüssel von der *ID* unterscheiden.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionierung und Protokollierung/Zeitreihendaten
Gängige Anwendungsfälle in Azure Cosmos DB sind Protokollierung und Telemetrie. Es ist wichtig, in diesem Szenario einen guten Partitionsschlüssel auswählen, da Sie möglicherweise große Datenmengen lesen oder schreiben müssen. Die Auswahl eines Partitionsschlüssels hängt von Ihren Lese- und Schreibraten und der Art der Abfragen ab, deren Ausführung Sie erwarten. Im Folgenden finden Sie einige Tipps zum Auswählen eines guten Partitionsschlüssels:

* Wenn Ihr Anwendungsfall eine kleine Rate von Schreibvorgängen umfasst, die über einen langen Zeitraum anfallen, und Sie Abfragen nach Zeitstempelbereichen mit anderen Filtern ausführen müssen, verwenden Sie einen Rollup des Zeitstempels. Ein guter Ansatz ist beispielsweise die Verwendung des Datums als Partitionsschlüssel. Mit diesem Ansatz können Sie Abfragen über alle Daten für ein angegebenes Datum von einer einzigen Partition ausführen. 
* Wenn Ihre Workload schreiblastig ist (was in diesem Szenario sehr häufig vorkommt), verwenden Sie einen Partitionsschlüssel, der nicht auf dem Zeitstempel basiert. Azure Cosmos DB kann die Schreibvorgänge dann gleichmäßig auf die verschiedenen Partitionen verteilen. In diesem Fall ist *Hostname*, *Prozess-ID*, *Aktivitäts-ID* oder eine andere Eigenschaft mit hoher Kardinalität eine gute Wahl. 
* Eine weitere Möglichkeit ist ein Hybridansatz mit mehreren Containern (ein Container für jeden Tag des Monats), bei dem der Partitionsschlüssel eine differenzierte Eigenschaft wie z.B. der *Hostname* ist. Dieser Ansatz hat den Vorteil, dass Sie je nach Zeitfenster, Skalierung und Leistungsbedarf unterschiedliche Durchsätze für jeden Container oder für eine Gruppe von Containern festlegen können. Ein Container für den aktuellen Monat kann z.B. mit einem höheren Durchsatz bereitgestellt werden, da er für Lese- und Schreibvorgänge verwendet wird. Frühere Monate können mit einem niedrigeren Durchsatz bereitgestellt werden, da sie nur Lesevorgängen dienen.

### <a name="partitioning-and-multitenancy"></a>Partitionierung und Mehrinstanzenfähigkeit
Wenn Sie mit Azure Cosmos DB eine mehrinstanzenfähige Anwendung implementieren, sind zwei gängige Entwürfe verfügbar: *ein Partitionsschlüssel pro Mandant* und *ein Container pro Mandant*. Hier sind ihre Vor- und Nachteile auflistet:

* **Ein Partitionsschlüssel pro Mandant**. Bei diesem Modell sind Mandanten innerhalb eines einzelnen Containers verbunden. Abfragen und Einfügungen für einen einzelnen Mandanten können für eine einzelne Partition ausgeführt werden. Sie können auch Transaktionslogik über alle Elemente hinweg implementieren, die zu einem Mandanten gehören. Da mehrere Mandanten einen Container gemeinsam nutzen, können Sie den Speicher und den bereitgestellten Durchsatz besser nutzen, indem Sie Ressourcen für alle Mandanten in einem einzigen Container in einem Pool zusammenfassen, anstatt eine Bereitstellung für jeden Mandanten auszuführen. Der Nachteil darin besteht, dass Sie über keine Leistungsisolation pro Mandant verfügen. Die Erhöhung des Durchsatzes zur Gewährleistung der Leistung gilt für den gesamten Container mit allen Mandanten im Gegensatz zur gezielten Erhöhung für einen einzelnen Mandanten.
* **Ein Container pro Mandant**. Bei diesem Modell hat jeder Mandant einen eigenen Container, und Sie können Durchsatz mit garantierter Leistung pro Mandant reservieren. Dieses Modell ist kosteneffizienter für mehrinstanzenfähige Anwendungen mit wenigen Mandanten.

Sie können auch einen Hybridansatz verwenden, bei dem kleine Mandanten zusammengefasst und größere Mandanten in einem eigenen Container isoliert werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde eine Übersicht über Konzepte und bewährte Methoden für die Skalierung und Partitionierung in Azure Cosmos DB bereitgestellt. 

* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).




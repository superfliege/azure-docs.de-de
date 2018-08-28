---
title: 'Azure Cosmos DB: .NET Change Feed Processor-API, -SDK und -Ressourcen | Microsoft-Dokumentation'
description: Wichtige Informationen zur Change Feed Processor-API und zum Change Feed Processor-SDK, einschließlich Terminen für Veröffentlichung und Außerbetriebnahme sowie Änderungen an den einzelnen Versionen des .NET Change Feed Processor-SDK.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/21/2018
ms.author: maquaran
ms.openlocfilehash: 443cb2786c12bddd87535a7267d66c0332473b1b
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "42145378"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET Change Feed Processor-SDK: Download und Anmerkungen zur Version
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Änderungsfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor: Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK-Download**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-Dokumentation**|[Referenzdokumentation zum Ändern der Feedprozessorbibliotheks-API](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Erste Schritte**|[Erste Schritte mit dem .NET Change Feed Processor-SDK](change-feed.md)|
|**Aktuelles unterstütztes Framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Versionshinweise

### <a name="v2-builds"></a>V2-Builds

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* Öffentliche Eigenschaft „ChangeFeedEventHost.HostName“ zur Kompatibilität mit v1 hinzugefügt.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Ein Problem mit einer Racebedingung, das während der Partitionsaufteilung auftritt, wurde behoben. Die Racebedingung kann dazu führen, dass das Lease angefordert, bei der Partitionsaufteilung sofort verloren geht und einen Konflikt verursacht. In diesem Release wurde das Problem mit der Racebedingung behoben.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* Allgemeine Verfügbarkeit (GA) des SDK

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3 – Vorabversion
* Die folgenden Probleme wurden behoben:
  * Wenn eine Partition geteilt wird, werden Dokumente, die vor der Teilung modifiziert wurden, möglicherweise doppelt verarbeitet.
  * Die GetEstimatedRemainingWork-API gab 0 zurück, wenn in der Leasesammlung keine Leases vorhanden waren.

* Die folgenden Ausnahmen wurden öffentlich gemacht. Erweiterungen, die IPartitionProcessor implementieren, können diese Ausnahmen auslösen.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-Vorabversion
* Geringfügige API-Änderungen:
  * Die als veraltet gekennzeichnete ChangeFeedProcessorOptions.IsAutoCheckpointEnabled wurde entfernt.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Verbesserungen der Stabilität:
  * Bessere Handhabung der Leasespeicherinitialisierung. Wenn der Leasespeicher leer ist, kann er nur von einer Instanz des Prozessors initialisiert werden, die anderen warten.
  * Stabilere/effizientere Leaseerneuerung/-freigabe. Das Erneuern und Freigeben einer Lease für eine Partition erfolgt unabhängig von der Erneuerung anderer Leases. In v1 erfolgte dies sequenziell für alle Partitionen.
* Neue v2-API:
  * Generatormuster für die flexible Erstellung des Prozessors: ChangeFeedProcessorBuilder-Klasse
    * Akzeptiert beliebige Kombinationen aus Parametern
    * Akzeptiert die DocumentClient-Instanz für die Überwachung und/oder Leasesammlung (nicht in v1 verfügbar)
  * „IChangeFeedObserver.ProcessChangesAsync“ akzeptiert nun „CancellationToken“.
  * IRemainingWorkEstimator: Die Schätzung der verbleibenden Arbeit kann separat vom Prozessor verwendet werden.
  * Neue Erweiterbarkeitspunkte:
    * IParitionLoadBalancingStrategy: für den benutzerdefinierten Lastenausgleich von Partitionen zwischen Prozessorinstanzen
    * ILease, ILeaseManager: für die benutzerdefinierte Leaseverwaltung
    * IPartitionProcessor: für benutzerdefinierte Verarbeitungsänderungen an einer Partition
* Protokollierung: Verwendet die [LibLog](https://github.com/damianh/LibLog)-Bibliothek
* 100-prozentige Abwärtskompatibilität mit der v1-API
* Neue Codebasis.
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.21.1 und höher

### <a name="v1-builds"></a>V1-Builds

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Weitere Protokollierung hinzugefügt.
* Ein DocumentClient-Fehler beim mehrfachen Aufrufen der Schätzung für ausstehende Arbeit wurde behoben.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Korrekturen der Schätzung ausstehender Arbeit.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Verbesserungen der Stabilität.
  * Korrektur für das Behandeln von Problemen mit abgebrochenen Aufgaben, die dazu führen können, dass Beobachter auf einigen Partitionen beendet werden.
* Unterstützung für das manuelle Setzen von Prüfpunkten.
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.21 und höher.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Unterstützung für .NET Standard 2.0 wurde hinzugefügt. Das Paket unterstützt nun die Frameworkmoniker `netstandard2.0` und `net451`.
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.17.0 und höher.
* Kompatibel mit [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md), Version 1.5.1 und höher.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Ein Problem bei der Kalkulierung der geschätzten verbleibenden Arbeit wurde behoben, wenn der Änderungsfeed leer war oder keine Arbeit ausstand.
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.13.2 und höher.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Eine Methode wurde hinzugefügt, mit der eine Schätzung der verbleibenden im Änderungsfeed zu verarbeitenden Arbeit abgerufen werden kann.
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.13.2 und höher.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Allgemeine Verfügbarkeit (GA) des SDK
* Kompatibel mit [SQL .NET SDK](sql-api-sdk-dotnet.md), Version 1.14.1 und niedriger.


## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen an Cosmos DB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.3.3](#1.3.3) |8. Mai 2018 |--- |
| [1.3.2](#1.3.2) |18. April 2018 |--- |
| [1.3.1](#1.3.1) |13. März 2018 |--- |
| [1.2.0](#1.2.0) |31. Oktober 2017 |--- |
| [1.1.1](#1.1.1) |29. August 2017 |--- |
| [1.1.0](#1.1.0) |13. August 2017 |--- |
| [1.0.0](#1.0.0) |7. Juli 2017 |--- |


## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 


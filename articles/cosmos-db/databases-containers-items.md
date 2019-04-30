---
title: Arbeiten mit Azure Cosmos DB-Datenbanken, -Containern und -Elementen
description: In diesem Artikel wird die Erstellung und Verwendung von Azure Cosmos DB-Datenbanken, -Containern und -Elementen beschrieben.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 8eaca83b7ea89737a63fe56a18505c8df7e93fdc
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678793"
---
# <a name="work-with-databases-containers-and-items"></a>Arbeiten mit Datenbanken, Containern und Elementen

Nach der Erstellung eines [Azure Cosmos-Kontos](account-overview.md) unter Ihrem Azure-Abonnement können Sie Daten in Ihrem Konto verwalten, indem Sie Datenbanken, Container und Elemente erstellen. In diesem Artikel werden diese Entitäten einzeln beschrieben. In der folgenden Abbildung ist die Hierarchie der verschiedenen Entitäten in einem Azure Cosmos-Konto dargestellt:

![Entitäten in einem Azure Cosmos-Konto](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos-Datenbanken

Unter Ihrem Konto können Sie mehrere Azure Cosmos-Datenbanken erstellen. Eine Datenbank ist analog zu einem Namespace. Sie ist eine Verwaltungseinheit für eine Gruppe von Azure Cosmos-Containern. In der folgenden Tabelle ist dargestellt, wie eine Azure Cosmos-Datenbank verschiedenen API-spezifischen Entitäten zugeordnet wird:

| **Azure Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Datenbank | Datenbank | Keyspace | Datenbank | Datenbank | Nicht verfügbar |

> [!NOTE]
> Wenn Sie in Tabellen-API-Konten die erste Tabelle erstellen, wird in Ihrem Azure Cosmos-Konto automatisch eine Standarddatenbank erstellt.

### <a name="operations-on-an-azure-cosmos-database"></a>Vorgänge in einer Azure Cosmos-Datenbank

Mit einer Azure Cosmos-Datenbank können Sie über die folgenden Azure Cosmos-APIs interagieren:

| **Vorgang** | **Azure-Befehlszeilenschnittstelle**|**SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- |
|Auflisten aller Datenbanken| Ja | Ja | Ja (Datenbank ist einem Keyspace zugeordnet) | Ja | Nicht verfügbar | Nicht verfügbar |
|Lesen der Datenbank| Ja | Ja | Ja (Datenbank ist einem Keyspace zugeordnet) | Ja | Nicht verfügbar | Nicht verfügbar |
|Neue Datenbank erstellen| Ja | Ja | Ja (Datenbank ist einem Keyspace zugeordnet) | Ja | Nicht verfügbar | Nicht verfügbar |
|Aktualisieren der Datenbank| Ja | Ja | Ja (Datenbank ist einem Keyspace zugeordnet) | Ja | Nicht verfügbar | Nicht verfügbar |


## <a name="azure-cosmos-containers"></a>Azure Cosmos-Container

Ein Azure Cosmos-Container ist die Skalierungseinheit für den bereitgestellten Durchsatz und den Speicher. Ein Container wird horizontal partitioniert und dann in mehreren Regionen repliziert. Die Elemente, die Sie dem Container hinzuzufügen, und der Durchsatz, den Sie für den Container bereitstellen, werden basierend auf dem Partitionsschlüssel automatisch auf eine Gruppe von logischen Partitionen verteilt. Weitere Informationen zur Partitionierung und zum Partitionsschlüssel finden Sie in [diesem](partition-data.md) Artikel. 

Beim Erstellen eines Azure Cosmos-Containers konfigurieren Sie den Durchsatz in einem der folgenden Modi:

* Modus **Dedizierter bereitgestellter Durchsatz**: Der für einen Container bereitgestellte Durchsatz ist ausschließlich für diesen Container reserviert und wird durch die SLAs zugesichert. Weitere Informationen finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).

* Modus **Bereitgestellter, gemeinsam genutzter Durchsatz**: Die Container teilen sich den bereitgestellten Durchsatz mit anderen Containern in der gleichen Datenbank (mit Ausnahme der Container, die mit dediziert bereitgestelltem Durchsatz konfiguriert wurden). Das heißt, der bereitgestellte Durchsatz für die Datenbank wird von allen Containern mit freigegebenem Durchsatz genutzt. Weitere Informationen finden Sie unter [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

Ein Azure Cosmos-Container kann elastisch skaliert werden, und zwar unabhängig davon, ob Sie Container mit gemeinsam genutztem oder dediziert bereitgestelltem Durchsatz erstellen.

Ein Azure Cosmos-Container ist ein schemaunabhängiger Container für Elemente. Elemente innerhalb eines Containers können beliebige Schemas aufweisen. Beispielsweise können ein Element, das eine Person darstellt, und ein Element, das ein Fahrzeug darstellt, im *selben Container* platziert werden. Standardmäßig werden alle Elemente, die Sie einem Container hinzufügen, automatisch indiziert, ohne dass eine explizite Index- oder Schemaverwaltung erforderlich ist. Sie können das Indizierungsverhalten anpassen, indem Sie die [Indizierungsrichtlinie](index-overview.md) für einen Container konfigurieren. 

Sie können die [Gültigkeitsdauer (TTL)](time-to-live.md) für ausgewählte Elemente innerhalb eines Azure Cosmos-Containers oder für den gesamten Container festlegen, um die Elemente ordnungsgemäß aus dem System zu löschen. Die Elemente werden nach Ablauf der Gültigkeitsdauer in Azure Cosmos DB automatisch gelöscht. Zudem wird sichergestellt, dass eine für den Container ausgeführte Abfrage die abgelaufenen Elemente innerhalb einer festen Grenze nicht zurückgibt. Weitere Informationen finden Sie unter [Konfigurieren der Gültigkeitsdauer in Ihrem Container](how-to-time-to-live.md).

Durch Verwenden des [Änderungsfeeds](change-feed.md) können Sie das Vorgangsprotokoll abonnieren, das für jede logische Partition des Containers verwaltet wird. Der Änderungsfeed enthält das Protokoll aller Aktualisierungen, die für den Container durchgeführt wurden, sowie die Vorher- und Nachher-Images der Elemente. Siehe [Erstellen von reaktiven Anwendungen mit dem Änderungsfeed](serverless-computing-database.md). Sie können auch die Aufbewahrungsdauer für den Änderungsfeed konfigurieren, indem Sie die Änderungsfeedrichtlinie für den Container verwenden. 

Sie können [gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen (UDFs)](stored-procedures-triggers-udfs.md) und [Mergeprozeduren](how-to-manage-conflicts.md) beim Azure Cosmos-Container registrieren. 

Sie können eine [eindeutige Schlüsseleinschränkung](unique-keys.md) für den Azure Cosmos-Container angeben. Durch Erstellen einer Richtlinie für eindeutige Schlüssel wird die Eindeutigkeit von einem oder mehreren Werten pro logischem Partitionsschlüssel sichergestellt. Nachdem ein Container mit einer Richtlinie für eindeutige Schlüssel erstellt wurde, ist keine Erstellung neuer oder aktualisierter Elemente mit Werten möglich, die die durch die Einschränkung für eindeutige Schlüssel festgelegten Werte duplizieren. Weitere Informationen finden Sie unter [Einschränkungen für eindeutige Schlüssel](unique-keys.md).

Ein Azure Cosmos-Container wird wie folgt in API-spezifische Entitäten kategorisiert:

| **Azure Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Container | Sammlung | Table | Sammlung | Graph | Table |

### <a name="properties-of-an-azure-cosmos-container"></a>Eigenschaften eines Azure Cosmos-Containers

Ein Azure Cosmos-Container enthält einen Satz von systemdefinierten Eigenschaften. Abhängig von der API-Auswahl werden einige möglicherweise nicht direkt verfügbar gemacht. In der folgenden Tabelle werden die systemdefinierten Eigenschaften beschrieben:

| **Systemdefinierte Eigenschaft** | **Systemgeneriert oder vom Benutzer konfigurierbar** | **Zweck** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_rid | Systemgeneriert | Eindeutiger Bezeichner des Containers | Ja | Nein  | Nein  | Nein  | Nein  |
|_etag | Systemgeneriert | Entitätstag, das zur Steuerung der optimistischen Nebenläufigkeit genutzt wird | Ja | Nein  | Nein  | Nein  | Nein  |
|_ts | Systemgeneriert | Zuletzt aktualisierter Zeitstempel des Containers | Ja | Nein  | Nein  | Nein  | Nein  |
|_self | Systemgeneriert | Adressierbarer URI des Containers | Ja | Nein  | Nein  | Nein  | Nein  |
|id | Vom Benutzer konfigurierbar | Benutzerdefinierter eindeutiger Name des Containers | Ja | Ja | Ja | Ja | Ja |
|indexingPolicy | Vom Benutzer konfigurierbar | Bietet die Möglichkeit, den Indexpfad, den Indextyp und den Indexmodus zu ändern. | Ja | Nein  | Nein  | Nein  | Ja |
|TimeToLive | Vom Benutzer konfigurierbar | Bietet die Möglichkeit, Elemente nach einem bestimmten Zeitraum automatisch aus einem Container zu löschen. Weitere Details finden Sie im Artikel zur [Gültigkeitsdauer](time-to-live.md). | Ja | Nein  | Nein  | Nein  | Ja |
|changeFeedPolicy | Vom Benutzer konfigurierbar | Wird zum Lesen von Änderungen verwendet, die an Elementen in einem Container vorgenommen wurden. Weitere Informationen finden Sie im Artikel zum [Änderungsfeed](change-feed.md). | Ja | Nein  | Nein  | Nein  | Ja |
|uniqueKeyPolicy | Vom Benutzer konfigurierbar | Damit stellen Sie die Eindeutigkeit von Werten innerhalb einer logischen Partition sicher. Weitere Informationen finden Sie im Artikel zu [Eindeutigen Schlüsseleinschränkungen](unique-keys.md). | Ja | Nein  | Nein  | Nein  | Ja |

### <a name="operations-on-an-azure-cosmos-container"></a>Vorgänge in einem Azure Cosmos-Container

In einem Azure Cosmos-Container werden die folgenden Vorgänge über die Azure Cosmos-APIs unterstützt.

| **Vorgang** | **Azure-Befehlszeilenschnittstelle** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- |
| Auflisten von Containern in einer Datenbank | Ja | Ja | Ja | Ja | Nicht verfügbar | Nicht verfügbar |
| Lesen eines Containers | Ja | Ja | Ja | Ja | Nicht verfügbar | Nicht verfügbar |
| Erstellen eines neuen Containers | Ja | Ja | Ja | Ja | Nicht verfügbar | Nicht verfügbar |
| Aktualisieren eines Containers | Ja | Ja | Ja | Ja | Nicht verfügbar | Nicht verfügbar |
| Löschen eines Containers | Ja | Ja | Ja | Ja | Nicht verfügbar | Nicht verfügbar |

## <a name="azure-cosmos-items"></a>Azure Cosmos-Elemente

Abhängig von der API-Auswahl kann ein Azure Cosmos-Element ein Dokument in einer Sammlung, eine Zeile in einer Tabelle oder einen Knoten oder Edge in einem Graph darstellen. In der folgenden Tabelle ist die Zuordnung zwischen API-spezifischen Entitäten zu einem Azure Cosmos-Element dargestellt:

| **Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Element | Dokument | Zeile | Dokument | Knoten oder Edge | Item |

### <a name="properties-of-an-item"></a>Eigenschaften eines Elements

Jedes Azure Cosmos-Element verfügt über die folgenden systemdefinierten Eigenschaften. Abhängig von der API-Auswahl werden einige möglicherweise nicht direkt verfügbar gemacht.

|**Systemdefinierte Eigenschaft** | **Systemgeneriert oder vom Benutzer konfigurierbar**| **Zweck** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_id | Systemgeneriert | Eindeutiger Bezeichner eines Elements | Ja | Nein  | Nein  | Nein  | Nein  |
|_etag | Systemgeneriert | Entitätstag, das zur Steuerung der optimistischen Nebenläufigkeit genutzt wird | Ja | Nein  | Nein  | Nein  | Nein  |
|_ts | Systemgeneriert | Der Zeitstempel des letzten Elementeupdates | Ja | Nein  | Nein  | Nein  | Nein  |
|_self | Systemgeneriert | Adressierbarer URI des Elements | Ja | Nein  | Nein  | Nein  | Nein  |
|id | Sie können das | Benutzerdefinierter eindeutiger Name innerhalb einer logischen Partition. Wenn der Benutzer die ID nicht angibt, wird im System automatisch eine generiert. | Ja | Ja | Ja | Ja | Ja |
|Beliebige benutzerdefinierte Eigenschaften | Benutzerdefiniert | Benutzerdefinierte Eigenschaften, die in einer API-nativen Darstellung (JSON, BSON, CQL usw.) dargestellt werden | Ja | Ja | Ja | Ja | Ja |

### <a name="operations-on-items"></a>Vorgänge in Entitäten

Azure Cosmos-Elemente unterstützen die folgenden Vorgänge, die über sämtliche Azure Cosmos-APIs durchgeführt werden können.

| **Vorgang** | **Azure-Befehlszeilenschnittstelle** | **SQL-API** | **Cassandra-API** | **MongoDB-API von Azure Cosmos DB** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- |
| Einfügen, Ersetzen, Löschen, Upsert, Lesen | Nein  | Ja | Ja | Ja | Ja | Ja |

## <a name="next-steps"></a>Nächste Schritte

Sie können sich jetzt mit den folgenden Konzepten vertraut machen:

* [Konfigurieren des bereitgestellten des Durchsatzes für eine Azure Cosmos-Datenbank](how-to-provision-database-throughput.md)
* [Konfigurieren des bereitgestellten Durchsatzes für einen Azure Cosmos-Container](how-to-provision-container-throughput.md)
* [Logische Partitionen](partition-data.md)
* [Konfigurieren der Gültigkeitsdauer in Azure Cosmos-Containern](how-to-time-to-live.md)
* [Erstellen von reaktiven Anwendungen mit dem Änderungsfeed](change-feed.md)
* [Eindeutige Schlüssel in Azure Cosmos DB](unique-keys.md)

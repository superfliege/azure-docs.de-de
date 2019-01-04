---
title: Arbeiten mit Azure Cosmos DB-Datenbanken, -Containern und -Elementen
description: In diesem Artikel wird die Erstellung und Verwendung von Azure Cosmos DB-Datenbanken, -Containern und -Elementen beschrieben.
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 39de7453c9d3b0335748cd37e4b1eef91b64b207
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409540"
---
# <a name="working-with-azure-cosmos-databases-containers-and-items"></a>Arbeiten mit Azure Cosmos-Datenbanken, -Containern und -Elementen

Nach der Erstellung eines [Azure Cosmos DB-Kontos](account-overview.md) unter Ihrem Azure-Abonnement können Sie Daten in Ihrem Konto verwalten, indem Sie Datenbanken, Container und Elemente erstellen. In diesem Artikel werden diese Entitäten einzeln beschrieben. In der folgenden Abbildung ist die Hierarchie der verschiedenen Entitäten in einem Azure Cosmos-Konto dargestellt:

![Entitäten in einem Azure Cosmos-Konto](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos-Datenbanken

Unter Ihrem Konto können Sie mehrere Azure Cosmos-Datenbanken erstellen. Eine Datenbank entspricht einem Namespace, sie ist die Verwaltungseinheit für mehrere Azure Cosmos-Container. In der folgenden Tabelle ist dargestellt, wie eine Azure Cosmos-Datenbank verschiedenen API-spezifischen Entitäten zugeordnet wird:

| **Azure Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Datenbank | Datenbank | Keyspace | Datenbank | Datenbank | Nicht verfügbar |

> [!NOTE]
> Wenn Sie in Tabellen-API-Konten die erste Tabelle erstellen, wird in Ihrem Azure Cosmos-Konto automatisch eine Standarddatenbank erstellt.

### <a name="operations-on-an-azure-cosmos-database"></a>Vorgänge in einer Azure Cosmos-Datenbank

Mit einer Azure Cosmos-Datenbank können Sie über die folgenden Azure Cosmos-APIs interagieren:

| **Vorgang** | **Azure-Befehlszeilenschnittstelle**|**SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- |
|Auflisten aller Datenbanken| JA | JA | Ja (Datenbank ist einem Keyspace zugeordnet) | JA | Nicht verfügbar | Nicht verfügbar |
|Lesen der Datenbank| JA | JA | Ja (Datenbank ist einem Keyspace zugeordnet) | JA | Nicht verfügbar | Nicht verfügbar |
|Neue Datenbank erstellen| JA | JA | Ja (Datenbank ist einem Keyspace zugeordnet) | JA | Nicht verfügbar | Nicht verfügbar |
|Aktualisieren der Datenbank| JA | JA | Ja (Datenbank ist einem Keyspace zugeordnet) | JA | Nicht verfügbar | Nicht verfügbar |


## <a name="azure-cosmos-containers"></a>Azure Cosmos-Container

Ein Azure Cosmos-Container ist die Skalierungseinheit für den bereitgestellten Durchsatz und den Speicher von Elementen. Ein Container wird horizontal partitioniert und dann in mehreren Regionen repliziert. Die Elemente, die Sie dem Container hinzuzufügen, und der Durchsatz, den Sie für den Container bereitstellen, werden basierend auf dem Partitionsschlüssel automatisch auf eine Gruppe von logischen Partitionen verteilt. Weitere Informationen zur Partitionierung und zum Partitionsschlüssel finden Sie unter [Logische Partitionen](partition-data.md). 

Beim Erstellen eines Azure Cosmos-Containers konfigurieren Sie den Durchsatz in einem der folgenden Modi:

* Modus **Dedizierter bereitgestellter Durchsatz**: Der für einen Container bereitgestellte Durchsatz ist ausschließlich für diesen reserviert und wird durch die SLAs zugesichert. Weitere Informationen finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md).

* Modus **Bereitgestellter, gemeinsam genutzter Durchsatz**: Die Container teilen sich den bereitgestellten Durchsatz mit anderen Containern in der gleichen Datenbank (mit Ausnahme der Container, die mit dediziert bereitgestelltem Durchsatz konfiguriert wurden). Das heißt, der bereitgestellte Durchsatz für die Datenbank wird von allen Containern gemeinsam genutzt. Weitere Informationen finden Sie unter [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).

Ein Azure Cosmos-Container kann elastisch skaliert werden, und zwar unabhängig davon, ob Sie Container mit gemeinsam genutztem oder dediziert bereitgestelltem Durchsatz erstellen.

Ein Azure Cosmos-Container ist ein schemaunabhängiger Container für Elemente. Elemente innerhalb eines Containers können beliebige Schemas aufweisen. Beispielsweise können ein Element, das eine Person darstellt, und ein Element, das ein Fahrzeug darstellt, im selben Container platziert werden. Standardmäßig werden alle Elemente, die Sie einem Container hinzufügen, automatisch indiziert, ohne dass eine explizite Index- oder Schemaverwaltung erforderlich ist. Sie können das Indizierungsverhalten anpassen, indem Sie die Indizierungsrichtlinie für einen Container konfigurieren. 

Sie können die Gültigkeitsdauer (TTL) für ausgewählte Elemente innerhalb eines Azure Cosmos-Containers oder für den gesamten Container festlegen, um die Elemente ordnungsgemäß aus dem System zu löschen. Die Elemente werden nach Ablauf der Gültigkeitsdauer in Azure Cosmos DB automatisch gelöscht. Zudem wird sichergestellt, dass eine für den Container ausgeführte Abfrage die abgelaufenen Elemente innerhalb einer festen Grenze nicht zurückgibt. Weitere Informationen finden Sie unter [Konfigurieren der Gültigkeitsdauer in Ihrem Container](how-to-time-to-live.md).

Durch Verwenden des Änderungsfeeds können Sie das Vorgangsprotokoll abonnieren, das für jede logische Partition des Containers verwaltet wird. Der Änderungsfeed enthält das Protokoll aller Aktualisierungen, die für den Container durchgeführt wurden, sowie die Vorher- und Nachher-Images der Elemente. Siehe [Erstellen von reaktiven Anwendungen mit dem Änderungsfeed](change-feed.md). Sie können auch die Aufbewahrungsdauer für den Änderungsfeed konfigurieren, indem Sie die Änderungsfeedrichtlinie für den Container verwenden. 

Sie können gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen (UDFs) und Mergeprozeduren beim Azure Cosmos-Container registrieren. 

Sie können einen eindeutigen Schlüssel für den Azure Cosmos-Container angeben. Durch Erstellen einer Richtlinie für eindeutige Schlüssel wird die Eindeutigkeit von einem oder mehreren Werten pro logischem Partitionsschlüssel sichergestellt. Nachdem ein Container mit einer Richtlinie für eindeutige Schlüssel erstellt wurde, ist keine Erstellung neuer oder aktualisierter Elemente mit Werten möglich, die die durch die Einschränkung für eindeutige Schlüssel festgelegten Werte duplizieren. Weitere Informationen finden Sie unter [Einschränkungen für eindeutige Schlüssel](unique-keys.md).

Ein Azure Cosmos-Container wird wie folgt in API-spezifische Entitäten kategorisiert:

| **Azure Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Container | Sammlung | Table | Sammlung | Graph | Table |

### <a name="properties-of-an-azure-cosmos-container"></a>Eigenschaften eines Azure Cosmos-Containers

Ein Azure Cosmos-Container enthält einen Satz von systemdefinierten Eigenschaften. Abhängig von der API-Auswahl werden einige möglicherweise nicht direkt verfügbar gemacht. In der folgenden Tabelle werden die unterstützten systemdefinierten Eigenschaften beschrieben:

| **Systemdefinierte Eigenschaft** | **Systemgeneriert oder benutzerdefinierbar** | **Zweck** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__rid | Systemgeneriert | Eindeutiger Bezeichner des Containers | JA | Nein  | Nein  | Nein  | Nein  |
|__etag | Systemgeneriert | Entitätstag, das zur Steuerung der optimistischen Nebenläufigkeit genutzt wird | JA | Nein  | Nein  | Nein  | Nein  |
|__ts | Systemgeneriert | Zuletzt aktualisierter Zeitstempel des Containers | JA | Nein  | Nein  | Nein  | Nein  |
|__self | Systemgeneriert | Adressierbarer URI des Containers | JA | Nein  | Nein  | Nein  | Nein  |
|id | Vom Benutzer konfigurierbar | Benutzerdefinierter eindeutiger Name des Containers | JA | Ja | Ja | Ja | JA |
|indexingPolicy | Vom Benutzer konfigurierbar | Bietet die Möglichkeit, den Indexpfad, die Genauigkeit und das Konsistenzmodell zu ändern. | JA | Nein  | Nein  | Nein  | JA |
|TimeToLive | Vom Benutzer konfigurierbar | Bietet die Möglichkeit, Elemente nach einem bestimmten Zeitraum automatisch aus einem Container zu löschen. Weitere Details finden Sie im Artikel zur [Gültigkeitsdauer](time-to-live.md). | JA | Nein  | Nein  | Nein  | JA |
|changeFeedPolicy | Vom Benutzer konfigurierbar | Wird zum Lesen von Änderungen verwendet, die an Elementen in einem Container vorgenommen wurden. Weitere Informationen finden Sie im Artikel zum [Änderungsfeed](change-feed.md). | JA | Nein  | Nein  | Nein  | JA |
|uniqueKeyPolicy | Vom Benutzer konfigurierbar | Mit eindeutigen Schlüsseln stellen Sie die Eindeutigkeit von Werten innerhalb einer logischen Partition sicher. Weitere Informationen finden Sie im Artikel zu [eindeutigen Schlüsseln](unique-keys.md). | JA | Nein  | Nein  | Nein  | JA |

### <a name="operations-on-an-azure-cosmos-container"></a>Vorgänge in einem Azure Cosmos-Container

In einem Azure Cosmos-Container werden die folgenden Vorgänge über die Azure Cosmos-APIs unterstützt.

| **Vorgang** | **Azure-Befehlszeilenschnittstelle** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Auflisten von Containern in einer Datenbank | Ja* | JA | Ja | JA | Nicht verfügbar | Nicht verfügbar |
| Lesen eines Containers | JA | Ja | Ja | JA | Nicht verfügbar | Nicht verfügbar |
| Erstellen eines neuen Containers | JA | Ja | Ja | JA | Nicht verfügbar | Nicht verfügbar |
| Aktualisieren eines Containers | JA | Ja | Ja | JA | Nicht verfügbar | Nicht verfügbar |
| Löschen eines Containers | JA | Ja | Ja | JA | Nicht verfügbar | Nicht verfügbar |

## <a name="azure-cosmos-items"></a>Azure Cosmos-Elemente

Abhängig von der API-Auswahl kann ein Azure Cosmos-Element ein Dokument in einer Sammlung, eine Zeile in einer Tabelle oder einen Knoten oder Edge in einem Graph darstellen. In der folgenden Tabelle ist die Zuordnung zwischen API-spezifischen Entitäten zu einem Azure Cosmos-Element dargestellt:

| **Cosmos-Entität** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos-Element | Dokument | Zeile | Dokument | Knoten oder Edge | Item |

### <a name="properties-of-an-item"></a>Eigenschaften eines Elements

Jedes Azure Cosmos-Element verfügt über die folgenden systemdefinierten Eigenschaften. Abhängig von der API-Auswahl werden einige möglicherweise nicht direkt verfügbar gemacht.

|**Systemdefinierte Eigenschaft** | **Systemgeneriert oder benutzerdefinierbar**| **Zweck** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|__id | Systemgeneriert | Eindeutiger Bezeichner eines Elements | JA | Nein  | Nein  | Nein  | Nein  |
|__etag | Systemgeneriert | Entitätstag, das zur Steuerung der optimistischen Nebenläufigkeit genutzt wird | JA | Nein  | Nein  | Nein  | Nein  |
|__ts | Systemgeneriert | Zuletzt aktualisierter Zeitstempel des Elements | JA | Nein  | Nein  | Nein  | Nein  |
|__self | Systemgeneriert | Adressierbarer URI des Elements | JA | Nein  | Nein  | Nein  | Nein  |
|id | Sie können das | Benutzerdefinierter eindeutiger Name innerhalb einer logischen Partition. Wenn der Benutzer die ID nicht angibt, wird im System automatisch eine generiert. | JA | Ja | Ja | Ja | JA |
|Beliebige benutzerdefinierte Eigenschaften | Benutzerdefiniert | Benutzerdefinierte Eigenschaften, die in einer API-nativen Darstellung (JSON, BSON, CQL usw.) dargestellt werden | JA | Ja | Ja | Ja | JA |

### <a name="operations-on-items"></a>Vorgänge in Entitäten

Azure Cosmos-Elemente unterstützen die folgenden Vorgänge, die über sämtliche Azure Cosmos-APIs durchgeführt werden können.

| **Vorgang** | **Azure-Befehlszeilenschnittstelle** | **SQL-API** | **Cassandra-API** | **MongoDB-API** | **Gremlin-API** | **Tabellen-API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Einfügen, Ersetzen, Löschen, Upsert, Lesen | Nein  | Ja | Ja | Ja | Ja | JA |

## <a name="next-steps"></a>Nächste Schritte

Sie können nun mit der Bereitstellung des Durchsatzes in einem Azure Cosmos-Konto fortfahren oder sich mit weiteren Konzepten vertraut machen:

* [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md)
* [Bereitstellen von Durchsatz für einen Azure Cosmos DB-Container](how-to-provision-container-throughput.md)
* [Logische Partitionen](partition-data.md)
* [Konfigurieren der Gültigkeitsdauer in Azure Cosmos-Containern](how-to-time-to-live.md)
* [Erstellen von reaktiven Anwendungen mit dem Änderungsfeed](change-feed.md)
* [Eindeutige Schlüssel in Azure Cosmos DB](unique-keys.md)

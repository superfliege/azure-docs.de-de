---
title: 'Partitionierung in Azure Cosmos DB: Gremlin-API | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie in Azure Cosmos DB einen partitionierten Graphen verwenden können.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: 7e9de68866b5e5849d0e48ad5073fc7b89fbb1ca
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239303"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Verwenden eines partitionierten Graphen in Azure Cosmos DB

Eines der Hauptmerkmale der Gremlin-API in Azure Cosmos DB ist die Fähigkeit, große Graphen mittels horizontaler Skalierbarkeit zu verarbeiten. Dieser Prozess erfolgt mithilfe der [Partitionierungsmöglichkeiten in Azure Cosmos DB](partition-data.md), bei denen auf Container zurückgegriffen wird, die in Bezug auf Speicherung und Durchsatz unabhängig skaliert werden können. Microsoft Azure Cosmos DB unterstützt die folgenden Containertypen in sämtlichen APIs:

- **Fester Container**: Diese Container können eine bis zu 10 GB große Graphdatenbank mit einem Maximum von 10.000 zugeordneten Anforderungseinheiten pro Sekunde speichern. Zum Erstellen eines festen Containers ist es nicht notwendig, eine Partitionsschlüsseleigenschaft in den Daten anzugeben.

- **Unbegrenzter Container**: Diese Container können automatisch so skaliert werden, dass ein mehr als 10 GB großer Graph mittels horizontaler Partitionierung gespeichert wird. Jede Partition speichert 10 GB, und die Daten werden automatisch auf Grundlage des **angegebenen Partitionsschlüssels** abgestimmt, der ein Pflichtparameter bei Verwendung eines unbegrenzten Containers ist. Dieser Containertyp kann eine praktisch unbegrenzte Datengröße speichern und bis zu 100.000 Anforderungseinheiten pro Sekunde ermöglichen. Wenn Sie mehr wünschen, [ kontaktieren Sie den Support](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

In diesem Dokument werden die Besonderheiten der Partitionierung von Graphendatenbanken und ihre Auswirkungen auf Knoten und Kanten beschrieben.

## <a name="requirements-for-partitioned-graph"></a>Anforderungen für einen partitionierten Graphen

Die folgenden Details sind zu beachten, wenn Sie einen Container mit einem partitionierten Graphen erstellen:

- **Die Einrichtung einer Partitionierung ist erforderlich**, wenn der Container voraussichtlich mehr als 10 GB groß ist bzw. wenn mehr als 10.000 Anforderungseinheiten pro Sekunde zugeordnet werden müssen.

- **Knoten und Kanten werden als JSON-Dokumente gespeichert**, und zwar im Back-End eines Azure Cosmos DB-Gremlin-API-Containers.

- **Knoten erfordern einen Partitionsschlüssel**. Dieser Schlüssel bestimmt, in welcher Partition der Knoten mithilfe eines Hashalgorithmus gespeichert wird. Der Name dieses Partitionsschlüssels ist eine Zeichenfolge mit einem Wort ohne Leer- und Sonderzeichen und wird beim Erstellen eines neuen Containers im Format `/partitioning-key-name` im Portal definiert.

- **Kanten werden mit ihren Quellknoten gespeichert**. Das heißt, dass für jeden Knoten dessen Partitionsschlüssel definiert, wo er zusammen mit seinen ausgehenden Kanten gespeichert wird. Dies erfolgt, um bei der Verwendung der Kardinalität `out()` in Graphenabfragen partitionsübergreifende Abfragen zu vermeiden.

- **Abfragen von Graphen müssen einen Partitionsschlüssel angeben**. Um die Vorteile der horizontalen Partitionierung in Azure Cosmos DB voll auszunutzen, muss bei Auswahl eines einzelnen Knotens nach Möglichkeit der Partitionsschlüssel angegeben werden. Es folgen Abfragen zur Auswahl eines oder mehrerer Knoten in einem partitionierten Graphen:

    - `/id` und `/label` werden als Partitionsschlüssel für einen Container in der Gremlin-API nicht unterstützt.


    - Auswählen eines Knotens anhand der ID und **befolgen des Schritts `.has()` zum Angeben der Partitionsschlüsseleigenschaft**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Auswählen eines Knotens durch **Angeben eines Tupels einschließlich Partitionsschlüssel und ID**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Angeben eines **Arrays von Tupeln mit Partitionsschlüsselwerten und IDs**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Auswählen einer Gruppe von Knoten und **Angeben einer Liste von Partitionsschlüsselwerten**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bewährte Methoden bei der Verwendung eines partitionierten Graphen

Die folgenden Richtlinien sollten befolgt werden, um eine möglichst effiziente Leistung und Skalierbarkeit bei der Verwendung partitionierter Graphen in unbegrenzten Containern sicherzustellen:

- **Geben Sie bei der Abfrage eines Knotens immer den Partitionsschlüsselwert an**. Das Abrufen eines Knotens aus einer bekannten Partition ist im Hinblick auf die Leistung am effizientesten.

- **Verwenden Sie beim Abfragen von Kanten nach Möglichkeit die Ausgangsrichtung**. Wie bereits erwähnt, werden Kanten mit ihren Quellknoten in Ausgangsrichtung gespeichert. Dies bedeutet, dass die Chancen eines Rückgriffs auf partitiongsübergreifende Abfragen minimiert werden, wenn die Daten und Abfragen unter Berücksichtigung dieses Musters entworfen werden.

- **Wählen Sie einen Partitionsschlüssel, der die Daten gleichmäßig auf die Partitionen verteilt**. Diese Entscheidung hängt stark vom Datenmodell der Lösung ab. Weitere Informationen zum Erstellen eines geeigneten Partitionsschlüssels finden Sie unter [Partitionierung und Skalierung in Azure Cosmos DB](partition-data.md).

- **Optimieren Sie Abfragen, um nach Möglichkeit Daten innerhalb der Grenzen einer Partition zu erhalten**. Eine optimale Partitionierungsstrategie ist auf die Abfragemuster abgestimmt. Abfragen, die Daten aus einer einzelnen Partition abrufen, bieten die bestmögliche Leistung.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Übersicht über Konzepte und bewährte Methoden für die Partitionierung mit einer Azure Cosmos DB-Gremlin-API erhalten. 

* Erfahren Sie mehr über das [Partitionieren und Skalieren in Azure Cosmos DB](partition-data.md).
* Erfahren Sie mehr über die [Gremlin-Unterstützung in der Gremlin-API](gremlin-support.md).
* Erfahren Sie mehr in der [Einführung in die Gremlin-API](graph-introduction.md).
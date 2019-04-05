---
title: Indextypen in Azure Cosmos DB
description: Übersicht über Indextypen in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/13/2019
ms.author: mjbrown
ms.openlocfilehash: 56c0fcb24ac5d255c6a36bcffd327df76f459963
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57990554"
---
# <a name="index-types-in-azure-cosmos-db"></a>Indextypen in Azure Cosmos DB

Beim Konfigurieren der Indizierungsrichtlinie für einen Pfad stehen Ihnen mehrere Optionen zur Verfügung. Für jeden Pfad können Sie eine oder mehrere Indizierungsdefinitionen angeben:

- **Datentyp:** „String“, „Number“, „Point“, „Polygon“ oder „LineString“ (kann nur einen Eintrag pro Datentyp pro Pfad enthalten).

- **Indexart:** „Range“ (Gleichheits-, Bereichs- oder ORDER BY-Abfragen) oder „Spatial“ (räumliche Abfragen).

- **Genauigkeit:** Für einen Range-Index beträgt der Wert der maximalen Genauigkeit „-1“. Dies ist gleichzeitig der Standardwert.

## <a name="index-kind"></a>Indexart

Azure Cosmos DB unterstützt den Range-Index für alle Pfade, die für die Datentypen „String“ und „Number“ konfiguriert werden können.

- **Index „Range“** unterstützt effiziente Gleichheitsabfragen, JOIN-Abfragen, Bereichsabfragen (mit >, <, >=, <= und !=) und ORDER BY-Abfragen. Bei ORDER BY-Abfragen muss standardmäßig auch die maximale Indexgenauigkeit (-1) angegeben werden. Der Datentyp kann „String“ oder „Number“ sein.

- **Index „Spatial“** unterstützt effiziente räumliche Abfragen zur Entfernung und zu enthaltenen Elementen. Der Datentyp kann „Point“, „Polygon“ oder „LineString“ sein. Azure Cosmos DB unterstützt auch die Indexart „Spatial“ für alle Pfade, die für den Datentyp „Point“, „Polygon“ oder „LineString“ angegeben werden können. Der Wert am angegebenen Pfad muss ein gültiges GeoJSON-Fragment sein, beispielsweise {"type": "Point", "coordinates": [0.0, 10.0]}. Azure Cosmos DB unterstützt die automatische Indizierung der Datentypen „Point“, „Polygon“ und „LineString“.

Im Folgenden finden Sie einige Beispiele, wie die Indizes „Range“ und „Spatial“ für die Verarbeitung genutzt werden können:

| **Indexart** | **Beschreibung/Anwendungsfall** |
| ---------- | ---------------- |
| Range      | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop<br><br>Range over /props/[]/? (oder / oder /props/) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5  |
| Spatial    | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECT FROM collection c WHERE ST_WITHIN(c.prop, {"type": "Point", ... }) – mit aktivierter Indizierung nach Punkten<br><br>SELECT FROM collection c WHERE ST_WITHIN({"type": "Polygon", ... }, c.prop) – mit aktivierter Indizierung nach Polygonen |

## <a name="default-behavior-of-index-kinds"></a>Standardverhalten von Indexarten

- Standardmäßig wird ein Fehler für Abfragen mit Bereichsoperatoren wie „>=“ zurückgegeben, wenn kein Bereichsindex (mit Genauigkeit) vorhanden ist, um zu signalisieren, dass möglicherweise eine Überprüfung erforderlich ist, um die Abfrage zu verarbeiten.

- Bereichsabfragen können ohne Bereichsindex mit dem Header "x-ms-documentdb-enable-scan" in der REST-API oder mithilfe der Anforderungsoption "EnableScanInQuery" des .NET SDK ausgeführt werden. Wenn die Abfrage weitere Filter enthält, die Azure Cosmos DB für den Index verwenden kann, wird kein Fehler zurückgegeben.

- Standardmäßig wird ein Fehler für räumliche Abfragen zurückgegeben, wenn kein räumlicher Index oder andere Filter vorhanden sind, für die die Verarbeitung über den Index möglich ist. Solche Abfragen können als Scan mit "x-ms-documentdb-enable-scan" oder "EnableScanInQuery" ausgeführt werden.

## <a name="index-precision"></a>Indexgenauigkeit

> [!NOTE]
> Azure Cosmos-Container unterstützen ein neues Indexlayout, das nur den Wert für die maximale Genauigkeit (-1) und keine andere benutzerdefinierte Indexgenauigkeit mehr erfordert. Mit dieser Methode werden die Pfade stets mit maximaler Genauigkeit indiziert. Wenn Sie in der Indizierungsrichtlinie einen Genauigkeitswert angeben, ignorieren die CRUD-Anfragen für den Container automatisch den Genauigkeitswert, und die Antwort vom Container enthält nur den Wert für die maximale Genauigkeit (-1).  Alle neuen Cosmos-Container verwenden standardmäßig das neue Indexlayout.

- Mit der Indexgenauigkeit können Sie Abstimmungen zwischen dem Indexspeicheraufwand und der Abfrageleistung vornehmen. Bei Zahlen wird die Verwendung der Standardkonfiguration für die Genauigkeit „-1“ (maximale Genauigkeit) empfohlen. Da Zahlen in JSON eine Größe von 8 Byte aufweisen, entspricht dies einer Konfiguration von 8 Byte. Die Auswahl eines niedrigeren Werts für die Genauigkeit, z. B. 1 bis 7, bedeutet, dass Werte in einigen Bereichen dem gleichen Indexeintrag zugeordnet werden. Daher reduziert sich der Indexspeicherplatz, bei der Abfrageausführung müssen jedoch möglicherweise mehr Elemente verarbeitet werden. Folglich erhöht sich der Durchsatz in Anforderungseinheiten.

- Die Indexgenauigkeit ist eher im Zusammenhang mit Zeichenfolgenbereichen hilfreich. Da Zeichenfolgen eine beliebige Länge aufweisen können, kann sich die Wahl der Indexgenauigkeit auf die Leistung von Abfragen von Zeichenfolgenbereichen auswirken. Außerdem kann sich dies auch auf den benötigten Indexspeicherplatz auswirken. Zeichenfolgenbereich-Indizes können mit einer Indexgenauigkeit zwischen 1 und 100 oder -1 (Maximum) konfiguriert werden. Wenn Sie ORDER BY-Abfragen für Zeichenfolgeneigenschaften ausführen möchten, müssen Sie eine Genauigkeit von „-1“ für die entsprechenden Pfade angeben.

- Räumliche Indizes verwenden stets die Standardindexgenauigkeit für alle Typen („Point“, „LineString“ und „Polygon“). Die Standardindexgenauigkeit für räumliche Indizes kann nicht überschrieben werden.

Azure Cosmos DB gibt einen Fehler zurück, wenn in einer Abfrage ORDER BY verwendet wird, aber kein Bereichsindex für den abgefragten Pfad mit maximaler Genauigkeit vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Indizierung in Azure Cosmos DB finden Sie in den folgenden Artikeln:

- [Übersicht über die Indizierung](index-overview.md)
- [Indizierungsrichtlinie](indexing-policies.md)
- [Indexpfade](index-paths.md)


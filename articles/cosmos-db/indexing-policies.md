---
title: Indizierungsrichtlinien für Azure Cosmos DB | Microsoft-Dokumentation
description: Erhalten Sie Informationen zur Funktionsweise der Indizierung in Azure Cosmos DB. In diesem Artikel wird das Konfigurieren und Ändern der Indizierungsrichtlinie zur automatischen Indizierung und zur Steigerung der Leistung erläutert.
keywords: Funktionsweise von Indizierung, automatische Indizierung, Indizierungsdatenbank
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 79585195cf95e2074a1c455c82faa500af20218a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618766"
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Unterstützen von Indexdaten durch Azure Cosmos DB

Standardmäßig werden alle Azure Cosmos DB-Daten indiziert. Zahlreiche Kunden lassen zwar gerne alle Aspekte der Indizierung von Azure Cosmos DB automatisch verwalten, aber Sie können in Azure Cosmos DB auch eine benutzerdefinierte *Indizierungsrichtlinie* für Sammlungen während der Erstellung angeben. Indizierungsrichtlinien in Azure Cosmos DB sind flexibler und leistungsfähiger als sekundäre Indizes, die auf anderen Datenbankplattformen angeboten werden. In Azure Cosmos DB können Sie die Form des Index entwerfen und anpassen, ohne Einbußen bei der Schemaflexibilität hinnehmen zu müssen. 

Um zu erfahren, wie die Indizierung in Azure Cosmos DB funktioniert, müssen Sie unbedingt verstehen, dass Sie beim Verwalten der Indizierungsrichtlinie differenzierte Kompromisse zwischen dem Indexspeicheraufwand, dem Schreib- und Abfragedurchsatz und der Abfragekonsistenz eingehen können.  

Im folgenden Video zeigt Azure Cosmos DB-Programm-Manager Andrew Liu die Azure Cosmos DB-Funktionen zur automatischen Indizierung, und wie Sie die Indizierungsrichtlinie auf Ihrem Azure Cosmos DB-Container optimieren und konfigurieren können. 

>[!VIDEO https://www.youtube.com/embed/uFu2D-GscG0]

In diesem Artikel befassen wir uns eingehend mit Azure Cosmos DB-Indizierungsrichtlinien, der Anpassung von Indizierungsrichtlinien und den zugehörigen Vor- und Nachteilen. 

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

* Wie kann ich angeben, welche Eigenschaften in die Indizierung einbezogen bzw. davon ausgeschlossen werden sollen?
* Wie kann ich den Index nach eventuellen Updates konfigurieren?
* Wie kann ich die Indizierung für die Ausführung von ORDER BY- oder Bereichsabfragen konfigurieren?
* Wie nehme ich Änderungen an der Indizierungsrichtlinie einer Sammlung vor?
* Wie vergleiche ich die Speicherung und Leistung verschiedener Indizierungsrichtlinien?

## Anpassen der Indizierungsrichtlinie einer Sammlung <a id="CustomizingIndexingPolicy"></a>  
Sie können die Kompromisse zwischen Speicherleistung, Schreib- und Abfrageleistung und Abfragekonsistenz durch Überschreiben der Standardindizierungsrichtlinie in einer Azure Cosmos DB-Sammlung anpassen. Sie können die folgenden Aspekte konfigurieren:

* **Ein- oder Ausschließen von Dokumenten und Pfaden in den/aus dem Index**: Sie können bestimmte Dokumente aus dem Index ausschließen oder in den Index einschließen, wenn Sie die Dokumente in die Sammlung einfügen oder ersetzen. Sie können auch bestimmte JSON-Eigenschaften, auch *Pfade* genannt, die für Dokumente indiziert werden sollen, die in einem Index enthalten sind, ein- oder ausschließen. Pfade enthalten Platzhaltermuster.
* **Konfigurieren verschiedener Indextypen**: Sie können für jeden eingeschlossenen Pfad den jeweils für eine Sammlung erforderlichen Indextyp angeben. Sie können den Indextyp basierend auf den Daten des Pfads, der erwarteten Abfragearbeitsauslastung und der numerischen/Zeichenfolgengenauigkeit angeben.
* **Konfigurieren von Indexaktualisierungsmodi**: Azure Cosmos DB unterstützt die drei Indizierungsmodi „Konsistent“, „Verzögert“ und „Keine“. Sie können die Indizierungsmodi über die Indizierungsrichtlinie in einer Azure Cosmos DB-Sammlung konfigurieren. 

Im folgenden Microsoft .NET-Codeausschnitt wird gezeigt, wie eine benutzerdefinierte Indizierungsrichtlinie beim Erstellen einer Sammlung festgelegt wird. In diesem Beispiel haben wir die Richtlinie mit einem Bereichsindex für Zeichenfolgen und Zahlen mit maximaler Genauigkeit festgelegt. Sie können diese Richtlinie verwenden, um ORDER BY-Abfragen für Zeichenfolgen auszuführen.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> Das JSON-Schema für die Indizierungsrichtlinie wurde mit der Veröffentlichung der REST-API-Version 2015-06-03 geändert. Ab dieser Version unterstützt das JSON-Schema für die Indizierungsrichtlinie Bereichsindizes für Zeichenfolgen. .NET SDK 1.2.0 und Java, Python und Node.js SDKs 1.1.0 unterstützen das neue Richtlinienschema. Frühere Versionen des SDKs verwenden die REST-API-Version 2015-04-08. Sie unterstützen das frühere Schema für die Indizierungsrichtlinie.
> 
> Standardmäßig indiziert Azure Cosmos DB alle Zeichenfolgeneigenschaften in Dokumenten konsistent mit einem Hashindex. Alle numerischen Eigenschaften in Dokumenten werden konsistent mit einem Bereichsindex indiziert.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Anpassen der Indizierungsrichtlinie im Portal

Sie können die Indizierungsrichtlinie einer Sammlung im Azure-Portal ändern: 

1. Wechseln Sie im Portal zu Ihrem Azure Cosmos DB-Konto, und wählen Sie dann Ihre Sammlung aus. 
2. Wählen Sie im linken Navigationsmenü die Option **Einstellungen** aus, und wählen Sie dann **Indizierungsrichtlinie** aus. 
3. Ändern Sie unter **Indizierungsrichtlinie** Ihre Indizierungsrichtlinie, und wählen Sie dann **OK** aus. 

### Datenbank-Indizierungsmodi <a id="indexing-modes"></a>  
Azure Cosmos DB unterstützt die folgenden drei Indizierungsmodi, die Sie über die Indizierungsrichtlinie in einer Azure Cosmos DB-Sammlung konfigurieren können: „Konsistent“, „Verzögert“ und „Keine“.

**Konsistent**: Wenn die Richtlinie einer Azure Cosmos DB-Sammlung „Konsistent“ ist, folgen die Abfragen für eine bestimmte Azure Cosmos DB-Sammlung derselben Konsistenzebene wie für die Punktlesevorgänge angegeben („stark“, „begrenzte Veraltung“, „Sitzung“ oder „letztlich“). Der Index wird im Rahmen der Aktualisierung des Dokuments (Einfügen, Ersetzen, Aktualisieren und Löschen eines Dokuments in einer Azure Cosmos DB-Sammlung) synchron aktualisiert.

Die konsistente Indizierung unterstützt konsistente Abfragen auf Kosten einer möglichen Verringerung des Schreibdurchsatzes. Diese Verringerung ist eine Funktion der eindeutigen Pfade, die indiziert werden müssen, und der „Konsistenzebene“. Der konsistente Indizierungsmodus ist für Workloads mit schnellem Schreiben und sofortigem Abfragen vorgesehen.

**Verzögert**: Der Index wird asynchron aktualisiert, wenn eine Azure Cosmos DB-Sammlung untätig ist, d. h. wenn die Durchsatzkapazität der Sammlung nicht vollständig mit dem Verarbeiten von Benutzeranforderungen ausgelastet ist.  Beachten Sie, dass Sie möglicherweise inkonsistente Ergebnisse erhalten, weil die Daten langsam erfasst und indiziert werden. Das bedeutet, dass Ihre COUNT-Abfragen oder bestimmte Abfrageergebnisse zu einem bestimmten Zeitpunkt möglicherweise nicht konsistent oder wiederholbar sind. 

Der Index befindet sich im Allgemeinen im Abgleichmodus mit erfassten Daten. Bei der verzögerten Indizierung führen Änderungen der Gültigkeitsdauer (Time to live, TTL) dazu, dass der Index gelöscht und neu erstellt wird. Dadurch werden die COUNT- und Abfrageergebnisse für einen bestimmten Zeitraum inkonsistent. Die meisten Azure Cosmos DB-Konten sollten den Indizierungsmodus „Konsistent“ verwenden.

**Keine**: Einer Sammlung mit dem Indexmodus „Keine“ ist kein Index zugeordnet. Diese Option wird häufig verwendet, wenn Azure Cosmos DB als Schlüssel-Wert-Speicher genutzt wird und auf Dokumente nur nach ihrer ID-Eigenschaft zugegriffen wird. 

> [!NOTE]
> Das Konfigurieren der Indizierungsrichtlinie mit „Keine“ hat den Nebeneffekt, dass alle vorhandenen Indizes gelöscht werden. Verwenden Sie diese Option, wenn Ihre Zugriffsmuster nur „ID“ oder „Self-Link“ erfordern.
> 
> 

Die folgende Tabelle zeigt die Konsistenz für Abfragen auf Grundlage des konfigurierten Indizierungsmodus (Konsistent und Verzögert) für die Sammlung und die für die Abfrageanforderung angegebene Konsistenzebene. Dies gilt für Abfragen, die über beliebige Schnittstellen (REST-API, SDKs) oder innerhalb von gespeicherten Prozeduren und Triggern erfolgen. 

|Konsistenz|Indizierungsmodus: Konsistent|Indizierungsmodus: Verzögert|
|---|---|---|
|STARK (Strong)|STARK (Strong)|Letztlich (Eventual)|
|Bounded staleness|Bounded staleness|Letztlich (Eventual)|
|Sitzung|Sitzung|Letztlich (Eventual)|
|Letztlich (Eventual)|Letztlich (Eventual)|Letztlich (Eventual)|

Azure Cosmos DB gibt einen Fehler für Abfragen zurück, die für Sammlungen mit dem Indizierungsmodus „Keine“ ausgeführt wurden. Abfragen können weiterhin als Scans über den expliziten Header **x-ms-documentdb-enable-scan** in der REST-API oder über die Anforderungsoption **EnableScanInQuery** mithilfe des .NET SDK ausgeführt werden. Einige Abfragefeatures wie ORDER BY werden als Scans mit **EnableScanInQuery** nicht unterstützt.

Der folgenden Tabelle können Sie die Konsistenz für Abfragen auf Grundlage des Indizierungsmodus („Konsistent“, „Verzögert“ und „Keine“) bei Angabe von **EnableScanInQuery** entnehmen.

|Konsistenz|Indizierungsmodus: konsistent|Indizierungsmodus: verzögert|Indizierungsmodus: keiner|
|---|---|---|---|
|STARK (Strong)|STARK (Strong)|Letztlich (Eventual)|STARK (Strong)|
|Bounded staleness|Bounded staleness|Letztlich (Eventual)|Bounded staleness|
|Sitzung|Sitzung|Letztlich (Eventual)|Sitzung|
|Letztlich (Eventual)|Letztlich (Eventual)|Letztlich (Eventual)|Letztlich (Eventual)|

Das folgende Codebeispiel zeigt die Erstellung einer Azure Cosmos DB-Sammlung mithilfe des .NET SDK mit konsistenter Indizierung bei jeder Dokumenteinfügung.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Indexpfade
Azure Cosmos DB modelliert JSON-Dokumente und den Index als Strukturen. Sie können Richtlinien für Pfade innerhalb der Struktur optimieren. Innerhalb von Dokumenten können Sie die Pfade auswählen, die in die Indizierung eingeschlossen oder von der Indizierung ausgeschlossen werden sollen. Dies kann bessere Schreibleistungen und geringeren Indexspeicherbedarf für Szenarien bieten, in denen die Abfragemuster im Voraus bekannt sind.

Indexpfade beginnen mit dem Stamm (/) und enden in der Regel mit dem Platzhalterzeichen "?", Platzhalteroperator. Dies bedeutet, dass es mehrere mögliche Werte für das Präfix gibt. Für SELECT * FROM Families F WHERE F.familyName = "Andersen" müssen Sie z. B. einen Indexpfad für /familyName/? in die Indexrichtlinie der Sammlung einbinden.

Indexpfade können auch das Platzhalterzeichen \* verwenden, um das Verhalten für Pfade rekursiv unter dem Präfix anzugeben. Beispielsweise kann „/payload/*“ dazu verwendet werden, um sämtliche Elemente unter der payload-Eigenschaft von der Indizierung auszuschließen.

Im Folgenden sind die allgemeinen Muster zum Angeben von Indexpfaden aufgeführt:

| path                | Beschreibung/Anwendungsfall                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Der Standardpfad für die Sammlung. Rekursiv und gilt für die gesamte Dokumentstruktur.                                                                                                                                                                                                                                   |
| /prop/?             | Zum Verarbeiten der nachfolgenden Abfragen erforderlicher Indexpfad (entsprechend mit Hash- oder Bereichstyp):<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop                                                                       |
| /prop/*             | Indexpfad für alle Pfade unter der angegebenen Bezeichnung. Funktioniert mit den folgenden Abfragen.<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5<br><br>SELECT FROM collection c WHERE c.prop.subprop.nextprop = "value"<br><br>SELECT FROM collection c ORDER BY c.prop         |
| /props/[]/?         | Indexpfad ist erforderlich für Iteration und JOIN-Abfragen für Arrays mit Skalaren wie ["a", "b", "c"]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag > 5                                                                         |
| /props/[]/subprop/? | Indexpfad ist erforderlich für Iteration und JOIN-Abfragen für Arrays mit Objekten wie [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag.subprop = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag.subprop = "value"                                  |
| /prop/subprop/?     | Zum Verarbeiten der folgenden Abfragen erforderlicher Indexpfad (entsprechend mit Hash- oder Bereichstyp):<br><br>SELECT FROM collection c WHERE c.prop.subprop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Beim Festlegen von benutzerdefinierten Indexpfaden müssen Sie die Standardindizierungsregel für die gesamte Dokumentstruktur angeben, die durch den speziellen Pfad „/*“ gekennzeichnet ist. 
> 
> 

Im folgenden Beispiel wird ein bestimmter Pfad mit Bereichsindex und einem benutzerdefinierten Genauigkeitswert von 20 Byte konfiguriert:

```
    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);
```

Wenn ein Pfad für die Indizierung hinzugefügt wird, werden Zahlen und Zeichenfolgen in diesen Pfaden indiziert. Obwohl Sie die Indizierung nur für Zeichenfolgen definieren, fügt Azure Cosmos DB also ebenfalls Standarddefinitionen für Zahlen hinzu. Azure Cosmos DB kann also Pfade aus der Indizierungsrichtlinie ausschließen, jedoch keine Typen aus einem bestimmten Pfad. Im Folgenden finden Sie ein Beispiel. Beachten Sie, dass nur ein Index für beide Pfade angegeben ist (Path =  "/*" and Path =  "/\"attr1\"/?"), der Datentyp „Number“ jedoch auch zum Ergebnis hinzugefügt wird.

```
var indices = new[]{
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 }// <- note: only 1 index specified
                    },
                    Path =  "/*"
                },
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 } // <- note: only 1 index specified
                    },
                    Path =  "/\"attr1\"/?"
                }
            };...

            foreach (var index in indices)
            {
                documentCollection.IndexingPolicy.IncludedPaths.Add(index);
            }
```

Ergebnis der Indexerstellung:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        },
        {
            "path": "/\"attr\"/?",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        }
    ],
}
```

### <a name="index-data-types-kinds-and-precisions"></a>Indexdatentypen, Indexarten und Indexgenauigkeiten
Beim Konfigurieren der Indizierungsrichtlinie für einen Pfad stehen Ihnen mehrere Optionen zur Verfügung. Für jeden Pfad können Sie eine oder mehrere Indizierungsdefinitionen angeben:

* **Datentyp**: „String“, „Number“, „Point“, „Polygon“ oder „LineString“ (kann nur einen Eintrag pro Datentyp pro Pfad enthalten).
* **Indexart**: „Hash“ (Gleichheitsabfragen), „Range“ (Gleichheits-, Bereichs- und ORDER BY-Abfragen) oder „Spatial“ (räumliche Abfragen).
* **Genauigkeit**: Beim Hashindex variiert dies von 1 bis 8 für Zeichenfolgen und Zahlen. Der Standardwert ist 3. Für einen Bereichsindex kann dieser Wert „-1“ (maximale Genauigkeit) sein. Für Zeichenfolgen- oder numerische Werte kann dieser zwischen 1 und 100 (maximale Genauigkeit) variieren.

#### <a name="index-kind"></a>Indexart
Azure Cosmos DB unterstützt die Indexarten „Hash“ und „Range“ für alle Pfade, die für die Datentypen „String“ und „Number“ konfiguriert werden können.

* **Hash** unterstützt effiziente Gleichheits- und JOIN-Abfragen. In den meisten Fällen benötigen Hashindizes keine höhere Genauigkeit als den Standardwert von 3 Byte. Der Datentyp kann „String“ oder „Number“ sein.
* **Range** unterstützt effiziente Gleichheitsabfragen, Bereichsabfragen (mit >, <, >=, <=, !=) und ORDER BY-Abfragen. Bei ORDER BY-Abfragen muss standardmäßig auch die maximale Indexgenauigkeit (-1) angegeben werden. Der Datentyp kann „String“ oder „Number“ sein.

Azure Cosmos DB unterstützt auch die Indexart „Spatial“ für alle Pfade, die für den Datentyp „Point“, „Polygon“ oder „LineString“ angegeben werden können. Der Wert im angegebenen Pfad muss ein gültiges GeoJSON-Fragment wie `{"type": "Point", "coordinates": [0.0, 10.0]}`sein.

* **Spatial** unterstützt effiziente räumliche Abfragen zur Entfernung und zu enthaltenen Elementen. Der Datentyp kann „Point“, „Polygon“ oder „LineString“ sein.

> [!NOTE]
> Azure Cosmos DB unterstützt die automatische Indizierung der Datentypen „Point“, „Polygon“ und „LineString“.
> 
> 

Hier sehen Sie die unterstützten Indexarten und Beispiele für Abfragen, für die sie verwendet werden können:

| Indexart | Beschreibung/Anwendungsfall                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>Hash über /props/[]/? (oder / oder /props/) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5                                                                                                                       |
| Range      | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop                                                                                                                                                                                                              |
| Spatial     | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c<br><br>WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECT FROM collection c WHERE ST_WITHIN(c.prop, {"type": "Polygon", ... }) --mit aktivierter Indizierung nach Punkten<br><br>SELECT FROM collection c WHERE ST_WITHIN({"type": "Point", ... }, c.prop) --mit aktivierter Indizierung nach Polygonen              |

Standardmäßig wird ein Fehler für Abfragen mit Bereichsoperatoren wie „>=“ zurückgegeben, wenn kein Bereichsindex (mit Genauigkeit) vorhanden ist, um zu signalisieren, dass möglicherweise eine Überprüfung erforderlich ist, um die Abfrage zu verarbeiten. Bereichsabfragen können ohne Bereichsindex mit dem Header **x-ms-documentdb-enable-scan** in der REST-API oder mithilfe der Anforderungsoption **EnableScanInQuery** des .NET SDK ausgeführt werden. Wenn die Abfrage weitere Filter enthält, die Azure Cosmos DB für den Index verwenden kann, wird kein Fehler zurückgegeben.

Die gleichen Regeln gelten für räumliche Abfragen. Standardmäßig wird ein Fehler für räumliche Abfragen zurückgegeben, wenn kein räumlicher Index vorhanden ist und keine anderen Filter vorhanden sind, für die die Verarbeitung über den Index möglich ist. Sie können als Scan mit **x-ms-documentdb-enable-scan** oder **EnableScanInQuery** ausgeführt werden.

#### <a name="index-precision"></a>Indexgenauigkeit
Mit der Indexgenauigkeit können Sie Abstimmungen zwischen dem Indexspeicheraufwand und der Abfrageleistung vornehmen. Bei Zahlen wird die Verwendung der Standardkonfiguration für die Genauigkeit „-1“ (maximale Genauigkeit) empfohlen. Da Zahlen in JSON eine Größe von 8 Byte aufweisen, entspricht dies einer Konfiguration von 8 Byte. Die Auswahl eines niedrigeren Werts für die Genauigkeit, z. B. 1 bis 7, bedeutet, dass Werte in einigen Bereichen dem gleichen Indexeintrag zugeordnet werden. Daher reduziert sich der Indexspeicherplatz, bei der Abfrageausführung müssen jedoch möglicherweise mehr Dokumente verarbeitet werden. Folglich erhöht sich der Durchsatz in Anforderungseinheiten.

Die Konfiguration der Indexgenauigkeit ist eher im Zusammenhang mit Zeichenfolgenbereichen hilfreich. Da Zeichenfolgen eine beliebige Länge aufweisen können, kann sich die Wahl der Indexgenauigkeit auf die Leistung von Abfragen von Zeichenfolgenbereichen auswirken. Außerdem kann sich dies auch auf den benötigten Indexspeicherplatz auswirken. Zeichenfolgenbereich-Indizes können mit einem Wert von 1 bis 100 oder „-1“ (maximale Genauigkeit) konfiguriert werden. Wenn Sie ORDER BY-Abfragen für Zeichenfolgeneigenschaften ausführen möchten, müssen Sie eine Genauigkeit von „-1“ für die entsprechenden Pfade angeben.

Räumliche Indizes verwenden stets die Standardindexgenauigkeit für alle Typen („Point“, „LineString“ und „Polygon“). Die Standardindexgenauigkeit für räumliche Indizes kann nicht überschrieben werden. 

Das folgende Beispiel zeigt, wie die Genauigkeit für Bereichsindizes in einer Sammlung mithilfe des .NET SDKs erhöht wird. 

**Erstellen einer Sammlung mit benutzerdefinierter Indexgenauigkeit**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB gibt einen Fehler zurück, wenn in einer Abfrage ORDER BY verwendet wird, aber kein Bereichsindex für den abgefragten Pfad mit maximaler Genauigkeit vorhanden ist. 
> 
> 

Auf ähnliche Weise können Sie Pfade vollständig von der Indizierung ausschließen. Im nächsten Beispiel wird gezeigt, wie Sie einen gesamten Abschnitt der Dokumente (eine *Unterstruktur*) mit dem Platzhalterzeichen \* von der Indizierung ausschließen.

    var excluded = new DocumentCollection { Id = "excludedPathCollection" };
    excluded.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    excluded.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Aktivieren und Deaktivieren der Indizierung
Sie können wählen, ob die Sammlung automatisch alle Dokumente indizieren soll. Standardmäßig werden alle Dokumente automatisch indiziert, aber Sie können die automatische Indizierung deaktivieren. Wenn die Indizierung deaktiviert ist, kann auf die Dokumente nur über ihre Self-Links oder über Abfragen mithilfe der Dokument-ID zugegriffen werden.

Mit deaktivierter automatischer Indizierung können Sie lediglich bestimmte Dokumente weiterhin zum Index hinzufügen. Umgekehrt können Sie die automatische Indizierung aktiviert lassen und nur bestimmte Dokumente einzeln ausschließen. Konfigurationen mit aktivierter/deaktivierter Indizierung sind hilfreich, wenn nur eine Teilmenge der Dokumente abgefragt werden muss.

Im folgenden Beispiel wird das explizite Einbeziehen eines Dokuments mithilfe des [SQL API .NET SDK](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) und der [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx)-Eigenschaft veranschaulicht.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Ändern der Indizierungsrichtlinie einer Sammlung
In Azure Cosmos DB können Sie die Indizierungsrichtlinie einer Sammlung bei Bedarf ändern. Eine Änderung der Indizierungsrichtlinie in einer Azure Cosmos DB-Sammlung kann zu einer Änderung der Form des Index führen. Die Änderung wirkt sich auf die indizierbaren Pfade, deren Genauigkeit und auf das Konsistenzmodell des Index aus. Eine Änderung der Indizierungsrichtlinie erfordert effektiv eine Transformation des alten Index in einen neuen Index. 

**Onlineindextransformationen**

![Funktionsweise der Indizierung – Azure Cosmos DB-Onlineindextransformationen](./media/indexing-policies/index-transformations.png)

Indextransformationen erfolgen online. Das bedeutet, dass die gemäß der alten Richtlinien indizierten Dokumente effizient gemäß der neuen Richtlinie transformiert werden, *ohne dass Auswirkungen auf die Schreibverfügbarkeit oder den bereitgestellten Durchsatz* der Sammlung entstehen. Die Indextransformation hat keine Auswirkungen auf die Konsistenz der Lese- und Schreibvorgänge, die mithilfe der REST-API, SDKs oder innerhalb von gespeicherten Prozeduren und Triggern erfolgen. 

Das Ändern der Indizierungsrichtlinie ist ein asynchroner Prozess, und die Zeit, die für die Durchführung des Vorgangs benötigt wird, hängt von der Anzahl von Dokumenten, den bereitgestellten RUs und der Größe der Dokumente ab. Während die Neuindizierung durchgeführt wird, gibt Ihre Abfrage ggf. nicht alle übereinstimmenden Ergebnisse zurück, wenn die Abfrage den Index verwendet, der geändert wird. Bei Abfragen werden dann keine Fehler bzw. Ausfälle zurückgegeben. Während der Neuindizierung werden Abfragen unabhängig von der Indizierungsmoduskonfiguration („Consistent“ oder „Lazy“) letztendlich konsistent durchgeführt. Nachdem die Indextransformation abgeschlossen ist, werden weiterhin konsistente Ergebnisse angezeigt. Dies gilt auch für Abfragen über beliebige Schnittstellen (REST-API, SDKs) und innerhalb von gespeicherten Prozeduren und Triggern. Wie die verzögerte Indizierung wird auch die Indextransformation asynchron im Hintergrund auf den Replikaten mithilfe der freien Ressourcen ausgeführt, die für ein bestimmtes Replikat verfügbar sind. 

Indextransformationen werden auch direkt vorgenommen. Azure Cosmos DB behält nicht zwei Kopien des Index bei, sondern tauscht den alten Index durch den neuen aus. Dies bedeutet, dass beim Ausführen von Indextransformationen in Ihren Sammlungen kein zusätzlicher Speicherplatz erforderlich ist oder belegt wird.

Wenn Sie die Indizierungsrichtlinie ändern, werden die Änderungen aus dem alten Index in den neuen Index in erster Linie auf der Grundlage der Konfigurationen des Indizierungsmodus übernommen. Die Konfigurationen des Indizierungsmodus spielen eine größere Rolle als andere Werte wie eingeschlossene/ausgeschlossene Pfade, Indexarten und Genauigkeiten. 

Wenn sowohl Ihre alte als auch die neue Richtlinie eine konsistente Indizierung verwendet, führt Azure Cosmos DB eine Onlineindextransformation aus. Während der Ausführung der Transformation können Sie keine weitere Änderung an der Indizierungsrichtlinie vornehmen, die den Indizierungsmodus „Konsistent“ aufweist. Sie können jedoch in den Indizierungsmodus „Verzögert“ oder „Keine“ wechseln, während eine Transformation ausgeführt wird: 

* Beim Wechsel zu „Verzögert“ erfolgt die Änderung der Indexrichtlinie effektiv sofort. Azure Cosmos DB beginnt mit dem erneuten asynchronen Erstellen des Index. 
* Wenn Sie zu „Keine“ wechseln, wird der Index sofort gelöscht. Der Wechseln zu „Keine“ ist nützlich, wenn Sie eine laufende Transformation abbrechen und mit einer anderen Indizierungsrichtlinie neu beginnen möchten. 

Der folgende Codeausschnitt zeigt die Änderung der Indizierungsrichtlinie einer Sammlung vom Indizierungsmodus „Konsistent“ in „Verzögert“. Wenn Sie das .NET SDK verwenden, können Sie die Änderung einer Indizierungsrichtlinie mit der neuen **ReplaceDocumentCollectionAsync**-Methode starten.

**Ändern der Indizierungsrichtlinie von „Konsistent“ in „Verzögert“**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Nachverfolgen des Fortschritts der Indextransformation**

Sie können den prozentualen Fortschritt der Indextransformation in einen konsistenten Index mit der **IndexTransformationProgress**-Antworteigenschaft eines **ReadDocumentCollectionAsync**-Aufrufs nachverfolgen. Andere SDKs und die REST-API unterstützen entsprechende Eigenschaften und Methoden, um Änderungen an Indizierungsrichtlinien vorzunehmen. Sie können den Fortschritt einer Indextransformation in einen konsistenten Index durch Aufrufen von **ReadDocumentCollectionAsync** überprüfen: 

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

> [!NOTE]
> * Die **IndexTransformationProgress**-Eigenschaft ist nur bei der Transformation in einen konsistenten Index anwendbar. Verwenden Sie die **ResourceResponse.LazyIndexingProgress**-Eigenschaft für die Nachverfolgung von Transformationen in einen verzögerten Index.
> * Die Eigenschaften **IndexTransformationProgress** und **LazyIndexingProgress** werden nur für nicht partitionierte Sammlungen aufgefüllt, d. h. Sammlungen, die ohne Partitionsschlüssel erstellt werden.
>

Sie können den Index für eine Sammlung verwerfen, indem Sie in den Indizierungsmodus „Keine“ wechseln. Dies kann ein operatives Tool sein, wenn Sie eine laufende Transformation abbrechen und dann sofort eine neue Transformation starten möchten.

**Löschen des Index für eine Sammlung**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Warum sollten Sie Änderungen an der Indizierungsrichtlinie Ihrer Azure Cosmos DB-Sammlungen vornehmen? Folgende Anwendungsfälle sind die gängigsten:

* Bereitstellung konsistenter Ergebnisse während des normalen Betriebs, aber Reduzierung auf den Indizierungsmodus „Verzögert“ bei Massendatenimporten
* Verwenden neuer Indizierungsfunktionen für Ihre aktuellen Azure Cosmos DB-Sammlungen. Sie können z. B. räumliche Abfragen verwenden, die die Indexart „Spatial“ erfordern, oder ORDER BY-/Zeichenfolgenbereich-Abfragen, die die Indexart „Range“ für Zeichenfolgen erfordern.
* Manuelles Auswählen der zu indizierenden Eigenschaften und Ändern dieser Eigenschaften im Lauf der Zeit
* Optimieren der Indizierungsgenauigkeit zur Verbesserung der Abfrageleistung oder zum Reduzieren der Speicherbelegung

> [!NOTE]
> Zum Ändern der Indizierungsrichtlinie mit **ReplaceDocumentCollectionAsync** benötigen Sie Version 1.3.0 oder eine höhere Version des .NET SDK.
> 
> Damit die Indextransformation erfolgreich abgeschlossen werden kann, müssen Sie sicherstellen, dass ausreichend freier Speicherplatz in der Sammlung verfügbar ist. Wenn das Speicherkontingent der Sammlung erreicht ist, wird die Indextransformation angehalten. Die Indextransformation wird automatisch fortgesetzt, sobald Speicherplatz verfügbar ist, z. B. wenn Sie einige Dokumente löschen.
> 
> 

## <a name="performance-tuning"></a>Leistungsoptimierung
Die SQL-APIs enthalten Informationen zu Leistungsindikatoren, z. B. dem genutzten Indexspeicher, und zu den Durchsatzkosten (Anforderungseinheiten) für jeden Vorgang. Sie können diese Informationen zum Vergleichen verschiedener Indizierungsrichtlinien und zum Optimieren der Leistung verwenden.

Um das Speicherkontingent und die Verwendung einer Sammlung zu überprüfen, führen Sie eine **HEAD**- oder **GET**-Anforderung für die Sammlungsressource aus. Überprüfen Sie dann die Header **x-ms-request-quota** und **x-ms-request-usage**. Im .NET SDK enthalten die Eigenschaften [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) und [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) in [ResourceResponse<T\>](http://msdn.microsoft.com/library/dn799209.aspx) diese entsprechenden Werte.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Um den Indizierungsaufwand für jeden Schreibvorgang (Erstellen, Aktualisieren oder Löschen) zu messen, überprüfen Sie den Header **x-ms-request-charge** (oder die entsprechende [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx)-Eigenschaft in [ResourceResponse<T\>](http://msdn.microsoft.com/library/dn799209.aspx) im .NET SDK). Dabei können Sie die Anzahl der Anforderungseinheiten messen, die von diesen Vorgängen verwendet werden.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Änderungen an der Spezifikation der Indizierungsrichtlinie
Am 7. Juli 2015 wurde mit der REST-API-Version 2015-06-03 eine Änderung im Schema für die Indizierungsrichtlinie eingeführt. Die entsprechenden Klassen in den SDK-Versionen verfügen über neue Implementierungen für das Schema. 

Die folgenden Änderungen wurden in die JSON-Spezifikation implementiert:

* Die Indizierungsrichtlinie unterstützt Bereichsindizes für Zeichenfolgen.
* Jeder Pfad kann mehrere Indexdefinitionen aufweisen. Er kann eine für jeden Datentyp aufweisen.
* Die Indizierungsgenauigkeit unterstützt 1 bis 8 bei Zahlen und 1 bis 100 bei Zeichenfolgen sowie den Wert „-1“ (maximale Genauigkeit).
* Pfadsegmente erfordern für die einzelnen Pfade keine doppelten Anführungszeichen als Escapezeichen. Sie können beispielsweise einen Pfad für **/title/?** hinzufügen, statt **/"title"/?** zu verwenden.
* Der Stammpfad für "alle Pfade" kann als **/\*** dargestellt werden (zusätzlich zu **/**).

Wenn in Ihrem Code Sammlungen mit einer benutzerdefinierten Indizierungsrichtlinie bereitgestellt werden, die mit Version 1.1.0 des .NET SDK oder einer früheren Version geschrieben wurden, müssen Sie Ihren Anwendungscode zum Verarbeiten dieser Änderungen ändern, um die Umstellung auf SDK-Version 1.2.0 durchzuführen. Wenn Sie keinen Code zur Konfiguration der Indizierungsrichtlinie nutzen oder weiterhin eine ältere SDK-Version verwenden möchten, sind keine Änderungen erforderlich.

Für einen praktischen Vergleich folgen ein Beispiel für eine benutzerdefinierte Indizierungsrichtlinie, die mit der REST-API-Version 2015-06-03 geschrieben wurde, und ein Beispiel der gleichen Indizierungsrichtlinie, die mit der früheren REST-API-Version 2015-04-08 geschrieben wurde.

**Aktuelle Indizierungsrichtlinie, JSON (REST-API-Version 2015-06-03)**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }


**Vorherige Indizierungsrichtlinie, JSON (REST-API-Version 2015-04-08)**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }


## <a name="next-steps"></a>Nächste Schritte
Nutzen Sie die folgenden Links, um Beispiele für die Verwaltung der Indizierungsrichtlinie und weitere Informationen zur Abfragesprache von Azure Cosmos DB zu finden:

* [Codebeispiele für die SQL-API-.NET-Indexverwaltung](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [SQL-API-REST-Vorgänge für Sammlungen](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [Abfrage mit SQL](sql-api-sql-query.md)


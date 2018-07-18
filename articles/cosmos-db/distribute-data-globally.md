---
title: Globale Verteilung von Daten mit Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie mehr über weltweite Georeplikation, Failover und Datenwiederherstellungen mithilfe der globalen Datenbanken von Azure Cosmos DB, einem global verwalteten Datenbankdienst, der mehrere Modelle unterstützt.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 4f548e180ca315013d5ca91118041cac2e622520
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611448"
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Wie werden Daten mit Azure Cosmos DB global verteilt?
Azure ist ubiquitär: Es ist in über 50 geografischen Regionen auf der ganzen Welt präsent und breitet sich kontinuierlich weiter aus. Die weltweite Präsenz von Azure ermöglicht den Entwicklern unter anderem die problemlose Erstellung, Bereitstellung und Verwaltung global verteilter Anwendungen. 

[Azure Cosmos DB](../cosmos-db/introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet sofort einsetzbare globale Verteilung, [flexible Skalierung von Durchsatz und Speicher](../cosmos-db/partition-data.md) weltweit, Wartezeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzmodelle](consistency-levels.md) sowie garantierte Hochverfügbarkeit, gestützt durch [branchenführende, umfassende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch alle Ihre Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- oder Indexverwaltung befassen müssen. Dieser Dienst für mehrere Datenmodelle unterstützt Dokumente, Schlüssel-Wert-Paare, Diagramme und spaltenbasierte Daten. Als ursprünglich aus der Cloud entstandener Dienst wurde Azure Cosmos DB von Grund auf für Mehrinstanzenfähigkeit und globale Verteilung konzipiert.


![Azure Cosmos DB-Sammlung, partitioniert und auf drei Regionen verteilt](./media/distribute-data-globally/global-apps.png)

**Ein einzelner Azure Cosmos DB-Container, partitioniert und auf mehrere Azure-Regionen verteilt**

Bei der Entwicklung von Azure Cosmos DB haben wir gelernt, dass die globale Verteilung nicht als Anhängsel behandelt werden darf. Und sie kann nicht einfach auf ein Datenbanksystem mit einem einzelnen Standort aufgesetzt werden. Die Funktionen einer global verteilten Datenbank gehen über die herkömmliche geografische Notfallwiederherstellung (Geographical Disaster Recovery, Geo-DR) von Datenbanken mit nur einem Standort hinaus. Bei Datenbanken mit nur einem Standort und Geo-DR-Funktion handelt es sich um eine echte Untermenge weltweit verteilter Datenbanken. 

Dank der sofort einsatzbereiten globalen Verteilung von Azure Cosmos DB müssen Entwickler kein eigenes Replikationsgerüst erstellen – weder in Form des Lambda-Musters (Beispiel: [AWS DynamoDB-Replikation](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) über das Datenbankprotokoll noch durch doppelte Schreibvorgänge in mehreren Regionen. Diese Verfahren werden *nicht* empfohlen, da ihre Korrektheit nicht gewährleistet werden kann und keine vernünftigen SLAs bereitgestellt werden können. 

Dieser Artikel enthält eine Übersicht über die Azure Cosmos DB-Funktionen für die globale Verteilung. Außerdem beschreiben wir hier das einzigartige Konzept von Azure Cosmos DB für die Bereitstellung umfassender SLAs. 

## <a id="EnableGlobalDistribution"></a>Aktivieren der sofort einsatzbereiten globalen Verteilung
Folgende Funktionen von Azure Cosmos DB ermöglichen die problemlose Erstellung weltweit verteilter Anwendungen. Diese Funktionen stehen über die ressourcenanbieterbasierten [REST-APIs](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) von Azure Cosmos DB sowie über das Azure-Portal zur Verfügung.

Sehen Sie sich das folgende Video an, um das schlüsselfertige Feature für die globale Verteilung in Azure Cosmos DB in Aktion zu sehen.

> [!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

### <a id="RegionalPresence"></a>Ubiquitäre regionale Präsenz 
Es werden stetig [neue Regionen](https://azure.microsoft.com/regions/) online geschaltet, um die geografische Präsenz von Azure zu erweitern. Azure Cosmos DB ist als *Basisdienst* in Azure klassifiziert und ist standardmäßig in allen neuen Azure-Regionen verfügbar. Dadurch können Sie Ihrem Azure Cosmos DB-Datenbankkonto eine geografische Region zuordnen, sobald die neue Azure-Region den Betrieb aufnimmt.

### <a id="UnlimitedRegionsPerAccount"></a>Zuordnen einer unbegrenzten Anzahl von Regionen zu Ihrem Azure Cosmos DB-Datenbankkonto
Mit Azure Cosmos DB können Sie Ihrem Azure Cosmos DB-Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. Mit Ausnahme von Geofencing-Einschränkungen (China, Deutschland...) gibt es hinsichtlich der Anzahl von Regionen, die Sie Ihrem Azure Cosmos DB-Datenbankkonto zuordnen können, keinerlei Einschränkungen. Die folgende Abbildung zeigt ein Datenbankkonto, das sich über 25 Azure-Regionen erstreckt.  

![Azure Cosmos DB-Datenbankkonto, das sich über 25 Azure-Regionen erstreckt](./media/distribute-data-globally/spanning-regions.png)

**Azure Cosmos DB-Datenbankkonto eines Mandanten, das sich über 25 Azure-Regionen erstreckt**


### <a id="PolicyBasedGeoFencing"></a>Richtlinienbasiertes Geofencing
Azure Cosmos DB ist zur Unterstützung von richtlinienbasiertem Geofencing konzipiert. Geofencing – eine wichtige Komponente zur Einhaltung von Einschränkungen in den Bereichen Datengovernance und Compliance – kann verhindern, dass Ihrem Konto eine bestimmte Region zugeordnet wird. Geofencing kann beispielsweise verwendet werden, um die globale Verteilung auf die Bereiche innerhalb einer unabhängigen Cloud (China, Deutschland...) oder innerhalb der Besteuerungsgrenzen einer Regierung (beispielsweise Australien) zu beschränken. Die Richtlinien werden mithilfe der Metadaten Ihres Azure-Abonnements gesteuert.

### <a id="DynamicallyAddRegions"></a>Dynamisches Hinzufügen und Entfernen von Regionen
Mit Azure Cosmos DB können Sie jederzeit Regionen zu Ihrem Datenbankkonto hinzufügen (zuordnen) oder daraus entfernen (Zuordnung aufheben), wie in der [vorherigen Abbildung](#UnlimitedRegionsPerAccount) zu sehen. Durch die parallele Replikation von Daten über Partitionen hinweg stellt Azure Cosmos DB sicher, dass eine neue Region innerhalb von 30 Minuten überall auf der Welt verfügbar ist (vorausgesetzt, Ihre Daten überschreiten nicht die Größe von 100 TB). 

### <a id="FailoverPriorities"></a>Failoverprioritäten
Mit Azure Cosmos DB können Sie bei Ausfällen die exakte Reihenfolge von regionalen Failovern steuern, indem Sie eine *Priorität* mehreren, dem Datenbankkonto zugeordneten Regionen zuordnen, wie in der folgenden Abbildung zu sehen. Azure Cosmos DB stellt sicher, dass bei der automatischen Failoversequenz die von Ihnen angegebene Prioritätenreihenfolge eingehalten wird. Weitere Informationen zu regionalen Failovern finden Sie unter [Automatisches regionales Failover für die Geschäftskontinuität in Azure Cosmos DB](regional-failover.md).


![Konfigurieren von Failoverprioritäten mit Azure Cosmos DB](./media/distribute-data-globally/failover-priorities.png)

**Ein Azure Cosmos DB-Mandant kann die Reihenfolge der Failoverpriorität (rechter Bereich) für Regionen konfigurieren, die einem Datenbankkonto zugeordnet sind.**

### <a id="ConsistencyLevels"></a>Mehrere klar definierte Konsistenzmodelle für global verteilte Datenbanken
Azure Cosmos DB unterstützt [mehrere klar definierte, intuitive und praktische Konsistenzmodelle](consistency-levels.md), die durch SLAs gesichert werden. In Abhängigkeit von der Workload/den Szenarien können Sie aus den verfügbaren Optionen ein bestimmtes Konsistenzmodell auswählen. 

### <a id="TunableConsistency"></a>Verschiedene Konsistenzeinstellungen für global replizierte Datenbanken
Mit Azure Cosmos DB können Sie die Standardkonsistenzauswahl programmgesteuert zur Laufzeit für einzelne Anforderungen außer Kraft setzen und lockern. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamisch konfigurierbare Lese- und Schreibregionen
Mit Azure Cosmos DB können Sie die (der Datenbank zugeordneten) Regionen für „Lesen“, „Schreiben“ oder „Lesen/Schreiben“ konfigurieren. 

### <a id="ElasticallyScaleThroughput"></a>Elastisches Skalieren des Durchsatzes für Azure-Regionen
Sie können einen Azure Cosmos DB-Container durch programmgesteuertes Bereitstellen eines Durchsatzes elastisch skalieren. Der Durchsatz wird auf alle Regionen angewendet, in denen der Azure Cosmos DB-Container verteilt wird.

### <a id="GeoLocalReadsAndWrites"></a>Geolokale Lese- und Schreibvorgänge
Der Hauptvorteil einer global verteilten Datenbank besteht in der niedrigen Wartezeit beim weltweiten Datenzugriff. Azure Cosmos DB bietet weltweit niedrige Wartezeiten für Lese- und Schreibvorgänge beim 99. Perzentil. Dabei wird sichergestellt, dass alle Lesevorgänge von der nächstgelegenen (lokale) Region bedient werden. Bei der Verarbeitung einer Leseanforderung wird das lokale Quorum für die Region verwendet, in der der Lesevorgang initiiert wurde. Gleiches gilt für Schreibvorgänge. Ein Schreibvorgang wird erst bestätigt, wenn die Mehrheit der Replikate den Schreibvorgang dauerhaft lokal committet hat, sodass keine Abhängigkeit von der Bestätigung der Schreibvorgänge durch Remotereplikate besteht. Anders ausgedrückt: Für das Replikationsprotokoll von Azure Cosmos DB wird davon ausgegangen, dass das Lese- und Schreibquorum immer lokal für die Region gilt, in der die Anforderung initiiert wurde.

### <a id="ManualFailover"></a>Manuelles Failover
Mit Azure Cosmos DB können Sie das Failover eines Datenbankkontos auslösen, um die Eigenschaften der *End-to-End-Verfügbarkeit* der gesamten Anwendung (außerhalb der Datenbank) zu überprüfen. Da sowohl die Sicherheits- und Verfügbarkeitseigenschaften der Fehlererkennung als auch die Wahl einer übergeordneten Instanz garantiert werden, garantiert Azure Cosmos DB, dass bei einem vom Mandanten manuell initiierten Failovervorgang *keine Daten verloren gehen*.

### <a id="AutomaticFailover"></a>Automatisches Failover
Azure Cosmos DB unterstützt automatisches Failover bei Ausfällen einer oder mehrerer Regionen. Während eines regionalen Failovers erfüllt Azure Cosmos DB weiterhin die SLAs für Leselatenz, Verfügbarkeit, Konsistenz und Durchsatz. Azure Cosmos DB bietet eine Obergrenze für die Dauer eines automatischen Failovervorgangs. Hierbei handelt es sich um das Zeitfenster für potenzielle Datenverluste während eines regionalen Ausfalls.

### <a id="GranularFailover"></a>Konzipiert für verschiedene Failovergranularitäten
Aktuell werden die automatischen und manuellen Failoverfunktionen mit der Granularität des Datenbankkontos verfügbar gemacht. Intern ist Azure Cosmos DB allerdings für ein präziseres *automatisches* Failover einer Datenbank, eines Containers oder sogar einer Partition (ein Container mit einer Reihe von Schlüsseln) konzipiert. 

### <a id="MultiHomingAPIs"></a>Multihosting in Azure Cosmos DB
Azure Cosmos DB ermöglicht die Interaktion mit einer Datenbank über *logische* (regionsunabhängige) oder *physische* (regionsspezifische) Endpunkte. Die Verwendung logischer Endpunkte ermöglicht transparentes Multihosting der Anwendung im Falle eines Failovers. Ein physischer Endpunkt bietet präzise Steuerungsmöglichkeiten für die Anwendung, um Lese- und Schreibvorgänge an bestimmte Regionen umzuleiten.

Informationen zum Konfigurieren der Leseeinstellungen für die [SQL-API](../cosmos-db/tutorial-global-distribution-sql-api.md), [Gremlin-API](../cosmos-db/tutorial-global-distribution-graph.md), [Tabellen-API](../cosmos-db/tutorial-global-distribution-table.md) und [MongoDB-API](../cosmos-db/tutorial-global-distribution-mongodb.md) finden Sie in diesen Artikeln.

### <a id="TransparentSchemaMigration"></a>Transparente und konsistente Migration von Datenbankschemas und Indizes 
Azure Cosmos DB ist vollkommen [schemaunabhängig](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Dank des einzigartigen Designs der Datenbank-Engine kann Azure Cosmos DB automatisch und synchron sämtliche erfasste Daten indizieren, ohne dass der Benutzer dafür ein Schema oder sekundäre Indizes bereitstellen muss. Dadurch können Sie Ihre global verteilte Anwendung schnell durchlaufen, ohne sich Gedanken über die Migration von Datenbankschemas oder Indizes zu machen oder mehrstufige Anwendungsrollouts mit Schemaänderungen zu koordinieren. Azure Cosmos DB garantiert, dass Änderungen, die Sie explizit an Indizierungsrichtlinien vornehmen, keine Beeinträchtigung der Leistung oder Verfügbarkeit zur Folge haben.  

### <a id="ComprehensiveSLAs"></a>Umfassende, über bloße Hochverfügbarkeit hinausgehende SLAs
Als global verteilter Datenbankdienst bietet Azure Cosmos DB klar definierte und umfassende SLAs für **Verfügbarkeit**, **Latenz**, **Durchsatz** und **Konsistenz** für die global skalierte Datenbank – unabhängig davon, wie viele Regionen der Datenbank zugeordnet sind.  

## <a id="LatencyGuarantees"></a>Latenzgarantien
Der Hauptvorteil eines global verteilten Datenbankdiensts wie Azure Cosmos DB besteht in der niedrigen Latenz beim weltweiten Zugriff auf Ihre Daten. Azure Cosmos DB garantiert eine niedrige Wartezeit beim 99. Perzentil für verschiedene Datenbankvorgänge. Das von Azure Cosmos DB verwendete Replikationsprotokoll sorgt dafür, dass die Datenbankvorgänge (sowohl Lese- als auch Schreibvorgänge) stets in der lokalen Region des Clients ausgeführt werden. Die Latenz-SLA von Azure Cosmos DB liegt garantiert beim 99. Perzentil für Lesevorgänge, (synchron) indizierte Schreibvorgänge und Abfragen für verschiedene Anforderungs- und Antwortgrößen. Die Latenzgarantien für Schreibvorgänge beinhalten permanente Mehrheitsquorum-Commits innerhalb der lokalen Region.

### <a id="LatencyAndConsistency"></a>Zusammenhang zwischen Latenz und Konsistenz 
Damit ein global verteilter Dienst eine starke Konsistenz in einem global verteilten Setup bietet, muss er die Schreibvorgänge synchron replizieren oder überregionale Lesevorgänge synchron durchführen. Die Lichtgeschwindigkeit und die Zuverlässigkeit des Fernnetzes erfordern eine starke Konsistenz, die zu längeren Wartezeiten und einer geringeren Verfügbarkeit von Datenbankvorgängen führt. Um also sowohl niedrige Wartezeiten beim 99. Perzentil und eine Verfügbarkeit von 99,99 Prozent für alle Konten mit einzelner Region sowie für alle Konten mit mehreren Regionen und gelockerter Konsistenz als auch eine Verfügbarkeit von 99,999 Prozent für alle Datenbankkonten mit mehreren Regionen bieten zu können, muss der Dienst eine asynchrone Replikation verwenden. Hierzu muss der Dienst wiederum [klar definierte, gelockerte Konsistenzmodelle](consistency-levels.md) bieten. Diese sollten im Idealfall zwischen starker Konsistenz (für garantierte niedrige Latenz und hohe Verfügbarkeit) und letztlicher Konsistenz (mit einem intuitiven Programmiermodell) liegen.

Azure Cosmos DB stellt sicher, dass kein Lesevorgang erforderlich ist, um Replikate in mehreren Regionen zu kontaktieren und eine spezifische garantierte Konsistenzebene zu erreichen. Analog dazu wird sichergestellt, dass ein Schreibvorgang beim regionsübergreifenden Replizieren der Daten nicht blockiert wird. (Mit anderen Worten: Schreibvorgänge werden synchron regionsübergreifend repliziert.) Für Datenbankkonten mit mehreren Regionen stehen sowohl starke als auch mehrere gelockerte Konsistenzebenen zur Verfügung. 

### <a id="LatencyAndAvailability"></a>Zusammenhang zwischen Latenz und Verfügbarkeit 
Latenz und Verfügbarkeit sind zwei Seiten einer Medaille. Im stabilen Zustand sprechen wir von der Latenz eines Vorgangs, bei Ausfällen und Netzwerkpartitionen verwenden wir den Begriff Verfügbarkeit. Für die Anwendung macht es keinen Unterschied, ob ein Datenbankvorgang langsam ausgeführt wird oder ob die Datenbank nicht verfügbar ist. 

Zur Unterscheidung zwischen hoher Latenz und Nichtverfügbarkeit bietet Azure Cosmos DB eine absolute Obergrenze für die Latenz verschiedener Datenbankvorgänge. Übersteigt die Dauer des Datenbankvorgangs die Obergrenze, gibt Azure Cosmos DB einen Timeoutfehler zurück. Die Verfügbarkeits-SLA von Azure Cosmos DB gewährleistet, dass die Timeouts für die Verfügbarkeits-SLA berücksichtigt werden. 

### <a id="LatencyAndThroughput"></a>Zusammenhang zwischen Latenz und Durchsatz
Bei Azure Cosmos DB müssen Sie sich nicht zwischen Latenz und Durchsatz entscheiden. Die Lösung erfüllt die SLA für Latenz beim 99. Perzentil und bietet gleichzeitig den von Ihnen bereitgestellten Durchsatz. 

## <a id="ConsistencyGuarantees"></a>Konsistenzgarantien
Das [starke Konsistenzmodell](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) ist zwar das Nonplusultra in Sachen Datenprogrammierbarkeit, es wird jedoch teuer durch hohe Wartezeit (im stabilen Zustand) bzw. durch geringere Verfügbarkeit (im Falle eines Ausfalls) erkauft. 

Azure Cosmos DB bietet ein klar definiertes Programmiermodell, sodass Sie sich mit der Konsistenz der replizierten Daten befassen können. Damit Sie auf einfache Weise global verteilte Anwendungen mit Multihosting erstellen können, sind die von Azure Cosmos DB verfügbar gemachten Konsistenzmodelle regionsunabhängig, und es spielt auch keine Rolle, in welcher Region die Lese- und Schreibvorgänge verarbeitet werden. 

Die Konsistenz-SLA von Azure Cosmos DB garantiert, dass 100 Prozent der Leseanforderungen die Konsistenzgarantie für das von Ihnen festgelegte Konsistenzmodell erfüllen (entweder auf Ebene des Datenbankkontos oder auf Anforderungsebene.) Bei einer Leseanforderung gilt die Konsistenz-SLA als erfüllt, wenn alle Konsistenzgarantien der Konsistenzebene erfüllt werden. Die folgende Tabelle enthält die Konsistenzgarantien für bestimmte Konsistenzmodelle von Azure Cosmos DB.

<table>
    <tr>
        <td><strong>Konsistenzmodell</strong></td>
        <td><strong>Konsistenzmerkmale</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
        <tr>
        <td>STARK (Strong)</td>
        <td>Linearisierbar</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td rowspan="3">Bounded staleness</td>
        <td>Monotones Lesen (innerhalb einer Region)</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Begrenzte Veraltung &lt; K,T</td>
        <td>100 %</td>
    </tr>
<tr>
        <td rowspan="3">Sitzung</td>
        <td>Lesen eigener Schreibvorgänge</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Monotones Lesen</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Konsistentes Präfix</td>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
</table>

**Konsistenzgarantien in Verbindung mit einem bestimmten Konsistenzmodell in Azure Cosmos DB**


### <a id="ConsistencyAndAvailability"></a>Zusammenhang zwischen Konsistenz und Verfügbarkeit
Das [Unmöglichkeitsergebnis](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) des [CAP-Theorems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) beweist, dass ein System im Falle eines Ausfalls nicht gleichzeitig verfügbar bleiben und linearisierbare Konsistenz bieten kann. Der Datenbankdienst kann entweder Konsistenz und Partitionen oder Verfügbarkeit und Partitionen bieten. Im ersten Fall wird die Verfügbarkeit zugunsten von linearisierbarer Konsistenz geopfert, im zweiten Fall wird die [linearisierbare Konsistenz](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) zugunsten der Verfügbarkeit aufgegeben. Azure Cosmos DB verstößt niemals gegen das angeforderte Konsistenzmodell und ist damit formal betrachtet ein System mit Konsistenz und Partitionen. In der Praxis ist Konsistenz allerdings keine Alles-oder-Nichts-Option: Es gibt mehrere klar definierte Konsistenzmodelle entlang des Konsistenzspektrums zwischen linearisierbarer und letztlicher Konsistenz. Azure Cosmos DB nutzt mehrere der gelockerten Konsistenzmodelle mit praktischer Anwendbarkeit und einer intuitiven Verwendung. Azure Cosmos DB bietet als Kompromiss zwischen Konsistenz und Verfügbarkeit [mehrere gelockerte, aber trotzdem klar definierte Konsistenzmodelle](consistency-levels.md) sowie eine Verfügbarkeit von 99,99 Prozent für alle Datenbankkonten mit einzelner Region und eine Leseverfügbarkeit von 99,999 Prozent für alle Datenbankkonten mit mehreren Regionen. 

### <a id="ConsistencyAndAvailability"></a>Zusammenhang zwischen Konsistenz und Latenz
Die umfassendere Variante des CAP-Theorems, auch als [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf) bezeichnet, berücksichtigt auch Kompromisse zwischen Latenz und Konsistenz in einem stabilen Zustand. Bei dieser Variante muss sich ein Datenbanksystem in einem stabilen Zustand zwischen Konsistenz und Latenz entscheiden. Mit mehreren gelockerten Konsistenzmodellen (unterstützt durch asynchrone Replikation und Quoren für lokale Lese- und Schreibvorgänge) stellt Azure Cosmos DB sicher, dass es sich bei allen Lese- und Schreibvorgängen um lokale Vorgänge für die Lese- bzw. Schreibregion handelt. Dadurch kann Azure Cosmos DB innerhalb der Region eine niedrige Latenz für die bestehenden Konsistenzmodelle garantieren.  

### <a id="ConsistencyAndThroughput"></a>Zusammenhang zwischen Konsistenz und Durchsatz
Da die Implementierung eines bestimmten Konsistenzmodells von der Wahl eines [Quorumstyps](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) abhängt, variiert auch der Durchsatz abhängig vom gewählten Konsistenzmodell. So entspricht beispielsweise in Azure Cosmos DB die RU-Gebühr für Lesevorgänge mit starker Konsistenz in etwa dem *Doppelten* der Gebühr für Lesevorgänge mit letztlicher Konsistenz. In diesem Fall müssen Sie doppelt so viele RUs bereitstellen, um den gleichen Durchsatz zu erzielen.


![Zusammenhang zwischen Konsistenz und Durchsatz](./media/distribute-data-globally/consistency-and-throughput.png)

**Zusammenhang zwischen Lesekapazitäten für einem bestimmten Konsistenzmodell in Azure Cosmos DB**

## <a id="ThroughputGuarantees"></a>Durchsatzgarantien 
Mit Azure Cosmos DB lässt sich der Durchsatz (ebenso wie der Speicher) elastisch, über eine beliebige Anzahl an Regionen und bedarfsgerecht skalieren. 

![Azure Cosmos DB – verteilte und partitionierte Sammlungen](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

**Ein einzelner Azure Cosmos DB-Container, der horizontal (über drei Ressourcenpartitionen innerhalb einer Region) partitioniert und dann global über drei Azure-Regionen verteilt ist**

Ein Azure Cosmos DB-Container wird auf der Grundlage von zwei Dimensionen (i) innerhalb einer Region und (ii) regionsübergreifend verteilt. Das geht so: 

* **Lokale Verteilung**: Innerhalb einer einzelnen Region wird ein Azure Cosmos DB-Container mithilfe von *Ressourcenpartitionen* horizontal hochskaliert. Jede Ressourcenpartition verwaltet einen Satz von Schlüsseln und ist stark konsistent und hochverfügbar, da sie physisch durch vier Replikate repräsentiert wird, die auch *Replikatgruppe* genannt werden. Zudem verwaltet sie die Zustandsautomatreplikation zwischen diesen Replikaten. Azure Cosmos DB ist ein vollständig ressourcengesteuertes System, in dem eine Ressourcenpartition für die Bereitstellung ihres Durchsatzanteils am Budget der zugeteilten Systemressourcen verantwortlich ist. Die Skalierung eines Azure Cosmos DB-Containers erfolgt für die Benutzer im Hintergrund. Azure Cosmos DB verwaltet die Ressourcenpartitionen, teilt sie auf und führt sie bei Bedarf zusammen, wenn sich die Speicher- und Durchsatzanforderungen ändern. 
* **Globale Verteilung**: Bei einer Datenbank für mehrere Regionen wird jede der Ressourcenpartitionen dann auf diese Regionen verteilt. Ressourcenpartitionen, die in verschiedenen Regionen den gleichen Satz von Schlüsseln besitzen, bilden eine *Partitionsgruppe*, wie in der [vorherigen Abbildung](#ThroughputGuarantees) zu sehen.  Ressourcenpartitionen innerhalb einer Partitionsgruppe werden mittels Zustandsautomatreplikation über mehrere Regionen hinweg koordiniert, die mit der Datenbank verbunden sind. Je nach konfigurierter Konsistenzebene werden die Ressourcenpartitionen innerhalb einer Partitionsgruppe dynamisch unter Verwendung verschiedener Topologien (Stern, Verkettung, Baumstruktur usw.) konfiguriert. 

Dank extrem reaktionsschneller Partitionsverwaltung, Lastenausgleich und strenger Ressourcengovernance können Sie den Durchsatz mit Azure Cosmos DB für eine Azure Cosmos DB-Sammlung elastisch über mehrere Azure-Regionen hinweg skalieren. Die Änderung des bereitgestellten Durchsatzes ist ein Runtimevorgang in Azure Cosmos DB. Ähnlich wie bei anderen Datenbankvorgängen garantiert Azure Cosmos DB die absolute Obergrenze der Wartezeit für Ihre Anforderung, den bereitgestellten Durchsatz zu ändern. Als Beispiel zeigt die folgende Abbildung den Container eines Kunden mit elastisch bereitgestelltem Durchsatz nach Bedarf (im Bereich zwischen einer Million und zehn Millionen Anforderungen/Sek. für zwei Regionen).

![Azure Cosmos DB – elastisch bereitgestellter Durchsatz](./media/distribute-data-globally/elastic-throughput.png)

**Container eines Kunden mit elastisch bereitgestelltem Durchsatz (variiert zwischen einer Million und zehn Millionen Anforderungen/Sek.)**

### <a id="ThroughputAndConsistency"></a>Zusammenhang zwischen Durchsatz und Konsistenz 
Eine identische Beschreibung finden Sie unter [Zusammenhang zwischen Konsistenz und Durchsatz](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Zusammenhang zwischen Durchsatz und Verfügbarkeit
Bei Änderungen am bereitgestellten Durchsatz hält Azure Cosmos DB seine Hochverfügbarkeit weiter aufrecht. Azure Cosmos DB bietet eine transparente Ressourcenpartitionsverwaltung (und führt Aufteilungs-, Zusammenführungs- und Klonvorgänge aus) und stellt sicher, dass die Vorgänge weder die Leistung noch die Verfügbarkeit beeinträchtigen, wenn die Anwendung den Durchsatz elastisch erhöht oder verringert. 

## <a id="AvailabilityGuarantees"></a>Verfügbarkeitsgarantien
Azure Cosmos DB bietet eine SLA mit einer Verfügbarkeit von 99,99 Prozent für alle Datenbankkonten mit einer einzelnen Region und für alle Konten mit mehreren Regionen und gelockerter Konsistenz sowie eine Verfügbarkeit von 99,999 Prozent für alle Datenbankkonten mit mehreren Regionen. Wie bereits weiter oben beschrieben beinhalten die Verfügbarkeitsgarantien von Azure Cosmos DB eine absolute Latenzobergrenze für jeden Vorgang auf Daten- und Steuerungsebene. Die Verfügbarkeitsgarantien sind unabhängig von der Anzahl von Regionen und der geografischen Entfernung zwischen Regionen. Verfügbarkeitsgarantien gelten unter Berücksichtigung von sowohl manueller als auch automatischer Failover. Azure Cosmos DB bietet transparente Multihosting-APIs, die sicherstellen, dass Ihre Anwendung logische Endpunkte verwendet und die Anforderungen im Falle eines Failovers transparent an eine neue Region weiterleiten kann. Ihre Anwendung muss bei einem regionalen Failover nicht erneut bereitgestellt werden, und die Verfügbarkeits-SLAs werden jederzeit eingehalten.

### <a id="AvailabilityAndConsistency"></a>Zusammenhang zwischen Verfügbarkeit, Konsistenz, Latenz und Durchsatz
Der Zusammenhang zwischen Verfügbarkeit, Konsistenz, Latenz und Durchsatz wird in den Abschnitten [Zusammenhang zwischen Konsistenz und Verfügbarkeit](#ConsistencyAndAvailability), [Zusammenhang zwischen Latenz und Verfügbarkeit](#LatencyAndAvailability) und [Zusammenhang zwischen Durchsatz und Verfügbarkeit](#ThroughputAndAvailability) beschrieben. 

## <a id="CustomerFacingSLAMetrics"></a>Kundenorientierte SLA-Metriken
Azure Cosmos DB macht die Durchsatz-, Latenz-, Konsistenz- und Verfügbarkeitsmetriken transparent verfügbar. Auf diese Metriken kann programmgesteuert und über das Azure-Portal zugegriffen werden, wie in der unten gezeigten Abbildung zu sehen. Mit Azure Application Insights können Sie außerdem Warnungen für verschiedene Schwellenwerte einrichten.
 

![Azure Cosmos DB – für Kunden sichtbare SLA-Metriken](./media/distribute-data-globally/customer-slas.png)

**Konsistenz-, Latenz-, Durchsatz- und Verfügbarkeitsmetriken sind transparent für jeden Mandanten verfügbar.**

## <a id="Next Steps"></a>Nächste Schritte
* Informationen zum Implementieren der globalen Replikation für Ihr Azure Cosmos DB-Konto über das Azure-Portal finden Sie unter [Ausführen einer globalen Azure Cosmos DB-Datenbankreplikation mit dem Azure-Portal](tutorial-global-distribution-sql-api.md).
* Informationen zum Implementieren von Architekturen mit mehreren Mastern mit Azure Cosmos DB finden Sie unter [Multi-master database architectures with Azure Cosmos DB](multi-region-writers.md) (Architekturen mit mehreren Mastern mit Azure Cosmos DB).
* Weitere Informationen zur Funktionsweise von automatischem und manuellem Failover in Azure Cosmos DB finden Sie unter [Regional Failovers in Azure Cosmos DB](regional-failover.md) (Regionale Failover in Azure Cosmos DB).

## <a id="References"></a>Referenzen
1. Eric Brewer. [Towards Robust Distributed Systems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP Twelve Years Later – How the rules have changed](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer&amp;#39;s Conjecture and Feasibility of Consistent, Available, Partition Tolerant Web Services](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Consistency Tradeoffs in Modern Distributed Database Systems Design](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor and Wool. [Load, Capacity and Availability in Quorum Systems](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy and Wing. [Lineralizability: A correctness condition for concurrent objects](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Azure Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/)

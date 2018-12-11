---
title: Einführung in Azure Cosmos DB
description: Hier finden Sie Informationen zu Azure Cosmos DB. Diese global verteilte Datenbank mit mehreren Modellen ist für geringe Wartezeiten, flexible Skalierbarkeit und Hochverfügbarkeit konzipiert und bietet native Unterstützung für NoSQL-Daten.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 04/08/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 89f5ae27e7ce3ec3155fd3fdbf42bb0f4322aa10
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835062"
---
# <a name="welcome-to-azure-cosmos-db"></a>Willkommen bei Azure Cosmos DB

Azure Cosmos DB ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Mit einem Klick ermöglicht Azure Cosmos DB Ihnen, Durchsatz und Speicher elastisch und unabhängig voneinander über eine beliebige Anzahl von geografischen Azure-Regionen hinweg zu skalieren. Azure Cosmos DB bietet Ihnen mit umfassenden [Vereinbarungen zum Servicelevel](https://aka.ms/acdbsla) (Service Level Agreements, SLAs) Durchsatz-, Wartezeit-, Verfügbarkeits- und Konsistenzgarantien wie kein anderer Datenbankdienst. Sie können [Azure Cosmos DB ohne Azure-Abonnement kostenlos und unverbindlich testen](https://azure.microsoft.com/try/cosmosdb/).

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer horizontaler Hochskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien.](./media/introduction/azure-cosmos-db.png)

## <a name="key-capabilities"></a>Wichtige Funktionen
Als global verteilter Datenbankdienst mit mehreren Modellen erleichtert Azure Cosmos DB die Erstellung skalierbarer, extrem reaktionsschneller Anwendungen auf globaler Ebene:

* **Globale, sofort einsatzbereite Verteilung**
    * Sie haben die Möglichkeit, Ihre Daten [auf Knopfdruck](tutorial-global-distribution-sql-api.md) auf beliebig viele [Azure-Regionen](https://azure.microsoft.com/regions/) zu [verteilen](distribute-data-globally.md). Dadurch können Sie Ihre Daten in der Nähe Ihrer Benutzer platzieren und eine möglichst kurze Wartezeit für Ihre Kunden gewährleisten. 
    * Durch die Multihosting-APIs von Azure Cosmos DB weiß die App immer, welche Region am nächsten ist, und kann Anforderungen an das nächstgelegene Rechenzentrum senden. Für all das sind keine Änderungen an der Konfiguration erforderlich. Sie legen einfach nur Ihre Schreibregion und beliebig viele Leseregionen fest. Alles andere erfolgt automatisch.
    * Wenn Sie Ihrer Azure Cosmos DB-Datenbank Regionen hinzufügen bzw. Regionen daraus entfernen, muss Ihre Anwendung nicht erneut bereitgestellt werden – dank der Funktion für Multihosting-APIs ist sie weiterhin hoch verfügbar.

* **Mehrere Datenmodelle und beliebte APIs für den Zugriff auf und die Abfrage von Daten**
    * Das ARS-basierte (Atom Record Sequence) Datenmodell, das Azure Cosmos DB zugrunde liegt, unterstützt von Haus aus mehrere Datenmodelle. Hierzu zählen unter anderem Modelle für Dokumente, Diagramme, Schlüssel-Wert-Paare, Tabellen und Spaltendaten.
    * APIs für die folgenden Datenmodelle werden über SDKs unterstützt, die in verschiedenen Sprachen verfügbar sind:
        * [SQL-API](sql-api-introduction.md): Ein schemaloses JSON-Datenbankmodul mit umfangreichen SQL-Abfragefunktionen
        * [MongoDB-API](mongodb-introduction.md): Eine extrem skalierbare *MongoDB-as-a-Service*-Lösung auf der Grundlage der Azure Cosmos DB-Plattform Kompatibel mit bestehenden MongoDB-Bibliotheken, -Treibern, -Tools und -Anwendungen
        * [Cassandra-API](cassandra-introduction.md): Eine global verteile Cassandra-as-a-Service-Lösung auf der Grundlage der Azure Cosmos DB-Plattform Kompatibel mit vorhandenen [Apache Cassandra](https://cassandra.apache.org/)-Bibliotheken, -Treibern, -Tools und -Anwendungen
        * [Gremlin-API:](graph-introduction.md) Ein vollständig verwalteter, horizontal skalierbarer Graphdatenbankdienst, der die Erstellung und Ausführung von Anwendungen mit stark vernetzten Datasets erleichtert, die Open Gremlin-APIs unterstützen (auf der Grundlage der [Apache TinkerPop-Spezifikation](http://tinkerpop.apache.org/), Apache Gremlin).
        * [Tabellen-API](table-introduction.md): Ein Schlüssel-Wert-Datenbankdienst zur Bereitstellung von Premiumfunktionen (beispielsweise automatische Indizierung, garantierte geringe Wartezeiten, globale Verteilung) für vorhandene Azure Table Storage-Anwendungen. App-Änderungen sind dabei nicht erforderlich.
        * Weitere Datenmodelle und APIs folgen in Kürze.

* **Elastische und unabhängige Skalierung von Durchsatz und Speicher, bedarfsorientiert und weltweit**
    * Skalieren Sie den Datenbankdurchsatz [sekundengenau](request-units.md), und nehmen Sie Änderungen vor, wann immer Sie möchten. 
    * Skalieren Sie die Speichergröße [transparent und automatisch](partition-data.md), um jederzeit Ihre Größenanforderungen zu erfüllen.

* **Erstellen extrem reaktionsschneller unternehmenskritischer Anwendungen**
    * Azure Cosmos DB garantiert seinen Kunden eine kurze End-to-End-Wartezeit im 99. Perzentil. 
    * Für ein typisches 1-KB-Element garantiert Cosmos DB eine End-to-End-Wartezeit bei Lesevorgängen von weniger als 10 ms und bei indizierten Schreibvorgängen von weniger als 10 ms im 99. Perzentil (innerhalb derselben Azure-Region). Die mittlere Wartezeit liegt deutlich niedriger (unter 5 ms).

* **Sicherstellen der AlwaysOn-Verfügbarkeit**
    * SLA mit einer Verfügbarkeit von 99,99 Prozent für alle Datenbankkonten mit einer einzelnen Region und eine Leseverfügbarkeit von 99,999 Prozent für alle Datenbankkonten mit mehreren Regionen
    * Nehmen Sie die Bereitstellung für höhere Verfügbarkeit und bessere Leistung in einer beliebigen Anzahl von [Azure-Regionen](https://azure.microsoft.com/regions) vor.
    * Legen Sie dynamisch Prioritäten für Regionen fest und [simulieren Sie den Ausfall](high-availability.md) einer oder mehrerer Regionen garantiert ohne Datenverluste, um die End-to-End-Verfügbarkeit für die gesamte App (über die Datenbank hinaus) zu testen. 

* **Korrektes Schreiben global verteilter Anwendungen**
    * Fünf klar definierte, praktische und intuitive [Konsistenzmodelle](consistency-levels.md) bieten ein Spektrum an hoher SQL-ähnlicher Konsistenz bis hin zu gelockerter NoSQL-ähnlicher letztlicher Konsistenz – und sämtliche Abstufungen dazwischen. 
  
* **Geld-zurück-Garantien**
    * Branchenführende, finanziell abgesicherte, umfassende [Vereinbarungen zum Servicelevel](https://aka.ms/acdbsla) (Service Level Agreements, SLAs) für Verfügbarkeit, Wartezeit, Durchsatz und Konsistenz Ihrer unternehmenskritischen Daten 

* **Keine Datenbankschema-/-indexverwaltung**
    * Schnelles Durchlaufen des Anwendungsschemas, ohne sich Gedanken über Datenbankschemas und/oder die Indexverwaltung zu machen
    * Die Datenbank-Engine von Azure Cosmos DB ist vollständig schemaunabhängig. Alle erfassten Daten werden automatisch und ohne Schema oder Indizes erfasst, was extrem schnelle Abfragen ermöglicht. 

* **Niedrige Betriebskosten**
    * Fünf- bis zehnmal kosteneffizienter als eine nicht verwaltete Lösung oder eine lokale NoSQL-Lösung.
    * Dreimal kostengünstiger als AWS DynamoDB oder Google Spanner

## <a name="capability-comparison"></a>Vergleich der Funktionen

Azure Cosmos DB kombiniert die besten Funktionen herkömmlicher relationaler und nicht relationaler Datenbanken.

| Funktionen | Relationale Datenbanken   | Nicht relationale Datenbanken (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Globale Verteilung | Nein  | Nein  | Ja. Sofort einsatzbereite Verteilung in über 30 Regionen mit Multihosting-APIs|
| Horizontale Skalierung | Nein  | JA | Ja. Speicher und Durchsatz können unabhängig voneinander skaliert werden. | 
| Wartezeitgarantien | Nein  | JA | Ja. 99 Prozent der Lesevorgänge in unter 10 ms und Schreibvorgänge in unter 10 ms | 
| Hochverfügbarkeit | Nein  | JA | Ja. Azure Cosmos DB ist immer aktiv (Always On), verfügt über klar definierte PACELC-Kompromisse und bietet Optionen für automatisches und manuelles Failover.|
| Datenmodell + API | Relational + SQL | API mit mehreren Modellen + OSS-API | API mit mehreren Modellen + SQL-API + OSS-API |
| SLAs | JA | Nein  | Ja. Umfassende SLAs für Wartezeit, Durchsatz, Konsistenz, Verfügbarkeit |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Lösungen, die von Azure-Cosmos-DB profitieren

Von der [garantierten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) Hochverfügbarkeit, dem hohen Durchsatz, der kurzen Wartezeit und der justierbaren Konsistenz von Azure Cosmos DB profitieren alle [Web-, Mobil-, Gaming- und IoT-Anwendungen](use-cases.md), die [global](distribute-data-globally.md) große Datenmengen und zahlreiche Lese- und Schreibvorgänge mit Antwortzeiten nahezu in Echtzeit für verschiedenste Daten bewältigen müssen. Erfahren Sie mehr darüber, wie Azure Cosmos DB für [IoT und Telematik](use-cases.md#iot-and-telematics), [Einzelhandel und Marketing](use-cases.md#retail-and-marketing), [Gaming](use-cases.md#gaming) sowie für [mobile Anwendungen und Webanwendungen](use-cases.md#web-and-mobile-applications) genutzt werden kann.

## <a name="next-steps"></a>Nächste Schritte
Erste Schritte mit Azure Cosmos DB mit einem unserer Schnellstarts:

* [Erste Schritte mit der SQL-API von Azure Cosmos DB](create-sql-api-dotnet.md)
* [Erste Schritte mit der MongoDB-API von Azure Cosmos DB](create-mongodb-nodejs.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](create-cassandra-dotnet.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](create-graph-dotnet.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)

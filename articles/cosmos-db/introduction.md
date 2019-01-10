---
title: Einführung in Azure Cosmos DB
description: Hier finden Sie Informationen zu Azure Cosmos DB. Diese global verteilte Datenbank mit mehreren Modellen ist für geringe Wartezeiten, flexible Skalierbarkeit und Hochverfügbarkeit konzipiert und bietet native Unterstützung für NoSQL-Daten.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 12/18/2018
ms.author: sngun
ms.openlocfilehash: b384bc51ac371ef75f5128c92f7e4b8d7f45ecc6
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034946"
---
# <a name="welcome-to-azure-cosmos-db"></a>Willkommen bei Azure Cosmos DB

Moderne Anwendungen erfordern eine hohe Reaktionsschnelligkeit und müssen immer online sein. Um eine geringe Latenz und Hochverfügbarkeit zu erreichen, müssen Instanzen dieser Anwendungen in Rechenzentren eingesetzt werden, die sich in der Nähe ihrer Benutzer befinden. Anwendungen müssen in Echtzeit auf große Nutzungsänderungen zu Spitzenzeiten reagieren, immer größere Datenmengen speichern und diese den Benutzern innerhalb von Millisekunden zur Verfügung stellen.

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Mit einem Klick ermöglicht Azure Cosmos DB Ihnen, Durchsatz und Speicher elastisch und unabhängig voneinander über eine beliebige Anzahl von geografischen Azure-Regionen hinweg zu skalieren. Sie können den Durchsatz und Speicherplatz elastisch skalieren und den schnellen Datenzugriff im einstelligen Millisekundenbereich über Ihre bevorzugte API mit SQL, MongoDB, Cassandra, Tables oder Gremlin nutzen. Azure Cosmos DB bietet Ihnen umfassende [Vereinbarungen zum Servicelevel](https://aka.ms/acdbsla) (Service Level Agreements, SLAs) mit Durchsatz-, Wartezeit-, Verfügbarkeits- und Konsistenzgarantien. Dies ist etwas, das kein anderer Datenbankdienst bereitstellen kann.

Sie können [Azure Cosmos DB ohne Azure-Abonnement kostenlos und unverbindlich testen](https://azure.microsoft.com/try/cosmosdb/).

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer horizontaler Hochskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien.](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Hauptvorteile

### <a name="turnkey-global-distribution"></a>Globale, sofort einsatzbereite Verteilung

Cosmos DB ermöglicht es Ihnen, hochgradig reaktive und hochverfügbare Anwendungen weltweit zu erstellen. Cosmos DB repliziert Ihre Daten transparent, wo auch immer sich Ihre Benutzer befinden, sodass Ihre Benutzer mit einem Replikat ihrer Daten interagieren können, das sich in ihrer Nähe befindet.

Cosmos DB ermöglicht es Ihnen, jederzeit mit einem Klick auf eine Schaltfläche Ihrem Cosmos-Konto eine der Azure-Regionen hinzuzufügen oder diese aus ihm zu entfernen. Cosmos DB repliziert Ihre Daten nahtlos in alle Regionen, die Ihrem Cosmos-Konto zugeordnet sind, während Ihre Anwendung dank der Multi-Homing-Funktionen des Diensts weiterhin hochverfügbar ist.

Weitere Informationen finden Sie im Artikel zur [globalen Verteilung](distribute-data-globally.md).

### <a name="always-on"></a>Always „On“

Aufgrund der engen Integration in die Azure-Infrastruktur und der [transparenten Multimasterreplikation](global-dist-under-the-hood.md) bietet Cosmos DB 99,999 % [Hochverfügbarkeit](high-availability.md) für Lese- und Schreibvorgänge. Cosmos DB bietet Ihnen auch die Möglichkeit, programmgesteuert (oder über das Portal) ein regionales Failover Ihres Cosmos-Kontos aufzurufen. Diese Funktion hilft Ihnen sicherzustellen, dass eine Cosmos-Datenbank zwar ein automatisches Failover durchführen kann, der Rest Ihrer Anwendung aber auch für ein Failover bei einer regionalen Katastrophe ausgelegt ist.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Flexible Skalierbarkeit für Durchsatz und Speicher weltweit

Cosmos DB wurde mit transparenter horizontaler Partitionierung und Multimasterreplikation konzipiert und bietet eine nie dagewesene elastische Skalierbarkeit für alle Ihre Schreib- und Lesevorgänge rund um den Globus. Sie können mit einem einzigen API-Aufruf von Tausenden auf Hunderte von Millionen Anforderungen/Sek. rund um den Globus zentral hochskalieren, wobei Sie nur für den von Ihnen benötigten Durchsatz (und Speicher) zahlen. Diese Funktionalität hilft Ihnen, unerwartete Belastungsspitzen in Ihren Workloads zu bewältigen, ohne für Spitzen übermäßig Kapazität vorhalten zu müssen. Weitere Informationen dazu finden Sie unter [Partitionierung in Cosmos DB](partitioning-overview.md), [Bereitgestellter Durchsatz für Container und Datenbanken](set-throughput.md) und [Globale Skalierung bereitgestellten Durchsatzes](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Weltweit garantiert niedrige Latenz im 99. Quantil

Mit Cosmos DB können Sie global nutzbare Anwendungen mit hoher Reaktionsfähigkeit erstellen. Dank des neuen Protokolls für die Multimasterreplikation und der [schreiboptimierten Datenbank-Engine](index-policy.md) ohne Latches garantiert Cosmos DB Latenzen von unter 10 ms sowohl für Lesevorgänge als auch für (indizierte) Schreibvorgänge auf der ganzen Welt im 99. Quantil. Diese Funktion ermöglicht eine nachhaltige Erfassung von Daten und blitzschnelle Abfragen für hochgradig reaktive Anwendungen.

### <a name="precisely-defined-multiple-consistency-choices"></a>Genau definiert, mehrere Konsistenzoptionen

Sie müssen keine [unbefriedigenden Kompromisse zwischen Konsistenz, Verfügbarkeit, Latenz und Programmierbarkeit](consistency-levels-tradeoffs.md) mehr eingehen. Das Cosmos DB-Protokoll für die Multimasterreplikation bietet [fünf klar definierte Konsistenzoptionen](consistency-levels.md) (starke Veraltung, begrenzte Veraltung, Präfixkonsistenz, Sitzungskonsistenz und letztliche Konsistenz) für ein intuitives Programmiermodell mit niedriger Latenz und Hochverfügbarkeit für Ihre global verteilte Anwendung.

### <a name="no-schema-or-index-management"></a>Keine Schema- oder Indexverwaltung

Die Synchronisierung von Datenbankschemas und -indizes mit dem Schema einer Anwendung ist bei global verteilten Anwendungen besonders problematisch. Dank Cosmos DB müssen Sie sich nicht länger mit Schemas oder Indizes befassen. Die Datenbank-Engine ist vollkommen unabhängig vom Schema.  Da keine Schema- und Indexverwaltung erforderlich ist, müssen Sie sich auch bei der Migration von Schemas keine Gedanken um Anwendungsausfallzeiten machen. Cosmos DB [indiziert automatisch alle Daten](index-policy.md), ohne dass ein Schema oder Index erforderlich ist, und unterstützt blitzschnelle Abfragen.

### <a name="battle-tested-database-service"></a>Kampferprobter Datenbankdienst

Cosmos DB ist ein Basisdienst für Azure. Seit fast einem Jahrzehnt wird Cosmos DB von vielen Microsoft-Produkten für unternehmenskritische Anwendungen auf globaler Ebene eingesetzt, darunter Skype, Xbox, Office 365, Azure und viele andere. Heute ist Cosmos DB einer der am schnellsten wachsenden Dienste in Azure, der von vielen externen Kunden und Anwendungen genutzt wird, die eine elastische Skalierung und/oder schlüsselfertige Multidatencenter-/Multiregion-, Multimasterreplikation für niedrige Latenzzeiten und hohe Verfügbarkeit von Lese- und Schreibvorgängen erfordern.

### <a name="ubiquitous-regional-presence"></a>Ubiquitäre regionale Präsenz

Cosmos DB ist in allen Azure-Regionen weltweit verfügbar, z.B. in mehr als 54 Regionen in der öffentlichen Cloud, in Azure China 21Vianet, Azure Deutschland, Azure Government und Azure Government for Department of Defense (DoD). Weitere Informationen finden Sie unter [Regionale Präsenz von Cosmos DB](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Standardmäßig sicher und einsatzbereit.

Cosmos DB ist für eine [Vielzahl von Konformitätsstandards](compliance.md) zertifiziert. Zusätzlich werden alle Daten in Cosmos DB im Ruhezustand und während der Übertragung verschlüsselt. Cosmos DB bietet Autorisierung auf Zeilenebene und erfüllt strenge Sicherheitsstandards.

### <a name="significant-tco-savings"></a>Erhebliche Einsparungen bei den Gesamtbetriebskosten

Da Cosmos DB ein vollständig verwalteter Dienst ist, müssen Sie nicht mehr komplexe Bereitstellungen in mehreren Datencentern und Upgrades Ihrer Datenbanksoftware verwalten und betreiben oder den Support, die Lizenzierung oder den Betrieb bezahlen. Weitere Informationen finden Sie unter [Optimieren von Kosten mit Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Branchenweit führende, umfassende SLAs

Cosmos DB ist der erste und einzige Dienst, der [branchenführende umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) bietet, die 99,999 % Hochverfügbarkeits-, Lese- und Schreiblatenzzeiten mit dem 99. Quantil, garantierten Durchsatz und Konsistenz umfassen.

### <a name="apache-spark--cosmos-db--operational-analytics-at-global-scale"></a>Apache Spark und Cosmos DB = operative Analysen auf globaler Ebene

Sie können [Spark](spark-connector.md) für Daten ausführen, die in Cosmos DB gespeichert sind. Diese Funktion ermöglicht es Ihnen, operative Analysen mit niedriger Latenz auf globaler Ebene durchzuführen, ohne die transaktionalen Workloads zu beeinträchtigen, die direkt mit Cosmos DB ausgeführt werden.

### <a name="develop-applications-for-cosmos-db-using-popular-nosql-apis"></a>Entwickeln von Anwendungen für Cosmos DB mit gängigen NoSQL-APIs

Cosmos DB bietet eine Auswahl an APIs zur Aktualisierung und Abfrage der in Ihrer Cosmos-Datenbank gespeicherten Daten. Standardmäßig können Sie [SQL verwenden](how-to-sql-query.md), um die Daten in Ihrer Cosmos-Datenbank zu aktualisieren und abzufragen.

Cosmos DB implementiert auch die Wire Protocols [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) und [Azure Table Storage](table-introduction.md) direkt im Dienst. Auf diese Weise können Sie Clienttreiber (und Tools) für die häufig verwendeten NoSQL-APIs direkt auf Ihre Cosmos-Datenbank verweisen. Durch die Unterstützung von Wire Protocols der häufig verwendeten NoSQL-APIs ermöglicht Cosmos DB Folgendes:

* Einfaches Migrieren Ihrer Anwendung zu Cosmos DB unter Beibehaltung wesentlicher Teile Ihrer Anwendungslogik.
* Bewahren der Portierbarkeit Ihrer Anwendung und Unabhängigkeit von Cloudanbietern.
* Nutzen von branchenführenden, finanziell abgesicherten SLAs für die gängigen NoSQL-APIs. 
* Elastischen Skalieren des bereitgestellten Durchsatzes und Speichers für Ihre Datenbanken basierend auf Ihrem Bedarf und Bezahlung nur für den Durchsatz und den Speicher, den Sie benötigen. Dies führt zu erheblichen Kosteneinsparungen.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Lösungen, die von Azure-Cosmos-DB profitieren

Von der [garantierten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) Hochverfügbarkeit, dem hohen Durchsatz, der kurzen Wartezeit und der justierbaren Konsistenz von Azure Cosmos DB profitieren alle [Web-, Mobil-, Gaming- und IoT-Anwendungen](use-cases.md), die [global](distribute-data-globally.md) große Datenmengen und zahlreiche Lese- und Schreibvorgänge mit Antwortzeiten nahezu in Echtzeit für verschiedenste Daten bewältigen müssen. Erfahren Sie mehr darüber, wie Azure Cosmos DB für [IoT und Telematik](use-cases.md#iot-and-telematics), [Einzelhandel und Marketing](use-cases.md#retail-and-marketing), [Gaming](use-cases.md#gaming) sowie für [mobile Anwendungen und Webanwendungen](use-cases.md#web-and-mobile-applications) genutzt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [globale Verteilung](distribute-data-globally.md) von Cosmos DB und die [Partitionierungsfunktionen](partitioning-overview.md).

Erste Schritte mit Azure Cosmos DB mit einem unserer Schnellstarts:

* [Erste Schritte mit der SQL-API von Azure Cosmos DB](create-sql-api-dotnet.md)
* [Erste Schritte mit der Azure Cosmos DB-API für MongoDB](create-mongodb-nodejs.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](create-cassandra-dotnet.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](create-graph-dotnet.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)

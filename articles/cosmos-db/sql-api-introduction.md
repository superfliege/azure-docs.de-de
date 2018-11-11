---
title: 'Azure Cosmos DB: Einführung in die SQL-API | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie Azure Cosmos DB zum Speichern und Abfragen von sehr großen Mengen von JSON-Dokumenten mit geringer Wartezeit mithilfe von SQL und JavaScript verwenden können.
keywords: JSON-Datenbank, Dokumentendatenbank
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: overview
ms.date: 05/22/2017
ms.author: rafats
ms.openlocfilehash: f71a7ad538e22359a2a0dd9c2e3f6c4448ee5a2d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232392"
---
# <a name="introduction-to-azure-cosmos-db-sql-api"></a>Einführung in Azure Cosmos DB: SQL-API

[Azure Cosmos DB](introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](consistency-levels.md) sowie garantierte Hochverfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- und Indexverwaltung befassen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüssel-Werte, Diagramme und spaltenorientierte Datenmodelle.

![Azure SQL-API](./media/sql-api-introduction/cosmosdb-sql-api.png) 

Mit der SQL-API stellt Azure Cosmos DB umfassende und vertraute [SQL-Abfragefunktionen](sql-api-sql-query.md) mit einheitlich geringen Wartezeiten für schemalose JSON-Daten bereit. Dieser Artikel enthält eine Übersicht über die SQL-API von Azure Cosmos DB. Außerdem wird beschrieben, wie Sie sie zum Speichern von sehr großen Mengen von JSON-Daten und Abfragen mit einer Wartezeit im Millisekundenbereich verwenden und das Schema leicht weiterentwickeln können. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Welche Funktionen und wichtigen Features sind in Azure Cosmos DB enthalten?
Azure Cosmos DB verfügt über die SQL-API über die folgenden wichtigen Funktionen und Vorteile:

* **Flexibel skalierbarer Durchsatz und Speicher**: Die JSON-Datenbank lässt sich problemlos zentral hoch- und herunterskalieren, um Ihren Anwendungsanforderungen gerecht zu werden. Ihre Daten werden zur Erzielung geringer, planbarer Wartezeiten auf SSDs gespeichert. Azure Cosmos DB unterstützt Container zum Speichern von JSON-Daten, die als Sammlungen bezeichnet werden. Diese Container können auf nahezu unbegrenzte Speichergrößen und Mengen an bereitgestelltem Durchsatz skaliert werden. Sie können Azure Cosmos DB mit vorhersagbarer Leistung flexibel skalieren, wenn Ihre Anwendung wächst. 


* **Replikation in mehreren Regionen:** Azure Cosmos DB repliziert Ihre Daten auf transparente Weise in allen Regionen, die Sie Ihrem Azure Cosmos DB-Konto zugeordnet haben. Sie können Anwendungen entwickeln, die globalen Zugriff auf Daten benötigen, und dabei für ein ausgewogenes Maß an Konsistenz, Verfügbarkeit und Leistung sorgen – jeweils mit den entsprechenden Garantien. Azure Cosmos DB bietet transparentes regionales Failover mit Multihosting-APIs sowie die Möglichkeit, Durchsatz und Speicher für alle Konten weltweit flexibel zu skalieren. Weitere Informationen finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

* **Ad-hoc-Abfragen mit vertrauter SQL-Syntax**: Heterogene JSON-Dokumente können gespeichert und mithilfe einer vertrauten SQL-Syntax abgefragt werden. Azure Cosmos DB nutzt eine sperrfreie, protokollstrukturierte Indizierungstechnologie für die gleichzeitige Ausführung zahlreicher Vorgänge, um sämtliche Dokumentinhalte automatisch zu indizieren. Dies ermöglicht umfassende Echtzeitabfragen, ohne Schemahinweise, Sekundärindizes oder Sichten angeben zu müssen. Weitere Informationen finden Sie unter [Abfragen von Azure Cosmos DB](sql-api-sql-query.md). 
* **JavaScript-Ausführung innerhalb der Datenbank:** Anwendungslogik kann mithilfe von Standard-JavaScript als gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen ausgedrückt werden. Auf diese Weise kann die Anwendungslogik auf Daten angewendet werden, ohne dass Sie sich Gedanken über den Konflikt zwischen Anwendung und Datenbankschema machen müssen. Die SQL-API bietet eine umfassende transaktionale Ausführung von JavaScript-Anwendungslogik direkt in der Datenbank-Engine. Die enge Integration von JavaScript ermöglicht es, Einfüge-, Ersetzungs-, Lösch- und Auswahlvorgänge über ein JavaScript-Programm als isolierte Transaktion auszuführen. Weitere Einzelheiten finden Sie unter [Serverseitige SQL-Programmierung](programming.md).

* **Einstellbare Konsistenzebenen:** Die Konsistenz kann über fünf wohl definierte Ebenen abgestimmt werden, um für ein ausgewogenes Verhältnis zwischen Konsistenz und Leistung zu sorgen. Für Abfragen und Lesevorgänge bietet Azure Cosmos DB fünf verschiedene Konsistenzebenen – „stark“, „begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „letztlich“. Mit diesen granularen, gut abgegrenzten Konsistenzebenen können fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz geschlossen werden. Weitere Informationen finden Sie unter [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung](consistency-levels.md).

* **Vollständig verwaltet**: Sie müssen sich nicht mehr mit der Verwaltung von Datenbanken und Rechenressourcen befassen. Dank des vollständig verwalteten Microsoft Azure-Diensts bleiben Ihnen die Verwaltung virtueller Computer, die Bereitstellung und Konfiguration von Software, die Skalierung oder komplexe Datentarifupgrades erspart. Alle Datenbanken werden automatisch gesichert und vor regionalen Ausfällen geschützt. Sie können einfach Azure Cosmos DB-Konten hinzufügen und nach Bedarf Kapazitäten bereitstellen. Dies ermöglicht es Ihnen, sich auf Ihre Anwendung zu konzentrieren, ohne sich mit dem Betrieb und der Verwaltung der Datenbank aufhalten zu müssen. 

* **Offen konzipiert:** Mithilfe Ihrer vorhandenen Kenntnisse und Tools finden Sie ganz schnell den Einstieg. Die Programmierung mit der SQL-API ist einfach und verständlich und erfordert keine neuen Tools oder benutzerdefinierten Erweiterungen für JSON oder JavaScript. Sie können auf sämtliche Datenbankfunktionen – einschließlich CRUD-, Abfrage- und JavaScript-Verarbeitung – über eine einfache HTTP-basierte RESTful-Schnittstelle zugreifen. Die SQL-API bezieht vorhandene Formate, Sprachen und Standards ein und bietet gleichzeitig wichtige, darauf aufbauende Datenbankfunktionen.

* **Automatische Indizierung**: Alle Dokumente in der Datenbank werden von Azure Cosmos DB automatisch indiziert, ohne dass ein Schema oder die Erstellung sekundärer Indizes erwartet oder angefordert wird. Ist keine Indizierung aller Elemente gewünscht? Keine Sorge. Sie können [in Ihren JSON-Dateien auch Pfade ausschließen](indexing-policies.md).

* **Unterstützung von Änderungsfeeds:** Ein Änderungsfeed bietet eine Liste von Dokumenten in einer Azure Cosmos DB-Sammlung, die in der Reihenfolge sortiert ist, in der die Dokumente geändert wurden. Dieser Feed kann verwendet werden, um die Sammlung auf Änderungen an Daten zu überwachen und Daten zu replizieren, API-Aufrufe auszulösen oder die Streamverarbeitung von Updates auszuführen. Der Änderungsfeed ist automatisch aktiviert und einfach zu verwenden: [Weitere Informationen zum Änderungsfeed](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Wie verwalten Sie Daten mit der SQL-API?
Die SQL-API ermöglicht die Verwaltung von JSON-Daten mit klar definierten Datenbankressourcen. Diese Ressourcen werden werden repliziert, um Hochverfügbarkeit zu bieten, und sind anhand ihres logischen URI eindeutig adressierbar. Die SQL-API bietet ein einfaches HTTP-basiertes RESTful-Programmiermodell für alle Ressourcen. 


Ein Azure Cosmos DB-Datenbankkonto ist ein eindeutiger Namespace, über den Sie Zugriff auf Azure Cosmos DB erhalten. Um ein Datenbankkonto erstellen zu können, müssen Sie über ein Azure-Abonnement verfügen. Dadurch erhalten Sie Zugriff auf eine Vielzahl von Azure-Diensten. 

Alle Ressourcen in Azure Cosmos DB werden als JSON-Dokumente modelliert und gespeichert. Ressourcen werden als Elemente verwaltet, bei denen es sich um JSON-Dokumente mit Metadaten handelt, und als Feeds, bei denen es sich um Auflistungen von Elementen handelt. Gruppen von Elementen sind in ihren jeweiligen Feeds enthalten.

Die folgende Abbildung veranschaulicht die Beziehung zwischen den Azure Cosmos DB-Ressourcen:

![Hierarchische Beziehung zwischen Ressourcen in Azure Cosmos DB][1] 

Ein Datenbankkonto besteht aus einer Reihe von Datenbanken, die jeweils mehrere Auflistungen umfassen, die wiederum jeweils gespeicherte Prozeduren, Trigger, UDFs, Dokumente und zugehörige Anhänge enthalten können. Einer Datenbank sind zudem Benutzer zugeordnet, die jeweils über eine Reihe von Berechtigungen für den Zugriff auf verschiedene andere Auflistungen, gespeicherte Prozeduren, Trigger, UDFs, Dokumente oder Anhänge verfügen. Während es sich bei Datenbanken, Benutzern, Berechtigungen und Auflistungen um systemdefinierte Ressourcen mit bekannten Schemas handelt, enthalten Dokumente, gespeicherte Prozeduren, Trigger, UDFs und Anhänge beliebige benutzerdefinierte JSON-Inhalte.  

## <a name="develop"></a> Wie kann ich mit der SQL-API Apps entwickeln?

Mit Azure Cosmos DB werden Ressourcen über die REST-APIs verfügbar gemacht, die von allen Sprachen aufgerufen werden können, mit denen HTTP/HTTPS-Anforderungen möglich sind. Darüber hinaus bieten wir Programmierbibliotheken für mehrere beliebte Sprachen für die SQL-API an. Die Clientbibliotheken vereinfachen viele Aspekte der Arbeit mit der API, da Details wie Zwischenspeicherung von Adressen, Ausnahmeverwaltung und automatische Wiederholungsversuche verarbeitet werden können. Bibliotheken sind aktuell für die folgenden Sprachen und Plattformen erhältlich:  

| Download | Dokumentation |
| --- | --- |
| [.NET SDK](https://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-Referenzdokumentation](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Java SDK](https://go.microsoft.com/fwlink/?LinkID=402380) |[Java-Referenzdokumentation](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](https://www.npmjs.com/package/@azure/cosmos) |[JavaScript-Referenzdokumentation](https://docs.microsoft.com/javascript/api/@azure/cosmos/?view=azure-node-latest) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-Referenzdokumentation](https://github.com/Azure/azure-cosmos-python) |

Durch Verwendung des [Azure Cosmos DB-Emulators](local-emulator.md) können Sie die Anwendung lokal mit der SQL-API entwickeln und testen, ohne ein Azure-Abonnement zu erstellen oder sonstige Kosten zu verursachen. Wenn Sie mit der Funktion der Anwendung im Emulator zufrieden sind, können Sie zur Verwendung eines Azure Cosmos DB-Kontos in der Cloud wechseln.

Abgesehen von den grundlegenden Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen bietet die SQL-API eine funktionsreiche SQL-Abfrageschnittstelle für den Abruf von JSON-Dokumenten und serverseitige Unterstützung für die transaktionale Ausführung von JavaScript-Anwendungslogik. Die Schnittstellen für die Abfrage- und Skriptausführung sind über alle Plattformbibliotheken verfügbar. Gleiches gilt für die REST-APIs. 

### <a name="sql-query"></a>SQL-Abfrage
Azure Cosmos DB unterstützt die Abfrage von Dokumenten mithilfe einer SQL-Sprache, die im JavaScript-Typsystem verankert ist, sowie mithilfe von Ausdrücken, die relationale, hierarchische und räumliche Abfragen unterstützen. Die Azure Cosmos DB-Abfragesprache ist eine einfache und dennoch effektive Schnittstelle für die Abfrage von JSON-Dokumenten. Die Sprache unterstützt einen Teil der ANSI SQL-Grammatik und sorgt für eine enge Integration von JavaScript-basierten Objekt-, Array-, Objektkonstruktions- und Funktionsaufrufen. Die SQL-API stellt ihr Abfragemodell ohne explizite Schema- oder Indizierungshinweise vom Entwickler bereit.

Benutzerdefinierte Funktionen (UDFs) können mit der SQL-API registriert und als Teil einer SQL-Abfrage referenziert werden, um so die Grammatik zu erweitern und die benutzerdefinierte Anwendungslogik zu unterstützen. Diese UDFs werden als JavaScript-Programme programmiert und in der Datenbank ausgeführt. 

Für .NET-Entwickler wird vom [.NET-SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) der SQL-API auch ein LINQ-Abfrageanbieter bereitgestellt. 

### <a name="transactions-and-javascript-execution"></a>Transaktionen und JavaScript-Ausführung
Die SQL-API ermöglicht es Ihnen, Anwendungslogik als festgelegte Programme zu programmieren, die vollständig in JavaScript verfasst sind. Diese Programme werden für eine Auflistung registriert und unterstützen die Ausführung von Datenbankvorgängen in einer gegebenen Auflistung. JavaScript kann für die Ausführung als Trigger, gespeicherte Prozedur oder UDF registriert werden. Trigger und gespeicherte Prozeduren können Dokumente erstellen, lesen, aktualisieren und löschen. UDFs hingegen werden ohne Schreibzugriff auf die Auflistung als Teil der Abfrageausführung ausgeführt.

Die JavaScript-Ausführung in der Cosmos-Datenbank wurde anhand der gleichen Konzepte modelliert wie relationale Datenbanksysteme. JavaScript dient dabei als moderner Ersatz für Transact-SQL. Sämtliche JavaScript-Logik wird innerhalb einer umgebenden ACI-Transaktion mit Snapshotisolation ausgeführt. Während des Ausführungsverlaufs wird die gesamte Transaktion abgebrochen, wenn JavaScript eine Ausnahme auslöst.

## <a name="next-steps"></a>Nächste Schritte
Verfügen Sie bereits über ein Azure-Konto? Anschließend könne Sie mit der Nutzung von Azure Cosmos DB beginnen, indem Sie die Schritte in den [Schnellstart](../cosmos-db/create-sql-api-dotnet.md)-Dokumenten ausführen. Darin werden das Erstellen eines Kontos und die ersten Schritte mit Cosmos DB beschrieben.

[1]: ./media/sql-api-introduction/json-database-resources1.png


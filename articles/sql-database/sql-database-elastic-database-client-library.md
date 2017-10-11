---
title: Erstellen von skalierbaren Cloud-Datenbanken | Microsoft Docs
description: "Erstellen von skalierbaren .NET Datenbank-apps mit der Clientbibliothek für elastische Datenbanken"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 0128b333f04847ab646dcb0759fcef5f7e86ffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="building-scalable-cloud-databases"></a>Erstellen von skalierbaren Cloud-Datenbanken
Horizontales Skalieren von Datenbanken kann problemlos ausgeführt werden, über die skalierbare Tools und Funktionen für Azure SQL-Datenbank. Insbesondere können Sie die **Clientbibliothek für elastische Datenbanken** zum Erstellen und Verwalten von Datenbanken dezentral skaliert. Mit diesem Feature können Sie einfach mit Shards Anwendungen mit Hunderten entwickeln – oder sogar Tausenden – der Azure SQL-Datenbanken. [Elastische Aufträge](sql-database-elastic-jobs-powershell.md) kann dann verwendet werden, um einfache Verwaltung dieser Datenbanken zu helfen.

Um die Bibliothek zu installieren, wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Dokumentation
1. [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md)
2. [Elastische Datenbankfunktionen](sql-database-elastic-scale-introduction.md)
3. [Shardverwaltung-Karte](sql-database-elastic-scale-shard-map-management.md)
4. [Migrieren Sie vorhandener Datenbanken für die horizontale Skalierung](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md)
6. [Abfragen mit mehreren Shards](sql-database-elastic-scale-multishard-querying.md)
7. [Hinzufügen von einem Shard mithilfe von Tools für elastische Datenbanken](sql-database-elastic-scale-add-a-shard.md)
8. [Anwendungen mit mehreren Mandanten mit Tools für elastische Datenbanken und Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Aktualisieren Sie die Client-Bibliothek-apps](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Übersicht über die flexible Abfragen](sql-database-elastic-query-overview.md)
11. [Glossar für elastische Datenbanken-tools](sql-database-elastic-scale-glossary.md)
12. [Clientbibliothek für elastische Datenbank mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Clientbibliothek für elastische Datenbanken mit dapper, durch](sql-database-elastic-scale-working-with-dapper.md)
14. [Split-Zusammenführungstool](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Leistungsindikatoren für shardzuordnungs-manager](sql-database-elastic-database-client-library.md) 
16. [Häufig gestellte Fragen zur elastischen Datenbanktools](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Clientfunktionen
Dezentrales Skalieren von Anwendungen mit *Sharding* führte die Herausforderungen für sowohl Entwickler als auch den Administrator. Die Clientbibliothek vereinfacht die Aufgaben zur Verwaltung Bereitstellen von Tools, mit denen sowohl Entwickler und Administratoren verwalten ausskalierte Datenbanken. In einem typischen Beispiel gibt es viele andere Datenbanken als "Shards" bezeichnet wird, zu verwalten. Kunden befinden sich in derselben Datenbank zusammen, und es wird eine Datenbank pro Kunde (einem einzigen Mandanten-Schema). Die Clientbibliothek enthält die folgenden Funktionen:

- **Zuordnung Shardverwaltung**: eine spezielle Datenbank mit dem Namen der "shardzuordnungs-Manager" wird erstellt. Shardverwaltung-Karte ist die Fähigkeit zum Verwalten von Metadaten über die Shards einer Anwendung. Entwickler können diese Funktionalität verwenden, um Datenbanken als Shards registrieren, beschreiben die Zuordnungen von einzelnen shardingschlüssel oder Schlüsselbereiche für die Datenbanken und verwalten diese Metadaten als die Anzahl und Zusammensetzung der Datenbanken weiterentwickelt, um Kapazität Änderungen widerzuspiegeln. Ohne die Clientbibliothek für elastische Datenbanken müssten Sie viel Zeit den Management-Code zu schreiben, bei der Implementierung von Sharding verbringen. Weitere Informationen finden Sie unter [Zuordnung shardverwaltung](sql-database-elastic-scale-shard-map-management.md).

- **Datenabhängiges routing**: Angenommen, eine Anforderung an die Anwendung. Basierend auf den Wert des shardingschlüssels der Anforderung, muss die Anwendung die richtige Datenbank basierend auf den Wert des Schlüssels zu bestimmen. Er öffnet dann eine Verbindung mit der Datenbank zum Verarbeiten der Anforderung. Datenabhängiges routing bietet die Möglichkeit, Verbindungen mit einem einzigen einfach Aufruf in der shardzuordnung der Anwendung zu öffnen. Datenabhängiges routing wurde einem anderen Bereich der Infrastrukturcode, den jetzt durch Funktionalität in der Clientbibliothek für elastische Datenbanken abgedeckt wird. Weitere Informationen finden Sie unter [datenabhängiger Weiterleitung](sql-database-elastic-scale-data-dependent-routing.md).
- **Mit mehreren Shard-Abfragen (MSQ)**: Abfragen mit mehreren Shard funktioniert, wenn eine Anforderung (oder alle) mehrere Shards umfasst. Eine Abfrage mit mehreren Shard führt den gleichen T-SQL-Code auf alle Shards oder einem Satz von Shards. Die Ergebnisse von den Beteiligten Shards werden in einer gesamten Resultset mit UNION ALL-Semantik zusammengeführt. Die Funktionalität über die Clientbibliothek zur Verfügung gestellt behandelt viele Aufgaben, einschließlich: verbindungsverwaltung, Threadverwaltung und Verarbeiten von Zwischenergebnissen zur Behandlung. MSQ kann bis zu Hunderte von Shards Abfragen. Weitere Informationen finden Sie unter [Abfragen mit mehreren Shard](sql-database-elastic-scale-multishard-querying.md).

Im Allgemeinen können Kunden, die mithilfe von Tools für elastische Datenbanken erwarten, um vollständige T-SQL-Funktionalität zu erhalten, wird beim Übermitteln von Shard-lokale Vorgänge im Gegensatz zu Shard-übergreifenden Vorgänge, die eigene Semantik auf.

## <a name="next-steps"></a>Nächste Schritte
Wiederholen Sie den [Beispiel-app](sql-database-elastic-scale-get-started.md) die Clientfunktionen veranschaulicht. 

Um die Bibliothek zu installieren, wechseln Sie zu [elastischen Datenbank-Clientbibliothek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Mit dem Tool Split-Merge-Anweisungen, finden Sie unter der [Split-Merge-Tool-Übersicht](sql-database-elastic-scale-overview-split-and-merge.md).

[Clientbibliothek für elastische Datenbanken ist jetzt geöffnet stammenden!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Verwendung [flexible Abfragen](sql-database-elastic-query-overview.md).

Die Bibliothek steht als open Source-Software auf [GitHub](https://github.com/Azure/elastic-db-tools). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png


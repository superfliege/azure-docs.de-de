---
title: 'Azure Cosmos DB: BulkExecutor – .NET-API, SDK und Ressourcen'
description: Wichtige Informationen zur .NET-API und zum SDK für Bulk Executor, einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des .NET SDK für Azure Cosmos DB Bulk Executor.
author: tknandu
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: c239464a37637b21504227951d917977cfea6726
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343954"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET-BulkExecutor-Bibliothek: Informationen zum Download 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Änderungsfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Beschreibung**</td><td>Die BulkExecutor-Bibliothek ermöglicht Clientanwendungen die Ausführung von Massenvorgängen in Azure Cosmos DB-Konten. Die BulkExecutor-Bibliothek stellt die Namespaces „BulkImport“, „BulkUpdate“ und „BulkDelete“ bereit. Das BulkImport-Modul kann Dokumente auf optimierte Weise per Massenimport erfassen, sodass der für eine Sammlung bereitgestellte Durchsatz maximal genutzt wird. Das BulkUpdate-Modul kann vorhandene Daten in Azure Cosmos DB-Containern per Massenvorgang in Form von Patches aktualisieren. Das BulkDelete-Modul kann Dokumente auf optimierte Weise per Massenlöschung entfernen, sodass der für eine Sammlung bereitgestellte Durchsatz maximal genutzt wird.</td></tr>

<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**BulkExecutor-Bibliothek in GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem .NET SDK für die Bulk Executor-Bibliothek](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Aktuelles unterstütztes Framework**</td><td>Microsoft .NET Framework 4.5.2, 4.6.1 und .NET Standard 2.0 </td></tr>
</table></br>

## <a name="release-notes"></a>Versionshinweise

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Enthält nun MongoBulkExecutor zur Unterstützung von .NET Standard 2.0. Dieses Feature stellt funktionale Gleichheit mit dem Release 1.3.0 her und unterstützt als Zielframework zusätzlich auch .NET Standard 2.0.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* .NET Standard 2.0 wurde als unterstütztes Zielframework hinzugefügt, damit die BulkExecutor-Bibliothek mit .NET Core-Anwendungen verwendet werden kann.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Eine Überladung für den BulkDelete-Vorgang wurde hinzugefügt, damit SQL-API-Konten Tupel aus Partitionsschlüssel und Dokument-ID für das Löschen akzeptieren.
* Eine Überladung für den BulkDelete-Vorgang wurde hinzugefügt, damit SQL-API-Konten RequestOptions akzeptieren, die den Partitionsschlüssel dem Wert des Partitionsschlüssels enthalten, und diesen nicht nur als Filter in der Eingabeabfrage für das Löschen von Dokumenten verwenden.
* Ein Problem wurde behoben, durch das ein Formatierungsproblem in dem von BulkExecutor verwendeten Benutzer-Agent verursacht wurde.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Es wurden Verbesserungen an den Import- und Update-APIs von BulkExecutor vorgenommen, um transparent die flexible Skalierung von Cosmos DB-Containern bei Überschreiten der aktuellen Speicherkapazität zu ermöglichen, ohne dass Ausnahmen ausgelöst werden.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Aktualisierung der DocumentDB .NET SDK-Abhängigkeit auf Version 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Ein Problem wurde behoben, durch das BulkExecutor beim Importieren in feste Sammlungen JSRT-Fehler auslöste.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Unterstützung für BulkDelete-Vorgang für die SQL-API von Azure Cosmos DB-Konten.
* Unterstützung für BulkImport-Vorgang für die MongoDB-API von Azure Cosmos DB-Konten.
* Aktualisierung der DocumentDB .NET SDK-Abhängigkeit auf Version 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Unterstützung für BulkImport-Vorgang für die Gremlin-API von Azure Cosmos DB-Konten.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Kleinere Fehlerbehebung am BulkImport-Vorgang für die SQL-API von Azure Cosmos DB-Konten.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Unterstützung für BulkImport- und Bulkupdate-Vorgänge für die SQL-API von Azure Cosmos DB-Konten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur BulkExecutor-Java-Bibliothek finden Sie im folgenden Artikel:

[SDK und Versionshinweise zur Java-BulkExecutor-Bibliothek](sql-api-sdk-bulk-executor-java.md)

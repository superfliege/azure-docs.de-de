---
title: 'Azure Cosmos DB: .NET-API, SDK und Ressourcen für Bulk Executor | Microsoft Docs'
description: Wichtige Informationen zur .NET-API und zum SDK für Bulk Executor, einschließlich Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen des .NET SDK für Azure Cosmos DB Bulk Executor.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/14/2018
ms.author: ramkris
ms.openlocfilehash: ffd8f438429cd8769ac0dbff7f489327166e0000
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294457"
---
# <a name="net-bulk-executor-library-download-information"></a>Bulk Executor-Bibliothek für .NET: Informationen zum Download 

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
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Bulk Executor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Beschreibung**</td><td>Die Bulk Executor-Bibliothek ermöglicht Clientanwendungen die Ausführung von Massenvorgängen in Azure Cosmos DB-Konten. Die Bulk Executor-Bibliothek stellt die Namespaces „BulkImport“, „BulkUpdate“ und „BulkDelete“ bereit. Das BulkImport-Modul kann Dokumente auf optimierte Weise per Massenimport erfassen, sodass der für eine Sammlung bereitgestellte Durchsatz maximal genutzt wird. Das BulkUpdate-Modul kann vorhandene Daten in Azure Cosmos DB-Containern per Massenvorgang in Form von Patches aktualisieren. Das BulkDelete-Modul kann Dokumente auf optimierte Weise per Massenlöschung entfernen, sodass der für eine Sammlung bereitgestellte Durchsatz maximal genutzt wird.</td></tr>

<tr><td>**SDK-Download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**BulkExecutor-Bibliothek in GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit dem .NET SDK für die Bulk Executor-Bibliothek](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Aktuelles unterstütztes Framework**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (Version >= 2.0.0)</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Version >= 9.0.1)
</li></ul></td></tr>
</table></br>

## <a name="release-notes"></a>Versionshinweise

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

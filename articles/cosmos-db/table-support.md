---
title: "Unterstützung für Azure Table Storage in Azure Cosmos DB | Microsoft-Dokumentation"
description: Erfahren Sie, wie die Azure Cosmos DB-Tabellen-API und Azure Storage-Tabellen zusammen funktionieren.
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: 378f00f2-cfd9-4f6b-a9b1-d1e4c70799fd
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: c0c8f1aee75c4ee5cc35758b71ef573637fd3edd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Entwickeln mit der Azure Cosmos DB-Tabellen-API und Azure Table Storage

Die Azure Cosmos DB-Tabellen-API und Azure Table Storage nutzen dasselbe Tabellendatenmodell und dieselben Erstellungs-, Lösch-, Update- und Abfragevorgänge über ihre SDKs. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Entwickeln mit der Azure Cosmos DB-Tabellen-API

Derzeit sind für die [Tabellen-API von Azure Cosmos DB](table-introduction.md) vier SDKs zur Entwicklung verfügbar: 
- .NET SDK [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget). Diese Bibliothek enthält dieselben Klassen- und Methodensignaturen wie das öffentliche [Microsoft Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), kann aber mithilfe der Tabellen-API auch eine Verbindung mit Azure Cosmos DB-Konten herstellen. 
- [Python SDK](table-sdk-python.md). Das neue Azure Cosmos DB Python SDK ist das einzige SDK, das Azure Table Storage in Python unterstützt. Dieses SDK stellt eine Verbindung mit Azure Table Storage sowie mit der Tabellen-API von Azure Cosmos DB her.
- [Java SDK](table-sdk-java.md). Dieses Azure Storage SDK kann mithilfe der Tabellen-API eine Verbindung mit Azure Cosmos DB-Konten herstellen.
- [Node.js SDK](table-sdk-nodejs.md). Dieses Azure Storage SDK kann mithilfe der Tabellen-API eine Verbindung mit Azure Cosmos DB-Konten herstellen.

Weitere Informationen zum Arbeiten mit der Tabellen-API finden Sie im FAQ-Artikel unter [Entwickeln mit der Tabellen-API (Vorschauversion)](faq.md#develop-with-the-table-api).

## <a name="developing-with-the-azure-table-storage"></a>Entwickeln mit Azure Table Storage

[Azure Table Storage](table-storage-overview.md) hat zahlreiche verfügbare SDKs und Tutorials, die nun im Abschnitt [Azure Table Storage](table-storage-overview.md) verfügbar sind. Diese Artikel werden aktualisiert, sobald Azure Table Storage-SDKs und die Azure Cosmos-DB-Tabelle-APIs interoperabel werden.  






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
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 1513fc53501f1cfec93134841fbef9a8552dd43c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Entwickeln mit der Azure Cosmos DB-Tabellen-API und Azure Table Storage

Die Azure Cosmos DB-Tabellen-API und Azure Table Storage nutzen dasselbe Tabellendatenmodell und dieselben Erstellungs-, Lösch-, Update- und Abfragevorgänge über ihre SDKs. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Entwickeln mit der Azure Cosmos DB-Tabellen-API

Zurzeit ist für die [Azure Cosmos DB-Tabellen-API](table-introduction.md) ein .NET SDK verfügbar: das [Windows Azure Storage Premium Table (Vorschauversion)](https://aka.ms/premiumtablenuget). Diese Bibliothek enthält dieselben Klassen- und Methodensignaturen wie das öffentliche [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), kann aber mithilfe der Tabellen-API (Vorschauversion) auch einer Verbindung mit Azure Cosmos DB-Konten herstellen. In den folgenden Artikeln finden Sie einen Schnellstart und ein Tutorial zur Azure Cosmos DB-Tabellen-API:
- Schnellstart: [Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Tabellen-API](create-table-dotnet.md)
- Tutorial: [Azure Cosmos DB: Entwickeln mit der Tabellen-API in .NET](tutorial-develop-table-dotnet.md)

Weitere Informationen zum Arbeiten mit der Tabellen-API finden Sie im FAQ-Artikel unter [Entwickeln mit der Tabellen-API (Vorschauversion)](faq.md#develop-with-the-table-api-preview).

## <a name="developing-with-the-azure-table-storage"></a>Entwickeln mit Azure Table Storage

[Azure Table Storage](table-storage-overview.md) hat zahlreiche verfügbare SDKs und Tutorials, die nun im Abschnitt [Azure Table Storage](table-storage-overview.md) verfügbar sind. Diese Artikel werden aktualisiert, sobald Azure Table Storage-SDKs und die Azure Cosmos-DB-Tabelle-APIs interoperabel werden.  






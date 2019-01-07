---
title: 'Azure Cosmos DB-Tabellen-API: .NET Standard SDK und Ressourcen'
description: Enthält Informationen zur Azure Cosmos DB-Tabellen-API und zum .NET Standard SDK, z.B. Veröffentlichungstermine, Deaktivierungstermine und Änderungen an den einzelnen Versionen.
services: cosmos-db
author: donghexu
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/18/2018
ms.author: dox
ms.openlocfilehash: dd095328f8ba4cc22eea21133e7c8600a2cd64e1
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438658"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>.NET Standard-API für Azure Cosmos DB-Tabellen: Download und Versionshinweise
> [!div class="op_single_selector"]

> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK-Download**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Aktuelles unterstütztes Framework**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Versionshinweise

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0-preview
* Es wurde Unterstützung für grundlegende CRUD-, Batch- und Abfragevorgänge für Azure Storage-Tabellenendpunkte hinzugefügt. [!NOTE] Einige Funktionen in vorherigen Azure Storage-Tabellen-SDKs werden noch nicht unterstützt (z. B. die clientseitige Verschlüsselung).

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1-preview
* „Azure Cosmos DB-Tabellen-API – .NET Standard SDK“ ist eine plattformübergreifende .NET-Bibliothek, die den effizienten Zugriff auf das Tabellendatenmodell unter Cosmos DB ermöglicht. Diese erste Version unterstützt den gesamten Satz mit CRUD- und Abfragefunktionen für Tabellen- und Entitätsvorgänge mit ähnlichen APIs wie das [Cosmos DB-Tabellen-SDK für .NET Framework](table-sdk-dotnet.md). [!NOTE] Azure Storage-Tabellenendpunkte werden in der Version 0.9.1-preview noch nicht unterstützt.

## <a name="release-and-retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [0.10.0-preview](#0.10.0-preview) |18. Dezember 2018 |--- |
| [0.9.1-preview](#0.9.1-preview) |18. Oktober 2018 |--- |


## <a name="faq"></a>Häufig gestellte Fragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zur Table-API von Azure Cosmos DB finden Sie in der [Einführung in die Tabellen-API von Azure Cosmos DB](table-introduction.md). 

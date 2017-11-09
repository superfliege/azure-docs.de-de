---
title: Azure DocumentDB .NET Change Feed Processor-SDK und -Ressourcen | Microsoft-Dokumentation
description: "Wichtige Informationen zur Change Feed Processor-API und zum Change Feed Processor-SDK, einschließlich Terminen für Veröffentlichung und Außerbetriebnahme sowie Änderungen an den einzelnen Versionen des DocumentDB .NET Change Feed Processor-SDK."
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: maquaran
ms.openlocfilehash: 8c5a081105d37b8f1c78e90d9ccb8d945099b621
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a>DocumentDB .NET Change Feed Processor-SDK: Download und Anmerkungen zur Version
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET-Änderungsfeed](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

|   |   |
|---|---|
|**SDK-Download**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-Dokumentation**|[Referenzdokumentation zum Ändern der Feedprozessorbibliotheks-API](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Erste Schritte**|[Erste Schritte mit dem DocumentDB .NET Change Feed Processor-SDK](change-feed.md)|
|**Aktuelles unterstütztes Framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Versionshinweise

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Unterstützung für .NET Standard 2.0 wurde hinzugefügt. Das Paket unterstützt nun die Frameworkmoniker `netstandard2.0` und `net451`.
* Kompatibel mit [DocumentDB .NET SDK](documentdb-sdk-dotnet.md), Version 1.17.0 und höher
* Kompatibel mit [DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md), Version 1.5.1 und höher

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Ein Problem bei der Kalkulierung der geschätzten verbleibenden Arbeit wurde behoben, wenn der Änderungsfeed leer war oder keine Arbeit ausstand.
* Kompatibel mit [DocumentDB .NET SDK](documentdb-sdk-dotnet.md), Version 1.13.2 und höher

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Eine Methode wurde hinzugefügt, mit der eine Schätzung der verbleibenden im Änderungsfeed zu verarbeitenden Arbeit abgerufen werden kann.
* Kompatibel mit [DocumentDB .NET SDK](documentdb-sdk-dotnet.md), Version 1.13.2 und höher

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Allgemeine Verfügbarkeit (GA) des SDK
* Kompatibel mit [DocumentDB .NET SDK](documentdb-sdk-dotnet.md), Version 1.14.1 und niedriger

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen an Cosmos DB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [1.2.0](#1.2.0) |31. Oktober 2017 |--- |
| [1.1.1](#1.1.1) |29. August 2017 |--- |
| [1.1.0](#1.1.0) |13. August 2017 |--- |
| [1.0.0](#1.0.0) |7. Juli 2017 |--- |


## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 


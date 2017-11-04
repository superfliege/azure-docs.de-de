---
title: ".NET-SDKs und Ressourcen für die Azure Cosmos DB Graph-API | Microsoft-Dokumentation"
description: "Alle Informationen zur Azure CosmosDB Graph-API, einschließlich Veröffentlichungsdaten, Deaktivierungsdaten und Änderungen in den einzelnen Versionen."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/17/2017
ms.author: mimig
ms.openlocfilehash: 7d6ba5794e4a3e431abd72a780b60b9e59e9f4db
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2017
---
# <a name="azure-cosmos-db-graph-net-api-download-and-release-notes"></a>Azure Cosmos DB Graph-.NET-API: Download und Versionshinweise

|   |   |
|---|---|
|**SDK-Download**|[NuGet](https://aka.ms/acdbgraphnuget)|
|**API-Dokumentation**|[.NET API-Referenzdokumentation](https://aka.ms/acdbgraphapiref)|
|**Schnellstart**|[Azure Cosmos DB: Erstellen einer Diagramm-App mit .NET und der Graph-API](create-graph-dotnet.md)|
|**Tutorial**|[Azure CosmosDB: Erstellen eines Containers mit der Graph-API](tutorial-develop-graph-dotnet.md)|
|**Aktuelles unterstütztes Framework**| [Microsoft .NET Framework 4.6.1](https://www.microsoft.com/en-us/download/details.aspx?id=49981)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |


## <a name="release-notes"></a>Versionshinweise

### <a name="a-name031-preview031-preview"></a><a name="0.3.1-preview"/>0.3.1-Vorschau

#### <a name="bug-fixes"></a>Fehlerbehebungen
* Behebung zum optionalen Laden von `appsettings.json` (`netstandard1.6`)

#### <a name="whats-new"></a>Neuigkeiten
* Wechseln Sie für Microsoft.Azure.Graphs zur Zielplattform AnyCPU.
* Entfernen Sie die Mono-Assembly aus dem `net461`-Paketmanifest.

### <a name="a-name030-preview030-preview"></a><a name="0.3.0-preview"/>0.3.0-Vorschau

#### <a name="whats-new"></a>Neuigkeiten
* Unterstützung für `.netstandard 1.6` wurde hinzugefügt.
  * Erfordert `Microsoft.Azure.DocumentDB.Core >= 1.5.1`
* Ein neuer `gremlin-groovy`-Parser zum Ersetzen des vorhandenen Parsers wurde hinzugefügt. Dieser Parser unterstützt einen Teil der `gremlin-groovy`-Syntax von Tinkerpop und enthält:
  * Verdoppelung der Analyseleistung.
  * Eine Reihe von Problemen mit Escapesequenzen in Zeichenfolgen, nicht ordnungsgemäß behandelten Literalwerten und andere Unregelmäßigkeiten im alten Parser wurden behoben.
* Optimierungen für Edgeausnahmen wurden hinzugefügt.
  *  Traversale Hops mit Filtern sollten diese Verbesserung erkennen, z.B.: `g.V('1').outE().has('name', 'marko').inV()`.
* Optimierungen für Edgeausnahmen mit `limit()`-Schritt wurden hinzugefügt.

#### <a name="breaking-changes"></a>Wichtige Änderungen
* Unterstützung für .NET Framework 4.5.1 wurde entfernt.

* Der neue Parser ist an der `gremlin-groovy`-Grammatik ausgerichtet. Dies hat zur Folge, dass einige Ausdrücke, die zuvor funktionierten, für den neuen Parser mehrdeutig sind. Beachten Sie etwa:
  * `in` und `as` sind reservierte Schlüsselwörter in `gremlin-groovy`, sodass diese Schritte mit `.in()` oder `.as()` näher bestimmt werden müssen, um Syntaxfehler zu vermeiden. Beispiel: `g.V().repeat(in()).times(2)` -> _löst einen Syntaxfehler aus_  
 `g.V().repeat(__.in()).times(2)` -> _ist erfolgreich_

### <a name="a-name024-preview024-preview"></a><a name="0.2.4-preview"/>0.2.4-Vorschau

### <a name="a-name022-preview022-preview"></a><a name="0.2.2-preview"/>0.2.2-Vorschau

### <a name="a-name021-preview021-preview"></a><a name="0.2.1-preview"/>0.2.1-Vorschau

### <a name="a-name020-preview020-preview"></a><a name="0.2.0-preview"/>0.2.0-Vorschau

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-preview
* Erste Vorschauversion

## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen an Azure Cosmos DB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [0.3.1-Vorschau](#0.3.1-preview) |17. Oktober 2017 |--- |
| [0.3.0-Vorschau](#0.3.0-preview) |2. Oktober 2017 |--- |
| [0.2.4-Vorschau](#0.2.4-preview) |4. August 2017 |--- |
| [0.2.2-Vorschau](#0.2.2-preview) |23. Juni 2017 |--- |
| [0.2.1-Vorschau](#0.2.2-preview) |8. Juni 2017 |--- |
| [0.2.0-Vorschau](#0.2.2-preview) |10. Mai 2017 |--- |
| [0.1.0-preview](#0.1.0-preview) |8. Mai 2017 |--- |

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zur Azure Cosmos DB Graph-API finden Sie unter [Einführung in Azure Cosmos DB: Graph-API](graph-introduction.md). 

---
title: REST-API für den Azure Search-Dienst, Version 2017-11-11-Preview | Microsoft-Dokumentation
description: Die REST-API für den Azure Search-Dienst, Version 2017-11-11-Preview, beinhaltet experimentelle Features wie Synonyme und moreLikeThis-Suchvorgänge.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/17/2018
ms.author: HeidiSteen
ms.openlocfilehash: ce5771777762414a0229cf83425c2f3601cb979a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659627"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>REST-API für den Azure Search-Dienst: Version 2017-11-11-Preview
Dieser Artikel beschreibt die Version `api-version=2017-11-11-Preview` der Azure Search-Dienst-REST-API, welche die folgenden experimentellen Features bietet, die noch nicht allgemein verfügbar sind.

> [!NOTE]
> Vorschaufeatures stehen für Tests und Versuche zur Verfügung, um Feedback zu sammeln, und können sich ändern. Wir raten dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.


## <a name="new-in-2017-11-11-preview"></a>Neuerungen in 2017-11-11-Preview

Die [kognitive Suche](cognitive-search-concept-intro.md) ist eine neue Anreicherungsfunktion in Azure Search, die latente Informationen in Nicht-Textquellen und undifferenziertem Text findet und sie in durchsuchbare Volltextinhalte in Azure Search umwandelt.

Die folgenden Ressourcen werden in der REST-API-Vorschauversion eingeführt oder geändert. Alle anderen REST-APIs sind beim Aufrufen der allgemein verfügbaren Version und der Vorschauversion identisch.

+ [Erstellen einer Fähigkeitengruppe (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)

+ [Erstellen eines Indexers (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)

Alle anderen REST-APIs sind identisch, unabhängig davon, wie Sie die API-Version festlegen. Beispielsweise sind `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` und `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (ohne `Preview`) funktionell gleichwertig.

## <a name="other-preview-features"></a>Weitere Vorschaufeatures

In früheren Vorschauversionen angekündigte Features befinden sich immer noch in der öffentlichen Vorschau. Wenn Sie eine API mit einer früheren API-Vorschauversion aufrufen, können Sie weiterhin diese Version verwenden oder zu `2017-11-11-Preview` wechseln, ohne Änderungen am erwarteten Verhalten feststellen zu müssen.

+ Das in `api-version=2015-02-28-Preview` eingeführte Feature [CSV-Dateien in der Azure-BLOB-Indizierung](search-howto-index-csv-blobs.md) bleibt ein Vorschaufeature. Dieses Feature ist Teil der Azure-BLOB-Indizierung und wird durch eine Parametereinstellung aufgerufen. Jede Zeile in einer CSV-Datei wird als separates Dokument indiziert.

+ Das in `api-version=2015-02-28-Preview` eingeführte Feature [JSON-Arrays in der Azure-BLOB-Indizierung](search-howto-index-json-blobs.md) bleibt ein Vorschaufeature. Dieses Feature ist Teil der Azure-BLOB-Indizierung und wird durch eine Parametereinstellung aufgerufen. Dabei wird jedes Element im Array als separates Dokument indiziert.

+ [moreLikeThis-Abfrageparameter](search-more-like-this.md) sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. 


## <a name="how-to-call-a-preview-api"></a>Aufrufen einer Vorschau-API

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2016-09-01-Preview` oder `api-version=2015-02-28-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert. 

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Der Azure-Suchdienst ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

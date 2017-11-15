---
title: "REST-API für den Azure Search-Dienst, Version 2016-09-01-Preview | Microsoft-Dokumentation"
description: "Die REST-API für den Azure Search-Dienst, Version 2016-09-01-Preview, beinhaltet Testfeatures wie Synonyme und moreLikeThis-Suchvorgänge."
services: search
documentationcenter: na
author: mhko
manager: jlembicz
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 082c207f892fcc277d30d66c6165dd9920d3ab27
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>REST-API für den Azure Search-Dienst: Version 2016-09-01-Preview
Dieser Artikel bildet die Referenzdokumentation zu `api-version=2016-09-01-Preview`. Diese Vorschauversion erweitert die aktuelle allgemein verfügbare Version [api-version=2016-09-01](https://msdn.microsoft.com/library/dn798935.aspx) um folgende Testfeatures:

* [Synonyms-API](search-synonyms.md) zum Hochladen von Synonymzuordnungen und Erweitern der Suche.
* [`moreLikeThis`-Abfrageparameter](search-more-like-this.md) zum Suchen von Dokumenten, die für ein bestimmtes Dokument relevant sind.

Vergewissern Sie sich, dass Sie die Vorschauversion `api-version=2016-09-01-Preview` der API verwenden, um diese Features zu testen. Im folgenden Beispiel wird veranschaulicht, wie die API-Vorschauversion bei einer moreLikeThis-Abfrage angegeben wird.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Vorschaufeatures stehen für Tests und Versuche zur Verfügung, um Feedback zu sammeln, und können sich ändern. **Wir raten dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.**

Der Azure-Suchdienst ist in mehreren Versionen verfügbar. Weitere Informationen erhalten Sie im Artikel [Versionsverwaltung für den Azure-Suchdienst](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

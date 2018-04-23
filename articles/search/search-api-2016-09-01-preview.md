---
title: REST-API für den Azure Search-Dienst, Version 2016-09-01-Preview | Microsoft-Dokumentation
description: Die REST-API für den Azure Search-Dienst, Version 2016-09-01-Preview, beinhaltet Testfeatures wie Synonyme und moreLikeThis-Suchvorgänge.
author: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: reference
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 034c7c9c6e97ebb128860b6041089b311ffaf528
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
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

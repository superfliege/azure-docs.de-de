---
title: Aktualisieren auf die aktuelle Version der REST-API für den Azure Search-Dienst | Microsoft-Dokumentation
description: Aktualisieren auf die aktuelle Version der REST-API für den Azure Search-Dienst
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 3848f317fd6d760961756f132edf9cbcb5f431ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181969"
---
# <a name="upgrading-to-the-latest-azure-search-service-rest-api-version"></a>Aktualisieren auf die aktuelle Version der REST-API für den Azure Search-Dienst
Wenn Sie eine frühere Version der [REST-API für den Azure Search-Dienst](https://docs.microsoft.com/rest/api/searchservice/) verwenden, unterstützt dieser Artikel Sie beim Upgrade Ihrer Anwendung auf die aktuelle allgemein verfügbare API-Version 2017-11-11.

Version 2017-11-11 der REST-API enthält einige Änderungen im Vergleich zu früheren Versionen. Sie sind größtenteils abwärtskompatibel, daher sollte das Ändern von Code nur minimalen Aufwand bedeuten, abhängig davon, welche Version Sie zuvor verwendet haben. Anweisungen zum Ändern Ihres Codes, um die neue API-Version zu verwenden, finden Sie unter [Schritte zum Upgrade](#UpgradeSteps).

> [!NOTE]
> Ihre Azure Search-Dienstinstanz unterstützt mehrere REST-API-Versionen, einschließlich der neuesten. Sie können auch ältere Versionen weiterhin verwenden, aber es wird empfohlen, den Code zur neuesten Version zu migrieren.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Neuerungen in Version 2017-11-11
Version 2017-11-11 ist das allgemein verfügbare Release der REST-API für den Azure Search-Dienst. Neue Funktionen in dieser API-Version:

* [Synonyme](search-synonyms.md). Mit dem neuen Synonymefeature können Sie gleichwertige Begriffe definieren und den Umfang der Abfrage erweitern.
* [Unterstützung der effizienten Indizierung von Text-Blobs](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). Der neue `text`-Analysemodus für Azure-Blobindizierer verbessert die Indizierungsleistung erheblich.
* [Dienststatistik-API](https://aka.ms/azure-search-stats). Zeigen Sie die aktuelle Verwendung und die Beschränkungen von Ressourcen in Azure Search mit dieser neuen API an.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Schritte zum Upgrade
Wenn Sie das Upgrade von einer allgemein verfügbaren Version, 2015-02-28 oder 2016-09-01 ausführen, müssen Sie wahrscheinlich keine Änderungen am Code vornehmen, außer die Versionsnummer zu ändern. Nur in folgenden Situationen müssen Sie möglicherweise Code ändern:

* Der Code erzeugt Fehler, wenn nicht erkannte Eigenschaften in einer API-Antwort zurückgegeben werden. Standardmäßig sollte die Anwendung Eigenschaften ignorieren, die sie nicht versteht.
* Ihr Code behält API-Anforderungen bei und versucht, sie erneut an die neue API-Version zu senden. Dies kann beispielsweise vorkommen, wenn Ihre Anwendung Fortsetzungstoken beibehält, die von der Search-API zurückgegeben wurden (weitere Informationen finden Sie, indem Sie in der [Search-API-Referenz](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) nach `@search.nextPageParameters` suchen).

Wenn diese Situationen für Sie zutreffen, müssen Sie den Code möglicherweise entsprechend ändern. Ansonsten sollten keine Änderungen nötig sein, es sei denn, Sie möchten die [neuen Features](#WhatsNew) der Version 2017-11-11 verwenden.

Wenn Sie das Upgrade von einer Vorschau-API-Version ausführen, gilt das oben Genannte ebenfalls. Sie müssen jedoch außerdem beachten, dass einige Features der Vorschauversion in der Version 2017-11-11 nicht verfügbar sind:

* Unterstützung für CSV-Dateien und Blobs, die JSON-Arrays enthalten, durch Azure Blob Storage-Indexer
* MLT-Abfragen (More Like This)

Wenn der Code diese Features verwendet, können Sie erst auf 2017-11-11 aktualisieren, sobald Sie sie nicht mehr nutzen.

> [!IMPORTANT]
> Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.
> 
> 

## <a name="conclusion"></a>Zusammenfassung
Weitere Informationen zur Verwendung der REST-API für den Azure Search-Dienst finden Sie in der kürzlich aktualisierten [API-Referenz](https://docs.microsoft.com/rest/api/searchservice/) auf MSDN.

Wir freuen uns auf Ihr Feedback zu Azure Search. Wenn Probleme auftreten, können Sie sich gerne über das [Azure Search-MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) oder [StackOverflow](http://stackoverflow.com/) an uns wenden. Wenn Sie eine Frage zu Azure Search auf StackOverflow stellen, müssen Sie sie mit `azure-search` kennzeichnen.

Vielen Dank für die Nutzung von Azure Search!


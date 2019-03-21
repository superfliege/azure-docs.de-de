---
title: moreLikeThis in Azure Search (Vorschau) – Azure Search
description: Vorläufige Dokumentation für das Feature moreLikeThis (Vorschau), das in der REST-API für Azure Search verfügbar gemacht wird.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d55a6d883e0dcd5ad4b1c1584b76bae06e6c742a
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569041"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (Vorschau)

`moreLikeThis=[key]` ist ein Abfrageparameter in der [Such-API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Durch Angabe des Parameters `moreLikeThis` in einer Suchabfrage können Sie Dokumente suchen, die mit dem über den Dokumentschlüssel angegebenen Dokument übereinstimmen. Beim Ausführen einer Suchanforderung mit `moreLikeThis` wird eine Abfrage mit Suchbegriffen generiert, die aus dem angegebenen Dokument extrahiert werden und das Dokument am besten beschreiben. Anhand der generierten Abfrage wird dann die Suchanforderung erstellt. Standardmäßig wird der Inhalt aller Felder mit dem Attribut `searchable` berücksichtigt, es sei denn, die Felder werden durch den Parameter `searchFields` eingeschränkt. Der Parameter `moreLikeThis` kann nicht mit dem Suchparameter `search=[string]` verwendet werden.

## <a name="examples"></a>Beispiele 

Unten ist ein Beispiel für eine moreLikeThis-Abfrage angegeben. Mit der Abfrage werden Dokumente gesucht, deren Beschreibungsfelder dem Feld des Quelldokuments entsprechend der Angabe durch den Parameter `moreLikeThis` am ähnlichsten sind.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Verfügbarkeit von Funktionen

Das Feature „moreLikeThis“ befindet sich derzeit in der Vorschauphase und wird nur in den API-Vorschauversionen `2015-02-28-Preview` und `2016-09-01-Preview` unterstützt. Da die API-Version in der Anforderung angegeben ist, können allgemein verfügbare und Vorschau-APIs in derselben App kombiniert werden. Vorschauversionen von APIs sind nicht durch ein SLA abgedeckt, und die Features können sich ändern, sodass die Verwendung in Produktionsanwendungen nicht empfehlenswert ist.

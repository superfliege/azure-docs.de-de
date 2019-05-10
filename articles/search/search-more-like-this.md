---
title: moreLikeThis in Azure Search (Vorschau) – Azure Search
description: Vorläufige Dokumentation für das Feature moreLikeThis (Vorschau), das in der REST-API für Azure Search verfügbar gemacht wird.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d18069335bb20f78a5bcda85eb6fcb2a5abe75f7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024667"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (Vorschau)

`moreLikeThis=[key]` ist ein Abfrageparameter in der [API zum Durchsuchen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/search-documents), über den Dokumente gesucht werden, die dem mit dem Dokumentschlüssel angegebenen Dokument ähneln. Beim Ausführen einer Suchanforderung mit `moreLikeThis` wird eine Abfrage mit Suchbegriffen generiert, die aus dem angegebenen Dokument extrahiert werden und das Dokument am besten beschreiben. Anhand der generierten Abfrage wird dann die Suchanforderung erstellt. Standardmäßig werden die Inhalte aller durchsuchbaren Felder berücksichtigt, mit Ausnahme aller eingeschränkten Felder, die Sie mit dem Parameter `searchFields` angegeben haben. Der Parameter `moreLikeThis` kann nicht mit dem Suchparameter `search=[string]` verwendet werden.

Standardmäßig werden die Inhalte aller durchsuchbaren Felder auf oberster Ebene berücksichtigt. Wenn Sie stattdessen bestimmte Felder angeben möchten, können Sie den Parameter `searchFields` verwenden. 

> [!NOTE]
> Die Vorschauversion von `moreLikeThis` kann in durchsuchbaren Unterfeldern in einem [komplexen Typ](search-howto-complex-data-types.md) nicht verwendet werden.

## <a name="examples"></a>Beispiele 

Unten ist ein Beispiel für eine moreLikeThis-Abfrage angegeben. Mit der Abfrage werden Dokumente gesucht, deren Beschreibungsfelder dem Feld des Quelldokuments entsprechend der Angabe durch den Parameter `moreLikeThis` am ähnlichsten sind.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Verfügbarkeit von Funktionen

Der Parameter `moreLikeThis` ist nur in Vorschau-REST-APIs verfügbar (`api-version=2019-05-06-Preview`).

## <a name="next-steps"></a>Nächste Schritte

Sie können diese Funktion mit einem beliebigen Webtesttool ausprobieren.  Es empfiehlt sich die Verwendung von Postman.

> [!div class="nextstepaction"]
> [Erkunden von Azure Search-REST-APIs mit Postman](search-fiddler.md)
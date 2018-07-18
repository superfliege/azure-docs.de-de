---
title: 'Benutzerdefinierte Bing-Suche: Abrufen von Vorschlägen für die benutzerdefinierte Vorschlagssuche | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie Vorschläge für die benutzerdefinierte Vorschlagssuche abrufen.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 73059038018602f7aa76377bf7c98d4658576576
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374707"
---
# <a name="get-custom-suggestions"></a>Abrufen von benutzerdefinierten Vorschlägen
Rufen Sie vor dem Senden von Abfragen an die benutzerdefinierte Bing-Suche die API für die benutzerdefinierte Vorschlagssuche ab, um Vorschläge für Suchbegriffe zu erhalten und das Suchergebnis zu verbessern. Die API für die benutzerdefinierte Vorschlagssuche gibt basierend auf einem Teil der Abfragezeichenfolge, die der Benutzer bereitstellt, eine Liste mit vorgeschlagenen Abfragen zurück. Alle relevanten benutzerdefinierten Suchbegriffe, die Sie angeben, werden vor den von der Vorschlagssuche generierten Vorschlägen angezeigt. Weitere Informationen finden Sie unter [Define custom search suggestions](define-custom-suggestions.md) (Definieren von Vorschlägen für die benutzerdefinierte Suche).

## <a name="endpoint"></a>Endpunkt
Um mithilfe der API für die benutzerdefinierte Bing-Suche vorgeschlagene Abfragen zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

## <a name="response-json"></a>JSON-Antwort
Die Antwort enthält eine Liste mit SearchAction-Objekten, in denen die vorgeschlagenen Abfragebegriffe enthalten sind.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Jeder Vorschlag enthält die Felder `displayText` und `query`. Das Feld `displayText` enthält die vorgeschlagene Abfrage, die Sie zum Auffüllen der Dropdownliste des Suchfelds verwenden.

Wenn der Benutzer in der Dropdownliste eine vorgeschlagene Abfrage auswählt, können Sie den Abfragebegriff im Feld `query` verwenden, wenn Sie die [API für die benutzerdefinierte Bing-Suche](overview.md) aufrufen.

## <a name="throttling-requests"></a>Drosselungsanforderungen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Aufrufen der benutzerdefinierten Suche](./search-your-custom-view.md)
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
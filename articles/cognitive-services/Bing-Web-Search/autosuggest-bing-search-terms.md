---
title: 'Bing-Vorschlagssuche: Suchbegriffe'
titleSuffix: Microsoft Cognitive Services
description: Durch Verbinden der Bing-Websuche-API mit der Bing-Vorschlagssuche-API können Sie eine verbesserte Suchoberfläche für Benutzer bereitstellen.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/13/2018
ms.author: erhopf
ms.openlocfilehash: df8a57b3136abfacce971f4d01ccb2296dfa784c
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889365"
---
# <a name="autosuggest-bing-search-terms"></a>Bing-Vorschlagssuche: Suchbegriffe

Wenn Sie ein Suchfeld bereitstellen, in das Benutzer ihre Suchbegriffe eingeben, verwenden Sie die [Bing-Vorschlagssuche-API](../bing-autosuggest/get-suggested-search-terms.md), um die Benutzerfreundlichkeit zu verbessern. Die API gibt vorgeschlagene Abfragezeichenfolgen zurück, während der Benutzer einen Suchbegriff eingibt.

Nachdem der Benutzer einen Suchbegriff eingegeben hat, muss dieser vor dem Festlegen des Abfrageparameters [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) URL-codiert werden. Wenn der Benutzer also beispielsweise *sailing dinghies* eingibt, legen Sie `q` auf `sailing+dinghies` oder `sailing%20dinghies` fest.

Wenn der Abfragebegriff einen Rechtschreibfehler enthält, umfasst die Suchantwort ein [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext)-Objekt. Das Objekt zeigt die ursprüngliche Schreibweise und die korrigierte Schreibweise, die Bing für die Suche verwendet hat.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

Diese Informationen können beim Anzeigen der Suchergebnisse verwendet werden, um Benutzer wissen zu lassen, dass die Abfragezeichenfolge geändert wurde.

![Beispielbenutzeroberfläche zum Abfragekontext](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Nächste Schritte  

* Sehen Sie sich das Beispiel [Antworten der Bing-Websuche-API](search-responses.md) an.  

## <a name="see-also"></a>Weitere Informationen  

* [Referenz zur Bing-Websuche-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)

---
title: Was ist die Bing-Rechtschreibprüfungs-API?
titlesuffix: Azure Cognitive Services
description: Die Bing-Rechtschreibprüfungs-API verwendet Machine Learning und statistische maschinelle Übersetzung für eine kontextbezogene Rechtschreibprüfung.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: 4caa05ffa96dbc15922fed85edfdefdb68ead68b
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361710"
---
# <a name="what-is-bing-spell-check-api"></a>Was ist die Bing-Rechtschreibprüfungs-API?

Mit der Bing-Rechtschreibprüfungs-API können Sie Grammatik und Rechtschreibung im Kontext überprüfen.

Was ist der Unterschied zwischen regulären Rechtschreibprüfungen und der Rechtschreibprüfung der dritten Generation von Bing? Bei aktuellen Rechtschreibprüfungen erfolgt die Rechtschreib- und Grammatikprüfung auf der Grundlage wörterbuchbasierter Regelsätze, die regelmäßig aktualisiert und erweitert werden. Die Qualität der Rechtschreibprüfung hängt also von der Qualität des zugrunde liegenden Wörterbuchs und von den Personen ab, die das Wörterbuch pflegen. Diese Art der Rechtschreibprüfung ist aus Microsoft Word und anderen Textverarbeitungsprogrammen bekannt.

Bing hat dagegen eine webbasierte Rechtschreibprüfung entwickelt, die Machine Learning und statistische maschinelle Übersetzung nutzt, um einen hochgradig kontextbezogenen Algorithmus dynamisch zu trainieren und kontinuierlich weiterzuentwickeln. Die Rechtschreibprüfung basiert auf einem enormen Korpus von Websuchvorgängen und Dokumenten.

Diese Rechtschreibprüfung eignet sich für jedes beliebige Textverarbeitungsszenario:

- Erkennung von Jargon und Umgangssprache
- Erkennung verbreiteter Namensfehler im Kontext
- Korrektur der Silbentrennung mit einem einzelnen Flag
- Korrektur von Homophonen im Kontext sowie anderer schwer zu erkennender Fehler
- Unterstützung neuer Marken, digitaler Unterhaltung und gängiger Ausdrücke, sobald sich diese verbreiten
- Wörter, die gleich klingen, aber eine unterschiedliche Schreibweise und Bedeutung haben (beispielsweise „viel“ und „fiel“).

## <a name="spell-check-modes"></a>Rechtschreibprüfungsmodi

Die API unterstützt zwei Prüfmodi: `Proof` und `Spell`.  Beispiele finden Sie [hier](https://azure.microsoft.com/services/cognitive-services/spell-check/).
### <a name="proof---for-documents-scenario"></a>Proof (für Dokumente)
Der Standardmodus ist `Proof`. Der Rechtschreibmodus `Proof` bietet die umfangreichsten Prüfungen – unter anderem mit Features für Groß-/Kleinschreibung, grundlegende Zeichensetzung und anderen hilfreichen Features für die Dokumenterstellung. Er ist allerdings nur für „en-US“ (Englisch – USA), „es-ES“ (Spanisch) und „pt-BR“ (Portugiesisch) verfügbar. (Hinweis: Für Spanisch und Portugiesisch steht nur eine Betaversion zur Verfügung.) Für alle anderen Märkte muss der Modusabfrageparameter auf „Spell“ festgelegt werden. 
<br /><br/>**Hinweis:** Wenn die Länge des Abfragetexts 4.096 Zeichen übersteigt, wird er vor der Verarbeitung auf 4.096 Zeichen gekürzt. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Spell (für Websuchvorgänge/-abfragen)
`Spell` ist aggressiver, um bessere Suchergebnisse zu liefern. Der Modus `Spell` findet zwar die meisten Rechtschreibfehler, aber nicht alle Grammatikfehler, die von `Proof` erkannt werden (beispielsweise Groß-/Kleinschreibung und Wortwiederholungen).
<br /></br>**HINWEIS:** Angaben zur maximal unterstützten Abfragelänge finden Sie im Anschluss. Ist die Abfrage zu lang, wird die Abfrage nicht geändert und das Ergebnis angezeigt.
<ul><li>130 Zeichen für folgende Sprachcodes: en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh, ko. </li>
<li>65 Zeichen für andere.</li></ul>

## <a name="market-setting"></a>Markteinstellung
Der Markt muss im Abfrageparameter der Anforderungs-URL angegeben werden. Andernfalls verwendet die Rechtschreibprüfung den Standardmarkt (basierend auf der IP-Adresse).


## <a name="post-vs-get"></a>Gegenüberstellung von POST und GET

Die API unterstützt HTTP POST oder HTTP GET. Welche dieser Optionen Sie verwenden, hängt von der Länge des zu prüfenden Texts ab. Wenn die Zeichenfolgen immer weniger als 1.500 Zeichen umfassen, verwenden Sie GET. Für Zeichenfolgen mit einer Länge von bis zu 10.000 Zeichen muss dagegen POST verwendet werden. Die Textzeichenfolge kann beliebige gültige UTF-8-Zeichen enthalten.

Das folgende Beispiel zeigt eine POST-Anforderung für die Überprüfung der Rechtschreibung und Grammatik einer Textzeichenfolge. Der Vollständigkeit halber enthält das Beispiel auch den Abfrageparameter [mode](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode). Dieser ist hier aber nicht unbedingt notwendig, da `mode` standardmäßig „Proof“ verwendet. Der Abfrageparameter [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) enthält die zu prüfende Zeichenfolge.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Bei Verwendung von HTTP GET muss der Abfrageparameter `text` in die Abfragezeichenfolge der URL eingefügt werden.
  
Das folgende Beispiel zeigt die Antwort auf die vorherige Anforderung. Die Antwort enthält ein Objekt vom Typ [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck). 
  
```  
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
Im Feld [flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) werden die Rechtschreib- und Grammatikfehler aufgeführt, die die API in der Zeichenfolge [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) gefunden hat. Das Feld `token` enthält das zu ersetzende Wort. Der nullbasierte Offset im Feld `offset` ermöglicht es, das Token in der Zeichenfolge `text` zu finden. Anschließend wird das Wort an dieser Position durch das Wort im Feld `suggestion` ersetzt. 

Ist das Feld `type` auf „RepeatedToken“ festgelegt, wird das Token weiterhin durch `suggestion` ersetzt, dabei muss aber wahrscheinlich auch das nachfolgende Leerzeichen entfernt werden.

## <a name="throttling-requests"></a>Drosselungsanforderungen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Nächste Schritte

Der Artikel [Erstellen Ihrer ersten Anforderung](quickstarts/csharp.md) ermöglicht einen schnellen Einstieg in die Verwendung Ihrer ersten Anforderung.

Machen Sie sich mit der [Referenz für die Bing-Rechtschreibprüfungs-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference) vertraut. Die Referenz enthält die Liste mit den Endpunkten, Headern und Abfrageparametern, die Sie zum Anfordern von Suchergebnissen verwenden können, sowie die Definitionen der Antwortobjekte. 

Machen Sie sich außerdem mit den [Verwendungs- und Anzeigeanforderungen von Bing](./useanddisplayrequirements.md) vertraut, um keine Regeln für die Verwendung der Ergebnisse zu verletzen.

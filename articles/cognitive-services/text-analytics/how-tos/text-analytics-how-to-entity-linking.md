---
title: Verwenden der Entitätsverknüpfung in der Textanalyse-REST-API (Microsoft Cognitive Services in Azure) | Microsoft-Dokumentation
description: In diesem Tutorial mit exemplarischer Vorgehensweise erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API von Microsoft Cognitive Services in Azure Entitäten identifizieren und auflösen.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: 55bec1a0223b70749a97a30e2da92ef15128038c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374514"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Erkennen von verknüpften Entitäten in der Textanalyse (Vorschauversion)

Für die [API für die Entitätsverknüpfung](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) wird unstrukturierter Text verwendet, und für jedes JSON-Dokument wird eine Liste mit eindeutig unterscheidbaren Entitäten und Links zu weiteren Informationen im Web (Wikipedia und Bing) zurückgegeben. 

## <a name="entity-linking-vs-named-entity-recognition"></a>Vergleich: Entitätsverknüpfung und Erkennung benannter Entitäten

Bei der Verarbeitung von natürlicher Sprache können die Konzepte der Entitätsverknüpfung und der Erkennung von benannten Entitäten (Named Entity Recognition, NER) leicht verwechselt werden. In der Vorschauversion des `entities`-Endpunkts der Textanalyse wird nur die Entitätsverknüpfung unterstützt.

Die Entitätsverknüpfung ist die Möglichkeit, die Identität einer im Text gefundenen Entität zu identifizieren und eindeutig zu machen (beispielsweise die Ermittlung, ob „Mars“ als Planet oder als römischer Kriegsgott verwendet wird). Für diesen Prozess ist das Vorhandensein einer Knowledge Base erforderlich, mit der erkannte Entitäten verknüpft sind. Wikipedia wird als Knowledge Base für den `entities`-Endpunkt der Textanalyse verwendet.

### <a name="language-support"></a>Sprachunterstützung

Zum Verwenden der Entitätsverknüpfung in verschiedenen Sprachen ist die Nutzung einer entsprechenden Knowledge Base in jeder Sprache erforderlich. Für die Entitätsverknüpfung in der Textanalyse bedeutet dies, dass jede Sprache, die vom `entities`-Endpunkt unterstützt wird, über einen Link zum entsprechenden Wikipedia-Corpus dieser Sprache verfügt. Da die Größe der Corpora zwischen Sprachen variiert, ist zu erwarten, dass auch der Abruf der Funktionalität für die Entitätsverknüpfung variiert.


## <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Informationen zu den derzeit unterstützten Sprachen finden Sie in [dieser Liste](../text-analytics-supported-languages.md).

Die Dokumentgröße darf 5.000 Zeichen pro Dokument nicht übersteigen, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel enthält eine Darstellung von Inhalten, die Sie an die Entitätsverknüpfung übermitteln können.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                },
               {"id": "2",
                "language": "en",
                "text": "The Seattle Seahawks won the Super Bowl in 2014."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Schritt 1: Strukturieren der Anforderung

Details zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine Anforderung vom Typ **POST**. Lesen Sie die API-Dokumentation für diese Anforderung: [API für Entitätsverknüpfung](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

+ Legen Sie den HTTP-Endpunkt für die Schlüsselbegriffserkennung fest. Er muss die Ressource `/entities` enthalten: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`.

+ Legen Sie einen Anforderungsheader fest, der den Zugriffsschlüssel für Textanalysevorgänge enthält. Weitere Informationen finden Sie unter [Ermitteln von Endpunkten und Zugriffsschlüsseln](text-analytics-how-to-access-key.md).

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634), um eine Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Der Dienst akzeptiert bis zu 100 Anforderungen pro Minute. Jede Anforderung darf maximal 1 MB groß sein.

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.

## <a name="step-3-view-results"></a>Schritt 3: Anzeigen der Ergebnisse

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Eigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Als Nächstes wird ein Beispiel für die Ausgabe der Entitätsverknüpfung angegeben:

```
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

Falls verfügbar, enthält die Antwort für jede erkannte Entität die Wikipedia-ID, die Wikipedia-URL und die Bing-ID. Diese Angaben können verwendet werden, um Ihre Anwendung mit Informationen zur verknüpften Entität zu erweitern.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Entitätsverknüpfung unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

+ Die [API für die Entitätsverknüpfung](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/entities` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](text-analytics-how-to-access-key.md) verwendet, der für Ihr Abonnement gültig ist.
+ Die Antwortausgabe, die aus verknüpften Entitäten besteht (z.B. Zuverlässigkeitsbewertungen, Offsets und Weblinks für jede Dokument-ID), kann in allen Anwendungen verwendet werden.

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

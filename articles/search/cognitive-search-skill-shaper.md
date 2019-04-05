---
title: Die Qualifikation „Shaper“ der kognitiven Suche – Azure Search
description: Extrahieren Sie Metadaten und strukturierte Informationen aus unstrukturierten Daten, und bringen Sie diese in die Form eines komplexen Typs in einer Azure Search-Anreicherungspipeline.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c55783e9b209a1280a21edca34b75e72481f4cb6
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806987"
---
#   <a name="shaper-cognitive-skill"></a>Der Skill „Shaper“

Die Qualifikation **Shaper** konsolidiert mehrere Eingaben in einem komplexen Typ, auf den später in der Anreicherungspipeline verwiesen werden kann. Im Wesentlichen ermöglicht es Ihnen der Skill **Shaper**, eine Struktur zu erstellen, den Namen der Elemente dieser Struktur zu definieren und jedem Element Werte zuzuweisen. Beispiele für konsolidierte Felder, die in Suchszenarien nützlich sind, sind die Kombination von Vor- und Nachnamen in einer einzigen Struktur, Stadt und Land in einer einzigen Struktur oder Name und Geburtsdatum in einer einzigen Struktur, um eine eindeutige Identität zu schaffen.

Standardmäßig unterstützt diese Methode Objekte, die nur über eine Ebene verfügen. Für komplexere Objekte können Sie mehrere **Shaper**-Schritte verketten.

In der Antwort lautet der Ausgabename immer „output“. Intern kann die Pipeline einen anderen Namen zuordnen (z. B. „analyzedText“ wie in den folgenden Beispielen), der Skill **Shaper** selbst gibt in der Antwort jedoch „output“ zurück. Dies kann wichtig sein, wenn Sie angereicherte Dokumente debuggen und die Namensdiskrepanz bemerken, oder wenn Sie einen benutzerdefinierten Skill erstellen und die Antwort selbst strukturieren.

> [!NOTE]
> Dieser Skill ist nicht an eine Cognitive Services-API gebunden, und Ihnen entstehen für seine Nutzung keine Kosten. Es wird jedoch empfohlen, dennoch eine [Cognitive Services-Ressource anzufügen](cognitive-search-attach-cognitive-services.md), um die Ressourcenoption **Free** außer Kraft zu setzen, durch die Sie auf eine geringe Anzahl von Anreicherungen pro Tag beschränkt werden.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Beispiel 1: Komplexe Typen

Stellen Sie sich ein Szenario vor, in dem Sie eine Struktur namens *analyzedText* erstellen möchten, die zwei Elemente hat: *text* und *sentiment*. In Azure Search wird ein mehrteiliges durchsuchbares Feld als *komplexer Typ* bezeichnet. Es wird noch nicht standardmäßig unterstützt. In dieser Vorschauversion kann ein **Shaper**-Skill verwendet werden, um Felder eines komplexen Typs in Ihrem Index zu generieren. 

Das folgende Beispiel stellt die Elementnamen als Eingabe bereit. Die Ausgabestruktur (Ihr komplexes Feld in Azure Search) wird über *targetName* festgelegt. 


```json
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "context": "/document/content/phrases/*",
  "inputs": [
    {
      "name": "text",
      "source": "/document/content/phrases/*"
    },
    {
      "name": "sentiment",
      "source": "/document/content/phrases/*/sentiment"
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": "analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Beispieleingabe
So könnte ein JSON-Dokument aussehen, das hilfreiche Eingabewerte für diesen Skill **Shaper** enthält:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="sample-output"></a>Beispielausgabe
Der Skill **Shaper** generiert ein neues Element namens *analyzedText* mit den kombinierten Elementen von *text* und *sentiment*. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="sample-2-input-consolidation"></a>Beispiel 2: Eingabekonsolidierung

In einem anderen Beispiel stellen Sie sich vor, dass Sie in verschiedenen Phasen der Pipelineverarbeitung den Titel eines Buchs und die Kapitelüberschriften auf verschiedenen Seiten des Buchs extrahiert haben. Sie können nun eine einzige Struktur aus diesen verschiedenen Eingaben erstellen.

Die Definition des Skills „Shaper“ für dieses Szenario könnte wie im folgenden Beispiel aussehen:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*"
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="sample-output"></a>Beispielausgabe
In diesem Fall vereinfacht „Shaper“ alle Kapiteltitel, um ein einzelnes Array zu erstellen. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)


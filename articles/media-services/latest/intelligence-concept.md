---
title: Abrufen von Azure-Medieninformationen | Microsoft-Dokumentation
description: Wenn Sie Azure Media Services verwenden, können Sie Ihre Audio- und Videoinhalte mit AudioAnalyzerPreset und VideoAnalyzerPreset analysieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: 804a418f6ee88974d6e74a2c18bc5d01b6adf838
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33783549"
---
# <a name="media-intelligence"></a>Medieninformationen

Die Azure Media Services REST 3-API ermöglicht Ihnen, Audio-und Videoinhalte zu analysieren. Um Ihren Inhalt zu analysieren, erstellen Sie eine **Transformation** und senden einen **Auftrag**, der eine dieser Voreinstellungen verwendet: **AudioAnalyzerPreset** oder **VideoAnalyzerPreset**. 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** ermöglicht Ihnen, mehrere Audioinformationen aus einer Audio- oder Videodatei zu extrahieren. Die Ausgabe enthält eine JSON-Datei (mit allen Informationen) und eine VTT-Datei für die Audiotranskription. Diese Voreinstellung akzeptiert eine Eigenschaft, die die Sprache der Eingabedatei in Form einer [BCP47](https://tools.ietf.org/html/bcp47)-Zeichenfolge angibt. Die Audioinformationen umfassen Folgendes:

* Audiotranskription: Ein Transkript der gesprochenen Worte mit Zeitstempeln. Mehrere Sprachen werden unterstützt.
* Sprecherindizierung: Eine Zuordnung der Sprecher und der entsprechenden gesprochenen Worte
* Standpunktanalyse der Sprache: Die Ausgabe der auf die Audiotranskription angewendeten Standpunktanalyse.
* Stichwörter: Aus der Audiotranskription extrahierte Schlüsselbegriffe.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** ermöglicht Ihnen, mehrere Audio- und Videoinformationen aus einer Videodatei zu extrahieren. Die Ausgabe enthält eine JSON-Datei (mit allen Informationen), eine VTT-Datei für die Videotranskription und eine Sammlung von Miniaturbildern. Diese Voreinstellung akzeptiert auch eine [BCP47](https://tools.ietf.org/html/bcp47)-Zeichenfolge (die die Sprache des Videos darstellt) als Eigenschaft. Die Videoinformationen umfassen alle oben genannten Audioinformationen sowie die folgenden zusätzlichen Elemente:

* Gesichtsverfolgung: Die Zeit, während der Gesichter im Video zu sehen sind. Jedes Gesicht weist eine Gesichts-ID und eine entsprechende Sammlung von Miniaturbildern auf.
* Sichtbarer Text: Der Text, der über optische Zeichenerkennung erkannt wird. Der Text ist mit einem Zeitstempel versehen und wird auch zum Extrahieren von Stichwörtern verwendet (zusätzlich zum Audiotranskript).
* Keyframes: Eine Sammlung von Keyframes, die aus dem Video extrahiert werden.
* Moderation von Videoinhalten: Die Teile der Videos, die als nicht für Jugendliche geeignet eingestuft wurden.
* Anmerkung: Das Ergebnis der Kommentierung der Videos anhand eines vordefinierten Objektmodells.

##  <a name="insightsjson-elements"></a>Elemente in „insights.json“

Die Ausgabe umfasst eine JSON-Datei (insights.json) mit allen Informationen, die im Video oder Audio gefunden wurden. Die JSON-Datei kann die folgenden Elemente enthalten:

### <a name="transcript"></a>Transkript

|NAME|Beschreibung|
|---|---|
|id|Die Zeilen-ID.|
|text|Das Transkript selbst.|
|language|Die Sprache des Transkripts. Vorgesehen zur Unterstützung von Transkripts, bei denen jede Zeile eine andere Sprache enthalten kann.|
|instances|Eine Liste der Zeitbereiche, in denen diese Zeile angezeigt wurde. Wenn die Instanz ein Transkript ist, gibt es nur 1 Instanz.|

Beispiel:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

### <a name="ocr"></a>ocr

|Name|Beschreibung|
|---|---|
|id|Die OCR-Zeilen-ID.|
|text|Der OCR-Text.|
|confidence|Die Zuverlässigkeit der Erkennung.|
|language|Die OCR-Sprache.|
|instances|Eine Liste der Zeitbereiche, in denen diese OCR angezeigt wurde (die gleiche OCR kann mehrfach vorkommen).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

### <a name="keywords"></a>keywords

|Name|Beschreibung|
|---|---|
|id|Die Stichwort-ID.|
|text|Der Stichworttext.|
|confidence|Die Zuverlässigkeit der Erkennung des Stichworts.|
|language|Die Sprache des Stichworts (sofern übersetzt).|
|instances|Eine Liste der Zeitbereiche, in denen dieses Stichwort angezeigt wurde (ein Stichwort kann mehrfach vorkommen).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

### <a name="faces"></a>faces

|name|Beschreibung|
|---|---|
|id|Die Gesichts-ID.|
|name|Der Name des Gesichts. Möglich sind „Unknown #0“, ein identifizierter Prominenter oder eine vom Kunden trainierte Person.|
|confidence|Die Zuverlässigkeit der Gesichtsidentifikation.|
|description|Bei einem Prominenten dessen Beschreibung (Satya Nadella wurde am ... geboren). |
|thumbnalId|Die ID des Miniaturbilds dieses Gesichts.|
|knownPersonId|Bei einer bekannten Person deren interne ID.|
|referenceId|Bei einem Bing-Prominenten dessen Bing-ID.|
|referenceType|Zurzeit nur Bing.|
|title|Bei einem Prominenten dessen Titel (z.B. „CEO von Microsoft“).|
|imageUrl|Bei einem Prominenten dessen Bild-URL.|
|instances|Dies sind Vorkommen des Gesichts in einem bestimmten Zeitbereich. Jedes Vorkommen hat auch eine Miniaturbild-ID. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

### <a name="labels"></a>labels

|Name|Beschreibung|
|---|---|
|id|Die Bezeichnungs-ID.|
|name|Der Bezeichnungsname (z. B. „Computer“, „TV“).|
|language|Die Sprache des Bezeichnungsnamens (sofern übersetzt). BCP-47|
|instances|Eine Liste der Zeitbereiche, in denen diese Bezeichnung angezeigt wurde (eine Bezeichnung kann mehrfach vorkommen). Jedes Vorkommen weist ein Zuverlässigkeitsfeld auf. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

### <a name="shots"></a>shots

|Name|BESCHREIBUNG|
|---|---|
|id|Die ID der Aufnahme.|
|keyFrames|Eine Liste mit Keyframes innerhalb der Aufnahme (jede verfügt über eine ID und eine Liste der Zeitbereiche der Vorkommen).|
|instances|Eine Liste der Zeitbereiche dieser Aufnahme (Aufnahmen kommen nur einmal vor).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="audioeffects"></a>audioEffects

|Name|Beschreibung|
|---|---|
|id|Die ID des Audioeffekts.|
|type|Der Typ des Audioeffekts (z. B. Geklatsche, Sprache, Stille).|
|instances|Eine Liste der Zeitbereiche, in denen dieser Audioeffekt vorkam.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```


### <a name="sentiments"></a>sentiments

Stimmungen werden anhand ihres Felds „SentimentType“ (neutral/positiv/negativ) aggregiert. Beispiel: 0-0,1, 0,1-0,2.

|Name|Beschreibung|
|---|---|
|id|Die Stimmungs-ID.|
|averageScore |Der Durchschnitt aller Bewertungen aller Vorkommen dieses Stimmungstyps: neutral/positiv/negativ|
|instances|Eine Liste der Zeitbereiche, in denen diese Stimmung vorkam.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Analysieren von Videos mit Azure Media Services](analyze-videos-tutorial-with-api.md)

---
title: Die analyzers-Methode in der API für die linguistische Analyse | Microsoft-Dokumentation
description: Die REST-API des Analysetools enthält eine Liste von Analysemodulen, die derzeit vom Dienst in Microsoft Cognitive Services unterstützt werden.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: 3fc243a0da77c5bae9009929f2b82e1353347752
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372843"
---
# <a name="analyzers-method"></a>analyzers-Methode

Die **analyzers**-REST-API enthält eine Liste der Analysetools, die derzeit vom Dienst unterstützt werden.
Die Antwort enthält ihre [Namen](Analyzer-Names.md) und die jeweils unterstützten Sprachen (z. B. „en“ für Englisch).

## <a name="request-parameters"></a>Anforderungsparameter
Keine

<br>

## <a name="response-parameters"></a>Antwortparameter
NAME | Typ | BESCHREIBUNG
-----|------|--------------
languages | Liste von Zeichenfolgen | Liste von aus zwei Buchstaben bestehenden ISO-Sprachcodes, für die das Analysetool verwendet werden können
id   | Zeichenfolge | eindeutige ID für dieses Analysetool
kind | Zeichenfolge | der allgemeine Typ des verwendeten Analysetools
specification | Zeichenfolge | der Name der für dieses Analysetool verwendeten Spezifikation
implementation | Zeichenfolge | Beschreibung des Modells und/oder des Algorithmus hinter diesem Analysetool

<br>
## <a name="example"></a>Beispiel
GET /analyzers

Antwort: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3", 
        "implementation": "SplitMerge"
    }, 
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags", 
        "specification": "PennTreebank3", 
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens", 
        "specification":"PennTreebank3", 
        "implementation": "regexes"
    } 
]
```

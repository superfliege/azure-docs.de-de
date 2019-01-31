---
title: Anforderungslimits – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel werden die Anforderungslimits für die Textübersetzungs-API aufgeführt. Die Gebühren werden basierend auf der Anzahl der Zeichen berechnet, nicht nach der Anforderungshäufigkeit, mit einem Grenzwert von 5.000 Zeichen pro Anforderung. Die Zeichengrenzwerte sind abonnementbasiert, wobei F0 auf 2 Millionen Zeichen pro Stunde beschränkt ist.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: erhopf
ms.openlocfilehash: 2115baae3f13c6ca6c0b9aca73d6fd43f5fcdbb4
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455710"
---
# <a name="request-limits-for-translator-text"></a>Anforderungslimits für die Textübersetzungs-API

In diesem Artikel werden die Drosselungslimits für die Textübersetzungs-API aufgeführt. Zu den Diensten gehören Übersetzung, Transliteration, Satzlängenerkennung, Spracherkennung und alternative Übersetzungen.

## <a name="character-limits-per-request"></a>Zeichengrenzwerte pro Anforderung

Jede Anforderung kann maximal 5.000 Zeichen lang sein. Die Abrechnung erfolgt nach der Anzahl der Zeichen, nicht nach der Anzahl der Anforderungen. Es wird empfohlen, kürzere Anforderungen zu senden und zu jedem Zeitpunkt einige ausstehende Anforderungen zu haben.

Es gibt keine Beschränkung für die Anzahl der ausstehenden Anforderungen für die Textübersetzungs-API.

## <a name="character-limits-per-hour"></a>Zeichengrenzwerte pro Stunde

Ihre Zeichengrenzwerte pro Stunde basieren auf der Abonnementebene der Textübersetzungs-API. Wenn Sie diese Grenzwerte erreichen oder überschreiten, erhalten Sie wahrscheinlich eine Antwort zur Kontingentüberschreitung:

| Tarif | Zeichengrenzwert |
|------|-----------------|
| F0 | 2 Millionen Zeichen pro Stunde |
| S1 | 40 Millionen Zeichen pro Stunde |
| S2 | 40 Millionen Zeichen pro Stunde |
| S3 | 120 Millionen Zeichen pro Stunde |
| S4 | 200 Millionen Zeichen pro Stunde |

Diese Grenzwerte gelten nur für die generischen Microsoft-Systeme. Für benutzerdefinierte Übersetzungssysteme, die den Translator-Hub von Microsoft verwenden, gilt ein Grenzwert von 1.800 Zeichen pro Sekunde.

## <a name="latency"></a>Latency

Die Latenz der Textübersetzungs-API beträgt maximal 13 Sekunden. Bis zu diesem Zeitpunkt müssen Sie ein Ergebnis oder eine Zeitlimitantwort erhalten haben. In der Regel werden die Antworten innerhalb von 150 Millisekunden bis 300 Millisekunden zurückgegeben. Die Antwortzeiten variieren je nach der Größe der Anforderung und dem Sprachpaar.

## <a name="sentence-length-limits"></a>Grenzwerte bei der Satzlänge

Bei Verwendung der [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence)-Funktion ist die Satzlänge auf 275 Zeichen beschränkt. Es gelten Ausnahmen für diese Sprachen:

| Sprache | Code | Zeichengrenzwert |
|----------|------|-----------------|
| Chinesisch | zh | 132 |
| Deutsch | de | 290 |
| Italienisch | it | 280 |
| Japanisch | ja | 150 |
| Portugiesisch | pt | 290 |
| Spanisch | es | 280 |
| Italienisch | it | 280 |
| Thailändisch | th | 258 |

> [!NOTE]
> Dieser Grenzwert gilt nicht für Übersetzungen.

## <a name="next-steps"></a>Nächste Schritte

* [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Regionale Verfügbarkeit](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [Referenz für Version 3 der Textübersetzungs-API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

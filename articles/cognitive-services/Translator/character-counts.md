---
title: Microsoft-Textübersetzungs-API – Zählen von Zeichen | Microsoft-Dokumentation
description: Es wird beschrieben, wie mit der Microsoft-Textübersetzungs-API Zeichen gezählt werden.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 1b4987509c17e4064d7c54608395e272efa8de3b
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41936761"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Zählen von Zeichen mit der Microsoft-Textübersetzungs-API

Microsoft Translator zählt jedes Zeichen der Eingabe. Hiermit sind Zeichen im Unicode-Sinn gemeint, keine Bytes. Unicode-Ersatzzeichen gelten als zwei Zeichen. Leerzeichen und Markup zählen als Zeichen. Die Länge der Antwort spielt keine Rolle.

Aufrufe der Methoden Detect und BreakSentence werden nicht in den Zeichenverbrauch eingerechnet. Es wird aber erwartet, dass die Aufrufe der Methoden Detect und BreakSentence in einem angemessenen Verhältnis zur Verwendung anderer zu zählender Funktionen stehen. Microsoft behält sich das Recht vor, mit dem Zählen von Detect und BreakSentence zu beginnen. 

Weitere Informationen zur Zeichenzählung finden Sie unter [Microsoft Translator FAQ](https://www.microsoft.com/en-us/translator/faq.aspx) (Microsoft Translator – Häufig gestellte Fragen).

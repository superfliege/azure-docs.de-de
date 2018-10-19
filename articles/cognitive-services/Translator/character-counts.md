---
title: Textübersetzungs-API – Zählen von Zeichen
titlesuffix: Azure Cognitive Services
description: Es wird beschrieben, wie mit der Textübersetzungs-API Zeichen gezählt werden.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c6234a46ae55d73739dcc23110c5e0f6375c3f96
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128739"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Zählen von Zeichen mit der Textübersetzungs-API

Die Textübersetzungs-API zählt jedes Zeichen der Eingabe. Hiermit sind Zeichen im Unicode-Sinn gemeint, keine Bytes. Unicode-Ersatzzeichen gelten als zwei Zeichen. Leerzeichen und Markup zählen als Zeichen. Die Länge der Antwort spielt keine Rolle.

Aufrufe der Methoden Detect und BreakSentence werden nicht in den Zeichenverbrauch eingerechnet. Es wird aber erwartet, dass die Aufrufe der Methoden Detect und BreakSentence in einem angemessenen Verhältnis zur Verwendung anderer zu zählender Funktionen stehen. Microsoft behält sich das Recht vor, mit dem Zählen von Detect und BreakSentence zu beginnen. 

Weitere Informationen zur Zeichenzählung finden Sie unter [Microsoft Translator FAQ](https://www.microsoft.com/en-us/translator/faq.aspx) (Microsoft Translator – Häufig gestellte Fragen).

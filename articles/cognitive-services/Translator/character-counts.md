---
title: Zählen von Zeichen – Textübersetzungs-API
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
ms.openlocfilehash: 8e04158d34765b8a077fc56f2108ea6d7b4b03a6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649193"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Zählen von Zeichen mit der Textübersetzungs-API

Die Textübersetzungs-API zählt jedes Zeichen der Eingabe. Hiermit sind Zeichen im Unicode-Sinn gemeint, keine Bytes. Unicode-Ersatzzeichen gelten als zwei Zeichen. Leerzeichen und Markup zählen als Zeichen. Die Länge der Antwort spielt keine Rolle.

Aufrufe der Methoden Detect und BreakSentence werden nicht in den Zeichenverbrauch eingerechnet. Es wird aber erwartet, dass die Aufrufe der Methoden Detect und BreakSentence in einem angemessenen Verhältnis zur Verwendung anderer zu zählender Funktionen stehen. Microsoft behält sich das Recht vor, mit dem Zählen von Detect und BreakSentence zu beginnen.

Weitere Informationen zur Zeichenzählung finden Sie unter [Microsoft Translator FAQ](https://www.microsoft.com/en-us/translator/faq.aspx) (Microsoft Translator – Häufig gestellte Fragen).

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
ms.openlocfilehash: ebe3e3606a0413730e1fbfd704a6403f77275f89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374339"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Zählen von Zeichen mit der Microsoft-Textübersetzungs-API

Microsoft Translator zählt jedes Zeichen der Eingabe. Hiermit sind Zeichen im Unicode-Sinn gemeint, keine Bytes. Unicode-Ersatzzeichen gelten als zwei Zeichen. Leerzeichen und Markup zählen als Zeichen. Die Länge der Antwort spielt keine Rolle.

Aufrufe der Methoden Detect und BreakSentence werden nicht in den Zeichenverbrauch eingerechnet. Es wird aber erwartet, dass die Aufrufe der Methoden Detect und BreakSentence in einem angemessenen Verhältnis zur Verwendung anderer zu zählender Funktionen stehen. Microsoft behält sich das Recht vor, mit dem Zählen von Detect und BreakSentence zu beginnen. 

Translate ermöglicht eine automatische Erkennung, wenn Sie den Sprachparameter „From“ weglassen. 

Weitere Informationen zur Zeichenzählung finden Sie unter [Microsoft Translator FAQ](https://www.microsoft.com/en-us/translator/faq.aspx) (Microsoft Translator – Häufig gestellte Fragen).

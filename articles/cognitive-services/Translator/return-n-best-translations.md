---
title: Zurückgeben von N besten Übersetzungen – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Zurückgeben von N besten Übersetzungen mit der Textübersetzungs-API von Microsoft
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a0f4e4dd5cea9c3ea7b682e6372d2ffa8726e6ef
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646472"
---
# <a name="how-to-return-n-best-translations"></a>Zurückgeben von N besten Übersetzungen

> [!NOTE]
> Diese Methode ist veraltet. Sie ist unter V3.0 der Textübersetzungs-API nicht verfügbar.

Die Methoden GetTranslations() und GetTranslationsArray() der Microsoft Translator-API enthalten ein optionales boolesches Flag „IncludeMultipleMTAlternatives“.
Die Methode gibt bis zu „maxTranslations“ Alternativen zurück, wobei das Delta aus der Übersetzer-Engine-Liste mit den N besten Übersetzungen stammt.

Die Signatur ist:

**Syntax**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parameter**

| Parameter | BESCHREIBUNG |
|:---|:---|
| appId | **Erforderlich** Lassen Sie das Feld „appId“ leer, wenn sie den Autorisierungsheader verwenden. Geben Sie andernfalls eine Zeichenfolge mit „Bearer“ + „ “ + Zugriffstoken an.|
| text | **Erforderlich** Eine Zeichenfolge, die den Text darstellt, der übersetzt werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|
| from | **Erforderlich** Eine Zeichenfolge, die den Code der Sprache darstellt, aus der der Text übersetzt werden soll. |
| zu | **Erforderlich** Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll. |
| maxTranslations | **Erforderlich** Ein Integer, der die maximale Anzahl von Übersetzungen darstellt, die zurückgegeben werden sollen. |
| options | **Optional** Ein TranslateOptions-Objekt, das die unten aufgeführten Werte enthält. Diese sind alle optional und entsprechen den Standardwerten für die häufigsten Einstellungen.

* Category: „general“ ist der einzige unterstützte und standardmäßige Wert.
* ContentType: „text/plain“ ist der einzige unterstützte und standardmäßige Wert.
* State: Mit diesem Benutzerstatus können Anforderung und Antwort korreliert werden. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* IncludeMultipleMTAlternatives: Dieses Flag bestimmt, ob mehr als eine Alternative aus der MT-Engine zurückgegeben wird. Der Standardwert ist „FALSE“ und enthält nur eine Alternative.

## <a name="ratings"></a>Ratings
Bewertungen werden folgendermaßen angewendet: Die beste automatische Übersetzung hat die Bewertung von 5.
Die automatisch generierten (N besten) Übersetzungsalternativen haben eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.

## <a name="number-of-alternatives"></a>Anzahl von Alternativen
Die Anzahl zurückgegebener Alternativen beträgt höchstens „maxTranslations“.

## <a name="language-pairs"></a>Sprachenpaare
Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch (in beide Richtungen) nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.

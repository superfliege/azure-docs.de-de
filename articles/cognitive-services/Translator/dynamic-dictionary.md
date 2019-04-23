---
title: Dynamisches Wörterbuch – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die Funktion für das dynamische Wörterbuch der Textübersetzungs-API.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: 0b4362d78ef105c249aafb4c6b203f69754a56c8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58916596"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Verwenden der Funktion für das dynamische Wörterbuch der Textübersetzungs-API

Wenn Ihnen die Übersetzung eines Worts oder eines Ausdrucks bereits bekannt ist, können Sie diese als Markup in der Anforderung angeben. Das dynamische Wörterbuch ist nur für zusammengesetzte Substantive wie Eigennamen und Produktnamen sicher.

**Syntax:**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**Beispiel: en-de:**

Quelleingabe: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

Zielausgabe: Das Wort „wordomatic“ ist ein Wörterbucheintrag.

Diese Funktion lässt sich gleichermaßen mit und ohne HTML-Modus ausführen.

Sie sollte in Maßen eingesetzt werden. Für die Anpassung einer Übersetzung ist Custom Translator deutlich besser geeignet. Custom Translator nutzt den Kontext und statistische Wahrscheinlichkeiten in vollem Umfang. Sie erhalten viel bessere Ergebnisse, wenn Sie über Trainingsdaten verfügen oder Trainingsdaten erstellen können, die den Kontext des Worts oder des Ausdrucks zeigen. Weitere Informationen zu Custom Translator finden Sie unter [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).

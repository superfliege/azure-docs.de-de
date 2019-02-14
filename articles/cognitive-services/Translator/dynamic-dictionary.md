---
title: Dynamisches Wörterbuch – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die Funktion für das dynamische Wörterbuch der Textübersetzungs-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 6140361d3cffaf7938aa8f24b6f0c3d27784af88
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55876010"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Verwenden der Funktion für das dynamische Wörterbuch der Textübersetzungs-API

Wenn Ihnen die Übersetzung eines Worts oder eines Ausdrucks bereits bekannt ist, können Sie diese als Markup in der Anforderung angeben. Das dynamische Wörterbuch ist nur für zusammengesetzte Substantive wie Eigennamen und Produktnamen sicher.

**Syntax:**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**Beispiel: en-de:**

Quelleingabe: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

Zielausgabe: Das Wort „wordomatic“ ist ein Wörterbucheintrag.

Diese Funktion lässt sich gleichermaßen mit und ohne HTML-Modus ausführen.

Sie sollte in Maßen eingesetzt werden. Für die Anpassung einer Übersetzung ist Custom Translator deutlich besser geeignet. Custom Translator nutzt den Kontext und statistische Wahrscheinlichkeiten in vollem Umfang. Sie erhalten viel bessere Ergebnisse, wenn Sie über Trainingsdaten verfügen oder Trainingsdaten erstellen können, die den Kontext des Worts oder des Ausdrucks zeigen. Weitere Informationen zu Custom Translator finden Sie unter [http://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).

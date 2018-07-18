---
title: Dynamisches Wörterbuch der Textübersetzungs-API von Microsoft | Microsoft-Dokumentation
description: Verwenden der Funktion für das dynamische Wörterbuch der Textübersetzungs-API von Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a18348c9786669ac41c4e149577d97cd631d5531
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35378412"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Verwenden der Funktion für das dynamische Wörterbuch der Textübersetzungs-API von Microsoft

Wenn Ihnen die Übersetzung eines Worts oder eines Ausdrucks bereits bekannt ist, können Sie diese als Markup in der Anforderung angeben. Das dynamische Wörterbuch ist nur für zusammengesetzte Substantive wie Eigennamen und Produktnamen sicher. 

**Syntax:** 

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**Beispiel: en-de:**

Quelleingabe: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

Zielausgabe: Das Wort „wordomatic“ ist ein Wörterbucheintrag.

Diese Funktion lässt sich gleichermaßen mit und ohne HTML-Modus ausführen. 

Sie sollte in Maßen eingesetzt werden. Der Microsoft Translator Hub ist für die Anpassung einer Übersetzung deutlich besser geeignet. Er nutzt den Kontext und statistische Wahrscheinlichkeiten in vollem Umfang. Sie erhalten viel bessere Ergebnisse, wenn Sie über Trainingsdaten verfügen oder sich die Erstellung von Trainingsdaten leisten können, die den Kontext des Worts oder des Ausdrucks zeigen. Weitere Informationen zum Hub finden Sie unter [http://hub.microsofttranslator.com](http://hub.microsofttranslator.com).


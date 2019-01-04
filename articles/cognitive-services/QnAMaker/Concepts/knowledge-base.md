---
title: 'Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 865525d9c978f3168a4c580dec20f0f91f0a0d16
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074075"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Was ist eine QnA Maker-Wissensdatenbank?

Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.

## <a name="key-knowledge-base-concepts"></a>Wichtige Konzepte für Wissensdatenbanken

* **Fragen**: Eine Frage enthält Text, der eine Benutzerabfrage am besten darstellt. 
* **Antworten**: Eine Antwort ist die Reaktion, die zurückgegeben wird, wenn eine Benutzerabfrage mit der zugehörigen Frage übereinstimmt.  
* **Metadaten**: Metadaten sind Tags, die einem QnA-Paar zugeordnet sind und als Schlüssel-Wert-Paare dargestellt werden. Metadaten werden verwendet, um QnA-Paare zu filtern und die Datenmenge einzuschränken, für die ein Abfrageabgleich vorgenommen wird.

Eine einzelnes QnA-Element, dargestellt durch eine numerische QnA-ID, verfügt über mehrere Varianten einer Frage (alternative Fragen), die alle auf einer einzigen Antwort abgebildet werden. Außerdem können jedem dieser Paare mehrere Metadatenfelder zugeordnet sein.

![QnA Maker-Wissensdatenbanken](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhaltsformat der Wissensdatenbank

Wenn Sie reichhaltige Inhalte in einer Wissensdatenbank erfassen, versucht QnA Maker, die Inhalte in Markdown zu konvertieren. Lesen Sie [diesen](https://aka.ms/qnamaker-docs-markdown-support) Blog, um die Markdownformate zu verstehen, die von den meisten Chatclients verstanden werden.

Metadatenfelder bestehen aus Schlüssel-Wert-Paaren, die durch einen Doppelpunkt **(Produkt:Aktenvernichter)** getrennt werden. Schlüssel und Wert müssen ausschließlich aus Text bestehen. Der Metadatenschlüssel darf keine Leerzeichen enthalten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklungslebenszyklus einer Wissensdatenbank](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

[Übersicht über QnA Maker](../Overview/overview.md)
---
title: Übersicht über die Web Language Model-API
titleSuffix: Azure Cognitive Services
description: Die Web Language Model API in Microsoft Cognitive Services bietet moderne Tools für die Verarbeitung natürlicher Sprache.
services: cognitive-services
author: piyushbehre
manager: cgronlun
ms.service: cognitive-services
ms.component: web-language-model
ms.topic: overview
ms.date: 08/12/2016
ms.author: pibehre
ROBOTS: NOINDEX
ms.openlocfilehash: 3c7f9e4cc4cf023aa93bd80ad1165ba9595e9034
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807436"
---
# <a name="what-is-the-web-language-model-api-preview"></a>Worum handelt es sich bei der Websprachmodell-API?  (Vorschau)

> [!IMPORTANT]
> Die Vorschauversion von Web Language Model wurde am 9. August 2018 außer Betrieb gesetzt. Es wird empfohlen, [Azure Machine Learning-Textanalysemodule](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) für die Textverarbeitung und -analyse zu verwenden.

Die Microsoft Web Language Model API ist ein REST-basierter Clouddienst, der moderne Tools für die Verarbeitung natürlicher Sprache bereitstellt. Mit dieser API kann Ihre Anwendung die Leistungsfähigkeit großer Datenmengen durch Sprachmodelle nutzen, die auf webbasierten Korpora trainiert wurden, die von Bing auf dem US-amerikanischen Markt erfasst wurden.

Diese geglätteten Backoff-N-Gramm-Sprachmodelle, die Markov-Ketten bis zu fünfter Ordnung unterstützen, werden mit den folgenden Korpora trainiert:

- Haupttext auf Webseiten
- Titeltext auf Webseiten
- Ankertext auf Webseiten
- Abfragetext für Websuchvorgänge

Die Web Language Model API unterstützt vier Suchvorgänge:

1. Verbundene (log10) Wahrscheinlichkeit einer Sequenz von Wörtern.
2. Bedingte (log10) Wahrscheinlichkeit eines Worts anhand einer Sequenz von vorherigen Wörtern.
3. Liste mit Wörtern (Vervollständigungen), die mit der größten Wahrscheinlichkeit auf eine bestimmte Wortsequenz folgen.
4. Worttrennung von Zeichenfolgen, die keine Leerzeichen enthalten.

## <a name="getting-started"></a>Erste Schritte

1. Abonnieren Sie den Dienst.
2. Laden Sie das [SDK](https://www.github.com/microsoft/cognitive-weblm-windows) herunter.
3. Führen Sie den SDK-Beispielcode aus.
4. Vollständige Details zu den Endpunkten (einschließlich der Codeausschnitte in einer Vielzahl von Sprachen) finden Sie in der [API-Referenz](http://web.archive.org/web/20170503191852/westus.dev.cognitive.microsoft.com/docs/services/55de9ca4e597ed1fd4e2f104/operations/55de9ca4e597ed19b0de8a51).

## <a name="underlying-technology"></a>Zugrunde liegende Technologie

Das folgende Dokument enthält Einzelheiten zur Entwicklung dieser Sprachmodelle und sollte in Forschungspublikationen, die diesen Dienst nutzen, zitiert werden:

- [Eine Übersicht über N-Gramm-Korpus Microsoft Web und Anwendungen](https://research.microsoft.com/apps/pubs/default.aspx?id=130762), NAACL-HLT 2010

Klicken Sie [hier](https://academic.microsoft.com/#/search?iq=And%28Ty%3D'0'%2CRId%3D2145833060%29&q=papers%20citing%20an%20overview%20of%20microsoft%20web%20n%20gram%20corpus%20and%20applications&filters=&from=0&sort=0), um eine aktuelle Liste von Dokumenten abzurufen, die dieses Dokument zitieren.

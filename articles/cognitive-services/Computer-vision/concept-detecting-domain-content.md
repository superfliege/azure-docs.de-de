---
title: Erkennen domänenspezifischer Inhalte – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Beschreibung von Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: b197227b6550703e4cc303fc800839dc48bf2d1c
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985208"
---
# <a name="detecting-domain-specific-content"></a>Erkennen domänenspezifischer Inhalte

Außer dem Tagging und der Kategorisierung auf oberster Ebene unterstützt das maschinelle Sehen auch spezielle (oder domänenspezifische) Informationen. Spezielle Informationen kann als eigenständige Methode oder mit der allgemeinen Kategorisierung implementiert werden. Sie ermöglichen eine weitere Optimierung der 86-Kategorien-Taxonomie durch das Hinzufügen domänenspezifischer Modelle.

Es gibt zwei Optionen für die Verwendung der domänenspezifischen Modelle:

* Bereichsbezogene Analyse  
  Analyse nur eines ausgewählten Modells durch einen HTTP POST-Aufruf. Wenn Sie wissen, welches Modell Sie verwenden möchten, geben Sie den Namen des Modells an. Sie erhalten nur Informationen, die für dieses Modell relevant sind. Sie können diese Option beispielsweise verwenden, um nur nach berühmten Personen zu suchen. Die Antwort enthält eine Liste möglicher Übereinstimmungen mit berühmten Personen zusammen mit der jeweiligen Zuverlässigkeitsbewertung.
* Erweiterte Analyse  
  Analyse zur Bereitstellung zusätzlicher Details im Zusammenhang mit Kategorien der 86-Kategorien-Taxonomie. Diese Option steht zur Verwendung in Anwendungen bereit, bei denen Benutzer zusätzlich zu den Details aus einem oder mehreren domänenspezifischen Modellen auch eine generische Bildanalyse erhalten möchten. Bei Aufruf dieser Methode wird zuerst die Klassifizierung der 86-Kategorien-Taxonomie aufgerufen. Wenn eine der Kategorien mit der eines bekannten oder übereinstimmenden Modells übereinstimmt, wird die Klassifizierung ein zweites Mal aufgerufen. Wenn z. B. der `details`-Parameter des HTTP-POST-Aufrufs entweder auf „all“ festgelegt ist oder „celebrities“ enthält, ruft die Methode die Klassifizierung für berühmte Personen auf, nachdem die 86-Kategorien-Klassifizierung aufgerufen wurde. Wenn das Bild als `people_` oder als eine Unterkategorie dieser Kategorie klassifiziert ist, wird die Klassifikation für berühmte Personen aufgerufen.

## <a name="listing-domain-specific-models"></a>Auflisten domänenspezifischer Modelle

Sie können die vom maschinellen Sehen unterstützten domänenspezifischen Modelle auflisten. Derzeit unterstützt das maschinelle Sehen die folgenden domänenspezifischen Modelle zur Erkennung von domänenspezifischen Inhalten:

| NAME | BESCHREIBUNG |
|------|-------------|
| Prominente | Erkennung berühmter Personen, die für Bilder unterstützt wird, die in die Kategorie `people_` klassifiziert werden |
| Wahrzeichen | Erkennung von Wahrzeichen, die für Bilder unterstützt wird, die in die Kategorien `outdoor_` oder `building_` klassifiziert werden |

### <a name="domain-model-list-example"></a>Beispiel für die Domänenmodellliste

Die folgende JSON-Antwort listet die vom maschinellen Sehen unterstützten domänenspezifischen Modelle auf.

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Kategorisieren von Bildern](concept-categorizing-images.md).
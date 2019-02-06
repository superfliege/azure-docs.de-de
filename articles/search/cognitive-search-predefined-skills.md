---
title: Integrierte Datenextraktion, natürliche Sprache, Bildverarbeitung – Azure Search
description: Durch kognitive Qualifikationen für Datenextraktion, natürliche Sprache und Bildverarbeitung erhält der Rohdateninhalt in einer Azure Search-Pipeline Semantik und Struktur.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 8f87cb68aeea8b953b9883e92fd5080569c6cdd0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203690"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Vordefinierte Skills für die Inhaltsanreicherung (Azure Search)

In diesem Artikel erfahren Sie mehr über die kognitiven Skills, die mit Azure Search bereitgestellt werden. Ein *kognitiver Skill* ist ein Vorgang, der Inhalte in irgendeiner Weise transformiert. Oft handelt es sich um eine Komponente, die Daten extrahiert oder die Struktur ableitet und Eingabedaten somit verständlicher macht. Die Ausgabe erfolgt fast immer textbasiert. Ein *Skillset* ist eine Sammlung von Skills, die die Anreicherungspipeline definieren. 

> [!NOTE]
> Ab dem 21. Dezember 2018 können Sie Cognitive Services-Ressourcen einer Azure Search-Qualifikationsgruppe zuordnen. Dies ermöglicht uns, für die Ausführung von Qualifikationsgruppen mit der Gebührenberechnung zu beginnen. Außerdem beginnen wir an diesem Datum damit, die Bildextraktion als Teil der Aufschlüsselung von Dokumenten zu berechnen. Die Textextraktion aus Dokumenten wird weiterhin ohne Zusatzkosten angeboten.
>
> Die Ausführung interner Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion entsprechen den Vorschaupreisen. Sie werden auf der [Preisseite von Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400) beschrieben. [Weitere Informationen](cognitive-search-attach-cognitive-services.md).

## <a name="predefined-skills"></a>Vordefinierte Skills

Mehrere Skills sind flexibel in dem, was sie verwenden oder generieren. Im Allgemeinen basieren die meisten Skills auf vortrainierten Modellen, d.h. Sie können das Modell nicht mit Ihren eigenen Trainingsdaten trainieren. Eine Anleitung zum Erstellen eines benutzerdefinierten Skills finden Sie unter [Definieren einer benutzerdefinierten Schnittstelle](cognitive-search-custom-skill-interface.md) und [Beispiel: Erstellen eines benutzerdefinierten Skills](cognitive-search-create-custom-skill-example.md). In der folgenden Tabelle werden die von Microsoft bereitgestellten Skills aufgelistet und beschrieben. 

| Skill | BESCHREIBUNG |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Dieser Skill verwendet ein vortrainiertes Modell, um wichtige Phrasen zu erkennen, die auf der Platzierung von Begriffen, sprachlichen Regeln, der Nähe zu anderen Begriffen und der Ungewöhnlichkeit des Begriffs innerhalb der Quelldaten basieren. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um die verwendete Sprache zu erkennen (eine Sprachen-ID pro Dokument). Wenn mehrere Sprachen in den gleichen Textsegmenten verwendet werden, wird die LCID der vorwiegend verwendeten Sprache ausgegeben.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Konsolidiert Text aus einer Sammlung von Feldern in einem einzigen Feld.  |
| [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) | Dieser Skill verwendet ein vortrainiertes Modell, um Entitäten für eine feste Gruppe von Kategorien zu erstellen: Personen, Ort, Organisation, E-Mails, URLs, datetime-Felder. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um eine positive oder negative Stimmung anhand von Datensätzen zu bewerten. Der Wert liegt zwischen 0 und 1. Neutrale Bewertungen treten sowohl im Fall „0“ auf, wenn die Stimmung nicht erkannt werden kann, als auch für Text, der als neutral angesehen wird.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Teilt Text in Seiten auf, damit Sie Inhalt inkrementell anreichern oder erweitern können. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Dieser Skill verwendet einen Bilderkennungsalgorithmus, um den Inhalt eines Bildes zu identifizieren und eine Textbeschreibung zu erzeugen. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optische Zeichenerkennung |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Ordnet die Ausgabe einem komplexen Typ zu (ein mehrteiliger Datentyp, der für einen vollständigen Namen, eine mehrzeilige Adresse oder eine Kombination aus Nachname und persönlicher ID verwendet werden kann). |

## <a name="see-also"></a>Weitere Informationen

+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Schnittstellendefinition für benutzerdefinierte Skills](cognitive-search-custom-skill-interface.md)
+ [Tutorial: Angereicherte Indizierung mit kognitiver Suche](cognitive-search-tutorial-blob.md)
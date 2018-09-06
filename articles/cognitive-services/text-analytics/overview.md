---
title: Übersicht über die Textanalyse – Azure Cognitive Services | Microsoft-Dokumentation
description: Textanalyse in Azure Cognitive Services für Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung und Entitätsverknüpfung
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 8/30/2018
ms.author: ashmaka
ms.openlocfilehash: 71af2bcbf58279681bbea66d4f76e951a2efce59
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341485"
---
# <a name="what-is-text-analytics"></a>Was ist Textanalyse?

Der Textanalysedienst bietet eine erweiterte Verarbeitung von unformatiertem, unstrukturiertem Text in natürlicher Sprache. Er umfasst vier Hauptfunktionen: Standpunktanalyse, Schlüsselbegriffserkennung. Sprachenerkennung und Entitätsverknüpfung.

## <a name="analyze-sentiment"></a>Analysieren von Stimmungen

[Ermitteln Sie](how-tos/text-analytics-how-to-sentiment-analysis.md), was die Kunden von Ihrer Marke oder von Ihrem Thema halten, indem Sie unformatierten Text auf Hinweise für positive oder negative Standpunkte analysieren. Die API gibt für jedes Dokument eine Bewertung des Standpunkts zwischen 0 und 1 zurück. Hierbei entspricht 1 einer positiven Bewertung.<br />
Die Analysemodelle sind mithilfe großer Textmengen und Microsoft-Technologien für natürliche Sprache vortrainiert. Für [ausgewählte Sprachen](text-analytics-supported-languages.md) kann die API jeden bereitgestellten unformatierten Text analysieren und bewerten.

## <a name="extract-key-phrases"></a>Extrahieren von Schlüsselbegriffen

[Extrahieren Sie automatisch Schlüsselbegriffe](how-tos/text-analytics-how-to-keyword-extraction.md), um schnell die wichtigsten Punkte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt der Textanalysedienst die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück.

## <a name="detect-language"></a>Sprache erkennen

Dieses Feature [erkennt](how-tos/text-analytics-how-to-language-detection.md) die Sprache von Eingabetexten für bis zu 120 Sprachen und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt.

## <a name="identify-linked-entities-preview"></a>Erkennen von verknüpften Entitäten (Vorschauversion)

[Identifizieren](how-tos/text-analytics-how-to-entity-linking.md) Sie bekannte Entitäten in Ihrem Text, und fügen Sie Links zu weiteren Informationen im Internet hinzu. Die Entitätsverknüpfung erkennt und unterscheidet, wenn ein Begriff als eine von mehreren voneinander unterscheidbaren Entitäten, Verben oder anderen Wortformen verwendet wird.

## <a name="typical-workflow"></a>Typischer Workflow

Der Workflow ist einfach: Sie übermitteln Daten für die Analyse und verarbeiten Ausgaben in Ihrem Code. Analysetool werden unverändert (d.h. ohne Konfigurationen oder Anpassungen) verwendet.

1. [Registrieren Sie sich](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) für einen [Zugriffsschlüssel](how-tos/text-analytics-how-to-access-key.md). Der Schlüssel muss bei jeder Anforderung übergeben werden.

2. [Erstellen Sie eine Anforderung](how-tos/text-analytics-how-to-call-api.md#json-schema) in JSON, die Ihre Daten als unformatierten, unstrukturierten Text enthält.

3. Übermitteln Sie die Anforderung an den Endpunkt, der während der Registrierung eingerichtet wurde, und fügen Sie die aufzurufende API an: Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung oder Entitätsidentifikation.

4. Streamen oder speichern Sie die Antwort lokal. Je nach Anforderung ist das Ergebnis entweder eine Standpunktbewertung, eine Sammlung von extrahierten Schlüsselbegriffen oder ein Sprachcode.

Die Ausgabe wird als einzelnes JSON-Dokument mit Ergebnissen für jedes bereitgestellte Textdokument zurückgegeben und basiert auf der ID. Sie können die Ergebnisse anschließend analysieren, visualisieren oder in verwertbare Erkenntnisse kategorisieren.

Vom Textanalysedienst ausgeführte Vorgänge sind zustandslos. Daten werden nicht in Ihrem Konto gespeichert.

<a name="data-limits"></a>

## <a name="specifications"></a>Spezifikationen

### <a name="supported-languages"></a>Unterstützte Sprachen

Siehe [Unterstützte Sprachen in der Textanalyse-API](text-analytics-supported-languages.md).

### <a name="data-limits"></a>Datengrenzwerte

Alle Endpunkte des Textanalysediensts akzeptieren unformatierte Textdaten. Der aktuelle Grenzwert liegt bei 5.000 Zeichen für jedes Dokument. Wenn Sie größere Dokumente analysieren müssen, können Sie diese in kleinere Blöcke aufteilen. Wenn Sie dennoch einen höheren Grenzwert benötigen, [kontaktieren Sie uns](https://azure.microsoft.com/overview/sales-number/), damit Ihre Anforderungen besprochen werden können.

| Begrenzung | Wert |
|------------------------|---------------|
| Maximale Größe eines einzelnen Dokuments | 5.000 Zeichen (von `String.Length` gemessen) |
| Maximale Größe der gesamte Anforderung | 1 MB |
| Maximale Anzahl von Dokumenten in einer Anforderung | 1.000 Dokumente |

Das Ratenlimit beträgt 100 Aufrufe pro Minute. Beachten Sie, dass Sie viele Dokumente (bis zu 1000) in einem einzigen Aufruf übermitteln können.

### <a name="unicode-encoding"></a>Unicode-Codierung

Der Textanalysedienst verwendet Unicode-Codierung für die Textdarstellung und die Berechnung der Zeichenanzahl. Sie können Anforderungen entweder in UTF-8 oder UTF-16 übermitteln, dabei gibt es keine messbaren Unterschiede bei der Zeichenanzahl. Wenn Sie `String.Length` verwenden, um die Zeichenanzahl abzurufen, verwenden Sie die gleiche Methode wie wir, um die Datengröße zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie zunächst die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Sie können eine Texteingabe (maximal 5.000 Zeichen) einfügen, um die Sprache (bis zu 120 verschiedene) zu erkennen, eine Standpunktbewertung zu berechnen, Schlüsselbegriffe zu extrahieren oder verknüpfte Entitäten zu identifizieren. Eine Registrierung ist nicht erforderlich.

Wenn Sie zum direkten Aufruf des Textanalysediensts bereit sind:

+ [Registrieren Sie sich](how-tos/text-analytics-how-to-signup.md), um einen Zugriffsschlüssel zu erhalten, und befolgen Sie die Schritte zum [Aufrufen der API](how-tos/text-analytics-how-to-call-api.md).

+ Der [Schnellstart](quickstarts/csharp.md) stellt eine exemplarische Vorgehensweise für REST-API-Aufrufe in C# dar. Erfahren Sie, wie Sie mit minimalem Code Text übermitteln, eine Analyse auswählen und Ergebnisse anzeigen.

+ Die [API-Referenzdokumentation](//go.microsoft.com/fwlink/?LinkID=759346) beinhaltet die technische Dokumentation für die REST-APIs. Die Dokumentation unterstützt eingebettete Aufrufe, sodass Sie die API von jeder Seite der Dokumentation aus aufrufen können.

+ Der Artikel [Externe Inhalte und Community-Inhalte](text-analytics-resource-external-community.md) stellt eine Liste von Blogbeiträgen und Videos bereit, in denen die Verwendung der Textanalyse mit anderen Tools und Technologien veranschaulicht wird.

## <a name="see-also"></a>Weitere Informationen

 [Dokumentationsseite zu Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

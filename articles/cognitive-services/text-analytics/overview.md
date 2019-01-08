---
title: Was ist Textanalyse?
titleSuffix: Azure Cognitive Services
description: Textanalyse in Azure Cognitive Services für Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung und Entitätsverknüpfung
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: overview
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: 0fe4a9f05e0f6d1abed7b906cc5cd89854885ae5
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "53992854"
---
# <a name="what-is-text-analytics"></a>Was ist Textanalyse?

Die Textanalyse-API ist ein cloudbasierter Dienst für die erweiterte Verarbeitung natürlicher Sprache aus unformatiertem Text. Sie bietet vier Hauptfunktionen: Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung und Entitätsverknüpfung.

Die API basiert auf Ressourcen in [Microsoft Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), einer Sammlung von Algorithmen für maschinelles Lernen und KI in der Cloud, und kann in Ihren Entwicklungsprojekten verwendet werden.

## <a name="capabilities-in-text-analytics"></a>Funktionen der Textanalyse

Der Begriff „Textanalyse“ hat verschiedene Bedeutungen, in Cognitive Services stellt die Textanalyse-API jedoch vier Analysearten bereit, die in der folgenden Tabelle beschrieben werden.

| Vorgänge| BESCHREIBUNG | APIs |
|-----------|-------------|------|
|[**Standpunktanalyse**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Ermitteln Sie, was die Kunden von Ihrer Marke oder von Ihrem Thema halten, indem Sie unformatierten Text auf Hinweise für positive oder negative Standpunkte analysieren. Die API gibt für jedes Dokument eine Bewertung des Standpunkts zwischen 0 und 1 zurück. Hierbei entspricht 1 einer positiven Bewertung.<br /> Die Analysemodelle sind mithilfe großer Textmengen und Microsoft-Technologien für natürliche Sprache vortrainiert. Für [ausgewählte Sprachen](text-analytics-supported-languages.md) kann die API jeden bereitgestellten unformatierten Text analysieren und bewerten und der aufrufenden Anwendung direkt Ergebnisse zurückgeben. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Schlüsselbegriffserkennung**](how-tos/text-analytics-how-to-keyword-extraction.md) | Extrahieren Sie automatisch Schlüsselwörter, um die wichtigsten Inhalte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt die API die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück.  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Sprachenerkennung**](how-tos/text-analytics-how-to-language-detection.md) | Dieses Feature erkennt die Sprache von Eingabetexten für bis zu 120 Sprachen und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Entitätserkennung (Vorschauversion)**](how-tos/text-analytics-how-to-entity-linking.md) | Identifizieren und kategorisieren Sie Entitäten in Ihrem Text als Personen, Orte, Organisationen, Datums-/Zeitangaben, Mengen, Prozentsätze, Währungen usw. Bekannte Entitäten werden ebenfalls erkannt und mit weiteren Informationen im Internet verknüpft. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) |

## <a name="use-containers"></a>Verwenden von Containern

[Verwenden Sie die Container für die Textanalyse](how-tos/text-analytics-how-to-install-containers.md), um lokal Schlüsselbegriffe zu extrahieren, die Sprache zu ermitteln and die Stimmung zu analysieren. Installieren Sie dazu standardisierte Dock-Container, die sich näher bei Ihren Daten befinden.

## <a name="typical-workflow"></a>Typischer Workflow

Der Workflow ist einfach: Sie übermitteln Daten für die Analyse und verarbeiten Ausgaben in Ihrem Code. Analysetool werden unverändert (d.h. ohne Konfigurationen oder Anpassungen) verwendet.

1. [Registrieren Sie sich](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) für einen [Zugriffsschlüssel](how-tos/text-analytics-how-to-access-key.md). Der Schlüssel muss bei jeder Anforderung übergeben werden.

2. [Formulieren Sie eine Anforderung](how-tos/text-analytics-how-to-call-api.md#json-schema), die Ihre Daten als unformatierten, unstrukturierten Text im JSON-Format enthält.

3. Übermitteln Sie die Anforderung an den Endpunkt, der während der Registrierung eingerichtet wurde, und fügen Sie die gewünschten Ressourcen an: Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung und Entitätsidentifikation.

4. Streamen oder speichern Sie die Antwort lokal. Je nach Anforderung ist das Ergebnis entweder eine Standpunktbewertung, eine Sammlung von extrahierten Schlüsselbegriffen oder ein Sprachcode.

Die Ausgabe wird als einzelnes JSON-Dokument mit Ergebnissen für jedes bereitgestellte Textdokument zurückgegeben und basiert auf der ID. Sie können die Ergebnisse anschließend analysieren, visualisieren oder in verwertbare Erkenntnisse kategorisieren.

Daten werden nicht in Ihrem Konto gespeichert. Von der Textanalyse-API durchgeführte Vorgänge sind zustandslos. Dies bedeutet, dass der bereitgestellte Text verarbeitet wird und die Ergebnisse sofort zurückgegeben werden.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Unterstützte Sprachen

Für eine bessere Auffindbarkeit wurde dieser Abschnitt in einen separaten Artikel verschoben. Diesen Inhalt finden Sie unter [Supported languages in the Text Analytics API (Unterstützte Sprachen in der Textanalyse-API)](text-analytics-supported-languages.md).

<a name="data-limits"></a>

## <a name="data-limits"></a>Datengrenzwerte

Alle Endpunkte der Textanalyse-API akzeptieren unformatierte Textdaten. Der aktuelle Grenzwert liegt bei 5.000 Zeichen für jedes Dokument. Wenn Sie größere Dokumente analysieren müssen, können Sie diese in kleinere Blöcke aufteilen. Wenn Sie dennoch einen höheren Grenzwert benötigen, [kontaktieren Sie uns](https://azure.microsoft.com/overview/sales-number/), damit Ihre Anforderungen besprochen werden können.

| Begrenzung | Wert |
|------------------------|---------------|
| Maximale Größe eines einzelnen Dokuments | 5.000 Zeichen (von `String.Length` gemessen) |
| Maximale Größe der gesamte Anforderung | 1 MB |
| Maximale Anzahl von Dokumenten in einer Anforderung | 1.000 Dokumente |

Das Ratenlimit beträgt 100 Aufrufe pro Minute. Beachten Sie, dass Sie viele Dokumente (bis zu 1.000) in einem einzigen Aufruf übermitteln können.

## <a name="unicode-encoding"></a>Unicode-Codierung

Die Textanalyse-API verwendet Unicode-Codierung für die Textdarstellung und die Berechnung der Zeichenanzahl. Anforderungen können in UTF-8 und UTF-16 übermittelt werden, dabei gibt es keine messbaren Unterschiede bei der Zeichenanzahl. Unicode-Codepunkte werden als Heuristik für die Zeichenlänge verwendet und werden für die Grenzwerte der Textanalysedaten als äquivalent betrachtet. Wenn Sie [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) verwenden, um die Zeichenanzahl abzurufen, verwenden Sie die gleiche Methode wie wir, um die Datengröße zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie zunächst die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Sie können eine Texteingabe (maximal 5.000 Zeichen) einfügen, um die Sprache (bis zu 120 verschiedene) zu erkennen, eine Standpunktbewertung zu berechnen und Schlüsselbegriffe zu extrahieren. Es ist keine Registrierung erforderlich.

Wenn Sie bereit sind, die API direkt aufzurufen:

+ [Registrieren Sie sich](how-tos/text-analytics-how-to-signup.md), um einen Zugriffsschlüssel zu erhalten, und befolgen Sie die Schritte zum [Aufrufen der API](how-tos/text-analytics-how-to-call-api.md).

+ Der [Schnellstart](quickstarts/csharp.md) stellt eine exemplarische Vorgehensweise für REST-API-Aufrufe in C# dar. Erfahren Sie, wie Sie mit minimalem Code Text übermitteln, eine Analyse auswählen und Ergebnisse anzeigen.

+ Die [API-Referenzdokumentation](//go.microsoft.com/fwlink/?LinkID=759346) ist die technische Dokumentation für die APIs. Die Dokumentation unterstützt eingebettete Aufrufe, sodass Sie die API von jeder Seite der Dokumentation aus aufrufen können.

+ Der Artikel [Externe Inhalte und Community-Inhalte](text-analytics-resource-external-community.md) stellt eine Liste von Blogbeiträgen und Videos bereit, in denen die Verwendung der Textanalyse mit anderen Tools und Technologien veranschaulicht wird.

## <a name="see-also"></a>Weitere Informationen

 [Dokumentationsseite zu Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)

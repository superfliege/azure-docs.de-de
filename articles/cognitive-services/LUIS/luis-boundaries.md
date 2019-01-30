---
title: Einschränkungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Dieser Artikel behandelt die bekannten Einschränkungen von Azure Cognitive Services Language Understanding (LUIS). Für LUIS gelten verschiedene Einschränkungsbereiche. Die Modelleinschränkung steuert Absichten, Entitäten und Features in LUIS. Kontingentgrenzen basierend auf dem Schlüsseltyp. Tastenkombinationen steuern die LUIS-Website.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 7f8f4848b7181ad3df7ad4fa009ff284de381b75
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54820409"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Grenzen für das LUIS-Modell und die Schlüssel
Für LUIS gelten verschiedene Einschränkungsbereiche. Der erste umfasst die [Modelleinschränkung](#model-boundaries), mit der Absichten, Entitäten und Features in LUIS gesteuert werden. Der zweite Bereich sind [Kontingentlimits](#key-limits), die auf dem Schlüsseltyp basieren. Ein dritter Bereich umfasst die [Tastenkombinationen](#keyboard-controls) zum Steuern der LUIS-Website. Ein vierter Bereich bezieht sich auf die [Zuordnung der Regionen weltweit](luis-reference-regions.md) zwischen der LUIS-Erstellungswebsite und den LUIS-[Endpunkt](luis-glossary.md#endpoint)-APIs. 


## <a name="model-boundaries"></a>Modelleinschränkungen


|Bereich|Begrenzung|
|--|:--|--|
| [App-Name][luis-get-started-create-app] | * Standardzeichenlimit |
| [Batchtests][batch-testing]| 10 Datasets, 1.000 Äußerungen pro Dataset|
| Explizite Listen | 50 pro Anwendung|
| [Absichten][intents]|500 pro Anwendung<br>[Dispatchbasierte](https://aka.ms/dispatch-tool) Anwendungen verfügen entsprechend über 500 Dispatchquellen|
| [List-Entitäten](./luis-concept-entity-types.md) | Übergeordnet: 50, untergeordnet: 20.000 Elemente. Kanonischer Name mit * Standardzeichenlimit. Für Synonymwerte gelten keine Längenbeschränkungen. |
| [Durch maschinelles Lernen erworbene Entitäten](./luis-concept-entity-types.md):<br> Composite,<br>  Hierarchical<br> Einfach|100 <br>Die Gesamtanzahl durch maschinelles Lernen erworbener Simple-, Hierarchical- und Composite-Entitäten darf 100 nicht übersteigen. Composite- und Hierarchical-Entitäten können über nicht mehr als 10 untergeordnete Elemente verfügen.  |
| [Muster](luis-concept-patterns.md)|500 Muster pro Anwendung.<br>Muster dürfen maximal 400 Zeichen lang sein.<br>3 Pattern.any-Entitäten pro Muster<br>Maximal 2 geschachtelte optionale Texte im Muster|
| [Pattern.any-Entitäten](./luis-concept-entity-types.md)|100 pro Anwendung, 3 Pattern.any-Entitäten pro Muster |
| [Ausdrucksliste][phrase-list]|10 Ausdruckslisten; 5.000 Elemente pro Liste|
| [Vordefinierte Entitäten](./luis-prebuilt-entities.md) | Keine Begrenzung|
| [RegEx-Entitäten](./luis-concept-entity-types.md)|20 Entitäten<br>max. 500 Zeichen pro RegEx-Entitätsmuster|
| [Rollen](luis-concept-roles.md)|300 Rollen pro Anwendung. 10 Rollen pro Entität|
| [Äußerung][utterances] | 500 Zeichen|
| [Äußerungen][utterances] | 15.000 pro Anwendung|
| [Versionen](luis-concept-version.md)| Keine Begrenzung |
| [Versionsname][luis-how-to-manage-versions] | 10 Zeichen, ausschließlich alphanumerische Zeichen und Punkt (.) |

* Das Standardzeichenlimit beträgt 50 Zeichen. 

## <a name="intent-and-entity-naming"></a>Namen von Absichten und Entitäten
Verwenden Sie die folgenden Zeichen nicht in Namen von Absichten und Entitäten:

|Zeichen|NAME|
|--|--|
|`{`|Linke geschweifte Klammer|
|`}`|Rechte geschweifte Klammer|
|`[`|Linke eckige Klammer|
|`]`|Rechte eckige Klammer|
|`\`|Umgekehrter Schrägstrich|

## <a name="key-usage"></a>Schlüsselverwendung

Language Understanding verfügt über separate Schlüssel, einen Typ für die Erstellung und einen Typ für die Abfrage des Vorhersageendpunkts. Weitere Informationen zu den Unterschieden zwischen Schlüsseltypen finden Sie unter [Erstellungsschlüssel und Endpunktschlüssel für Vorhersageabfragen in LUIS](luis-concept-keys.md).

## <a name="key-limits"></a>Schlüsselgrenzwerte

Beim Erstellungsschlüssel gelten verschiedene Limits für die Verwendung beim Erstellen und auf Endpunkten. Der Endpunktschlüssel für den LUIS-Dienst gilt ausschließlich für Endpunktabfragen.


|Schlüssel|Erstellen|Endpunkt|Zweck|
|--|--|--|--|
|Language Understanding Erstellung/Starter|1 Mio./Monat, 5/Sekunde|1.000/Monat, 5/Sekunde|Erstellen Ihrer LUIS-App|
|Language Understanding [Abonnement][pricing] – F0 – Free-Tarif |ungültig|10.000/Monat, 5/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|Language Understanding [Abonnement][pricing] – F0 – Basic-Tarif|ungültig|50/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|Cognitive Services [Abonnement][pricing] – S0 – Standard-Tarif|ungültig|50/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|[Integration der Standpunktanalyse](luis-how-to-publish-app.md#enable-sentiment-analysis)|ungültig|kostenlos|Hinzufügen von Informationen zur Stimmung, einschließlich Datenextraktion für Schlüsselausdrücke |
|Speech-Integration|ungültig|5,50 USD/1.000 Endpunktanforderungen|Konvertieren gesprochener Äußerungen in Textäußerungen und Zurückgeben der LUIS-Ergebnisse|

## <a name="keyboard-controls"></a>Tastenkombinationen

|Tastatureingabe | BESCHREIBUNG | 
|--|--|
|STRG+E|Wechselt in der Liste der Äußerungen zwischen Token und Entitäten|

## <a name="website-sign-in-time-period"></a>Anmeldezeitraum für die Website

Ihr Anmeldezugriff gilt für **60 Minuten**. Nach diesem Zeitraum erhalten Sie diesen Fehler. Sie müssen sich erneut anmelden.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/

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
ms.date: 12/07/2018
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 716c6b67676bb3421fd5dbd0274ed41c7705c676
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133518"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Grenzen für das LUIS-Modell und die Schlüssel
Für LUIS gelten verschiedene Einschränkungsbereiche. Der erste umfasst die [Modelleinschränkung](#model-boundaries), mit der Absichten, Entitäten und Features in LUIS gesteuert werden. Der zweite Bereich sind [Kontingentlimits](#key-limits), die auf dem Schlüsseltyp basieren. Ein dritter Bereich umfasst die [Tastenkombinationen](#keyboard-controls) zum Steuern der LUIS-Website. Ein vierter Bereich bezieht sich auf die [Zuordnung der Regionen weltweit](luis-reference-regions.md) zwischen der LUIS-Erstellungswebsite und den LUIS-[Endpunkt](luis-glossary.md#endpoint)-APIs. 


## <a name="model-boundaries"></a>Modelleinschränkungen

|Bereich|Begrenzung|
|--|:--|--|
| [App-Name][luis-get-started-create-app] | * Standardzeichenlimit |
| [Batchtests][batch-testing]| 10 Datasets, 1.000 Äußerungen pro Dataset|
| **[Composite-Entitäten](./luis-concept-entity-types.md)|100 mit bis zu 10 untergeordneten Entitäten |
| Explizite Listen | 50 pro Anwendung|
| **[Hierarchical-Entitäten](./luis-concept-entity-types.md) |100 mit bis zu 10 untergeordneten Entitäten |
| [Absichten][intents]|500 pro Anwendung<br>[Dispatchbasierte](https://aka.ms/dispatch-tool) Anwendungen verfügen entsprechend über 500 Dispatchquellen|
| [List-Entitäten](./luis-concept-entity-types.md) | Übergeordnet: 50, untergeordnet: 20.000 Elemente. Kanonischer Name mit * Standardzeichenlimit. Für Synonymwerte gelten keine Längenbeschränkungen. |
| [Muster](luis-concept-patterns.md)|500 Muster pro Anwendung.<br>Muster dürfen maximal 400 Zeichen lang sein.<br>3 Pattern.any-Entitäten pro Muster<br>Maximal 2 geschachtelte optionale Texte im Muster|
| [Pattern.any-Entitäten](./luis-concept-entity-types.md)|100 pro Anwendung, 3 Pattern.any-Entitäten pro Muster |
| [Ausdrucksliste][phrase-list]|10 Ausdruckslisten; 5.000 Elemente pro Liste|
| [Vordefinierte Entitäten](./luis-prebuilt-entities.md) | Keine Begrenzung|
| [RegEx-Entitäten](./luis-concept-entity-types.md)|20 Entitäten<br>max. 500 Zeichen pro RegEx-Entitätsmuster|
| [Rollen](luis-concept-roles.md)|300 Rollen pro Anwendung. 10 Rollen pro Entität|
| **[Simple-Entitäten](./luis-concept-entity-types.md)| 100 Entitäten|
| [Äußerung][utterances] | 500 Zeichen|
| [Äußerungen][utterances] | 15.000 pro Anwendung|
| [Versionen](luis-concept-version.md)| Keine Begrenzung |
| [Versionsname][luis-how-to-manage-versions] | 10 Zeichen, ausschließlich alphanumerische Zeichen und Punkt (.) |

* Das Standardzeichenlimit beträgt 50 Zeichen. 

** Die Gesamtanzahl von Simple-, Hierarchical- und Composite-Entitäten darf 100 nicht übersteigen. Die Gesamtzahl von Hierarchical-, Composite- und Simple-Entitäten sowie untergeordneten Hierarchical-Entitäten darf 330 nicht übersteigen. 

## <a name="intent-and-entity-naming"></a>Namen von Absichten und Entitäten
Verwenden Sie die folgenden Zeichen nicht in Namen von Absichten und Entitäten:

|Zeichen|NAME|
|--|--|
|`{`|Linke geschweifte Klammer|
|`}`|Rechte geschweifte Klammer|
|`[`|Linke eckige Klammer|
|`]`|Rechte eckige Klammer|
|`\`|Umgekehrter Schrägstrich|

## <a name="key-limits"></a>Schlüsselgrenzwerte
Beim Erstellungsschlüssel gelten verschiedene Limits für die Verwendung beim Erstellen und auf Endpunkten. Der Endpunktschlüssel für den LUIS-Dienst gilt ausschließlich für Endpunktabfragen.

|Schlüssel|Erstellen|Endpunkt|Zweck|
|--|--|--|--|
|Erstellungs-/Startschlüssel|1 Mio./Monat, 5/Sekunde|1.000/Monat, 5/Sekunde|Erstellen Ihrer LUIS-App|
|[Abonnement][pricing] – F0 – Free-Tarif |ungültig|10.000/Monat, 5/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|[Abonnement][pricing] – S0 – Basic-Tarif|ungültig|50/Sekunde|Abfragen Ihres LUIS-Endpunkts|
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

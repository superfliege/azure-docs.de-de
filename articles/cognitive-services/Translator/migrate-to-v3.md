---
title: Text-API von Microsoft Translator – Migrieren zu V3 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie von V2 zu V3 der Text-API von Microsoft Translator migrieren.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: translator
ms.topic: article
ms.date: 03/27/2018
ms.author: v-jansko
ms.openlocfilehash: 16fec351af5b5b3875657ee244c18f305311d965
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377539"
---
# <a name="microsoft-translator-text-api-v2-to-v3-migration"></a>Migration der Text-API von Microsoft Translator von V2 zu V3

Das Microsoft Translator-Team hat Version 3 (V3) der Textübersetzungs-API veröffentlicht. Dieses Release enthält neue Features, veraltete Methoden und ein neues Format für das Senden und Empfangen von Daten an bzw. vom Microsoft Translator-Dienst. Dieses Dokument stellt Informationen für das Ändern von Anwendungen für die Verwendung von V3 bereit. V2 gilt ab dem 30. April 2018 als veraltet und wird ab dem 30. April 2019 nicht mehr unterstützt.

Am Ende dieses Dokuments finden Sie nützliche Links, unter denen Sie weitere Informationen finden.

## <a name="summary-of-features"></a>Zusammenfassung der Features

* Keine Ablaufverfolgung: In V3 gilt das Feature „Keine Ablaufverfolgung“ für alle Tarife im Azure-Portal. Das bedeutet, dass kein Text, der an die V3-API übermittelt wird, von Microsoft gespeichert wird.
* JSON: XML wird durch JSON ersetzt. Alle Daten, die an diesen Dienst gesendet und von ihm empfangen werden, sind im JSON-Format.
* Mehrere Zielsprachen in einer einzelnen Anforderung: Die Übersetzungsmethode akzeptiert mehrere Zielsprachen für die Übersetzung einer einzelnen Anforderung. Eine einzelne Anforderung kann also beispielsweise von Englisch in Deutsch, Spanisch und Japanisch (oder in eine andere Gruppe von Sprachen) übersetzt werden.
* Bilinguales Wörterbuch: Eine Methode für ein bilinguales Wörterbuch wurde zur API hinzugefügt. Diese Methode enthält „lookup“ und „examples“.
* Transkription: Die Transliterate-Methode wurde zur API hinzugefügt. Diese Methode konvertiert Wörter und Sätze in einem Skript (z.B. Arabisch) in ein anderes Skript (z.B. Latein).
* Sprachen: Eine neue Languages-Methode stellt Sprachinformationen im JSON-Format bereit, die mit den Methoden „Translate“, „Dictionary“ und „Transliterate“ verwendet werden können.
* Erweiterung der Translate-Methode: Neue Funktionen wurden zur Translate-Methode hinzugefügt, um einige der Features zu unterstützen, die in der V2-API als separate Methoden enthalten waren. Ein Beispiel dafür ist TranslateArray.
* Speak-Methode: Die Text-zu-Sprache-Funktion wird in der Microsoft Translator-API nicht mehr unterstützt. Diese Funktion ist nun in der Bing-Spracheingabe-API von Microsoft Azure Cognitive Services verfügbar.

Die folgende Liste der V2- und V3-Methoden enthält die V3-Methoden und -APIs, die die Funktionen von V2 übernehmen.

| API-Methode in V2   | API-Kompatibilität in V3 |
|:----------- |:-------------|
| Translate     | Translate          |
| TranslateArray      | Translate          |
| GetLanguageNames      | Sprachen          |
| GetLanguagesForTranslate     | Sprachen        |
| GetLanguagesForSpeak      | Spracheingabe-APIs von Cognitive Services         |
| Speak     | Spracheingabe-APIs von Cognitive Services          |
| Erkennen     | Erkennen         |
| DetectArray     | Erkennen         |
| AddTranslation     | Microsoft Translator Hub-API         |
| AddTranslationArray    | Microsoft Translator Hub-API          |
| BreakSentences      | BreakSentence         |
| GetTranslations      | Das Feature wird nicht mehr unterstützt.         |
| GetTranslationsArray      | Das Feature wird nicht mehr unterstützt.         |

## <a name="move-to-json-format"></a>Wechsel zum JSON-Format

Die Textübersetzung von Microsoft Translator V2 hat Daten im XML-Format akzeptiert und zurückgegeben. In V3 sind alle mithilfe der API gesendeten und empfangenen Daten im JSON-Format. XML wird in V3 nicht mehr akzeptiert oder zurückgegeben. 

Diese Änderung wirkt sich auf verschiedene Aspekte von Anwendungen aus, die für die Textübersetzungs-API von V2 geschrieben wurden. Die Sprachen-API gibt beispielsweise Sprachinformationen für die Textübersetzung, Transkription und zwei Wörterbuchmethoden zurück. Sie können alle Sprachinformationen für alle Methoden in einem Aufruf oder einzeln anfordern.

Die Languages-Methode erfordert keine Authentifizierung. Klicken Sie auf den folgenden Link, um alle Sprachinformationen für V3 im JSON-Format anzuzeigen:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Authentifizierungsschlüssel

Der Authentifizierungsschlüssel, den Sie für V2 verwenden, wird in V3 akzeptiert. Sie müssen kein neues Abonnement erstellen. Während der Migrationsphase von einem Jahr können Sie V2 und V3 in Ihren Apps mischen. Dadurch wird das Veröffentlichen neuer Versionen vereinfacht, während Sie noch von V2 (XML) zu V3 (JSON) migrieren.

## <a name="pricing-model"></a>Preismodell

Microsoft Translator V3 wird genau wie V2 berechnet: pro Zeichen, einschließlich Leerzeichen. Durch die neuen Features in V3 werden einige Änderungen daran vorgenommen, welche Zeichen abgerechnet werden.

| V3-Methode   | Abgerechnete Zeichen |
|:----------- |:-------------|
| Sprachen     | Wenn keine Zeichen übermittelt werden, werden keine Zeichen gezählt und abgerechnet.          |
| Translate     | Der Betrag hängt davon ab, wie viele Zeichen für die Übersetzung übermittelt werden und in wie viele Sprachen diese übersetzt werden. Wenn 50 Zeichen in 5 Sprachen übersetzt werden, werden 50×5 Zeichen berechnet.           |
| Transliterate     | Die Anzahl der Zeichen, die für die Transkription übermittelt werden, wird berechnet.         |
| Dictionary/lookup und Dictionary/example     | Die Anzahl der Zeichen, die für die Wörterbuchsuche und Beispiele übermittelt werden, wird berechnet.         |
| BreakSentence     | Kostenlos.       |
| Erkennen     | Kostenlos.      |

## <a name="v3-end-points"></a>V3-Endpunkte

Global

* api.cognitive.microsofttranslator.com


## <a name="v3-api-text-translations-methods"></a>Textübersetzungsmethoden der V3-API

[Languages](reference/v3-0-languages.md)

[Translate](reference/v3-0-translate.md)

[Transliterate](reference/v3-0-transliterate.md)

[BreakSentence](reference/v3-0-break-sentence.md)

[Detect](reference/v3-0-detect.md)

[Dictionary/lookup](reference/v3-0-dictionary-lookup.md)

[Dictionary/example](reference/v3-0-dictionary-examples.md)

## <a name="customization"></a>Anpassung

Microsoft Translator V3 verwendet standardmäßig eine neuronale maschinelle Übersetzung. Deshalb kann V3 nicht mit Microsoft Translator Hub verwendet werden, denn dieser Hub unterstützt nur die veraltete statistische maschinelle Übersetzung. Die neuronale Übersetzung kann nun mithilfe des benutzerdefinierten Translators angepasst werden. [Weitere Informationen zum Anpassen von neuronaler maschineller Übersetzung](customization.md)

Die neuronale Übersetzung mit der Text-API von V3 unterstützt die Verwendung der Standardkategorien (smt, speech, text, generalnn) nicht.


## <a name="links"></a>Links

* [Microsoft-Datenschutzrichtlinie](https://privacy.microsoft.com/privacystatement)
* [Rechtliche Informationen zu Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Online Services Terms (Lizenzbedingungen für Onlinedienste)](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Dokumentation zu V3.0](reference/v3-0-reference.md)

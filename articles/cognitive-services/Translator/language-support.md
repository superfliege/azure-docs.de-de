---
title: Sprachunterstützung – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: Eine Liste der von der Textübersetzungs-API unterstützten natürlichen Sprachen.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 09/25/2018
ms.author: v-jansko
ms.openlocfilehash: 775e098eb2a067e3e0446bccc223c1c54e082347
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435308"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Sprach- und Regionsunterstützung für die Textübersetzungs-API

Die Textübersetzungs-API unterstützt die folgenden Sprachen für die Übersetzung von Texten. Die neuronale maschinelle Übersetzung (NMT) ist der neue Standard für qualitativ hochwertige, auf künstlicher Intelligenz basierende, maschinelle Übersetzungen und steht standardmäßig in V3 der Translator-Text-API bereit, wenn ein neuronales System verfügbar ist. Die neuronale maschinelle Übersetzung ist in V2 über die Kategorie „generalnn“ verfügbar.

[Weitere Informationen zur Funktionsweise von maschineller Übersetzung](https://www.microsoft.com/translator/mt.aspx)

| Sprache    | Übersetzungstyp |Sprachcode |
|:----------- |:-------:|:-------------:|
| Afrikaans      | Statistisch |`af`          |
| Arabisch      | Neuronal | `ar`          |
| Arabisch, Levantine    | Neuronal | `apc`
| Bengalisch      | Neuronal |`bn`          |
| Bosnisch (Lateinisch)      | Statistisch |`bs`          |
| Bulgarisch     |  Neuronal |`bg`          |
| Chinesisch (traditionell)      | Statistisch |`yue`          |
| Katalanisch      | Statistisch |`ca`          |
| Chinesisch (vereinfacht)        |  Neuronal |`zh-Hans`          |
| Chinesisch (traditionell)        |  Neuronal |`zh-Hant`          |
| Kroatisch      | Neuronal |`hr`          |
| Tschechisch        |  Neuronal |`cs`          |
| Dänisch        |  Neuronal |`da`          |
| Niederländisch        |  Neuronal |`nl`          |
| Englisch       |  Neuronal |`en`          |
| Estnisch      | Neuronal |`et`          |
| Fidschi      | Statistisch |`fj`          |
| Filipino      | Statistisch |`fil`          |
| Finnisch      | Neuronal |`fi`          |
| Französisch        |  Neuronal |`fr`          |
| Deutsch       |  Neuronal |`de`          |
| Griechisch      | Neuronal |`el`          |
| Haitianisches Kreolisch      | Statistisch |`ht`          |
| Hebräisch      | Neuronal |`he`          |
| Hindi        |  Neuronal |`hi`          |
| Hmong Daw      | Statistisch |`mww`          |
| Ungarisch      | Neuronal |`hu`          |
| Isländisch      |  Neuronal |`is`           |
| Indonesisch      | Statistisch |`id`          |
| Italienisch        |  Neuronal |`it`          |
| Japanisch        |  Neuronal |`ja`          |
| Suaheli      | Statistisch |`sw`          |
| Klingonisch      | Statistisch |`tlh`          |
| Klingonisch (pIqaD)      | Statistisch |`tlh-Qaak`          |
| Koreanisch        |  Neuronal |`ko`          |
| Lettisch      | Neuronal |`lv`          |
| Litauisch      | Neuronal |`lt`          |
| Madagassisch      | Statistisch |`mg`          |
| Malaiisch      | Statistisch |`ms`          |
| Maltesisch      | Statistisch |`mt`          |
| Norwegisch        |  Neuronal |`nb`          |
| Persisch      | Statistisch |`fa`          |
| Polnisch        |  Neuronal |`pl`          |
| Portugiesisch        |  Neuronal |`pt`          |
| Queretaro-Otomi      | Statistisch |`otq`          |
| Rumänisch        |  Neuronal |`ro`          |
| Russisch        |  Neuronal |`ru`          |
| Samoanisch      | Statistisch |`sm`          |
| Serbisch (Kyrillisch)      | Statistisch |`sr-Cyrl`          |
| Serbisch (Lateinisch)      | Statistisch |`sr-Latn`          |
| Slowakisch     | Neuronal |`sk`          |
| Slowenisch      | Neuronal |`sl`          |
| Spanisch        |  Neuronal |`es`          |
| Schwedisch        |  Neuronal |`sv`          |
| Tahitisch      | Statistisch |`ty`          |
| Tamilisch      | Statistisch |`ta`          |
| Telugu   | Neuronal   | `te` |
| Thailändisch      | Neuronal |`th`          |
| Tongaisch      | Statistisch |`to`          |
| Türkisch       |  Neuronal |`tr`          |
| Ukrainisch      | Neuronal |`uk`          |
| Urdu      | Statistisch |`ur`          |
| Vietnamesisch      | Neuronal |`vi`          |
| Walisisch      | Neuronal |`cy`          |
| Yukatekisches Maya      | Statistisch |`yua`          |

## <a name="transliteration"></a>Transliteration

Die „Transliterate“-Methode unterstützt die folgenden Sprachen. In der Spalte „Von/In“ wird durch „<-->“ angezeigt, dass für die Sprache eine Transliteration beider Skripts in das jeweils andere erfolgen kann. Mit „-->“ wird angezeigt, dass für die Sprache nur eine Transliteration von einem Skript in das andere möglich ist.

| Sprache    | Sprachcode | Skript | Von/In | Skript|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabisch | ar | Arabisch | <--> | Lateinisch |
|Bengalisch  | bn | Bangla | <--> | Lateinisch |
| Chinesisch (vereinfacht) | zh-Hans | Chinesisch (vereinfacht) | <--> | Lateinisch |
| Chinesisch (vereinfacht) | zh-Hans | Chinesisch (vereinfacht) | <--> | Chinesisch (traditionell) |
| Chinesisch (traditionell) | zh-Hant | Chinesisch (traditionell) | <--> | Lateinisch |
| Chinesisch (traditionell) | zh-Hant | Chinesisch (traditionell) | <--> | Chinesisch (vereinfacht) |
| Gujarati | gu  | Gujarati | --> | Lateinisch |
| Hebräisch | he | Hebräisch | <--> | Lateinisch |
| Hindi | hi | Devanagari | <--> | Lateinisch |
| Japanisch | ja | Japanisch | <--> | Lateinisch |
| Kannada | kn | Kannada | --> | Lateinisch |
| Malaiisch | ml | Malayalam | --> | Lateinisch |
| Marathi | mr | Devanagari | --> | Lateinisch |
| Oriya | oder | Oriya | <--> | Lateinisch |
| Pandschabi | pa | Gurmukhi | <--> | Lateinisch  |
| Serbisch (Kyrillisch) | sr-Cyrl | Kyrillisch  | --> | Lateinisch |
| Serbisch (Lateinisch) | sr-Latn | Lateinisch | --> | Kyrillisch |
| Tamilisch | ta | Tamilisch | --> | Lateinisch |
| Telugu | te | Telugu | --> | Lateinisch |
| Thailändisch | th | Thailändisch | <--> | Lateinisch |

## <a name="dictionary"></a>Wörterbuch

Das Wörterbuch unterstützt die Übertragung der folgenden Sprachen in das oder aus dem Englischen anhand der „Lookup“- und „Examples“-Methode.

| Sprache    | Sprachcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
| Katalanisch      | `ca`          |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Estnisch      | `et`          |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Haitianisches Kreolisch      | `ht`          |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungarisch      | `hu`          |
| Isländisch    | `is`  |
| Indonesisch      | `id`          |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
| Suaheli      | `sw`          |
| Klingonisch      | `tlh`          |
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Malaiisch      | `ms`          |
| Maltesisch      | `mt`          |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch      | `pt`          |
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Schwedisch      | `sv`          |
| Tamilisch      | `ta`          |
| Thailändisch      | `th`          |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Urdu      | `ur`          |
| Vietnamesisch      | `vi`          |
| Walisisch      | `cy`          |

## <a name="languages-detected-by-the-detect-method"></a>Mit der „Detect“-Methode erkannte Sprachen

Die folgenden Sprachen können von der „Detect“-Methode erkannt werden. Mithilfe von „Detect“ können Sprachen erkannt werden, die nicht von Microsoft Translator übersetzt werden können.

| Sprache    |
|:----------- |
| Afrikaans |
| Albanisch |
| Arabisch |
| Baskisch |
| Belarussisch |
| Bulgarisch |
| Katalanisch |
| Chinesisch |
| Chinesisch (vereinfacht) |
| Chinesisch (traditionell) |
| Kroatisch |
| Tschechisch |
| Dänisch |
| Niederländisch |
| Englisch |
| Esperanto |
| Estnisch |
| Finnisch |
| Französisch |
| Galizisch |
| Deutsch |
| Griechisch |
| Haitianisches Kreolisch |
| Hebräisch |
| Hindi |
| Ungarisch |
| Isländisch |
| Indonesisch |
| Irisch |
| Italienisch |
| Japanisch |
| Koreanisch |
| Kurdisch (arabisch) |
| Kurdisch (lateinisch) |
| Lateinisch |
| Lettisch |
| Litauisch |
| Mazedonisch |
| Malaiisch |
| Maltesisch |
| Norwegisch |
| Norwegisch (Nynorsk) |
| Paschtu |
| Persisch |
| Polnisch |
| Portugiesisch |
| Rumänisch |
| Russisch |
| Serbisch (Kyrillisch) |
| Serbisch (Lateinisch) |
| Slowakisch |
| Slowenisch |
| Somali |
| Spanisch |
| Suaheli |
| Schwedisch |
| Tagalog |
| Telugu |
| Thailändisch |
| Türkisch |
| Ukrainisch |
| Urdu |
| Usbekisch (kyrillisch) |
| Usbekisch (lateinisch) |
| Vietnamesisch |
| Walisisch |
| Jiddisch |

## <a name="access-the-list-programmatically"></a>Programmgesteuertes Zugreifen auf die Liste

Mithilfe des Vorgangs „Languages“ der Text-API in V3.0 können Sie programmgesteuert auf die Liste der unterstützten Sprachen zugreifen. Sie können die Liste nach Funktion, Sprachcode sowie Sprachname in Englisch oder jeder anderen unterstützten Sprache anzeigen. Sobald neue Sprachen verfügbar sind, wird diese Liste automatisch vom Microsoft Translator-Dienst aktualisiert.

[Anzeigen der Referenzdokumentation zum Vorgang „Languages“](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Zugreifen auf die Liste auf der Microsoft Translator-Website

Für einen schnellen Überblick über die Sprachen werden auf der Microsoft Translator-Website alle Sprachen angezeigt, die von der Translator-Text-API und -Sprach-API unterstützt werden. Diese Liste enthält keine entwicklerspezifischen Informationen wie z.B. Sprachcodes.

[Anzeige der Sprachenliste](https://www.microsoft.com/translator/languages.aspx)

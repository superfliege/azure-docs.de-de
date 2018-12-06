---
title: 'Sprachunterstützung: API für den Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Eine Liste der natürlichen Sprachen, die vom Spracherkennungsdienst unterstützt werden.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: erhopf
ms.openlocfilehash: fa4563b84eb0882832a89ccc98396ff487f51b48
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284760"
---
# <a name="language-and-region-support-for-speech-service-api"></a>Sprach- und Regionsunterstützung für die API für den Spracherkennungsdienst

Für verschiedene Funktionen des Spracherkennungsdiensts werden verschiedene Sprachen unterstützt. In den folgenden Tabellen wird die Sprachunterstützung zusammengefasst.

## <a name="speech-to-text"></a>Spracherkennung

Die Microsoft-Spracherkennungs-API unterstützt die folgenden Sprachen. Es sind verschiedene Anpassungsebenen für jede Sprache verfügbar.

  Code | Sprache | [Anpassung der Akustik](how-to-customize-acoustic-models.md) | [Anpassung der Sprache](how-to-customize-language-model.md) | [Anpassung der Aussprache](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-EG | Arabisch (Ägypten), modernes Hocharabisch | Nein  | Ja | Nein 
 ca-ES | Katalanisch (Spanien) | Nein  | Nein  | Nein 
 da-DK | Dänisch (Dänemark) | Nein  | Nein  | Nein 
 de-DE | Deutsch (Deutschland) | JA | JA | Nein 
 en-AU | Englisch (Australien) | Nein  | Ja | JA
 en-CA | Englisch (Kanada) | Nein  | Ja | JA
 en-GB | Englisch (Vereinigtes Königreich) | Nein  | Ja | JA
 en-IN | Englisch (Indien) | JA | Ja | JA
 en-NZ | Englisch (Neuseeland) | Nein  | Ja | JA  
 en-US | Englisch (USA) | JA | Ja | JA
 es-ES | Spanisch (Spanien) | Nein  | Ja | Nein 
 es-MX | Spanisch (Mexiko) | Nein  | Ja | Nein 
 fi-FI | Finnisch (Finnland) | Nein  | Nein  | Nein 
 fr-CA | Französisch (Kanada) | Nein  | Ja | Nein 
 fr-FR | Französisch (Frankreich) | JA | JA | Nein 
 hi-IN | Hindi (Indien) | Nein  | Ja | Nein 
 it-IT | Italienisch (Italien) | JA | JA | Nein 
 ja-JP | Japanisch (Japan) | Nein  | Ja | Nein 
 ko-KR | Koreanisch (Korea) | Nein  | Ja | Nein 
 nb-NO | Norwegisch, Bokmål (Norwegen) | Nein  | Nein  | Nein 
 nl-NL | Niederländisch (Niederlande) | Nein  | Ja | Nein 
 pl-PL | Polnisch (Polen) | Nein  | Nein  | Nein 
 pt-BR | Portugiesisch (Brasilien) | Nein  | Ja | Nein 
 pt-PT | Portugiesisch (Portugal) | Nein  | Ja | Nein 
 ru-RU | Russisch (Russland) | JA | JA | Nein 
 sv-SE | Schwedisch (Schweden) | Nein  | Nein  | Nein 
 zh-CN | Chinesisch (Mandarin, vereinfacht) | JA | JA | Nein 
 zh-HK | Chinesisch (Mandarin, Traditionell) | Nein  | Ja | Nein 
 zh-TW | Chinesisch (Taiwanesisch, Mandarin) | Nein  | Ja | Nein 
 th-TH | Thailändisch (Thailand) | Nein  | Nein  | Nein 


## <a name="text-to-speech"></a>Text-zu-Sprache

Die Sprachsynthese-API bietet die folgenden Stimmen. Jede steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert.

Gebietsschema | Sprache | Geschlecht | Dienstnamenzuordnung
-------|----------|---------|--------------------
ar-EG\* | Arabisch (Ägypten) | Female | Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)
ar-SA | Arabisch (Saudi-Arabien) | Male | Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)
bg-BG | Bulgarisch | Male | Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)
ca-ES | Katalanisch (Spanien) | Female | Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)
cs-CZ | Tschechisch | Male | Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)
da-DK | Dänisch | Female | Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)
de-AT | Deutsch (Österreich) | Male | Microsoft Server Speech Text to Speech Voice (de-AT, Michael)
de-CH | Deutsch (Schweiz) | Male | Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)
de-DE | Deutsch (Deutschland) | Female | Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)
| | | Female | Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)
| | | Male | Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)
el-GR | Griechisch | Male | Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)
en-AU | Englisch (Australien) | Female | Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)
| | | Female | Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)
en-CA | Englisch (Kanada) | Female | Microsoft Server Speech Text to Speech Voice (en-CA, Linda)
| | | Female | Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)
en-GB | English (UK) | Female | Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)
en-IE | Englisch (Irland) |Male | Microsoft Server Speech Text to Speech Voice (en-IE, Sean)
en-IN | Englisch (Indien) | Female | Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)
en-US | Englisch (USA) |Female | Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)
| | |Female | Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)
| | |Female | Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)
es-ES | Spanisch (Spanien) |Female | Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)
es-MX | Spanisch (Mexiko) | Female | Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)
| | | Male | Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)
fi-FI | Finnisch | Female | Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)
fr-CA | Französisch (Kanada) |Female | Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)
| | | Female | Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)
fr-CH | Französisch (Schweiz)|Male | Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)
fr-FR | Französisch (Frankreich)|Female | Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)
he-IL| Hebräisch (Israel) | Male| Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)
hi-IN | Hindi (Indien) | Female | Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)
| | | Male | Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)
hr-HR | Kroatisch | Male | Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)
hu-HU | Ungarisch | Male | Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)
id-ID | Indonesisch| Male | Microsoft Server Speech Text to Speech Voice (id-ID, Andika)
it-IT | Italienisch |Male | Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)
ja-JP | Japanisch |Female | Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)
| | |Male | Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)
ko-KR | Koreanisch |Female | Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)
ms-MY | Malaiisch | Male | Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)
nb-NO | Norwegisch | Female | Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)
nl-NL | Niederländisch | Female | Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)
pl-PL | Polnisch | Female | Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)
pt-BR | Portugiesisch (Brasilien) | Female | Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)
pt-PT | Portugiesisch (Portugal) | Female | Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)
ro-RO | Rumänisch | Male | Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)
ru-RU |Russisch| Female | Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)
| | |Male | Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)
sk-SK | Slowakisch|Male | Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)
sl-SI | Slowenisch|Male | Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)
sv-SE | Schwedisch|Female | Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)
ta-IN | Tamil (Indien) |Male | Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)
te-IN | Telugu (Indien) |Female | Microsoft Server Speech Text to Speech Voice (tr-IN, Chitra)
th-TH | Thailändisch|Male | Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)
tr-TR |Türkisch| Female | Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)
vi-VN | Vietnamesisch|Male | Microsoft Server Speech Text to Speech Voice (vi-VN, An)
zh-CN | Chinesisch (Festland)|Female | Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)
| | |Female | Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)
| | |Male | Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)
zh-HK | Chinesisch (Hongkong)|Female | Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)
| || Male | Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)
zh-TW | Chinesisch (Taiwan)|Female | Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)
| || Female | Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)
| || Male | Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)

\* *ar-EG unterstützt modernes Hocharabisch (Modern Standard Arabic, MSA).*

### <a name="customization"></a>Anpassung

Die Stimmanpassung ist für US-amerikanisches Englisch (en-US), vereinfachtes Chinesisch (zh-CN), Französisch (fr-FR), Deutsch (de-DE) und Italienisch (it-IT) verfügbar.

> [!NOTE]
> Für das französische, deutsche und italienische Stimmtraining steht ein Dataset mit über 2.000 Äußerungen zur Verfügung. Außerdem werden zweisprachige Chinesisch-Englisch-Modelle mit einem Anfangsdataset von über 2.000 Äußerungen unterstützt.

## <a name="speech-translation"></a>Sprachübersetzung

Die **Sprachübersetzungs**-API unterstützt verschiedene Sprachen für die Übersetzung von Sprache in Sprache und Sprache in Text. Die Ausgangssprache muss immer eine der Sprachen aus der folgenden Sprachtabelle sein. Welche Zielsprachen verfügbar sind, richtet sich danach, ob es sich beim Übersetzungsziel um Sprache oder Text handelt. Sie können Spracheingaben in mehr als [60 Sprachen](https://www.microsoft.com/translator/business/languages/) übersetzen. Eine Teilmenge dieser Sprachen steht für die [Sprachsynthese](language-support.md#text-languages) zur Verfügung. 

### <a name="speech-languages"></a>Sprachen für Sprachausgabe

| Sprache für Sprachausgabe   | Sprachcode |
|:----------- |-|
| Arabisch (modernes Hocharabisch)      | `ar` |
| Chinesisch (Mandarin)      | `zh` |
| Englisch      | `en` |
| Französisch      | `fr` |
| Deutsch      | `de` |
| Italienisch      | `it` |
| Japanisch      | `jp` |
| Portugiesisch (Brasilien)     | `pt` |
| Russisch      | `ru` |
| Spanisch      |  `es` |

### <a name="text-languages"></a>Sprachen für Textausgabe

| Sprache für Textausgabe    | Sprachcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
| Chinesisch (traditionell)      | `yue`          |
| Katalanisch      | `ca`          |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
| Chinesisch (traditionell)      | `zh-Hant`          |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Englisch      | `en`          |
| Estnisch      | `et`          |
| Fidschi      | `fj`          |
| Filipino      | `fil`          |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Haitianisches Kreolisch      | `ht`          |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungarisch      | `hu`          |
| Indonesisch      | `id`          |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
| Suaheli      | `sw`          |
| Klingonisch      | `tlh`          |
| Klingonisch (plqaD)      | `tlh-Qaak`          |
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Madagassisch      | `mg`          |
| Malaiisch      | `ms`          |
| Maltesisch      | `mt`          |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch      | `pt`          |
| Queretaro-Otomi      | `otq`          |
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Samoanisch      | `sm`          |
| Serbisch (Kyrillisch)      | `sr-Cyrl`          |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Schwedisch      | `sv`          |
| Tahitisch      | `ty`          |
| Tamilisch      | `ta`          |
| Thailändisch      | `th`          |
| Tongaisch      | `to`          |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Urdu      | `ur`          |
| Vietnamesisch      | `vi`          |
| Walisisch      | `cy`          |
| Yukatekisches Maya      | `yua`          |


## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)

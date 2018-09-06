---
title: Unterstützte Sprachen für die Speech-Dienste – Microsoft Cognitive Services
description: Hier finden Sie eine Liste der Sprachen, die von den Speech-Diensten unterstützt werden.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 7e2634db33d760207e9190b9955bd1f2ead179be
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123153"
---
# <a name="supported-languages-for-speech-service"></a>Unterstützte Sprachen für die Speech-Dienste

Für verschiedene Funktionen der Speech-Dienste werden verschiedene Sprachen unterstützt. In den folgenden Tabellen finden Sie eine Übersicht der Sprachunterstützung.

## <a name="speech-to-text"></a>Spracherkennung

Die Microsoft-Spracherkennungs-API unterstützt die folgenden Sprachen. 

  Code | Sprache | Anpassung der Akustik | Anpassung der Sprache | Anpassung der Aussprache
 ------|----------|---------------------|---------------------|-------------------------
 ar-EG | Arabisch (Ägypten), modernes Hocharabisch | Nein  | Ja | Nein 
 It-IT | Italienisch (Italien) | Nein  | Ja | Nein  
 de-DE | Deutsch (Deutschland) | Nein  | Ja | JA
 ja-JP | Japanisch (Japan) | Nein  | Ja | Nein 
 en-US | Englisch (USA) | JA | Ja | JA
 pt-BR | Portugiesisch (Brasilien) | Nein  | Ja | Nein 
 es-ES | Spanisch (Spanien) | Nein  | Ja | Nein 
 ru-RU | Russisch (Russland) | Nein  | Ja | Nein 
 fr-FR | Französisch (Frankreich) | Nein  | Ja | Nein 
 zh-CN | Chinesisch (Mandarin, vereinfacht) | Nein  | Ja | Nein 

## <a name="text-to-speech"></a>Text-zu-Sprache

Die **Text-to-Speech-API** bietet die folgenden Stimmen. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert.

Gebietsschema | Sprache | Geschlecht | Dienstnamenzuordnung
-------|----------|---------|--------------------
ar-EG* | Arabisch (Ägypten) | Female | Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)
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
ms-MY | Malaiisch|Male | Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)
nb-NO | Norwegisch|Female | Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)
nl-NL | Niederländisch|Female | Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)
pl-PL | Polnisch|Female | Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)
pt-BR | Portugiesisch (Brasilien)|Female | Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)
| | |Male | Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)
pt-PT | Portugiesisch (Portugal)|Female | Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)
ro-RO | Rumänisch|Male | Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)
ru-RU |Russisch| Female | Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)
| | |Male | Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)
| | |Female | Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)
sk-SK | Slowakisch|Male | Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)
sl-SI | Slowenisch|Male | Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)
sv-SE | Schwedisch|Female | Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)
ta-IN | Tamil (Indien) |Male | Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)
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

Benutzerdefinierte Voicefonts für die **Text-to-Speech**-API unterstützen Englisch (USA) (en-US) und Chinesisch (zh-CN).

## <a name="speech-translation"></a>Sprachübersetzung

Die **Sprachübersetzungs**-API unterstützt verschiedene Sprachen für die Übersetzung von Sprache in Sprache und Sprache in Text. Die Ausgangssprache muss immer eine der Sprachen aus der unten stehenden Tabelle sein. Welche Zielsprachen verfügbar sind, richtet sich danach, ob es sich beim Übersetzungsziel um Sprache oder Text handelt.

Sie können die Liste der Sprachen entweder programmgesteuert oder über den Sprachenendpunkt der REST-API abrufen. Die Liste enthält alle Sprachcodes sowie den Sprachnamen in Englisch bzw. einer anderen unterstützten Sprache. Sobald eine neue Sprache verfügbar ist, wird diese Liste vom Microsoft Translator-Dienst aktualisiert.

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

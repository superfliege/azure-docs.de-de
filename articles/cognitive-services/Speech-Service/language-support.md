---
title: Sprachunterstützung – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Azure Speech Services unterstützen neben der Sprachübersetzung zahlreiche Sprachen für die Konvertierung von Sprache zu Text und Text zu Sprache. Dieser Artikel enthält eine umfassende Liste zur Sprachunterstützung der einzelnen Dienste.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 0a82c2ba8bdf3d01041aa06f55eaaecab29817b2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58803102"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Sprach- und Regionsunterstützung für den Spracherkennungsdienst

Für verschiedene Funktionen von Speech Services werden verschiedene Sprachen unterstützt. In den folgenden Tabellen wird die Sprachunterstützung zusammengefasst.

## <a name="speech-to-text"></a>Spracherkennung

Die Microsoft-Spracherkennungs-API unterstützt die folgenden Sprachen. Es sind verschiedene Anpassungsebenen für jede Sprache verfügbar.

  Code | Sprache | [Anpassung der Akustik](how-to-customize-acoustic-models.md) | [Anpassung der Sprache](how-to-customize-language-model.md) | [Anpassung der Aussprache](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-EG | Arabisch (Ägypten), modernes Hocharabisch | Nein  | Ja | Nein 
 ca-ES | Katalanisch (Spanien) | Nein  | Nein  | Nein 
 da-DK | Dänisch (Dänemark) | Nein  | Nein  | Nein 
 de-DE | Deutsch (Deutschland) | Ja | Ja | Nein 
 en-AU | Englisch (Australien) | Nein  | Ja | Ja
 en-CA | Englisch (Kanada) | Nein  | Ja | Ja
 en-GB | Englisch (Vereinigtes Königreich) | Nein  | Ja | Ja
 en-IN | Englisch (Indien) | Ja | Ja | Ja
 en-NZ | Englisch (Neuseeland) | Nein  | Ja | Ja  
 en-US | Englisch (USA) | Ja | Ja | Ja
 es-ES | Spanisch (Spanien) | Ja | Ja | Nein 
 es-MX | Spanisch (Mexiko) | Nein  | Ja | Nein 
 fi-FI | Finnisch (Finnland) | Nein  | Nein  | Nein 
 fr-CA | Französisch (Kanada) | Nein  | Ja | Nein 
 fr-FR | Französisch (Frankreich) | Ja | Ja | Nein 
 hi-IN | Hindi (Indien) | Nein  | Ja | Nein 
 it-IT | Italienisch (Italien) | Ja | Ja | Nein 
 ja-JP | Japanisch (Japan) | Nein  | Ja | Nein 
 ko-KR | Koreanisch (Korea) | Nein  | Ja | Nein 
 nb-NO | Norwegisch, Bokmål (Norwegen) | Nein  | Nein  | Nein 
 nl-NL | Niederländisch (Niederlande) | Nein  | Ja | Nein 
 pl-PL | Polnisch (Polen) | Nein  | Nein  | Nein 
 pt-BR | Portugiesisch (Brasilien) | Ja | Ja | Nein 
 pt-PT | Portugiesisch (Portugal) | Nein  | Ja | Nein 
 ru-RU | Russisch (Russland) | Ja | Ja | Nein 
 sv-SE | Schwedisch (Schweden) | Nein  | Nein  | Nein 
 zh-CN | Chinesisch (Mandarin, vereinfacht) | Ja | Ja | Nein 
 zh-HK | Chinesisch (Kantonesisch, traditionell) | Nein  | Ja | Nein 
 zh-TW | Chinesisch (Taiwanesisch, Mandarin) | Nein  | Ja | Nein 
 th-TH | Thailändisch (Thailand) | Nein  | Nein  | Nein 


## <a name="text-to-speech"></a>Text-zu-Sprache

Die Text-to-Speech-REST-API unterstützt diese Stimmen. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert.

> [!IMPORTANT]
> Die Preise variieren für Standardstimmen, benutzerdefinierte und neuronale Stimmen. Weitere Informationen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices-preview"></a>Neuronale Stimmen (Vorschau)

Die neuronale Sprachsynthese ist eine neue Art der Sprachsynthese mithilfe von Deep Neural Networks. Wenn Sie eine neuronale Stimme verwenden, kann die synthetisierte Sprache kaum von menschlichen Aufzeichnungen unterschieden werden.

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und virtuellen Helfern noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern.

Eine vollständige Liste der neuronalen Stimmen mit der regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#neural-voices).

| Gebietsschema | Sprache | Geschlecht | Dienstnamenzuordnung|
|--------|----------|--------|---------------------|
| de-DE | Deutsch (Deutschland) | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, KatjaNeural)" |
| en-US | Englisch (USA) | Male | „Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)“ |
| en-US | Englisch (USA) | Female | „Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)“ |
| it-IT | Italienisch (Italien) | Female | "Microsoft Server Speech Text to Speech Voice (it-IT, ElsaNeural)" |
| zh-CN | Chinesisch | Female | „Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)“ |

> [!IMPORTANT]
> Die Sprachsynthesestimme (zh-CN, XiaoxiaoNeural) für Microsoft Server steht nur über den Endpunkt „Asien, Südosten“ zur Verfügung: https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1.

> [!IMPORTANT]
> „Microsoft Server Speech Text to Speech Voice (de-DE, KatjaNeural)“ und „Microsoft Server Speech Text to Speech Voice (it-IT, ElsaNeural)“ sind nur über den Endpunkt „Europa, Westen“ verfügbar: https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1.

### <a name="standard-voices"></a>Standardstimmen

Es stehen mehr als 75 Standardstimmen in mehr als 45 Sprachen und Gebietsschemas zur Verfügung, mit denen Sie Text in Sprache umwandeln können. Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-voices).

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

Die **Sprachübersetzungs**-API unterstützt verschiedene Sprachen für die Übersetzung von Sprache in Sprache und Sprache in Text. Die Ausgangssprache muss immer eine der Sprachen aus der Sprachentabelle für die Spracherkennung sein. Welche Zielsprachen verfügbar sind, richtet sich danach, ob es sich beim Übersetzungsziel um Sprache oder Text handelt. Sie können Spracheingaben in mehr als [60 Sprachen](https://www.microsoft.com/translator/business/languages/) übersetzen. Eine Teilmenge dieser Sprachen steht für die [Sprachsynthese](language-support.md#text-languages) zur Verfügung.

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

* [Abrufen Ihres Speech Services-Testabonnements](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)

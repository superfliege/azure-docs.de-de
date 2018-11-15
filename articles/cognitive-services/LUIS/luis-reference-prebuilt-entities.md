---
title: Vordefinierte Entitäten – LUIS
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Listen mit vordefinierten Entitäten, die in LUIS (Language Understanding Intelligent Service) enthalten sind.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: bc23f2e5d8304400802c74093d4d78a1fcd8cf22
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219483"
---
# <a name="entities-per-culture"></a>Entitäten nach Kultur

LUIS stellt vordefinierte Entitäten bereit. Wenn eine vordefinierte Entität in Ihrer Anwendung enthalten ist, schließt LUIS die entsprechende Entitätsvorhersage in die Endpunktantwort ein. Alle Beispieläußerungen sind ebenfalls mit der Entität gekennzeichnet. Das Verhalten der vordefinierten Entitäten **kann nicht** geändert werden. Sofern nicht anders vermerkt, sind vordefinierte Entitäten in allen LUIS-Anwendungsgebietsschemas (Kulturen) verfügbar. Die folgende Tabelle zeigt die vordefinierten Entitäten, die für die jeweilige Kultur unterstützt werden.

Vordefinierte Entität   |   Englisch (USA)<br>```En-us```   |   Französisch (Frankreich)<br>```fr-FR```   |   Italienisch (Italien)<br>```it-IT```   |   Spanisch (Spanien)<br>```es-ES```   |   Chinesisch<br>```zh-CN```   |   Deutsch<br>```de-DE```   |   Portugiesisch (Brasilien)<br>```pt-BR```   |   Japanisch (Japan)<br>```ja-JP```   |   Koreanisch (Korea)<br>```ko-kr```   | Französisch (Kanada)<br>```fr-CA```   |   Spanisch (Mexiko)<br>```es-MX```   |   Niederländisch (Niederlande)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Currency](luis-reference-prebuilt-currency.md):<br>dollar<br>Untereinheit (z.B. „penny“)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[E-Mail](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Number](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   |    -   |    -   |    -   |    ✔   |    -   |    -   |    -   |   -   |   -   |   -   |   -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Beachten Sie auch die Hinweise in [Veraltete Entitäten](luis-reference-prebuilt-deprecated.md).

Die keyPhrase-Entität ist nicht für alle Unterkulturen von Portugiesisch (Brasilien) verfügbar (```pt-BR```).

## <a name="contribute-to-prebuilt-entity-cultures"></a>Beitragen zu vordefinierten Entitäten in verschiedenen Kulturen
Die vordefinierten Entitäten werden im Open Source-Projekt „Recognizers-Text“ entwickelt. Zum Projekt [beitragen](https://github.com/Microsoft/Recognizers-Text). Dieses Projekt enthält Beispiele zur Entität „currency“ (Währung) in den verschiedenen Kulturen. 

„GeographyV2“ und „PersonName“ sind im Projekt „Recognizers-Text“ nicht enthalten. Bei Problemen mit diesen vordefinierten Entitäten stellen Sie eine [Supportanfrage](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [number](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) und [currency](luis-reference-prebuilt-currency.md). 

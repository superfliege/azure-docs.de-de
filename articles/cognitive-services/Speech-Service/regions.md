---
title: Regionen des Speech-Diensts
titlesuffix: Azure Cognitive Services
description: Referenz zu den Regionen des Speech-Diensts.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8d36036332e939075ffac8763bec9c23d8e4a3f7
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712890"
---
# <a name="speech-service-supported-regions"></a>Vom Speech-Dienst unterstützte Regionen

Mithilfe des Speech-Diensts kann Ihre Anwendung Sprache in Text konvertieren, Sprachübersetzungen ausführen und Text in Sprache konvertieren. Der Dienst ist in mehreren Regionen mit eindeutigen Endpunkten für das Speech SDK und REST-APIs verfügbar.

Stellen Sie sicher, dass Sie den Endpunkt verwenden, der mit der Region für Ihr Abonnement übereinstimmt.

## <a name="speech-sdk"></a>Speech SDK

Im [Speech-Dienst-SDK](speech-sdk.md) werden Regionen als Zeichenfolge angegeben (z.B. als Parameter für `SpeechConfig.FromSubscription` im Speech-SDK für C#).

### <a name="speech-recognition-and-translation"></a>Spracherkennung und Übersetzung

Das Speech SDK ist in den folgenden Regionen für **Spracherkennung** und **Übersetzung** verfügbar:

  Region | Speech SDK-Parameter | Speech-Anpassungsportal
 ------|-------|--------
 USA (Westen) | `westus` | https://westus.cris.ai
 USA, Westen 2 | `westus2` | https://westus2.cris.ai
 USA (Ost) | `eastus` | https://eastus.cris.ai
 USA (Ost 2) | `eastus2` | https://eastus2.cris.ai
 Asien, Osten | `eastasia` | https://eastasia.cris.ai
 Südostasien | `southeastasia` | https://southeastasia.cris.ai
 Nordeuropa | `northeurope` | https://northeurope.cris.ai
 Europa, Westen | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Absichtserkennung

Die **Absichtserkennung** für das Speech SDK weist die gleiche Unterstützung für Regionen wie LUIS auf. Eine vollständige Liste der verfügbaren Regionen finden Sie unter [Veröffentlichen von Regionen und Endpunkten – LUIS](https://docs.microsoft.comazure/cognitive-services/luis/luis-reference-regions).

Verfügbare Regionen für die **Absichtserkennung** über das Speech SDK finden Sie auf der [Seite mit den Regionen des Language Understanding-Diensts](/azure/cognitive-services/luis/luis-reference-regions).

Verwenden Sie für jede aufgeführte Veröffentlichungsregion den bereitgestellten **API-Regionsnamen**. Verwenden Sie z.B. `westus` für USA (Westen).

## <a name="rest-apis"></a>REST-APIs

Der Speech-Dienst macht auch REST-Endpunkte für Spracherkennungs- und Sprachsyntheseanforderungen verfügbar.

### <a name="speech-to-text"></a>Spracherkennung

Die Referenzdokumentation zur Spracherkennung finden Sie unter [REST-APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Sprachsynthese

Die Referenzdokumentation zur Sprachsynthese finden Sie unter [REST-APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

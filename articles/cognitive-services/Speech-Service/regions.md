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
ms.openlocfilehash: a5fce6f9547a96da3ce482ce388e5ba2093f2af4
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468125"
---
# <a name="regions-of-the-speech-service"></a>Regionen des Speech-Diensts

Der Speech-Dienst ist in unterschiedlichen Regionen verfügbar.
Wenn Sie ein Abonnement erstellen, können Sie je nach Ihren Anforderungen eine verfügbare Region auswählen.

Wenn Sie Ihr Abonnement verwenden, müssen Sie die ausgewählte Region berücksichtigen.

## <a name="rest-api"></a>REST-API

Verwenden Sie die REST-API, um die richtigen regionsspezifischen Endpunkte auszuwählen.
Details finden Sie unter [REST-APIs](rest-apis.md).

## <a name="speech-sdk"></a>Sprach-SDK

Im [Speech-Dienst-SDK](speech-sdk.md) werden Regionen als Zeichenfolge angegeben (z.B. als Parameter für `SpeechConfig.FromSubscription` im Speech-SDK für C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regionen für die Spracherkennung und Übersetzung

Die folgende Tabelle enthält die verfügbaren Regionen für **Spracherkennung** und **Übersetzung**.

  Region | Speech SDK-Parameter | Portal
 ------|-------|--------
 USA (Westen) | `westus` | https://westus.cris.ai
 USA, Westen 2 | `westus2` | https://westus2.cris.ai 
 USA (Ost) | `eastus` | https://eastus.cris.ai
 USA (Ost 2) | `eastus2` | https://eastus2.cris.ai
 Asien, Osten | `eastasia` | https://eastasia.cris.ai
 Südostasien | `southeastasia` | https://southeastasia.cris.ai
 Nordeuropa | `northeurope` | https://northeurope.cris.ai
 Europa, Westen | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>Regionen für die Absichtserkennung

Verfügbare Regionen für die **Absichtserkennung** über das Speech SDK finden Sie auf der [Seite mit den Regionen des Language Understanding-Diensts](/azure/cognitive-services/luis/luis-reference-regions).
Für jede aufgeführte Veröffentlichungsregion wird der entsprechende Sprach-SDK-Regionsparameter als erster Teil des Domänennamens des Endpunkts festgelegt.
Verwenden Sie z.B. `westus` zur Angabe der Veröffentlichungsregion „USA, Westen“.

---
title: Regionen des Spracherkennungsdiensts | Microsoft-Dokumentation
description: Referenz für Regionen des Spracherkennungsdiensts.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 11360d163fdba057d373d091d46903cde7789a8b
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071418"
---
# <a name="regions-of-the-speech-service"></a>Regionen des Spracherkennungsdiensts

Der Spracherkennungsdienst ist in unterschiedlichen Regionen verfügbar.
Wenn Sie ein Abonnement erstellen, können Sie je nach Ihren Anforderungen eine verfügbare Region auswählen.

Wenn Sie Ihr Abonnement verwenden, müssen Sie die ausgewählte Region berücksichtigen.

## <a name="rest-api"></a>REST-API

Wählen Sie mithilfe der REST-API die richtigen regionsspezifischen Endpunkte aus.
Details finden Sie unter [REST-APIs](rest-apis.md).

## <a name="speech-sdk"></a>Sprach-SDK

Im [Sprach-SDK](speech-sdk.md) werden Regionen als Zeichenfolge angegeben (z.B. als Parameter für [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) im Sprach-SDK für C#).

Die folgende Tabelle listet die verfügbaren Regionen für **Spracherkennung** und **Übersetzung** auf:

Region| Wert für den Regionsparameter im Sprach-SDK
-|-
USA (Westen)| `westus`
Asien, Osten| `eastasia`
Nordeuropa| `northeurope`

Verfügbare Regionen für die **Absichtserkennung** über das Sprach-SDK finden Sie auf der [Seite „Regionen und Schlüssel“ des Language Understanding-Diensts](/azure/cognitive-services/luis/luis-reference-regions).
Für jede aufgeführte Veröffentlichungsregion wird der entsprechende Sprach-SDK-Regionsparameter als erster Teil des Domänennamens des Endpunkts festgelegt.
Verwenden Sie z.B. `westus` zur Angabe der Veröffentlichungsregion „USA, Westen“.

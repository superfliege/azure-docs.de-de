---
title: Regionen des Spracherkennungsdiensts
description: Referenz für Regionen des Spracherkennungsdiensts.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 082002b25b02e1e496221f4686d0e636630dd438
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324390"
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

### <a name="regions-for-speech-recognition-and-translation"></a>Regionen für die Spracherkennung und Übersetzung

Die folgende Tabelle listet die verfügbaren Regionen für **Spracherkennung** und **Übersetzung** auf:

Region| Wert für den Regionsparameter im Sprach-SDK
-|-
USA (Westen)| `westus`
Asien, Osten| `eastasia`
Nordeuropa| `northeurope`

### <a name="regions-for-intent-recognition"></a>Regionen für die Absichtserkennung

Verfügbare Regionen für die **Absichtserkennung** über das Sprach-SDK finden Sie auf der [Seite „Regionen und Schlüssel“ des Language Understanding-Diensts](/azure/cognitive-services/luis/luis-reference-regions).
Für jede aufgeführte Veröffentlichungsregion wird der entsprechende Sprach-SDK-Regionsparameter als erster Teil des Domänennamens des Endpunkts festgelegt.
Verwenden Sie z.B. `westus` zur Angabe der Veröffentlichungsregion „USA, Westen“.

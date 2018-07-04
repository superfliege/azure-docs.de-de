---
title: Regionen des Spracherkennungsdiensts | Microsoft-Dokumentation
description: Referenz für Regionen des Spracherkennungsdiensts.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/27/2018
ms.author: mahilleb
ms.openlocfilehash: a201cc043f673e2285ea48950804d97b96f881ed
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054907"
---
# <a name="regions-and-endpoints-of-the-speech-service"></a>Regionen und Endpunkte des Spracherkennungsdiensts

> [!NOTE]
> Regionsnamen im [Sprach-SDK](speech-sdk.md) entsprechen dem ersten Teil der Domäne der unten angegebenen Endpunkte.
> Verwenden Sie z.B. `westus` zur Angabe der Region „USA, Westen“ im Sprach-SDK.

## <a name="speech-to-text"></a>Spracherkennung

[!include[](includes/endpoints-speech-to-text.md)]

## <a name="text-to-speech"></a>Text-zu-Sprache

[!include[](includes/endpoints-text-to-speech.md)]

## <a name="authentication"></a>Authentifizierung

[!include[](includes/endpoints-token-service.md)]

Ausführliche Informationen zum Abrufen und Aktualisieren von Autorisierungstoken finden Sie [hier](rest-apis.md#authentication).

## <a name="language-understanding-speech-sdk-only"></a>Language Understanding (nur Sprach-SDK)

Regionen für den Dienst Language Understanding sind [hier](/azure/cognitive-services/luis/luis-reference-regions) aufgelistet.
Geben Sie im Sprach-SDK diese Regionen durch den ersten Teil des Domänennamens des Endpunkts an (z.B. `westus`).

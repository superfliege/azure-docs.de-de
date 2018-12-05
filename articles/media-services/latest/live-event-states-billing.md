---
title: 'LiveEvent: Zustandswerte und Abrechnung in Azure Media Services | Microsoft-Dokumentation'
description: Dieses Thema bietet eine Übersicht über LiveEvent-Zustandswerte und die entsprechende Abrechnung in Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: 1a49f62d7b5e21fe9d6483f71b729a9100aff1a3
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585399"
---
# <a name="liveevent-states-and-billing"></a>LiveEvent-Zustandswerte und Abrechnung

In Azure Media Services fallen für LiveEvent Gebühren an, sobald der Zustand **Wird ausgeführt** ausgewählt wird. Damit für LiveEvent keine Gebühren mehr anfallen, müssen Sie es beenden.

Wenn **LiveEventEncodingType** für Ihr [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) auf „Standard“ (Basic) festgelegt ist, schaltet Media Services jedes LiveEvent, das sich noch im Zustand **Wird ausgeführt** befindet, 12 Stunden nachdem der Eingangsfeed verloren gegangen ist, automatisch ab. Es werden keine **LiveOutputs** mehr ausgeführt. Allerdings wird Ihnen die Zeit in Rechnung gestellt, in der LiveEvent den Zustand **Wird ausgeführt** hatte.

## <a name="states"></a>Zustände

LiveEvent kann folgende Zustandswerte aufweisen:

|Zustand|BESCHREIBUNG|
|---|---|
|**Beendet**| Dies ist der anfängliche LiveEvent-Zustand nach der Erstellung (es sei denn, der automatische Start wurde auf TRUE festgelegt). In diesem Status werden keine Gebühren berechnet. In diesem Zustand können die LiveEvent-Eigenschaften aktualisiert werden, aber Streaming ist nicht zulässig.|
|**Wird gestartet**| LiveEvent wird gestartet, und Ressourcen werden zugewiesen. In diesem Status werden keine Gebühren berechnet. In diesem Zustand sind weder Updates noch Streaming zulässig. Wenn ein Fehler auftritt, wird das LiveEvent in den Zustand „Stopped“ (Angehalten) zurückgesetzt.|
|**Wird ausgeführt**| Die LiveEvent-Ressourcen wurden zugewiesen, Erfassungs- und Vorschau-URLs wurden generiert, und Livestreams können empfangen werden. Zu diesem Zeitpunkt ist die Abrechnung aktiv. Sie müssen für die LiveEvent-Ressource explizit „Beenden“ auswählen, damit keine Gebühren mehr anfallen.|
|**Wird beendet**| LiveEvent wird beendet und die Bereitstellung der Ressourcen aufgehoben. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Zustand sind weder Updates noch Streaming zulässig.|
|**Wird gelöscht**.| Das LiveEvent wird gelöscht. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Zustand sind weder Updates noch Streaming zulässig.|

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Livestreaming](live-streaming-overview.md)
- [Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)

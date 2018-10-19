---
title: Reaktion auf Azure Media Services-Ereignisse | Microsoft-Dokumentation
description: Verwenden Sie Azure Event Grid, um Media Services-Ereignisse zu abonnieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 3541a5b33aa0bb98d9381b51caefc63b6aa677ad
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377547"
---
# <a name="handling-event-grid-events"></a>Behandeln von Event Grid-Ereignissen

Media Services-Ereignisse ermöglichen es Anwendungen, mithilfe moderner serverloser Architekturen auf verschiedene Ereignisse zu reagieren (z.B. auf Statusänderungsereignisse für Aufträge). Dies geschieht ohne komplizierten Code oder teure und ineffiziente Abrufdienste. Stattdessen werden die Ereignisse per Push über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Ereignishandler wie [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) oder sogar Ihren eigenen Webhook übertragen, und Sie zahlen nur für das, was Sie tatsächlich nutzen. Informationen zu den Preisen finden Sie unter [Event Grid – Preise](https://azure.microsoft.com/pricing/details/event-grid/).

Die Verfügbarkeit von Media Services-Ereignissen ist an die [Verfügbarkeit](../../event-grid/overview.md) von Event Grid gebunden und wird in anderen Regionen verfügbar, wenn dort Event Grid verfügbar ist.  

## <a name="available-media-services-events"></a>Verfügbare Media Services-Ereignisse

Event Grid verwendet [Ereignisabonnements](../../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten.  Zurzeit können Media Services-Ereignisabonnements folgende Ereignisse umfassen:  

|Veranstaltungsname|BESCHREIBUNG|
|----------|-----------|
| Microsoft.Media.JobStateChange| Wird ausgelöst, wenn sich der Status eines Auftrags ändert. |
| Microsoft.Media.LiveEventConnectionRejected | Verbindungsversuch des Encoders wurde verweigert. |
| Microsoft.Media.LiveEventEncoderConnected | Encoder stellt die Verbindung mit dem Liveereignis her. |
| Microsoft.Media.LiveEventEncoderDisconnected | Verbindung des Encoders wird getrennt. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Media-Server verwirft Datenblöcke, da es zu spät ist oder es einen überlappenden Zeitstempel gibt (Zeitstempel des neuen Datenblocks liegt vor der Endzeit des vorherigen Datenblocks). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Media-Server empfängt den ersten Datenblock für jede Spur im Stream oder über die Verbindung. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Media-Server hat erkannt, dass Audio- und Videostreams nicht synchron sind. Verwenden Sie dies als Warnung, da die Darstellung für den Benutzer möglicherweise nicht beeinträchtigt ist. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Media-Server hat erkannt, dass die beiden Videostreams von einem externen Encoder nicht mehr synchron sind. Verwenden Sie dies als Warnung, da die Darstellung für den Benutzer möglicherweise nicht beeinträchtigt ist. |
| Microsoft.Media.LiveEventIngestHeartbeat | Wird alle 20 Sekunden für jede Spur veröffentlicht, wenn ein Liveereignis ausgeführt wird. Bietet eine Zusammenfassung der Erfassungsintegrität. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Media-Server hat eine Diskontinuität in der eingehenden Spur erkannt. |

## <a name="event-schema"></a>Ereignisschema

Media Services-Ereignisse enthalten alle Informationen, die Sie zur Reaktion auf Änderungen in Ihren Daten benötigen.  Sie können ein Media Services-Ereignis daran erkennen, dass die eventType-Eigenschaft mit „Microsoft.Media“ beginnt.

Weitere Informationen finden Sie unter [Media Services-Ereignisschemas](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Methoden zum Nutzen von Ereignissen

Anwendungen, die Media Services-Ereignisse behandeln, sollten einige empfohlene Methoden verwenden:

* Da mehrere Abonnements zum Weiterleiten von Ereignissen an den gleichen Ereignishandler konfiguriert werden können, ist es wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen, sondern das Thema der Nachricht zu überprüfen, um sicherzustellen, dass es aus dem Speicherkonto stammt, das Sie erwarten.
* Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
* Ignorieren Sie Felder, die Sie nicht verstehen.  So müssen Sie sich nicht mit neuen Features auseinandersetzen, die in der Zukunft hinzugefügt werden könnten.
* Verwenden Sie Präfix- und Suffixübereinstimmungen für „subject“, um Ereignisse auf ein bestimmtes Ereignis zu beschränken.

## <a name="next-steps"></a>Nächste Schritte

[Abrufen von Auftragsstatusereignissen](job-state-events-cli-how-to.md)

---
title: Reaktion auf Azure Media Services-Ereignisse | Microsoft-Dokumentation
description: Verwenden Sie Azure Event Grid, um Media Services-Ereignisse zu abonnieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 969957d53824bd70440e5529b83bc830bb5d9cc4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="reacting-to-media-services-events"></a>Reagieren auf Media Services-Ereignisse

Media Services-Ereignisse ermöglichen es Anwendungen, mithilfe moderner serverloser Architekturen auf verschiedene Ereignisse zu reagieren (z.B. auf Statusänderungsereignisse für Aufträge). Dies geschieht ohne komplizierten Code oder teure und ineffiziente Abrufdienste. Stattdessen werden die Ereignisse per Push über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Ereignishandler wie [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) oder sogar Ihren eigenen Webhook übertragen, und Sie zahlen nur für das, was Sie tatsächlich nutzen. Informationen zu den Preisen finden Sie unter [Event Grid – Preise](https://azure.microsoft.com/pricing/details/event-grid/).

Die Verfügbarkeit von Media Services-Ereignissen ist an die [Verfügbarkeit](../../event-grid/overview.md) von Event Grid gebunden und wird in anderen Regionen verfügbar, wenn dort Event Grid verfügbar ist.  

## <a name="available-media-services-events"></a>Verfügbare Media Services-Ereignisse

Event Grid verwendet [Ereignisabonnements](../../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten.  Zurzeit können Media Services-Ereignisabonnements folgende Ereignistypen umfassen:  

|Veranstaltungsname|BESCHREIBUNG|
|----------|-----------|
| Microsoft.Media.JobStateChange| Wird ausgelöst, wenn sich der Status eines Auftrags ändert. |

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

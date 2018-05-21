---
title: 'Azure Application Insights-Telemetriedatenmodell: Ausnahmentelemetrie | Microsoft-Dokumentation'
description: Application Insights-Datenmodell für Ausnahmentelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin; sergkanz
ms.openlocfilehash: 036670c46a0ff40dd7b20a03c90f10513395cd71
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="exception-telemetry-application-insights-data-model"></a>Ausnahmentelemetrie: Application Insights-Datenmodell

In [Application Insights](app-insights-overview.md) stellt eine Instanz einer Ausnahme eine behandelte oder nicht behandelte Ausnahme dar, die während der Ausführung der überwachten Anwendung aufgetreten ist.

## <a name="problem-id"></a>Problem-ID

Bezeichner der Stelle, an der die Ausnahme im Code ausgelöst wurde. Dient zum Gruppieren von Ausnahmen. In der Regel eine Kombination von Ausnahmetyp und einer Funktion in der Aufrufliste.

Maximale Länge: 1024 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Ausnahmedetails

(Erweiterung folgt)

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](application-insights-data-model.md).
- Erfahren Sie mehr zum [Diagnostizieren von Ausnahmen in Ihren Web-Apps mit Application Insights](app-insights-asp-net-exceptions.md).
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

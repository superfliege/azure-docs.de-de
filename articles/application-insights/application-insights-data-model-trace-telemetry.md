---
title: 'Azure Application Insights-Telemetriedatenmodell: Ablaufverfolgungstelemetrie | Microsoft-Dokumentation'
description: Application Insights-Datenmodell für Ablaufverfolgungstelemetrie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 2487df93baec80c0e4f7741aa7b052ac89459b3b
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "35636217"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Ablaufverfolgungstelemetrie: Application Insights-Datenmodell

Ablaufverfolgungstelemetrie stellt (in [Application Insights](app-insights-overview.md)) Ablaufverfolgungsanweisungen im Format `printf` dar, die sich für eine Textsuche eignen. `Log4Net`, `NLog` und andere textbasierte Protokolldateieinträge werden in Instanzen dieses Typs übersetzt. Die Ablaufverfolgung weist für die Erweiterbarkeit keine Messungen auf.

## <a name="message"></a>Message

Ablaufverfolgungsmeldung.

Maximale Länge: 32.768 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen von .NET-Ablaufverfolgungsprotokollen in Application Insights](app-insights-asp-net-trace-logs.md).
- [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](app-insights-java-trace-logs.md).
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](application-insights-data-model.md).
- [Schreiben benutzerdefinierter Telemetriedaten für die Ablaufverfolgung](app-insights-api-custom-events-metrics.md#tracktrace)
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](app-insights-platforms.md).

---
title: 'Azure Application Insights: Unterstützte Features von Azure Functions | Microsoft-Dokumentation'
description: In Application Insights unterstützte Features von Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 0f4eaaefb7d2080218e19574621a4962e61057c3
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394305"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Unterstützte Features in Application Insights für Azure Functions

Azure Functions bietet [von Haus aus Integration](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) in Application Insights, die über die ILogger-Schnittstelle verfügbar ist. Unten folgt die Liste der aktuell unterstützten Features. Arbeiten Sie zum [Einstieg](https://github.com/Azure/Azure-Functions/wiki/App-Insights) das Azure Functions-Handbuch durch.

## <a name="supported-features"></a>Unterstützte Features

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights .NET SDK**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Automatische Sammlung von**        |                 |                   |               
| &bull;Anforderungen                     | JA             | JA               | 
| &bull;Ausnahmen                   | JA             | JA               | 
| &bull;Abhängigkeiten                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | JA               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | JA               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | JA               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | JA               | 
| | | | 
| **Unterstützte Features**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | JA             | JA               | 
| &bull; Stichprobenentnahme                     | JA             | JA               | 
| &bull; Heartbeats                   |                 | JA               | 
| | | | 
| **Korrelation**                       |                   |                   |               
| &bull; ServiceBus                     |                   | JA               | 
| &bull; EventHub                       |                   | JA               | 
| | | | 
| **Konfigurierbar**                      |                   |                   |           
| &bull;Vollständig konfigurierbar.<br/>Anweisungen finden Sie unter [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852).<br/>Alle Optionen finden Sie unter [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration).               |                   | JA                   | 


## <a name="sampling"></a>Stichproben

Azure Functions aktiviert die Stichprobenentnahme in der Konfiguration standardmäßig. Weitere Informationen finden Sie unter [Configure Sampling](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling) (Konfigurieren der Stichprobenentnahme).

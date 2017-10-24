---
title: Analysieren von Azure CDN-Verwendungsmustern | Microsoft Docs
description: "Kunden können die Protokollanalyse für Azure CDN aktivieren."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: af396e9f8847421d529c32956216cfc47294edb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analysieren von Azure CDN-Verwendungsmustern

Nachdem Sie CDN für Ihre Anwendung aktiviert haben, können Sie die CDN-Nutzung überwachen, die Integrität Ihrer Bereitstellung überprüfen und potenzielle Probleme behandeln. Es gibt zwei Möglichkeiten für die Bereitstellung dieser Funktionen über Azure CDN: 

## <a name="verizon-core-reports"></a>Verizon-Kernberichte

Als Benutzer von Azure CDN mit einem Standard- oder Premiumprofil bei Verizon können Sie im Verizon-Portal Verizon-Kernberichte anzeigen. Das Verizon-Portal bietet eine Vielzahl von Diagrammen und Ansichten. Im Azure-Portal kann über die Option **Verwalten** darauf zugegriffen werden. Weitere Informationen finden Sie unter [Kernberichte aus Verizon](cdn-analyze-usage-patterns.md).

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Kernanalyse über Azure-Diagnoseprotokolle

Die Kernanalyse ist für alle CDN-Endpunkte verfügbar, die zu CDN-Profilen von Verizon (Standard und Premium) und Akamai (Standard) gehören. Durch die Azure-Diagnoseprotokolle kann die Kernanalyse in den Azure-Speicher, Event Hubs oder OMS Log Analytics (OMS = Operations Management Suite) exportiert werden. OMS Log Analytics bietet eine Lösung mit Diagrammen, die von Benutzern konfiguriert und angepasst werden können. Weitere Informationen finden Sie unter [Azure-Diagnoseprotokolle](cdn-azure-diagnostic-logs.md).


---
title: Analysieren von Azure CDN-Verwendungsmustern | Microsoft Docs
description: In diesem Artikel werden die verschiedenen Arten von Analyseberichten beschrieben, die für Azure CDN-Produkte zur Verfügung stehen.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: rli; v-deasim
ms.openlocfilehash: 61fbe6e29df787048a9694138d3c9095f5cba76b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analysieren von Azure CDN-Verwendungsmustern

Nachdem Sie CDN für Ihre Anwendung aktiviert haben, können Sie die CDN-Nutzung überwachen, die Integrität Ihrer Bereitstellung überprüfen und potenzielle Probleme behandeln. Es gibt die folgenden Möglichkeiten für die Bereitstellung dieser Funktionen über Azure CDN: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Kernanalyse über Azure-Diagnoseprotokolle

Die Kernanalyse ist für CDN-Endpunkte für alle Tarife verfügbar. Durch die Azure-Diagnoseprotokolle kann die Kernanalyse in Azure Storage, Event Hubs oder Azure Log Analytics exportiert werden. Azure Log Analytics bietet eine Lösung mit Diagrammen, die von Benutzern konfiguriert und angepasst werden können. Weitere Informationen zu Azure-Diagnoseprotokollen finden Sie unter [Azure-Diagnoseprotokolle](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon-Kernberichte

Als Benutzer von Azure CDN mit dem Profil **Azure CDN Standard von Verizon** oder **Azure CDN Premium von Verizon** können Sie im zusätzlichen Verizon-Portal Verizon-Kernberichte anzeigen. Auf Verizon-Kernberichte kann im Azure-Portal über die Option **Verwalten** zugegriffen werden. Sie bieten eine Vielzahl von Diagrammen und Ansichten. Weitere Informationen finden Sie unter [Kernberichte aus Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Benutzerdefinierte Verizon-Berichte

Als Benutzer von Azure CDN mit dem Profil **Azure CDN Standard von Verizon** oder **Azure CDN Premium von Verizon** können Sie im zusätzlichen Verizon-Portal benutzerdefinierte Verizon-Berichte anzeigen. Auf benutzerdefinierte Verizon-Berichte kann im Azure-Portal über die Option **Verwalten** zugegriffen werden. Auf der Seite für benutzerdefinierte Verizon-Berichte wird die Anzahl der Treffer oder übertragenen Daten für jeden „Edge CName“ angezeigt, der zu einem Azure CDN-Profil gehört. Die Daten können über einen beliebigen Zeitraum nach HTTP-Antwortcode oder Cachestatus gruppiert werden. Weitere Informationen finden Sie unter [Benutzerdefinierte Berichte aus Verizon](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Azure CDN Premium von Verizon: Berichte

Mit **Azure CDN Premium von Verizon** können Sie darüber hinaus auf die folgenden Berichte zugreifen:
   * [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md)
   * [Echtzeitstatistiken](cdn-real-time-stats.md)
   * [Edgeknotenleistung](cdn-edge-performance.md)


---
title: Vergleichen von Produktfeatures des Azure Content Delivery Network (CDN) | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Featureunterstützung der Azure CDN-Produkte (Content Delivery Network).
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/09/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 3368a8a14a3d1314e4c7ecae9256071f1fe646f9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="compare-azure-cdn-product-features"></a>Vergleichen von Azure CDN-Produktfeatures

Azure CDN (Content Delivery Network) beinhaltet vier Produkte: **Azure CDN Standard von Microsoft** (Vorschauversion), **Azure CDN Standard von Akamai**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon**. 

In der folgenden Tabelle werden die Features der einzelnen Produkte gegenübergestellt.

| **Leistungsfeatures und -optimierungen** | **Microsoft Standard (Vorschauversion)** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** |
| --- | --- | --- | --- | --- |
| [Beschleunigung dynamischer Websites](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  |  | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Beschleunigung dynamischer Websites – Adaptive Bildkomprimierung](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Beschleunigung dynamischer Websites – Objektvorabruf](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only)  |  | **&#x2713;**  |  |  |
| [Videostreamingoptimierung](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | \* | **&#x2713;**  | \* |  \* |
| [Optimierung großer Dateien](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | \* | **&#x2713;**  | \* |  \* |
| [Globaler Serverlastenausgleich (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Schnelles Löschen](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Vorabladen von Assets](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| Cache-/Headereinstellungen (mit [Zwischenspeicherungsregeln](cdn-caching-rules.md))  |  |**&#x2713;** |**&#x2713;** | |
| Cache-/Headereinstellungen (mit [Regel-Engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| [Zwischenspeicherung von Abfragezeichenfolgen](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Regionales Zwischenspeichern  |**&#x2713;** |  |  |  |
| Dualer IPv4-/IPv6-Stapel | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2-Unterstützung](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Sicherheit** | **Microsoft Standard (Vorschauversion)** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| Unterstützung von HTTPS mit CDN-Endpunkt | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTPS für benutzerdefinierte Domänen](cdn-custom-ssl.md)  | **&#x2713;** | |**&#x2713;** |**&#x2713;** |
| [Unterstützung benutzerdefinierter Domänennamen](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geofilterung](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenauthentifizierung](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [DDOS-Schutz](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Bereitstellen eines eigenen Zertifikats](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) |**&#x2713;** |  |  |  |
||||
| **Analysen und Berichte** | **Microsoft Standard (Vorschauversion)** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| [Azure-Diagnoseprotokolle](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Kernberichte aus Verizon](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Benutzerdefinierte Berichte aus Verizon](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [Echtzeitstatistiken](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [Edgeknotenleistung](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [Warnungen in Echtzeit](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **Benutzerfreundlichkeit** | **Microsoft Standard (Vorschauversion)** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| Einfache Integration mit Azure-Diensten wie [Storage](cdn-create-a-storage-account-with-cdn.md), [Web-Apps](cdn-add-to-web-app.md) und [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Verwaltung per [REST-API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) oder [PowerShell](cdn-manage-powershell.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Anpassbare, regelbasierte Engine zur Inhaltsübermittlung](cdn-rules-engine.md)  |  | | |**&#x2713;** |
| URL-Umleitung/-Umschreibung (mit [Regel-Engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |
| Regeln für mobile Geräte (mit [Regel-Engine](cdn-rules-engine.md))  |  | | |**&#x2713;** |

\* Microsoft und Verizon unterstützen die direkte Übermittlung umfangreicher Dateien und Medien über die Optimierung der allgemeinen Webbereitstellung.




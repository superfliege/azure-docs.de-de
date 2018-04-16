---
title: Produktfeatures des Azure Content Delivery Network (CDN) | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Featureunterstützung der Azure CDN-Produkte (Content Delivery Network).
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/23/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: 40638e2180b63c90fbcbe15cc2c1e944a97e166e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cdn-product-features"></a>Azure CDN-Produktfeatures

Es stehen drei Azure CDN-Produkte (Content Delivery Network) zur Verfügung: **Azure CDN Standard von Akamai**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon**. In der folgenden Tabelle werden die Features der einzelnen Produkte gegenübergestellt.

| **Leistungsfeatures und -optimierungen** | Standard Akamai | Standard Verizon | Premium Verizon |
| --- | --- | --- | --- |
| [Beschleunigung dynamischer Websites](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Beschleunigung dynamischer Websites – Adaptive Bildkomprimierung](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Beschleunigung dynamischer Websites – Objektvorabruf](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Videostreamingoptimierung](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Optimierung großer Dateien](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Globaler Serverlastenausgleich (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Schnelles Löschen](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Vorabladen von Assets](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| Cache-/Headereinstellungen (mit [Zwischenspeicherungsregeln](cdn-caching-rules.md)) |**&#x2713;** |**&#x2713;** | |
| Cache-/Headereinstellungen (mit [Regel-Engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| [Zwischenspeicherung von Abfragezeichenfolgen](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Dualer IPv4-/IPv6-Stapel |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2-Unterstützung](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Sicherheit** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| Unterstützung von HTTPS mit CDN-Endpunkt |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTPS für benutzerdefinierte Domänen](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Unterstützung benutzerdefinierter Domänennamen](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geofilterung](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenauthentifizierung](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS-Schutz](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
| **Analysen und Berichte** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| [Azure-Diagnoseprotokolle](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Kernberichte aus Verizon](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Benutzerdefinierte Berichte aus Verizon](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Echtzeitstatistiken](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Edgeknotenleistung](cdn-edge-performance.md) | | |**&#x2713;** |
| [Warnungen in Echtzeit](cdn-real-time-alerts.md) | | |**&#x2713;** |
||||
| **Benutzerfreundlichkeit** | **Akamai Standard** | **Verizon Standard** | **Verizon Premium** | 
| Einfache Integration in Azure-Dienste wie [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web-Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) und [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Verwaltung per [REST-API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) oder [PowerShell](cdn-manage-powershell.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Anpassbare, regelbasierte Engine zur Inhaltsübermittlung](cdn-rules-engine.md) | | |**&#x2713;** |
| URL-Umleitung/-Umschreibung (mit [Regel-Engine](cdn-rules-engine.md)) | | |**&#x2713;** |
| Regeln für mobile Geräte (mit [Regel-Engine](cdn-rules-engine.md)) | | |**&#x2713;** |

\* Verizon unterstützt die direkte Übermittlung umfangreicher Dateien und Medien über die Optimierung der allgemeinen Webbereitstellung.




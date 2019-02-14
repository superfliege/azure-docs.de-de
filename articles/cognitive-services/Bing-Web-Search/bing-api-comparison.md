---
title: Was sind die Bing-Suche-APIs?
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie etwas über die Bing-Suche-APIs und wie Sie kognitive Suchvorgänge im Internet in Ihren Apps und Diensten aktivieren können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 60de69c71df61cb53e446aa8759842f72c2acf5d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864178"
---
# <a name="what-are-the-bing-search-apis"></a>Was sind die Bing-Suche-APIs?

Die Bing-Suche-APIs ermöglichen Ihnen die Erstellung von verbundenen Web-Apps und Diensten, die Webseiten, Bilder, Nachrichten, Orte u.v.m. ohne Werbung finden. Durch das Senden von Suchanforderungen mit den Bing-Suche-REST-APIs oder -SDKs können Sie relevante Informationen und Inhalte für die Websuchen abrufen. In diesem Artikel erfahren Sie etwas über die verschiedenen Bing-Suche-APIs und wie Sie kognitive Suchvorgänge im Internet in Ihre Apps und Dienste integrieren können. Die Preise und Ratengrenzwerte der einzelnen APIs können variieren.

## <a name="the-bing-web-search-api"></a>Die Bing-Websuche-API

Die [Bing-Websuche-API](../Bing-Web-Search/index.yml) gibt Webseiten, Bilder, Videos, Nachrichten und mehr zurück. Die an diese API gesendeten Suchabfragen können gefiltert werden, um bestimmte Inhaltstypen ein- oder auszuschließen.

Ziehen Sie die Bing-Websuche-API in Anwendungen in Betracht, die möglicherweise nach allen Arten von relevanten Webinhalten suchen müssen. Wenn Ihre Anwendung für einen bestimmten Typ von Onlineinhalten vorgesehen ist, erwägen Sie eine der folgenden Such-APIs: 

## <a name="content-specific-bing-search-apis"></a>Inhaltsspezifische Bing-Such-APIs

Die folgenden Bing-Suche-APIs geben bestimmte Inhalte aus dem Internet zurück, z.B. Bilder, Nachrichten, lokale Unternehmen und Videos.

| Bing-API | BESCHREIBUNG |
| -- | -- | 
| [Entitätssuche](../Bing-Entities-Search/index.yml) | Die Bing-Entitätssuche-API gibt Suchergebnisse mit Entitäten zurück. Dies können Personen, Orte oder Dinge sein. Abhängig von der Abfrage gibt die API eine oder mehrere Entitäten zurück, die der Suchabfrage entsprechen. Dabei kann es sich um wichtige Einzelpersonen, lokale Unternehmen, Orientierungspunkte, Ziele und mehr handeln. |
| [Bildersuche](../Bing-Image-Search/index.yml) | Mit der Bing-Bildersuche-API können Sie nach qualitativ hochwertigen statischen und animierten Bildern suchen, ähnlich wie unter [Bing.com/images](https://www.Bing.com/images). Sie können die Suchvorgänge verfeinern, um Bilder nach Attributen wie Größe, Farbe, Lizenz und Aktualität auszuschließen. Außerdem können Sie nach derzeit beliebten Bildern suchen, Bilder hochladen, um Erkenntnisse zu diesen zu gewinnen, und Miniaturansichten als Vorschau anzeigen. |
| [News-Suche](../Bing-News-Search/index.yml) | Die Bing-News-Suche-API ermöglicht das Auffinden von Nachrichten, ähnlich der Suche unter [Bing.com/news](https://www.Bing.com/news). Die API gibt Nachrichtenartikel aus mehreren Quellen oder von bestimmten Domänen zurück. Sie können nach Kategorien suchen, um aktuell relevante Artikel, wichtige Berichte und Schlagzeilen abzurufen.
| [Videosuche](../Bing-Video-Search/index.yml) | Mit der Bing-Videosuche-API können Sie Videos überall im Web suchen. Finden Sie beliebte Videos, verwandte Inhalte und Miniaturansichten als Vorschau. |
| [Visuelle Suche](../Bing-visual-search/index.yml) | Laden Sie ein Bild hoch, oder verwenden Sie eine URL, um aufschlussreiche Informationen darüber zu erhalten. Dies umfasst z.B. visuell ähnliche Produkte, Bilder und verwandte Suchvorgänge. |
 [Suche nach lokalen Unternehmen](../bing-local-business-search/index.yml) | Mit der Bing-Such-API für ortsansässige Unternehmen können Ihre Anwendungen basierend auf Suchabfragen Kontakt- und Standortdaten von Unternehmen in der näheren Umgebung suchen. |

## <a name="the-bing-custom-search-api"></a>API für die benutzerdefinierte Bing-Suche

Das Erstellen einer Instanz der benutzerdefinierten Suche mit der API für die [benutzerdefinierte Bing-Suche](../Bing-Custom-Search/index.yml) ermöglicht Ihnen die Erstellung eine Suchfunktion, die ausschließlich die für Sie interessanten Inhalte und Themen umfasst. Nach dem Angeben der Domänen, Websites und spezifischen Webseiten für die Bing-Suche werden die Ergebnisse z.B. auf den jeweiligen Inhalt angepasst. Sie können die APIs für die benutzerdefinierte Bing-Vorschlagssuche, die Bildersuche und die Videosuche integrieren, um das Suchergebnis noch weiter anzupassen.  

## <a name="additional-bing-search-apis"></a>Weitere Bing-Suche-APIs

Mit den folgenden Bing-Suche-APIs können Sie Ihre Suchergebnisse noch weiter verbessern, indem Sie sie mit anderen Bing-Suche-APIs kombinieren.

| API | BESCHREIBUNG |
| -- | -- | 
| [Bing-Vorschlagssuche](../Bing-Autosuggest/index.yml) | Verbessern Sie Suchergebnisse Ihrer Anwendung mit der Bing-Vorschlagssuche-API, indem Sie empfohlene Suchbegriffe in Echtzeit zurückgegeben.  |
| [Bing-Statistik](bing-web-stats.md) | Die Bing-Statistik stellt Analysen für die Bing-Suche-APIs bereit, die Ihre Anwendung verwendet. Einige der verfügbaren Analysen enthalten das Aufrufvolumen, die häufigsten Abfragezeichenfolgen und die geografische Verteilung. |

## <a name="next-steps"></a>Nächste Schritte

* [Details zu den Preisen](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) der Bing-Suche-API
* In den [Verwendungs- und Anzeigeanforderungen für Bing](./use-display-requirements.md) erfahren Sie, wie Inhalte und Informationen verwendet werden dürfen, die über die Bing-Suche-APIs gefunden wurden.

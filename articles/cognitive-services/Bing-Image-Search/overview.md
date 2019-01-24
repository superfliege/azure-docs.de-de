---
title: Was ist die Bing-Bildersuche-API?
titleSuffix: Azure Cognitive Services
description: Die Bing-Bildersuche-API ermöglicht die Verwendung kognitiver Bildersuchfunktionen in Ihrer Anwendung. Sie können Suchabfragen von Benutzern über die API senden und ähnlich wie bei der Bing-Bildersuche relevante Bilder in hoher Qualität abrufen und anzeigen.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 71e8e363a02e08a099d9051c7f851e11bbd9b80b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467280"
---
# <a name="what-is-the-bing-image-search-api"></a>Was ist die Bing-Bildersuche-API?

Die Bing-Bildersuche-API ermöglicht die Verwendung kognitiver Bildersuchfunktionen in Ihrer Anwendung. Sie können Suchabfragen von Benutzern über die API senden und ähnlich wie bei [Bing Bilder](https://www.bing.com/images) relevante, hochwertige Bilder erhalten und anzeigen.

Hinweis: Die Bing-Bildersuche-API liefert ausschließlich Bilder als Suchergebnisse. Verwenden Sie für andere Arten von Webinhalten die [Bing-Websuche-API](../bing-web-search/search-the-web.md), die [Bing-Videosuche-API](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search) und die [Bing-News-Suche-API](https://review.docs.microsoft.com/azure/cognitive-services/bing-news-search).

## <a name="bing-image-search-features"></a>Features der Bing-Bildersuche

Die Bing-Bildersuche gibt in erster Linie relevante Bilder für eine Suchabfrage zurück. Der Dienst bietet aber auch noch einige andere Features zum intelligenten und zielgerichteten Abrufen von Bildern aus dem Web.


| Feature                                                                                                                                                                                 | BESCHREIBUNG                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [In Echtzeit vorgeschlagene Suchbegriffe](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Verwenden Sie die [Bing-Vorschlagssuche-API](../bing-autosuggest/get-suggested-search-terms.md), um während der Eingabe Suchvorschläge anzuzeigen und die Benutzerfreundlichkeit Ihrer App zu verbessern. |
| [Filtern und Einschränken von Bildergebnissen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#filtering-images)                       | Bearbeiten Sie Abfrageparameter, um die von Bing zurückgegebenen Bilder zu filtern.                                                                                                       |
| [Zuschneiden, Ändern der Größe und Anzeigen Miniaturansichten](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/resize-and-crop-thumbnails)                                                | Für die von der Bing-Bildersuche zurückgegebenen Bilder können Miniaturansichten als Vorschau bearbeitet und angezeigt werden.                                                                                      |
| [Pivotieren und Erweitern der Suchabfragen von Benutzern](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries#pivoting-the-query)               | Erweitern Sie Ihre Suchfunktionen, indem Sie von Bing vorgeschlagene Suchbegriffe in Abfragen einschließen und anzeigen.                                                                    |
| [Abrufen von beliebten Bildern](https://review.docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)                                                                     | Richten Sie eine Suche nach beliebten Bildern aus der ganzen Welt ein.                                                                                                          |

## <a name="workflow"></a>Workflow

Die Bing-Bildersuche-API ist ein RESTful-Webdienst und kann somit problemlos in jeder Programmiersprache aufgerufen werden, die HTTP-Anforderungen beherrscht und JSON analysieren kann. Der Dienst kann entweder über die [REST-API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) oder über das [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) verwendet werden.

1. Erstellen Sie ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit Zugriff auf die Bing-Suche-APIs. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses [Konto erstellen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Senden Sie eine Anforderung mit einer gültigen [Suchabfrage](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) an die API.
3. Analysieren Sie die zurückgegebene JSON-Nachricht, um die API-Antwort zu verarbeiten.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich zunächst die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) für die Bing-Bildersuche-API an.
Diese Demo zeigt, wie Sie schnell eine Suchabfrage anpassen und das Web nach Bildern durchsuchen.

Wenn Sie zum Aufrufen der API bereit sind, erstellen Sie ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses [Konto erstellen](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Um schnell mit Ihrer ersten API-Anforderung zu beginnen, können Sie sich mit Folgendem vertraut machen:

* [Senden von Suchabfragen an Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) mithilfe der REST-API. Oder:
* [Anfordern und Filtern](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) der von Bing zurückgegebenen Bilder mithilfe des SDK.

## <a name="see-also"></a>Weitere Informationen

* Der Referenzabschnitt für die [Bing-Bildersuche-API (Version 7)](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) enthält Definitionen und Informationen zu den Endpunkten, Headern, API-Antworten und Abfrageparametern, die Sie verwenden können, um bildbasierte Suchergebnisse anzufordern.

* In den [Verwendungs- und Anzeigeanforderungen für Bing](./useanddisplayrequirements.md) erfahren Sie, wie Inhalte und Informationen verwendet werden dürfen, die über die Bing-Suche-APIs gefunden wurden.

* Im Thema [Abrufen von Bildern aus dem Web mit der Bing-Bildersuche-API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) erfahren Sie, wie Bilder im Web gesucht und abgerufen werden.

* Das Thema [Senden von und Arbeiten mit Suchabfragen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) enthält Informationen zum Ausführen, Anpassen und Pivotieren von Suchabfragen.

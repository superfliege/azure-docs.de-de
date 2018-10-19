---
title: Websuche-Endpunkt
titleSuffix: Azure Cognitive Services
description: Überblick über den API-Endpunkt für die Websuche.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 3058ca6cf0eb99486dd4c269d43b274fb367f7a9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125458"
---
# <a name="web-search-endpoint"></a>Websuche-Endpunkt
Die **Websuche-API** gibt Webseiten, Nachrichten, Bilder, Videos und [Entitäten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) zurück. Entitäten enthalten zusammenfassende Informationen zur einer Person, einem Ort oder einem Thema.
## <a name="endpoint"></a>Endpunkt
Um mithilfe der Bing-API Websuchergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt. Die Header und die URL-Parameter definieren weitere Spezifikationen.

**Endpunkt:** gibt Webergebnisse zurück, die für die von `?q=""` definierte Suchabfrage des Benutzers relevant sind
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Endpunkt: Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern usw. finden Sie in der Referenz [API für die Bing-Websuche v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).

## <a name="response-json"></a>JSON-Antwort
Die Antwort auf eine Websuchanforderung enthält alle Ergebnisse als JSON-Objekten. Für eine Analyse sind Prozeduren zum Verarbeiten der einzelnen Elementtypen erforderlich. Beispiele finden Sie unter [Tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) und [Quellcode](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source).

## <a name="next-steps"></a>Nächste Schritte
Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse gemäß ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützen Abfragen, die eine bestimmte Sprache und/oder einen bestimmten Ort nach Längengrad, Breitengrad und Suchradius zurückgeben.

Vollständige Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten für jeden Typ.
Beispiele für grundlegende Anforderung mithilfe der Websuche-API finden Sie in den [Web-Schnellstarts](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).

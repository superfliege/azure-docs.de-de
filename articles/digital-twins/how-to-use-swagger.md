---
title: Grundlegendes zum Verwenden von Azure Digital Twins-Swagger | Microsoft-Dokumentation
description: Verwenden von Azure Digital Twins-Swagger
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 737c33f6b8cdf9bcb2530816601ff9b5eb994087
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624249"
---
# <a name="use-azure-digital-twins-swagger"></a>Verwenden von Azure Digital Twins-Swagger

Jede bereitgestellte Azure Digital Twins-Instanz enthält ihre eigene automatisch generierte Swagger-Referenzdokumentation.

[Swagger](https://swagger.io/) (oder [OpenAPI](https://www.openapis.org/)) vereinigt komplexe API-Informationen in einer interaktiven und sprachunabhängigen Referenzressource. Swagger stellt wichtiges Referenzmaterial dazu bereit, welche JSON-Nutzlasten, HTTP-Methoden und speziellen Endpunkte zum Ausführen von Vorgängen für eine API zu verwenden sind.

> [!IMPORTANT]
> Die Unterstützung für Swagger-Authentifizierung wird während der öffentlichen Vorschauphase vorübergehend deaktiviert.

## <a name="swagger-summary"></a>Swagger-Zusammenfassung

Swagger stellt eine interaktive Zusammenfassung Ihrer API bereit. Dazu gehören:

* API- und Objektmodellinformationen
* REST-API-Endpunkte, die erforderliche Anforderungsnutzlasten, Header, Parameter, Kontextpfade und HTTP-Methoden angeben
* Testen von API-Funktionalitäten
* Beispielantwortinformationen zum Überprüfen und Bestätigen von HTTP-Antworten
* Fehlercodeinformationen

Swagger ist ein hervorragendes Tool zur Unterstützung beim Entwickeln und Testen von Aufrufen, die an die Verwaltungs-API gesendet werden.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Referenzmaterial

In automatisch generiertem Referenzmaterial werden wichtige Konzepte und Objektmodelle erläutert.

Eine präzise Zusammenfassung beschreibt die API.

![Swagger-Einstieg][1]

Kern-API-Objektmodelle werden ebenfalls aufgelistet.

![Swagger-Modelle][2]

Sie können jedes aufgelistete Objektmodell auswählen, um eine detailliertere Zusammenfassung der Schlüsselattribute zu erhalten.

![Swagger-Modell][3]

Die generierten Swaggerobjektmodelle eignen sich ideal zum Anzeigen aller verfügbaren [Objekte und APIs](./concepts-objectmodel-spatialgraph.md) von Azure Digital Twins. Entwickler können diese Ressource nutzen, wenn sie Lösungen in Azure Digital Twins erstellen.

## <a name="endpoint-summary"></a>Zusammenfassung der Endpunkte

Swagger stellt auch eine umfassende Übersicht über alle Endpunkte bereit, die die API bilden.

Für jeden aufgelisteten Endpunkt werden auch die erforderlichen Anforderungsinformationen aufgeführt. Dazu gehören:

* Erforderliche Parameter
* Datentypen der erforderlichen Parameter
* HTTP-Methode zum Zugreifen auf die Ressource

![Swagger-Endpunkte][4]

Wählen Sie eine Ressource aus, um eine detailliertere Übersicht zu erhalten.

## <a name="use-swagger-to-test-endpoints"></a>Verwenden von Swagger zum Testen von Endpunkten

Eine der leistungsstarken Funktionalitäten von Swagger besteht darin, einen API-Endpunkt direkt über die Benutzeroberfläche der Dokumentation zu testen.

Nachdem Sie einen bestimmten Endpunkt ausgewählt haben, wird **Jetzt ausprobieren** angezeigt.

![Swagger vor Test][5]

Erweitern Sie diesen Abschnitt, um Eingabefelder für jeden erforderlichen und optionalen Parameter anzuzeigen. Geben Sie die entsprechenden Werte ein, und wählen Sie **Ausführen** aus.

![Swagger nach Test][6]

Wenn Sie den Test ausgeführt haben, können Sie die Antwortdaten überprüfen.

## <a name="swagger-response-data"></a>Swagger-Antwortdaten

Jeder aufgelistete Endpunkt enthält auch Antworttextdaten, um Ihre Entwicklung und Tests zu überprüfen. Diese Beispiele enthalten die Statuscodes und JSON-Objekte, die für erfolgreiche HTTP-Anforderungen angezeigt werden sollen.

![Swagger-Antwort][7]

Die Beispiele enthalten auch Fehlercodes zur Unterstützung beim Debuggen oder Verändern von fehlgeschlagenen Tests.

## <a name="swagger-oauth-20-authorization"></a>Swagger-OAuth 2.0-Autorisierung

Weitere Informationen zum interaktiven Testen von Anforderungen, die durch OAuth 2.0 geschützt sind, finden Sie in der [offiziellen Dokumentation](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> Die Unterstützung für OAuth 2.0-Authentifizierung wird während der öffentlichen Vorschauphase vorübergehend deaktiviert.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Digital Twins-Objektmodellen und zum Raumintelligenzgraph finden Sie unter [Grundlegendes zum Azure Digital Twins-Objektmodell](./concepts-objectmodel-spatialgraph.md).

Wie eine Authentifizierung über Ihre Verwaltungs-API erfolgt, erfahren Sie unter [Authentifizieren über APIs](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG

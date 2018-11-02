---
title: Grundlegendes zum Verwenden von Azure Digital Twins Swagger | Microsoft-Dokumentation
description: Verwenden von Azure Digital Twins Swagger
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: adgera
ms.openlocfilehash: dbadc90a206937d4c9f1d7b75a872d93b1a8a587
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323869"
---
# <a name="how-to-use-azure-digital-twins-swagger"></a>Verwenden von Azure Digital Twins Swagger

Jede bereitgestellte Azure Digital Twins-Instanz enthält ihre eigene automatisch generierte Swagger-Referenzdokumentation.

[Swagger](https://swagger.io/) (oder [OpenAPI](https://www.openapis.org/)) vereinigt komplexe API-Informationen in einer interaktiven und sprachunabhängigen Referenzressource. Insbesondere stellt Swagger wichtiges Referenzmaterial dazu bereit, welche JSON-Nutzlasten, HTTP-Methoden und speziellen Endpunkte zu verwenden sind, um Vorgänge für eine API auszuführen.

## <a name="swagger-summary"></a>Swagger-Zusammenfassung

Swagger stellt eine interaktive Zusammenfassung Ihrer API bereit. Dazu gehören:

* API- und Objektmodellinformationen
* REST-API-Endpunkte, die erforderliche Anforderungsnutzlasten, Header, Parameter, Kontextpfade und HTTP-Methoden angeben
* Testen von API-Funktionalitäten
* Beispielantwortinformationen zur Überprüfung und Bestätigung von HTTP-Antworten
* Fehlercodeinformationen

Swagger ist daher ein hervorragendes Tool zur Unterstützung beim Entwickeln und Testen von Aufrufen, die an die Verwaltungs-API gesendet werden.

> [!TIP]
> Zu Referenzzwecken wird eine Swagger-Vorschau bereitgestellt, um den API-Funktionsumfang zu veranschaulichen.
> Diese Vorschau finden Sie unter [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Auf Ihre eigene generierte Verwaltungs-API-Swagger-Dokumentation können Sie hier zugreifen:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourInstanceName` | Den Namen Ihrer Azure Digital Twins-Instanz |
| `yourLocation` | Die Serverregion, in der Ihre Instanz gehostet wird |

## <a name="reference-material"></a>Referenzmaterial

In automatisch generiertem Referenzmaterial werden entscheidende Konzepte und Objektmodelle erläutert.

In einer präzisen Zusammenfassung wird die API beschrieben:

![Swagger-Einstieg][1]

Kern-API-Objektmodelle werden ebenfalls aufgelistet:

![Swagger-Modelle][2]

Sie können auf jedes aufgelistete Objektmodell klicken, um eine detailliertere Übersicht über Schlüsselattribute zu erhalten:

![Swagger-Modell][3]

Die generierten Swagger-Objektmodelle sind praktisch, um alle verfügbaren Azure Digital Twins[-Objekte und -APIs](./concepts-objectmodel-spatialgraph.md) zu sehen. Dies ist eine großartige Ressource für Entwickler, die sie bei der Entwicklung von Lösungen für Azure Digital Twins nutzen können.

## <a name="endpoint-summary"></a>Zusammenfassung der Endpunkte

Swagger stellt auch eine umfassende Übersicht über alle Endpunkte bereit, die die API bilden.

Für jeden aufgelisteten Endpunkt werden auch die erforderlichen Anforderungsinformationen aufgeführt. Dazu gehören:

* Erforderliche Parameter
* Datentypen der erforderlichen Parameter
* Die HTTP-Methode zum Zugreifen auf die Ressource

![Swagger-Endpunkte][4]

Es kann auf jede Ressource geklickt werden, um eine detailliertere Übersicht zu sehen.

## <a name="using-swagger-to-test-endpoints"></a>Verwenden von Swagger, um Endpunkte zu testen

Eine der von Swagger bereitgestellten leistungsfähigen Funktionalitäten ist die Möglichkeit, über **Jetzt ausprobieren** einen API-Endpunkt direkt aus der UI-Dokumentation zu testen.

Nach dem Klicken auf einen bestimmten Endpunkt sehen Sie eine **Jetzt ausprobieren**-Schaltfläche:

![Swagger vor Test][5]

Nach Erweitern dieses Abschnitts werden Eingabefelder für jeden erforderlichen und optionalen Parameter angezeigt. Geben Sie die entsprechenden Werte ein, und klicken Sie auf **Ausführen**:

![Swagger nach Test][6]

Nach dem Ausführen des Tests können Sie die Antwortdaten überprüfen.

## <a name="swagger-response-data"></a>Swagger-Antwortdaten

Jeder aufgelistete Endpunkt enthält auch Antworttextdaten, um Ihre Entwicklung und Tests zu überprüfen. Diese Beispiele enthalten die gewünschten Statuscodes und JSON-Objekte für erfolgreiche HTTP-Anforderungen.

![Swagger-Antwort][7]

Die Beispiele enthalten auch Fehlercodes zur Unterstützung beim Debuggen oder Verändern von fehlgeschlagenen Tests.

## <a name="swagger-oauth-20-authorization"></a>Swagger-OAuth 2.0-Autorisierung

Informationen, wie Sie Anforderungen für API-Ressourcen, die durch OAuth 2.0 geschützt sind, interaktiv testen können, finden Sie in der [offiziellen Dokumentation](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen zu Azure Digital Twins-Objektmodellen und Raumintelligenzgraphen wünschen, lesen Sie [diesen Artikel](./concepts-objectmodel-spatialgraph.md).

Informationen dazu, wie ein Authentifizieren über Ihre Verwaltungs-API erfolgt, finden Sie unter [Verbinden mit und Authentifizieren über APIs](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.png
[2]: media/how-to-use-swagger/swagger_management_models.png
[3]: media/how-to-use-swagger/swagger_management_model.png
[4]: media/how-to-use-swagger/swagger_management_endpoints.png
[5]: media/how-to-use-swagger/swagger_management_try.png
[6]: media/how-to-use-swagger/swagger_management_tried.png
[7]: media/how-to-use-swagger/swagger_management_response.png

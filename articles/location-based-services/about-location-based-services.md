---
title: "Übersicht über Azure Location Based Services | Microsoft-Dokumentation"
description: "Einführung in Azure Location Based Services (Vorschauversion)"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: overview
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6871f174eb9bae57d9b4767520d0fb2d8d9631d3
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Einführung in Azure Location Based Services (Vorschauversion)
Bei Azure Location Based Services handelt es sich um ein Portfolio mit Geodiensten, die Dienst-APIs für Karten, Suche, Routing, Verkehr und Zeitzonen enthalten. Dank des Portfolios mit OneAPI-konformen Azure-Diensten können Sie vertraute Entwicklertools verwenden, um schnell Lösungen entwickeln und skalieren zu können, mit denen Standortinformationen in Ihre Azure-Lösungen integriert werden. Azure Location Based Services stellt Entwicklern aus allen Branchen leistungsstarke Geofunktionen mit umfassenden aktuellen Kartendaten zur Verfügung, die zum Bereitstellen von geografischem Kontext für Web- und Mobilanwendungen erforderlich sind. Bei Azure Location Based Services handelt es sich um verschiedene OneAPI-konforme Azure-REST-APIs. Zusätzlich ist ein webbasiertes JavaScript-Steuerelement enthalten, um die Entwicklung auf verschiedenen Medien sehr einfach, flexibel und portabel zu gestalten. 

Azure Location Based Services umfasst fünf Hauptdienste für Azure-Anwendungen, für die ein geografischer Kontext erforderlich ist. Die Dienste werden im Folgenden einzeln beschrieben.

**Renderingdienst**: Der Renderingdienst ist auf Entwickler ausgelegt, die Web- und mobile Anwendungen mit Kartenfunktionen erstellen möchten. Für den Dienst werden entweder qualitativ hochwertige Rastergrafikbilder, die in 19 Zoomebenen verfügbar sind, oder vollständig anpassbare Vektorformat-Kartenbilder verwendet.

![Azure Location Based Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Routendienst**: Der Routendienst verfügt über stabile reale Infrastruktur-Geometrieberechnungen und mehrere Transportmodus-Wegbeschreibungen. Der Dienst ermöglicht Entwicklern das Berechnen von Wegbeschreibungen für verschiedene Fortbewegungsarten, z.B. Auto, LKW, Fahrrad oder zu Fuß. Außerdem können einige Eingaben vorgenommen werden, z.B. Verkehrslage, Gewichtsbeschränkungen oder Gefahrentransport.

![Azure Location Based Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Suchdienst**: Der Suchdienst ist für Entwickler gedacht, um nach Adressen, Orten, Geschäftseinträgen nach Name oder Kategorie und anderen geografischen Informationen zu suchen. Außerdem kann mit dem Suchdienst eine [Inverse Geocodierung](https://en.wikipedia.org/wiki/Reverse_geocoding) für Adressen und Querstraßen basierend auf dem Breiten- bzw. Längengrad durchgeführt werden. 

![Azure Location Based Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Zeitzonendienst**: Mit dem Zeitzonendienst können Sie aktuelle, vergangene und zukünftige Zeitzoneninformationen abfragen, indem Sie entweder Breitengrad/Längengrad-Paare oder eine [IANA-ID](http://www.iana.org/) verwenden. Außerdem können Sie mit dem Zeitzonendienst Microsoft Windows-Zeitzonen-IDs in IANA-Zeitzonen konvertieren und einen Zeitzonenoffset zu UTC und die aktuelle Zeit in einer entsprechenden Zeitzone abrufen. Eine typische JSON-Antwort für eine Abfrage mit dem Zeitzonendienst sieht wie folgt aus:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

**Verkehrsinfodienst**: Der Verkehrsinfodienst ist eine Suite mit Webdiensten für Entwickler, mit denen Web- und mobile Anwendungen erstellt werden können, für die Verkehrsinfos erforderlich sind. Das Angebot ist wie folgt unterteilt:
1. Traffic Flow (Verkehrsfluss): Ermittelte Echtzeit-Geschwindigkeiten und Fahrtdauern für alle wichtigen Straßen des Netzes 
2. Traffic Incidents (Verkehrsunfälle): Präzise Übersicht über die Staus und Vorfälle/Unfälle im Straßennetz

![Azure Location Based Services – Verkehr](media/about-location-based-services/Introduction_Traffic.png)

Azure Location Based Services ist auf Mobilität ausgelegt und kann zum Betreiben von plattformübergreifenden Anwendungen verwendet werden, da das Programmiermodell agnostisch ist und die JSON-Ausgabe über REST-APIs unterstützt. Außerdem verfügt Azure LBS über ein komfortables JavaScript-Kartensteuerelement mit einem einfachen Programmiermodell zur schnellen und einfachen Entwicklung webbasierter und mobiler Anwendungen. 

Für Azure Location Based Services wird ein schlüsselbasiertes Authentifizierungsschema verwendet. Zum Zugreifen auf die Dienste navigieren Sie also zum [Azure-Portal](http://portal.azure.com) und erstellen ein Azure Location Based Services-Konto. Für Ihr Konto wurden zwei Schlüssel vorab generiert. Beginnen Sie damit, diese Standortfunktionen direkt in Ihre Anwendungen zu integrieren, indem Sie in den Anforderungen, die Sie an den Azure Location Based Services-Dienst senden, einen Ihrer Schlüssel verwenden.

**Beziehung zu Bing Maps:** Beachten Sie, dass sich die in diesem Dokument beschriebenen Azure Location Based Services von den Diensten unterscheiden, die von Bing Maps bereitgestellt werden.  Die Funktionen sind zwar größtenteils gleich, bei den beiden Diensten handelt es sich jedoch um zwei unterschiedliche, nicht zusammenhängende Dienste.  Die Verfügbarkeit dieses neuen, separat verwalteten Diensts in Azure hat keine Auswirkungen auf das Bing Maps-Produktangebot oder auf die dazugehörige Roadmap.

Das Ziel von Microsoft besteht darin, der Entwicklercommunity eine Wahl bei den angebotenen Standortdiensten zu geben.  Im Anschluss finden Sie einen kurzen Entwicklerleitfaden, der anhand verschiedener Anwendungsfälle und Kundensituationen Aufschluss darüber gibt, wann welcher Dienst verwendet wird.  Beachten Sie, dass dieser Leitfaden momentan für Azure LBS in der öffentlichen Vorschauphase gilt und aktualisiert wird, sobald im Laufe von 2018 die allgemeine Verfügbarkeit erreicht wird.

| Kundenkriterien | Verwendung von Azure Location Based Services | Verwendung von Bing Maps |
| ------------- | ------------- | ------------- |
| Entwicklungsumgebung | Integrieren oder Nutzen anderer Azure-Dienste | Verwenden einer Cloud- oder anderen Entwicklungsumgebung eines Drittanbieters |
| Entwicklungsphase  | Da Azure LBS derzeit als öffentliche Vorschauversion verfügbar ist, ist es für frühzeitige Tests und Proof of Concept-Entwicklung optimiert. | Für eine Produktionsumgebung wird ein SLA auf Unternehmensniveau benötigt. |
| Preisoptionen | Vorläufige Preisoptionen für Entwickler ausreichend | Angepasste Preise für Unternehmen erforderlich |
| Anwendungsfallumgebung | Verwendung im Fahrzeug erforderlich | Verwendung im Fahrzeug nicht erforderlich |
| Geografische Abdeckung | Indien, China, Japan und Südkorea nicht erforderlich | Kartenabdeckung für Indien, China, Japan und Südkorea erforderlich |
| Karteninhalte | Standardmäßige Oberflächenkarten ausreichend | Satellitenbilder, Luftaufnahmen und Bilder auf Straßenebene erforderlich |
| Zugrunde liegende Kartenquelle | TomTom-Kartendaten bevorzugt | HERE-Kartendaten bevorzugt |

[Registrieren Sie sich noch heute für ein Azure Location Based Services-Konto!](http://aka.ms/azurelbsportal)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun eine Übersicht über Azure Location Based Services (Vorschauversion) erhalten. Der nächste Schritt umfasst das Ausprobieren einer Beispiel-App, mit der Location Based Services veranschaulicht werden, sowie das Erstellen eines End-to-End-Szenarios in Ihrer Webanwendung.

> [!div class="nextstepaction"]
> [Launch a demo interactive map search using Azure Location Based Services (preview)](quick-demo-map-app.md) (Starten einer interaktiven Demonstration einer Kartensuche mit Azure Location Based Services (Vorschauversion))
> [Suchen nach einem Point of Interest in der Nähe mit Azure Location Based Services](tutorial-search-location.md)

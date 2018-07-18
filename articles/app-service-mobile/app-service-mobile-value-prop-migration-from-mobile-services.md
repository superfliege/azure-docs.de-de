---
title: Ich verwende Mobile Services – welche Vorteile bietet App Service?
description: Erfahren Sie, welche Vorteile App Service für Ihre vorhandenen Mobile Services-Projekte bietet.
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 365f00ced38a1ddc20df211121fba43efff8ea87
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
ms.locfileid: "29460411"
---
# <a name="getting-started"></a>Ich verwende Mobile Services – welche Vorteile bietet App Service?
## <a name="overview"></a>Übersicht
Ihr vorhandener Mobile Service ist sicher und wird weiterhin unterstützt. Die *Azure App Service*-Plattform hat für Ihre mobile App aber Vorteile, die mit Mobile Services derzeit nicht verfügbar sind:

* Einfacheres und kostengünstigeres Angebot für Apps, die sowohl Web- als auch mobile Clients enthalten
* Neue Hostfunktionen, einschließlich Webaufträgen, benutzerdefinierter CNames, besserer Überwachung
* Integration in Traffic Manager
* Konnektivität mit lokalen Ressourcen und VPNs mit VNet zusätzlich zu Hybridverbindungen
* Überwachung, Warnungen und Problembehandlung für Ihre App AppInsights
* Umfangreicheres Spektrum an zugrunde liegenden Computeressourcen und Preisen
* Integrierte automatische Skalierung, Lastenausgleich und Leistungsüberwachung
* Integrierte Funktionen für Staging, Sicherung, Rollback und Produktionstests

## <a name="new-hosting-features"></a>Neue Hostingfunktionen
In *Azure App Service* wird der *Mobile App*-Back-End-Code in demselben Container wie Web-App und API-App ausgeführt. Sie können alle Funktionen in diesem Container nutzen, z.B. einige, die derzeit nicht in Mobile Services vorhanden sind:

* Hinzufügen ständig ausgeführter Back-End-Logik über Webaufträge
* Sicherstellen der ständigen Ausführung des Back-End-Codes
* Verwenden benutzerdefinierter CNames zum Bereitstellen benutzerfreundlicher und stabiler Namen für Ihre mobilen Back-End-Endpunkte
* Geoskalierung Ihrer App mit Traffic Manager
* Fügen Sie alle Bibliotheken und Pakete hinzu, die Sie möchten.
* (Für .NET) Nutzen aller Features von ASP.NET, einschließlich MVC
* (For Node.js) Nutzen aller reinen JavaScript-Bibliotheken des Knotenökosystems, einschließlich gemeinsamer MVC-Bibliotheken

## <a name="access-on-premises-data-using-vnet"></a>Zugriff auf lokale Daten über VNet
Mit Mobile Services können Sie schon heute Hybrid Connections für den Zugriff auf lokale Ressourcen verwenden. Es gibt jedoch Situationen, in denen eine VPN-Lösung bevorzugt wird. Mit *Azure App Service* können Sie Azure-VNet für Mobile App-Back-End-Code verwenden.

## <a name="use-your-favorite-backend-language"></a>Verwenden Ihrer bevorzugten Back-End-Sprache
*Azure App Service* bietet umfangreichere Unterstützung mit mehr Funktionen für ASP.NET- und Node.js-Plattformen, einschließlich Zugriff auf die neuesten Laufzeiten.

## <a name="set-up-automatic-scale"></a>Einrichten der automatischen Skalierung
Mit Mobile Services wurden alle Instanzen des Back-End-Codes auf kleinen virtuellen Computern ausgeführt. *Azure App Service* können Sie die Größe der virtuellen Computer aus einem umfassenderen Satz von Optionen auswählen. Sie können zudem schnell zentral oder horizontal hochskalieren, um die gesamte von den Kunden eingehende Workload basierend auf verschiedenen Leistungsmetriken zu bewältigen.

## <a name="be-in-the-know"></a>Auf dem Laufenden bleiben
Reagieren Sie auf Probleme in Echtzeit mithilfe von Überwachung und Warnungen, über die Sie und Ihr Team automatisch benachrichtigt werden. Integrieren Sie die erweiterten App-Analyse- und Überwachungsfunktionen von AppInsights, und erhalten Sie Informationen zur Leistung Ihrer mobilen App. Mit *Azure App Service* können Sie jetzt programmgesteuert oder über das Azure-Portal Warnungen basierend auf verschiedenen Leistungsmetriken einrichten.

## <a name="keep-your-assets-safe"></a>Schützen Ihrer Ressourcen
Sichern Sie automatisch Back-End und Datenbank. Ihr Code und Ihre Daten werden in Notfällen geschützt und können problemlos wiederhergestellt werden, sodass Sie sich keine Sorgen um Ihr Unternehmen machen müssen.

## <a name="ready-stage-go"></a>Auf die Plätze, Staging, los!
Mit *Azure App Service* können Sie jetzt mehrere private Test- und Stagingumgebungen für Ihre mobilen Apps erstellen. Verwenden Sie sie zum Durchführen von Tests vor der Bereitstellung. Wechseln Sie ohne Ausfallzeiten in die Produktion. Web-Apps werden vorab geladen, um höchste Kundenzufriedenheit zu gewährleisten.

Sie können mithilfe dieses *Tutorials* die Vorteile von [App Service](app-service-mobile-migrating-from-mobile-services.md) für Ihre vorhandenen Mobile Service-Projekte nutzen.

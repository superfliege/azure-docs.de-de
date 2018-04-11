---
title: Details zum Enterprise-Legacytarif für Azure Application Insights | Microsoft-Dokumentation
description: Verwalten Sie Telemetriedatenvolumen, und überwachen Sie Kosten in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 6e7591ccf0f21099474a08dda088422c377135f6
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="enterprise-plan-details"></a>Details des Enterprise-Tarifs

Application Insights ist in zwei Tarifen verfügbar. Der Standardtarif heißt [Basic](app-insights-pricing.md) und umfasst ohne Zusatzkosten die gleichen Features wie der Enterprise-Tarif. Die Abrechnung erfolgt in erster Linie anhand des erfassten Datenvolumens. Bei Verwendung der Operations Management Suite sollten Sie ggf. den Enterprise-Tarif wählen, für den eine Gebühr pro Knoten zusammen mit täglichen Datenkontingenten gilt. Anschließend werden die über das enthaltene Kontingent hinausgehenden Daten berechnet.

Aktuelle Preise in Ihrer Währung und Region finden Sie auf der [Application Insights-Seite mit Preisen](http://azure.microsoft.com/pricing/details/application-insights/).

## <a name="heres-how-the-enterprise-plan-works"></a>So funktioniert der Enterprise-Tarif

* Sie bezahlen pro Knoten, der Telemetriedaten für Apps im Enterprise-Tarif sendet.
 * Ein *Knoten* ist entweder ein physischer oder ein virtueller Servercomputer oder eine PaaS-Rolleninstanz (Platform-as-a-Service), der bzw. die Ihre App hostet.
 * Entwicklungscomputer, Clientbrowser und mobile Geräte werden nicht als Knoten gezählt.
 * Wenn die App mehrere Komponenten aufweist, die Telemetriedaten senden, z.B. ein Webdienst und ein Back-End-Worker, werden sie separat gezählt.
 * [Live Metrics Stream](app-insights-live-stream.md)-Daten werden nicht zu Preiszwecken gezählt.* In einem Abonnement fallen Gebühren knoten- und nicht App-bezogen an. Wenn Sie fünf Knoten haben, die Telemetriedaten für 12 Apps senden, wird die Gebühr für fünf Knoten berechnet.
* Obwohl Gebühren pro Monat angegeben sind, müssen Sie nur für jede Stunde bezahlen, in der ein Knoten Telemetriedaten von einer App sendet. Die Stundengebühr wird als Monatsgebühr angegeben: 744 (die Anzahl der Stunden pro Monat mit 31 Tagen).
* Eine Datenvolumenzuteilung von 200 MB pro Tag ist für jeden erkannten Knoten vorgesehen (mit stundenbezogener Granularität). Eine nicht genutzte Datenzuteilung wird nicht von einem Tag auf den nächsten übertragen.
 * Wenn Sie sich für die Preisoption Enterprise entscheiden, wird jedem Abonnement basierend auf der Anzahl von Knoten, die in diesem Abonnement Telemetriedaten an Application Insights-Ressourcen senden, ein tägliches Datenkontingent zugeordnet. Wenn Sie also fünf Knoten haben, die den ganzen Tag Daten senden, wird Ihnen insgesamt 1 GB für alle Application Insights-Ressourcen in diesem Abonnement zugeteilt. Es ist dabei unerheblich, ob bestimmte Knoten mehr Daten senden als andere, da die enthaltene Datenmenge für alle Knoten zusammen berechnet wird. Wenn die Application Insights-Ressourcen an einem bestimmten Tag mehr Daten empfangen, als in der täglichen Datenzuteilung für dieses Abonnement vorgesehen ist, gelten Gebühren pro GB Überschreitungsdaten. 
 * Das tägliche Datenkontingent wird berechnet aus der Anzahl der Stunden pro Tag (nach UTC), die jeder Knoten Telemetriedaten sendet, dividiert durch 24 mal 200 MB. Wenn Sie also über vier Knoten verfügen, die an 15 Stunden pro Tag Telemetriedaten senden, sind pro Tag ((4 x 15) / 24) x 200 MB = 500 MB an Daten enthalten. Bei einem Preis von 2,30 USD pro GB für Datenüberschreitung wäre die Gebühr 1,15 USD, wenn die Knoten an diesem Tag 1 GB an Daten senden.
 * Das tägliche Kontingent gemäß Enterprise-Tarif gilt nicht für Anwendungen, für die Sie die Basic-Option ausgewählt haben, und ein nicht genutztes Kontingent wird nicht auf den nächsten Tag übertragen. 

## <a name="here-are-some-examples-of-determining-distinct-node-count"></a>Einige Beispiele zum Bestimmen der eindeutigen Knotenanzahl

| Szenario                               | Gesamtanzahl der Knoten pro Tag |
|:---------------------------------------|:----------------:|
| 1 Anwendung verwendet 3 Azure App Service-Instanzen und 1 virtuellen Server | 4 |
| 3 Anwendungen auf 2 virtuellen Computern, und die Application Insights-Ressourcen für diese Anwendungen befinden sich im selben Abonnement und gehören zum Enterprise-Tarif | 2 | 
| 4 Anwendungen, deren Application Insights-Ressourcen Teil des gleichen Abonnements sind. Jede Anwendung führt in 16 Stunden außerhalb der Spitzenzeiten 2 Instanzen und in 8 Stunden während der Spitzenzeiten 4 Instanzen aus. | 13,33 | 
| Clouddienste mit 1 Workerrolle und 1 Webrolle, die je 2 Instanzen ausführen | 4 | 
| Service Fabric-Cluster mit 5 Knoten und 50 Microservices, wobei jeder Microservice 3 Instanzen ausführt | 5|

* Die genaue Knotenanzahl hängt vom Application Insights SDK ab, das von der Anwendung verwendet wird. 
  * In den SDK-Versionen 2.2 und höher melden das [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) und das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) von Application Insights jeden Anwendungshost als Knoten, z.B. den Computernamen für physische Server und VM-Hosts oder den Instanznamen bei Clouddiensten.  Die einzige Ausnahme bilden Anwendungen, die nur [.NET Core](https://dotnet.github.io/) und das Application Insights Core SDK verwenden. In diesem Fall wird nur ein Knoten für alle Hosts gemeldet, da der Hostname nicht verfügbar ist. 
  * Für frühere Versionen des SDK verhält sich das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) wie die höheren SDK-Versionen, das [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) meldet jedoch nur einen Knoten, unabhängig von der Anzahl der tatsächlichen Anwendungshosts. 
  * Ihre Anwendung verwendet das SDK, um „roleInstance“ auf einen benutzerdefinierten Wert festzulegen. Standardmäßig wird dieser Wert verwendet, um die Anzahl der Knoten zu bestimmen. 
  * Wenn Sie eine neue SDK-Version mit einer App verwenden, die über Clientcomputer oder Mobilgeräte ausgeführt wird, ist es möglich, dass für die Anzahl der Knoten eine sehr hohe Zahl zurückgegeben wird (durch die große Anzahl von Clientcomputern oder Mobilgeräten). 

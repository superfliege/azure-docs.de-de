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
ms.openlocfilehash: 6df013506e4541fee7850850776d26e5c69a799d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="enterprise-plan-details"></a>Details des Enterprise-Tarifs

Azure Application Insights bietet zwei Tarife: Basic und Enterprise. Der [Basic](app-insights-pricing.md)-Tarif ist der Standardtarif. Er umfasst alle Features des Enterprise-Tarifs, die ohne Aufpreis genutzt werden können. Im Basic-Tarif wird in erster Linie das Volumen der erfassten Daten abgerechnet. 

Für den Enterprise-Tarif gilt eine knotenbasierte Gebühr, und jedem Knoten wird ein tägliches Datenkontingent zugewiesen. Im Enterprise-Tarif werden die Daten, die über das enthaltene Datenkontingent hinausgehen, berechnet. Wenn Sie die Operations Management Suite verwenden, sollten Sie den Enterprise-Tarif wählen. 

Aktuelle Preise in Ihrer Währung und für Ihre Region finden Sie auf der Seite [Application Insights – Preise](http://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-the-enterprise-plan-works"></a>So funktioniert der Enterprise-Tarif

* Sie bezahlen für jeden Knoten, der Telemetriedaten für irgendwelche Apps im Enterprise-Tarif sendet.
 * Ein *Knoten* ist entweder ein physischer oder ein virtueller Servercomputer oder eine PaaS-Rolleninstanz (Platform-as-a-Service), der bzw. die Ihre App hostet.
 * Entwicklungscomputer, Clientbrowser und mobile Geräte zählen nicht als Knoten.
 * Wenn die App mehrere Komponenten aufweist, die Telemetriedaten senden, z. B. ein Webdienst und ein Back-End-Worker, werden die Komponenten separat gezählt.
 * [Live Metrics Stream](app-insights-live-stream.md)-Daten werden bei der Preisgestaltung nicht berechnet. In einem Abonnement fallen die Gebühren pro Knoten und nicht pro App an. Wenn Sie fünf Knoten haben, die Telemetriedaten für 12 Apps senden, wird die Gebühr für fünf Knoten berechnet.
* Obwohl Gebühren pro Monat angegeben sind, müssen Sie nur für jede Stunde bezahlen, in der ein Knoten Telemetriedaten von einer App sendet. Die Stundengebühr entspricht der angegebenen Monatsgebühr, die durch 744 (die Anzahl der Stunden eines Monats mit 31 Tagen) dividiert wurde.
* Eine Datenvolumenzuteilung von 200 MB pro Tag ist für jeden erkannten Knoten vorgesehen (mit stundenbezogener Granularität). Eine nicht genutzte Datenzuteilung wird nicht von einem Tag auf den nächsten übertragen.
 * Wenn Sie sich für den Enterprise-Tarif entscheiden, wird jedem Abonnement basierend auf der Anzahl von Knoten, die in diesem Abonnement Telemetriedaten an Application Insights-Ressourcen senden, ein tägliches Datenkontingent zugeordnet. Wenn Sie also fünf Knoten haben, die den ganzen Tag Daten senden, wird Ihnen insgesamt 1 GB für alle Application Insights-Ressourcen in diesem Abonnement zugeteilt. Es ist dabei unerheblich, ob bestimmte Knoten mehr Daten senden als andere, da die enthaltene Datenmenge für alle Knoten zusammen berechnet wird. Wenn die Application Insights-Ressourcen an einem bestimmten Tag mehr Daten empfangen, als in der täglichen Datenzuteilung für dieses Abonnement vorgesehen ist, gelten Gebühren pro GB Überschreitungsdaten. 
 * Das tägliche Datenkontingent wird berechnet aus der Anzahl der Stunden pro Tag (nach UTC), die jeder Knoten Telemetriedaten sendet, dividiert durch 24 und multipliziert mit 200 MB. Wenn Sie also vier Knoten haben, die an 15 Stunden des Tages Telemetriedaten senden, sind für diesen Tag ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB an Daten enthalten. Bei einem Preis von 2,30 USD pro GB für Datenüberschreitung wäre die Gebühr 1,15 USD, wenn die Knoten an diesem Tag 1 GB an Daten senden.
 * Das tägliche Kontingent des Enterprise-Tarifs wird nicht für Anwendungen verwendet, für die Sie den Basic-Tarif gewählt haben. Ein nicht verwendetes Kontingent wird nicht von Tag zu Tag übernommen. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Beispiele, wie unterschiedliche Anzahlen von Knoten bestimmt werden

| Szenario                               | Gesamtanzahl der Knoten pro Tag |
|:---------------------------------------|:----------------:|
| 1 Anwendung verwendet 3 Azure App Service-Instanzen und 1 virtuellen Server | 4 |
| 3 Anwendungen, die auf 2 virtuellen Computern ausgeführt werden; die Application Insights-Ressourcen für diese Anwendungen befinden sich im selben Abonnement und gehören zum Enterprise-Tarif | 2 | 
| 4-Anwendungen, deren Insights-Ressourcen sich im selben Abonnement befinden; jede Anwendung wird in 2 Instanzen in 16 Nebenstunden und in 4 Instanzen in 8 Spitzenstunden ausgeführt | 13,33 | 
| Clouddienste mit 1 Workerrolle und 1 Webrolle, die je 2 Instanzen ausführen | 4 | 
| Ein Service Fabric-Cluster mit 5 Knoten, in dem 50 Microservices ausgeführt werden; jeder Microservice führt 3 Instanzen aus | 5|

* Die genaue Knotenanzahl hängt vom Application Insights SDK ab, das von der Anwendung verwendet wird. 
  * Ab der SDK-Version 2.2 melden sowohl das Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) als auch das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) jeden Anwendungshost als einen Knoten. Beispiele sind der Computername für einen physischen Server und VM-Hosts oder der Instanzname für Clouddienste.  Die einzige Ausnahme ist eine Anwendung, die nur [.NET Core](https://dotnet.github.io/) und das Application Insights Core SDK verwendet. In diesem Fall wird nur ein Knoten für alle Hosts gemeldet, weil der Hostname nicht verfügbar ist. 
  * Für frühere Versionen des SDK verhält sich das [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) wie die neueren SDK-Versionen, das [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) meldet jedoch nur einen Knoten, unabhängig von der Anzahl der Anwendungshosts. 
  * Wird in Ihrer Anwendung das SDK verwendet, um **roleInstance** auf einen benutzerdefinierten Wert festzulegen, wird standardmäßig dieser Wert verwendet, um die Anzahl der Knoten zu bestimmen. 
  * Wenn Sie eine neue SDK-Version mit einer App verwenden, die auf Clientcomputern oder Mobilgeräten ausgeführt wird, kann für die Anzahl der Knoten eine sehr große Zahl zurückgegeben werden (wegen der großen Anzahl von Clientcomputern oder Mobilgeräten). 

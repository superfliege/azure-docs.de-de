---
title: "Anwendungsübersicht in Azure Application Insights | Microsoft-Dokumentation"
description: "Überwachen von komplexen Anwendungstopologien mit der Anwendungsübersicht"
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 3bbed59bf93eab5e729fbdd3ccae04599ac47081
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="application-map-triage-distributed-applications"></a>Anwendungsübersicht: Selektieren von verteilten Anwendungen
Mit der Anwendungsübersicht können Sie Leistungsengpässe oder Fehlerhotspots in allen Komponenten Ihrer verteilten Anwendung erkennen. Jeder Knoten in der Übersicht repräsentiert eine Anwendungskomponente bzw. deren Abhängigkeiten und weist einen Key Performance Indicator (KPI) zur Integrität und einen Warnungsstatus auf. Für jede Komponente können Sie detailliertere Diagnosen anzeigen, z.B. für Application Insights-Ereignisse. Wenn Ihre App Azure-Dienste nutzt, können Sie auch Azure-Diagnosedaten anzeigen, z.B. SQL Database Advisor-Empfehlungen.

## <a name="what-is-a-component"></a>Was ist eine Komponente?

Komponenten sind unabhängig bereitstellbare Teile Ihrer verteilten Anwendung/Microserviceanwendung. Entwickler und Betriebsteams verfügen über Sichtbarkeit oder Zugriff für die von diesen Anwendungskomponenten generierte Telemetrie auf Codeebene. 

* Komponenten unterscheiden sich von „beobachteten“ externen Abhängigkeiten, wie SQL, EventHub usw., auf die Ihr Team/Ihre Organisation möglicherweise keinen Zugriff hat (Code oder Telemetrie).
* Komponenten können in einer beliebigen Anzahl von Server-/Rollen-/Containerinstanzen ausgeführt werden.
* Komponenten können separate Application Insights-Instrumentierungsschlüssel (sogar in verschiedenen Abonnements) oder verschiedene Rollen aufweisen, die alle an einen einzelnen Application Insights-Instrumentierungsschlüssel berichten. In der Vorschau werden die Komponenten unabhängig von der Art ihrer Einrichtung in der Übersicht angezeigt.

## <a name="composite-application-map-preview"></a>Verbundanwendungsübersicht (Vorschau)
*Dies ist eine frühe Vorschauversion, und wir werden dieser Übersicht weitere Features hinzufügen. Wir würden uns sehr über Ihr Feedback zu dieser neuen Oberfläche freuen. Sie können problemlos zwischen der Vorschauversion und der klassischen Oberfläche wechseln.*

Aktivieren Sie in der [Vorschauliste](app-insights-previews.md) die Option „Verbundanwendungsübersicht“, oder klicken Sie auf der Umschaltfläche in der rechten oberen Ecke auf „Vorschauübersicht“. Sie können diese Umschaltfläche verwenden, um zur klassischen Oberfläche zurückzuwechseln.
![Aktivieren der Vorschauübersicht](media/app-insights-app-map/preview-from-classic.png)

>[!Note]
Diese Vorschau ersetzt die vorherige Vorschau der Anwendungsübersicht mit mehreren Rollen. Verwenden Sie diese Version aktuell, um die gesamte Topologie über mehrere Ebenen von Abhängigkeiten zwischen Anwendungskomponenten hinweg anzuzeigen. Senden Sie uns Feedback. Wir werden weitere Funktionen ähnlich denen hinzufügen, die von der klassischen Übersicht unterstützt werden.

Sie können die vollständige Anwendungstopologie über mehrere Ebenen verwandter Anwendungskomponenten hinweg anzeigen. Bei Komponenten kann es sich um verschiedene Application Insights-Ressourcen oder verschiedene Rollen in einer einzelnen Ressource handeln. Die App-Übersicht ermittelt Komponenten, indem sie HTTP-Abhängigkeitsaufrufe zwischen Servern verfolgt, auf denen das Application Insights SDK installiert ist. 

Diese Funktionalität beginnt mit einer progressiven Ermittlung der Komponenten. Wenn Sie die Vorschauversion zum ersten Mal laden, wird eine Reihe von Abfragen ausgelöst, um die Komponenten zu ermitteln, die mit dieser Komponente in Zusammenhang stehen. Mit einer Schaltfläche in der oberen linken Ecke wird die Anzahl der Komponenten in Ihrer Anwendung aktualisiert, wenn diese ermittelt werden. 
![Vorschauübersicht](media/app-insights-app-map/preview.png)

Durch Klicken auf „Übersichtskomponenten aktualisieren“ wird die Übersicht mit allen bis zu diesem Zeitpunkt ermittelten Komponenten aktualisiert.
![Vorschau der geladenen Übersicht](media/app-insights-app-map/components-loaded-hierarchical.png)

Wenn es sich bei allen Komponenten um Rollen in einer einzelnen Application Insights-Ressource handelt, ist dieser Ermittlungsschritt nicht erforderlich. Nach dem ersten Ladevorgang für eine solche Anwendung sind alle Komponenten vorhanden.

Eines der wichtigsten Ziele dieser neuen Oberfläche ist es, komplexe Topologien mit Hunderten von Komponenten zu visualisieren. Die neue Oberfläche unterstützt das Zoomen und ergänzt weitere Details, während Sie zoomen. Sie können herauszoomen, um eine größere Anzahl von Komponenten auf einem Blick anzuzeigen, und dennoch weiterhin Komponenten mit höheren Fehlerraten ermitteln. 

![Zoomebenen](media/app-insights-app-map/zoom-levels.png)

Klicken Sie auf eine beliebige Komponente, um die zugehörigen Details anzuzeigen und zur Selektierung von Leistungs- und Fehleraspekten für diese Komponente zu wechseln.

![Flyout](media/app-insights-app-map/preview-flyout.png)


## <a name="classic-application-map"></a>Klassische Anwendungsübersicht

Die Zuordnung zeigt Folgendes:

* Verfügbarkeitstests
* Clientseitige Komponente (mit dem JavaScript SDK überwacht)
* Serverseitige Komponente
* Abhängigkeiten von den Client- und Serverkomponenten

![App-Zuordnung](./media/app-insights-app-map/02.png)

Sie können Abhängigkeitslinkgruppen erweitern oder reduzieren:

![Reduzieren](./media/app-insights-app-map/03.png)

Wenn zahlreiche Abhängigkeiten eines Typs (SQL, HTTP usw.) vorhanden sind, werden sie ggf. gruppiert angezeigt. 

![Gruppierte Abhängigkeiten](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Erkennen von Problemen
Jeder Knoten verfügt über relevante Leistungsindikatoren wie Last, Leistung und Fehlerraten für die jeweilige Komponente. 

Warnsymbole weisen auf mögliche Probleme hin. Eine orangefarbene Warnung bedeutet, dass Fehler bei Anforderungen, Seitenaufrufen oder Abhängigkeitsaufrufen vorliegen. Rot gibt eine Fehlerquote von über 5 Prozent an. Wenn Sie diese Schwellenwerte anpassen möchten, öffnen Sie die Optionen.

![Fehlersymbole](./media/app-insights-app-map/04.png)

Aktive Warnungen werden ebenfalls angezeigt: 

![Aktive Warnungen](./media/app-insights-app-map/05.png)

Bei der Verwendung von SQL Azure wird mithilfe eines Symbols darauf hingewiesen, dass Empfehlungen zur Leistungssteigerung vorhanden sind. 

![Azure-Empfehlung](./media/app-insights-app-map/06.png)

Klicken Sie auf ein beliebiges Symbol, um weitere Details anzuzeigen:

![Azure-Empfehlung](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Anzeigen detaillierter Diagnoseinformationen
Für alle Knoten in der Zuordnung können detaillierte Diagnoseinformationen angezeigt werden. Die Optionen variieren abhängig vom Knotentyp.

![Serveroptionen](./media/app-insights-app-map/09.png)

Für in Azure gehostete Komponenten beinhalten die Optionen direkte Links zu den Komponenten.

## <a name="filters-and-time-range"></a>Filter und Zeitraum
In der Zuordnung werden standardmäßig alle Daten zusammengefasst, die für den ausgewählten Zeitraum zur Verfügung stehen. Sie können sie jedoch nach bestimmten Vorgangsnamen oder Abhängigkeiten filtern.

* Vorgangsname: Dazu gehören Seitenaufrufe und serverseitige Anforderungen. Mit dieser Option zeigt die Zuordnung den KPI für den server-/clientseitigen Knoten nur für die ausgewählten Vorgänge an. Die im Kontext dieser spezifischen Vorgänge aufgerufenen Abhängigkeiten werden angezeigt.
* Basisname der Abhängigkeit: Dazu zählen die browserseitigen AJAX-Abhängigkeiten und die serverseitigen Abhängigkeiten. Wenn Sie benutzerdefinierte Abhängigkeitstelemetriedaten mit der TrackDependency-API erfassen, werden diese hier ebenfalls angezeigt. Sie können die Abhängigkeiten auswählen, die in der Zuordnung angezeigt werden sollen. Derzeit werden die serverseitigen Anforderungen oder die clientseitigen Seitenaufrufe nicht gefiltert.

![Filter festlegen](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Speichern von Filtern
Heften Sie zum Speichern der angewendeten Filter die gefilterte Ansicht an ein [Dashboard](app-insights-dashboards.md)an.

![An Dashboard anheften](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Fehlerbereich
Wenn Sie auf einen Knoten in der Zuordnung klicken, wird ein Fehlerbereich auf der rechten Seite angezeigt, in dem die Fehler für diesen Knoten zusammengefasst sind. Fehler werden zunächst nach Vorgangs-ID und dann nach Problem-ID gruppiert.

![Fehlerbereich](./media/app-insights-app-map/error-pane.png)

Durch Klicken auf einen Fehler wird die letzte Instanz dieses Fehlers aufgerufen.

## <a name="resource-health"></a>Ressourcenintegrität
Für einige Ressourcentypen wird die Ressourcenintegrität oben im Fehlerbereich angezeigt. Beispiel: Wenn Sie auf einen SQL-Knoten klicken, werden die Datenbankintegrität und ausgelöste Warnungen angezeigt.

![Ressourcenintegrität](./media/app-insights-app-map/resource-health.png)

Sie können auf den Ressourcennamen klicken, um Standardübersichtsmetriken für diese Ressourcen anzuzeigen.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Senden Sie über die Feedbackoption im Portal Feedback.

![MapLink-1 (Abbildung)](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Nächste Schritte

* [Azure-Portal](https://portal.azure.com)

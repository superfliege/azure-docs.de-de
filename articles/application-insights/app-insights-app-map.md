---
title: Anwendungszuordnung in Azure Application Insights | Microsoft-Dokumentation
description: Überwachen von komplexen Anwendungstopologien mit der Anwendungsübersicht
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 9b39eef5accec4764f61ab31dd894d368242ee3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094649"
---
# <a name="application-map-triage-distributed-applications"></a>Anwendungsübersicht: Selektieren von verteilten Anwendungen

Mit der Anwendungsübersicht können Sie Leistungsengpässe oder Fehlerhotspots in allen Komponenten Ihrer verteilten Anwendung erkennen. Jeder Knoten in der Übersicht repräsentiert eine Anwendungskomponente bzw. deren Abhängigkeiten und weist einen Key Performance Indicator (KPI) zur Integrität und einen Warnungsstatus auf. Für jede Komponente können Sie detailliertere Diagnosen anzeigen, z.B. für Application Insights-Ereignisse. Wenn Ihre App Azure-Dienste nutzt, können Sie auch Azure-Diagnosedaten anzeigen, z.B. SQL Database Advisor-Empfehlungen.

## <a name="what-is-a-component"></a>Was ist eine Komponente?

Komponenten sind unabhängig bereitstellbare Teile Ihrer verteilten Anwendung/Microserviceanwendung. Entwickler und Betriebsteams verfügen über Sichtbarkeit oder Zugriff für die von diesen Anwendungskomponenten generierte Telemetrie auf Codeebene. 

* Komponenten unterscheiden sich von „beobachteten“ externen Abhängigkeiten, wie SQL, EventHub usw., auf die Ihr Team/Ihre Organisation möglicherweise keinen Zugriff hat (Code oder Telemetrie).
* Komponenten können in einer beliebigen Anzahl von Server-/Rollen-/Containerinstanzen ausgeführt werden.
* Komponenten können separate Application Insights-Instrumentierungsschlüssel (sogar in verschiedenen Abonnements) oder verschiedene Rollen aufweisen, die alle an einen einzelnen Application Insights-Instrumentierungsschlüssel berichten. In der Vorschau werden die Komponenten unabhängig von der Art ihrer Einrichtung in der Übersicht angezeigt.

## <a name="composite-application-map"></a>Verbundanwendungsübersicht

Sie können die vollständige Anwendungstopologie über mehrere Ebenen verwandter Anwendungskomponenten hinweg anzeigen. Bei Komponenten kann es sich um verschiedene Application Insights-Ressourcen oder verschiedene Rollen in einer einzelnen Ressource handeln. Die App-Übersicht ermittelt Komponenten, indem sie HTTP-Abhängigkeitsaufrufe zwischen Servern verfolgt, auf denen das Application Insights SDK installiert ist. 

Diese Funktionalität beginnt mit einer progressiven Ermittlung der Komponenten. Wenn Sie die Anwendungsübersicht zum ersten Mal laden, wird eine Reihe von Abfragen ausgelöst, um die Komponenten zu ermitteln, die mit dieser Komponente in Zusammenhang stehen. Mit einer Schaltfläche in der oberen linken Ecke wird die Anzahl der Komponenten in Ihrer Anwendung aktualisiert, wenn diese ermittelt werden. 

Durch Klicken auf „Übersichtskomponenten aktualisieren“ wird die Übersicht mit allen bis zu diesem Zeitpunkt ermittelten Komponenten aktualisiert.

Wenn es sich bei allen Komponenten um Rollen in einer einzelnen Application Insights-Ressource handelt, ist dieser Ermittlungsschritt nicht erforderlich. Nach dem ersten Ladevorgang für eine solche Anwendung sind alle Komponenten vorhanden.

![Screenshot: Anwendungsübersicht](media/app-insights-app-map/001.png)

Eines der wichtigsten Ziele dieser Oberfläche ist es, komplexe Topologien mit Hunderten von Komponenten zu visualisieren.

Klicken Sie auf eine beliebige Komponente, um die zugehörigen Details anzuzeigen und zur Selektierung von Leistungs- und Fehleraspekten für diese Komponente zu wechseln.

![Flyout](media/app-insights-app-map/application-map-001.png)

### <a name="investigate-failures"></a>Untersuchen von Fehlern

Wählen Sie **Fehler untersuchen**, um den Bereich „Fehler“ zu starten.

![Screenshot: Schaltfläche „Fehler untersuchen“](media/app-insights-app-map/investigate-failures.png)

![Screenshot: Bereich „Fehler“](media/app-insights-app-map/failures.png)

### <a name="investigate-performance"></a>Untersuchen der Leistung

Wählen Sie **Leistung untersuchen**, um Probleme mit der Leistung zu behandeln.

![Screenshot: Schaltfläche „Leistung untersuchen“](media/app-insights-app-map/investigate-performance.png)

![Screenshot: Benutzeroberfläche für Leistung](media/app-insights-app-map/performance.png)

### <a name="go-to-details"></a>Zu Details wechseln

Wählen Sie **Zu Details wechseln**, um den Bereich für End-to-End-Transaktionen zu erkunden, in dem Informationen zur Aufrufliste angezeigt werden.

![Screenshot: Schaltfläche „Zu Details wechseln“](media/app-insights-app-map/go-to-details.png)

![Screenshot: Details von End-to-End-Transaktionen](media/app-insights-app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>In Analytics anzeigen

Klicken Sie auf **In Analytics anzeigen**, um Ihre Anwendungsdaten abzufragen und eingehender zu untersuchen.

![Screenshot: Schaltfläche „In Analytics anzeigen“](media/app-insights-app-map/view-in-analytics.png)

![Screenshot: Analytics-Bereich](media/app-insights-app-map/analytics.png)

### <a name="alerts"></a>Warnungen

Wählen Sie **Warnungen**, um aktive Warnungen und die zugrunde liegenden Regeln anzuzeigen, die zur Ausführung der Warnungen führen.

![Screenshot: Schaltfläche „Warnungen“](media/app-insights-app-map/alerts.png)

![Screenshot: Analytics-Bereich](media/app-insights-app-map/alerts-view.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Feedback
Senden Sie über die Feedbackoption im Portal Feedback.

![MapLink-1 (Abbildung)](./media/app-insights-app-map/13.png)

## <a name="next-steps"></a>Nächste Schritte

* [Azure-Portal](https://portal.azure.com)

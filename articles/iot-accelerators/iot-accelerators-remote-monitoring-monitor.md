---
title: Erweiterte Überwachung in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird erläutert, wie Sie Geräte über das Dashboard der Remoteüberwachungslösung überwachen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 02/22/2018
ms.topic: conceptual
ms.openlocfilehash: 4d2dabd348d7fda4fa7ca3aac9975fd4179400c5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627398"
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Ausführen der erweiterten Überwachung mithilfe der Remoteüberwachungslösung

In diesem Tutorial werden die Funktionen des Dashboards für die Remoteüberwachung erläutert. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

In diesem Tutorial wird anhand zweier simulierter Contoso-LKW-Geräte veranschaulicht, wie Sie Geräte über das Dashboard des Solution Accelerators überwachen. Als Contoso-Bediener müssen Sie den Standort und das Verhalten der Trucks im Feld überwachen.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Benachrichtigungen Ihrer Geräte
> * Anzeigen der System-KPIs

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, absolvieren Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy.md).

## <a name="choose-the-devices-to-display"></a>Auswählen der anzuzeigenden Geräte

Verwenden Sie Filter, um festzulegen, welche Geräte auf der Seite **Dashboard** angezeigt werden. Um nur die **Truck**-Geräte anzuzeigen, wählen Sie in der Dropdownliste zum Filtern den integrierten Filter **Trucks** aus:

![Filtern nach Trucks im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter.png)

Wenn Sie einen Filter anwenden, werden nur die Geräte, die den Filterbedingungen entsprechen, auf der Karte auf der Seite **Dashboard** angezeigt:

![Anzeige der Trucks auf der Karte](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap.png)

Durch den Filter wird zudem vorgegeben, welche Geräte im Diagramm **Telemetry** (Telemetrie) angezeigt werden:

![Angezeigte Truck-Telemetrie im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry.png)

Wählen Sie **Manage filters** (Filter verwalten) aus, um Filter zu erstellen, zu bearbeiten und zu löschen.

## <a name="view-real-time-telemetry"></a>Anzeigen von Echtzeit-Telemetriedaten

Der Solution Accelerator stellt im Diagramm auf der Seite **Dashboard** detaillierte Echtzeit-Telemetriedaten dar. Im Telemetriediagramm werden Telemetrieinformationen für die Geräte angezeigt, die durch den aktuellen Filter ausgewählt wurden:

![Truck-Telemetriediagramm](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview.png)

Um die anzuzeigenden Telemetriewerte festzulegen, wählen Sie oben im Diagramm den Telemetrietyp aus:

![Truck-Telemetriediagramm](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry.png)

<!-- 05/01 - this features appears to have been removed
To pause the live telemetry display, choose **Flowing**. To re-enable the live display, choose **Pause**:

![Pause and restart telemetry display](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetrypause.png)-->

## <a name="use-the-map"></a>Verwenden der Karte

Auf der Karte werden Informationen zu den simulierten Trucks angezeigt, die durch den aktuellen Filter ausgewählt sind. Sie können die Karte zoomen und schwenken, um Positionen mehr oder weniger detailliert anzuzeigen. Die Gerätesymbole auf der Karte geben alle für das Gerät aktiven **Benachrichtigungen** oder **Warnungen** an. Eine Zusammenfassung der Anzahl von **Benachrichtigungen** und **Warnungen** wird auf der linken Seite der Karte angezeigt.

<!-- 05/01 - cannot select a deice on the map
To view the device details, pan and zoom the map to locate the devices, then click the device on the map. The details include:

* Recent telemetry values
* Methods the device supports
* Device properties

![View device details on the dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail.png)-->

## <a name="view-alerts-from-your-devices"></a>Anzeigen von Benachrichtigungen Ihrer Geräte

Auf der Karte werden die Geräte im aktuellen Filter mit **Benachrichtigungen** und **Warnungen** hervorgehoben. Im Bereich **Benachrichtigungen** werden detaillierte Informationen zu den neuesten Benachrichtigungen von den Geräten angezeigt:

![Anzeigen von Systembenachrichtigungen im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms.png)

Mit dem Filter **Dashboard** können Sie die Zeitspanne für aktuelle Benachrichtigungen anpassen. Standardmäßig werden Benachrichtigungen der letzten Stunde angezeigt:

![Filtern der Benachrichtigungen nach Zeit](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>Anzeigen der System-KPIs

Auf der Seite **Dashboard** werden System-KPIs angezeigt:

![Dashboard-KPIs](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis.png)

Mit dem Filter **Dashboard** können Sie die Zeitspanne für die KPI-Aggregation anpassen. Standardmäßig werden die während der letzten Stunde aggregierten KPIs im Bereich angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde erläutert, wie Sie auf der Seite **Dashboard** die simulierten Trucks, die in Ihrer Remoteüberwachungslösung bereitgestellt werden, filtern und überwachen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Benachrichtigungen Ihrer Geräte
> * Anzeigen der System-KPIs

Sie wissen nun, wie Sie Ihre Geräte überwachen können. In den nächsten empfohlenen Schritten erfahren Sie Folgendes:

* [Erkennen von Problemen mithilfe schwellenwertbasierter Regeln](iot-accelerators-remote-monitoring-automate.md)
* [Verwalten und Konfigurieren von Geräten](iot-accelerators-remote-monitoring-manage.md)
* [Beheben von Geräteproblemen](iot-accelerators-remote-monitoring-maintain.md)
* [Testen der Lösung mit simulierten Geräten](iot-accelerators-remote-monitoring-test.md)

<!-- Next tutorials in the sequence -->
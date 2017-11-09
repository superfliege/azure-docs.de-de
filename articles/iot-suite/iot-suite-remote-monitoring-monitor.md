---
title: "Erweiterte Überwachung in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial wird erläutert, wie Sie Geräte über das Dashboard der Remoteüberwachungslösung überwachen."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/28/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: d523902505e38ef3fccf60e7648ea5333daae4f7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Ausführen der erweiterten Überwachung mithilfe der Remoteüberwachungslösung

In diesem Tutorial werden die Funktionen des Dashboards für die Remoteüberwachung erläutert. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

In diesem Tutorial wird anhand zweier simulierter Contoso Truck-Geräte veranschaulicht, wie Sie Geräte über das Dashboard der vorkonfigurierten Lösung überwachen. Als Contoso-Bediener müssen Sie den Standort und das Verhalten der Trucks im Feld überwachen.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Alarmen von den Geräten
> * Anzeigen der System-KPIs

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

## <a name="choose-the-devices-to-display"></a>Auswählen der anzuzeigenden Geräte

Verwenden Sie Filter, um festzulegen, welche Geräte auf der Seite **Dashboard** angezeigt werden. Um nur die **Truck**-Geräte anzuzeigen, wählen Sie in der Dropdownliste zum Filtern den integrierten Filter **Trucks** aus:

![Filtern nach Trucks im Dashboard](media/iot-suite-remote-monitoring-monitor/dashboardtruckfilter.png)

Wenn Sie einen Filter anwenden, werden nur die Geräte, die den Filterbedingungen entsprechen, auf der Karte auf der Seite **Dashboard** angezeigt:

![Anzeige der Trucks auf der Karte](media/iot-suite-remote-monitoring-monitor/dashboardtruckmap.png)

Durch den Filter wird zudem vorgegeben, welche Geräte im Diagramm **Telemetry** (Telemetrie) angezeigt werden:

![Angezeigte Truck-Telemetrie im Dashboard](media/iot-suite-remote-monitoring-monitor/dashboardtelemetry.png)

Wählen Sie **Manage filters** (Filter verwalten) aus, um Filter zu erstellen, zu bearbeiten und zu löschen.

## <a name="view-real-time-telemetry"></a>Anzeigen von Echtzeit-Telemetriedaten

Die vorkonfigurierte Lösung stellt detaillierte Echtzeit-Telemetriedaten im Diagramm auf der Seite **Dashboard** dar. Im Telemetriediagramm werden Telemetrieinformationen für die Geräte angezeigt, die durch den aktuellen Filter ausgewählt wurden:

![Truck-Telemetriediagramm](media/iot-suite-remote-monitoring-monitor/dashboardtelemetryview.png)

Um die anzuzeigenden Telemetriewerte festzulegen, wählen Sie oben im Diagramm den Telemetrietyp aus:

![Truck-Telemetriediagramm](media/iot-suite-remote-monitoring-monitor/dashboardselecttelemetry.png)

Um die Anzeige der Livetelemetrie anzuhalten, wählen Sie **Flowing** (Übertragung) aus. Um die Liveanzeige wieder zu aktivieren, wählen Sie **Pause** aus:

![Anhalten und Neustarten der Telemetrieanzeige](media/iot-suite-remote-monitoring-monitor/dashboardtelemetrypause.png)

## <a name="use-the-map"></a>Verwenden der Karte

Auf der Karte werden Informationen zu den simulierten Trucks angezeigt, die durch den aktuellen Filter ausgewählt sind. Sie können die Karte zoomen und schwenken, um Positionen mehr oder weniger detailliert anzuzeigen. Die Gerätesymbole auf der Karte geben alle für das Gerät aktiven **Alarme** oder **Warnungen** an. Eine Zusammenfassung der Anzahl von **Alarmen** und **Warnungen** wird auf der linken Seite der Karte angezeigt.

Zum Anzeigen der Gerätedetails können Sie die Karte zoomen und schwenken, um die Geräte zu lokalisieren. Klicken Sie dann auf der Karte auf das Gerät. Die Details umfassen Folgendes:

* Aktuelle Telemetriewerte
* Vom Gerät unterstützte Methoden
* Geräteeigenschaften

![Anzeigen von Gerätedetails im Dashboard](media/iot-suite-remote-monitoring-monitor/dashboarddevicedetail.png)

## <a name="view-alarms-from-your-devices"></a>Anzeigen von Alarmen von den Geräten

Auf der Karte werden die Geräte im aktuellen Filter mit **Alarmen** und **Warnungen** hervorgehoben. Im Bereich **System alarms** (Systemalarme) werden detaillierte Informationen zu den neuesten Alarmen von den Geräten angezeigt:

![Anzeigen von Systemalarmen im Dashboard](media/iot-suite-remote-monitoring-monitor/dashboardsystemalarms.png)

Mit dem Filter **System alarms** (Systemalarme) können Sie die Zeitspanne für aktuelle Alarme anpassen. Standardmäßig werden Alarme der letzten Stunde im Bereich angezeigt:

![Filtern der Alarme nach Zeitspanne](media/iot-suite-remote-monitoring-monitor/dashboardalarmsfilter.png)

> [!NOTE]
> Scrollen Sie nach rechts, um den Zeitspannenfilter anzuzeigen.

## <a name="view-the-system-kpis"></a>Anzeigen der System-KPIs

Auf der Seite **Dashboard** werden System-KPIs angezeigt:

![Filtern der Alarme nach Zeitspanne](media/iot-suite-remote-monitoring-monitor/dashboardkpis.png)

Mit dem Filter **System KPIs** können Sie die Zeitspanne für die KPI-Aggregation anpassen. Standardmäßig werden die während der letzten Stunde aggregierten KPIs im Bereich angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde erläutert, wie Sie auf der Seite **Dashboard** die simulierten Trucks, die in Ihrer Remoteüberwachungslösung bereitgestellt werden, filtern und überwachen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Alarmen von den Geräten
> * Anzeigen der System-KPIs

Sie wissen nun, wie Sie Ihre Geräte überwachen können. In den nächsten empfohlenen Schritten erfahren Sie Folgendes:

* [Erkennen von Problemen mithilfe schwellenwertbasierter Regeln](./iot-suite-remote-monitoring-automate.md)
* [Verwalten und Konfigurieren von Geräten](./iot-suite-remote-monitoring-manage.md)
* [Beheben von Geräteproblemen](./iot-suite-remote-monitoring-maintain.md)
* [Testen der Lösung mit simulierten Geräten](iot-suite-remote-monitoring-test.md)

<!-- Next tutorials in the sequence -->
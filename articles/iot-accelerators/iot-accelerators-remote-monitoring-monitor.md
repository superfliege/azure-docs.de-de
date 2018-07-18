---
title: Überwachen Ihrer IoT-Geräte über eine Azure-Lösung | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Ihre IoT-Geräte mit dem Solution Accelerator für die Remoteüberwachung überwachen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5f42ed0fa5362959e5619f2d550ca1ae3711ed65
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097460"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Tutorial: Überwachen Ihrer IoT-Geräte

In diesem Tutorial verwenden Sie den Solution Accelerator für die Remoteüberwachung, um Ihre verbundenen IoT-Geräte zu überwachen. Sie nutzen das Lösungsdashboard, um Telemetriedaten, Geräteinformationen, Warnungen und KPIs anzuzeigen.

Zur Einführung in diese Überwachungsfunktionen werden im Tutorial zwei simulierte Geräte vom Typ „LKW“ verwendet. Die LKW werden von der Organisation Contoso verwaltet und sind mit dem Solution Accelerator für die Remoteüberwachung verbunden. Als Contoso-Bediener müssen Sie den Standort und das Verhalten der Trucks im Feld überwachen.

In diesem Tutorial haben Sie Folgendes durchgeführt:

>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Benachrichtigungen Ihrer Geräte
> * Anzeigen der System-KPIs

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz des Solution Accelerators für die Remoteüberwachung in Ihrem Azure-Abonnement.

Falls Sie den Solution Accelerator für die Remoteüberwachung noch nicht bereitgestellt haben, sollten Sie die Schnellstartanleitung [Bereitstellen einer cloudbasierten Lösung für die Remoteüberwachung](quickstart-remote-monitoring-deploy.md) durcharbeiten.

## <a name="choose-the-devices-to-display"></a>Auswählen der anzuzeigenden Geräte

Verwenden Sie Filter, um festzulegen, welche verbundenen Geräte auf der Seite **Dashboard** angezeigt werden. Um nur die **Truck**-Geräte anzuzeigen, wählen Sie in der Dropdownliste zum Filtern den integrierten Filter **Trucks** aus:

[![Filtern nach LKW im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Wenn Sie einen Filter anwenden, werden nur die Geräte, die die Filterbedingungen erfüllen, auf der Karte der Seite **Dashboard** angezeigt:

[![Nur LKW werden auf der Karte angezeigt](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

Durch den Filter wird zudem vorgegeben, welche Geräte im Diagramm **Telemetry** (Telemetrie) angezeigt werden:

[![LKW-Telemetriedaten werden im Dashboard angezeigt](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-expanded.png#lightbox)

Wählen Sie **Manage device groups** (Gerätegruppen verwalten), um Filter zu erstellen, zu bearbeiten und zu löschen.

## <a name="view-real-time-telemetry"></a>Anzeigen von Echtzeit-Telemetriedaten

Der Solution Accelerator stellt im Diagramm auf der Seite **Dashboard** Echtzeit-Telemetriedaten dar. Oben im Diagramm mit den Telemetriedaten werden die verfügbaren Telemetriedatentypen für die Geräte angezeigt, die mit dem aktuellen Filter ausgewählt wurden:

[![Typen von LKW-Telemetriedaten](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Klicken Sie auf **Temperature** (Temperatur), um die Telemetriedaten für die Temperatur anzuzeigen:

[![Darstellung der Telemetriedaten für die LKW-Temperatur](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="use-the-map"></a>Verwenden der Karte

Auf der Karte werden Informationen zu den simulierten Trucks angezeigt, die durch den aktuellen Filter ausgewählt sind. Sie können die Karte zoomen und schwenken, um Positionen mehr oder weniger detailliert anzuzeigen. Mit der Farbe eines Gerätesymbols auf der Karte wird angegeben, ob für das Gerät **Alerts** (Benachrichtigungen) oder **Warnings** (Warnungen) vorliegen. Eine Zusammenfassung der Anzahl von **Benachrichtigungen** und **Warnungen** wird auf der linken Seite der Karte angezeigt.

Zum Anzeigen der Gerätedetails können Sie die Karte zoomen und schwenken, um die Geräte zu lokalisieren. Anschließend können Sie das Gerät in der Karte auswählen. Klicken Sie dann auf die Gerätebezeichnung, um das Panel **Device details** (Gerätedetails) zu öffnen. Zu den Gerätedetails zählt Folgendes:

* Aktuelle Telemetriewerte
* Vom Gerät unterstützte Methoden
* Geräteeigenschaften

[![Anzeigen von Gerätedetails im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Anzeigen von Warnungen

Im Bereich **Benachrichtigungen** werden detaillierte Informationen zu den neuesten Benachrichtigungen von den Geräten angezeigt:

[![Anzeigen von Gerätebenachrichtigungen im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Mit einem Filter können Sie die Zeitspanne für aktuelle Benachrichtigungen anpassen. Standardmäßig werden Benachrichtigungen der letzten Stunde angezeigt:

[![Filtern der Benachrichtigungen nach Zeit](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-expanded.png#lightbox)

## <a name="view-the-system-kpis"></a>Anzeigen der System-KPIs

Auf der Seite **Dashboard** werden vom Solution Accelerator berechnete System-KPIs im Panel **Analytics** (Analyse) angezeigt:

[![Dashboard-KPIs](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

Im Dashboard werden drei KPIs für die Benachrichtigungen angezeigt, die mit den aktuellen Filtern für Geräte und die Zeitspanne ausgewählt wurden:

* Die Anzahl von aktiven Benachrichtigungen für die Regeln, die die meisten Benachrichtigungen ausgelöst haben.
* Der Anteil von Benachrichtigungen nach dem Gerätetyp.
* Der Prozentsatz an Benachrichtigungen, die kritisch sind.

Die gleichen Filter, mit denen die Zeitspanne für Benachrichtigungen festgelegt und mit denen gesteuert wird, welche Geräte angezeigt werden, bestimmen die Art der Aggregierung von KPIs. Standardmäßig werden die während der letzten Stunde aggregierten KPIs im Bereich angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren möchten, können Sie die Bereitstellung des Solution Accelerators für die Remoteüberwachung beibehalten. Zum Reduzieren der Kosten für die Ausführung des Solution Accelerators, wenn Sie ihn nicht nutzen, können Sie die simulierten Geräte im Panel mit den Einstellungen beenden:

[![Anhalten der Telemetrie](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-expanded.png#lightbox)

Sie können die simulierten Geräte neu starten, wenn Sie mit dem nächsten Tutorial beginnen möchten.

Falls Sie den Solution Accelerator nicht mehr benötigen, können Sie ihn auf der Seite [Bereitgestellte Lösungen](https://www.azureiotsolutions.com/Accelerators#dashboard) löschen:

![Löschen der Lösung](media/iot-accelerators-remote-monitoring-monitor/deletesolution.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde veranschaulicht, wie Sie die Seite **Dashboard** im Solution Accelerator für die Remoteüberwachung verwenden, um die simulierten LKW zu filtern und zu überwachen. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Solution Accelerator zum Erkennen von Problemen für Ihre verbundenen Geräte verwenden.

> [!div class="nextstepaction"]
> [Erkennen von Problemen mit Geräten, die mit der Überwachungslösung verbunden sind](iot-accelerators-remote-monitoring-automate.md)
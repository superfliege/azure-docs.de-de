---
title: "Beheben von Problemen mit Geräten in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial wird erläutert, wie Sie Geräteprobleme in der Remoteüberwachungslösung beheben."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/16/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: ac284126c2c940caf97fb95bd46234de49b1f678
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Beheben von Geräteproblemen

In diesem Tutorial wird erläutert, wie Sie auf der Seite **Maintenance** (Wartung) in der Lösung Geräteprobleme beheben. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso testet ein neues Gerät des Typs **Prototype** im Feld. Als Contoso-Bediener bemerken Sie während der Tests, dass das **Prototype**-Gerät unerwartet einen Temperaturalarm im Dashboard auslöst. Sie müssen nun das Verhalten dieses fehlerhaften **Prototype**-Geräts untersuchen.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Untersuchen des Alarms auf der Seite **Maintenance** (Wartung)
> * Aufrufen einer Gerätemethode zum Beheben des Problems

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

## <a name="use-the-maintenance-dashboard"></a>Verwenden des Dashboards für die Wartung

Auf der Seite **Dashboard** stellen Sie fest, dass unerwartete Temperaturalarme von der mit den **Prototype**-Geräten verknüpften Regel ausgegeben wurden:

![Im Dashboard angezeigte Alarme](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Wählen Sie die Option **Explore Alarm** (Alarm untersuchen) neben dem Alarm aus, um das Problem weiter zu untersuchen:

![„Explore Alarm“ (Alarm untersuchen) im Dashboard](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

Nun können Sie auf der Seite **Maintenance** (Wartung) eine Liste der Alarme sehen:

![Liste der Alarme auf der Seite „Maintenance“ (Wartung)](media/iot-suite-remote-monitoring-maintain/maintenancealarms.png)

Wählen Sie den Alarm in der Liste **Alarms** (Alarme) aus, um Details zum Alarm anzuzeigen. In der Detailansicht wird Folgendes angezeigt:

* Zeitpunkt, zu dem der Alarm ausgelöst wurde
* Statusinformationen zu den Geräten, die dem Alarm zugeordnet sind
* Telemetriedaten von den Geräten, die dem Alarm zugeordnet sind

![Alarmdetails](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Um den Alarm zu bestätigen, wählen Sie die **Alarm occurrences** (Alarmvorkommen) und dann **Acknowledge** (Bestätigen) aus. Dadurch können andere Bediener sehen, dass Sie den Alarm gesehen haben und daran arbeiten.

![Bestätigen der Alarme](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

In der Liste können Sie das **Prototype**-Gerät sehen, für das der Temperaturalarm ausgelöst wurde:

![Liste der Geräte, die den Alarm verursachen](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Beheben des Problems

Um das Problem bei dem **Prototype**-Gerät zu beheben, müssen Sie die **DecreaseTemperature**-Methode auf dem Gerät aufrufen.

Um Aktionen für ein Gerät auszuführen, wählen Sie es in der Geräteliste aus, und wählen Sie dann **Schedule** (Zeitplan) aus. Das Gerätemodell **Engine** gibt drei Methoden an, die ein Gerät unterstützen muss:

![Anzeigen der vom Gerät unterstützten Methoden](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Wählen Sie **DecreaseTemperature** aus, und legen Sie den Auftragsnamen auf **DecreaseTemperature** fest. Wählen Sie dann **Apply** (Übernehmen) aus:

![Erstellen des Auftrags zum Verringern der Temperatur](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **System Status** aus. In der Ansicht **System Status** können Sie alle Aufträge und Methodenaufrufe in der Lösung anzeigen:

![Überwachen des Auftrags zum Verringern der Temperatur](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Zum Anzeigen der Details eines bestimmten Auftrags oder Methodenaufrufs wählen Sie ihn in der Liste in der Ansicht **System Status** aus:

![Anzeigen von Auftragsdetails](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Untersuchen des Alarms auf der Seite **Maintenance** (Wartung)
> * Aufrufen einer Gerätemethode zum Beheben des Problems

Nun wissen Sie, wie Geräteprobleme verwaltet werden. Im nächsten empfohlenen Schritt erfahren Sie, wie Sie [Ihre Lösung mit simulierten Geräten testen](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->
---
title: Beheben von Problemen mit Geräten in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird erläutert, wie Sie Geräteprobleme in der Remoteüberwachungslösung beheben.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: 9a620d91238393ba0bde89f521f790b58ab35baf
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628071"
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Beheben von Geräteproblemen

In diesem Tutorial wird erläutert, wie Sie auf der Seite **Maintenance** (Wartung) in der Lösung Geräteprobleme beheben. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso testet ein neues Gerät des Typs **Prototype** im Feld. Als Contoso-Bediener bemerken Sie während der Tests, dass das **Prototype**-Gerät unerwartet eine Temperaturwarnung im Dashboard auslöst. Sie müssen nun das Verhalten dieses fehlerhaften **Prototype**-Geräts untersuchen.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Untersuchen des Alarms auf der Seite **Maintenance (Wartung)**
> * Aufrufen einer Gerätemethode zum Beheben des Problems

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Lösung für die Remoteüberwachung noch nicht bereitgestellt haben, sollten Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy.md) durcharbeiten.

## <a name="use-the-maintenance-dashboard"></a>Verwenden des Dashboards für die Wartung

Auf der Seite **Dashboard** stellen Sie fest, dass unerwartete Temperaturwarnungen von der mit den **Prototype**-Geräten verknüpften Regel ausgegeben wurden:

![Im Dashboard angezeigte Warnungen](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm.png)

Wählen Sie die Option **Explore Alert (Warnung untersuchen)** neben dem Alarm aus, um das Problem weiter zu untersuchen:

![„Explore Alert“ (Warnung untersuchen) im Dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm.png)

Die Detailansicht der Warnung zeigt Folgendes an:

* Zeitpunkt, zu dem die Warnung ausgelöst wurde
* Statusinformationen zu den Geräten, die der Warnung zugeordnet sind
* Telemetriedaten von den Geräten, die der Warnung zugeordnet sind

![Warnungsdetails](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail.png)

Um die Warnung zu bestätigen, wählen Sie die **Alert occurrences (Warnungsvorkommen)** und dann **Acknowledge (Bestätigen)** aus. So können andere Bediener sehen, dass Sie die Warnung gesehen haben und daran arbeiten.

![Bestätigen der Warnungen](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge.png)

Wenn Sie die Warnung bestätigen, ändert sich der Status des Vorkommens in **Bestätigt**.

In der Liste können Sie das **Prototype**-Gerät sehen, für das die Temperaturwarnung ausgelöst wurde:

![Liste der Geräte, die die Warnung verursachen](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Beheben des Problems

Um das Problem bei dem **Prototype**-Gerät zu beheben, müssen Sie die **DecreaseTemperature**-Methode auf dem Gerät aufrufen.

Um Aktionen für ein Gerät auszuführen, wählen Sie es in der Geräteliste aus, und wählen Sie dann **Jobs (Aufträge)** aus. Das Gerätemodell **Prototype** gibt sechs Methoden an, die ein Gerät unterstützen muss:

![Anzeigen der vom Gerät unterstützten Methoden](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods.png)

Wählen Sie **DecreaseTemperature** aus, und legen Sie den Auftragsnamen auf **DecreaseTemperature** fest. Wählen Sie dann **Apply** (Übernehmen) aus:

![Erstellen des Auftrags zum Verringern der Temperatur](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **Jobs** (Aufträge) aus. In der Ansicht **Jobs** (Aufträge) können Sie alle Aufträge und Methodenaufrufe in der Lösung anzeigen:

![Überwachen des Auftrags zum Verringern der Temperatur](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob.png)

Zum Anzeigen der Details eines bestimmten Auftrags oder Methodenaufrufs wählen Sie ihn in der Liste in der Ansicht **Jobs** (Aufträge) aus:

![Anzeigen von Auftragsdetails](./media/iot-accelerators-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Untersuchen des Alarms auf der Seite **Maintenance (Wartung)**
> * Aufrufen einer Gerätemethode zum Beheben des Problems

Nun wissen Sie, wie Geräteprobleme verwaltet werden. Im nächsten empfohlenen Schritt erfahren Sie, wie Sie [Ihre Lösung mit simulierten Geräten testen](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->
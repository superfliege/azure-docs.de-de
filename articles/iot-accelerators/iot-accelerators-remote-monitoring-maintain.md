---
title: Verwenden von Warnungen und Beheben von Geräteproblemen in der Lösung für die Remoteüberwachung | Microsoft-Dokumentation
description: In diesem Tutorial wird veranschaulicht, wie Sie Warnungen zum Identifizieren und Beheben von Problemen mit Geräten verwenden können, die mit dem Solution Accelerator für die Remoteüberwachung verbunden sind.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/18/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 9607705220450b30d2ffaf0f2be9fa2a5664b879
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081787"
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Beheben von Geräteproblemen

In diesem Tutorial verwenden Sie den Solution Accelerator für die Remoteüberwachung zum Identifizieren und Beheben von Problemen mit Ihren verbundenen IoT-Geräten. Verwenden Sie Warnungen im Dashboard des Solution Accelerators, um Probleme zu identifizieren, führen Sie dann Remoteaufträge aus, um diese Probleme zu beheben.

In diesem Beispiel führt Contoso Praxistests an dem **Prototyp** eines Geräts durch. Als Contoso-Bediener bemerken Sie während der Tests, dass das **Prototyp**-Gerät unerwartet eine Temperaturwarnung im Dashboard auslöst. Sie müssen nun das Verhalten dieses fehlerhaften **Prototyp**-Geräts untersuchen und das Problem lösen.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Untersuchen einer Warnung von einem Gerät
> * Beheben des Problems mit dem Gerät

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz des Solution Accelerators für die Remoteüberwachung in Ihrem Azure-Abonnement.

Falls Sie den Solution Accelerator für die Remoteüberwachung noch nicht bereitgestellt haben, sollten Sie den Schnellstart [Bereitstellen einer cloudbasierten Lösung für die Remoteüberwachung](quickstart-remote-monitoring-deploy.md) durcharbeiten.

## <a name="investigate-an-alert"></a>Untersuchen einer Warnung

Auf der Seite **Dashboard** stellen Sie fest, dass unerwartete Temperaturwarnungen von der mit den **Prototyp**-Geräten verknüpften Regel ausgegeben wurden:

[![Im Dashboard angezeigte Warnungen](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-expanded.png#lightbox)

Wählen Sie die Option **Explore Alert** (Warnung untersuchen) neben dem Alarm aus, um das Problem weiter zu untersuchen:

[![„Explore Alert“ (Warnung untersuchen) im Dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-expanded.png#lightbox)

Die Detailansicht der Warnung zeigt Folgendes an:

* Zeitpunkt, zu dem die Warnung ausgelöst wurde
* Statusinformationen zu den Geräten, die der Warnung zugeordnet sind
* Telemetriedaten von den Geräten, die der Warnung zugeordnet sind

[![Warnungsdetails](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-expanded.png#lightbox)

Wählen Sie alle **Alert occurrences** (Warnungsvorkommen) aus, und klicken Sie auf **Acknowledge** (Bestätigen). So können andere Bediener sehen, dass Sie die Warnung gesehen haben und daran arbeiten:

[![Bestätigen der Warnungen](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-expanded.png#lightbox)

Wenn Sie die Warnung bestätigen, ändert sich der Status des Vorkommens in **Acknowledged** (Bestätigt).

In der Liste können Sie das **Prototyp**-Gerät sehen, für das die Temperaturwarnung ausgelöst wurde:

[![Liste der Geräte, die die Warnung verursachen](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-expanded.png#lightbox)

## <a name="resolve-the-issue"></a>Beheben des Problems

Um das Problem bei dem **Prototyp**-Gerät zu beheben, müssen Sie die **DecreaseTemperature**-Methode auf dem Gerät aufrufen.

Um Aktionen für ein Gerät auszuführen, wählen Sie es in der Geräteliste aus, und wählen Sie dann **Jobs** (Aufträge) aus. Das Gerätemodell **Prototyp** gibt sechs Methoden an, die ein Gerät unterstützen muss:

[![Anzeigen der vom Gerät unterstützten Methoden](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-expanded.png#lightbox)

Wählen Sie **DecreaseTemperature** aus, und legen Sie den Auftragsnamen auf **DecreaseTemperature** fest. Wählen Sie dann **Apply** (Übernehmen) aus:

[![Erstellen des Auftrags zum Verringern der Temperatur](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-expanded.png#lightbox)

Klicken Sie auf **View job status** (Auftragsstatus anzeigen), um den Status des Auftrags nachzuverfolgen. In der Ansicht **Jobs** (Aufträge) können Sie alle Aufträge und Methodenaufrufe in der Lösung anzeigen:

[![Überwachen des Auftrags zum Verringern der Temperatur](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-expanded.png#lightbox)

Sie können überprüfen, ob sich die Temperatur des Geräts verringert hat, indem Sie die Telemetrie auf der Seite **Dashboard** anzeigen:

[![Anzeigen des Temperaturabstiegs](./media/iot-accelerators-remote-monitoring-maintain/jobresult-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/jobresult-expanded.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Warnungen zum Identifizieren von Problemen mit Ihren Geräten verwenden, und wie Sie auf diese Geräte zugreifen, um Probleme zu beheben. Fahren Sie mit den Artikeln zu den Vorgehensweisen fort, um zu erfahren, wie Sie ein physisches Gerät mit dem Solution Accelerator verbinden.

Sie wissen nun, wie Geräteprobleme verwaltet werden. Der nächste empfohlene Schritt ist, zu erfahren, wie Sie [ein Gerät mit dem Solution Accelerator für die Remoteüberwachung verbinden](iot-accelerators-connecting-devices.md).

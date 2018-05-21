---
title: Geräteverwaltung in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird veranschaulicht, wie Sie die mit der Remoteüberwachungslösung verbundenen Geräte verwalten.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3ad6de2a0ebcd257ca90ea3c5c69988d4c1afd7a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="manage-and-configure-your-devices"></a>Verwalten und Konfigurieren von Geräten

In diesem Tutorial werden die Geräteverwaltungsfunktionen der Remoteüberwachungslösung erläutert. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso hat neue Maschinen geordert, um eine seiner Betriebsstätten zu erweitern und die Produktion zu erhöhen. Während Sie auf die Lieferung der neuen Maschinen warten, möchten Sie eine Simulation ausführen, um das Verhalten der Lösung zu überprüfen. Als Bediener möchten Sie die Geräte in der Remoteüberwachungslösung verwalten und konfigurieren.

Um ein breites Spektrum an Möglichkeiten zum Verwalten und Konfigurieren von Geräten bereitzustellen, werden in der Remoteüberwachungslösung IoT Hub-Features wie z.B. [Aufträge](../iot-hub/iot-hub-devguide-jobs.md) und [direkte Methoden](../iot-hub/iot-hub-devguide-direct-methods.md) verwendet. Informationen dazu, wie ein Geräteentwickler Methoden auf einem physischen Gerät implementiert, finden Sie unter [Anpassen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-customize.md).

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Bereitstellen eines simulierten Geräts
> * Testen des simulierten Geräts
> * Aufrufen von Gerätemethoden aus der Lösung
> * Neukonfigurieren eines Geräts

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Sollten Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, absolvieren Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy.md).

## <a name="add-a-simulated-device"></a>Hinzufügen eines simulierten Geräts

Navigieren Sie in der Lösung zur Seite **Devices** (Geräte), und wählen Sie **+ New Device** (Neues Gerät) aus. Wählen Sie im Bereich **New Device** (Neues Gerät) die Option **Simulated** (Simuliert) aus:

![Bereitstellen eines simulierten Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesprovision.png)

Behalten Sie die auf **1** festgelegte Anzahl der bereitzustellenden Geräte bei. Wählen Sie das Gerätemodell **Faulty Engine** (Fehlerhafte Engine) und dann **Apply** (Übernehmen) aus, um das simulierte Gerät zu erstellen:

![Bereitstellen eines simulierten Engine-Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine.png)

Informationen zum Bereitstellen eines *physischen* Geräts finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Node.js)](iot-accelerators-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Testen des simulierten Geräts

Um Details zu dem neuen simulierten Gerät anzuzeigen, wählen Sie es in der Geräteliste auf der Seite **Devices** (Geräte) aus. Informationen zu dem Gerät werden im Bereich **Device detail** (Gerätedetails) angezeigt:

![Anzeigen des simulierten Engine-Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew.png)

Überprüfen Sie in **Device detail** (Gerätedetails), ob das neue Gerät Telemetriedaten sendet. Um die verschiedenen Telemetriedatenströme von Ihrem Gerät anzuzeigen, wählen Sie einen Telemetrienamen wie z.B. **vibration** aus:

![Auswählen eines anzuzeigenden Telemetriedatenstroms](./media/iot-accelerators-remote-monitoring-manage/devicesvibration.png)

Im Bereich **Device detail** (Gerätedetails) werden weitere Informationen zum Gerät angezeigt, z.B. Tagwerte, die unterstützten Methoden und die vom Gerät gemeldeten Eigenschaften.

Scrollen Sie zum Anzeigen von detaillierten Diagnosedaten nach unten, um **Diagnostics** (Diagnose) anzuzeigen.

## <a name="act-on-a-device"></a>Aktionen für ein Gerät

Um Aktionen für ein oder mehrere Geräte auszuführen, wählen Sie sie in der Geräteliste aus, und wählen Sie dann **Jobs** (Aufträge) aus. Das Gerätemodell **Engine** gibt drei Methoden an, die ein Gerät unterstützen muss:

![Engine-Methoden](./media/iot-accelerators-remote-monitoring-manage/devicesmethods.png)

Wählen Sie **FillTank**, legen Sie für den Auftragsnamen **FillEngineTank** fest, und wählen Sie dann **Apply (Übernehmen)**:

![Planen der Neustartmethode](./media/iot-accelerators-remote-monitoring-manage/devicesrestartengine.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **Jobs** (Aufträge) aus.

![Überwachen des geplanten Auftrags](./media/iot-accelerators-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Methoden in anderen Geräten

Wenn Sie sich die verschiedenen simulierten Gerätetypen ansehen, werden Sie feststellen, dass andere Gerätetypen andere Methoden unterstützen. In einer Bereitstellung mit physischen Geräten gibt das Gerätemodell die Methoden an, die das Gerät unterstützen muss. In der Regel ist der Geräteentwickler für die Entwicklung des Codes zuständig, über den das Gerät auf einen Methodenaufruf reagiert.

Um eine Methode für die Ausführung auf mehreren Geräten zu planen, können Sie mehrere Geräte in der Liste auf der Seite **Devices** (Geräte) auswählen. Im Bereich **Jobs** (Aufträge) werden die für alle ausgewählten Geräte gemeinsamen Methodentypen angezeigt.

## <a name="reconfigure-a-device"></a>Neukonfigurieren eines Geräts

Um die Konfiguration eines Geräts zu ändern, wählen Sie es in der Geräteliste auf der Seite **Devices** (Geräte) aus, wählen Sie dann **Jobs** (Aufträge) und dann **Reconfigure** (Neu konfigurieren) aus. Im Bereich „Jobs“ (Aufträge) werden die Eigenschaftswerte für das ausgewählte Gerät angezeigt, die Sie ändern können:

![Neukonfigurieren eines Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure.png)

Um eine Änderung vorzunehmen, fügen Sie einen Namen für den Auftrag hinzu, aktualisieren Sie die Eigenschaftswerte, und wählen Sie **Apply** (Übernehmen) aus:

![Aktualisieren des Eigenschaftswerts eines Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **Jobs** (Aufträge) aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bereitstellen eines simulierten Geräts
> * Testen des simulierten Geräts
> * Aufrufen von Gerätemethoden aus der Lösung
> * Neukonfigurieren eines Geräts

Sie wissen nun, wie Sie Ihre Geräte verwalten können. In den nächsten empfohlenen Schritten erfahren Sie Folgendes:

* [Beheben von Geräteproblemen](iot-accelerators-remote-monitoring-maintain.md)
* [Testen der Lösung mit simulierten Geräten](iot-accelerators-remote-monitoring-test.md)
* [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Node.js)](iot-accelerators-connecting-devices-node.md)

<!-- Next tutorials in the sequence -->
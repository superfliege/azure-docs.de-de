---
title: "Geräteverwaltung in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial wird veranschaulicht, wie Sie die mit der Remoteüberwachungslösung verbundenen Geräte verwalten."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 84c2eaaab2dfc09c93fbfeac3fe2bfcc7066a411
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="manage-and-configure-your-devices"></a>Verwalten und Konfigurieren von Geräten

In diesem Tutorial werden die Geräteverwaltungsfunktionen der Remoteüberwachungslösung erläutert. Zur Einführung dieser Funktionen wird in diesem Tutorial ein Szenario in der Contoso IoT-Anwendung verwendet.

Contoso hat neue Maschinen geordert, um eine seiner Betriebsstätten zu erweitern und die Produktion zu erhöhen. Während Sie auf die Lieferung der neuen Maschinen warten, möchten Sie eine Simulation ausführen, um das Verhalten der Lösung zu überprüfen. Als Bediener möchten Sie die Geräte in der Remoteüberwachungslösung verwalten und konfigurieren.

Um ein breites Spektrum an Möglichkeiten zum Verwalten und Konfigurieren von Geräten bereitzustellen, werden in der Remoteüberwachungslösung IoT Hub-Features wie z.B. [Aufträge](../iot-hub/iot-hub-devguide-jobs.md) und [direkte Methoden](../iot-hub/iot-hub-devguide-direct-methods.md) verwendet. Informationen dazu, wie ein Entwickler Methoden auf einem physischen Gerät implementiert, finden Sie unter [Anpassen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-customize.md).

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Bereitstellen eines simulierten Geräts
> * Testen des simulierten Geräts
> * Aufrufen von Gerätemethoden aus der Lösung
> * Neukonfigurieren eines Geräts

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

## <a name="provision-a-simulated-device"></a>Bereitstellen eines simulierten Geräts

Navigieren Sie in der Lösung zur Seite **Devices** (Geräte), und wählen Sie **Provision** (Bereitstellen) aus. Wählen Sie im Bereich **Provision** (Bereitstellen) die Option **Simulated** (Simuliert) aus:

![Bereitstellen eines simulierten Geräts](media/iot-suite-remote-monitoring-manage/devicesprovision.png)

Behalten Sie die auf **1** festgelegte Anzahl der bereitzustellenden Geräte bei. Wählen Sie **Engine** als **Device model** (Gerätemodell) und dann **Apply** (Übernehmen) aus, um das simulierte Gerät zu erstellen:

![Bereitstellen eines simulierten Engine-Geräts](media/iot-suite-remote-monitoring-manage/devicesprovisionengine.png)

Informationen zum Bereitstellen eines *physischen* Geräts finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung](iot-suite-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Testen des simulierten Geräts

Um Details zu dem neuen simulierten Gerät anzuzeigen, wählen Sie es in der Geräteliste auf der Seite **Devices** (Geräte) aus. Informationen zu dem Gerät werden im Bereich **Device detail** (Gerätedetails) angezeigt:

![Anzeigen des simulierten Engine-Geräts](media/iot-suite-remote-monitoring-manage/devicesviewnew.png)

Überprüfen Sie in **Device detail** (Gerätedetails), ob das neue Gerät Telemetriedaten sendet. Um die verschiedenen Telemetriedatenströme von Ihrem Gerät anzuzeigen, wählen Sie einen Telemetrienamen wie z.B. **vibration** aus:

![Auswählen eines anzuzeigenden Telemetriedatenstroms](media/iot-suite-remote-monitoring-manage/devicesvibration.png)

Im Bereich **Device detail** (Gerätedetails) werden weitere Informationen zum Gerät angezeigt, z.B. Tagwerte, die unterstützten Methoden und die vom Gerät gemeldeten Eigenschaften.

Scrollen Sie zum Anzeigen von detaillierten Diagnosedaten nach unten, um **Diagnostics** (Diagnose) anzuzeigen.

## <a name="act-on-a-device"></a>Aktionen für ein Gerät

Um Aktionen für ein Gerät auszuführen, wählen Sie es in der Geräteliste aus, und wählen Sie dann **Schedule** (Zeitplan) aus. Das Gerätemodell **Engine** gibt vier Methoden an, die ein Gerät unterstützen muss:

![Engine-Methoden](media/iot-suite-remote-monitoring-manage/devicesmethods.png)

Wählen Sie **Restart** (Neustart) aus, legen Sie den Auftragsnamen auf **RestartEngine** fest, und wählen Sie dann **Apply** (Übernehmen) aus:

![Planen der Neustartmethode](media/iot-suite-remote-monitoring-manage/devicesrestartengine.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **System Status** aus:

![Überwachen des geplanten Auftrags](media/iot-suite-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Methoden in anderen Geräten

Wenn Sie sich die verschiedenen simulierten Gerätetypen ansehen, werden Sie feststellen, dass andere Gerätetypen andere Methoden unterstützen. In einer Bereitstellung mit physischen Geräten gibt das Gerätemodell die Methoden an, die das Gerät unterstützen muss. In der Regel ist der Geräteentwickler für die Entwicklung des Codes zuständig, über den das Gerät auf einen Methodenaufruf reagiert.

Um eine Methode für die Ausführung auf mehreren Geräten zu planen, können Sie mehrere Geräte in der Liste auf der Seite **Devices** (Geräte) auswählen. Im Bereich **Schedule** (Zeitplan) werden die für alle ausgewählten Geräte gemeinsamen Methodentypen angezeigt.

## <a name="reconfigure-a-device"></a>Neukonfigurieren eines Geräts

Um die Konfiguration eines Geräts zu ändern, wählen Sie es in der Geräteliste auf der Seite **Devices** (Geräte) aus, und wählen Sie dann **Reconfigure** (Neu konfigurieren) aus. Im Bereich „Reconfigure“ (Neu konfigurieren) werden die Eigenschaftswerte für das ausgewählte Gerät angezeigt, die Sie ändern können:

![Neukonfigurieren eines Geräts](media/iot-suite-remote-monitoring-manage/devicesreconfigure.png)

Um eine Änderung vorzunehmen, fügen Sie einen Namen für den Auftrag hinzu, aktualisieren Sie die Eigenschaftswerte, und wählen Sie **Apply** (Übernehmen) aus:

![Aktualisieren des Eigenschaftswerts eines Geräts](media/iot-suite-remote-monitoring-manage/devicesreconfigurephysical.png)

Um den Status des Auftrags auf der Seite **Maintenance** (Wartung) nachzuverfolgen, wählen Sie **System Status** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bereitstellen eines simulierten Geräts
> * Testen des simulierten Geräts
> * Aufrufen von Gerätemethoden aus der Lösung
> * Neukonfigurieren eines Geräts

Sie wissen nun, wie Sie Ihre Geräte verwalten können. In den nächsten empfohlenen Schritten erfahren Sie Folgendes:

* [Beheben von Geräteproblemen](iot-suite-remote-monitoring-maintain.md)
* [Testen der Lösung mit simulierten Geräten](iot-suite-remote-monitoring-test.md)
* [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung](iot-suite-connecting-devices-node.md)

<!-- Next tutorials in the sequence -->
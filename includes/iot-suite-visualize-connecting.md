---
title: Includedatei
description: Includedatei
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 5702c6e9c9d75c6cccb82f1c57684ef7b9898c34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666008"
---
## <a name="view-device-telemetry"></a>Anzeigen der Gerätetelemetrie

Sie können die von Ihrem Gerät gesendeten Telemetriedaten in der Lösung auf der Seite **Geräte** sehen.

1. Wählen Sie auf der Seite **Geräte** in der Liste der Geräte das von Ihnen bereitgestellte Gerät aus. In einem Bereich werden Informationen über Ihr Gerät angezeigt, einschließlich eines Diagramms der Gerätetelemetrie:

    ![Gerätedetails anzeigen](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Wählen Sie **Druck**, um die Anzeige der Telemetriedaten zu ändern:

    ![Telemetriedaten zum Druck anzeigen](media/iot-suite-visualize-connecting/devicespressure.png)

1. Um Diagnoseinformationen zu Ihrem Gerät anzuzeigen, scrollen Sie nach unten zu **Diagnose**:

    ![Gerätediagnose anzeigen](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Aktionen auf Ihrem Gerät

Um Methoden auf Ihren Geräten aufzurufen, verwenden Sie in der Remoteüberwachungslösung die Seite **Geräte**. In der Remoteüberwachungslösung implementieren **Kühlgeräte** beispielsweise eine **FirmwareUpdate**-Methode.

1. Wählen Sie **Geräte**, um zur Seite **Geräte** in der Lösung zu navigieren.

1. Wählen Sie auf der Seite **Geräte** in der Liste der Geräte das von Ihnen bereitgestellte Gerät aus:

    ![Auswählen des physischen Geräts](media/iot-suite-visualize-connecting/devicesselect.png)

1. Um eine Liste der Methoden anzuzeigen, die Sie auf Ihrem Gerät aufrufen können, wählen Sie **Aufträge** und dann **Run method** (Methode ausführen) aus. Um einen Auftrag für die Ausführung auf mehreren Geräten zu planen, können Sie mehrere Geräte in der Liste auswählen. Im Bereich **Aufträge** werden die für alle ausgewählten Geräte gemeinsamen Methodentypen angezeigt.

1. Wählen Sie **FirmwareUpdate** aus, und legen Sie **UpdatePhysischesKühlgerät** als Auftragsnamen fest. Legen Sie die **Firmwareversion** auf **2.0.0** und den **Firmware-URI** auf **http://contoso.com/updates/firmware.bin** fest, und klicken Sie dann auf **Anwenden**:

    ![Planen des Firmwareupdates](media/iot-suite-visualize-connecting/deviceschedule.png)

1. In der Konsole wird bei der Ausführung Ihres Gerätecodes eine Reihe von Meldungen angezeigt, während das simulierte Gerät die Methode verarbeitet.

1. Wenn das Update abgeschlossen ist, wird die neue Firmwareversion auf der Seite **Geräte** angezeigt:

    ![Update abgeschlossen](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Um den Status des Auftrags in der Lösung nachzuverfolgen, wählen Sie **Anzeigen**.

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Anpassen des Solution Accelerators für Remoteüberwachung](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) werden einige Möglichkeiten zum Anpassen des Solution Accelerators beschrieben.
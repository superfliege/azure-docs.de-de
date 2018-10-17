---
title: Durchführen einer Ursachenanalyse für eine Warnung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Time Series Insights eine Ursachenanalyse für eine Warnung durchführen.
author: aditidugar
ms.author: adugar
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/11/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 8258c8f34b4b9a1b216d9d497dcdf7d3b8db1373
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369400"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Tutorial: Durchführen einer Ursachenanalyse für eine Warnung

In diesem Tutorial erfahren Sie, wie Sie den Solution Accelerator für die Remoteüberwachung verwenden, um die Ursache einer Warnung zu diagnostizieren. Sie stellen im Dashboard der Remoteüberwachungslösung fest, dass eine Warnung ausgelöst wurde, und verwenden den Explorer von Azure Time Series Insights, um die Ursache zu untersuchen.

Im Tutorial werden zwei Geräte verwendet, die Lieferwagen simulieren und Telemetriedaten zu Standort, Höhe über NN, Geschwindigkeit und Temperatur der Ladung senden. Die LKW werden von der Organisation Contoso verwaltet und sind mit dem Solution Accelerator für die Remoteüberwachung verbunden. Als Contoso-Bediener müssen Sie herausfinden, warum einer Ihrer LKW (delivery-truck-02) eine Warnung aufgrund einer niedrigen Temperatur protokolliert hat.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Erkunden der Daten im Time Series Insights-Explorer
> * Durchführen einer Ursachenanalyse für eine Warnung
> * Erstellen einer neuen Regel basierend auf Ihren Erkenntnissen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Auswählen der anzuzeigenden Geräte

Verwenden Sie Filter, um festzulegen, welche verbundenen Geräte auf der Seite **Dashboard** angezeigt werden. Um nur die **Truck**-Geräte anzuzeigen, wählen Sie in der Dropdownliste zum Filtern den integrierten Filter **Trucks** aus:

[![Filtern nach LKW im Dashboard](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Wenn Sie einen Filter anwenden, werden auf der Karte und im Telemetriebereich auf der Seite **Dashboard** nur die Geräte angezeigt, die die Filterbedingungen erfüllen. Sie sehen, dass zwei LKW mit dem Solution Accelerator verbunden sind, darunter auch **truck-02**.

## <a name="view-real-time-telemetry"></a>Anzeigen von Echtzeit-Telemetriedaten

Der Solution Accelerator stellt im Diagramm auf der Seite **Dashboard** Echtzeit-Telemetriedaten dar. Standardmäßig zeigt das Diagramm Telemetriedaten zur Höhe über NN, die sich im Lauf der Zeit verändert:

[![Plot der Telemetriedaten zur Höhe über NN für LKW](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Klicken Sie im **Bereich „Telemetrie“** auf **Temperatur**, um für die Telemetriedaten der LKW zur Temperatur anzuzeigen. Sie sehen, wie sich die Temperatur für beide LKW während der letzten 15 Minuten verändert hat. Sie sehen auch, dass im Bereich „Warnungen“ eine Warnung aufgrund einer niedrigen Temperatur für „delivery-truck-02“ ausgelöst wurde.

[![Dashboard der Remoteüberwachung mit Warnung aufgrund niedriger Temperatur](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Untersuchen der Daten

Um die Ursache für die Warnung aufgrund einer niedrigen Temperatur zu ermitteln, öffnen Sie die Telemetriedaten für den LKW im Time Series Insights-Explorer. Klicken Sie im Dashboard auf einen der Links **In Time Series Insights erkunden**:

[![Dashboard der Remoteüberwachung mit hervorgehobenen Links zu Time Series Insights](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Wenn der Explorer gestartet wird, sehen Sie, dass all Ihre Geräte aufgeführt sind:

[![Anfängliche Ansicht des Time Series Insights-Explorers](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtern Sie die Geräte, indem Sie **delivery-truck** im Feld „Filter“ eingeben, und wählen Sie im linken Bereich **temperature** als **Measure** aus:

[![Time Series Insights-Explorer – LKW-Temperatur](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Es wird die gleiche Ansicht wie im Dashboard der Remoteüberwachung angezeigt, und Sie können jetzt auf den Zeitraum zoomen, in dem die Warnung ausgelöst wurde:

[![Time Series Insights-Explorer – Zoom](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Sie können auch weitere Telemetriedatenströme aus den LKW hinzufügen. Klicken Sie in der linken oberen Ecke auf die Schaltfläche **Hinzufügen**. Ein neuer Bereich wird angezeigt:

[![Time Series Insights-Explorer mit neuem Bereich](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

Ändern Sie im neuen Bereich den Namen der neuen Bezeichnung zu **Geräte**, sodass sie der vorherigen entspricht. Wählen Sie **altitute** als **Measure** und **iothub-connection-device-id** als Wert für **Teilen nach** aus, um die Telemetriedaten für die Höhe über NN zu Ihrer Ansicht hinzuzufügen:

[![Time Series Insights-Explorer mit Temperatur und Höhe über NN](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Diagnostizieren der Warnung

Wenn Sie sich die Datenströme in der aktuellen Ansicht ansehen, stellen Sie fest, dass die Profile der beiden LKW für die Höhe über NN sehr unterschiedlich sind. Der Temperatursturz in **delivery-truck-02** trat auf, als der LKW eine große Höhe über NN erreichte. Dieses Ergebnis überrascht Sie, da beide LKW die gleiche Route fahren sollten.

Um Ihren Verdacht zu bestätigen, dass die beiden LKW unterschiedliche Fahrtrouten genommen haben, fügen Sie mithilfe der Schaltfläche **Hinzufügen** einen neuen Bereich zum seitlichen Bereich hinzu. Ändern Sie im neuen Bereich den Namen der neuen Bezeichnung zu **Geräte**, sodass sie der vorherigen entspricht. Wählen Sie **longitude** als **Measure** und **iothub-connection-device-id** als Wert für **Teilen nach** aus, um die Telemetriedaten für den Längengrad zu Ihrer Ansicht hinzuzufügen. Indem Sie sich den Unterschied zwischen den Datenströmen für den **Längengrad** ansehen, können Sie sich davon überzeugen, dass die LKW tatsächlich unterschiedliche Strecken gefahren sind:

[![Time Series Insights-Explorer mit Temperatur, Höhe über NN und Längengrad](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Neue Regel erstellen

Fahrtrouten für LKW werden zwar in der Regel im Voraus optimiert, Sie stellen aber fest, dass die Verkehrslage, das Wetter und unvorhersehbare Ereignisse Verzögerungen verursachen können. Daher überlassen Sie den Fahrern kurzfristige Entscheidungen zur Strecke. Da die Temperatur der Waren im Fahrzeug aber von entscheidender Bedeutung ist, sollten Sie eine zusätzliche Regel in Ihrer Remoteüberwachungslösung erstellen, damit Sie eine Warnung erhalten, wenn die durchschnittliche Höhe über NN über einen Zeitraum von 1 Minute 350 Fuß überschreitet:

[![Registerkarte „Regeln“ der Remoteüberwachung mit Regel zur Höhe über NN](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Um zu erfahren, wie Sie Regeln erstellen und bearbeiten, lesen Sie das vorherige Tutorial zum [Erkennen von Geräteproblemen](iot-accelerators-remote-monitoring-automate.md).

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie den Time Series Insights-Explorer mit dem Solution Accelerator für die Remoteüberwachung verwenden, um die Ursache einer Warnung zu diagnostizieren. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Solution Accelerator zum Identifizieren und Beheben von Problemen mit Ihren verbundenen Geräten verwenden.

> [!div class="nextstepaction"]
> [Verwenden von Gerätewarnungen zum Identifizieren und Beheben von Problemen mit Geräten, die mit Ihrer Überwachungslösung verbunden sind](iot-accelerators-remote-monitoring-maintain.md)

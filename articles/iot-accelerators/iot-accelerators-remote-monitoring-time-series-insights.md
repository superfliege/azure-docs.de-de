---
title: Visualisieren von Telemetriedaten der Remoteüberwachung mit Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Time Series Insights-Umgebung konfigurieren und die vom Solution Accelerator für Remoteüberwachung generierten Telemetriedaten untersuchen und analysieren.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 02b3afbc62be7a7a9c396de888378d762405248a
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383022"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-remote-monitoring-solution-accelerator"></a>Verwenden von Time Series Insights zum Visualisieren der Telemetriedaten, die vom Solution Accelerator für Remoteüberwachung gesendet wurden

Als Operator möchten Sie die sofort einsatzbereiten Datenvisualisierungsfunktionen des Solution Accelerators für Remoteüberwachung möglicherweise erweitern. In dieser Anleitung wird gezeigt, wie Sie die an den Solution Accelerator für Remoteüberwachung gesendeten Telemetriedaten mithilfe einer Time Series Insights-Umgebung visualisieren und analysieren.

## <a name="prerequisites"></a>Voraussetzungen

Um die in dieser Anleitung aufgeführten Schritte ausführen zu können, benötigen Sie ein aktives Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Bei den Schritten in dieser Anleitung wird angenommen, dass Sie den Solution Accelerator für Remoteüberwachung im Rahmen Ihres Azure-Abonnements bereitgestellt haben. Falls Sie den Solution Accelerator für Remoteüberwachung noch nicht bereitgestellt haben, führen Sie die Schritte im Schnellstart [Bereitstellen und Ausführen einer cloudbasierten Lösung für Remoteüberwachung](quickstart-remote-monitoring-deploy.md) aus.

In diesem Artikel wird angenommen, dass der Name Ihres Solution Accelerators **Contoso-Simulation** lautet. Ersetzen Sie **Contoso-Simulation** durch den Namen Ihres Solution Accelerators, während Sie die nachstehenden Schritte ausführen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="time-series-insights-explorer"></a>Time Series Insights-Explorer

Der Time Series Insights-Explorer ist eine Web-App, mit der Sie Ihre Telemetriedaten visualisieren können.

1. Wählen Sie im Azure-Portal die Time Series Insights-Registerkarte **Übersicht** aus.

1. Um die Web-App Time Series Insights-Explorer zu öffnen, klicken Sie auf **Zur Umgebung wechseln**:

    ![Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-environment.png)

1. Wählen Sie im Zeitauswahlpanel **Letzte 30 Minuten** aus dem Menü für schnelle Zeitangaben aus, und klicken Sie auf **Suchen**:

    ![Suche im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-search-time.png)

1. Wählen Sie im Bedingungspanel auf der linken Seite **Temperatur** als **Measure** und **iothub-connection-device-id** als **Teilen nach**-Wert aus:

    ![Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query1.png)

1. Klicken Sie mit der rechten Maustaste auf das Diagramm, und wählen Sie **Ereignisse untersuchen** aus:

    ![Ereignisse im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explore-events.png)

1. Die Ereignisdaten werden in einem Raster gezeigt:

    ![Tabelle im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-table.png)

1. Klicken Sie auf die Schaltfläche für die Perspektivenansicht:

    ![Perspektive im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klicken Sie auf **+**, um der Perspektive eine neue Abfrage hinzuzufügen:

    ![Hinzufügen einer Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-new-query.png)

1. Wählen Sie **Letzte 30 Minuten** als Zeitspanne, **Luftfeuchtigkeit** als **Measure** und **iothub-connection-device-id** als **Teilen nach**-Wert aus:

    ![Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query2.png)

1. Klicken Sie auf die Schaltfläche für die Perspektivenansicht, um Ihr Dashboard mit Gerätetelemetriedaten anzuzeigen:

    ![Dashboard im Time Series Insights-Explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Durchsuchen und Abfragen von Daten im Time Series Insights-Explorer finden Sie unter [Azure Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).

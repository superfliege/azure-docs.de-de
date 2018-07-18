---
title: Visualisieren von Remoteüberwachungsdaten mit Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Time Series Insights-Umgebung konfigurieren und die Zeitreihendaten Ihrer Remoteüberwachungslösung untersuchen und analysieren.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: f16fdfca704b8f8cb175de637ad7f3ef143d3ed7
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968954"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Visualisieren von Remoteüberwachungsdaten mit Time Series Insights

Ein Operator kann die sofort einsatzbereite Datenvisualisierung erweitern, die durch die vorkonfigurierte Lösung der Remoteüberwachung bereitgestellt wird. Unser Solution Accelerator bietet eine vorkonfigurierte Integration in TSI. In dieser Anleitung erfahren Sie, wie Sie Time Series Insights für die Analyse von Gerätetelemetriedaten und die Erkennung von Anomalien konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Vorgehensweise ist Folgendes erforderlich:

* [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-accelerators-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Erstellen einer Consumergruppe

Sie müssen eine dedizierte Consumergruppe in Ihrem IoT Hub erstellen, die zum Streamen von Daten an Time Series Insights verwendet wird.

> [!NOTE]
> Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure IoT Hub abzurufen. Jede Consumergruppe kann bis zu fünf Ausgabeconsumer enthalten. Sie sollten eine neue Consumergruppe für jeweils fünf Ausgabesenken erstellen. Es können bis zu 32 Consumergruppen erstellt werden.

1. Klicken Sie im Azure-Portal auf die Schaltfläche „Cloud Shell“.

1. Führen Sie den folgenden Befehl aus, um eine neue Consumergruppe zu erstellen:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Erstellen einer neuen Time Series Insights-Umgebung

Azure Time Series Insights ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst für die Verwaltung von IoT-Zeitreihendaten in der Cloud. Dieser bietet Ihnen in höchstem Maße skalierbaren Speicher für Zeitreihendaten und ermöglicht die Prüfung und Analyse von Milliarden Ereignissen, die in Sekundenschnelle aus der ganzen Welt gestreamt werden. Mit Time Series Insights speichern und verwalten Sie Terabytes von Zeitreihendaten, analysieren und visualisieren Milliarden Ereignisse gleichzeitig, führen Fehlerursachenanalysen aus und vergleichen mehrere Sites und Ressourcen.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.

1. Wählen Sie **Ressource erstellen** > **Internet der Dinge** > **Time Series Insights**.

    ![Neu: Time Series Insights](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Verwenden Sie zur Erstellung Ihrer Time Series Insights-Umgebung die Werte in der folgenden Tabelle:

    | Einstellung | Wert |
    | ------- | ----- |
    | Umgebungsname | Im folgenden Screenshot wird der Name **contorosrmtsi** verwendet. Wählen Sie Ihren eigenen eindeutigen Namen, wenn Sie diesen Schritt durchführen. |
    | Abonnement | Wählen Sie Ihr Azure-Abonnement in der Dropdownliste aus. |
    | Ressourcengruppe | **Neu erstellen**. Wir verwenden den Namen **ContosoRM**. |
    | Speicherort | Hier verwenden wir **USA, Osten**. Erstellen Sie Ihre Umgebung in derselben Region wie Ihre Remoteüberwachungslösung. |
    | Sku |**S1** |
    | Capacity | **1** |
    | An Dashboard anheften | **Ja** |

    ![Time Series Insights erstellen](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. Klicken Sie auf **Create**. Die Erstellung der Umgebung kann einen Moment dauern.

## <a name="create-event-source"></a>Erstellen der Ereignisquelle

Erstellen Sie eine neue Ereignisquelle für die Verbindung mit Ihrem IoT Hub. Stellen Sie sicher, dass Sie die Consumergruppe verwenden, die in den vorherigen Schritten erstellt wurde. Für Time Series Insights muss jeder Dienst über eine dedizierte Consumergruppe verfügen, die von keinem anderen Dienst verwendet wird.

1. Navigieren Sie zu Ihrer neuen Time Series-Umgebung.

1. Wählen Sie auf der linken Seite **Ereignisquellen**.

    ![Anzeigen von Ereignisquellen](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen einer Ereignisquelle](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. Verwenden Sie die Werte in der folgenden Tabelle, um Ihren IoT Hub als eine neue Ereignisquelle zu konfigurieren:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name der Ereignisquelle | Im folgenden Screenshot wird der Name **contosorm-iot-hub** verwendet. Verwenden Sie Ihren eigenen eindeutigen Namen, wenn Sie diesen Schritt durchführen. |
    | Quelle | **IoT Hub** |
    | Importoption | **Verwenden eines IoT Hub aus verfügbaren Abonnements** |
    | Subscription Id (Abonnement-ID) | Wählen Sie Ihr Azure-Abonnement in der Dropdownliste aus. |
    | IoT Hub-Name | **contosorma57a6**. Verwenden Sie den Namen Ihres IoT Hub aus Ihrer Remoteüberwachungslösung. |
    | IoT Hub-Richtlinienname | **iothubowner** Stellen Sie sicher, dass die verwendete Richtlinie eine Besitzerrichtlinie ist. |
    | IoT Hub-Richtlinienschlüssel | Dieses Feld wird automatisch ausgefüllt. |
    | IoT Hub-Consumergruppe | **timeseriesinsights** |
    | Ereignisserialisierungsformat | **JSON**     | Name der Timestamp-Eigenschaft | Nicht ausfüllen |

    ![Erstellen der Ereignisquelle](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. Klicken Sie auf **Create**.

> [!NOTE]
> Wenn Sie weiteren Benutzern Zugriff auf den Time Series Insights-Explorer gewähren möchten, können Sie folgendermaßen vorgehen, um [Datenzugriff zu erteilen](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Time Series Insights-Explorer

Der Time Series Insights-Explorer ist eine Web-App, mit der Sie Visualisierungen Ihrer Daten erstellen können.

1. Klicken Sie auf die Registerkarte **Übersicht**.

1. Klicken Sie auf **Zur Umgebung wechseln**. Die Time Series Insights-Explorer-Web-App wird geöffnet.

    ![Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. Wählen Sie im Zeitauswahlpanel **Letzte 12 Stunden** aus dem Menü für schnelle Zeitangaben aus, und klicken Sie auf **Suchen**.

    ![Suche im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. Wählen Sie im Bedingungspanel auf der linken Seite den Measurewert **Temperatur** und für „Teilen nach“ den Wert **iothub-connection-device-id** aus.

    ![Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Klicken Sie mit der rechten Maustaste auf das Diagramm, und wählen Sie **Ereignisse untersuchen**.

    ![Ereignisse im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. Die Ereignisse werden im Raster im Tabellenformat gerendert.

    ![Tabelle im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Klicken Sie auf die Schaltfläche für die Perspektivenansicht.

    ![Perspektive im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klicken Sie auf **Hinzufügen**, um eine neue Abfrage in der Perspektive zu erstellen.

    ![Hinzufügen einer Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Wählen Sie im Menü für schnelle Zeitangaben **Letzte 12 Stunden**, das Measure **Luftfeuchtigkeit** und für „Teilen nach“ den Wert **iothub-connection-device-id** aus.

    ![Abfrage im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Klicken Sie auf die Schaltfläche für die Perspektivenansicht, um Ihr Dashboard mit Gerätemetriken anzuzeigen.

    ![Dashboard im Time Series Insights-Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Durchsuchen und Abfragen von Daten im Time Series Insights-Explorer finden Sie unter [Azure Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).

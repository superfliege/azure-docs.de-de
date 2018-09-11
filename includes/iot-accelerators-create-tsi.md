---
title: Includedatei
description: Includedatei
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/20/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 8c114ed137089e70899e601ebdc1d4d39f562601
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383081"
---
## <a name="create-a-consumer-group"></a>Erstellen einer Consumergruppe

Sie müssen eine dedizierte Consumergruppe in Ihrem IoT Hub erstellen, um Telemetriedaten an Time Series Insights zu streamen. Eine Ereignisquelle in Time Series Insights sollte über die exklusive Nutzung einer IoT Hub-Consumergruppe verfügen.

Bei den nachstehenden Schritten wird die Azure-Befehlszeilenschnittstelle in der Azure Cloud Shell verwendet, um die Consumergruppe zu erstellen:

1. Der IoT Hub ist eine von mehreren Ressourcen, die bei Ihrer Bereitstellung des Solution Accelerators für Gerätesimulation erstellt wurden. Führen Sie den folgenden Befehl aus, um den Namen Ihres IoT Hubs zu suchen. Denken Sie daran, den Namen Ihres Solution Accelerators zu verwenden:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    Der IoT Hub ist die Ressource des Typs **Microsoft.Devices/IotHubs**.

1. Fügen Sie dem Hub die Consumergruppe **devicesimulationtsi** hinzu. Verwenden Sie im folgenden Befehl den Namen Ihres Hubs und Solution Accelerators:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    Jetzt können Sie die Azure Cloud Shell schließen.

## <a name="create-a-new-time-series-insights-environment"></a>Erstellen einer neuen Time Series Insights-Umgebung

[Azure Time Series Insights](../articles/time-series-insights/time-series-insights-overview.md) ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst für die Verwaltung von IoT-Zeitreihendaten in der Cloud. So erstellen Sie eine neue Time Series Insights-Umgebung:

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.

1. Wählen Sie **Ressource erstellen** > **Internet der Dinge** > **Time Series Insights** aus:

    ![Neu: Time Series Insights](./media/iot-accelerators-create-tsi/new-time-series-insights.png)

1. Wenn Sie Ihre Time Series Insights-Umgebung in derselben Ressourcengruppe wie der Ihres Solution Accelerators erstellen möchten, verwenden Sie die Werte in der nachstehenden Tabelle:

    | Einstellung | Wert |
    | ------- | ----- |
    | Umgebungsname | Im nachstehenden Screenshot wird der Name **Contoso-TSI** verwendet. Wählen Sie Ihren eigenen eindeutigen Namen, wenn Sie diesen Schritt durchführen. |
    | Abonnement | Wählen Sie Ihr Azure-Abonnement in der Dropdownliste aus. |
    | Ressourcengruppe | **Contoso-Simulation**. Verwenden Sie den Namen Ihres Solution Accelerators. |
    | Standort | In diesem Beispiel wird **USA, Osten** verwendet. Erstellen Sie Ihre Umgebung in derselben Region wie Ihren Accelerator für Gerätesimulation. |
    | Sku |**S1** |
    | Capacity | **1** |

    ![Time Series Insights erstellen](./media/iot-accelerators-create-tsi/new-time-series-insights-create.png)

    > [!NOTE]
    > Wenn Sie die Time Series Insights-Umgebung zu derselben Ressourcengruppe wie der des Solution Accelerators hinzufügen, bedeutet dies: Sie wird beim Löschen des Solution Accelerators ebenfalls gelöscht.

1. Klicken Sie auf **Create**. Die Erstellung der Umgebung kann ein paar Minuten dauern.

## <a name="create-event-source"></a>Erstellen der Ereignisquelle

Erstellen Sie eine neue Ereignisquelle für die Verbindung mit Ihrem IoT Hub. Verwenden Sie dazu die Consumergruppe, die Sie in den vorherigen Schritten erstellt haben. Eine Time Series Insights-Ereignisquelle erfordert eine dedizierte Consumergruppe, die von keinem anderen Dienst verwendet wird.

1. Navigieren Sie im Azure-Portal zu Ihrer neuen Time Series Insights-Umgebung.

1. Klicken Sie auf der linken Seite auf **Ereignisquellen**:

    ![Anzeigen von Ereignisquellen](./media/iot-accelerators-create-tsi/time-series-insights-event-sources.png)

1. Klicken Sie auf **Hinzufügen**:

    ![Hinzufügen einer Ereignisquelle](./media/iot-accelerators-create-tsi/time-series-insights-event-sources-add.png)

1. Verwenden Sie die Werte in der folgenden Tabelle, um Ihren IoT Hub als eine neue Ereignisquelle zu konfigurieren:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name der Ereignisquelle | Im nachstehenden Screenshot wird der Name **contoso-iot-hub** verwendet. Verwenden Sie Ihren eigenen eindeutigen Namen, wenn Sie diesen Schritt durchführen. |
    | Quelle | **IoT Hub** |
    | Importoption | **Verwenden eines IoT Hub aus verfügbaren Abonnements** |
    | Subscription Id (Abonnement-ID) | Wählen Sie Ihr Azure-Abonnement in der Dropdownliste aus. |
    | IoT Hub-Name | **contoso-simulation7d894**. Verwenden Sie den Namen Ihres IoT Hubs aus Ihrem Solution Accelerator für Gerätesimulation. |
    | IoT Hub-Richtlinienname | **iothubowner** |
    | IoT Hub-Richtlinienschlüssel | Dieses Feld wird automatisch ausgefüllt. |
    | IoT Hub-Consumergruppe | **devicesimulationtsi** |
    | Ereignisserialisierungsformat | **JSON** |
    | Name der Timestamp-Eigenschaft | Nicht ausfüllen |

    ![Erstellen der Ereignisquelle](./media/iot-accelerators-create-tsi/time-series-insights-event-source-create.png)

1. Klicken Sie auf **Create**.

> [!NOTE]
> Sie können [weiteren Benutzern Zugriff gewähren](../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) auf den Time Series Insights-Explorer.
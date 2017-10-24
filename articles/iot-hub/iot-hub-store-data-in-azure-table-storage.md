---
title: Speichern Ihrer IoT Hub-Nachrichten im Azure-Datenspeicher | Microsoft-Dokumentation
description: "Verwenden Sie das Nachrichtenrouting von IoT Hub, um Ihre IoT Hub-Nachrichten in Ihrem Azure-Blobspeicher zu speichern. IoT Hub-Nachrichten enthalten Informationen wie Sensordaten, die von Ihrem IoT-Gerät gesendet werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IoT-Datenspeicher, IoT-Sensordatenspeicher
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2017
ms.author: xshi
ms.openlocfilehash: aa33800de82b27d4819fe0eade127c2a40e3a493
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Speichern von IoT Hub-Nachrichten mit Sensordaten in Ihrem Azure-Blobspeicher

![Lückenloses Diagramm](media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie ein Azure-Speicherkonto und eine Azure-Funktions-App zum Speichern von IoT Hub-Nachrichten in Ihrem Tabellenspeicher erstellen.

## <a name="what-you-do"></a>Aufgaben

- Erstellen Sie ein Azure-Speicherkonto.
- Bereiten Sie Ihren IoT Hub zum Weiterleiten von Nachrichten an den Speicher vor.

## <a name="what-you-need"></a>Erforderliches Element

- [Richten Sie Ihr Gerät ein](iot-hub-raspberry-pi-kit-node-get-started.md), um folgende Anforderungen zu erfüllen:
  - Ein aktives Azure-Abonnement
  - Ein IoT Hub in Ihrem Abonnement 
  - Eine ausgeführte Anwendung, die Nachrichten an Ihren IoT Hub sendet

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Speicher** > **Speicherkonto** > **Erstellen**.

2. Geben Sie die erforderlichen Informationen für das Speicherkonto ein:

   ![Erstellen eines Speicherkontos im Azure-Portal](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Name**: Der Name des Speicherkontos. Der Name muss global eindeutig sein.

   * **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

3. Klicken Sie auf **Erstellen**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Vorbereiten Ihres IoT Hub zum Weiterleiten von Nachrichten an den Speicher

IoT Hub unterstützt nativ das Weiterleiten von Nachrichten an den Azure-Speicher als Blobs.

### <a name="add-storage-as-a-custom-endpoint"></a>Hinzufügen von Speicher als benutzerdefinierter Endpunkt

Navigieren Sie im Azure-Portal zu Ihrem IoT Hub. Klicken Sie auf **Endpunkte** > **Hinzufügen**. Benennen Sie den Endpunkt, und wählen Sie **Azure-Speichercontainer** als Typ des Endpunkts. Verwenden Sie die Auswahl, um das Speicherkonto auszuwählen, das Sie im vorherigen Abschnitt erstellt haben. Erstellen Sie einen Speichercontainer, wählen Sie ihn aus, und klicken Sie auf **OK**.

  ![Erstellen eines benutzerdefinierten Endpunkts in IoT Hub](media\iot-hub-store-data-in-azure-table-storage\2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Hinzufügen einer Route zum Weiterleiten von Daten an den Speicher

Klicken Sie auf **Routen** > **Hinzufügen**, und geben Sie einen Namen für die Route ein. Wählen Sie **Gerätemeldungen** als Datenquelle aus, und wählen Sie den Speicherendpunkt, den Sie gerade erstellt haben, als Endpunkt in der Route aus. Geben Sie `true` als Abfragezeichenfolge ein, und klicken Sie dann auf **Speichern**.

  ![Erstellen einer Route in IoT Hub](media\iot-hub-store-data-in-azure-table-storage\3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Hinzufügen einer Route für Langsamster-Pfad-Telemetrie (optional)

Standardmäßig leitet IoT Hub alle Nachrichten, die nicht mit anderen Routen übereinstimmen, dem integrierten Endpunkt zu. Da alle Telemetrienachrichten jetzt der Regel entsprechen, die Nachrichten an den Speicher weiterleitet, müssen Sie für Nachrichten, die in den integrierten Endpunkt geschrieben werden sollen, eine andere Route hinzufügen. Es fallen keine zusätzlichen Gebühren zum Weiterleiten von Nachrichten an mehrere Endpunkte an.

> [!NOTE]
> Sie können diesen Schritt überspringen, wenn Sie keine zusätzliche Verarbeitung Ihrer Telemetrienachrichten ausführen.

Klicken Sie im Bereich „Routen“ auf **Hinzufügen**, und geben Sie einen Namen für die Route ein. Wählen Sie **Gerätemeldungen** als Datenquelle und **Ereignisse** als Endpunkt aus. Geben Sie `true` als Abfragezeichenfolge ein, und klicken Sie dann auf **Speichern**.

  ![Erstellen einer Langsamster-Pfad-Route in IoT Hub](media\iot-hub-store-data-in-azure-table-storage\4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Überprüfen der Nachricht in Ihrem Speichercontainer

1. Führen Sie die Beispielanwendung auf Ihrem Gerät aus, um Nachrichten an Ihren IoT Hub zu senden.

2. [Laden Sie den Azure Storage Explorer herunter, und installieren Sie ihn](http://storageexplorer.com/).

3. Öffnen Sie den Storage Explorer. Klicken Sie auf **Azure-Konto hinzufügen** > **Anmelden**, und melden Sie sich bei Ihrem Azure-Konto an.

4. Klicken Sie auf Ihr Azure-Abonnement > **Speicherkonten** > Ihr Speicherkonto > **Blobcontainer** > Ihr Container.

   Der Blobcontainer sollte protokollierte Nachrichten enthalten, die von Ihrem Gerät an Ihren IoT Hub gesendet wurden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich Ihr Azure-Speicherkonto erstellt und Nachrichten vom IoT Hub zu einem Blobcontainer in diesem Speicherkonto weitergeleitet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

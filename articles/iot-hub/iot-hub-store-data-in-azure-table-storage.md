---
title: Speichern Ihrer IoT Hub-Nachrichten im Azure-Datenspeicher | Microsoft-Dokumentation
description: Verwenden Sie das Nachrichtenrouting von IoT Hub, um Ihre IoT Hub-Nachrichten in Azure Blob Storage zu speichern. IoT Hub-Nachrichten enthalten Informationen wie Sensordaten, die von Ihrem IoT-Gerät gesendet werden.
author: rangv
manager: ''
keywords: IoT-Datenspeicher, IoT-Sensordatenspeicher
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856289"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Speichern von IoT Hub-Nachrichten mit Sensordaten in Azure Blob Storage

![Lückenloses Diagramm](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie ein Azure-Speicherkonto und eine Azure-Funktions-App zum Speichern von IoT Hub-Nachrichten in Azure-Blobspeicher erstellen.

## <a name="what-you-do"></a>Aufgaben

- Erstellen eines Azure-Speicherkontos
- Einrichten Ihres IoT Hub zum Weiterleiten von Nachrichten an den Speicher

## <a name="what-you-need"></a>Voraussetzungen

- [Richten Sie Ihr Gerät ein](iot-hub-raspberry-pi-kit-node-get-started.md), um folgende Anforderungen zu erfüllen:
  - Ein aktives Azure-Abonnement
  - Ein IoT Hub in Ihrem Abonnement 
  - Eine ausgeführte Anwendung, die Nachrichten an Ihren IoT Hub sendet

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Ressource erstellen** > **Storage** > **Speicherkonto**.

2. Geben Sie die erforderlichen Informationen für das Speicherkonto ein:

   ![Erstellen eines Speicherkontos im Azure-Portal](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Name**: Der Name des Speicherkontos. Der Name muss global eindeutig sein.

   * **Kontoart**: Wählen Sie `Storage (general purpose v1)`.

   * **Standort**: Wählen Sie denselben Standort, der für Ihren IoT Hub verwendet wird.

   * **Replikation**: Wählen Sie `Locally-redundant storage (LRS)`.

   * **Leistung**: Wählen Sie `Standard`.

   * **Sichere Übertragung erforderlich**: Wählen Sie `Disabled`.

   * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.

   * **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

3. Klicken Sie auf **Create**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Vorbereiten Ihres IoT Hub zum Weiterleiten von Nachrichten an den Speicher

IoT Hub unterstützt nativ das Weiterleiten von Nachrichten an den Azure-Speicher als Blobs. Mehr über die benutzerdefinierten Azure IoT Hub-Endpunkte erfahren Sie unter [Liste der integrierten IoT Hub-Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Hinzufügen von Speicher als benutzerdefinierter Endpunkt

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub. 

2. Klicken Sie auf **Endpunkte** > **Hinzufügen**. 

3. Benennen Sie den Endpunkt, und wählen Sie **Azure-Speichercontainer** als Typ des Endpunkts. 

4. Verwenden Sie die Auswahl, um das Speicherkonto auszuwählen, das Sie im vorherigen Abschnitt erstellt haben. Erstellen Sie einen Speichercontainer, wählen Sie ihn aus, und klicken Sie auf **OK**.

   ![Erstellen eines benutzerdefinierten Endpunkts in IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Hinzufügen einer Route zum Weiterleiten von Daten an den Speicher

1. Klicken Sie auf **Routen** > **Hinzufügen**, und geben Sie einen Namen für die Route ein. 

2. Wählen Sie **Gerätemeldungen** als Datenquelle aus, und wählen Sie den Speicherendpunkt, den Sie gerade erstellt haben, als Endpunkt in der Route aus. 

3. Geben Sie `true` als Abfragezeichenfolge ein, und klicken Sie dann auf **Speichern**.

   ![Erstellen einer Route in IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Hinzufügen einer Route für Langsamster-Pfad-Telemetrie (optional)

Standardmäßig leitet IoT Hub alle Nachrichten, die nicht mit anderen Routen übereinstimmen, dem integrierten Endpunkt zu. Da alle Telemetrienachrichten jetzt der Regel entsprechen, die Nachrichten an den Speicher weiterleitet, müssen Sie für Nachrichten, die in den integrierten Endpunkt geschrieben werden sollen, eine andere Route hinzufügen. Es fallen keine zusätzlichen Gebühren zum Weiterleiten von Nachrichten an mehrere Endpunkte an.

> [!NOTE]
> Sie können diesen Schritt überspringen, wenn Sie keine zusätzliche Verarbeitung Ihrer Telemetrienachrichten ausführen.

1. Klicken Sie im Bereich „Routen“ auf **Hinzufügen**, und geben Sie einen Namen für die Route ein. 

2. Wählen Sie **Gerätemeldungen** als Datenquelle und **Ereignisse** als Endpunkt aus. 

3. Geben Sie `true` als Abfragezeichenfolge ein, und klicken Sie dann auf **Speichern**.

  ![Erstellen einer Langsamster-Pfad-Route in IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Überprüfen der Nachricht in Ihrem Speichercontainer

1. Führen Sie die Beispielanwendung auf Ihrem Gerät aus, um Nachrichten an Ihren IoT Hub zu senden.

2. [Laden Sie den Azure Storage-Explorer herunter, und installieren Sie ihn](http://storageexplorer.com/).

3. Öffnen Sie den Storage-Explorer. Klicken Sie auf **Azure-Konto hinzufügen** > **Anmelden**, und melden Sie sich bei Ihrem Azure-Konto an.

4. Klicken Sie auf Ihr Azure-Abonnement > **Speicherkonten** > Ihr Speicherkonto > **Blobcontainer** > Ihr Container.

   Der Blobcontainer sollte protokollierte Nachrichten enthalten, die von Ihrem Gerät an Ihren IoT Hub gesendet wurden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

In diesem Tutorial haben Sie ein Speicherkonto und anschließend die Weiterleitung für Nachrichten vom IoT Hub hinzugefügt, damit diese in das Speicherkonto geschrieben werden. Zum Bereinigen der erstellten Ressourcen entfernen Sie die Weiterleitungsinformationen und löschen dann das Speicherkonto. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Ressourcengruppen**, und wählen Sie die von Ihnen verwendete Ressourcengruppe aus. Die Liste mit den Ressourcen der Gruppe wird angezeigt. 

   > [!NOTE]
   > Wenn Sie alle Ressourcen der Ressourcengruppe entfernen möchten, klicken Sie auf **Löschen**, um die Ressourcengruppe zu löschen, und befolgen Sie dann die Anleitung. Der gesamte Inhalt der Ressourcengruppe wird entfernt. Sie haben die Bereinigung hiermit abgeschlossen und können mit dem nächsten Abschnitt fortfahren.

3. Klicken Sie auf den IoT Hub, den Sie für dieses Tutorial verwendet haben. 

4. Klicken Sie im IoT Hub-Bereich auf **Routen**. Klicken Sie auf das Kontrollkästchen neben der Routingregel, die Sie hinzugefügt haben, und klicken Sie dann auf **Löschen**. Klicken Sie auf **Ja**, wenn die Aufforderung zum Löschen dieser Route angezeigt wird.

   ![Entfernen der Routingregel](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Schließen Sie den Bereich „Routing“. Sie gelangen zurück zum Bereich „Ressourcengruppe“.

5. Klicken Sie erneut auf den IoT Hub. 

6. Klicken Sie im Bereich „IoT Hub“ auf **Endpunkte**. Klicken Sie auf das Kontrollkästchen neben dem Endpunkt, den Sie für den Speichercontainer hinzugefügt haben, und klicken Sie anschließend auf **Löschen**. Klicken Sie auf **Ja**, wenn die Aufforderung zum Löschen des ausgewählten Endpunkts angezeigt wird.

    ![Entfernen des Endpunkts](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Schließen Sie den Bereich „Endpunkte“. Sie gelangen zurück zum Bereich „Ressourcengruppe“. 

7.  Klicken Sie auf das Speicherkonto, das Sie für dieses Tutorial eingerichtet haben. 

8.  Klicken Sie im Bereich „Speicherkonto“ auf **Löschen**, um das Speicherkonto zu entfernen. Sie gelangen zum Bereich **Speicherkonto löschen**.

   ![Entfernen des Speicherkontos](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Geben Sie den Namen des Speicherkontos ein, und klicken Sie dann unten im Bereich auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich Ihr Azure-Speicherkonto erstellt und Nachrichten vom IoT Hub zu einem Blobcontainer in diesem Speicherkonto weitergeleitet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

---
title: Exportieren von Daten in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/05/2017
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 15697f97a3b223e0a946795ce0dc5e9d3d86ccb8
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55772342"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportieren von Daten in Azure IoT Central

*Dieses Thema gilt für Administratoren.*

In diesem Artikel wird beschrieben, wie Sie in Azure IoT Central das Feature für den kontinuierlichen Datenexport verwenden, um Daten regelmäßig in Ihre eigenen **Azure Blob Storage**-, **Azure Event Hubs**- und **Azure Service Bus**-Instanzen zu exportieren. Sie können **Messungen**, **Geräte** und **Gerätevorlagen** an Ihr eigenes Ziel exportieren, um Analysen warmer und kalter Pfade durchzuführen. Sie können Daten in Blobspeicher exportieren, um langfristige Trendanalysen in Microsoft Power BI durchzuführen, oder Daten in Event Hubs und Service Bus exportieren, um Ihre Daten nahezu in Echtzeit mit Azure Logic Apps oder Azure Functions zu transformieren und anzureichern.

> [!Note]
> Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Derzeit können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Aktivieren Sie den fortlaufenden Datenexport frühzeitig, um umfassendere Verlaufsdaten zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen Administrator in Ihrer IoT Central-Anwendung sein.

## <a name="export-to-blob-storage"></a>Exportieren in Blob Storage

Messungen, Geräte und Gerätevorlagendaten werden einmal pro Minute in Ihr Speicherkonto exportiert, wobei jede Datei den Batch der Änderungen seit der letzten exportierten Datei enthält. Die exportierten Daten haben das Format [Apache AVRO](https://avro.apache.org/docs/current/index.html).

Erfahren Sie mehr über das [Exportieren in Azure Blob Storage](howto-export-data-blob-storage.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="export-to-event-hubs-and-service-bus"></a>Exportieren in Event Hubs und Service Bus

Messungen, Geräte und Gerätevorlagen werden in Ihren Event Hub, Ihre Service Bus-Warteschlange oder ein Thema exportiert. Exportierte Messdaten gehen nahezu in Echtzeit ein und enthalten die Gesamtheit der Nachricht, die Ihre Geräte an IoT Central gesendet haben, nicht nur die Werte der Messungen selbst. Exportierte Gerätedaten gehen batchweise mit einem Batch pro Sekunde ein und enthalten Änderungen an Eigenschaften und Einstellungen aller Geräte, und exportierte Gerätevorlagen enthalten Änderungen an allen Gerätevorlagen.


Weitere Informationen zum [Exportieren in Event Hubs und Service Bus](howto-export-data-event-hubs-service-bus.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="set-up-export-destination"></a>Einrichten des Exportziels

Wenn Sie keinen vorhandenen Storage/Event Hubs/Service als Exportziel besitzen, gehen Sie wie folgt vor:

### <a name="create-storage-account"></a>Speicherkonto erstellen

1. Erstellen Sie ein [neues Speicherkonto im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Weitere Informationen erhalten Sie in der [Azure Storage-Dokumentation](https://aka.ms/blobdocscreatestorageaccount).
2. Wählen Sie als Kontotyp **Allgemein** oder **Blobspeicher** aus.
3. Wählen Sie ein Abonnement aus. 

    > [!Note] 
    > Jetzt können Sie Daten in andere Abonnements exportieren, die **nicht identisch sind** mit dem für Ihre IoT Central-Anwendung mit nutzungsbasierter Bezahlung. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

4. Erstellen Sie einen Container in Ihrem Speicherkonto. Wechseln Sie zum Speicherkonto. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen**. Wählen Sie oben die Option **+ Container** aus, um einen neuen Container zu erstellen.

### <a name="create-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespaces

1. Erstellen Sie einen [neuen Event Hubs-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Weitere Informationen erhalten Sie in der [Azure Event Hubs-Dokumentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Wählen Sie ein Abonnement aus. 

    > [!Note] 
    > Jetzt können Sie Daten in andere Abonnements exportieren, die **nicht identisch sind** mit dem für Ihre IoT Central-Anwendung mit nutzungsbasierter Bezahlung. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.
3. Erstellen Sie einen Event Hub in Ihrem Event Hubs-Namespace. Wechseln Sie zu Ihrem Namespace, und wählen Sie oben **+ Event Hub** aus, um eine Event Hub-Instanz zu erstellen.

### <a name="create-service-bus-namespace"></a>Erstellen eines Service Bus-Namespaces

1. Erstellen Sie einen [neuen Service Bus-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Weitere Informationen erhalten Sie in der [Azure Service Bus-Dokumentation](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Wählen Sie ein Abonnement aus. 

    > [!Note] 
    > Jetzt können Sie Daten in andere Abonnements exportieren, die **nicht identisch sind** mit dem für Ihre IoT Central-Anwendung mit nutzungsbasierter Bezahlung. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

3. Wechseln Sie zu Ihrem Service Bus-Namespace, und wählen Sie oben **+ Warteschlange** oder **+ Thema** aus, um eine Warteschlange oder ein Thema als Exportziel zu erstellen.

## <a name="set-up-continuous-data-export"></a>Einrichten des fortlaufenden Datenexports

Nachdem Sie nun einen Speicher/Event Hubs/Service Bus als Exportziel für Daten besitzen, gehen Sie folgendermaßen vor, um den kontinuierlichen Datenexport einzurichten. 

1. Melden Sie sich bei Ihrer IoT Central-Anwendung an.

2. Klicken Sie im linken Menü auf **Kontinuierlicher Datenexport**.

    > [!Note]
    > Wenn im linken Menü „Kontinuierlicher Datenexport“ nicht angezeigt wird, sind Sie kein Administrator in Ihrer App. Wenden Sie sich an Ihren Administrator, damit dieser den Datenexport einrichtet.

    ![Erstellen eines neun Event Hubs für kontinuierlichen Datenexport](media/howto-export-data-experimental/export_menu.png)

3. Klicken Sie oben rechts auf die Schaltfläche **+ Neu**. Wählen Sie einen der Einträge **Azure Blob Storage**, **Azure Event Hubs** oder **Azure Service Bus** als Ziel für Ihren Export aus. 

    > [!NOTE] 
    > Die maximale Anzahl von Exporten pro App beträgt 5. 

    ![Erstellen eine neuen kontinuierlichen Datenexports](media/howto-export-data-experimental/export_new.png)

4. Wählen Sie im Dropdown-Listenfeld **Speicherkonto/Event Hubs-Namespace/Service Bus-Namespace** aus. Sie können auch die letzte Option in der Liste auswählen, bei der es sich um **Verbindungszeichenfolge eingeben** handelt. 

    > [!NOTE] 
    > Ihnen werden nur Speicherkonten/Event Hubs-Namespaces/Service Bus-Namespaces aus **demselben Abonnement wie dem Ihrer IoT Central-App** angezeigt. Wenn Sie in ein Ziel außerhalb dieses Abonnements exportieren möchten, wählen Sie **Verbindungszeichenfolge eingeben** aus, und fahren Sie mit Schritt 5 fort.

    > [!NOTE] 
    > Bei 7-Tage-Test-Apps ist eine Verbindungszeichenfolge die einzige Möglichkeit zur Konfiguration des kontinuierlichen Datenexports. Dies liegt daran, dass 7-Tage-Test-Apps kein Azure-Abonnement zugeordnet ist.

    ![Erstellen eines neun Event Hubs für kontinuierlichen Datenexport](media/howto-export-data-experimental/export_create.png)

5. (Optional) Wenn Sie **Verbindungszeichenfolge eingeben** ausgewählt haben, wird ein neues Feld angezeigt, in das Sie Ihre Verbindungszeichenfolge einfügen können. Um die Verbindungszeichenfolge für 
    - Ihr Speicherkonto abzurufen,wechseln Sie im Azure-Portal zu dem Speicherkonto.
        - Klicken Sie unter **Einstellungen** auf **Zugriffsschlüssel**.
        - Kopieren Sie entweder die Verbindungszeichenfolge „key1“ oder die Verbindungszeichenfolge „key2“.
    - Ihren Event Hubs oder Service Bus, wechseln Sie im Azure-Portal zu dem Namespace.
        - Klicken Sie unter **Einstellungen** auf **SAS-Richtlinien**.
        - Wählen Sie die Standardrichtlinie **RootManageSharedAccessKey** aus, oder erstellen Sie eine neue.
        - Kopieren Sie die primäre oder die sekundäre Verbindungszeichenfolge.
 
6. Wählen Sie eine/n Container/Event Hub/Warteschlange oder Thema aus dem Dropdown-Listenfeld aus.

7. Geben Sie unter **Data to export** (Zu exportierende Daten) die Typen der zu exportierenden Daten an, indem Sie den Typ jeweils auf **Ein** festlegen.

6. Um den kontinuierlichen Datenexport zu aktivieren, stellen Sie sicher, dass **Datenexport** auf **Ein** festgelegt ist. Wählen Sie **Speichern** aus.

  ![Konfigurieren des fortlaufenden Datenexports](media/howto-export-data-experimental/export_list.png)

7. Nach einigen Minuten werden Ihre Daten an Ihrem ausgewählten Ziel angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt mit dem Exportieren Ihrer Daten vertraut sind, können Sie mit dem nächsten Schritt fortfahren:

> [!div class="nextstepaction"]
> [Exportieren von Daten in Azure Blob Storage](howto-export-data-blob-storage.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

> [!div class="nextstepaction"]
> [Exportieren von Daten in Azure Event Hubs und Azure Service Bus](howto-export-data-event-hubs-service-bus.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

> [!div class="nextstepaction"]
> [Visualisieren Ihrer Daten in Power BI](howto-connect-powerbi.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

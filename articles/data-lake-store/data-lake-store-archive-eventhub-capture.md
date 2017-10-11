---
title: Erfassen von Daten von Event Hubs in Azure Data Lake-Speicher | Microsoft Docs
description: Mit Azure Data Lake-Speicher zum Erfassen von Daten von Event Hubs
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: a9e69576958ae96d22a4eb03d0df429f0b307298
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Mit Azure Data Lake-Speicher zum Erfassen von Daten von Event Hubs

Erfahren Sie mehr über das Azure Data Lake-Speicher verwenden, um von Azure Event Hubs empfangenen Daten zu erfassen.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Ein Event Hubs-Namespace**. Anweisungen hierzu finden Sie unter [Erstellen eines Event Hubs-Namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Stellen Sie sicher, dass das Data Lake-Speicher-Konto und den Namespace des Event Hubs desselben Azure-Abonnements sind.


## <a name="assign-permissions-to-event-hubs"></a>Zuweisen von Berechtigungen für Event Hubs

In diesem Abschnitt erstellen Sie einen Ordner im Konto Sie die Daten von Event Hubs zu erfassen möchten. Sie weisen auch Berechtigungen für Event Hubs, damit sie Daten in ein Data Lake-Speicher-Konto schreiben kann. 

1. Öffnen Sie das Data Lake-Speicher-Konto soll zum Erfassen von Daten von Event Hubs, und klicken Sie dann auf **Data Explorer**.

    ![Data Lake-Datenspeicher-Explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake-Datenspeicher-Explorer")

2.  Klicken Sie auf **neuer Ordner** und geben Sie dann einen Namen für den Ordner, in der Sie die Daten erfassen möchten.

    ![Erstellen Sie einen neuen Ordner im Data Lake-Speicher](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "erstellen Sie einen neuen Ordner im Data Lake-Speicher")

3. Zuweisen von Berechtigungen auf der Stammebene des Data Lake-Speicher. 

    a. Klicken Sie auf **Data Explorer**, wählen Sie den Stamm des Data Lake-Speicher an, und klicken Sie dann auf **Zugriff**.

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm")

    b. Klicken Sie unter **Zugriff**, klicken Sie auf **hinzufügen**, klicken Sie auf **Benutzer oder Gruppe auswählen**, und suchen Sie nach `Microsoft.EventHubs`. 

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm")
    
    Klicken Sie auf **Auswählen**.

    c. Klicken Sie unter **Zuweisen von Berechtigungen**, klicken Sie auf **Select-Berechtigungen**. Legen Sie **Berechtigungen** auf **ausführen**. Legen Sie **hinzufügen** auf **diesen Ordner und alle untergeordneten Elemente**. Legen Sie **hinzufügen als** auf **ein Berechtigungseintrag Zugriff und eine Standard-Berechtigungseintrag**.

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Stamm")

    Klicken Sie auf **OK**.

4. Zuweisen von Berechtigungen für den Unterordner Data Lake-Speicherkonto, in dem Sie Daten erfassen möchten.

    a. Klicken Sie auf **Data Explorer**, wählen Sie den Ordner, in das Data Lake-Speicher-Konto, und klicken Sie dann auf **Zugriff**.

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner")

    b. Klicken Sie unter **Zugriff**, klicken Sie auf **hinzufügen**, klicken Sie auf **Benutzer oder Gruppe auswählen**, und suchen Sie nach `Microsoft.EventHubs`. 

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner")
    
    Klicken Sie auf **Auswählen**.

    c. Klicken Sie unter **Zuweisen von Berechtigungen**, klicken Sie auf **Select-Berechtigungen**. Legen Sie **Berechtigungen** auf **lesen, schreiben,** und **ausführen**. Legen Sie **hinzufügen** auf **diesen Ordner und alle untergeordneten Elemente**. Legen Sie schließlich **hinzufügen als** auf **ein Berechtigungseintrag Zugriff und eine Standard-Berechtigungseintrag**.

    ![Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Zuweisen von Berechtigungen für Data Lake-Speicher-Ordner")
    
    Klicken Sie auf **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Konfigurieren von Event Hubs zum Erfassen von Daten in Data Lake-Speicher

In diesem Abschnitt erstellen Sie einen Event Hub innerhalb eines Event Hubs-Namespace. Sie müssen außerdem den Event Hub zum Erfassen von Daten mit einer Azure Data Lake-Speicher-Konto konfigurieren. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits einen Event Hubs-Namespace erstellt haben.

2. Aus der **Übersicht** Bereich des Event Hubs-Namespace, klicken Sie auf **+ Event Hub**.

    ![Erstellen von Event Hubs](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Event Hub erstellen")

3. Geben Sie die folgenden Werte zum Konfigurieren von Event Hubs zum Erfassen von Daten in Data Lake-Speicher.

    ![Erstellen von Event Hubs](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Event Hub erstellen")

    a. Geben Sie einen Namen für den Event Hub.
    
    b. Legen Sie für dieses Lernprogramm **Partitionsanzahl** und **Nachrichtenbeibehaltung** auf die Standardwerte.
    
    c. Legen Sie **erfassen** auf **auf**. Legen Sie die **Zeitfenster** (wie häufig die erfassen) und **Fenstergröße** (Datengröße erfassen). 
    
    d. Für **erfassen Anbieter**Option **Azure Data Lake-Speicher** und die SELECT-Anweisung der Data Lake-Speicher Sie zuvor erstellt haben. Für **Lake Datenpfad**, geben Sie den Namen des Ordners, die Sie in das Data Lake-Speicher-Konto erstellt haben. Sie müssen nur den relativen Pfad zu dem Ordner angeben.

    e. Lassen Sie die **Beispiel Capture Namen Dateiformate** auf den Standardwert. Diese Option steuert die Ordnerstruktur, die unter dem Ordner Capture erstellt wird.

    f. Klicken Sie auf **Erstellen**.

## <a name="test-the-setup"></a>Testen Sie das setup

Sie können jetzt testen Sie die Projektmappe durch Senden von Daten an den Azure Event Hub Folgen Sie den Anweisungen am [Senden von Ereignissen an Azure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Wenn Sie anfangen, die gesendet werden, sehen Sie die Daten im Data Lake-Speicher widergespiegelt verwenden die Ordnerstruktur, die Sie angegeben haben. Beispielsweise sehen Sie eine Ordnerstruktur, wie im folgenden Screenshot ist in Ihre Data Lake-Speicher gezeigt.

![Beispiel für EventHub-Daten in Data Lake-Speicher](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Beispiel EventHub-Daten in Data Lake-Speicher")

> [!NOTE]
> Auch wenn Sie keine Nachrichten an Event Hubs schreibt Event Hubs leere Dateien mit nur den Headern in das Data Lake-Speicher-Konto. Die Dateien werden an das gleiche Zeitintervall geschrieben, dass Sie beim Erstellen des Event Hubs angegeben.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Analysieren von Daten im Data Lake-Speicher

Sobald die Daten im Data Lake-Speicher befindet, können Sie analytische Aufträge zu verarbeiten und Druck zu nehmen die Daten ausführen. Finden Sie unter [USQL Avro Beispiel](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) Anleitungen hierfür unter Verwendung von Azure Data Lake Analytics.
  

## <a name="see-also"></a>Weitere Informationen
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher](data-lake-store-copy-data-azure-storage-blob.md)

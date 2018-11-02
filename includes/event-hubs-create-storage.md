---
title: Includedatei
description: Includedatei
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: e499a0f7bec47e672c599c729a15cc3e3d04a28a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471627"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Erstellen eines Speicherkontos für den Ereignisprozessorhost
Der Ereignisprozessorhost ist ein intelligenter Agent, mit dem das Empfangen von Ereignissen von Event Hubs vereinfacht wird, indem permanente Prüfpunkte und parallele Empfangsvorgänge verwaltet werden. Für das Setzen von Prüfpunkten benötigt der Ereignisprozessorhost ein Speicherkonto. Im folgenden Beispiel wird veranschaulicht, wie Sie ein Speicherkonto erstellen und die zugehörigen Zugriffsschlüssel beziehen:

1. Wählen Sie im Azure-Portal oben links **Ressource erstellen** aus.

2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
   
    ![Speicherkonto auswählen](./media/event-hubs-create-storage/create-storage1.png)

3. Führen Sie auf der Seite **Speicherkonto erstellen** die folgenden Schritte aus: 

    1. Geben Sie einen Namen für das Speicherkonto ein. 
    2. Wählen Sie ein Azure-Abonnement, das den Event Hub enthält.
    3. Wählen Sie die Ressourcengruppe aus, die den Event Hub enthält.
    4. Wählen Sie einen Speicherort für die Erstellung der Ressource aus. 
    5. Klicken Sie dann auf **Überprüfen + erstellen**.
   
    ![Speicherkonto erstellen: Seite](./media/event-hubs-create-storage/create-storage2.png)

4. Überprüfen Sie auf der Seite **Überprüfen + erstellen** die Werte, und wählen Sie **Erstellen** aus. 

    ![Speicherkontoeinstellungen überprüfen und erstellen](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Wenn Sie die Meldung **Bereitstellungen erfolgreich** sehen, wählen Sie oben auf der Seite **Zu Ressource wechseln** aus. Sie können auch die Seite für das Speicherkonto starten, indem Sie Ihr Speicherkonto aus der Ressourcenliste auswählen.  

    ![Speicherkonto in Bereitstellung auswählen](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. Klicken Sie im Fenster **Zusammenfassung** auf **Blobs**. 

    ![Auswählen des Blobdiensts](./media/event-hubs-create-storage/select-blobs-service.png)
1. Wählen Sie oben **+ Container** aus, geben Sie einen **Namen** für den Container ein, und klicken Sie auf **OK**. 

    ![Erstellen eines Blobcontainers](./media/event-hubs-create-storage/create-blob-container.png)
1. Wählen Sie im Menü auf der linken Seite **Zugriffsschlüssel** aus, und kopieren Sie den Wert von **key1**. 

    Speichern Sie die folgenden Werte im Editor oder an einem anderen temporären Speicherort.
    - Name des Speicherkontos
    - Zugriffsschlüssel für das Speicherkonto
    - Name des Containers
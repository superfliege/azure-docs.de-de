---
title: "Batchverarbeitung von EDI-Nachrichten als Gruppe oder Sammlung – Azure Logic Apps | Microsoft-Dokumentation"
description: "Senden von EDI-Nachrichten für die Batchverarbeitung in Logik-Apps"
keywords: Batch, Batchverarbeitung, Batchcodierung
author: divswa
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: 837cb0d9595da5b5bd4f01fb4576f75e98ab8912
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Senden von X12-Nachrichten in Batches an Parteien

In B2B-Szenarien tauschen Geschäftspartner häufig Nachrichten in Gruppen oder Batches aus. Wenn Sie Nachrichten in Gruppen oder Batches an Parteien senden möchten, können Sie einen Batch mit mehreren Elementen erstellen und die Elemente dann mithilfe der X12-Batchaktion als Batch verarbeiten.


Bei der Batchverarbeitung von X12-Nachrichten kommen genau wie bei anderen Nachrichten ein Batchtrigger und eine Batchaktion zum Einsatz. Bei X12 durchläuft der Batch vor der Übermittlung an den Partner oder an ein anderes Ziel außerdem den X12-Codierschritt. Weitere Informationen zum Batchtrigger und zur Batchaktion finden Sie unter [Versand, Empfang und Batchverarbeitung von Nachrichten in Logik-Apps](logic-apps-batch-process-send-receive-messages.md).

In diesem Thema erfahren Sie, wie Sie mithilfe folgender Aufgaben X12-Nachrichten als Batch verarbeiten können:
* [Erstellen einer Logik-App, die Elemente empfängt und einen Batch erstellt.](#receiver) Diese als Empfänger fungierende Logik-App führt die folgenden Aktionen aus:
 
   * Sie gibt den Batchnamen und die Freigabekriterien an, die erfüllt sein müssen, damit die Elemente als Batch freigegeben werden.

   * Sie verarbeitet oder codiert die Elemente im Batch unter Verwendung der angegebenen X12-Vereinbarung oder Partneridentitäten.

* [Erstellen einer Logik-App, die Elemente an einen Batch sendet.](#sender) Diese als Absender fungierende Logik-App gibt an, wohin Elemente für die Batchverarbeitung gesendet werden sollen. Bei dem Ziel muss es sich um eine vorhandene Logik-App für den Empfang handeln.


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md)

* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die Sie in Ihrem Integrationskonto definiert haben. Vergewissern Sie sich, dass jeder Partner in seinen Eigenschaften den X12-Qualifizierer (Standard Carrier Alpha Code) als Geschäftsidentität verwendet.

* Eine bereits in Ihrem Integrationskonto definierte [X12-Vereinbarung](logic-apps-enterprise-integration-x12.md).

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Erstellen einer Logik-App, die X12-Nachrichten empfängt und einen Batch erstellt

Um Nachrichten an einen Batch senden zu können, müssen Sie mit dem Trigger **Batch** zunächst eine Logik-App für den Empfang erstellen. Dadurch können Sie die Logik-App für den Empfang auswählen, wenn Sie die Logik-App für den Versand erstellen. Für den Empfänger werden der Batchname, die Freigabekriterien, die X12-Vereinbarung und weitere Einstellungen angegeben. 


1. Erstellen Sie über das [Azure-Portal](https://portal.azure.com) eine Logik-App namens „BatchX12Messages“.

2. Fügen Sie im Designer für Logik-Apps den Trigger **Batch** hinzu, um den Workflow Ihrer Logik-App zu starten. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. Wählen Sie den Trigger **Batch – Batchnachrichten** aus.

   ![Hinzufügen des Triggers „Batch“](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

3. Geben Sie einen Namen und Freigabekriterien für den Batch an:

   * **Batchname**: Der Name zum Identifizieren des Batchs („TestBatch“ in diesem Beispiel).

   * **Freigabekriterien**: Die Batchfreigabekriterien. Diese können auf der Nachrichtenanzahl, auf dem Zeitplan oder auf beidem basieren.
   
     ![Angeben von Details für den Batchtrigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **Nachrichtenanzahl**: Die Anzahl von Nachrichten, die in einem Batch gesammelt werden sollen, bevor er zur Verarbeitung freigegeben wird („5“ in diesem Beispiel).

     ![Angeben von Details für den Batchtrigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Zeitplan**: Der Batchfreigabezeitplan für die Verarbeitung („alle zehn Minuten“ in diesem Beispiel).

     ![Angeben von Details für den Batchtrigger](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


4. Fügen Sie eine weitere Aktion hinzu, die die Gruppen- oder Batchnachrichten codiert und eine X12-Batchnachricht erstellt. 

   a. Klicken Sie auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   b. Geben Sie im Suchfeld die Zeichenfolge „X12 batch“ als Filter ein, und wählen Sie eine Aktion für **X12 - Batch Encode** (X12 – Batchcodierung) aus. Genau wie beim X12-Codierungsconnector sind auch für die Batchcodierungsaktion verschiedene Varianten möglich. Sie können eine beliebige Option auswählen.

   ![Auswählen der X12-Codierungsaktion](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
5. Legen Sie die Eigenschaften für die soeben hinzugefügte Aktion fest.

   * Wählen Sie im Feld **Name der X12-Vereinbarung** die Vereinbarung aus der Dropdownliste aus. Sollte die Liste leer sein, vergewissern Sie sich, dass Sie eine Verbindung mit Ihrem Integrationskonto erstellt haben.

   * Wählen Sie im Feld **BatchName** das Feld **Batchname** aus der Liste mit den dynamischen Inhalten aus.
   
   * Wählen Sie im Feld **PartitionName** das Feld **Partitionsname** aus der Liste mit den dynamischen Inhalten aus.

   * Wählen Sie im Feld **Elemente** die Option **Elemente im Batch** aus der Liste mit den dynamischen Inhalten aus.

   ![Batchcodierungsaktion – Details](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

6. Fügen Sie zu Testzwecken eine HTTP-Aktion zum Senden der Batchnachricht an den [RequestBin-Dienst](https://requestbin.fullcontact.com/) hinzu. 

   1. Geben Sie im Suchfeld den Begriff „HTTP“ als Filter ein. Wählen Sie die folgende Aktion aus: **HTTP - HTTP**
    
      ![Auswählen der HTTP-Aktion](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   2. Wählen Sie in der Liste **Methode** die Option **POST** aus. Generieren Sie für das Feld **URI** einen URI für Ihre RequestBin-Instanz, und geben Sie ihn ein. Wählen Sie im Feld **Text** aus der Liste mit den dynamischen Inhalten unter dem Abschnitt **Nach Vereinbarungsname im Batch codieren** das Feld **Text** aus. Sollte **Text** nicht angezeigt werden, klicken Sie neben **Nach Vereinbarungsname im Batch codieren** auf **See more** (Mehr anzeigen).

      ![Angeben von Details für die HTTP-Aktion](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

7.  Sie haben nun eine Logik-App für den Empfang erstellt. Speichern Sie Ihre Logik-App.

    ![Speichern Ihrer Logik-App](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Eine Partition hat einen Grenzwert von 5.000 Nachrichten oder 80 MB. Wenn eine Bedingung erfüllt ist, kann der Batch freigegeben werden, auch wenn die benutzerdefinierte Bedingung nicht erfüllt ist.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>Erstellen einer Logik-App, die X12-Nachrichten an einen Batch sendet

Erstellen Sie als Nächstes mindestens eine Logik-App, die Elemente an den Batch sendet, der durch die Logik-App für den Empfang definiert wird. Geben Sie für den Absender die Logik-App für den Empfang und den Batchnamen, den Nachrichteninhalt sowie ggf. weitere Einstellungen an. Optional können Sie einen eindeutigen Partitionsschlüssel angeben, um den Batch zu unterteilen und Elemente mit dem entsprechenden Schlüssel zu sammeln.

Logik-Apps für den Versand müssen wissen, wohin die Elemente gesendet werden sollen. Logik-Apps für den Empfang benötigen hingegen keinerlei Absenderinformationen.


1. Erstellen Sie eine weitere Logik-App, und nennen Sie sie „X12MessageSender“. Fügen Sie Ihrer Logik-App den folgenden Trigger hinzu: **Anforderung/Antwort – Anforderung** 
   
   ![Hinzufügen des Anforderungstriggers](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

2. Fügen Sie einen neuen Schritt hinzu, um Nachrichten an einen Batch zu senden.

   1. Klicken Sie auf **+ Neuer Schritt** > **Aktion hinzufügen**.

   2. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. 

3. Wählen Sie die Aktion **Nachrichten an Batch senden – Logik-App-Workflow mit Batchtrigger auswählen** aus.

   ![Auswählen von „Nachrichten an Batch senden“](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

4. Wählen Sie nun die zuvor erstellte Logik-App „BatchX12Messages“ aus, die jetzt als Aktion angezeigt wird.

   ![Auswählen der Logik-App „BatchReceiver“](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > Die Liste enthält außerdem weitere Logik-Apps mit Batchtrigger.

5. Legen Sie die Batcheigenschaften fest.

   * **Batchname**: Der von der Logik-App für den Empfang definierte Batchname. Dieser wird zur Laufzeit überprüft und lautet in diesem Beispiel „TestBatch“.

     > [!IMPORTANT]
     > Lassen Sie den Batchnamen unverändert. Er muss dem Batchnamen entsprechen, der durch die Logik-App für den Empfang angegeben wird.
     > Wenn Sie den Batchnamen ändern, funktioniert die Logik-App für den Versand nicht.

   * **Nachrichteninhalt**: Der an den Batch zu sendende Nachrichteninhalt.
   
   ![Festlegen der Batcheigenschaften](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

6. Speichern Sie Ihre Logik-App. Ihre Logik-App für den Versand sieht nun in etwa wie folgt aus:

   ![Speichern Ihrer Logik-App für den Versand](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Testen Ihrer Logik-Apps

Veröffentlichen Sie X12-Nachrichten über [Postman](https://www.getpostman.com/postman) oder ein ähnliches Tool für Ihre als Absender fungierende Logik-App, um Ihre Batchlösung zu testen. Daraufhin sollten in Kürze in Ihrer RequestBin-Instanz X12-Nachrichten eingehen (entweder als Batch mit fünf Elementen oder alle zehn Minuten), die alle den gleichen Partitionsschlüssel besitzen.

## <a name="next-steps"></a>Nächste Schritte

* [Versand, Empfang und Batchverarbeitung von Nachrichten in Logik-Apps](logic-apps-batch-process-send-receive-messages.md) 
* [Erstellen einer serverlosen App mit Azure Logic Apps und Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

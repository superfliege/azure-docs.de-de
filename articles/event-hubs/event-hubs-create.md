---
title: Erstellen eines Azure Event Hubs | Microsoft-Dokumentation
description: Erstellen eines Azure Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: e444c4505d4744c95e08c4ef0d33566356785c81
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals

## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace
1. Melden Sie sich beim [Azure-Portal][Azure portal] an, und klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen**.
1. Klicken Sie auf **Internet der Dinge**, und klicken Sie dann auf **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. Geben Sie unter **Namespace erstellen** einen Namen für den Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Ist der Name verfügbar, wählen Sie den Tarif („Basic“ oder „Standard“) aus. Wählen Sie außerdem ein Azure-Abonnement, eine Ressourcengruppe und einen Standort, an dem die Ressource erstellt werden soll. 
1. Klicken Sie auf **Erstellen** , um den Namespace zu erstellen. Möglicherweise müssen Sie ein paar Minuten warten, bis das System die Ressourcen vollständig bereitgestellt hat.
2. Klicken Sie in der Portalliste mit den Namespaces auf den neu erstellten Namespace.
2. Klicken Sie auf **SAS-Richtlinien** und dann auf **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Klicken Sie auf die Kopierschaltfläche, um die Verbindungszeichenfolge **RootManageSharedAccessKey** in die Zwischenablage zu kopieren. Speichern Sie diese Verbindungszeichenfolge zur späteren Verwendung in einem temporären Speicherort, z.B. in Editor.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

1. Klicken Sie in der Liste der Event Hubs-Namespaces auf den neu erstellten Namespace.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Klicken Sie auf dem Blatt mit den Namespaces auf **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. Klicken Sie oben auf dem Blatt auf **Event Hub hinzufügen**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Geben Sie einen Namen für den Event Hub ein, und klicken Sie auf **Erstellen**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Ihr Event Hub wird jetzt erstellt, und Sie verfügen über die zum Senden und Empfangen von Ereignissen erforderlichen Verbindungszeichenfolgen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter diesen Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Übersicht über die Event Hubs-API](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/
---
title: Includedatei
description: Includedatei
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: c718fa972b9d6cba13a13edd3c7e8df29fa63207
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33835761"
---
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Ressourcen erstellen** > **Web + Mobil** > **Notification Hub**.
   
      ![Azure-Portal – Erstellen von Notification Hubs](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Geben Sie im Feld **Notification Hub** einen eindeutigen Namen ein. Wählen Sie **Region**, **Abonnement** und **Ressourcengruppe** (sofern bereits vorhanden). 
   
      Wenn Sie noch nicht über einen Service Bus-Namespace verfügen, können Sie den Standardnamen verwenden, der basierend auf dem Hub-Namen erstellt wird (sofern der Namespace-Name verfügbar ist).
    
      Gehen Sie wie folgt vor, wenn Sie bereits über einen Service Bus-Namespace verfügen, in dem Sie den Hub erstellen möchten:

    a. Wählen Sie im Bereich **Namespace** den Link **Vorhandene auswählen**. 
   
    b. Klicken Sie auf **Erstellen**.
   
      ![Azure-Portal – Festlegen von Eigenschaften für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Wählen Sie **Benachrichtigungen** (Glockensymbol) und **Zu Ressource wechseln** aus. 

      ![Azure-Portal – Benachrichtigungen -> Zu Ressource wechseln](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
5. Wählen Sie in der Liste die Option **Zugriffsrichtlinien** aus. Notieren Sie sich die beiden Verbindungszeichenfolgen, die für Sie verfügbar sind. Sie werden später für die Behandlung von Pushbenachrichtigungen benötigt.

      >[!IMPORTANT]
      >Verwenden Sie **NICHT** „DefaultFullSharedAccessSignature“ in Ihrer Anwendung. Diese ist nur für die Verwendung in Ihrem Back-End vorgesehen.
      >
   
      ![Azure-Portal – Verbindungszeichenfolgen für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)


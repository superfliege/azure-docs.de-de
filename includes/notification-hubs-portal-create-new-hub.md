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
ms.openlocfilehash: 19ea9c749b58f6f81dc2087caa77573062d883b5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39485789"
---
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Ressourcen erstellen** > **Mobil** > **Notification Hub**.
   
      ![Azure-Portal – Erstellen von Notification Hubs](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
1. Geben Sie im Feld **Notification Hub** einen eindeutigen Namen ein. Wählen Sie **Region**, **Abonnement** und **Ressourcengruppe** (sofern bereits vorhanden). 
   
      Wenn Sie noch nicht über einen Service Bus-Namespace verfügen, können Sie den Standardnamen verwenden, der basierend auf dem Hub-Namen erstellt wird (sofern der Namespace-Name verfügbar ist).
    
      Gehen Sie wie folgt vor, wenn Sie bereits über einen Service Bus-Namespace verfügen, in dem Sie den Hub erstellen möchten:

    a. Wählen Sie im Bereich **Namespace** den Link **Vorhandene auswählen**. 
   
    b. Klicken Sie auf **Erstellen**.
   
      ![Azure-Portal – Festlegen von Eigenschaften für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. Wählen Sie **Benachrichtigungen** (Glockensymbol) und **Zu Ressource wechseln** aus. 

      ![Azure-Portal – Benachrichtigungen -> Zu Ressource wechseln](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
1. Wählen Sie in der Liste die Option **Zugriffsrichtlinien** aus. Notieren Sie sich die beiden Verbindungszeichenfolgen, die für Sie verfügbar sind. Sie werden später für die Behandlung von Pushbenachrichtigungen benötigt.

      >[!IMPORTANT]
      >Verwenden Sie **NICHT** „DefaultFullSharedAccessSignature“ in Ihrer Anwendung. Diese ist nur für die Verwendung in Ihrem Back-End vorgesehen.
      >
   
      ![Azure-Portal – Verbindungszeichenfolgen für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)


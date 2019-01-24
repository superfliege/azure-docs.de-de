---
title: Includedatei
description: Includedatei
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: b68fa345d4772134c30ce8b8b559f98113a0496f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453098"
---
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Ressourcen erstellen** > **Mobil** > **Notification Hub**.

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

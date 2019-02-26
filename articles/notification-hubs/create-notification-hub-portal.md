---
title: Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal eine Azure Notification Hub-Instanz erstellen.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 62e72f27e48f7bf220901f4eb36090f926724a2a
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313989"
---
# <a name="create-an-azure-notification-hub-in-the-azure-portal"></a>Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal 
Azure Notification Hubs bietet ein benutzerfreundliches, horizontal skalierbares Pushmodul, mit dem Sie von einem beliebigen Back-End (cloudbasiert oder lokal) Benachrichtigungen an sämtliche Plattformen (iOS, Android, Windows, Kindle, Baidu usw.) senden können. Weitere Informationen zu diesem Dienst finden Sie unter [Was ist Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In dieser Schnellstartanleitung erstellen Sie einen Notification Hub über das Azure-Portal. Im ersten Abschnitt erfahren Sie, wie Sie einen Notification Hubs-Namespace und einen Hub in diesem Namespace erstellen. Im zweiten Abschnitt erfahren Sie, wie Sie einen Notification Hub in einem bereits vorhandenen Notification Hubs-Namespace erstellen. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Erstellen eines Namespace und eines Notification Hubs
In diesem Abschnitt erstellen Sie einen Namespace und einem Hub in dem Namespace. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Erstellen eines Notification Hubs in einem vorhandenen Namespace
In diesem Abschnitt erstellen Sie einen Notification Hub in einem vorhandenen Namespace. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Menü die Option **Alle Dienste** aus, suchen Sie nach **Notification Hub**, und wählen Sie neben **Notification Hub-Namespaces** den **Stern** (`*`) aus, um die Option dem Abschnitt **FAVORITEN** des linken Menüs hinzuzufügen. Wählen Sie **Notification Hub-Namespaces** aus. 

      ![Azure-Portal: Auswählen von „Notification Hub-Namespaces“](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Wählen Sie in der Liste auf der Seite **Notification Hub-Namespaces** Ihren Namespace aus. 

      ![Auswählen Ihres Namespace in der Liste](./media/create-notification-hub-portal/select-namespace.png)
1. Wählen Sie auf der Symbolleiste der Seite **Notification Hub-Namespaces** die Option **Hub hinzufügen** aus. 

      ![Notification Hub-Namespaces: Schaltfläche „Hub hinzufügen“](./media/create-notification-hub-portal/add-hub-button.png)
4. Geben Sie auf der Seite **Neuer Notification Hub** einen Namen für den Notification Hub ein, und wählen Sie **OK** aus.

      ![Seite „Neuer Notification Hub“: Eingeben eines Hub-Namens](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. Wählen Sie oben **Benachrichtigungen** (Glockensymbol) aus, um den Status der Bereitstellung des neuen Hubs anzuzeigen. Wählen Sie in der rechten oberen Ecke **X** aus, um das Benachrichtigungsfenster zu schließen. 

      ![Bereitstellungsbenachrichtigung](./media/create-notification-hub-portal/deployment-notification.png)
5. Aktualisieren Sie die Webseite **Notification Hub-Namespaces**, damit Ihr neuer Hub in der Liste angezeigt wird. 

      ![Azure-Portal – Benachrichtigungen -> Zu Ressource wechseln](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Wählen Sie Ihren **Notification Hub** aus, um dessen Startseite anzuzeigen. 

      ![Azure-Portal – Benachrichtigungen -> Zu Ressource wechseln](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie einen Notification Hub erstellt. Unter [Konfigurieren von Azure Notification Hubs mit Einstellungen des Plattformbenachrichtigungssystems über das Azure-Portal](configure-notification-hub-portal-pns-settings.md) erfahren Sie, wie Sie den Hub mit Einstellungen des Plattformbenachrichtigungssystems (Platform Notification System, PNS) konfigurieren. 
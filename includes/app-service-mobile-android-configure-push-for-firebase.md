---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: bb03e4b5b04a0272d8fa9b032da5adb50878b620
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809878"
---
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Alle durchsuchen** > **App Services** und anschließend auf Ihr Mobile Apps-Back-End. Klicken Sie unter **Einstellungen** auf **App Service Push** (App Service-Push) und dann auf den Namen Ihres Benachrichtigungs-Hubs.
2. Wechseln Sie zu **Google (GCM)**, geben den **Serverschlüssel** ein, den Sie im vorherigen Verfahren von Firebase erhalten haben, und klicken dann auf **Speichern**.

    ![Festlegen des API-Schlüssels im Portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Das Mobile Apps-Back-End ist nun für die Verwendung von Firebase Cloud Messaging konfiguriert. Dadurch können Sie Pushbenachrichtigungen über den Benachrichtigungs-Hub an Ihre auf einem Android-Gerät ausgeführte App senden.

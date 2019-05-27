---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140301"
---
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Alle durchsuchen** > **App Services** und anschließend auf Ihr Mobile Apps-Back-End. Klicken Sie unter **Einstellungen** auf **App Service Push** (App Service-Push) und dann auf den Namen Ihres Benachrichtigungs-Hubs.
2. Wechseln Sie zu **Google (GCM)** , geben den **Serverschlüssel** ein, den Sie im vorherigen Verfahren von Firebase erhalten haben, und klicken dann auf **Speichern**.

    ![Festlegen des API-Schlüssels im Portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Das Mobile Apps-Back-End ist nun für die Verwendung von Firebase Cloud Messaging konfiguriert. Dadurch können Sie Pushbenachrichtigungen über den Benachrichtigungs-Hub an Ihre auf einem Android-Gerät ausgeführte App senden.

---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 79459be30a5a2018dc82486a84895b1a954941bc
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133891"
---
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Alle durchsuchen** > **App Services** aus. Wählen Sie dann Ihr Mobile Apps-Back-End aus. Wählen Sie unter **Einstellungen** die Option **App Service Push** aus. Wählen Sie dann den Namen Ihres Notification Hub aus.
2. Wechseln Sie zu **Windows (WNS)**. Geben Sie nun den **Sicherheitsschlüssel** (Clientgeheimnis) und die **Paket-SID** von der Live Services-Website ein. Wählen Sie dann **Speichern** aus.

    ![Festlegen des WNS-Schlüssels im Portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Das Back-End ist jetzt für die Verwendung von WNS zum Senden von Pushbenachrichtigungen konfiguriert.

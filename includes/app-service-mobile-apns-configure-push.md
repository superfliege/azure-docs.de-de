---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 25bb5cfb0073f7533faddec43b38fb5031b82a43
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809781"
---
1. Starten Sie auf dem Mac **Keychain Access**. Öffnen Sie auf der linken Navigationsleiste unter **Kategorie** den Eintrag **Meine Zertifikate**. Suchen Sie das SSL-Zertifikat, das Sie im vorigen Abschnitt heruntergeladen haben, und legen Sie den Inhalt offen. Wählen Sie nur das Zertifikat (ohne den privaten Schlüssel) aus. [Exportieren](https://support.apple.com/kb/PH20122?locale=en_US) Sie es dann.
2. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Alle durchsuchen** > **App Services** aus. Wählen Sie dann Ihr Mobile Apps-Back-End aus. 
3. Wählen Sie unter **Einstellungen** die Option **App Service Push** aus. Wählen Sie dann den Namen Ihres Notification Hub aus. 
4. Wechseln Sie zu **Apple Push Notification Services** > **Zertifikat hochladen**. Laden Sie die P12-Datei hoch. Wählen Sie dabei den richtigen **Modus** aus (abhängig davon, ob Ihr Client-SSL-Zertifikat für die Produktion oder für den Sandkasten vorgesehen ist). Speichern Sie alle Änderungen.

Ihr Dienst ist jetzt so konfiguriert, dass er mit Pushbenachrichtigungen unter iOS arbeitet.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png

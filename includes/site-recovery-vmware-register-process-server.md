---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: e18d0a6a01a86f844edc213fc95003cf4f4b46c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165555"
---
* Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Prozessservercomputer her.
* Sie können „cspsconfigtool.exe“ starten, indem Sie auf dem Desktop auf die Verknüpfung klicken. (Das Tool wird automatisch gestartet, wenn Sie sich zum ersten Mal beim Prozessserver anmelden.)
  - Vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) oder IP-Adresse des Konfigurationsservers
  - Port, an dem der Konfigurationsserver lauscht. Der Wert sollte „443“ lauten.
  - Verbindungspassphrase zum Herstellen der Verbindung mit dem Konfigurationsserver
  - Zu konfigurierender Datenübertragungsport für diesen Prozessserver. Behalten Sie den Standardwert bei (es sei denn, Sie haben die Portnummer in Ihrer Umgebung geändert).

    ![Registrieren des Prozessservers](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Klicken Sie auf die Schaltfläche „Speichern“, um die Konfiguration zu speichern und den Prozessserver zu registrieren.

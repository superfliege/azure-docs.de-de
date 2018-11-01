---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: e743128932f6a136820819c21e0386eb678c4203
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166131"
---
<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Vorbereitung auf Updates
Sie müssen die folgenden Schritte ausführen, bevor Sie das Update überprüfen und anwenden können:

1. Erstellen Sie eine Cloudmomentaufnahme der Gerätedaten.
2. Stellen Sie sicher, dass die festen Controller-IP-Adressen routingfähig sind und eine Verbindung mit dem Internet herstellen können. Diese festen IPs werden zum Anwenden von Updates auf Ihr Gerät verwendet. Sie können die Konnektivität testen, indem Sie von der Windows PowerShell-Schnittstelle des Geräts aus auf jedem Controller das folgende Cmdlet ausführen:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Beispielausgabe für die Testverbindung, wenn feste IP-Adressen eine Verbindung mit dem Internet herstellen können**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Nachdem Sie diese manuellen Vorabprüfungen erfolgreich abgeschlossen haben, können Sie mit dem Überprüfen und Installieren der Updates beginnen.


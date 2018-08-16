---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 08/06/2018
ms.author: ramamill
ms.openlocfilehash: 81390d38b4c0c38b7ac6883ae2bf18c64542fa00
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582885"
---
Die Schritte zum Aufheben der Registrierung eines Prozessservers sind abhängig vom Status der Verbindung mit dem Konfigurationsserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Aufheben der Registrierung eines verbundenen Prozessservers

1. Stellen Sie als Administrator eine Remoteverbindung mit dem Prozessserver her.
2. Starten Sie die **Systemsteuerung**, und öffnen Sie **Programme > Programm deinstallieren**.
3. Deinstallieren Sie das Programm **Microsoft Azure Site Recovery Configuration/Process Server** (Microsoft Azure Site Recovery-Konfiguration/Prozessserver).
4. Nach Abschluss von Schritt 3 können Sie **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** (Microsoft Azure Site Recovery-Konfiguration/Prozessserverabhängigkeiten) deinstallieren.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Aufheben der Registrierung eines nicht verbundenen Prozessservers

> [!WARNING]
> Gehen Sie wie folgt vor, falls sich der virtuelle Computer, auf dem der Prozessserver installiert wurde, nicht reaktivieren lässt:

1. Melden Sie sich als Administrator bei Ihrem Konfigurationsserver an.
2. Öffnen Sie eine Administratoreingabeaufforderung, und navigieren Sie zum Verzeichnis `%ProgramData%\ASR\home\svsystems\bin`.
3. Führen Sie nun den folgenden Befehl aus:

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Mit diesem Befehl wird wie nachfolgend gezeigt die Liste der Prozessserver (mehrere Einträge möglich, falls doppelte Einträge vorhanden) mit Seriennummer (Nr.), IP-Adresse (IP), Name des virtuellen Computers mit dem bereitgestellten Prozessserver (Name) und Heartbeat des virtuellen Computers (Heartbeat) ausgegeben.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Geben Sie nun die Seriennummer des Prozessservers ein, dessen Registrierung aufgehoben werden soll.
6. Dadurch werden die Details zum Prozessservers endgültig aus dem System gelöscht, und die folgende Meldung wird angezeigt: **Successfully unregistered server-name> (server-IP-address)** (Die Registrierung des Servers <Name> (Server-IP-Adresse) wurde aufgehoben.).


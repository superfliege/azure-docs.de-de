---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0d090f43b69b42a07f1c8949d1662e8e720f3cf4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57908534"
---
Die Schritte zum Aufheben der Registrierung eines Prozessservers sind abhängig vom Status der Verbindung mit dem Konfigurationsserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Aufheben der Registrierung eines verbundenen Prozessservers

1. Stellen Sie als Administrator eine Remoteverbindung mit dem Prozessserver her.
2. Starten Sie die **Systemsteuerung**, und öffnen Sie **Programme > Programm deinstallieren**.
3. Deinstallieren Sie ein Programm namens **Microsoft Azure Site Recovery-Mobilitätsdienst/Masterzielserver**.
4. Deinstallieren Sie das Programm **Microsoft Azure Site Recovery-Konfiguration/Prozessserver**.
5. Nachdem Sie die Programme in den Schritten 3 und 4 deinstalliert haben, können Sie **Microsoft Azure Site Recovery-Konfiguration/Prozessserverabhängigkeiten** deinstallieren.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Aufheben der Registrierung eines nicht verbundenen Prozessservers

> [!WARNING]
> Führen Sie die unten stehenden Schritte aus, falls sich der virtuelle Computer, auf dem der Prozessserver installiert wurde, nicht reaktivieren lässt.

1. Melden Sie sich als Administrator bei Ihrem Konfigurationsserver an.
2. Öffnen Sie eine Administratoreingabeaufforderung, und navigieren Sie zum Verzeichnis `%ProgramData%\ASR\home\svsystems\bin`.
3. Führen Sie nun den folgenden Befehl aus:

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Mit diesem Befehl wird wie nachfolgend gezeigt die Liste der Prozessserver (mehrere Einträge möglich, falls doppelte Einträge vorhanden) mit Seriennummer (Nr.), IP-Adresse (IP), Name des virtuellen Computers mit dem bereitgestellten Prozessserver (Name) und Heartbeat des virtuellen Computers (Heartbeat) ausgegeben.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Geben Sie nun die Seriennummer des Prozessservers ein, dessen Registrierung aufgehoben werden soll.
6. Dadurch werden die Details des Prozessservers aus dem System entfernt, und die folgende Meldung wird angezeigt: **Servername> (Server-IP-Adresse) wurde erfolgreich deinstalliert**


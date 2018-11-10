---
title: Remotedesktopdienste für eine Azure-VM werden nicht gestartet | Microsoft-Dokumentation
description: Informationen zur Problembehandlung für die Remotedesktopdienste bei der Verbindungsherstellung mit einer VM |Microsoft-Dokumentation
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: a9967aec61aaab5bc6b4517407f36e2a6c7342c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238861"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Remotedesktopdienste für eine Azure-VM werden nicht gestartet

In diesem Artikel wird beschrieben, wie Sie Probleme bei der Verbindungsherstellung mit einem virtuellen Azure-Computer (VM) beheben, wenn die Remotedesktopdienste (TermService) nicht starten oder ein Startfehler auftritt.

>[!NOTE]
>Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel behandelt die Verwendung des Resource Manager-Bereitstellungsmodells. Es wird empfohlen, dieses Modell anstelle des klassischen Bereitstellungsmodells für neue Bereitstellungen zu verwenden.

## <a name="symptoms"></a>Symptome

Wenn Sie versuchen, eine Verbindung mit einer VM herzustellen, beobachten Sie dieses Verhalten:

- Der VM-Screenshot zeigt, dass das Betriebssystem vollständig geladen ist und auf Anmeldeinformationen wartet.

    ![Screenshot des VM-Status](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Sie zeigen die Ereignisprotokolle im virtuellen Computer per Remotezugriff über die Ereignisanzeige an. Sie können sehen, dass die Remotedesktopdienste (TermServ) nicht gestartet werden oder dass beim Start ein Fehler auftritt. Es folgt ein Beispielprotokoll:

    **Protokollname:**      System </br>
    **Quelle:**        Dienststeuerungs-Manager </br>
    **Datum:**          12/16/2017 11:19:36 AM</br>
    **Ereignis-ID:**      7022</br>
    **Aufgabenkategorie:** Keine</br>
    **Ebene:**         Fehler</br>
    **Schlüsselwörter:**      Klassisch</br>
    **Benutzer:**          N/V</br>
    **Computer:**      vm.contoso.com</br>
    **Beschreibung:** Remotedesktopdienste wurden nicht ordnungsgemäß gestartet. 

    Sie können diese Fehler auch über die serielle Zugriffskonsole suchen. Verwenden Sie dazu die folgende Abfrage: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Ursache
 
Dieses Problem tritt auf, weil die Remotedesktopdienste auf dem virtuellen Computer nicht ausgeführt werden. Dies kann folgende Ursachen haben: 

- Der TermService-Dienst ist auf **Deaktiviert** festgelegt. 
- Der TermService-Dienst ist abgestürzt oder reagiert nicht. 

## <a name="solution"></a>Lösung

Verwenden Sie zum Beheben dieses Problems die serielle Konsole, oder [reparieren Sie den virtuellen Computer offline](#repair-the-vm-offline), indem Sie den Betriebssystemdatenträger des virtuellen Computers an eine Wiederherstellungs-VM anfügen.

### <a name="use-serial-console"></a>Verwenden der seriellen Konsole

1. Greifen Sie auf die [serielle Konsole](serial-console-windows.md) zu, indem Sie **Support und Problembehandlung** > **Serielle Konsole** auswählen. Wenn dieses Feature auf der VM aktiviert ist, können Sie erfolgreich eine Verbindung mit der VM herstellen.

2. Erstellen Sie einen neuen Kanal für eine CMD-Instanz. Geben Sie **CMD** ein, um den Kanal zu starten und den Kanalnamen abzurufen.

3. Wechseln Sie zu dem Kanal, in dem die CMD-Instanz ausgeführt wird. In diesem Fall ist es Kanal 1.

   ```
   ch -si 1
   ```

4. Drücken Sie erneut die **EINGABETASTE**, und geben Sie einen gültigen Benutzernamen und ein Kennwort (lokal oder Domänen-ID) für den virtuellen Computer ein.

5. Fragen Sie den Status des TermService-Diensts ab.

   ```
   sc query TermService
   ```

6. Wenn als Dienststatus **Beendet** angezeigt wird, versuchen Sie, den Dienst zu starten.

    ```
    sc start TermService
     ``` 

7. Fragen Sie den Dienst erneut ab, um sicherzustellen, dass der Dienst erfolgreich gestartet wurde.

   ```
   sc query TermService
   ```
    Wenn der Dienst nicht gestartet wird, befolgen Sie je nach der erhaltenen Fehlermeldung die entsprechende Anweisung zur Problembehandlung:

    |  Error |  Vorschlag |
    |---|---|
    |5 – ACCESS DENIED |Siehe [TermService-Dienst wurde aufgrund eines „Access Denied“-Fehlers beendet](#termService-service-is-stopped-because-of-access-denied-error). |
    |1058 – ERROR_SERVICE_DISABLED  |Siehe [TermService-Dienst ist deaktiviert](#termService-service-is-disabled).  |
    |1059 – ERROR_CIRCULAR_DEPENDENCY |[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben.|
    |1068 – ERROR_SERVICE_DEPENDENCY_FAIL|[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben.|
    |1069 – ERROR_SERVICE_LOGON_FAILED  |[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben.    |
    |1070 – ERROR_SERVICE_START_HANG   | [Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben.  |
    |1077 – ERROR_SERVICE_NEVER_STARTED   | Siehe [TermService-Dienst ist deaktiviert](#termService-service-is-disabled).  |
    |1079 – ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben. |
    |1753   |[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu beheben.   |

#### <a name="termservice-service-is-stopped-because-of-access-denied-error"></a>TermService-Dienst wurde aufgrund eines „Access Denied“-Fehlers beendet

1. Stellen Sie eine Verbindung mit der [seriellen Konsole](serial-console-windows.md#) her, und öffnen Sie eine PowerShell-Instanz.
2. Laden Sie das Tool Process Monitor herunter, indem Sie das folgende Skript ausführen:

        remove-module psreadline  
        $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
        $destination = "c:\temp\ProcessMonitor.zip" 
        $wc = New-Object System.Net.WebClient 
        $wc.DownloadFile($source,$destination) 
3. Starten Sie dann eine procmon-Ablaufverfolgung:

        procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
4. Reproduzieren Sie das Problem, indem Sie den Dienst starten, der die Zugriffsverweigerung ausgibt: 

        sc start TermService 
        
    Wenn der Fehler auftritt, beenden Sie die Process Monitor-Ablaufverfolgung:

        procmon /Terminate 
5. Sammeln Sie die Datei  **c:\temp\ProcMonTrace.PML**, öffnen Sie sie über procmon, und filtern Sie sie wie im folgenden Screenshot gezeigt nach  **Result is ACCESS DENIED**:

    ![Filtern nach Ergebnis in Process Monitor](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Beheben Sie Probleme mit den Registrierungsschlüsseln, Ordnern oder Dateien, die in der Ausgabe aufgeführt sind. Normalerweise wird dieses Problem durch das im Dienst verwendete Anmeldekonto verursacht, das über keine ACL-Berechtigung für den Zugriff auf diese Objekte verfügt. Nach der entsprechenden ACL-Berechtigung für das Anmeldekonto können Sie in einem fehlerfreien virtuellen Computer suchen. 

#### <a name="termservice-service-is-disabled"></a>TermService-Dienst ist deaktiviert

1.  Stellen Sie den Standardstartwert des Diensts wieder her:

        sc config TermService start= demand 
        
2.  Starten Sie den Dienst:

        sc start TermService 
3.  Fragen Sie den Status des Diensts erneut ab, um sicherzustellen, dass der Dienst ausgeführt wird:     sc query TermService 
4.  Versuchen Sie, über Remotedesktop eine Verbindung mit dem virtuellen Computer herzustellen.


### <a name="repair-the-vm-offline"></a>Reparieren des virtuellen Computers im Offlinestatus

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Anfügen des Betriebssystemdatenträgers an eine VM für die Wiederherstellung

1. [Fügen Sie den Betriebssystemdatenträger an einen virtuellen Computer für die Wiederherstellung an](../windows/troubleshoot-recovery-disks-portal.md).
2. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Wiederherstellungscomputer her. Stellen Sie sicher, dass der angefügte Datenträger in der Datenträgerverwaltungskonsole als **Online** gekennzeichnet ist. Beachten Sie den Laufwerkbuchstaben, der dem angefügten Betriebssystemdatenträger zugewiesen ist.
3.  Öffnen Sie eine Instanz der Eingabeaufforderung mit erhöhten Rechten (**Als Administrator ausführen**), und führen Sie dann das folgende Skript aus. Es wird davon ausgegangen, dass der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe „F“ lautet. Ersetzen Sie ihn durch den entsprechenden Wert Ihrer VM. 

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
        REM Set default values back on the broken service 
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
4. [Trennen Sie den Betriebssystemdatenträger, und erstellen Sie den virtuellen Computer erneut.](../windows/troubleshoot-recovery-disks-portal.md) Überprüfen Sie dann, ob das Problem behoben wurde.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

---
title: "Hinzufügen von Skripts zu einem Wiederherstellungsplan in Azure Site Recovery | Microsoft-Dokumentation"
description: "Hier werden die Voraussetzungen zum Hinzufügen eines neuen Skripts zu einem Wiederherstellungsplan in VMM und Azure beschrieben."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>Hinzufügen von VMM-Skripts zu einem Wiederherstellungsplan


Dieser Artikel enthält Informationen zum Erstellen und Hinzufügen von VMM-Skripts für Wiederherstellungspläne in [Azure Site Recovery](site-recovery-overview.md).

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Voraussetzungen zum Hinzufügen eines Skripts zu einem Wiederherstellungsplan

Sie können PowerShell-Skripts in Ihren Wiederherstellungsplänen verwenden. Sie müssen die Skripts erstellen und in der VMM-Bibliothek ablegen, damit vom Wiederherstellungsplan darauf zugegriffen werden kann. Nachfolgend finden Sie einige Aspekte, die beim Erstellen des Skripts beachten werden sollten:

* Achten Sie darauf, dass in Skripts try/catch-Blöcke verwendet werden, damit die Ausnahmen ordnungsgemäß behandelt werden.
    - Falls im Skript eine Ausnahme auftritt, wird die Ausführung gestoppt, und für die Aufgabe wird ein Fehler angezeigt.
    - Wenn ein Fehler auftritt, werden keine verbleibenden Teile des Skripts ausgeführt.
    - Wenn beim Ausführen eines ungeplanten Failovers ein Fehler auftritt, wird der Wiederherstellungsplan fortgesetzt.
    - Wenn beim Ausführen eines geplanten Failovers ein Fehler auftritt, wird der Wiederherstellungsplan angehalten. Sie müssen das Skript korrigieren, überprüfen, ob es wie erwartet ausgeführt wird, und den Wiederherstellungsplan dann erneut ausführen.
        - Der Write-Host-Befehl funktioniert in einem Wiederherstellungsplanskript nicht, und die Ausführung des Skripts ist nicht erfolgreich. Um die Ausgabe zu erstellen, erstellen Sie ein Proxyskript, das wiederum Ihr Hauptskript ausführt. Stellen Sie sicher, dass alle Ausgaben mithilfe des Befehls „>>“ weitergeleitet werden.
        - Die Zeitüberschreitung für das Skript wird erreicht, falls die Rückgabe nicht innerhalb von 600 Sekunden erfolgt.
        - Falls etwas in STDERR geschrieben wird, wird das Skript als nicht erfolgreich klassifiziert. Diese Informationen werden in den Details zur Skriptausführung angezeigt.

* Skripts in einem Wiederherstellungsplan werden im Kontext des VMM-Dienstkontos ausgeführt. Stellen Sie sicher, dass dieses Konto über Leseberechtigungen für die Remotefreigabe verfügt, auf der sich das Skript befindet. Testen Sie, ob das Skript auf der Berechtigungsstufe für VMM-Dienstkonten ausgeführt werden kann.
* VMM-Cmdlets werden in einem Windows PowerShell-Modul bereitgestellt. Das Modul wird installiert, wenn Sie die VMM-Konsole installieren. Es kann mit folgendem Befehl in Ihr Skript geladen werden:
   - Import-Module -Name virtualmachinemanager. [Weitere Informationen](https://technet.microsoft.com/library/hh875013.aspx)
* Stellen Sie sicher, dass Ihre VMM-Bereitstellung mindestens einen Bibliothekserver enthält. Standardmäßig befindet sich der Bibliotheksfreigabepfad für einen VMM-Server lokal auf dem VMM-Server mit dem Ordnernamen „MSCVMMLibrary“.
    * Wenn Ihr Bibliotheksfreigabepfad remote vorhanden ist (oder lokal, aber nicht für „MSCVMMLibrary“ freigegeben), konfigurieren Sie die Freigabe wie folgt (hier dient „\\libserver2.contoso.com\share\“ als Beispiel):
      * Öffnen Sie den Registrierungseditor, und navigieren Sie zu **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.
      * Bearbeiten Sie den Wert **ScriptLibraryPath**, indem Sie dafür \\libserver2.contoso.com\share\. festlegen. Geben Sie den vollqualifizierten Domänennamen vollständig ein. Geben Sie die Berechtigungen für den Speicherort der Freigabe an. Beachten Sie, dass dies der Stammknoten der Freigabe ist. **Sie können die Bibliothek im Stammknoten in VMM durchsuchen, um dies zu überprüfen. Der Pfad, der geöffnet wird, ist der Stamm des Pfads, den Sie in der Variablen verwenden müssen**.
      * Stellen Sie sicher, dass Sie das Skript mit einem Benutzerkonto testen, das über dieselben Berechtigungen wie das VMM-Dienstkonto verfügt. Damit wird überprüft, ob eigenständige getestete Skripts auf dieselbe Weise wie in Wiederherstellungsplänen ausgeführt werden. Legen Sie für die Ausführungsrichtlinie auf dem VMM-Server wie folgt eine Umleitung fest:
        * Öffnen Sie die **Windows PowerShell-Konsole (64 Bit)** mit erweiterten Berechtigungen.
        * Geben Sie Folgendes ein: **Set-executionpolicy bypass**. [Weitere Informationen](https://technet.microsoft.com/library/ee176961.aspx)

> [!IMPORTANT]
> Sie sollten nur für 64-Bit-PowerShell für die Ausführungsrichtlinie „Bypass“ festlegen. Wenn dies für 32-Bit-PowerShell festgelegt wurde, werden die Skripts nicht ausgeführt.

## <a name="add-the-script-to-the-vmm-library"></a>Hinzufügen des Skripts zur VMM-Bibliothek

Falls Sie über eine VMM-Quellwebsite verfügen, können Sie ein Skript auf dem VMM-Server erstellen und es in Ihren Wiederherstellungsplan einfügen.

1. Erstellen Sie einen neuen Ordner in der Bibliothekfreigabe. Beispiel: \<VMMServerName>\MSSCVMMLibrary\RPScripts. Platzieren Sie diese Datei auf dem VMM-Quell- und -Zielserver.
2. Erstellen Sie das Skript (z. B. RPScript), und überprüfen Sie, ob es wie erwartet funktioniert.
3. Platzieren Sie das Skript am Speicherort „\<VMMServerName>\MSSCVMMLibrary“ auf den Quell- und Ziel-VMM-Servern.

## <a name="add-the-script-to-a-recovery-plan"></a>Hinzufügen des Skripts zu einem Wiederherstellungsplan

Sie können das Skript der Gruppe hinzufügen, nachdem Sie die virtuellen Computer oder Replikationsgruppen hinzugefügt und den Plan erstellt haben.

1. Öffnen Sie den Wiederherstellungsplan.
2. Klicken Sie auf ein Element in der Liste **Schritte**, und klicken Sie dann auf **Skript** oder **Manuelle Aktion**.
3. Geben Sie an, ob Sie das Skript oder die Aktion vor oder nach dem ausgewählten Eintrag hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten**, um die Position des Skripts nach oben oder unten zu verschieben.
4. Wenn Sie ein VMM-Skript hinzufügen, wählen Sie **Failover to VMM script** (Failover zu VMM-Skript) aus. Geben Sie unter **Skriptpfad** den relativen Pfad zur Freigabe ein. Geben Sie im VMM-Beispiel unten folgenden Pfad an: **\RPScripts\RPScript.PS1**.
5. Wenn Sie ein Azure-Automatisierungsrunbook hinzufügen, geben Sie das Azure Automation-Konto an, unter dem sich das Runbook befindet, und wählen das gewünschte Azure-Runbookskript aus.
6. Führen Sie ein Testfailover für den Wiederherstellungsplan aus, um sicherzustellen, dass das Skript wie erwartet funktioniert.


## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen](site-recovery-failover.md) zum Ausführen von Failovern

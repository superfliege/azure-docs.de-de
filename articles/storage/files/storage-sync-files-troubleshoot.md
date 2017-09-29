---
title: Problembehebung bei der Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation
description: "Beheben von gängigen Problemen bei der Azure-Dateisynchronisierung"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: wgries
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: cf3f3cf63cafc3b883d26144a53066ee421eb2a6
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="troubleshoot-azure-file-sync-preview"></a>Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)
Mit der Azure-Dateisynchronisierung (Vorschau) können die Freigaben auf Windows-Servern lokal oder in Azure repliziert werden. Sie und Ihre Benutzer greifen dann über den Windows-Server auf die Dateifreigabe zu, z.B. über eine SMB- oder NFS-Freigabe. Dies ist besonders nützlich für Szenarien, in denen Daten weit entfernt von einem Azure-Rechenzentrum abgerufen und geändert werden, z.B. in einer Zweigstelle. Daten können zwischen mehreren Windows Server-Endpunkten repliziert werden, z.B. zwischen mehreren Zweigstellen.

Dieser Artikel enthält Informationen zur Fehlerbehebung und Problemlösung für Ihre Bereitstellung der Azure-Dateisynchronisierung. Wenn dabei Fehler auftreten, wird in dieser Anleitung erläutert, wie Sie wichtige Protokolle aus dem System erfassen, die eine detailliertere Untersuchung der Probleme ermöglichen. Für die Azure-Dateisynchronisierung sind die folgenden Supportoptionen verfügbar:

- Microsoft-Support: Um eine neue Supportanfrage zu erstellen, navigieren Sie zur Registerkarte „Hilfe und Support“ im Azure-Portal und klicken auf „Neue Supportanfrage“.
- [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazuredata)

## <a name="how-to-troubleshoot-agent-installation-failures"></a>Beheben von Fehlern bei der Agent-Installation
Wenn bei der Installation des Agents für die Azure-Dateisynchronisierung ein Fehler auftritt, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um die Protokollierung während der Installation des Agents zu aktivieren:

```
StorageSyncAgent.msi /l*v Installer.log
```

Wenn der Installationsfehler auftritt, überprüfen Sie „installer.log“, um die Ursache zu ermitteln.

## <a name="server-is-not-listed-under-registered-servers-in-the-azure-portal"></a>Der Server ist im Azure-Portal nicht unter „Registrierte Server“ aufgeführt.
Wenn ein Server nicht unter „Registrierte Server“ für einen Speichersynchronisierungsdienst aufgeführt wird, führen Sie die folgenden Schritte aus:
1. Melden Sie sich an dem Server an, den Sie registrieren möchten.
2. Öffnen Sie den Datei-Explorer, und navigieren Sie zum Installationsverzeichnis für den Agent für die Speichersynchronisierung (der Standardspeicherort ist `C:\Program Files\Azure\StorageSyncAgent`). 
3. Führen Sie „ServerRegistration.exe“ aus, und folgen Sie den Anweisungen des Assistenten, um den Server bei einem Speichersynchronisierungsdienst zu registrieren.

## <a name="server-registration-displays-the-following-message-after-installing-the-azure-file-sync-agent-this-server-is-already-registered"></a>Bei der Serverregistrierung wird nach der Installation des Agents für die Azure-Dateisynchronisierung die folgende Meldung angezeigt: „Dieser Server ist bereits registriert.“
![Screenshot des Dialogfelds für die Serverregistrierung mit der Fehlermeldung „Dieser Server wurde bereits registriert“](media/storage-sync-files-troubleshoot/server-registration-1.png)

Diese Meldung wird angezeigt, wenn der Server zuvor bei einem Speichersynchronisierungsdienst registriert wurde. Wenn Sie die Registrierung des Servers beim aktuellen Speichersynchronisierungsdienst aufheben und ihn bei einem neuen Speichersynchronisierungsdienst registrieren möchten, führen Sie die [Schritte zum Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) aus.

## <a name="how-to-troubleshoot-sync-not-working-on-a-server"></a>Beheben von Synchronisierungsfehlern auf einem Server
Wenn bei der Synchronisierung auf einem Server ein Fehler auftritt, führen Sie folgende Schritte aus:
- Stellen Sie im Azure-Portal sicher, dass ein Serverendpunkt für das Verzeichnis vorhanden ist, das Sie mit einer Azure-Dateifreigabe synchronisieren möchten:
    
    ![Screenshot von einer Synchronisierungsgruppe mit einem Cloud- und einem Serverendpunkt im Azure-Portal](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll mit der Ereignisanzeige unter `Applications and Services\Microsoft\FileSync\Agent`.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist.
- Überprüfen Sie, ob der Azure-Dateisynchronisierungsdienst auf dem Server ausgeführt wird, indem Sie das Dienste-MMC-Snap-In öffnen und sicherstellen, dass der Agent für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.

## <a name="how-to-troubleshoot-individual-files-failing-to-sync"></a>Beheben von Synchronisierungsfehlern bei einzelnen Dateien 
Wenn einzelne Dateien nicht synchronisiert werden können, führen Sie folgende Schritte aus:
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Stellen Sie sicher, dass für die Datei keine offenen Handles vorhanden sind.
    - Hinweis: Die Azure-Dateisynchronisierung erfasst in regelmäßigen Abständen VSS-Momentaufnahmen, um Dateien mit offenen Handles zu synchronisieren.

## <a name="how-to-troubleshoot-files-that-fail-to-tier"></a>So beheben Sie Probleme bei Dateien, bei denen kein Tiering möglich ist
Wenn das Tiering für eine Datei in Azure Files nicht möglich ist, führen Sie die folgenden Schritte aus:

- Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.
    - Hinweis: Eine Datei muss mit einer Azure-Dateifreigabe synchronisiert werden, bevor ein Tiering möglich ist.
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist. 
- Stellen Sie sicher, dass die Filtertreiber für die Azure-Dateisynchronisierung („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden.
    - Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, führen Sie `fltmc` aus, und überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet werden.

## <a name="how-to-troubleshoot-files-that-fail-to-be-recalled"></a>Beheben von Rückruffehlern bei Dateien
Wenn Dateien nicht zurückgerufen werden können, führen Sie die folgenden Schritte aus:
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist. 
- Stellen Sie sicher, dass die Filtertreiber für die Azure-Dateisynchronisierung („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden.
    - Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, führen Sie `fltmc` aus, und überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet werden.

## <a name="how-to-troubleshoot-files-being-unexpectedly-recalled-on-a-server"></a>Fehlerbehebung bei Dateien, die unerwartet auf einem Server zurückgerufen werden
Virenschutz, Sicherung und andere Anwendungen, die viele Dateien lesen, können zu unerwünschten Rückrufen führen, wenn sie das Offline-Attribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Durch das Überspringen von Offlinedateien bei Produkten, die dies unterstützen, werden unerwünschte Rückrufe beim Ausführen von Vorgängen wie Virenscans oder Sicherungsaufträgen vermieden.

Wenden Sie sich an den Softwareanbieter, um weitere Informationen zu den erforderlichen Konfigurationsschritten zu erhalten, damit die Lösung das Lesen von Offlinedateien überspringt.

Unerwünschte Rückrufe können weiterhin in anderen Szenarien auftreten, z.B. beim Durchsuchen von Dateien im Datei-Explorer. Wenn Sie einen Ordner mit Cloudtieringdateien im Datei-Explorer auf dem Server öffnen, kann dies zu unerwünschten Rückrufen führen, insbesondere, wenn ein Virenschutz auf dem Server aktiviert ist.

## <a name="general-troubleshooting"></a>Allgemeine Problembehandlung
Wenn Probleme mit der Azure-Dateisynchronisierung auf einem Server auftreten, führen Sie zunächst folgende Schritte aus:
- Überprüfen Sie das Diagnose- und Betriebsereignisprotokoll in der Ereignisanzeige
    - Probleme mit Synchronisierung, Tiering und Rückrufen werden in den Diagnose- und Betriebsereignisprotokollen unter `Applications and Services\Microsoft\FileSync\Agent` protokolliert.
    - Probleme bei der Serververwaltung (z.B. Konfigurationseinstellungen) werden in den Diagnose- und Betriebsereignisprotokollen unter `Applications and Services\Microsoft\FileSync\Management` protokolliert.
- Stellen Sie sicher, dass der Azure-Dienst für die Dateisynchronisierung auf dem Server ausgeführt wird.
    - Öffnen Sie das Dienste-MMC-Snap-In, und stellen Sie sicher, dass der Agent für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.
- Stellen Sie sicher, dass die Filtertreiber für die Azure-Dateisynchronisierung („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden.
    - Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, führen Sie „fltmc“ aus, und überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet werden.

Wenn das Problem durch die oben aufgeführten Schritte nicht behoben wird, führen Sie das AFSDiag-Tool wie folgt aus:
1. Erstellen Sie ein Verzeichnis, in dem die AFSDiag-Ausgabe gespeichert wird (z.B. „c:\output“).
2. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden Befehle aus (drücken Sie nach jedem Befehl die Eingabetaste):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Geben Sie für die Kernelmodus-Ablaufverfolgungsebene der Azure-Dateisynchronisierung 1 ein (sofern nicht zum Erstellen von ausführlicheren Ablaufverfolgungen angegeben), und drücken Sie die Eingabetaste.
4. Geben Sie für die Benutzermodus-Ablaufverfolgungsebene der Azure-Dateisynchronisierung 1 ein (sofern nicht zum Erstellen von ausführlicheren Ablaufverfolgungen angegeben), und drücken Sie die Eingabetaste.
5. Reproduzieren Sie das Problem, und drücken Sie danach D.
6. Das angegebene Ausgabeverzeichnis enthält nun eine ZIP-Datei mit Protokollen und Ablaufverfolgungsdateien.

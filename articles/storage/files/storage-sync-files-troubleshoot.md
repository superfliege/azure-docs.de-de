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
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 1ea7956e92dbc85f62383e4b041c4c830599f765
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers zu verzichten. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Dieser Artikel enthält Informationen zur Fehlerbehebung und Problemlösung für Ihre Bereitstellung der Azure-Dateisynchronisierung. Wenn dabei Fehler auftreten, wird in dieser Anleitung erläutert, wie Sie wichtige Protokolle aus dem System erfassen, die eine detailliertere Untersuchung der Probleme ermöglichen. Für die Azure-Dateisynchronisierung sind die folgenden Supportoptionen verfügbar:

- Microsoft-Support: Um eine neue Supportanfrage zu erstellen, navigieren Sie zur Registerkarte „Hilfe und Support“ im Azure-Portal und klicken auf „Neue Supportanfrage“.
- [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazuredata)

## <a name="how-to-troubleshoot-agent-installation-failures"></a>Beheben von Fehlern bei der Agent-Installation
Wenn bei der Installation des Agents für die Azure-Dateisynchronisierung ein Fehler auftritt, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um die Protokollierung während der Installation des Agents zu aktivieren:

```
StorageSyncAgent.msi /l*v Installer.log
```

Wenn der Installationsfehler auftritt, überprüfen Sie „installer.log“, um die Ursache zu ermitteln. 

> [!Note]  
> Bei der Agentinstallation treten Fehler auf, wenn Sie die Verwendung von Microsoft Update auswählen und der Windows Update-Dienst nicht ausgeführt wird.

## <a name="cloud-endpoint-creation-fails-with-the-following-error-the-specified-azure-fileshare-is-already-in-use-by-a-different-cloudendpoint"></a>Fehler bei der Erstellung des Cloudendpunkts mit folgender Meldung: „The specified Azure FileShare is already in use by a different CloudEndpoint“ (Die angegebene Azure-Dateifreigabe wird bereits von einem anderen Cloudendpunkt verwendet)
Dieser Fehler tritt auf, wenn die Azure-Dateifreigabe bereits von einem anderen Cloudendpunkt verwendet wird. 

Wenn diese Fehlermeldung angezeigt wird und die Azure-Dateifreigabe aktuell von keinem Cloudendpunkt verwendet wird, führen Sie die Schritte unten aus, um die Azure File Sync-Metadaten in der Azure-Dateifreigabe zu löschen:

> [!Warning]  
> Wenn die Metadaten in einer Azure-Dateifreigabe gelöscht werden, die aktuell von einem Cloudendpunkt verwendet wird, treten bei Azure File Sync-Vorgängen Fehler auf. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure-Dateifreigabe.  
2. Klicken Sie mit der rechten Maustaste auf die Azure-Dateifreigabe, und wählen Sie **Metadaten bearbeiten** aus.
3. Klicken Sie mit der rechten Maustaste auf „SyncService“, und wählen Sie **Löschen** aus.

## <a name="server-is-not-listed-under-registered-servers-in-the-azure-portal"></a>Der Server ist im Azure-Portal nicht unter „Registrierte Server“ aufgeführt.
Wenn ein Server nicht unter „Registrierte Server“ für einen Speichersynchronisierungsdienst aufgeführt wird, führen Sie die folgenden Schritte aus:
1. Melden Sie sich an dem Server an, den Sie registrieren möchten.
2. Öffnen Sie den Datei-Explorer, und navigieren Sie zum Installationsverzeichnis für den Agent für die Speichersynchronisierung (der Standardspeicherort ist `C:\Program Files\Azure\StorageSyncAgent`). 
3. Führen Sie „ServerRegistration.exe“ aus, und folgen Sie den Anweisungen des Assistenten, um den Server bei einem Speichersynchronisierungsdienst zu registrieren.

## <a name="server-registration-displays-the-following-message-after-installing-the-azure-file-sync-agent-this-server-is-already-registered"></a>Bei der Serverregistrierung wird nach der Installation des Agents für die Azure-Dateisynchronisierung die folgende Meldung angezeigt: „Dieser Server ist bereits registriert.“
![Screenshot des Dialogfelds für die Serverregistrierung mit der Fehlermeldung „Dieser Server wurde bereits registriert“](media/storage-sync-files-troubleshoot/server-registration-1.png)

Diese Meldung wird angezeigt, wenn der Server zuvor bei einem Speichersynchronisierungsdienst registriert wurde. Wenn Sie die Registrierung des Servers beim aktuellen Speichersynchronisierungsdienst aufheben und ihn bei einem neuen Speichersynchronisierungsdienst registrieren möchten, führen Sie die [Schritte zum Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) aus.

Wenn der Server im Storage Sync-Dienst unter „Registrierte Server“ nicht aufgeführt ist, führen Sie die folgenden PowerShell-Befehle auf dem Server aus, für den die Registrierung aufgehoben werden soll:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Wenn der Server Teil eines Clusters ist, ist ein optionaler `Reset-StorageSyncServer -CleanClusterRegistration`-Parameter verfügbar, über den die Clusterregistrierung auch entfernt werden kann. Dieser Parameter sollte verwendet werden, wenn die Registrierung des letzten Knotens im Cluster aufgehoben wird.

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
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
ms.openlocfilehash: 6bc5f2da2b8628671037b9257db746e73cd3afad
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)
Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

Dieser Artikel enthält Informationen zur Fehlerbehebung und Problemlösung für Ihre Bereitstellung der Azure-Dateisynchronisierung. Wenn dabei Fehler auftreten, wird in dieser Anleitung erläutert, wie Sie wichtige Protokolle aus dem System erfassen, die eine detailliertere Untersuchung der Probleme ermöglichen. Wenn Sie hier keine Antwort auf Ihre Frage finden, zögern Sie nicht, uns über die folgenden Kanäle zu kontaktieren (in eskalierender Reihenfolge):

1. Im Abschnitt „Kommentare“ dieses Artikels.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft-Support: Um eine neue Supportanfrage zu erstellen, navigieren Sie zur Registerkarte „Hilfe und Support“ im Azure-Portal und klicken auf „Neue Supportanfrage“.

## <a name="agent-installation-and-server-registration"></a>Agent-Installation und Serverregistrierung
<a id="agent-installation-failures"></a>**Beheben von Fehlern bei der Agent-Installation**  
Wenn bei der Installation des Agents für die Azure-Dateisynchronisierung ein Fehler auftritt, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um die Protokollierung während der Installation des Agents zu aktivieren:

```
StorageSyncAgent.msi /l*v Installer.log
```

Wenn der Installationsfehler auftritt, überprüfen Sie „installer.log“, um die Ursache zu ermitteln. 

> [!Note]  
> Bei der Agentinstallation treten Fehler auf, wenn Sie die Verwendung von Microsoft Update auswählen und der Windows Update-Dienst nicht ausgeführt wird.

<a id="server-registration-missing"></a>**Der Server ist im Azure-Portal nicht unter „Registrierte Server“ aufgeführt.**  
Wenn ein Server nicht unter „Registrierte Server“ für einen Speichersynchronisierungsdienst aufgeführt wird, führen Sie die folgenden Schritte aus:
1. Melden Sie sich an dem Server an, den Sie registrieren möchten.
2. Öffnen Sie den Datei-Explorer, und navigieren Sie zum Installationsverzeichnis für den Agent für die Speichersynchronisierung (der Standardspeicherort ist `C:\Program Files\Azure\StorageSyncAgent`). 
3. Führen Sie „ServerRegistration.exe“ aus, und folgen Sie den Anweisungen des Assistenten, um den Server bei einem Speichersynchronisierungsdienst zu registrieren.

<a id="server-already-registered"></a>**Bei der Serverregistrierung wird nach der Installation des Azure File Sync-Agents die folgende Meldung angezeigt: „Dieser Server ist bereits registriert.“**  
![Screenshot des Dialogfelds für die Serverregistrierung mit der Fehlermeldung „Dieser Server wurde bereits registriert“](media/storage-sync-files-troubleshoot/server-registration-1.png)

Diese Meldung wird angezeigt, wenn der Server zuvor bei einem Speichersynchronisierungsdienst registriert wurde. Wenn Sie die Registrierung des Servers beim aktuellen Speichersynchronisierungsdienst aufheben und ihn bei einem neuen Speichersynchronisierungsdienst registrieren möchten, führen Sie die [Schritte zum Aufheben der Registrierung eines Servers bei der Azure-Dateisynchronisierung](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) aus.

Wenn der Server im Storage Sync-Dienst unter „Registrierte Server“ nicht aufgeführt ist, führen Sie die folgenden PowerShell-Befehle auf dem Server aus, für den die Registrierung aufgehoben werden soll:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Wenn der Server Teil eines Clusters ist, ist ein optionaler `Reset-StorageSyncServer -CleanClusterRegistration`-Parameter verfügbar, über den die Clusterregistrierung auch entfernt werden kann.

<a id="web-site-not-trusted"></a>**Warum erhalte ich bei der Registrierung eines Servers zahlreiche Antworten, dass eine Website nicht vertrauenswürdig sei?**  
Dieser Fehler tritt auf, weil die Richtlinie **Erhöhte Internet Explorer-Sicherheit** bei der Serverregistrierung aktiviert war. Weitere Informationen zum ordnungsgemäßen Deaktivieren der Richtlinie **Erhöhte Internet Explorer-Sicherheit** finden Sie unter [Vorbereiten von Windows-Servern für die Verwendung von Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) und [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Verwaltung von Synchronisierungsgruppen
<a id="cloud-endpoint-using-share"></a>**Fehler bei der Erstellung des Cloudendpunkts mit folgender Meldung: „The specified Azure FileShare is already in use by a different CloudEndpoint“ (Die angegebene Azure-Dateifreigabe wird bereits von einem anderen Cloudendpunkt verwendet)**  
Dieser Fehler tritt auf, wenn die Azure-Dateifreigabe bereits von einem anderen Cloudendpunkt verwendet wird. 

Wenn diese Fehlermeldung angezeigt wird und die Azure-Dateifreigabe aktuell von keinem Cloudendpunkt verwendet wird, führen Sie die Schritte unten aus, um die Azure File Sync-Metadaten in der Azure-Dateifreigabe zu löschen:

> [!Warning]  
> Wenn die Metadaten in einer Azure-Dateifreigabe gelöscht werden, die aktuell von einem Cloudendpunkt verwendet wird, treten bei Azure File Sync-Vorgängen Fehler auf. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure-Dateifreigabe.  
2. Klicken Sie mit der rechten Maustaste auf die Azure-Dateifreigabe, und wählen Sie **Metadaten bearbeiten** aus.
3. Klicken Sie mit der rechten Maustaste auf „SyncService“, und wählen Sie **Löschen** aus.

<a id="cloud-endpoint-authfailed"></a>**Fehler beim Erstellen des Cloudendpunkts: AuthorizationFailed**  
Dieses Problem tritt auf, wenn Ihr Benutzerkonto nicht über die erforderlichen Rechte verfügt, um einen Cloudendpunkt zu erstellen. 

Um einen Cloudendpunkt erstellen zu können, muss Ihr Benutzerkonto über die folgenden Microsoft-Autorisierungsberechtigungen verfügen:  
* Lesen: Rollendefinition abrufen
* Schreiben: Benutzerdefinierte Rollendefinition erstellen oder aktualisieren
* Lesen: Rollenzuweisung abrufen
* Schreiben: Rollenzuweisung erstellen

Die folgenden integrierten Rollen verfügen über die entsprechenden Microsoft-Autorisierungsberechtigungen:  
* Besitzer
* Benutzerzugriffsadministrator

Um festzustellen, ob Ihre Benutzerkontorolle über die entsprechenden Berechtigungen verfügt, führen Sie die folgenden Schritte aus:  
* Klicken Sie im Azure-Portal auf **Ressourcengruppen**.
* Wählen Sie die Ressourcengruppe aus, in der sich das Speicherkonto befindet, und klicken Sie auf **Zugriffssteuerung (IAM)**
* Klicken Sie auf die **Rolle** (Beispiel: Besitzer, Mitwirkender) für Ihr Benutzerkonto.
* Wählen Sie in der Liste **Ressourcenanbieter** die Option **Microsoft-Autorisierung** aus. 
* **Rollenzuweisung** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.
* **Rollendefinition** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.

<a id="cloud-endpoint-deleteinternalerror"></a>**Fehler beim Löschen des Endpunkts: MgmtInternalError**  
Dieses Problem kann auftreten, wenn die Azure-Dateifreigabe oder das Speicherkonto vor dem Löschen des Cloudendpunkts gelöscht wurde. Dieses Problem wird in einem künftigen Update behoben werden, sodass der Cloudendpunkt gelöscht werden kann.

Um zu verhindern, dass dieses Problem auftritt, löschen Sie den Cloudendpunkt, bevor Sie die Azure-Dateifreigabe oder das Speicherkonto löschen.

## <a name="sync"></a>Synchronisierung
<a id="afs-change-detection"></a>**Ich habe eine Datei direkt über SMB oder über das Portal in meiner Azure-Dateifreigabe erstellt. Wie lange dauert es, bis die Datei mit den Servern in der Synchronisierungsgruppe synchronisiert ist?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Beheben von Synchronisierungsfehlern auf einem Server**  
Wenn bei der Synchronisierung auf einem Server ein Fehler auftritt, führen Sie folgende Schritte aus:
- Stellen Sie im Azure-Portal sicher, dass ein Serverendpunkt für das Verzeichnis vorhanden ist, das Sie mit einer Azure-Dateifreigabe synchronisieren möchten:
    
    ![Screenshot von einer Synchronisierungsgruppe mit einem Cloud- und einem Serverendpunkt im Azure-Portal](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll mit der Ereignisanzeige unter `Applications and Services\Microsoft\FileSync\Agent`.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist.
- Überprüfen Sie, ob der Azure-Dateisynchronisierungsdienst auf dem Server ausgeführt wird, indem Sie das Dienste-MMC-Snap-In öffnen und sicherstellen, dass der Agent für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.

<a id="replica-not-ready"></a>**Fehler bei der Synchronisierung: 0x80c8300f – Das Replikat ist nicht zum Ausführen des erforderlichen Vorgangs bereit**  
Dieser Fehler wird erwartet, wenn Sie einen Cloudendpunkt erstellen und eine Azure-Dateifreigabe verwenden, die Daten enthält. Sobald die Erkennung von Änderungen an der Azure-Dateifreigabe abgeschlossen ist (was bis zu 24 Stunden dauern kann), sollte die Synchronisierung ordnungsgemäß starten und funktionieren.

<a id="broken-sync-files"></a>**Beheben von Synchronisierungsfehlern bei einzelnen Dateien**  
Wenn einzelne Dateien nicht synchronisiert werden können, führen Sie folgende Schritte aus:
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Stellen Sie sicher, dass für die Datei keine offenen Handles vorhanden sind.
    - Hinweis: Die Azure-Dateisynchronisierung erfasst in regelmäßigen Abständen VSS-Momentaufnahmen, um Dateien mit offenen Handles zu synchronisieren.

## <a name="cloud-tiering"></a>Cloudtiering 
<a id="files-fail-tiering"></a>**So beheben Sie Probleme bei Dateien, bei denen kein Tiering möglich ist**  
Wenn das Tiering für eine Datei in Azure Files nicht möglich ist, führen Sie die folgenden Schritte aus:

- Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.
    - Hinweis: Eine Datei muss mit einer Azure-Dateifreigabe synchronisiert werden, bevor ein Tiering möglich ist.
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist. 
- Stellen Sie sicher, dass die Filtertreiber für die Azure-Dateisynchronisierung („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden.
    - Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, führen Sie `fltmc` aus, und überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet werden.

<a id="files-fail-recall"></a>**Beheben von Rückruffehlern bei Dateien**  
Wenn Dateien nicht zurückgerufen werden können, führen Sie die folgenden Schritte aus:
- Überprüfen Sie das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll unter `Applications and Services\Microsoft\FileSync\Agent` in der Ereignisanzeige.
- Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.
- Vergewissern Sie sich, dass der Server mit dem Internet verbunden ist. 
- Stellen Sie sicher, dass die Filtertreiber für die Azure-Dateisynchronisierung („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden.
    - Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, führen Sie `fltmc` aus, und überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet werden.

<a id="files-unexpectedly-recalled"></a>**Fehlerbehebung bei Dateien, die unerwartet auf einem Server zurückgerufen werden**  
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

## <a name="see-also"></a>Weitere Informationen
- [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md)
- [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)

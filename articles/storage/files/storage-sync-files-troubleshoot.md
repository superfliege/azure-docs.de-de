---
title: Problembehebung bei der Azure-Dateisynchronisierung (Vorschau) | Microsoft-Dokumentation
description: "Beheben von häufigen Problemen bei der Azure-Dateisynchronisierung"
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
ms.openlocfilehash: 265c5f660c4bee53a2faf4a073384587eb3f65fc
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)
Verwenden Sie Azure File Sync (Vorschau), um die Dateifreigaben Ihrer Organisation in Azure Files zu zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit Azure File Sync werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, NFS und FTPS. Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält Informationen zur Behebung von Fehlern und Lösung von Problemen, die möglicherweise bei ihrer Azure File Sync-Bereitstellung auftreten. Wir beschreiben außerdem, wie Sie wichtige Protokolle aus dem System erfassen, wenn eine detailliertere Untersuchung des Problems erforderlich ist. Wenn Sie hier keine Antwort auf Ihre Frage finden, können Sie sich über die folgenden Kanäle an uns wenden (Eskalationsreihenfolge):

1. Im Abschnitt „Kommentare“ dieses Artikels.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft-Support. Wählen Sie zum Erstellen einer neuen Supportanfrage im Azure-Portal auf der Registerkarte **Hilfe** die Schaltfläche **Hilfe und Support** und anschließend die Option **Neue Supportanfrage**.

## <a name="agent-installation-and-server-registration"></a>Agent-Installation und Serverregistrierung
<a id="agent-installation-failures"></a>**Beheben von Fehlern bei der Agent-Installation**  
Wenn bei der Installation des Azure File Sync-Agents ein Fehler auftritt, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um die Protokollierung während der Installation des Agents zu aktivieren:

```
StorageSyncAgent.msi /l*v Installer.log
```

Überprüfen Sie „installer.log“, um die Ursache des Installationsfehlers zu bestimmen. 

> [!Note]  
> Bei der Agent-Installation tritt ein Fehler auf, wenn der Computer für die Nutzung von Microsoft Update eingerichtet ist und der Windows Update-Dienst nicht ausgeführt wird.

<a id="server-registration-missing"></a>**Der Server ist im Azure-Portal nicht unter „Registrierte Server“ aufgeführt.**  
Wenn ein Server nicht unter **Registrierte Server** für einen Speichersynchronisierungsdienst aufgeführt wird:
1. Melden Sie sich an dem Server an, den Sie registrieren möchten.
2. Öffnen Sie den Datei-Explorer, und wechseln Sie dann zum Installationsverzeichnis des Storage-Synchronisierungs-Agents (der Standardspeicherort ist C:\Programme\Azure\StorageSyncAgent). 
3. Führen Sie „ServerRegistration.exe“ aus, und schließen Sie den Assistenten ab, um den Server bei einem Storage-Synchronisierungsdienst zu registrieren.

<a id="server-already-registered"></a>**Bei der Serverregistrierung wird während der Installation des Azure File Sync-Agents die folgende Meldung angezeigt: „Dieser Server ist bereits registriert.“** 

![Screenshot des Dialogfelds für die Serverregistrierung mit der Fehlermeldung „Dieser Server wurde bereits registriert“](media/storage-sync-files-troubleshoot/server-registration-1.png)

Diese Meldung wird angezeigt, wenn der Server zuvor bei einem Speichersynchronisierungsdienst registriert wurde. Wenn Sie die Registrierung des Servers beim aktuellen Speichersynchronisierungsdienst aufheben und ihn bei einem neuen Speichersynchronisierungsdienst registrieren möchten, führen Sie die unter [Aufheben der Registrierung eines Servers bei Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) beschriebenen Schritte aus.

Wenn der Server nicht im Speichersynchronisierungsdienst unter **Registrierte Server** aufgeführt ist, führen Sie auf dem Server, dessen Registrierung Sie aufheben möchten, die folgenden PowerShell-Befehle aus:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Wenn der Server Bestandteil eines Clusters ist, können Sie den optionalen Parameter *Reset-StorageSyncServer -CleanClusterRegistration* verwenden, um die Clusterregistrierung ebenfalls aufzuheben.

<a id="web-site-not-trusted"></a>**Beim Registrieren eines Servers erhalte ich zahlreiche Antworten, dass eine Website nicht vertrauenswürdig sei. Warum?**  
Dieses Problem tritt auf, wenn die Richtlinie **Erhöhte Internet Explorer-Sicherheit** bei der Serverregistrierung aktiviert war. Weitere Informationen zum ordnungsgemäßen Deaktivieren der Richtlinie **Erhöhte Internet Explorer-Sicherheit** finden Sie unter [Vorbereiten von Windows-Servern für die Verwendung von Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) und [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Verwaltung von Synchronisierungsgruppen
<a id="cloud-endpoint-using-share"></a>**Fehler bei der Erstellung des Cloudendpunkts mit der Meldung: „The specified Azure FileShare is already in use by a different CloudEndpoint“ (Die angegebene Azure-Dateifreigabe wird bereits von einem anderen Cloudendpunkt verwendet)**  
Dieses Problem tritt auf, wenn die Azure-Dateifreigabe bereits von einem anderen Cloudendpunkt verwendet wird. 

Wenn diese Nachricht angezeigt wird und die Azure-Dateifreigabe aktuell von keinem Cloudendpunkt verwendet wird, führen Sie die folgenden Schritte aus, um die Azure File Sync-Metadaten in der Azure-Dateifreigabe zu löschen:

> [!Warning]  
> Wenn die Metadaten in einer Azure-Dateifreigabe gelöscht werden, die aktuell von einem Cloudendpunkt verwendet wird, treten bei Azure File Sync-Vorgängen Fehler auf. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure-Dateifreigabe.  
2. Klicken Sie mit der rechten Maustaste auf die Azure-Dateifreigabe, und wählen Sie dann **Metadaten bearbeiten** aus.
3. Klicken Sie mit der rechten Maustaste auf **SyncService**, und wählen Sie dann **Löschen** aus.

<a id="cloud-endpoint-authfailed"></a>**Fehler beim Erstellen des Cloudendpunkts: „AuthorizationFailed“**  
Dieses Problem tritt auf, wenn Ihr Benutzerkonto nicht über die erforderlichen Rechte verfügt, um einen Cloudendpunkt zu erstellen. 

Um einen Cloudendpunkt erstellen zu können, muss Ihr Benutzerkonto über die folgenden Microsoft-Autorisierungsberechtigungen verfügen:  
* Lesen: Rollendefinition abrufen
* Schreiben: Benutzerdefinierte Rollendefinition erstellen oder aktualisieren
* Lesen: Rollenzuweisung abrufen
* Schreiben: Rollenzuweisung erstellen

Die folgenden integrierten Rollen verfügen über die erforderlichen Microsoft-Autorisierungsberechtigungen:  
* Besitzer
* Benutzerzugriffsadministrator

So bestimmen Sie, ob Ihr Benutzerkonto über die erforderlichen Berechtigungen verfügt:  
1. Wählen Sie im Azure-Portal **Ressourcengruppen** aus.
2. Wählen Sie die Ressourcengruppe aus, in der sich das Speicherkonto befindet, und wählen Sie dann **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie die **Rolle** (z.B. Besitzer oder Mitwirkender) für Ihr Benutzerkonto aus.
4. Wählen Sie in der Liste **Ressourcenanbieter** die Option **Microsoft-Autorisierung** aus. 
    * **Rollenzuweisung** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.
    * **Rollendefinition** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.

<a id="cloud-endpoint-deleteinternalerror"></a>**Fehler beim Löschen des Cloudendpunkts: „MgmtInternalError“**  
Dieses Problem kann auftreten, wenn die Azure-Dateifreigabe oder das Speicherkonto vor dem Löschen des Cloudendpunkts gelöscht wurde. Dieses Problem wird in einer zukünftigen Version behoben. Wenn dies erfolgt ist, können Sie einen Cloudendpunkt löschen, nachdem Sie die Azure-Dateifreigabe oder das Speicherkonto gelöscht haben.

Um das Auftreten dieses Problems bis dahin zu verhindern, löschen Sie den Cloudendpunkt, bevor Sie die Azure-Dateifreigabe oder das Speicherkonto löschen.

## <a name="sync"></a>Synchronisierung
<a id="afs-change-detection"></a>**Wie lange dauert es, bis eine Datei auf Server in der Synchronisierungsgruppe synchronisiert wird, wenn ich die Datei direkt in meiner Azure-Dateifreigabe mithilfe von SMB oder über das Portal erstellt habe?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Fehler bei der Synchronisierung auf einem Server**  
Wenn auf einem Server ein Synchronisierungsfehler auftritt:
1. Stellen Sie im Azure-Portal sicher, dass ein Serverendpunkt für das Verzeichnis vorhanden ist, das Sie mit einer Azure-Dateifreigabe synchronisieren möchten:
    
    ![Screenshot von einer Synchronisierungsgruppe mit einem Cloudendpunkt und einem Serverendpunkt im Azure-Portal](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

2. Überprüfen Sie in der Ereignisanzeige die Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Agent“.
    1. Überprüfen Sie, ob der Server über eine Internetverbindung verfügt.
    2. Stellen Sie sicher, dass der Azure-Dienst für die Dateisynchronisierung auf dem Server ausgeführt wird. Öffnen Sie zu diesem Zweck das Dienste-MMC-Snap-In, und überprüfen Sie, ob der Agentdienst für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.

<a id="replica-not-ready"></a>**Fehler bei der Synchronisierung: „0x80c8300f – Das Replikat ist nicht zum Ausführen des erforderlichen Vorgangs bereit“**  
Dieses Problem wird erwartet, wenn Sie einen Cloudendpunkt erstellen und eine Azure-Dateifreigabe verwenden, die Daten enthält. Wenn die Ausführung eines Auftrags zum Erkennen von Änderungen auf der Azure-Dateifreigabe abgeschlossen ist (das kann bis zu 24 Stunden dauern), sollte die Synchronisierung anschließend ordnungsgemäß funktionieren.

<a id="broken-sync-files"></a>**Beheben von Synchronisierungsfehlern bei einzelnen Dateien**  
Wenn einzelne Dateien nicht synchronisiert werden können:
1. Überprüfen Sie in der Ereignisanzeige die Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Agent“.
2. Stellen Sie sicher, dass für die Datei keine Handles geöffnet sind.

    > [!NOTE]
    > Die Azure-Dateisynchronisierung erfasst in regelmäßigen Abständen VSS-Momentaufnahmen, um Dateien mit offenen Handles zu synchronisieren.

## <a name="cloud-tiering"></a>Cloudtiering 
<a id="files-fail-tiering"></a>**Beheben von Problemen bei Dateien, bei denen kein Tiering möglich ist**  
Wenn Tieringfehler von Dateien auf Azure Files auftreten:

1. Überprüfen Sie, ob die Dateien in der Azure-Dateifreigabe vorhanden sind.

    > [!NOTE]
    > Eine Datei muss mit einer Azure-Dateifreigabe synchronisiert werden, bevor ein Tiering möglich ist.
2. Überprüfen Sie in der Ereignisanzeige die Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Agent“.
    1. Überprüfen Sie, ob der Server über eine Internetverbindung verfügt. 
    2. Überprüfen Sie, ob die Filtertreiber von Azure File Sync („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden:
        - Geben Sie an einer Eingabeaufforderung mit erhöhten Rechten `fltmc` ein. Überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet sind.

<a id="files-fail-recall"></a>**Beheben von Rückruffehlern bei Dateien**  
Wenn bei Dateien Rückruffehler auftreten:
1. Überprüfen Sie in der Ereignisanzeige die Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Agent“.
    1. Überprüfen Sie, ob die Dateien in der Azure-Dateifreigabe vorhanden sind.
    2. Überprüfen Sie, ob der Server über eine Internetverbindung verfügt. 
    3. Überprüfen Sie, ob die Filtertreiber von Azure File Sync („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden:
        - Geben Sie an einer Eingabeaufforderung mit erhöhten Rechten `fltmc` ein. Überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet sind.

<a id="files-unexpectedly-recalled"></a>**Fehlerbehebung bei Dateien, die unerwartet auf einem Server zurückgerufen werden**  
Virenschutz, Sicherung und andere Anwendungen, die viele Dateien lesen, können zu unbeabsichtigten Rückrufen führen, wenn sie das Offline-überspringen-Attribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Das Überspringen von Offlinedateien für Produkte, die diese Option unterstützen, hilft, unbeabsichtigte Rückrufe während Vorgängen wie Virusscans oder Sicherungsaufträgen zu verhindern.

Wenden Sie sich an den Softwareanbieter, um weitere Informationen zu den erforderlichen Konfigurationsschritten zu erhalten, damit die Lösung das Lesen von Offlinedateien überspringt.

Unbeabsichtigte Rückrufe können auch in anderen Szenarien auftreten, z.B. beim Durchsuchen von Dateien im Datei-Explorer. Das Öffnen eines Ordners, der Dateien mit Cloudtiering enthält, im Datei-Explorer auf dem Server kann zu unbeabsichtigten Rückrufen führen. Die Wahrscheinlichkeit dafür steigt, wenn auf dem Server eine Virenschutzlösung aktiviert ist.

## <a name="general-troubleshooting"></a>Allgemeine Problembehandlung
Wenn Probleme mit Azure File Sync auf einem Server auftreten, führen Sie zunächst die folgenden Schritte aus:
1. Überprüfen Sie in der Ereignisanzeige das Betriebsereignisprotokoll und das Diagnoseereignisprotokoll.
    - Probleme mit Synchronisierung, Tiering und Rückrufen werden in den Diagnose- und Betriebsereignisprotokollen unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Agent“ protokolliert.
    - Probleme im Zusammenhang mit der Serververwaltung (z.B. Konfigurationseinstellungen) werden in den Betriebs- und Diagnoseereignisprotokollen unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Management“ protokolliert.
2. Überprüfen Sie, ob der Azure-Dienst für die Dateisynchronisierung auf dem Server ausgeführt wird:
    - Öffnen Sie das Dienste-MMC-Snap-In, und stellen Sie sicher, dass der Agent für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.
3. Überprüfen Sie, ob die Filtertreiber von Azure File Sync („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden:
    - Geben Sie an einer Eingabeaufforderung mit erhöhten Rechten `fltmc` ein. Überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet sind.

Wenn das Problem nicht behoben wird, führen Sie das AFSDiag-Tool aus:
1. Erstellen Sie eine Verzeichnis zum Speichern der Ausgabe von AFSDiag (z.B. „C:\Ausgabe“).
2. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden Befehle aus (drücken Sie nach jedem Befehl die EINGABETASTE):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Geben Sie für die Kernelmodus-Ablaufverfolgungsebene der Azure-Dateisynchronisierung **1** ein (sofern nicht anders angegeben, um ausführlichere Ablaufverfolgungen zu erstellen), und drücken Sie die EINGABETASTE.
4. Geben Sie für die Benutzermodus-Ablaufverfolgungsebene der Azure-Dateisynchronisierung **1** ein (sofern nicht anders angegeben, um ausführlichere Ablaufverfolgungen zu erstellen), und drücken Sie die EINGABETASTE.
5. Reproduzieren Sie das Problem. Klicken Sie abschließend auf **D**.
6. Eine ZIP-Datei, die Protokolle und Ablaufverfolgungsdateien enthält, wird im angegebenen Ausgabeverzeichnis gespeichert.

## <a name="see-also"></a>Weitere Informationen
- [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md)
- [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)

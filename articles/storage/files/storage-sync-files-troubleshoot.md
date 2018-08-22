---
title: Problembehandlung bei Azure File Sync | Microsoft-Dokumentation
description: Beheben von häufigen Problemen bei der Azure-Dateisynchronisierung
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: e0c9708107139ec899cd5902a68ff90b57b741f7
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005918"
---
# <a name="troubleshoot-azure-file-sync"></a>Problembehandlung für Azure-Dateisynchronisierung
Mit Azure File Sync können Sie die Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit Azure File Sync werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, NFS und FTPS. Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält Informationen zur Behebung von Fehlern und Lösung von Problemen, die möglicherweise bei ihrer Azure File Sync-Bereitstellung auftreten. Wir beschreiben außerdem, wie Sie wichtige Protokolle aus dem System erfassen, wenn eine detailliertere Untersuchung des Problems erforderlich ist. Wenn Sie hier keine Antwort auf Ihre Frage finden, können Sie sich über die folgenden Kanäle an uns wenden (Eskalationsreihenfolge):

1. [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
2. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)
3. Microsoft-Support. Wählen Sie zum Erstellen einer neuen Supportanfrage im Azure-Portal auf der Registerkarte **Hilfe** die Schaltfläche **Hilfe und Support** und anschließend die Option **Neue Supportanfrage**.

## <a name="im-having-an-issue-with-azure-file-sync-on-my-server-sync-cloud-tiering-etc-should-i-remove-and-recreate-my-server-endpoint"></a>Es besteht ein Problem mit Azure File Sync auf dem Server (Synchronisierung, Cloudtiering usw.). Soll der Serverendpunkt entfernt und neu erstellt werden?
[!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## <a name="agent-installation-and-server-registration"></a>Agent-Installation und Serverregistrierung
<a id="agent-installation-failures"></a>**Beheben von Fehlern bei der Agent-Installation**  
Wenn bei der Installation des Azure File Sync-Agents ein Fehler auftritt, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um die Protokollierung während der Installation des Agents zu aktivieren:

```
StorageSyncAgent.msi /l*v AFSInstaller.log
```

Überprüfen Sie „installer.log“, um die Ursache des Installationsfehlers zu bestimmen.

<a id="agent-installation-on-DC"></a>**Bei der Agent-Installation tritt ein Fehler auf dem Active Directory-Domänencontroller auf.**  
Wenn Sie versuchen, den Synchronisierungs-Agent auf einem Active Directory-Domänencontroller zu installieren, bei dem sich der Besitzer der PDC-Rolle unter Windows Server 2008 R2 oder einer älteren Version des Betriebssystems befindet, tritt bei der Installation des Synchronisierungs-Agents möglicherweise ein Fehler auf.

Um dieses Problem zu beheben, übertragen Sie die PDC-Rolle auf einen anderen Domänencontroller unter Windows Server 2012 R2 oder höher und installieren dann den Synchronisierungsdienst.

<a id="server-registration-missing"></a>**Der Server ist im Azure-Portal nicht unter „Registrierte Server“ aufgeführt.**  
Wenn ein Server für einen Speichersynchronisierungsdienst nicht unter **Registrierte Server** aufgeführt wird:
1. Melden Sie sich bei dem Server an, den Sie registrieren möchten.
2. Öffnen Sie den Datei-Explorer, und wechseln Sie dann zum Installationsverzeichnis des Storage-Synchronisierungs-Agents (der Standardspeicherort ist C:\Programme\Azure\StorageSyncAgent). 
3. Führen Sie „ServerRegistration.exe“ aus, und schließen Sie den Assistenten ab, um den Server bei einem Storage-Synchronisierungsdienst zu registrieren.

<a id="server-already-registered"></a>**Bei der Serverregistrierung wird während der Installation des Azure File Sync-Agents die folgende Meldung angezeigt: „Dieser Server ist bereits registriert.“** 

![Screenshot des Dialogfelds für die Serverregistrierung mit der Fehlermeldung „Dieser Server wurde bereits registriert“](media/storage-sync-files-troubleshoot/server-registration-1.png)

Diese Meldung wird angezeigt, wenn der Server zuvor bei einem Speichersynchronisierungsdienst registriert wurde. Wenn Sie die Registrierung des Servers beim aktuellen Speichersynchronisierungsdienst aufheben und ihn bei einem neuen Speichersynchronisierungsdienst registrieren möchten, führen Sie die unter [Aufheben der Registrierung eines Servers bei Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) beschriebenen Schritte aus.

Wenn der Server nicht im Speichersynchronisierungsdienst unter **Registrierte Server** aufgeführt ist, können Sie auf dem Server, dessen Registrierung Sie aufheben möchten, die folgenden PowerShell-Befehle ausführen:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Wenn der Server Bestandteil eines Clusters ist, können Sie den optionalen Parameter *Reset-StorageSyncServer -CleanClusterRegistration* verwenden, um die Clusterregistrierung ebenfalls aufzuheben.

<a id="web-site-not-trusted"></a>**Beim Registrieren eines Servers erhalte ich zahlreiche Antworten, dass eine Website nicht vertrauenswürdig sei. Warum?**  
Dieses Problem tritt auf, wenn die Richtlinie **Erhöhte Internet Explorer-Sicherheit** bei der Serverregistrierung aktiviert war. Weitere Informationen zum ordnungsgemäßen Deaktivieren der Richtlinie **Erhöhte Internet Explorer-Sicherheit** finden Sie unter [Vorbereiten von Windows Server für die Verwendung mit Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) und [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Verwaltung von Synchronisierungsgruppen
<a id="cloud-endpoint-using-share"></a>**Fehler bei der Erstellung des Cloudendpunkts mit der Meldung: „The specified Azure FileShare is already in use by a different CloudEndpoint“ (Die angegebene Azure-Dateifreigabe wird bereits von einem anderen Cloudendpunkt verwendet)**  
Dieses Problem tritt auf, wenn die Azure-Dateifreigabe bereits von einem anderen Cloudendpunkt verwendet wird. 

Wenn diese Nachricht erscheint und die Azure-Dateifreigabe derzeit von keinem Cloudendpunkt verwendet wird, sollten Sie die folgenden Schritte ausführen, um die Azure File Sync-Metadaten auf der Azure-Dateifreigabe zu löschen:

> [!Warning]  
> Wenn die Metadaten auf einer Azure-Dateifreigabe gelöscht werden, die derzeit von einem Cloudendpunkt verwendet wird, treten bei Azure File Sync-Vorgängen Fehler auf. 

1. Navigieren Sie im Azure-Portal zu Ihrer Azure-Dateifreigabe.  
2. Klicken Sie mit der rechten Maustaste auf die Azure-Dateifreigabe, und wählen Sie dann **Metadaten bearbeiten** aus.
3. Klicken Sie mit der rechten Maustaste auf **SyncService**, und wählen Sie dann **Löschen** aus.

<a id="cloud-endpoint-authfailed"></a>**Fehler beim Erstellen des Cloudendpunkts: „AuthorizationFailed“**  
Dieses Problem tritt auf, wenn Ihr Benutzerkonto nicht über die erforderlichen Rechte verfügt, um einen Cloudendpunkt zu erstellen. 

Für die Erstellung eines Cloudendpunkts muss Ihr Benutzerkonto über die folgenden Microsoft-Autorisierungsberechtigungen verfügen:  
* Lesen: Rollendefinition abrufen
* Schreiben: Benutzerdefinierte Rollendefinition erstellen oder aktualisieren
* Lesen: Rollenzuweisung abrufen
* Schreiben: Rollenzuweisung erstellen

Die folgenden integrierten Rollen verfügen über die erforderlichen Microsoft-Autorisierungsberechtigungen:  
* Owner (Besitzer)
* Benutzerzugriffsadministrator

So bestimmen Sie, ob Ihr Benutzerkonto über die erforderlichen Berechtigungen verfügt:  
1. Wählen Sie im Azure-Portal **Ressourcengruppen** aus.
2. Wählen Sie die Ressourcengruppe aus, in der sich das Speicherkonto befindet, und wählen Sie dann **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie die **Rolle** (z.B. Besitzer oder Mitwirkender) für Ihr Benutzerkonto aus.
4. Wählen Sie in der Liste **Ressourcenanbieter** die Option **Microsoft-Autorisierung** aus. 
    * **Rollenzuweisung** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.
    * **Rollendefinition** muss die Berechtigungen **Lesen** und **Schreiben** aufweisen.

<a id="server-endpoint-createjobfailed"></a>**Fehler beim Erstellen des Serverendpunkts: „MgmtServerJobFailed“ (Fehlercode: -2134375898)**  
Dieses Problem tritt auf, wenn sich der Serverendpunktpfad auf dem Systemvolume befindet und Cloudtiering aktiviert ist. Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.

<a id="server-endpoint-deletejobexpired"></a>**Fehler beim Löschen des Serverendpunkts: „MgmtServerJobExpired“**                
Dieses Problem tritt auf, wenn der Server offline ist oder keine Netzwerkkonnektivität aufweist. Ist der Server nicht mehr verfügbar, heben Sie die Registrierung des Servers im Portal auf, wodurch die Serverendpunkte gelöscht werden. Um die Serverendpunkte zu löschen, führen Sie die Schritte aus, die unter [Aufheben der Registrierung eines Servers mit Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service) beschrieben sind.

<a id="server-endpoint-provisioningfailed"></a>**Die Seite „Eigenschaften des Serverendpunkts“ kann nicht geöffnet werden, oder die Cloudtiering-Richtlinie kann nicht aktualisiert werden.**  
Dieses Problem kann auftreten, wenn bei einem Verwaltungsvorgang auf dem Serverendpunkt ein Fehler auftritt. Wenn die Seite „Eigenschaften des Serverendpunkts“ nicht im Azure-Portal geöffnet wird, kann ein Aktualisieren des Serverendpunkts mithilfe von PowerShell-Befehlen auf dem Server dieses Problem beheben. 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzureRmStorageSyncServerEndpoint `
    -SubscriptionId mysubguid `
    -ResourceGroupName myrgname `
    -StorageSyncServiceName storagesvcname `
    -SyncGroupName mysyncgroup

# Update the free space percent policy for the server endpoint
Set-AzureRmStorageSyncServerEndpoint `
    -Id serverendpointid `
    -CloudTiering true `
    -VolumeFreeSpacePercent 60
```

## <a name="sync"></a>Synchronisierung
<a id="afs-change-detection"></a>**Wie lange dauert es, bis eine Datei auf Servern in der Synchronisierungsgruppe synchronisiert wird, wenn ich die Datei direkt auf meiner Azure-Dateifreigabe mithilfe von SMB oder über das Portal erstellt habe?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="serverendpoint-pending"></a>**Die Serverendpunktintegrität befindet sich seit mehreren Stunden im Status „Ausstehend“.**  
Dieses Problem ist zu erwarten, wenn Sie einen Cloudendpunkt erstellen und eine Azure-Dateifreigabe verwenden, die Daten enthält. Der Änderungsenumerationsauftrag, der auf Änderungen in der Azure-Dateifreigabe prüft, muss abgeschlossen sein, bevor Dateien zwischen der Cloud und den Serverendpunkten synchronisiert werden können. Der Zeitaufwand für die Auftragsausführung hängt von der Größe des Namespace in der Azure-Dateifreigabe ab. Sobald der Änderungsenumerationsauftrag abgeschlossen ist, sollte die Serverendpunktintegrität aktualisiert werden.

### <a id="broken-sync"></a>Wie überwache ich die Integrität der Synchronisierung?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
In den einzelnen Synchronisierungsgruppen können Sie ein Drilldown auf die jeweiligen Serverendpunkte ausführen, um den Status der zuletzt abgeschlossenen Synchronisierungssitzungen anzuzeigen. Ein grünes Symbol in der Spalte „Integrität“ und der Wert „0“ unter „Dateien ohne Synchronisierung“ geben an, dass die Synchronisierung wie erwartet funktioniert. Wenn dies nicht der Fall sein sollte, sehen Sie sich nachfolgend die Liste mit allgemeinen Synchronisierungsfehlern und die Informationen an, wie Sie Dateien, die nicht synchronisiert wurden, behandeln. 

![Ein Screenshot des Azure-Portals](media/storage-sync-files-troubleshoot/portal-sync-health.png)

# <a name="servertabserver"></a>[Server](#tab/server)
Navigieren Sie zu den Telemetrieprotokollen des Servers, die Sie in der Ereignisanzeige unter `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry` finden. Das Ereignis 9102 entspricht einer abgeschlossenen Synchronisierungssitzung. Um den aktuellen Status einer Synchronisierung anzuzeigen, suchen Sie das aktuelle Ereignis mit der ID 9102. Über SyncDirection erfahren Sie, ob es sich bei dieser Sitzung um einen Upload oder Download gehandelt hat. Wenn HResult „0“ lautet, war eine Synchronisierungssitzung erfolgreich. Ein HResult ungleich „0“ bedeutet, dass während der Synchronisierung ein Fehler aufgetreten ist. Eine Liste von häufigen Fehlern finden Sie unten. Wenn die PerItemErrorCount größer als 0 ist, bedeutet dies, dass bestimmte Dateien oder Ordner nicht ordnungsgemäß synchronisiert wurden. Es kann vorkommen, dass ein HResult von „0“ angibt, aber eine PerItemErrorCount angegeben wird, die größer als 0 ist.

Im Folgenden wird ein Beispiel für einen erfolgreichen Upload vorgestellt. Der Übersichtlichkeit halber werden nachfolgend nur einige der Werte aufgeführt, die in den einzelnen 9102-Ereignissen enthalten sind. 

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: 0, 
SyncFileCount: 2, SyncDirectoryCount: 0,
AppliedFileCount: 2, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0,
TransferredFiles: 2, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Im Gegensatz dazu kann ein fehlerhafter Upload wie folgt aussehen:

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: -2134364065,
SyncFileCount: 0, SyncDirectoryCount: 0, 
AppliedFileCount: 0, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0, 
TransferredFiles: 0, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Gelegentlich tritt bei allen Synchronisierungssitzungen ein Fehler auf, oder PerItemErrorCount ist ungleich NULL, obwohl trotzdem ein Fortschritt angezeigt wird und einige Dateien dabei erfolgreich synchronisiert werden. Dies kann bei den Feldern beobachtet werden, die mit „Applied*“ beginnen (AppliedFileCount AppliedDirCount, AppliedTombstoneCount und AppliedSizeBytes). Diese geben den Anteil an, zu dem die Sitzung zum jeweiligen Zeitpunkt durchgeführt ist. Wenn mehrere Synchronisierungssitzungen in einer Zeile angezeigt werden, bei denen Fehler auftreten, die jedoch eine steigende Zahl in einem „Applied*“-Feld angeben, sollten Sie warten, bis die Synchronisierung abgeschlossen ist und es erneut versuchen. Erst dann sollten Sie auf die Möglichkeit zum Öffnen eines Supporttickets zurückgreifen.

---

### <a name="how-do-i-monitor-the-progress-of-a-current-sync-session"></a>Wie überwache ich den Fortschritt einer aktuellen Synchronisierungssitzung?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Navigieren Sie in Ihrer Synchronisierungsgruppe zum betreffenden Serverendpunkt und dann zum Abschnitt „Synchronisierungsaktivität“. Dort wird die Anzahl der Dateien angezeigt, die Sie in der aktuellen Synchronisierungssitzung hoch- oder heruntergeladen haben. Beachten Sie, dass dieser Status mit einer Verzögerung von ca. 5 Minuten angezeigt wird. Wenn Ihre Synchronisierungssitzung so kurz ist, dass sie innerhalb dieses Zeitraums abgeschlossen wird, wird sie eventuell nicht im Portal gemeldet. 

# <a name="servertabserver"></a>[Server](#tab/server)
Suchen Sie das aktuelle Ereignis 9302 im Telemetrieprotokoll auf dem Server (in der Ereignisanzeige unter „Anwendungs- und Dienstprotokolle\Microsoft\FileSync\Agent\Telemetry“). Dieses Ereignis gibt den Zustand der aktuellen Synchronisierungssitzung an. TotalItemCount gibt die Anzahl der zu synchronisierenden Dateien, AppliedItemCount die Anzahl der bisher synchronisierten Dateien und PerItemErrorCount die Anzahl der Dateien an, bei denen die Synchronisierung zu einem Fehler führte (weitere Informationen diesbezüglich finden Sie unten).

```
Replica Sync Progress. 
ServerEndpointName: <CI>sename</CI>, SyncGroupName: <CI>sgname</CI>, ReplicaName: <CI>rname</CI>, 
SyncDirection: Upload, CorrelationId: {AB4BA07D-5B5C-461D-AAE6-4ED724762B65}. 
AppliedItemCount: 172473, TotalItemCount: 624196. AppliedBytes: 51473711577, 
TotalBytes: 293363829906. 
AreTotalCountsFinal: true. 
PerItemErrorCount: 1006.
```
---

### <a name="how-do-i-know-if-my-servers-are-in-sync-with-each-other"></a>Wie erkenne ich, ob meine Server miteinander synchronisiert sind?
# <a name="portaltabportal1"></a>[Portal](#tab/portal1)
Stellen Sie bei jedem Server in einer bestimmten Synchronisierungsgruppe Folgendes sicher:
- Die Zeitstempel für „Letzter Synchronisierungsversuch“ für Uploads und Downloads sind aktuell.
- Der Status ist sowohl für Uploads als auch für Downloads grün.
- Das Feld „Synchronisierungsaktivität“ zeigt, dass nur sehr wenige oder keine Dateien mehr synchronisiert werden müssen.
- Das Feld „Dateien ohne Synchronisierung“ ist für Uploads und Downloads „0“.

# <a name="servertabserver"></a>[Server](#tab/server)
Sehen Sie sich die durchgeführten Synchronisierungssitzungen an, die von 9102-Ereignissen im Telemetrieereignisprotokoll für die einzelnen Server gekennzeichnet sind (in der Ereignisanzeige unter `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`). 

1. Auf dem betreffenden Server sollten Sie sicherstellen, dass die neuesten Upload- und Downloadsitzungen erfolgreich abgeschlossen wurden. Überprüfen Sie hierfür, ob das HResult und die PerItemErrorCount für Uploads und Downloads „0“ lauten (das Feld „SyncDirection“ gibt an, ob es sich bei einer Sitzung um eine Upload- oder eine Downloadsitzung handelt). Hinweis: Wenn eine kürzlich abgeschlossene Synchronisierungssitzung nicht angezeigt wird, wird aller Wahrscheinlichkeit nach momentan eine Synchronisierungssitzung ausgeführt. Dies ist normal, wenn Sie gerade eine große Menge von Daten hinzugefügt oder geändert haben.
2. Wenn ein Server vollständig mit der Cloud synchronisiert ist und es in beiden Richtungen keine zu synchronisierenden Änderungen gibt, werden leere Synchronisierungssitzungen angezeigt. Diese werden durch Upload- und Downloadereignisse angegeben, bei denen alle Felder, die mit „Sync*“ beginnen (SyncFileCount, SyncDirCount, SyncTombstoneCount und SyncSizeBytes), NULL sind. Dies bedeutet, dass es keine zu synchronisierenden Änderungen gab. Beachten Sie, dass diese leeren Synchronisierungssitzungen möglicherweise nicht auf Servern mit hohem Änderungsumfang angezeigt werden, da es laufend zu synchronisierende Änderungen gibt. Liegt keine Synchronisierungsaktivität vor, sollte sie alle 30 Minuten ausgeführt werden. 
3. Wenn alle Server mit der Cloud synchronisiert wurden (d.h., die aktuellen Upload- und Downloadsitzungen sind leere Synchronisierungssitzungen), können Sie mit hoher Wahrscheinlichkeit davon ausgehen, dass das gesamte System synchron ist. 
    
Beachten Sie, dass Azure File Sync Änderungen, die Sie direkt in Ihrer Azure-Dateifreigabe vorgenommen haben, erst erkennt, wenn die Änderungsenumeration ausgeführt wird (tritt einmal alle 24 Stunden auf). Es kann vorkommen, dass ein Server als mit der Cloud synchron angegeben wird, obwohl in Wirklichkeit in letzter Zeit keine Änderungen direkt in der Azure-Dateifreigabe vorgenommen wurden. 

---

### <a name="how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing"></a>Woran erkenne ich, dass bestimmte Dateien oder Ordnern nicht synchronisiert wurden?
Wenn im Portal die PerItemErrorCount auf dem Server oder „Dateien ohne Synchronisierung“ für eine bestimmte Synchronisierungssitzung einen größeren Wert als „0“ enthält, bedeutet dies, dass bei einigen Elementen ein Synchronisierungsfehler aufgetreten ist. Dateien und Ordner können Merkmale aufweisen, die eine Synchronisierung verhindern. Diese Merkmale können dauerhaft sein und eine explizite Aktion zum Fortsetzen der Synchronisierung erfordern, z.B. das Entfernen von nicht unterstützten Zeichen aus einem Datei- oder Ordnernamen. Sie können auch vorübergehend sein, d.h., die Synchronisierung der Datei oder des Ordners wird automatisch fortgesetzt. Beispielsweise wird die Synchronisierung von Dateien mit offenen Handles automatisch fortgesetzt, wenn die Datei geschlossen ist. Wenn die Azure File Sync-Engine ein solches Problem erkennt, wird ein Fehlerprotokoll erstellt, das für die Auflistung der Elemente, die derzeit nicht ordnungsgemäß synchronisiert werden, analysiert werden kann.

Um diese Fehler anzuzeigen, führen Sie das PowerShell-Skript **FileSyncErrorsReport.ps1** aus (im Agent-Installationsverzeichnis des Azure File Sync-Agents), um Dateien zu identifizieren, bei denen die Synchronisierung aufgrund von offenen Handles, nicht unterstützten Zeichen oder anderen Problemen zu Fehlern führte. Das ItemPath-Feld gibt den Speicherort der Datei in Bezug auf das Stammverzeichnis für die Synchronisierung an. Eine Liste von allgemeinen Synchronisierungsfehlern mit Schritten zur Fehlerbehebung finden Sie weiter unten.

#### <a name="troubleshooting-per-filedirectory-sync-errors"></a>Behandlung von Synchronisierungsfehlern nach Dateien und Verzeichnissen
**ItemResults-Protokoll – Synchronisierungsfehler nach Element**  
| HRESULT | HRESULT (dezimal) | Fehlerzeichenfolge | Problem | Wiederherstellung |
|---------|-------------------|--------------|-------|-------------|
| 0x80c80065 | -2134376347 | ECS_E_DATA_TRANSFER_BLOCKED | Die Datei hat persistente Fehler bei der Synchronisierung erzeugt, und daher wird die Synchronisierung nur einmal pro Tag ausgeführt. Den zugrunde liegenden Fehler finden Sie in einem älteren Ereignisprotokoll. | Bei R2-Agents mit der Version 2.0 und höher wird anstelle dieses Fehlers der ursprüngliche Fehler angezeigt. Führen Sie ein Upgrade auf den neuesten Agent durch, um den zugrunde liegenden Fehler anzuzeigen, oder sehen Sie sich ältere Ereignisprotokolle an, um die Ursache des ursprünglichen Fehlers zu ermitteln. |
| 0x7b | 123 | ERROR_INVALID_NAME | Der Datei- oder Verzeichnisname ist ungültig. | Benennen Sie die jeweiligen Datei oder das betreffende Verzeichnis um. Weitere Informationen finden Sie unter [Richtlinien für die Benennung in Azure Files](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) und in der Liste von nicht unterstützten Zeichen weiter unten. |
| 0x8007007b | -2147024773 | STIERR_INVALID_DEVICE_NAME | Der Datei- oder Verzeichnisname ist ungültig. | Benennen Sie die jeweiligen Datei oder das betreffende Verzeichnis um. Weitere Informationen finden Sie unter [Richtlinien für die Benennung in Azure Files](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) und in der Liste von nicht unterstützten Zeichen weiter unten. |
| 0x80c8031d | -2134375651 | ECS_E_CONCURRENCY_CHECK_FAILED | Eine Datei wurde geändert, doch die Änderung wurde noch nicht von der Synchronisierung erkannt. Die Synchronisierung wird aktualisiert, sobald diese Änderung erkannt wurde. | Keine weiteren Maßnahmen erforderlich. |
| 0x80c80018 | -2134376424 | ECS_E_SYNC_FILE_IN_USE | Eine Datei kann nicht synchronisiert werden, da sie momentan verwendet wird. Die Datei wird synchronisiert, wenn sie nicht mehr verwendet wird. | Keine weiteren Maßnahmen erforderlich. Azure File Sync erstellt einmal pro Tag eine temporäre VSS-Momentaufnahme auf dem Server, um Dateien mit offenen Handles zu synchronisieren. |
| 0x20 | 32 | ERROR_SHARING_VIOLATION | Eine Datei kann nicht synchronisiert werden, da sie momentan verwendet wird. Die Datei wird synchronisiert, wenn sie nicht mehr verwendet wird. | Keine weiteren Maßnahmen erforderlich. |
| 0x80c80207 | -2134375929 | ECS_E_SYNC_CONSTRAINT_CONFLICT | Eine Datei- oder Verzeichnisänderung kann noch nicht synchronisiert werden, da ein abhängiger Ordner noch nicht synchronisiert ist. Dieses Element wird synchronisiert, sobald die abhängigen Änderungen synchronisiert wurden. | Keine weiteren Maßnahmen erforderlich. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Eine Datei wurde während der Synchronisierung geändert, deshalb muss sie erneut synchronisiert werden. | Keine weiteren Maßnahmen erforderlich. |

#### <a name="handling-unsupported-characters"></a>Behandlung von nicht unterstützten Zeichen
Wenn das PowerShell-Skript **FileSyncErrorsReport.ps1** Fehler aufgrund von nicht unterstützten Zeichen (Fehlercodes 0x7b und 0x8007007b) anzeigt, sollten Sie die jeweiligen Zeichen aus den entsprechenden Dateien entfernen oder ändern. PowerShell gibt diese Zeichen wahrscheinlich als Fragezeichen oder leere Rechtecke aus, da die meisten dieser Zeichen keine standardisierte visuelle Codierung aufweisen.

Die folgende Tabelle enthält alle Unicode-Zeichen, die Azure File Sync noch nicht unterstützt.

| Zeichensatz | Zeichenanzahl |
|---------------|-----------------|
| <ul><li>0x0000009D (Operating System Command, OSC)</li><li>0x00000090 (Device Control String, DCS)</li><li>0x0000008F (Single Shift Three, SS3)</li><li>0x00000081 (High Octet Preset)</li><li>0x0000007F (Delete, DEL)</li><li>0x0000008D (Reverse Line Feed, RI)</li></ul> | 6 |
| <ul><li>0x0000200F (Right-to-Left Mark)</li><li>0x0000200E (Left-to-Right Mark)</li><li>0x0000202E (Right-to-Left Override)</li><li>0x0000202D (Left-to-Right Override)</li><li>0x0000202C (Pop Directional Formatting)</li><li>0x0000202B (Right-to-Left Embedding)</li><li>0x0000202A (Left-to-Right Embedding)</li></ul> | 7 |
| 0x0000FDD0–0x0000FDEF (arabische Darstellungsformen) | 32 |
| 0x0000FFF0–0x0000FFFF (Sonderzeichen) | 16 |
| <ul><li>0x0001FFFE–0x0001FFFF = 2 (Nicht-Zeichen)</li><li>0x0002FFFE–0x0002FFFF = 2 (Nicht-Zeichen)</li><li>0x0003FFFE–0x0003FFFF = 2 (Nicht-Zeichen)</li><li>0x0004FFFE–0x0004FFFF = 2 (Nicht-Zeichen)</li><li>0x0005FFFE–0x0005FFFF = 2 (Nicht-Zeichen)</li><li>0x0006FFFE–0x0006FFFF = 2 (Nicht-Zeichen)</li><li>0x0007FFFE–0x0007FFFF = 2 (Nicht-Zeichen)</li><li>0x0008FFFE–0x0008FFFF = 2 (Nicht-Zeichen)</li><li>0x0009FFFE–0x0009FFFF = 2 (Nicht-Zeichen)</li><li>0x000AFFFE–0x000AFFFF = 2 (Nicht-Zeichen)</li><li>0x000BFFFE–0x000BFFFF = 2 (Nicht-Zeichen)</li><li>0x000CFFFE–0x000CFFFF = 2 (Nicht-Zeichen)</li><li>0x000DFFFE–0x000DFFFF = 2 (Nicht-Zeichen)</li><li>0x000EFFFE–0x000EFFFF = 2 (nicht definiert)</li><li>0x000FFFFE–0x000FFFFF = 2 (zusätzlicher privater Verwendungsbereich)</li></ul> | 30 |
| 0x0010FFFE, 0x0010FFFF | 2 |

### <a name="common-sync-errors"></a>Allgemeine Synchronisierungsfehler
<a id="-2147023673"></a>**Die Synchronisierungssitzung wurde abgebrochen.**  
| | |
|-|-|
| **HRESULT** | 0x800704c7 |
| **HRESULT (dezimal)** | -2147023673 | 
| **Fehlerzeichenfolge** | ERROR_CANCELLED |
| **Korrektur erforderlich** | Nein  |

Bei Synchronisierungssitzungen kann aus verschiedenen Gründen ein Fehler auftreten, z.B. aufgrund eines Serverneustarts oder einer Serveraktualisierung oder aufgrund von VSS-Momentaufnahmen. Auch wenn dieser Fehler den Anschein erweckt, als ob eine Nachverfolgung erforderlich wäre, kann dieser ignoriert werden, es sei denn, er bleibt über einen Zeitraum von mehreren Stunden bestehen.

<a id="-2147012889"></a>**Eine Verbindung mit dem Dienst konnte nicht hergestellt werden.**    
| | |
|-|-|
| **HRESULT** | 0x80072ee7 |
| **HRESULT (dezimal)** | -2147012889 | 
| **Fehlerzeichenfolge** | WININET_E_NAME_NOT_RESOLVED |
| **Korrektur erforderlich** | JA |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134376372"></a>**Die Benutzeranforderung wurde durch den Dienst gedrosselt.**  
| | |
|-|-|
| **HRESULT** | 0x80c8004c |
| **HRESULT (dezimal)** | -2134376372 |
| **Fehlerzeichenfolge** | ECS_E_USER_REQUEST_THROTTLED |
| **Korrektur erforderlich** | Nein  |

Es ist keine Aktion erforderlich. Der Server wiederholt den Vorgang. Erstellen Sie nur dann eine Supportanfrage, wenn dieser Fehler länger als ein paar Stunden anhält.

<a id="-2134364065"></a>**Die Synchronisierung kann nicht auf die im Cloudendpunkt angegebene Azure-Dateifreigabe zugreifen.**  
| | |
|-|-|
| **HRESULT** | 0x80c8305f |
| **HRESULT (dezimal)** | -2134364065 |
| **Fehlerzeichenfolge** | ECS_E_CANNOT_ACCESS_EXTERNAL_STORAGE_ACCOUNT |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, da der Azure File Sync-Agent nicht auf die Azure-Dateifreigabe zugreifen kann. Dies ist möglicherweise darauf zurückzuführen, dass die Azure-Dateifreigabe oder das Speicherkontohosting nicht mehr vorhanden ist. Sie können diesen Fehler beheben, indem Sie die folgenden Schritte durchführen:

1. [Überprüfen Sie, ob das Speicherkonto vorhanden ist.](#troubleshoot-storage-account)
2. [Stellen Sie sicher, dass das Speicherkonto keine Netzwerkregeln enthält.](#troubleshoot-network-rules)
3. [Stellen Sie sicher, dass die Azure-Dateifreigabe vorhanden ist.](#troubleshoot-azure-file-share)
4. [Stellen Sie sicher, dass Azure File Sync über Zugriff auf das Speicherkonto verfügt.](#troubleshoot-rbac)

<a id="-2134364064"></a><a id="cannot-resolve-storage"></a>**Der verwendete Speicherkontoname konnte nicht aufgelöst werden.**  
| | |
|-|-|
| **HRESULT** | 0x80C83060 |
| **HRESULT (dezimal)** | -2134364064 |
| **Fehlerzeichenfolge** | ECS_E_STORAGE_ACCOUNT_NAME_UNRESOLVED |
| **Korrektur erforderlich** | JA |

1. Überprüfen Sie, ob Sie den Speicher-DNS-Namen vom Server auflösen können.

    ```PowerShell
    Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 443
    ```
2. [Überprüfen Sie, ob das Speicherkonto vorhanden ist.](#troubleshoot-storage-account)
3. [Stellen Sie sicher, dass das Speicherkonto keine Netzwerkregeln enthält.](#troubleshoot-network-rules)

<a id="-1906441138"></a>**Fehler bei der Synchronisierung aufgrund eines Problems mit der Synchronisierungsdatenbank.**  
| | |
|-|-|
| **HRESULT** | 0x8e5e044e |
| **HRESULT (dezimal)** | -1906441138 |
| **Fehlerzeichenfolge** | JET_errWriteConflict |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, wenn ein Problem mit der internen Datenbank besteht, die von Azure File Sync verwendet wird. Wenn dieses Problem auftritt, erstellen Sie eine Supportanfrage, und wir kontaktieren Sie, um Sie bei der Problemlösung zu unterstützen.

<a id="-2134351810"></a>**Sie haben das Speicherlimit für die Azure-Dateifreigabe erreicht.**  
| | |
|-|-|
| **HRESULT** | 0x80c8603e |
| **HRESULT (dezimal)** | -2134351810 |
| **Fehlerzeichenfolge** | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, wenn das Speicherlimit für die Azure Dateifreigabe erreicht wurde. Dies kann vorkommen, wenn ein Kontingent für eine Azure-Dateifreigabe angewendet wird oder die Nutzung die Grenzwerte einer Azure-Dateifreigabe überschreitet. Weitere Informationen finden Sie in den [aktuellen Grenzwerten für eine Azure-Dateifreigabe](storage-files-scale-targets.md).

1. Navigieren Sie zu der Synchronisierungsgruppe im Speichersynchronisierungsdienst.
2. Wählen Sie den Cloudendpunkt innerhalb der Synchronisierungsgruppe aus.
3. Beachten Sie den Namen der Azure-Dateifreigabe im geöffneten Bereich.
4. Wählen Sie das verknüpfte Speicherkonto aus. Wenn bei dieser Verknüpfung ein Fehler auftritt, wurde das Speicherkonto, auf das verwiesen wurde, entfernt.

    ![Ein Screenshot, das den Bereich mit Details zum Cloudendpunkt und einem Link zum Speicherkonto zeigt](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

5. Wählen Sie **Dateien** zum Anzeigen der Liste von Dateifreigaben aus.
6. Klicken Sie auf die drei Punkte am Ende der Zeile für die Azure-Dateifreigabe, auf die vom Cloudendpunkt verwiesen wird.
7. Überprüfen Sie, ob die **Nutzung** unterhalb des **Kontingents** liegt. Beachten Sie, dass das Kontingent der [maximalen Größe der Azure-Dateifreigabe](storage-files-scale-targets.md) entspricht, es sei denn, ein alternatives Kontingent wurde festgelegt.

    ![Ein Screenshot der Eigenschaften einer Azure Dateifreigabe](media/storage-sync-files-troubleshoot/file-share-limit-reached-1.png)

Wenn die Freigabe ausgeschöpft ist und kein Kontingent festgelegt ist, besteht eine Möglichkeit zur Behebung dieses Problems darin, jeden Unterordner des aktuellen Serverendpunkts in einen eigenen-Serverendpunkt in separaten Synchronisierungsgruppen umzuwandeln. Dadurch wird jeder Unterordner mit individuellen Azure-Dateifreigaben synchronisiert.

<a id="-2134351824"></a>**Die gelöschte Azure-Dateifreigabe kann nicht gefunden werden.**  
| | |
|-|-|
| **HRESULT** | 0x80c86030 |
| **HRESULT (dezimal)** | -2134351824 |
| **Fehlerzeichenfolge** | ECS_E_AZURE_FILE_SHARE_NOT_FOUND |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, wenn auf die Azure-Dateifreigabe nicht zugegriffen werden kann. Führen Sie zur Problembehandlung folgende Schritte durch:

1. [Überprüfen Sie, ob das Speicherkonto vorhanden ist.](#troubleshoot-storage-account)
2. [Stellen Sie sicher, dass die Azure-Dateifreigabe vorhanden ist.](#troubleshoot-azure-file-share)

Falls die Azure-Dateifreigabe gelöscht wurde, müssen Sie eine neue Dateifreigabe erstellen und die Synchronisierungsgruppe anschließend neu erstellen. 

<a id="-2134364042"></a>**Die Synchronisierung wird angehalten, solange dieses Azure-Abonnement ausgesetzt ist.**  
| | |
|-|-|
| **HRESULT** | 0x80C83076 |
| **HRESULT (dezimal)** | -2134364042 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_BLOCKED_ON_SUSPENDED_SUBSCRIPTION |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, wenn das Azure-Abonnement ausgesetzt ist. Die Synchronisierung wird erneut aktiviert, wenn das Azure-Abonnement wiederhergestellt wurde. Weitere Informationen finden Sie unter [Warum ist mein Azure-Abonnement deaktiviert, und wie reaktiviere ich es?](../../billing/billing-subscription-become-disable.md).

<a id="-2134364052"></a>**Für das Speicherkonto wurden eine Firewall oder virtuelle Netzwerke konfiguriert.**  
| | |
|-|-|
| **HRESULT** | 0x80c8306c |
| **HRESULT (dezimal)** | -2134364052 |
| **Fehlerzeichenfolge** | ECS_E_MGMT_STORAGEACLSNOTSUPPORTED |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, wenn aufgrund einer Firewall oder der Zugehörigkeit des Speicherkontos zu einem virtuellen Netzwerk nicht auf die Azure-Dateifreigabe zugegriffen werden kann. Azure File Sync bietet noch keine Unterstützung für dieses Feature. Führen Sie zur Problembehandlung folgende Schritte durch:

1. [Überprüfen Sie, ob das Speicherkonto vorhanden ist.](#troubleshoot-storage-account)
2. [Stellen Sie sicher, dass das Speicherkonto keine Netzwerkregeln enthält.](#troubleshoot-network-rules)

Entfernen Sie diese Regeln, um dieses Problem zu beheben. 

<a id="-2134375911"></a>**Fehler bei der Synchronisierung aufgrund eines Problems mit der Synchronisierungsdatenbank.**  
| | |
|-|-|
| **HRESULT** | 0x80c80219 |
| **HRESULT (dezimal)** | -2134375911 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_METADATA_WRITE_LOCK_TIMEOUT |
| **Korrektur erforderlich** | Nein  |

Dieser Fehler löst sich im Allgemeinen von selbst und kann folgende Ursachen haben:

* Eine hohe Anzahl von Dateiänderungen zwischen den Servern in der Synchronisierungsgruppe
* Eine hohe Anzahl von Fehlern in einzelnen Dateien und Verzeichnissen

Wenn dieser Fehler länger anhält als ein paar Stunden, erstellen Sie eine Supportanfrage, und wir kontaktieren Sie, um Sie bei der Problemlösung zu unterstützen.

<a id="-2146762487"></a>**Der Server konnte keine sichere Verbindung herstellen. Der Clouddienst hat ein unerwartetes Zertifikat empfangen.**  
| | |
|-|-|
| **HRESULT** | 0x800b0109 |
| **HRESULT (dezimal)** | -2146762487 |
| **Fehlerzeichenfolge** | CERT_E_UNTRUSTEDROOT |
| **Korrektur erforderlich** | JA |

Dieser Fehler kann vorkommen, wenn Ihre Organisation einen SSL-Terminierungsproxy verwendet oder eine schädliche Entität den Datenverkehr zwischen Ihrem Server und Azure File Sync abfängt. Wenn Sie sicher sind, dass dies normal ist (da Ihre Organisation einen SSL-Terminierungsproxy verwendet), überspringen Sie die Zertifikatüberprüfung durch eine Außerkraftsetzung der Registrierung.

1. Erstellen Sie den Registrierungswert „SkipVerifyingPinnedRootCertificate“.

    ```PowerShell
    New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Azure\StorageSync -Name SkipVerifyingPinnedRootCertificate -PropertyType DWORD -Value 1
    ```

2. Starten Sie den Synchronisierungsdienst auf dem registrierten Server neu.

    ```PowerShell
    Restart-Service -Name FileSyncSvc -Force
    ```

Durch das Festlegen dieses Registrierungswerts akzeptiert der Azure File Sync-Agent alle vertrauenswürdigen lokalen SSL-Zertifikate, wenn Daten zwischen dem Server und dem Clouddienst übertragen werden.

<a id="-2147012894"></a>**Eine Verbindung mit dem Dienst konnte nicht hergestellt werden.**  
| | |
|-|-|
| **HRESULT** | 0x80072ee2 |
| **HRESULT (dezimal)** | -2147012894 |
| **Fehlerzeichenfolge** | WININET_E_TIMEOUT |
| **Korrektur erforderlich** | JA |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134375680"></a>**Fehler bei der Synchronisierung aufgrund eines Problems mit der Authentifizierung.**  
| | |
|-|-|
| **HRESULT** | 0x80c80300 |
| **HRESULT (dezimal)** | -2134375680 |
| **Fehlerzeichenfolge** | ECS_E_SERVER_CREDENTIAL_NEEDED |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt häufig auf, weil die Serverzeit falsch oder das für die Authentifizierung verwendete Zertifikat abgelaufen ist. Wenn die Serverzeit richtig ist, führen Sie die folgenden Schritte durch, um das abgelaufene Zertifikat (falls abgelaufen) zu löschen und den Serverregistrierungszustand zurückzusetzen:

1. Öffnen Sie das Zertifikat-MMC-Snap-In, wählen Sie das Computerkonto aus, und navigieren Sie zu den Zertifikaten: „(Lokaler Computer)\Benutzer\Certificates“.
2. Löschen Sie das Clientauthentifizierungszertifikat, sofern es abgelaufen ist, und schließen Sie das Zertifikat-MMC-Snap-In.
3. Öffnen Sie Regedit, und löschen Sie den ServerSetting-Schlüssel in der Registrierung: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync\ServerSetting
4. Führen Sie die folgenden PowerShell-Befehle auf dem Server aus:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Reset-StorageSyncServer
    ```

5. Registrieren Sie den Server erneut, indem Sie „ServerRegistration.exe“ ausführen (der Standardspeicherort befindet sich unter „C:\Programme\Microsoft Files\Azure\StorageSyncAgent“).

<a id="-1906441711"></a><a id="-2134375654"></a><a id="doesnt-have-enough-free-space"></a>**Das Volume, auf dem sich der Serverendpunkt befindet, weist nicht genügend Speicherplatz auf dem Datenträger auf.**  
| | |
|-|-|
| **HRESULT** | 0x8e5e0211 |
| **HRESULT (dezimal)** | -1906441711 |
| **Fehlerzeichenfolge** | JET_errLogDiskFull |
| **Korrektur erforderlich** | JA |
| | |
| **HRESULT** | 0x80c8031a |
| **HRESULT (dezimal)** | -2134375654 |
| **Fehlerzeichenfolge** | ECS_E_NOT_ENOUGH_LOCAL_STORAGE |
| **Korrektur erforderlich** | JA |

Dieser Fehler tritt auf, da sich das Volume gefüllt hat. Dieser Fehler tritt häufig auf, da Dateien außerhalb des Serverendpunkts auf dem Volume Speicherplatz belegen. Geben Sie Speicherplatz auf dem Volume frei, indem Sie zusätzliche Serverendpunkte hinzufügen, Dateien in ein anderes Volume verschieben oder die Größe des Volumes, in dem sich der Serverendpunkt befindet, erhöhen.

<a id="-2134364145"></a><a id="replica-not-ready"></a>**Der Dienst ist für die Synchronisierung mit diesem Serverendpunkt noch nicht bereit.**  
| | |
|-|-|
| **HRESULT** | 0x80c8300f |
| **HRESULT (dezimal)** | -2134364145 |
| **Fehlerzeichenfolge** | ECS_E_REPLICA_NOT_READY |
| **Korrektur erforderlich** | Nein  |

Dieser Fehler tritt auf, weil direkt Änderungen an der Azure-Dateifreigabe vorgenommen wurden und die Erkennung von Änderungen momentan ausgeführt wird. Die Synchronisierung beginnt nach Abschluss der Änderungserkennung.

[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="-2134375877"></a><a id="-2134375908"></a><a id="-2134375853"></a>**Fehler bei der Synchronisierung aufgrund von Problemen mit einer Vielzahl einzelner Dateien.**  
| | |
|-|-|
| **HRESULT** | 0x80c8023b |
| **HRESULT (dezimal)** | -2134364145 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED |
| **Korrektur erforderlich** | JA |
| | |
| **HRESULT** | 0x80c8021c |
| **HRESULT (dezimal)** | -2134375908 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_METADATA_KNOWLEGE_LIMIT_REACHED |
| **Korrektur erforderlich** | JA |
| | |
| **HRESULT** | 0x80c80253 |
| **HRESULT (dezimal)** | -2134375853 |
| **Fehlerzeichenfolge** | ECS_E_TOO_MANY_PER_ITEM_ERRORS |
| **Korrektur erforderlich** | JA |

In Fällen, in denen viele Synchronisierungsfehler pro Datei auftreten, treten bei den Synchronisierungssitzungen Fehler auf. Um diesen Zustand zu beheben, lesen Sie [Behandlung von Synchronisierungsfehlern nach Dateien und Verzeichnissen](#troubleshooting-per-file-directory-sync-errors).

> [!NOTE]
> Azure File Sync erstellt einmal pro Tag eine temporäre VSS-Momentaufnahme auf dem Server, um Dateien mit offenen Handles zu synchronisieren.

<a id="-2134376423"></a>**Fehler bei der Synchronisierung aufgrund eines Problems mit dem Serverendpunktpfad.**  
| | |
|-|-|
| **HRESULT** | 0x80c80019 |
| **HRESULT (dezimal)** | -2134376423 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_INVALID_PATH |
| **Korrektur erforderlich** | JA |

Stellen Sie sicher, dass der Pfad vorhanden ist, sich in einem lokalen NTFS-Volume befindet und kein Analysepunkt oder vorhandener Serverendpunkt ist.

<a id="-2134376373"></a>**Der Dienst ist derzeit nicht verfügbar.**  
| | |
|-|-|
| **HRESULT** | 0x80c8004b |
| **HRESULT (dezimal)** | -2134376373 |
| **Fehlerzeichenfolge** | ECS_E_SERVICE_UNAVAILABLE |
| **Korrektur erforderlich** | Nein  |

Dieser Fehler tritt auf, da der Azure File Sync-Dienst nicht verfügbar ist. Dieser Fehler wird automatisch aufgelöst, wenn der Azure File Sync-Dienst wieder verfügbar ist.

<a id="-2134375922"></a>**Fehler bei der Synchronisierung aufgrund eines vorübergehenden Problems mit der Synchronisierungsdatenbank.**  
| | |
|-|-|
| **HRESULT** | 0x80c8020e |
| **HRESULT (dezimal)** | -2134375922 |
| **Fehlerzeichenfolge** | ECS_E_SYNC_METADATA_WRITE_LEASE_LOST |
| **Korrektur erforderlich** | Nein  |

Dieser Fehler tritt aufgrund eines internen Problems mit der Synchronisierungsdatenbank auf. Dieser Fehler wird automatisch aufgelöst, wenn Azure File Sync die Synchronisierung wiederholt. Wenn dieser Fehler für einen längeren Zeitraum anhält, erstellen Sie eine Supportanfrage, und wir kontaktieren Sie, um Sie bei der Problemlösung zu unterstützen.

### <a name="common-troubleshooting-steps"></a>Allgemeine Schritte zur Problembehandlung
<a id="troubleshoot-storage-account"></a>**Überprüfen Sie, ob das Speicherkonto vorhanden ist.**  
# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Navigieren Sie zu der Synchronisierungsgruppe im Speichersynchronisierungsdienst.
2. Wählen Sie den Cloudendpunkt innerhalb der Synchronisierungsgruppe aus.
3. Beachten Sie den Namen der Azure-Dateifreigabe im geöffneten Bereich.
4. Wählen Sie das verknüpfte Speicherkonto aus. Wenn bei dieser Verknüpfung ein Fehler auftritt, wurde das Speicherkonto, auf das verwiesen wurde, entfernt.
    ![Ein Screenshot, das den Bereich mit Details zum Cloudendpunkt und einem Link zum Speicherkonto zeigt](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
# Variables for you to populate based on your configuration
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"
$region = "<Az_Region>"
$resourceGroup = "<RG_Name>"
$syncService = "<storage-sync-service>"
$syncGroup = "<sync-group>"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# Log into the Azure account and put the returned account information
# in a reference variable.
$acctInfo = Connect-AzureRmAccount

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = [System.String[]]@()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the " + `
        " selected Azure Region or the region is mistyped.")
}

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = [System.String[]]@()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    throw [System.Exception]::new("The provided resource group $resourceGroup does not exist.")
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region

# Check to make sure the provided Storage Sync Service
# exists.
$syncServices = [System.String[]]@()

Get-AzureRmStorageSyncService -ResourceGroupName $resourceGroup | ForEach-Object {
    $syncServices += $_.DisplayName
}

if ($storageSyncServices -notcontains $syncService) {
    throw [System.Exception]::new("The provided Storage Sync Service $syncService does not exist.")
}

# Check to make sure the provided Sync Group exists
$syncGroups = [System.String[]]@()

Get-AzureRmStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $syncService | ForEach-Object {
    $syncGroups += $_.DisplayName
}

if ($syncGroups -notcontains $syncGroup) {
    throw [System.Exception]::new("The provided sync group $syncGroup does not exist.")
}

# Get reference to cloud endpoint
$cloudEndpoint = Get-AzureRmStorageSyncCloudEndpoint `
    -ResourceGroupName $resourceGroup `
    -StorageSyncServiceName $storageSyncService `
    -SyncGroupName $syncGroup

# Get reference to storage account
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object { 
    $_.Id -eq $cloudEndpoint.StorageAccountResourceId
}

if ($storageAccount -eq $null) {
    Write-Host "The storage account referenced in the cloud endpoint does not exist."
}
```
---

<a id="troubleshoot-network-rules"></a>**Stellen Sie sicher, dass das Speicherkonto keine Netzwerkregeln enthält.**  
# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Wählen Sie im Speicherkonto auf der linken Seite **Firewalls und virtuelle Netzwerke** aus.
2. Innerhalb des Speicherkontos sollte das Optionsfeld **Zugriff von allen Netzwerken zulassen** aktiviert sein.
    ![Ein Screenshot, der die deaktivierte Firewall und Netzwerkregeln des Speicherkontos zeigt](media/storage-sync-files-troubleshoot/file-share-inaccessible-2.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
if ($storageAccount.NetworkRuleSet.DefaultAction -ne 
    [Microsoft.Azure.Commands.Management.Storage.Models.PSNetWorkRuleDefaultActionEnum]::Allow) {
    Write-Host ("The storage account referenced contains network " + `
        "rules which are not currently supported by Azure File Sync.")
}
```
---

<a id="troubleshoot-azure-file-share"></a>**Stellen Sie sicher, dass die Azure-Dateifreigabe vorhanden ist.**  
# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Klicken Sie auf der linken Seite im Inhaltsverzeichnis auf **Übersicht**, um zur Hauptseite des Speicherkontos zurückzukehren.
2. Wählen Sie **Dateien** zum Anzeigen der Liste von Dateifreigaben aus.
3. Vergewissern Sie sich, dass die Dateifreigabe, auf die durch den Cloudendpunkt verwiesen wird, in der Liste der Dateifreigaben angezeigt wird (Sie sollten diese bei Schritt 1 notiert haben).

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $cloudEndpoint.StorageAccountShareName -and
    $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    Write-Host "The Azure file share referenced by the cloud endpoint does not exist"
}
```
---

<a id="troubleshoot-rbac"></a>**Stellen Sie sicher, dass Azure File Sync über Zugriff auf das Speicherkonto verfügt.**  
# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Klicken Sie auf **Zugriffssteuerung (IAM)** auf das Inhaltsverzeichnis auf der linken Seite, um zur Liste der Benutzer und Anwendungen (*Dienstprinzipale*) zu navigieren, die Zugriff auf Ihr Speicherkonto haben.
2. Stellen Sie sicher, dass der **Hybrid File Sync-Dienst** in der Liste mit der Rolle **Lese- und Datenzugriff** angezeigt wird. 

    ![Ein Screenshot des Hybrid File Sync-Dienstprinzipals auf der Registerkarte zur Zugriffssteuerung des Speicherkontos](media/storage-sync-files-troubleshoot/file-share-inaccessible-3.png)

    Wird **Hybrid-Dateisynchronisierungsdienst** nicht in der Liste angezeigt, führen Sie die folgenden Schritte aus:

    - Klicken Sie auf **Hinzufügen**.
    - Wählen Sie im Feld **Rolle** die Option **Lese- und Datenzugriff** aus.
    - Geben Sie im Feld **Auswählen** die Zeichenfolge **Hybrid-Dateisynchronisierungsdienst** ein, wählen Sie die Rolle aus, und klicken Sie auf **Speichern**.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell    
$foundSyncPrincipal = $false
Get-AzureRmRoleAssignment -Scope $storageAccount.Id | ForEach-Object { 
    if ($_.DisplayName -eq "Hybrid File Sync Service") {
        $foundSyncPrincipal = $true
        if ($_.RoleDefinitionName -ne "Reader and Data Access") {
            Write-Host ("The storage account has the Azure File Sync " + `
                "service principal authorized to do something other than access the data " + `
                "within the referenced Azure file share.")
        }

        break
    }
}

if (!$foundSyncPrincipal) {
    Write-Host ("The storage account does not have the Azure File Sync " + `
                "service principal authorized to access the data within the " + ` 
                "referenced Azure file share.")
}
```
---

### <a name="how-do-i-prevent-users-from-creating-files-containing-unsupported-characters-on-the-server"></a>Wie verhindere ich, dass Benutzer Dateien auf dem Server erstellen, die nicht unterstützte Zeichen enthalten?
Mit den [Dateiprüfungen des Ressourcen-Managers für Dateiserver (File Server Resource Manager, FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/file-screening-management) können Sie verhindern, dass Dateien mit nicht unterstützten Zeichen in deren Namen auf dem Server erstellt werden. Möglicherweise müssen Sie dazu PowerShell verwenden, da der Großteil der nicht unterstützten Zeichen nicht druckbar ist, und daher zuerst ihre hexadezimalen Darstellungen in Zeichen umwandeln.

Erstellen Sie zunächst mit dem [Cmdlet „New-FsrmFileGroup“](https://docs.microsoft.com/powershell/module/fileserverresourcemanager/new-fsrmfilegroup) eine FSRM-Dateigruppe. Dieses Beispiel definiert, dass die Gruppe nur zwei der nicht unterstützten Zeichen enthalten darf, Sie können jedoch so viele Zeichen wie erforderlich in Ihrer Dateigruppe einschließen.

```PowerShell
New-FsrmFileGroup -Name "Unsupported characters" -IncludePattern @(("*"+[char]0x00000090+"*"),("*"+[char]0x0000008F+"*"))
```

Nachdem Sie eine FSRM-Dateigruppe definiert haben, können Sie mit dem Cmdlet „New-FsrmFileScreen“ eine FSRM-Dateiprüfung erstellen.

```PowerShell
New-FsrmFileScreen -Path "E:\AFSdataset" -Description "Filter unsupported characters" -IncludeGroup "Unsupported characters"
```

> [!Important]  
> Beachten Sie, dass Dateiprüfungen nur dazu verwendet werden sollten, die Erstellung von Zeichen zu unterbinden, die von Azure File Sync nicht unterstützt werden. Wenn Dateiprüfungen in anderen Szenarios verwendet werden, wird bei der Synchronisierung fortlaufend versucht, die Dateien von der Azure-Dateifreigabe auf dem Server herunterzuladen. Die Synchronisierung wird aufgrund der Dateiprüfung dann gesperrt, wodurch ein hohes Volumen an ausgehenden Daten entsteht. 

## <a name="cloud-tiering"></a>Cloudtiering 
Es gibt beim Cloudtiering zwei Fehlerpfade:

- Für Dateien können Tieringfehler auftreten. Dies bedeutet, dass Azure File Sync erfolglos versucht, für eine Datei in Azure Files das Tiering durchzuführen.
- Für Dateien können Fehler beim Rückruf auftreten. Dies bedeutet, dass der Azure File Sync-Dateisystemfilter (StorageSync.sys) keine Daten herunterlädt, wenn ein Benutzer auf eine Datei zugreifen möchte, für die das Tiering durchgeführt wurde.

Es gibt zwei Hauptklassen von Fehlern, die für jeden Fehlerpfad auftreten können:

- Cloudspeicherfehler
    - *Vorübergehende Probleme mit der Speicherdienstverfügbarkeit*: Weitere Informationen finden Sie unter [SLA für Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).
    - *Nicht zugängliche Azure-Dateifreigabe*: Dieser Fehler tritt normalerweise auf, wenn Sie die Azure-Dateifreigabe löschen und es sich dabei noch um einen Cloudendpunkt in einer Synchronisierungsgruppe handelt.
    - *Nicht zugängliches Speicherkonto*: Dieser Fehler tritt normalerweise auf, wenn Sie das Speicherkonto löschen, während es noch über eine Azure-Dateifreigabe verfügt, bei der es sich um einen Cloudendpunkt in einer Synchronisierungsgruppe handelt. 
- Serverfehler 
    - *Azure File Sync-Dateisystemfilter (StorageSync.sys) wird nicht geladen*: Der Azure File Sync-Dateisystemfilter muss geladen werden, um auf das Tiering bzw. Rückrufanforderungen reagieren zu können. Es kann mehrere Gründe haben, warum der Filter nicht geladen wird. Der häufigste Grund ist, dass das Laden von einem Administrator manuell rückgängig gemacht wurde. Der Azure File Sync-Dateisystemfilter muss immer geladen sein, damit Azure File Sync richtig funktioniert.
    - *Fehlender, beschädigter oder anderweitig fehlerhafter Analysepunkt*: Ein Analysepunkt ist eine spezielle Datenstruktur in einer Datei, die aus zwei Teilen besteht:
        1. Einer Analysenkennung, die für das Betriebssystem angibt, dass der Azure File Sync-Dateisystemfilter (StorageSync.sys) für die Datei unter Umständen Aktionen in Bezug auf E/A-Abläufe durchführen muss. 
        2. Analysedaten, mit denen für den Dateisystemfilter der URI der Datei auf dem zugeordneten Cloudendpunkt (Azure-Dateifreigabe) angegeben wird. 
        
        Am häufigsten kommt es zu einer Beschädigung eines Analysepunkts, wenn ein Administrator versucht, entweder die Kennung oder die dazugehörigen Daten zu ändern. 
    - *Probleme mit der Netzwerkverbindung*: Der Server muss über eine Internetverbindung verfügen, damit für eine Datei das Tiering oder der Rückruf durchgeführt werden kann.

In den folgenden Abschnitten wird beschrieben, wie Sie Probleme mit dem Cloudtiering behandeln und ermitteln, ob ein Problem ein Cloudspeicherproblem oder ein Serverproblem ist.

<a id="monitor-tiering-activity"></a>**Gewusst wie: Überwachen der Tieringaktivität auf einem Server**  
Um die Tieringaktivität auf einem Server zu überwachen, verwenden Sie die Ereignis-ID 9002, 9003, 9016 und 9029 im Telemetrieereignisprotokoll (in der Ereignisanzeige unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“).

- Die Ereignis-ID 9002 bietet Ghostingstatistiken für einen Serverendpunkt. Beispiele: TotalGhostedFileCount, SpaceReclaimedMB usw.

- Die Ereignis-ID 9003 ermöglicht die Fehlerverteilung für einen Serverendpunkt. Beispiele: Gesamtfehlerzahl, ErrorCode usw. Beachten Sie, dass ein Ereignis pro Fehlercode protokolliert wird.

- Die Ereignis-ID 9016 stellt Ghostingergebnisse für ein Volume bereit. Beispiele: Freier Speicherplatz in Prozent, Anzahl der Dateien in der Sitzung, für die ein Ghosting durchgeführt wurde, Anzahl von Dateien, bei denen beim Ghosting ein Fehler aufgetreten ist usw.

- Die Ereignis-ID 9029 bietet Informationen über die Ghostingsitzung. Beispiele: Anzahl der in der Sitzung herangezogenen Dateien, Anzahl der Dateien in der Sitzung, für die ein Tiering durchgeführt wurde, Anzahl der Dateien, für die bereits ein Tiering durchgeführt ist usw.

<a id="monitor-recall-activity"></a>**Gewusst wie: Überwachen der Rückrufaktivität auf einem Server**  
Um die Rückrufaktivität auf einem Server zu überwachen, verwenden Sie die Ereignis-ID 9005, 9006 und 9007 im Telemetrieereignisprotokoll (in der Ereignisanzeige unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“). Beachten Sie, dass diese Ereignisse stündlich protokolliert werden.

- Die Ereignis-ID 9005 bietet Zuverlässigkeit beim Rückruf für einen Serverendpunkt. Beispiele: Gesamtanzahl eindeutiger Dateien, auf die zugegriffen wird, und Gesamtanzahl eindeutiger Dateien, bei denen beim Zugriff ein Fehler aufgetreten ist.

- Die Ereignis-ID 9006 ermöglicht die Rückruffehlerverteilung für einen Serverendpunkt. Beispiele: Fehlerhafte Anforderungen insgesamt, ErrorCode usw. Beachten Sie, dass ein Ereignis pro Fehlercode protokolliert wird.

- Die Ereignis-ID 9007 bietet Rückrufleistung für einen Serverendpunkt. Beispiele: TotalRecallIOSize, TotalRecallTimeTaken usw.

<a id="files-fail-tiering"></a>**Beheben von Problemen bei Dateien, bei denen kein Tiering möglich ist**  
Wenn Tieringfehler von Dateien auf Azure Files auftreten:

1. Überprüfen Sie in der Ereignisanzeige die Telemetrie-, Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“. 
    1. Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.

    > [!NOTE]
    > Eine Datei muss mit einer Azure-Dateifreigabe synchronisiert werden, bevor ein Tiering möglich ist.

    2. Überprüfen Sie, ob der Server über Internetkonnektivität verfügt. 
    3. Überprüfen Sie, ob die Filtertreiber von Azure File Sync („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden:
        - Geben Sie an einer Eingabeaufforderung mit erhöhten Rechten `fltmc` ein. Überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet sind.

> [!NOTE]
> Eine Ereignis-ID 9003 wird einmal pro Stunde im Telemetrieereignisprotokoll protokolliert, wenn beim Tiering einer Datei ein Fehler auftritt (pro Fehlercode wird ein Ereignis protokolliert). Die Betriebs- und Diagnoseereignisprotokolle sollten verwendet werden, wenn zusätzliche Informationen zum Diagnostizieren eines Problems benötigt werden.

<a id="files-fail-recall"></a>**Beheben von Rückruffehlern bei Dateien**  
Wenn bei Dateien Rückruffehler auftreten:
1. Überprüfen Sie in der Ereignisanzeige die Telemetrie-, Betriebs- und Diagnoseereignisprotokolle unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“.
    1. Stellen Sie sicher, dass die Dateien in der Azure-Dateifreigabe vorhanden sind.
    2. Überprüfen Sie, ob der Server über Internetkonnektivität verfügt. 
    3. Öffnen Sie das Dienst-MMC-Snap-In, und stellen Sie sicher, dass der Agent für die Speichersynchronisierung (FileSyncSvc) ausgeführt wird.
    4. Überprüfen Sie, ob die Filtertreiber von Azure File Sync („StorageSync.sys“ und „StorageSyncGuard.sys“) ausgeführt werden:
        - Geben Sie an einer Eingabeaufforderung mit erhöhten Rechten `fltmc` ein. Überprüfen Sie, ob die Dateisystem-Filtertreiber „StorageSync.sys“ und „StorageSyncGuard.sys“ aufgelistet sind.

> [!NOTE]
> Eine Ereignis-ID 9006 wird einmal pro Stunde im Telemetrieereignisprotokoll protokolliert, wenn beim Rückruf einer Datei ein Fehler auftritt (pro Fehlercode wird ein Ereignis protokolliert). Die Betriebs- und Diagnoseereignisprotokolle sollten verwendet werden, wenn zusätzliche Informationen zum Diagnostizieren eines Problems benötigt werden.

<a id="files-unexpectedly-recalled"></a>**Fehlerbehebung bei Dateien, die unerwartet auf einem Server zurückgerufen werden**  
Virenschutz, Sicherung und andere Anwendungen, die viele Dateien lesen, können zu unbeabsichtigten Rückrufen führen, wenn sie das Offline-überspringen-Attribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Das Überspringen von Offlinedateien für Produkte, die diese Option unterstützen, hilft, unbeabsichtigte Rückrufe während Vorgängen wie Virusscans oder Sicherungsaufträgen zu verhindern.

Wenden Sie sich an den Softwareanbieter, um weitere Informationen zu den erforderlichen Konfigurationsschritten zu erhalten, damit die Lösung das Lesen von Offlinedateien überspringt.

Unbeabsichtigte Rückrufe können auch in anderen Szenarien auftreten, z.B. beim Durchsuchen von Dateien im Datei-Explorer. Das Öffnen eines Ordners, der Dateien mit Cloudtiering enthält, im Datei-Explorer auf dem Server kann zu unbeabsichtigten Rückrufen führen. Die Wahrscheinlichkeit dafür steigt, wenn auf dem Server eine Virenschutzlösung aktiviert ist.

## <a name="general-troubleshooting"></a>Allgemeine Problembehandlung
Wenn Probleme mit Azure File Sync auf einem Server auftreten, führen Sie zunächst die folgenden Schritte aus:
1. Überprüfen Sie in der Ereignisanzeige die Telemetrie-, Betriebs- und Diagnoseereignisprotokolle.
    - Probleme mit der Synchronisierung, dem Tiering und dem Rückrufen werden in den Telemetrie-, Diagnose- und Betriebsereignisprotokollen unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“ protokolliert.
    - Probleme im Zusammenhang mit der Serververwaltung (z.B. Konfigurationseinstellungen) werden in den Betriebs- und Diagnoseereignisprotokollen unter „Anwendungen“ und „Dienste\Microsoft\FileSync\Management“ protokolliert.
2. Stellen Sie sicher, dass der Azure File Sync-Dienst auf dem Server ausgeführt wird:
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

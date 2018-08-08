---
title: Beheben von Problemen mit Azure Files unter Linux | Microsoft-Dokumentation
description: Behandeln von Azure Files-Problemen unter Linux
services: storage
documentationcenter: ''
author: jeffpatt24
manager: aungoo
editor: tamram
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffpatt
ms.openlocfilehash: 5781a3c2e121b81275683d73eb3047ba949857c7
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415713"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Behandeln von Azure Files-Problemen unter Linux

Dieser Artikel beschreibt allgemeine Probleme im Zusammenhang mit Microsoft Azure Files, wenn Sie eine Verbindung von Linux-Clients herstellen. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme bereitgestellt.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>„[Zugriff verweigert] Datenträgerkontingent überschritten“, wenn Sie versuchen, eine Datei zu öffnen

Unter Linux erhalten Sie eine Fehlermeldung, die wie folgt aussieht:

**<filename>[Zugriff verweigert] Datenträgerkontingent überschritten**

### <a name="cause"></a>Ursache

Sie haben die obere Grenze der gleichzeitig geöffneten Handles erreicht, die für eine Datei zulässig sind.

### <a name="solution"></a>Lösung

Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles, indem Sie einige Handles schließen und es anschließend erneut versuchen. Weitere Informationen finden Sie unter [Checkliste zur Leistung und Skalierbarkeit von Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Langsames Kopieren von Dateien in und aus Azure Files unter Linux

- Wenn Sie keine bestimmte Anforderung für die Mindest-E/A-Größe haben, empfehlen wir Ihnen für eine optimale Leistung die Verwendung von 1 MiB als E/A-Größe.
- Wenn Sie die endgültige Größe einer Datei kennen, die Sie durch die Verwendung von Schreibvorgängen erweitern, und Ihre Software keine Kompatibilitätsprobleme aufweist, wenn das ungeschriebene Fragment in der Datei Nullen enthält, legen Sie die Dateigröße im Voraus fest, anstatt aus jedem Schreibvorgang einen Erweiterungsschreibvorgang zu machen.
- Verwenden Sie die richtige Kopiermethode:
    - Verwenden Sie [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) für Übertragungen zwischen zwei Dateifreigaben.
    - Verwenden Sie [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) zwischen Dateifreigaben auf einem lokalen Computer.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>„Bereitstellungsfehler (112): Host ist ausgefallen“, aufgrund eines Timeouts bei der Herstellung einer erneuten Verbindung

Ein „112“-Bereitstellungsfehler tritt auf dem Linux-Client auf, wenn sich der Client für eine längere Zeit im Leerlauf befand. Nach längerer Zeit im Leerlauf trennt der Client die Verbindung, und die Verbindung wird abgebrochen.  

### <a name="cause"></a>Ursache

Die Verbindung kann aus den folgenden Gründen im Leerlauf sein:

-   Ein Grund dafür sind Netzwerkkommunikationsfehler, die das erneute Wiederherstellen einer TCP-Verbindung mit dem Server verhindern, wenn die Standardoption zur zeitweiligen Einbindung verwendet wird
-   Aktuelle Verbindungswiederherstellungs-Updates, die in älteren Kernel nicht vorhanden sind

### <a name="solution"></a>Lösung

Dieses Verbindungswiederherstellungs-Problem im Linux-Kernel wurde jetzt im Rahmen der folgenden Änderungen behoben:

- [Fix reconnect to not defer smb3 session reconnect long after socket reconnect (Stellen Sie die Verbindung wieder her, um die Verbindungswiederherstellung der smb3-Sitzung lange nach der Socket-Verbindungswiederherstellung rückzustellen)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Call echo service immediately after socket reconnect (Sofortiges Aufrufen des Echo-Diensts nach der Socket-Verbindungswiederherstellung)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Fix a possible memory corruption during reconnect (CIFS: Beheben einer Arbeitsspeicherbeschädigung während der Verbindungswiederherstellung)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Fix a possible double locking of mutex during reconnect - for kernel v4.9 and later (CIFS: Beheben einer möglichen doppelten Sperre des Mutex während der Verbindungswiederherstellung – für Kernel v4.9 und spätere Versionen)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Allerdings können diese Änderungen noch nicht zu allen Linux-Distributionen portiert werden. Diese Behebung und andere Verbindungswiederherstellungs-Behebungen erfolgen in folgenden gängigen Linux-Kernel: 4.4.40, 4.8.16 und 4.9.1. Sie können diese Behebung abrufen, indem Sie ein Upgrade auf eine dieser empfohlenen Kernelversionen vornehmen.

### <a name="workaround"></a>Problemumgehung

Sie können dieses Problem umgehen, indem Sie eine ständige Bereitstellung festlegen. Dies erzwingt, dass der Client wartet bis eine Verbindung hergestellt oder explizit unterbrochen wurde und verwendet werden kann, um Fehler aufgrund von Netzwerktimeouts zu verhindern. Diese Problemumgehung kann jedoch endlose Wartevorgänge verursachen. Seien Sie darauf vorbereitet, Verbindungen nach Bedarf zu beenden.

Wenn Sie kein Upgrade auf die neuesten Kernelversionen vornehmen können, können Sie dieses Problem umgehen, indem Sie eine Datei in der Azure-Dateifreigabe speichern, in die Sie alle 30 Sekunden oder früher schreiben. Dabei muss es sich um einen Schreibvorgang handeln, wie z.B. die Umschreibung des Erstellungs- oder Änderungsdatums in der Datei. Andernfalls erhalten Sie möglicherweise zwischengespeicherte Ergebnisse, und Ihr Vorgang kann die Verbindungswiederherstellung möglicherweise nicht auslösen.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>„Bereitstellungsfehler (115): Vorgang wird ausgeführt“, beim Bereitstellen von Azure Files mit SMB 3.0

### <a name="cause"></a>Ursache

Bei einigen Linux-Distributionen, die noch nicht die Verschlüsselungsfunktionen in SMB 3.0 unterstützen, können Benutzer die Fehlermeldung „115“ aufgrund einer fehlenden Funktion erhalten, wenn sie versuchen, Azure Files mithilfe von SMB 3.0 einzubinden. SMB 3.0 mit vollständiger Verschlüsselung wird derzeit nur bei Ubuntu 16.04 oder höher unterstützt.

### <a name="solution"></a>Lösung

Die Verschlüsselungsfunktionen für SMB 3.0 für Linux wurden im Kernel 4.11 eingeführt. Dieses Feature ermöglicht das Einbinden einer Azure-Dateifreigabe aus einer lokalen oder anderen Azure-Region. Zum Zeitpunkt der Veröffentlichung wurden diese Funktionen zu Ubuntu 17.04 und Ubuntu 16.10 zurückportiert. Wenn Ihr Linux-SMB-Client die Verschlüsselung nicht unterstützt, binden Sie Azure Files mithilfe von SMB 2.1 von einer Azure Linux-VM aus ein, die sich im gleichen Rechenzentrum wie das File Storage-Konto befindet.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Langsame Leistung in einer Azure-Dateifreigabe, die in einer Linux-VM bereit gestellt ist

### <a name="cause"></a>Ursache

Eine mögliche Ursache der langsamen Leistung ist der deaktivierte Cache.

### <a name="solution"></a>Lösung

Um zu überprüfen, ob der Cache deaktiviert ist, suchen Sie nach dem **cache=**-Eintrag. 

**Cache=none** gibt an, dass das Caching deaktiviert ist.  Stellen Sie die Freigabe erneut bereit, entweder durch die Verwendung des Standardbereitstellungsbefehls oder explizit durch das Hinzufügen der Option **cache=strict** zum Bereitstellungsbefehl, um sicherzustellen, dass das Standardcaching oder der Cachingmodus „strict“ aktiviert ist.

In einigen Szenarios kann die **serverino**-Bereitstellungsoption dazu führen, dass der **ls**-Befehl unverzüglich gegen jeden Verzeichniseintrag läuft. Dieses Verhalten führt zu einer Leistungsminderung, wenn Sie ein großes Verzeichnis auflisten. Sie können die Bereitstellungsoption in Ihrem **/etc/fstab**-Eintrag finden:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Sie können auch überprüfen, ob die richtigen Optionen verwendet werden, indem Sie den **sudo mount | grep cifs**-Befehl ausführen und dessen Output überprüfen, wie z.B. der folgende Beispieloutput:

`//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Wenn die Optionen **cache=strict** oder **serverino** nicht vorhanden sind, heben Sie die Bereitstellung von Azure Files auf, und stellen Sie sie wieder her, indem Sie den „mount“-Befehl aus der [Dokumentation](../storage-how-to-use-files-linux.md) ausführen. Überprüfen Sie dann erneut, ob der **/etc/fstab**-Eintrag die richtigen Optionen hat.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Zeitstempel gingen beim Kopieren von Dateien von Windows auf Linux verloren

Auf Linux/Unix-Plattformen schlägt der **cp -p**-Befehl fehl, wenn Datei 1 und Datei 2 verschiedenen Benutzern gehören.

### <a name="cause"></a>Ursache

Das Force-Flag **f** in COPYFILE führt zu der Ausführung von **cp -p -f** unter Unix. Dieser Befehl schlägt auch fehl, um den Zeitstempel der Datei beizubehalten, die Ihnen nicht gehört.

### <a name="workaround"></a>Problemumgehung

Verwenden Sie das Speicherbenutzerkonto, um die Dateien zu kopieren:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="cannot-connect-or-mount-an-azure-file-share"></a>Azure-Dateifreigabe kann nicht verbunden oder eingebunden werden

### <a name="cause"></a>Ursache

Häufige Ursachen für dieses Problem sind:


- Sie verwenden einen nicht kompatiblen Linux-Distributionsclient. Es wird empfohlen, die folgenden Linux-Distributionen zum Herstellen einer Verbindung mit Azure-Dateifreigaben zu verwenden:

* **Empfohlene Mindestversionen mit entsprechenden Einbindungsmöglichkeiten (SMB 2.1-Version im Vergleich zur SMB 3.0-Version)**    
    
    |   | SMB 2.1 <br>(Einbindungen auf VMs innerhalb derselben Azure-Region) | SMB 3.0 <br>(Einbindungen aus einer lokalen Region und regionsübergreifend) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04 und höher |
    | RHEL | 7 und höher | 7.5 und höher |
    | CentOS | 7 und höher |  7.5 und höher |
    | Debian | 8 und höher |   |
    | openSUSE | 13.2 und höher | 42.3 und höher |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 und höher |

- CIFS-Utils sind auf dem Client nicht installiert.
- Die mindestens erforderliche SMB-/CIFS-Version 2.1 ist nicht auf dem Client installiert.
- SMB 3.0-Verschlüsselung wird auf dem Client nicht unterstützt. SMB 3.0-Verschlüsselung ist in Ubuntu ab Version 16.4 und SUSE ab Version 12.3 verfügbar. Andere Distributionen erfordern Kernel 4.11 oder eine höhere Version.
- Sie versuchen, eine Verbindung mit einem Speicherkonto über TCP-Port 445 herstellen, der nicht unterstützt wird.
- Sie versuchen, auf einem virtuellen Computer eine Verbindung mit der Azure-Dateifreigabe herzustellen, und der virtuelle Computer befindet sich nicht in derselben Region wie das Speicherkonto.

### <a name="solution"></a>Lösung

Um das Problem zu beheben, verwenden Sie das [Problembehandlungstool für Azure Files-Bereitstellungsfehlern unter Linux](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089). Mit diesem Tool können Sie die Clientausführungsumgebung überprüfen und die nicht kompatible Clientkonfiguration erkennen, die zu einem Zugriffsfehler für Azure Files führen würde. Zudem stellt das Tool Anleitungen zur Selbsthilfe bereit und erfasst die Diagnoseablaufverfolgungen.

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: cannot access '&lt;path&gt;': Input/output error

Wenn Sie versuchen, mit dem Befehl „Is“ Dateien einer Azure-Dateifreigabe aufzulisten, hängt der Befehl beim Auflisten von Dateien, und Sie erhalten den folgenden Fehler:

**ls: cannot access'&lt;path&gt;': Input/output error**


### <a name="solution"></a>Lösung
Aktualisieren Sie den Linux-Kernel auf die folgenden Versionen, in denen dieses Problem behoben wurde:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- Alle Versionen ab 4.13

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Symbolische Verknüpfungen kann nicht erstellt werden – ln: Fehler beim Erstellen der symbolischen Verknüpfung „t“: Dieser Vorgang wird nicht unterstützt.

### <a name="cause"></a>Ursache
Standardmäßig ermöglicht das Bereitstellen von Azure-Dateifreigaben unter Linux mit CIFS keine Unterstützung für Symlinks. Es wird ein Fehler wie dieser angezeigt:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Lösung
Der Linux-CIFS-Client unterstützt nicht die Erstellung von symbolischen Verknüpfungen im Windows-Stil über das SMB2/3-Protokoll. Derzeit unterstützt der Linux-Client einen anderen Stil von symbolischen Verknüpfungen namens [Mishall+Französische Symlinks] (https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) sowohl für die Erstellung als auch Vorgänge zur Verfolgung). Kunden, die symbolische Verknüpfungen benötigen, können die Bereitstellungsoption „mfsymlinks“ verwenden. „mfsymlinks“ werden in der Regel empfohlen, da dies auch das von Mac-Computern genutzte Format ist.

Um Symlinks verwenden zu können, fügen Sie Folgendes am Ende des CIFS-Bereitstellungsbefehls hinzu:

```
,mfsymlinks
```

Der Befehl sieht dann in etwa so aus:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsynlinks
```

Nach dem Hinzufügen können Sie Symlinks erstellen, wie in dem [Wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers) vorgeschlagen.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

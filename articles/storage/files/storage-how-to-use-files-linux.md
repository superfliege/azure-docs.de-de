---
title: Verwenden von Azure Files mit Linux | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Linux eine Azure-Dateifreigabe über SMB einbinden.
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tamram
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: renash
ms.openlocfilehash: d4f77460ea6b0a31ed40286f33aa4296bafc9087
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007291"
---
# <a name="use-azure-files-with-linux"></a>Verwenden von Azure Files mit Linux
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können mithilfe des [SMB-Kernelclients](https://wiki.samba.org/index.php/LinuxCIFS) in Linux-Distributionen eingebunden werden. Dieser Artikel veranschaulicht zwei Möglichkeiten zum Einbinden einer Azure-Dateifreigabe: bedarfsgesteuert mit dem Befehl `mount` oder beim Start durch Erstellen eines Eintrags in `/etc/fstab`.

> [!NOTE]  
> Wenn Sie eine Azure-Dateifreigabe außerhalb der Azure-Region einbinden möchten, in der sie gehostet wird (beispielsweise lokal oder in einer anderen Azure-Region), muss das Betriebssystem die Verschlüsselungsfunktionen von SMB 3.0 unterstützen.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Voraussetzungen für das Einbinden einer Azure-Dateifreigabe in Linux und das Paket „cifs-utils“
<a id="smb-client-reqs"></a>
* **Wählen Sie eine Linux-Distribution gemäß Ihren Anforderungen an die Einbindung aus.**  
      Azure Files kann entweder über SMB 2.1 oder über SMB 3.0 eingebunden werden. Bei Verbindungen von lokalen Clients oder von Clients in anderen Azure-Regionen wird SMB 2.1 (oder SMB 3.0 ohne Verschlüsselung) von Azure Files abgelehnt. Ist für ein Speicherkonto die Option *Sichere Übertragung erforderlich* aktiviert, lässt Azure Files nur Verbindungen über SMB 3.0 mit Verschlüsselung zu.
    
    Die Unterstützung der SMB 3.0-Verschlüsselung wurde in der Linux-Kernelversion 4.11 eingeführt und nachträglich für ältere Kernelversionen gängiger Linux-Distributionen portiert. Zum Zeitpunkt der Veröffentlichung dieses Dokuments unterstützen die folgenden Distributionen aus dem Azure-Katalog die Einbindungsoption, die in den Tabellenüberschriften angegeben ist. 

* **Empfohlene Mindestversionen mit entsprechenden Einbindungsmöglichkeiten (SMB 2.1-Version im Vergleich zur SMB 3.0-Version)**    
    
    |   | SMB 2.1 <br>(Einbindungen auf VMs innerhalb derselben Azure-Region) | SMB 3.0 <br>(Einbindungen aus einer lokalen Region und regionsübergreifend) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04 und höher |
    | RHEL | 7 und höher | 7.5 und höher |
    | CentOS | 7 und höher |  7.5 und höher |
    | Debian | 8 und höher |   |
    | openSUSE | 13.2 und höher | 42.3 und höher |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 und höher |
    
    Sollte Ihre Linux-Distribution hier nicht aufgeführt sein, können Sie die Linux-Kernelversion mit dem folgenden Befehl ermitteln:    

   ```bash
   uname -r
   ```    

* <a id="install-cifs-utils"></a>**Das Paket „cifs-utils“ ist installiert.**  
    Das Paket „cifs-utils“ kann mithilfe des Paket-Managers für die Linux-Distribution Ihrer Wahl installiert werden. 

    Verwenden Sie bei auf **Ubuntu** und **Debian** basierenden Distributionen den Paket-Manager `apt-get`:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Verwenden Sie unter **RHEL** und **CentOS**den Paket-Manager `yum`:

    ```bash
    sudo yum install cifs-utils
    ```

    Verwenden Sie unter **OpenSUSE** den Paket-Manager `zypper`:

    ```bash
    sudo zypper install cifs-utils
    ```

    Verwenden Sie bei anderen Distributionen den entsprechenden Paket-Manager, oder [kompilieren Sie den Quellcode](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).
    
* **Festlegen der Verzeichnis-/Dateiberechtigungen der eingebundenen Freigabe:** In den folgenden Beispielen wird die Berechtigung `0777` verwendet, um allen Benutzern die Berechtigungen „Lesen“, „Schreiben“ und „Ausführen“ zu erteilen. Sie können dies nach Wunsch durch andere [chmod-Berechtigungen](https://en.wikipedia.org/wiki/Chmod) ersetzen. 

* **Name des Speicherkontos:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den Namen des Speicherkontos.

* **Speicherkontoschlüssel:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den primären (oder sekundären) Speicherschlüssel. SAS-Schlüssel können derzeit nicht zum Einbinden verwendet werden.

* **Prüfen Sie, ob Port 445 geöffnet ist**: SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Bedarfsgesteuertes Einbinden der Azure-Dateifreigabe mit `mount`
1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)**.

2. **Erstellen Sie einen Ordner für den Bereitstellungspunkt:** Ein Ordner für einen Bereitstellungspunkt kann zwar an einem beliebigen Ort im Dateisystem erstellt werden, üblicherweise wird er jedoch unter dem Ordner `/mnt` erstellt. Beispiel: 

    ```bash
    mkdir /mnt/MyAzureFileShare
    ```

3. **Binden Sie die Azure-Dateifreigabe mithilfe des Befehls „mount“ ein:** Ersetzen Sie dabei `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>` und `<mount-point>` durch die entsprechenden Angaben für Ihre Umgebung. Falls Ihre Linux-Distribution SMB 3.0 mit Verschlüsselung unterstützt (siehe [Grundlegendes zu SMB-Clientanforderungen](#smb-client-reqs)), verwenden Sie `3.0` für `<smb-version>`. Bei Linux-Distributionen ohne Unterstützung von SMB 3.0 mit Verschlüsselung muss `2.1` für `<smb-version>` verwendet werden. Beachten Sie, dass eine Azure-Dateifreigabe nur außerhalb einer Azure-Region mit SMB 3.0 eingebunden werden kann (gilt auch in der lokalen Umgebung sowie in einer anderen Azure-Region). 

    ```bash
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, können Sie `sudo umount <mount-point>` verwenden, um die Einbindung der Freigabe aufzuheben.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Erstellen eines permanenten Bereitstellungspunkts für die Azure-Dateifreigabe mit `/etc/fstab`
1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)**.

2. **Erstellen Sie einen Ordner für den Bereitstellungspunkt:** Ein Ordner für einen Bereitstellungspunkt kann zwar an einem beliebigen Ort im Dateisystem erstellt werden, üblicherweise wird er jedoch unter dem Ordner `/mnt` erstellt. Beachten Sie unabhängig vom Ort der Erstellung den absoluten Pfad des Ordners. Der folgende Befehl erstellt beispielsweise einen neuen Ordner unter `/mnt`. (Der Pfad ist ein absoluter Pfad.)

    ```bash
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Erstellen Sie eine Datei mit Anmeldeinformationen, um den Benutzernamen (den Namen des Speicherkontos) und das Kennwort (den Schlüssel des Speicherkontos) für die Dateifreigabe zu speichern.** Ersetzen Sie `<storage-account-name>` und `<storage-account-key>` durch die entsprechenden Angaben für Ihre Umgebung. 

    ```bash
    if [ -d "/etc/smbcredentials" ]; then
        sudo mkdir /etc/smbcredentials
    fi

    if [ ! -f "/etc/smbcredentials/<storage-account-name>.cred" ]; then
        sudo bash -c 'echo "username=<storage-account-name>" >> /etc/smbcredentials/<storage-account-name>.cred'
        sudo bash -c 'echo "password=<storage-account-key>" >> /etc/smbcredentials/<storage-account-name>.cred'
    fi
    ```

4. **Ändern Sie die Berechtigungen in der Datei mit den Anmeldeinformationen so, dass nur „root“ die Kennwortdatei lesen oder ändern kann.** Da der Speicherkontoschlüssel eigentlich ein Superadministratorkennwort für das Speicherkonto ist, ist es wichtig, die Berechtigungen für die Datei so festzulegen, dass nur „root“ darauf zugreifen kann, damit Benutzer mit geringeren Rechten den Speicherkontoschlüssel nicht abrufen können.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage-account-name>.cred
    ```

5. **Verwenden Sie den folgenden Befehl, um die folgende Zeile an `/etc/fstab`** anzufügen: Ersetzen Sie dabei `<storage-account-name>`, `<share-name>`, `<smb-version>` und `<mount-point>` durch die entsprechenden Angaben für Ihre Umgebung. Falls Ihre Linux-Distribution SMB 3.0 mit Verschlüsselung unterstützt (siehe [Grundlegendes zu SMB-Clientanforderungen](#smb-client-reqs)), verwenden Sie `3.0` für `<smb-version>`. Bei Linux-Distributionen ohne Unterstützung von SMB 3.0 mit Verschlüsselung muss `2.1` für `<smb-version>` verwendet werden. Beachten Sie, dass eine Azure-Dateifreigabe nur außerhalb einer Azure-Region mit SMB 3.0 eingebunden werden kann (gilt auch in der lokalen Umgebung sowie in einer anderen Azure-Region). 

    ```bash
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,credentials=/etc/smbcredentials/<storage-account-name>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Mithilfe von `sudo mount -a` können Sie die Azure-Dateifreigabe nach der Bearbeitung von `/etc/fstab` einbinden, anstatt einen Neustart durchzuführen.

## <a name="feedback"></a>Feedback
Linux-Benutzer, wir möchten von Ihnen hören!

Die Benutzergruppe „Azure Files for Linux“ bietet ein Forum, in dem Sie Ihr Feedback zu File Storage für Linux geben können. Senden Sie eine E-Mail an [Azure Files Linux Users](mailto:azurefileslinuxusers@microsoft.com), um der Benutzergruppe beizutreten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Files erhalten Sie über diese Links.
* [Einführung in Azure Files](storage-files-introduction.md)
* [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
* [HÄUFIG GESTELLTE FRAGEN](../storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-linux-file-connection-problems.md)

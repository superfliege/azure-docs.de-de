---
title: Einbinden von Azure Blob Storage als Dateisystem unter Linux | Microsoft-Dokumentation
description: Einbinden eines Azure Blob Storage-Containers mit FUSE unter Linux
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: seguler
ms.openlocfilehash: 50378fd7739567b0cc56066168ddd33c3ea14141
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957053"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>Einbinden von Blob Storage als Dateisystem mit blobfuse

## <a name="overview"></a>Übersicht
[blobfuse](https://github.com/Azure/azure-storage-fuse) ist ein virtueller Dateisystemtreiber für Azure Blob Storage, der Ihnen den Zugriff auf vorhandene Blockblobdaten in Ihrem Storage-Konto über das Linux-Dateisystem ermöglicht. Azure Blob Storage ist ein Objektspeicherdienst und verfügt daher nicht über einen hierarchischen Namespace. Blobfuse stellt diesen Namespace mithilfe des Schemas für virtuelle Verzeichnisse bereit. Dabei wird als Trennzeichen ein Schrägstrich (/) verwendet.  

In diesem Handbuch wird gezeigt, wie Sie blobfuse verwenden, einen Blobspeichercontainer unter Linux einbinden und auf Daten zugreifen. Weitere Informationen zu blobfuse finden Sie im [blobfuse-Repository](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> blobfuse garantiert keine 100-prozentige Konformität mit POSIX, da Anforderungen einfach in [Blob-REST-APIs](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) übersetzt werden. Umbenennungsvorgänge sind in POSIX beispielsweise atomisch, in blobfuse jedoch nicht.
> Eine vollständige Liste der Unterschiede zwischen einem nativen Dateisystem und blobfuse finden Sie im [blobfuse-Quellcoderepository](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Installieren von blobfuse unter Linux
blobfuse-Binärdateien befinden sich für Ubuntu- und RHEL-Distributionen in den [Microsoft-Softwarerepositorys für Linux](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Um blobfuse in diesen Distributionen zu installieren, konfigurieren Sie eins der Repositorys aus der Liste. Sie können die Binärdateien auch gemäß [dieser](https://github.com/Azure/azure-storage-fuse/wiki/1.-Installation#option-2---build-from-source) Installationsschritte aus dem Quellcode erstellen, wenn für Ihre Distribution keine Binärdateien verfügbar sind.

### <a name="configure-the-microsoft-package-repository"></a>Konfigurieren des Microsoft-Paketrepositorys
Konfigurieren Sie das [Linux-Paketrepository für Microsoft-Produkte](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Beispiel einer Konfiguration für eine Enterprise Linux 6-Distribution:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Analog dazu ändern Sie die URL in `.../rhel/7/...`, um auf eine Enterprise Linux 7-Distribution zu verweisen.

Ein weiteres Beispiel für eine Konfiguration für Ubuntu 14.04:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Analog dazu ändern Sie die URL in `.../ubuntu/16.04/...`, um auf eine Ubuntu 16.04-Distribution zu verweisen.

### <a name="install-blobfuse"></a>Installieren von blobfuse

In einer Ubuntu-/Debian-Distribution:
```bash
sudo apt-get install blobfuse
```

In einer Enterprise Linux-Distribution:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Vorbereiten der Einbindung
blobfuse erfordert einen temporären Pfad im Dateisystem zum Puffern und Zwischenspeichern geöffneter Dateien, wodurch nahezu die gleiche Leistung wie bei einer nativen Lösung erreicht wird. Wählen Sie für diesen temporären Pfad den leistungsstärksten Datenträger aus, oder verwenden Sie eine RAM-Disk, um eine optimale Leistung zu erzielen. 

> [!NOTE]
> Blobfuse speichert die Inhalte aller geöffneten Dateien am temporären Pfad. Stellen Sie sicher, dass ausreichend Speicher für alle geöffneten Dateien verfügbar ist. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(Optional) Verwenden einer RAM-Disk für den temporären Pfad
Das folgende Beispiel erstellt eine RAM-Disk mit 16 GB sowie ein Verzeichnis für blobfuse. Wählen Sie basierend auf Ihren Anforderungen die Größe aus. Diese RAM-Disk ermöglicht blobfuse das Öffnen von Dateien mit einer Größe von bis zu 16 GB. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Verwenden eines SSD für den temporären Pfad
In Azure können Sie die auf Ihren virtuellen Computern verfügbaren kurzlebigen Datenträger (SSD) nutzen, um für blobfuse einen Puffer mit geringer Latenz bereitzustellen. In Ubuntu-Distributionen wird dieser kurzlebige Datenträger unter „/mnt“, in Red Hat- und CentOS-Distributionen unter „/mnt/resource/“ bereitgestellt.

Stellen Sie sicher, dass Benutzer auf den temporären Pfad zugreifen können:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Konfigurieren der Anmeldeinformationen für Speicherkonten
blobfuse erfordert, dass Ihre Anmeldeinformationen in einer Textdatei in folgendem Format gespeichert werden: 

```
accountName myaccount
accountKey storageaccesskey
containerName mycontainer
```

Schränken Sie nach der Erstellung dieser Datei unbedingt den Zugriff ein, damit andere Benutzer sie nicht lesen können.
```bash
chmod 700 fuse_connection.cfg
```

> [!NOTE]
> Wenn Sie die Konfigurationsdatei unter Windows erstellt haben, stellen Sie sicher, dass Sie `dos2unix` ausführen, um die Datei zu bereinigen und in das Unix-Format zu konvertieren. 
>

### <a name="create-an-empty-directory-for-mounting"></a>Erstellen eines leeren Verzeichnisses für die Einbindung
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>Einbinden

> [!NOTE]
> Eine vollständige Liste der Einbindungsoptionen finden Sie im [blobfuse-Repository](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Führen Sie zum Einbinden von blobfuse den folgenden Befehl für den Benutzer aus. Dieser Befehl bindet den unter „/path/to/fuse_connection.cfg“ angegebenen Container am Speicherort „/mycontainer“ ein.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Sie sollten jetzt über die regulären Dateisystem-APIs auf Ihre Blockblobs zugreifen können. Beachten Sie, dass zur Sicherung des Zugriffs auf das eingebundene Verzeichnis nur von dem Benutzer zugegriffen werden kann, der es eingebunden hat. Wenn Sie den Zugriff für alle Benutzer zulassen möchten, können Sie für die Einbindung die Option ```-o allow_other``` verwenden. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Nächste Schritte

* [blobfuse-Homepage](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Melden von Problemen mit blobfuse](https://github.com/Azure/azure-storage-fuse/issues) 


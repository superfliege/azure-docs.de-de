---
title: "Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihrem virtuellen Linux-Computer mithilfe von Azure CLI 1.0 und 2.0 einen persistenten Datenträger hinzufügen."
keywords: "virtueller Linux-Computer,Ressourcendatenträger hinzufügen"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 9d16d16f0e57fab9f1827c37f181e579c627b3d9
ms.openlocfilehash: 9db7d300b745001906bdc38769dcbe6e4d7c7b83
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="add-a-disk-to-a-linux-vm"></a>Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer
In diesem Artikel wird gezeigt, wie Sie einen persistenten Datenträger an Ihren virtuellen Computer anfügen, um Ihre Daten beizubehalten, auch wenn der virtuelle Computer aufgrund einer Wartung oder Größenänderung neu bereitgestellt wird. 


## <a name="use-managed-disks"></a>Verwenden von verwalteten Datenträgern
Azure Managed Disks vereinfacht die Datenträgerverwaltung für Azure-VMs, indem die Speicherkonten verwaltet werden, die den VM-Datenträgern zugeordnet sind. Sie müssen nur die Art (Premium oder Standard) und die Größe des von Ihnen benötigten Datenträgers angeben. Azure erstellt und verwaltet den Datenträger dann für Sie. Weitere Informationen finden Sie in der [Managed Disks overview (Übersicht über verwaltete Datenträger)](managed-disks-overview.md).


### <a name="attach-a-new-disk-to-a-vm"></a>Anfügen eines neuen Datenträgers an einen virtuellen Computer
Wenn Sie nur einen neuen Datenträger für Ihren virtuellen Computer benötigen, verwenden Sie den Befehl [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach) mit dem `--new`-Parameter. Wenn Ihr virtueller Computer in einer Verfügbarkeitszone ist, wird der Datenträger automatisch in derselben Zone wie der virtuelle Computer erstellt. Weitere Informationen finden Sie in der [Overview of Availability Zones (Übersicht über Verfügbarkeitszonen)](../../availability-zones/az-overview.md). Das folgende Beispiel erstellt einen Datenträger mit dem Namen *myDataDisk* mit einer Größe von *50* GB:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers 
In vielen Fällen fügen Sie Datenträger an, die bereits erstellt wurden. Um einen vorhandenen Datenträger anzufügen, suchen Sie die Datenträger-ID, und übergeben Sie die ID an den Befehl [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach). Das folgende Beispiel fragt einen Datenträger mit dem Namen *MyDataDisk* in *MyResourceGroup* ab, und fügt ihn dann dem virtuellen Computer mit dem Namen *myVM* hinzu:

```azurecli
# find the disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

Die Ausgabe sieht in etwa wie folgt aus (Sie können die Option `-o table` bei jedem Befehl zum Formatieren der Ausgabe verwenden):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="use-unmanaged-disks"></a>Verwenden von nicht verwalteten Datenträgern
Nicht verwaltete Datenträger bedürfen eines Mehraufwands, um die zugrundeliegenden Speicherkonten zu erstellen und zu verwalten. Nicht verwaltete Datenträger werden im gleichen Speicherkonto wie Ihr Betriebssystemdatenträger erstellt. Verwenden Sie zum Erstellen und Anfügen eines nicht verwalteten Datenträgers den Befehl [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk?view=azure-cli-latest#az_vm_unmanaged_disk_attach). Im folgenden Beispiel wird ein *50* GB großer nicht verwalteter Datenträger dem virtuellen Computer mit dem Namen *myVM* in der Ressourcengruppe mit dem Namen *myResourceGroup* hinzugefügt:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```


## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Herstellen einer Verbindung mit dem virtuellen Linux-Computer zum Bereitstellen des neuen Datenträgers
Sie benötigen SSH für Ihren virtuellen Azure-Computer, um den neuen Datenträger zu partitionieren, zu formatieren und bereitzustellen, damit er von Ihrem virtuellen Linux-Computer verwendet werden kann. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux auf Azure](mac-create-ssh-keys.md). Im folgenden Beispiel wird eine Verbindung mit einem virtuellen Computer mit dem öffentlichen DNS-Eintrag von *mypublicdns.westus.cloudapp.azure.com* mit dem Benutzernamen *azureuser* erstellt: 

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Nachdem Sie die Verbindung zu Ihrem virtuellen Computer hergestellt haben, können Sie einen Datenträger anfügen. Suchen Sie zunächst den Datenträger mithilfe von `dmesg` (die Methode, die Sie zum Ermitteln Ihres neuen Datenträgers verwenden, kann möglicherweise anders sein). Im folgenden Beispiel wird „dmesg“ zum Filtern auf *SCSI*-Datenträgern verwendet:

```bash
dmesg | grep SCSI
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Hier ist *sdc* der Datenträger, den wir möchten. Partitionieren Sie nun den Datenträger mit `fdisk`, legen Sie ihn auf Partition 1 als primären Datenträger fest, und übernehmen Sie die anderen Standardwerte. Das folgende Beispiel startet den `fdisk` -Prozess auf */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Erstellen Sie die Partition durch Eingabe von `p` bei der Eingabeaufforderung wie folgt:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Schreiben Sie jetzt mit dem Befehl `mkfs` ein Dateisystem auf die Partition: Geben Sie Ihren Dateisystemtyp und den Gerätenamen an. Das folgende Beispiel erstellt ein *ext4*-Dateisystem auf der */dev/sdc1*-Partition, die in den vorherigen Schritten erstellt wurde:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Erstellen Sie jetzt mit `mkdir` ein Verzeichnis zum Bereitstellen des neuen Dateisystems. Im folgenden Beispiel wird ein Verzeichnis mit dem Namen */datadrive* erstellt:

```bash
sudo mkdir /datadrive
```

Verwenden Sie `mount`, um anschließend das Dateisystem bereitzustellen. Im folgenden Beispiel wird die */dev/sdc1*-Partition im */datadrive*-Einbindungspunkt eingebunden:

```bash
sudo mount /dev/sdc1 /datadrive
```

Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch wieder eingebunden wird, muss es der Datei */etc/fstab* hinzugefügt werden. Außerdem wird dringend empfohlen, den UUID (Universally Unique IDentifier) in */etc/fstab* zu verwenden, um auf das Laufwerk und nicht auf den Gerätenamen (z.B. */dev/sdc1*) zu verweisen. Wenn das Betriebssystem während des Starts einen Datenträgerfehler erkennt, wird durch den UUID verhindert, dass an einem bestimmten Ort der falsche Datenträger bereitgestellt wird. Die verbleibenden Datenträger werden dann diesen Geräte-IDs zugewiesen. Verwenden Sie das Hilfsprogramm `blkid`, um den UUID des neuen Laufwerks herauszufinden:

```bash
sudo -i blkid
```

Die Ausgabe sieht in etwa wie im folgenden Beispiel aus:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Eine falsche Bearbeitung der Datei **/etc/fstab** könnte zu einem nicht startfähigen System führen. Wenn Sie sich nicht sicher sind, helfen Ihnen die Informationen zur richtigen Bearbeitung dieser Datei in der Dokumentation weiter. Außerdem wird empfohlen, ein Backup der Datei /etc/fstab zu erstellen, bevor Sie sie bearbeiten.

Öffnen Sie anschließend die Datei */etc/fstab* in einem Text-Editor wie folgt:

```bash
sudo vi /etc/fstab
```

In diesem Beispiel verwenden wir den UUID-Wert für das Gerät */dev/sdc1*, das in den vorherigen Schritten erstellt wurde, und den Einbindungspunkt */datadrive*. Fügen Sie am Ende der Datei */etc/fstab* die folgende Zeile hinzu:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Wenn Sie später einen Datenträger entfernen, ohne „fstab“ zu bearbeiten, kann der Start des virtuellen Computers fehlschlagen. Die meisten Verteilungen stellen entweder die „fstab“-Optionen *nofail* und/oder *nobootwait* zur Verfügung. Mit diesen Optionen kann ein System auch dann starten, wenn der Datenträger beim Start nicht bereitgestellt wird. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu Ihrer Distribution.
> 
> Die *nofail*-Option stellt sicher, dass der virtuelle Computer gestartet wird, selbst wenn das Dateisystem beschädigt oder der Datenträger zur Startzeit nicht vorhanden ist. Ohne diese Option können Verhalten auftreten, die unter [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) (Aufgrund von FSTAB-Fehler keine SSH-Verbindung mit Linux-VM möglich) beschrieben sind.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP-Unterstützung für Linux in Azure
Einige Linux-Kernels unterstützen TRIM/UNMAP-Vorgänge, um ungenutzte Blöcke auf dem Datenträger zu verwerfen. Dies ist in erster Linie für Standardspeicher nützlich, um Azure darüber zu informieren, dass gelöschte Seiten nicht mehr gültig sind und verworfen werden können. Dies kann auch Kosten sparen, wenn Sie große Dateien erstellen und sie dann löschen.

Es gibt zwei Methoden, TRIM-Unterstützung auf Ihrem virtuellen Linux-Computer zu aktivieren. Den empfohlenen Ansatz finden Sie wie üblich in Ihrer Distribution:

* Verwenden Sie die Bereitstellungsoption `discard` in */etc/fstab*, z.B.:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* In einigen Fällen kann sich die Option `discard` möglicherweise auf die Leistung auswirken. Alternativ können Sie den Befehl `fstrim` manuell über die Befehlszeile ausführen oder ihn „crontab“ hinzufügen, um eine regelmäßige Ausführung zu erzielen:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte
* Beachten Sie, dass der neue Datenträger bei einem Neustart nicht für den virtuellen Computer zur Verfügung steht, sofern Sie diese Informationen nicht in Ihre [fstab-Datei](http://en.wikipedia.org/wiki/Fstab) geschrieben haben.
* Lesen Sie die Empfehlungen unter [Optimieren virtueller Linux-Computer in Azure](optimization.md) , um sicherzustellen, dass Ihr virtueller Linux-Computer richtig konfiguriert ist.
* Erweitern Sie die Speicherkapazität durch Hinzufügen zusätzlicher Datenträger, und [konfigurieren Sie RAID](configure-raid.md) für zusätzliche Leistung.



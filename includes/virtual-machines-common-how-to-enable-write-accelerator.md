---
title: Includedatei
description: Includedatei
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 04/30/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 4fe1f2ad4bad9d670094bbb4eed188baf28108ea
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="write-accelerator"></a>Schreibbeschleunigung
Bei der Schreibbeschleunigung handelt es sich um eine Datenträgerfunktion, die nur für virtuelle Computer (Virtual Machines, VMs) der M-Serie in Storage Premium mit Azure Managed Disks verfügbar ist. Wie der Name schon besagt, ist der Zweck der Funktion die Verbesserung der E/A-Wartezeit bei Schreibvorgängen für Azure Storage Premium. Die Schreibbeschleunigung ist hervorragend geeignet, wenn Protokolldateiupdates erforderlich sind, die für moderne Datenbanken auf äußerst leistungsfähige Weise auf dem Datenträger beibehalten werden sollen.

Die Schreibbeschleunigung ist für virtuelle Computer der M-Serie in der öffentlichen Cloud allgemein verfügbar.

## <a name="planning-for-using-write-accelerator"></a>Planen der Verwendung der Schreibbeschleunigung
Die Schreibbeschleunigung sollte für die Volumes verwendet werden, die das Transaktionsprotokoll oder die Protokolle der Rollforwardphase eines DBMS enthalten. Es wird davon abgeraten, die Schreibbeschleunigung für die Datenvolumes eines DBMS zu verwenden, weil die Funktion für die Verwendung auf Protokolldatenträgern optimiert wurde.

Die Schreibbeschleunigung funktioniert nur in Verbindung mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/). 


> [!IMPORTANT]
> Wenn Sie die Schreibbeschleunigung für ein vorhandenes Volume aktivieren oder deaktivieren möchten, das aus mehreren Azure Storage Premium-Stripesetdatenträgern mit Windows-Datenträgerverwaltung oder Volume-Manager, Windows-Speicherplätzen, einem Windows-Dateiserver mit horizontaler Skalierung (Scale-out File Server, SOFS), Linux LVM oder MDADM besteht, müssen alle zum Volume gehörigen Datenträger in separaten Schritten für die Schreibbeschleunigung aktiviert oder deaktiviert werden. **Fahren Sie vor dem Aktivieren oder Deaktivieren der Schreibbeschleunigung in einer solchen Konfiguration die Azure-VM herunter**. 


> [!IMPORTANT]
> Um die Schreibbeschleunigung für einen vorhandenen Azure-Datenträger zu aktivieren, der NICHT Teil eines Volumes ist, das aus mehreren Datenträgern mit Windows-Datenträgerverwaltung oder Volume-Manager, Windows-Speicherplätzen, einem Windows-Dateiserver mit horizontaler Skalierung, Linux LVM oder MDADM besteht, muss die Workload, die auf den Azure-Datenträger zugreift, heruntergefahren werden. Datenbankanwendungen, die den Azure-Datenträger verwenden, MÜSSEN beendet werden.

> [!IMPORTANT]
> Wenn Sie die Schreibbeschleunigung für den Betriebssystemdatenträger des virtuellen Computers aktivieren, wird der virtuelle Computer neu gestartet. 

Für SAP-spezifische VM-Konfigurationen sollte das Aktivieren der Schreibbeschleunigung für Betriebssystemdatenträger nicht erforderlich sein.

### <a name="restrictions-when-using-write-accelerator"></a>Einschränkungen bei der Verwendung der Schreibbeschleunigung
Bei der Verwendung der Schreibbeschleunigung für einen Azure-Datenträger/eine Azure-VHD gelten folgende Einschränkungen:

- Die Premium-Datenträgerzwischenspeicherung muss auf „Keine“ oder „Schreibgeschützt“ festgelegt werden. Alle anderen Zwischenspeicherungsmodi werden nicht unterstützt.
- Momentaufnahmen des Datenträgers, für den die Schreibbeschleunigung aktiviert ist, werden noch nicht unterstützt. Aufgrund dieser Einschränkung kann der Azure Backup-Dienst keine anwendungskonsistente Momentaufnahme aller Datenträger der VM erstellen.
- Der beschleunigte Pfad wird nur für kleinere E/A-Größen (<=32KiB) verwendet. In Workloadsituationen, bei denen Daten in einem Massenvorgang geladen oder die Transaktionsprotokollpuffer vor ihrer persistenten Speicherung stärker befüllt werden, wird für die auf Datenträger geschriebenen E/A-Vorgänge wahrscheinlich nicht der beschleunigte Pfad verwendet.

Die Anzahl von Azure Storage Premium-VHDs pro VM, die von der Schreibbeschleunigung unterstützt werden können, ist begrenzt. Die aktuellen Limits lauten wie folgt:

| VM-SKU | Anzahl von Datenträgern mit Schreibbeschleunigung | Datenträger-IOPS der Schreibbeschleunigung pro VM |
| --- | --- | --- |
| M128ms | 16 | 8.000 |
| M128s | 16 | 8.000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Aktivieren der Schreibbeschleunigung auf einem bestimmten Datenträger
In den nächsten Abschnitten wird beschrieben, wie die Schreibbeschleunigung auf Azure Storage Premium-VHDs aktiviert werden kann.


### <a name="prerequisites"></a>Voraussetzungen
Für die Verwendung der Schreibbeschleunigung gelten zurzeit die folgenden Voraussetzungen:

- Die Datenträger, auf die Sie die Azure-Schreibbeschleunigung anwenden, müssen [verwaltete Azure-Datenträger](https://azure.microsoft.com/services/managed-disks/) in Storage Premium sein.
- Sie müssen einen virtuellen Computer der M-Serie verwenden.

### <a name="enabling-through-power-shell"></a>Aktivierung über PowerShell
Das Azure PowerShell-Modul ab Version 5.5.0 enthält die Änderungen der relevanten Cmdlets zum Aktivieren oder Deaktivieren der Schreibbeschleunigung für bestimmte Azure Storage Premium-Datenträger.
Zum Aktivieren oder Bereitstellen von Datenträgern, die von der Schreibbeschleunigung unterstützt werden, wurden die folgenden PowerShell-Befehle geändert und zum Akzeptieren eines Parameters für die Schreibbeschleunigung erweitert.

Den folgenden Cmdlets wurde ein neuer Switch-Parameter „WriteAccelerator“ hinzugefügt: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Wird der Parameter nicht angegeben, wird die Eigenschaft auf „false“ festgelegt, und es werden Datenträger bereitgestellt, die nicht von der Schreibbeschleunigung unterstützt werden.

Dem folgenden Cmdlet wurde ein neuer Switch-Parameter „OsDiskWriteAccelerator“ hinzugefügt: 

- Set-AzureRmVmssStorageProfile

Wird der Parameter nicht angegeben, wird die Eigenschaft auf „false“ festgelegt, und es werden Datenträger bereitgestellt, die die Schreibbeschleunigung nicht nutzen.

Den folgenden Cmdlets wurde ein neuer boolescher (NULL-Werte nicht zulässig) Parameter „OsDiskWriteAccelerator“ hinzugefügt: 

- Update-AzureRmVM
- Update-AzureRmVmss

Setzen Sie den Parameter auf „$true“ oder „$false“, um die Unterstützung der Azure-Schreibbeschleunigung mit den Datenträgern zu steuern.

Im Folgenden sehen Sie einige Beispielbefehle:

```

New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false 

```

Für zwei Hauptszenarien kann wie in den folgenden Abschnitten erläutert ein Skript verwendet werden.

#### <a name="adding-a-new-disk-supported-by-write-accelerator"></a>Hinzufügen eines von der Schreibbeschleunigung unterstützten neuen Datenträgers
Sie können das folgende Skript verwenden, um Ihrer VM einen neuen Datenträger hinzuzufügen. Der mit diesem Skript erstellte Datenträger verwendet die Schreibbeschleunigung.

```

# Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "log001"
#LUN Id
$lunid=8
#size
$size=1023
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzureRmVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```
Sie müssen die Namen der VM, des Datenträgers und der Ressourcengruppe, die Größe des Datenträgers und die LUN-ID des Datenträgers für Ihre spezifische Bereitstellung anpassen.


#### <a name="enabling-azure-write-accelerator-on-an-existing-azure-disk"></a>Aktivieren der Azure-Schreibbeschleunigung auf einem vorhandenen Azure-Datenträger
Wenn Sie die Schreibbeschleunigung auf einem vorhandenen Datenträger aktivieren müssen, können Sie dazu das folgende Skript verwenden:

```

#Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

Sie müssen die Namen der VM, des Datenträgers und der Ressourcengruppe anpassen. Das obige Skript fügt die Schreibbeschleunigung zu einem vorhandenen Datenträger hinzu, wenn der Wert für „$newstatus“ auf „$true“ festgelegt ist. Mit dem Wert „$false“ wird die Schreibbeschleunigung auf einem Datenträger deaktiviert.

> [!Note]
> Durch die Ausführung des obigen Skripts wird der angegebene Datenträger getrennt, die Schreibbeschleunigung für den Datenträger aktiviert und anschließend der Datenträger wieder angefügt.

### <a name="enabling-through-azure-portal"></a>Aktivierung über das Azure-Portal

Sie können die Schreibbeschleunigung über das Portal aktivieren, in dem Sie die Datenträgercache-Einstellungen angeben: 

![Die Schreibbeschleunigung im Azure-Portal](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)


### <a name="enabling-through-rest-apis"></a>Aktivierung über REST-APIs
Zur Bereitstellung über die Azure-REST-API müssen Sie Azure-ARMClient installieren.

#### <a name="install-armclient"></a>Installieren von ARMClient

Um ARMClient auszuführen, müssen Sie ihn über Chocolatey installieren. Zur Installation können Sie „cmd.exe“ oder PowerShell verwenden. Verwenden Sie erhöhte Berechtigungen für diese Befehle („Als Administrator ausführen“).

Führen Sie mit „cmd.exe“ den folgenden Befehl aus:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

In PowerShell müssen Sie den folgenden Befehl verwenden:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Jetzt können Sie ARMClient mit dem unten stehenden Befehl in „cmd.exe“ oder PowerShell installieren.

```
choco install armclient
```

#### <a name="getting-your-current-vm-configuration"></a>Abrufen der aktuellen VM-Konfiguration
Um die Attribute Ihrer Datenträgerkonfiguration zu ändern, müssen Sie zunächst die aktuelle Konfiguration in einer JSON-Datei abrufen. Sie können die aktuelle Konfiguration abrufen, indem Sie den folgenden Befehl ausführen:

```
armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>
```
Ersetzen Sie die Begriffe in „<<   >>“ durch Ihre Daten (einschließlich des Dateinamens, den Sie für die JSON-Datei verwenden möchten).

Die Ausgabe sieht in etwa wie folgt aus:

```
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Als Nächstes aktualisieren Sie die JSON-Datei und aktivieren die Schreibbeschleunigung auf dem Datenträger namens „log1“. Für diesen Schritt können Sie das folgende Attribut nach dem Cacheeintrag des Datenträgers in der JSON-Datei hinzufügen. 

```
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Aktualisieren Sie anschließend die vorhandene Bereitstellung, indem Sie den folgenden Befehl ausführen:

```
armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>

```

Die Ausgabe sollte wie folgt aussehen. In der Ausgabe können Sie sehen, dass die Schreibbeschleunigung für einen Datenträger aktiviert wurde.

```
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Nachdem die Änderung vorgenommen wurde, sollte das Laufwerk von der Schreibbeschleunigung unterstützt werden.

 

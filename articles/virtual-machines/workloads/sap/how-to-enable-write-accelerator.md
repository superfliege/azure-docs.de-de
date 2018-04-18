---
title: Azure-Schreibbeschleunigung für SAP-Bereitstellungen | Microsoft-Dokumentation
description: Betriebshandbuch für SAP HANA-Systeme, die auf virtuellen Azure-Computern bereitgestellt werden.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0cb9b4003faa2ccdd07ccc78c2095472690f0e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="azure-write-accelerator-for-sap-deployments"></a>Azure-Schreibbeschleunigung für SAP-Bereitstellungen
Die Azure-Schreibbeschleunigung ist eine Funktion, die nur für VMs der M-Serie eingeführt wird. Die Azure-Schreibbeschleunigung ist für alle anderen VM-Serien in Azure mit Ausnahme der M-Serie nicht verfügbar. Der Name macht bereits deutlich, dass der Zweck der Funktion die Verbesserung der E/A-Wartezeit bei Schreibvorgängen für Azure Storage Premium ist. 

>[!NOTE]
> Die Azure-Schreibbeschleunigung ist zurzeit als öffentliche Vorschauversion verfügbar und erfordert die Eintragung Ihrer Azure-Abonnement-ID in der Zulassungsliste.

Die Azure-Schreibbeschleunigungsfunktion ist in folgenden Regionen als öffentliche Vorschauversion für die Bereitstellung der M-Serie verfügbar:

- USA, Westen 2
- Europa, Westen
- Asien, Südosten

## <a name="planning-for-using-azure-write-accelerator"></a>Planen der Verwendung der Azure-Schreibbeschleunigung
Die Azure-Schreibbeschleunigung sollte für die Volumes verwendet werden, die das Transaktionsprotokoll oder die Protokolle der Rollforwardphase eines DBMS enthalten. Die Verwendung der Azure-Schreibbeschleunigung für die Datenvolumes eines DBMS wird nicht empfohlen. Der Grund hierfür ist, dass die Azure Storage Premium-VHDs für die Azure-Schreibbeschleunigung ohne die für Storage Premium verfügbare zusätzliche Zwischenspeicherung für Lesevorgänge eingebunden werden müssen. Mit dieser Art der Zwischenspeicherung lassen sich bei herkömmlichen Datenbanken größere Vorteile erzielen. Da die Schreibbeschleunigung nur die Schreibaktivitäten betrifft und Lesevorgänge nicht beschleunigt, ist das unterstütze Konzept für SAP die Verwendung der Schreibbeschleunigung für die Laufwerke mit dem Transaktionsprotokoll oder dem Protokoll der Rollforwardphase von unterstützten SAP-Datenbanken. 

Die Azure-Schreibbeschleunigung funktioniert nur in Verbindung mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/). Sie müssen daher entsprechend planen. 

>[!NOTE]
> Da die Azure-Schreibbeschleunigungsfunktion noch als öffentliche Vorschauversion verfügbar ist, werden gegenwärtig keine Produktionsbereitstellungen der Funktion unterstützt.

Die Anzahl von Azure Storage Premium-VHDs pro VM, die von der Azure-Schreibbeschleunigung unterstützt werden können, ist begrenzt. Die aktuellen Limits lauten wie folgt:

- 16 VHDs für eine M128xx-VM
- 8 VHDs für eine M64xx-VM

> [!IMPORTANT]
> Wenn Sie die Azure-Schreibbeschleunigung für ein vorhandenes Volume aktivieren oder deaktivieren möchten, das aus mehreren Azure Storage Premium-Stripesetdatenträgern mit Windows-Datenträgerverwaltung oder -Volume-Manager, Windows-Speicherplätzen, einem Windows-Dateiserver mit horizontaler Skalierung (Scale-out File Server, SOFS), Linux LVM oder MDADM besteht, müssen alle zum Volume gehörigen Datenträger in separaten Schritten für die Schreibbeschleunigung aktiviert oder deaktiviert werden. **Fahren Sie vor dem Aktivieren oder Deaktivieren der Schreibbeschleunigung in einer solchen Konfiguration die Azure-VM herunter**. 


> [!IMPORTANT]
> Um die Azure-Schreibbeschleunigung für einen vorhandenen Azure-Datenträger zu aktivieren, der NICHT Teil eines Volumes ist, das aus mehreren Datenträgern mit Windows-Datenträgerverwaltung oder Volume-Manager, Windows-Speicherplätzen, einem Windows-Dateiserver mit horizontaler Skalierung, Linux LVM oder MDADM besteht, muss die Workload, die auf den Azure-Datenträger zugreift, heruntergefahren werden. Datenbankanwendungen, die den Azure-Datenträger verwenden, MÜSSEN beendet werden.

> [!IMPORTANT]
> Wenn Sie die Schreibbeschleunigung für den Betriebssystemdatenträger des virtuellen Azure-Computers aktivieren, wird der virtuelle Computer neu gestartet. 

Für SAP-spezifische VM-Konfigurationen sollte das Aktivieren der Azure-Schreibbeschleunigung für Betriebssystem-Datenträger nicht erforderlich sein.

### <a name="restrictions-when-using-azure-write-accelerator"></a>Einschränkungen bei der Verwendung der Azure-Schreibbeschleunigung
Bei der Verwendung der Azure-Schreibbeschleunigung für einen Azure-Datenträger/eine Azure-VHD gelten folgende Einschränkungen:

- Die Premium-Datenträgerzwischenspeicherung muss auf „Keine“ oder „Schreibgeschützt“ festgelegt werden. Alle anderen Zwischenspeicherungsmodi werden nicht unterstützt.
- Momentaufnahmen des Datenträgers, für den die Schreibbeschleunigung aktiviert ist, werden noch nicht unterstützt. Aufgrund dieser Einschränkung kann der Azure Backup-Dienst keine anwendungskonsistente Momentaufnahme aller Datenträger der VM erstellen.
- Der beschleunigte Pfad wird nur für kleinere E/A-Größen verwendet. In Workloadsituationen, bei denen Daten in einem Massenvorgang geladen oder die Transaktionsprotokollpuffer vor ihrer persistenten Speicherung stärker befüllt werden, wird für die auf Datenträger geschriebenen E/A-Vorgänge wahrscheinlich nicht der beschleunigte Pfad verwendet.


## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Aktivieren der Schreibbeschleunigung auf einem bestimmten Datenträger
In den nächsten Abschnitten wird beschrieben, wie die Azure-Schreibbeschleunigung auf Azure Storage Premium-VHDs aktiviert werden kann.

Gegenwärtig kann die Schreibbeschleunigung nur über die Azure-REST-API und PowerShell aktiviert werden. Im Laufe der nächsten Wochen werden weitere unterstützte Methoden im Azure-Portal verfügbar sein.

### <a name="prerequisites"></a>Voraussetzungen
Die folgenden Voraussetzungen gelten zurzeit für die Verwendung der Azure-Schreibbeschleunigung:

- Die Abonnement-ID, die zum Bereitstellen der VM und des Speichers für die VM verwendet wurde, muss in der Zulassungsliste eingetragen sein. Wenden Sie sich an Ihren Microsoft-CSA, -GBB oder -Kundenbetreuer, um Ihre Abonnement-ID in der Zulassungsliste eintragen zu lassen. 
- Die Datenträger, auf die Sie die Azure-Schreibbeschleunigung anwenden, müssen [verwaltete Azure-Datenträger](https://azure.microsoft.com/services/managed-disks/) sein.

### <a name="enabling-through-power-shell"></a>Aktivierung über PowerShell
Das Azure PowerShell-Modul ab Version 5.5.0 enthält die Änderungen der relevanten Cmdlets zum Aktivieren oder Deaktivieren der Azure-Schreibbeschleunigung für bestimmte Azure Storage Premium-Datenträger.
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

#### <a name="adding--new-disk-supported-by-azure-write-accelerator"></a>Hinzufügen eines neuen Datenträgers, der von der Azure-Schreibbeschleunigung unterstützt wird
Sie können das folgende Skript verwenden, um Ihrer VM einen neuen Datenträger hinzuzufügen. Der mit diesem Skript erstellte Datenträger verwendet die Azure-Schreibbeschleunigung.

```

# Specify your VM Name
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
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
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
#data disk name
$datadiskname = "testsap-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

Sie müssen die Namen der VM, des Datenträgers und der Ressourcengruppe anpassen. Das obige Skript fügt die Schreibbeschleunigung einem vorhandenen Datenträger hinzu, wenn „$newstatus“ auf „$true“ festgelegt ist. Mit dem Wert „$false“ wird die Schreibbeschleunigung auf einem Datenträger deaktiviert.

> [!Note]
> Durch die Ausführung des obigen Skripts wird der angegebene Datenträger getrennt, die Schreibbeschleunigung für den Datenträger aktiviert und anschließend der Datenträger wieder angefügt.




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

 
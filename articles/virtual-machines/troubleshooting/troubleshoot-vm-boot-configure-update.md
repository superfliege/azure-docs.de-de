---
title: VM-Start bleibt bei „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen. in Azure | Microsoft-Dokumentation
description: 'Hier werden die Schritte zur Behebung des Problems erläutert, bei dem der virtuelle Computer beim Starten hängen bleibt und die folgende Meldung angezeigt wird: „Windows wird vorbereitet. Computer nicht ausschalten.“'
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: 2bcdb2b458327a5e87dc36e4a5f50f0ac46bf96a
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621026"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>VM-Start bleibt bei „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen. in Azure

In diesem Artikel wird beschrieben, wie Sie das Problem beheben, wenn Ihr virtueller Computer (Virtual Machine, VM) in der Startphase hängen bleibt und die Meldung „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen. angezeigt wird.

## <a name="symptoms"></a>Symptome

Beim Verwenden der **Startdiagnose** zum Anzeigen des Screenshots eines virtuellen Computers wird das Betriebssystem nicht vollständig gestartet. Darüber hinaus wird auf dem virtuellen Computer die Meldung **„Windows wird vorbereitet. Computer nicht ausschalten.“** Meldung.

![Meldungsbeispiel](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Meldungsbeispiel](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Ursache

Dieses Problem tritt in der Regel auf, wenn der Server nach einer Änderung der Konfiguration den letzten Neustart ausführt. Die Konfigurationsänderung könnte durch Windows-Updates oder durch Änderungen an den Rollen/Features des Servers initiiert werden. Wenn im Falle von Windows Update die Updates groß waren, benötigt das Betriebssystem mehr Zeit, um die Änderungen neu zu konfigurieren.

## <a name="back-up-the-os-disk"></a>Sichern des Betriebssystemdatenträgers

Sichern Sie den Betriebssystemdatenträger, bevor Sie versuchen, das Problem zu beheben:

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Bei VMs mit einem verschlüsselten Datenträger müssen Sie die Datenträger zuerst entsperren (entschlüsseln)

Überprüfen Sie, ob es sich bei dem virtuellen Computer um einen verschlüsselten VM handelt. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie im Portal Ihren virtuellen Computer, und navigieren Sie dann zu den Datenträgern.

2. Es wird eine Spalte mit dem Namen „Verschlüsselung“ angezeigt, der Sie entnehmen können, ob die Verschlüsselung aktiviert ist.

Wenn der Betriebssystemdatenträger verschlüsselt ist, entsperren (entschlüsseln) Sie den verschlüsselten Datenträger. Gehen Sie dazu folgendermaßen vor:

1. Erstellen Sie einen Wiederherstellungs-VM, der sich in der gleichen Ressourcengruppe, im gleichen Speicherkonto und am gleichen Speicherort befindet wie der betroffene virtuelle Computer.

2. Löschen Sie im Azure-Portal den betroffenen virtuellen Computer, aber behalten Sie den Datenträger bei.

3. Führen Sie PowerShell als Administrator aus.

4. Führen Sie das folgende Cmdlet aus, um den Geheimnisnamen abzurufen.

    ```Powershell
    Login-AzureRmAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Führen Sie nach Erhalt des Geheimnisnamens die folgenden Befehle in PowerShell aus:

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Konvertieren Sie den Base64-codierten Wert in Bytes, und schreiben Sie die Ausgabe in eine Datei. 

    > [!Note]
    > Wenn Sie die USB-Entsperrungsoption verwenden, muss der Name der BEK-Datei dem ursprünglichen BEK-GUID entsprechen. Darüber hinaus müssen Sie auf dem Laufwerk „C:“ einen Ordner mit dem Namen „BEK“ erstellen, bevor Sie die folgenden Schritte ausführen können.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Kopieren Sie die BEK-Datei nach der Erstellung auf Ihrem PC auf den Wiederherstellungs-VM, dem Sie den gesperrten Betriebssystemdatenträger angefügt haben. Führen Sie die folgenden Befehle unter Verwendung des Speicherorts der BEK-Datei aus:

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Optional**: In einigen Szenarien müssen Sie ggf. auch den Datenträger mit dem folgenden Befehl entschlüsseln.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Hierbei wird davon ausgegangen, dass sich der verschlüsselte Datenträger auf Laufwerk „F:“ befindet.

8. Wenn Sie Protokolle sammeln müssen, können Sie zum Pfad **LAUFWERKBUCHSTABE:\Windows\System32\winevt\Logs** navigieren.

9. Trennen Sie das Laufwerk vom Wiederherstellungscomputer.

### <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme

Wenn Sie eine Momentaufnahme erstellen möchten, führen Sie die unter [Erstellen einer Momentaufnahme eines Datenträgers](..\windows\snapshot-copy-managed-disk.md) beschriebenen Schritte aus.

## <a name="collect-an-os-memory-dump"></a>Erfassen eines Betriebssystemspeicherabbilds

Führen Sie die im Abschnitt [Sammeln der Speichersicherungsdatei](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) aufgeführten Schritte aus, um ein Betriebssystemabbild zu erfassen, wenn sich der virtuelle Computer bei der Konfiguration aufgehängt hat.

## <a name="contact-microsoft-support"></a>Microsoft-Support kontaktieren

Nachdem Sie die Abbilddatei erfasst haben, können Sie sich an den [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) wenden, um die Grundursache analysieren zu lassen.


## <a name="rebuild-the-vm-using-powershell"></a>Neuerstellen des virtuellen Computers mithilfe von PowerShell

Führen Sie nach dem Erfassen der Speicherabbilddatei die folgenden Schritte aus, um den virtuellen Computer neu zu erstellen.

**Nicht verwaltete Datenträger**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID “SubscriptionID” | Select-AzureRmSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzureRmVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzureRmNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzureRmVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzureRmVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Verwaltete Datenträger**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID "SubscriptionID" | Select-AzureRmSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure Portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzureRmAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzureRmVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzureRmVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```

---
title: Aktivieren von Azure Disk Encryption für virtuelle Linux-IaaS-Computer
description: Dieser Artikel enthält eine Anleitung zur Aktivierung von Microsoft Azure Disk Encryption für virtuelle Linux-IaaS-Computer.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/17/2018
ms.custom: seodec18
ms.openlocfilehash: 749c139e35118ac8b83281bd255b152f61accc0d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542559"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Aktivieren von Azure Disk Encryption für virtuelle Linux-IaaS-Computer 

Es gibt viele Szenarien für die Aktivierung der Datenträgerverschlüsselung. Die Schritte können je nach Szenario variieren. In den folgenden Abschnitten werden diese Szenarien für virtuelle Linux-IaaS-Computer ausführlicher beschrieben. Bevor Sie die Datenträgerverschlüsselung verwenden können, müssen die [Azure Disk Encryption-Voraussetzungen](azure-security-disk-encryption-prerequisites.md) erfüllt werden, und es ist ratsam, den Abschnitt [Additional prerequisites for Linux IaaS VMs](azure-security-disk-encryption-prerequisites.md#bkmk_LinuxPrereq) (Zusätzliche Voraussetzungen für virtuelle Linux-IaaS-Computer) durchzulesen.

Erstellen Sie eine [Momentaufnahme](../virtual-machines/windows/snapshot-copy-managed-disk.md), und sichern Sie die Datenträger, bevor diese verschlüsselt werden. Durch Sicherungen wird sichergestellt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Für VMs mit verwalteten Datenträgern ist eine Sicherung erforderlich, bevor die Verschlüsselung durchgeführt wird. Nach der Erstellung einer Sicherung können Sie das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ verwenden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Weitere Informationen zum Sichern und Wiederherstellen von verschlüsselten VMs finden Sie im Artikel [Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
> - Wenn Sie zuvor [Azure Disk Encryption mit der Azure AD-App](azure-security-disk-encryption-prerequisites-aad.md) zum Verschlüsseln dieser VM verwendet haben, müssen Sie diese Verschlüsselungsoption auch weiterhin für Ihre VM verwenden. Sie können [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) auf dieser verschlüsselten VM nicht verwenden, da dies kein unterstütztes Szenario ist. Das bedeutet, das Verlassen der AAD-Anwendung für diese verschlüsselte VM wird noch nicht unterstützt.
 > - Für Azure Disk Encryption müssen sich der Schlüsseltresor und die virtuellen Computer in der gleichen Region befinden. Erstellen und verwenden Sie einen Schlüsseltresor, der sich in derselben Region wie die zu verschlüsselnde VM befindet.
> - Beim Verschlüsseln von Linux-Betriebssystemvolumes kann der Vorgang einige Stunden dauern. Es ist normal, dass die Verschlüsselung von Linux-Betriebssystemvolumes länger als für Datenvolumes dauert. 
> - Das Deaktivieren der Verschlüsselung auf virtuellen Linux-Computern wird nur für Datenvolumes unterstützt. Dies wird nicht auf Daten- oder Betriebssystemvolumes unterstützt, wenn das Betriebssystemvolume verschlüsselt wurde.  


## <a name="bkmk_RunningLinux"> </a> Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Linux-Computer
In diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. Wenn Sie Schemainformationen für die Erweiterung des virtuellen Computers benötigen, finden Sie diese im Artikel [Azure Disk Encryption für Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).

>[!IMPORTANT]
 >Es ist obligatorisch, außerhalb von Azure Disk Encryption und vor der Aktivierung von Azure Disk Encryption eine Momentaufnahme bzw. Sicherung einer VM zu erstellen, die auf einem verwalteten Datenträger basiert. Sie können über das Portal eine Momentaufnahme des verwalteten Datenträgers erstellen oder [Azure Backup](../backup/backup-azure-vms-encryption.md) verwenden. Mit Sicherungen ist dafür gesorgt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Nach der Erstellung einer Sicherung kann das Set-AzureRmVMDiskEncryptionExtension-Cmdlet verwendet werden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Der Befehl „Set-AzureRmVMDiskEncryptionExtension“ führt für VMs, die auf verwalteten Datenträgern basieren, zu einem Fehler, bis eine Sicherung erstellt und dieser Parameter angegeben wurde. 
>
>Das Verschlüsseln bzw. Deaktivieren der Verschlüsselung kann dazu führen, dass die VM neu gestartet wird. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Aktivieren der Verschlüsselung auf einem vorhandenen bzw. ausgeführten virtuellen Linux-Computer mit der Azure CLI 
Sie können die Datenträgerverschlüsselung auf Ihrer verschlüsselten VHD aktivieren, indem Sie das Befehlszeilentool [Azure CLI 2.0](/cli/azure) installieren und verwenden. Azure PowerShell kann mit [Azure Cloud Shell](../cloud-shell/overview.md) im Browser verwendet oder auf dem lokalen Computer installiert und in einer beliebigen PowerShell-Sitzung verwendet werden. Verwenden Sie die folgenden CLI-Befehle, um die Verschlüsselung auf vorhandenen oder ausgeführten virtuellen IaaS-Linux-Computern in Azure zu aktivieren:

Verwenden Sie den Befehl [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), um die Verschlüsselung auf einem ausgeführten virtuellen IaaS-Computer in Azure zu aktivieren.

-  **Verschlüsseln eines ausgeführten virtuellen Computers:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Verschlüsseln eines ausgeführten virtuellen Computers mit KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Überprüfen der Datenträgerverschlüsselung:** Verwenden Sie den Befehl [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show), um den Verschlüsselungsstatus eines virtuellen IaaS-Computers zu überprüfen. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Deaktivieren der Verschlüsselung:** Verwenden Sie den Befehl [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable), um die Verschlüsselung zu deaktivieren. Die Deaktivierung der Verschlüsselung ist nur auf Datenvolumes für virtuelle Linux-Computer zulässig.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen Linux-Computer mit PowerShell
Verwenden Sie das Cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension), um die Verschlüsselung auf einem ausgeführten virtuellen IaaS-Computer in Azure zu aktivieren. 

-  **Verschlüsseln eines ausgeführten virtuellen Computers:** Das folgende Skript initialisiert Ihre Variablen und führt das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ aus. Die Ressourcengruppe, der virtuelle Computer und der Schlüsseltresor wurden bereits bei der Vorbereitung erstellt. Ersetzen Sie „MySecureRg“, „MySecureVM“ und „MySecureVault“ durch Ihre eigenen Werte. Unter Umständen müssen Sie den Parameter „-VolumeType“ hinzufügen, wenn Sie normale Datenträger und nicht den Betriebssystemdatenträger verschlüsseln. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Verschlüsseln eines ausgeführten virtuellen Computers mit KEK:** Unter Umständen müssen Sie den Parameter „-VolumeType“ hinzufügen, wenn Sie normale Datenträger und nicht den Betriebssystemdatenträger verschlüsseln. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

    >[!NOTE]
    > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Überprüfen der Datenträgerverschlüsselung:** Verwenden Sie das Cmdlet [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus), um den Verschlüsselungsstatus eines virtuellen IaaS-Computers zu überprüfen. 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **Deaktivieren der Datenträgerverschlüsselung:** Verwenden Sie das Cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption), um die Verschlüsselung zu deaktivieren. Die Deaktivierung der Verschlüsselung ist nur auf Datenvolumes für virtuelle Linux-Computer zulässig.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Aktivieren der Verschlüsselung auf einem vorhandenen oder ausgeführten virtuellen IaaS-Linux-Computer mit einer Vorlage

Sie können die Datenträgerverschlüsselung auf einer vorhandenen oder ausgeführten virtuellen IaaS-Linux-VM in Azure aktivieren, indem Sie die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad) verwenden.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **Deploy to Azure** (In Azure bereitstellen).

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Ressourcengruppenstandort, die Parameter, die rechtlichen Bedingungen und die Vereinbarung aus. Klicken Sie auf **Erstellen**, um die Verschlüsselung auf dem vorhandenen oder ausgeführten virtuellen IaaS-Computer zu aktivieren.

Die folgende Tabelle enthält Resource Manager-Vorlagenparameter für vorhandene oder ausgeführte virtuelle Computer:

| Parameter | BESCHREIBUNG |
| --- | --- |
| vmName | Der Name des virtuellen Computers, der den Verschlüsselungsvorgang ausführt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Sie können ihn abrufen, indem Sie das Cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` oder den folgenden Azure CLI-Befehl verwenden: az keyvault list --resource-group "MySecureGroup" |ConvertFrom-JSON`|
| keyVaultResourceGroup | Der Name der Ressourcengruppe mit dem Schlüsseltresor|
|  keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dieser Parameter ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option **nokek** auswählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option **kek** auswählen, müssen Sie den Wert _keyEncryptionKeyURL_ eingeben. |
| volumeType | Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige Werte sind _OS_, _Data_ und _All_. 
| forceUpdateTag | Dient zum Übergeben eines eindeutigen Werts (beispielsweise einer GUID), wenn die Ausführung des Vorgangs erzwungen werden muss. |
| resizeOSDisk | Gibt an, ob die Größe der Betriebssystempartition angepasst werden soll, um die gesamte Betriebssystem-VHD einzunehmen, bevor das Systemvolume aufgeteilt wird. |
| location | Der Standort für alle Ressourcen. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Verschlüsseln von VM-Skalierungsgruppen
Mit [Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/overview.md) können Sie eine Gruppe identischer virtueller Computer mit Lastenausgleich erstellen und verwalten. Die Anzahl von VM-Instanzen kann automatisch erhöht oder verringert werden, wenn sich der Bedarf ändert, oder es kann ein Zeitplan festgelegt werden. Verwenden Sie zum Verschlüsseln von VM-Skalierungsgruppen die Befehlszeilenschnittstelle oder Azure PowerShell.

Ein Batchdateibeispiel für die Datenträgerverschlüsselung von Linux-Skalierungsgruppen finden Sie [hier](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss). Dieses Beispiel erstellt eine Ressourcengruppe und eine Linux-Skalierungsgruppe, bindet einen 5-GB-Datenträger für Daten ein und verschlüsselt die VM-Skalierungsgruppe.

### <a name="register-for-disk-encryption-preview-using-azure-cli"></a>Registrieren der Vorschauversion der Datenträgerverschlüsselung mit der Azure-Befehlszeilenschnittstelle

Azure Disk Encryption für die Vorschauversion von VM-Skalierungsgruppen erfordert, dass Sie Ihr Abonnement mithilfe des Befehls [az feature register](/cli/azure/feature#az-feature-register) selbst registrieren. Sie müssen die folgenden Schritte nur beim ersten Mal ausführen, wenn Sie das Vorschaufeature der Datenträgerverschlüsselung verwenden:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Es kann bis zu 10 Minuten dauern, bis die Registrierungsanforderung weitergegeben wurde. Sie können den Registrierungsstatus mithilfe des Befehls [az feature show](/cli/azure/feature#az-feature-show) überprüfen. Wenn *Registered* (Registriert) für `State` angegeben ist, registrieren Sie den Anbieter *Microsoft.Compute* erneut mithilfe von [az provider register](/cli/azure/provider#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Verschlüsseln von VM-Skalierungsgruppen unter Verwendung der Azure-Befehlszeilenschnittstelle
Verwenden Sie [az vmss encryption enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable), um die Verschlüsselung für eine VM-Skalierungsgruppe unter Windows zu aktivieren. Ist die Upgraderichtlinie für die Skalierungsgruppe auf „Manuell“ festgelegt, starten Sie die Verschlüsselung mit [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Die Ressourcengruppe, der virtuelle Computer und der Schlüsseltresor wurden bereits bei der Vorbereitung erstellt. 

-  **Verschlüsseln einer ausgeführten VM-Skalierungsgruppe**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Verschlüsseln einer ausgeführten VM-Skalierungsgruppe unter Verwendung von KEK zum Umschließen des Schlüssel**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
- **Abrufen des Verschlüsselungsstatus für eine VM-Skalierungsgruppe:** Verwenden Sie [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show).

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Deaktivieren der Verschlüsselung für eine VM-Skalierungsgruppe:** Verwenden Sie [az vmss encryption disable](/cli/azure/vmss/encryption#az-vmss-encryption-disable).
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

### <a name="register-for-disk-encryption-preview-using-azure-powershell"></a>Registrieren der Vorschauversion der Datenträgerverschlüsselung mit Azure PowerShell

Azure Disk Encryption für die Vorschauversion von VM-Skalierungsgruppen erfordert, dass Sie Ihr Abonnement mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) selbst registrieren. Sie müssen die folgenden Schritte nur beim ersten Mal ausführen, wenn Sie das Vorschaufeature der Datenträgerverschlüsselung verwenden:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Es kann bis zu 10 Minuten dauern, bis die Registrierungsanforderung weitergegeben wurde. Sie können den Registrierungsstatus mit [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen. Wenn `RegistrationState` *Registered* (Registriert) meldet, registrieren Sie den Anbieter *Microsoft.Compute* erneut mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Verschlüsseln von VM-Skalierungsgruppen unter Verwendung von Azure PowerShell

Verwenden Sie das Cmdlet [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension), um die Verschlüsselung für eine VM-Skalierungsgruppe unter Windows zu aktivieren. Die Ressourcengruppe, der virtuelle Computer und der Schlüsseltresor wurden bereits bei der Vorbereitung erstellt.

-  **Verschlüsseln einer ausgeführten VM-Skalierungsgruppe:**
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:
    ```powershell
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

- **Abrufen des Verschlüsselungsstatus für eine VM-Skalierungsgruppe:** Verwenden Sie das Cmdlet [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption).
    
    ```powershell
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Deaktivieren der Verschlüsselung für eine VM-Skalierungsgruppe:** Verwenden Sie das Cmdlet [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption). 

    ```powershell
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Azure Resource Manager-Vorlagen für Skalierungsgruppen für virtuelle Linux-Computer

Wenn Sie Skalierungsgruppen für virtuelle Linux-Computer verschlüsseln oder entschlüsseln möchten, verwenden Sie die Azure Resource Manager-Vorlagen und die folgenden Anweisungen:

- [Aktivieren der Verschlüsselung für eine Skalierungsgruppe für virtuelle Linux-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)
- [Bereitstellen einer Skalierungsgruppe für virtuelle Linux-Computer mit einer Jumpbox und Aktivieren der Verschlüsselung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)
- [Deaktivieren der Verschlüsselung für eine Skalierungsgruppe für virtuelle Linux-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

     1. Klicken Sie auf Schaltfläche zum **Bereitstellen in Azure**.
     2. Füllen Sie die erforderlichen Felder aus, und stimmen Sie dann den Geschäftsbedingungen zu.
     3. Klicken Sie auf **Kaufen**, um die Vorlage bereitzustellen.

## <a name="bkmk_EFA"> </a>Verwenden des Features EncryptFormatAll für Datenträger auf virtuellen Linux-IaaS-Computern
Mit dem Parameter **EncryptFormatAll** wird die Zeit reduziert, die zum Verschlüsseln von Linux-Datenträgern benötigt wird. Partitionen, die bestimmte Kriterien erfüllen, werden formatiert (mit dem aktuellen Dateisystem). Anschließend werden sie wieder an dem Ort bereitgestellt, an dem sie sich vor der Ausführung des Befehls befunden haben. Wenn Sie einen Datenträger ausschließen möchten, der die Kriterien erfüllt, können Sie die Bereitstellung vor dem Ausführen des Befehls aufheben.

 Nach der Ausführung dieses Befehls werden alle zuvor bereitgestellten Laufwerke formatiert. Anschließend wird die Verschlüsselungsebene für das Laufwerk gestartet, das nun leer ist. Bei Auswahl dieser Option wird auch der kurzlebige Ressourcendatenträger verschlüsselt, der an die VM angefügt ist. Nachdem das kurzlebige Laufwerk zurückgesetzt wurde, wird es erneut formatiert und bei der nächsten Gelegenheit von der Azure Disk Encryption-Lösung für die VM erneut verschlüsselt.

>[!WARNING]
> EncryptFormatAll sollte nicht verwendet werden, wenn sich auf den Datenvolumes der VM erforderliche Daten befinden. Sie können Datenträger von der Verschlüsselung ausschließen, indem Sie deren Bereitstellung aufheben. Sie sollten EncryptFormatAll zuerst auf einer Test-VM ausprobieren und sich mit dem Featureparameter und dessen Auswirkungen vertraut machen, bevor Sie das Feature auf einer VM für die Produktion einsetzen. Mit der EncryptFormatAll-Option wird der Datenträger formatiert, und alle darauf befindlichen Daten gehen verloren. Stellen Sie vor dem Fortfahren sicher, dass die Bereitstellung der auszuschließenden Datenträger ordnungsgemäß aufgehoben wird. </br></br>
 >Wenn Sie diesen Parameter festlegen, während Sie die Verschlüsselungseinstellungen aktualisieren, wird vor der eigentlichen Verschlüsselung ggf. ein Neustart durchgeführt. In diesem Fall sollten Sie auch den Datenträger, der formatiert werden soll, aus der FSTAB-Datei entfernen. Entsprechend sollten Sie die Partition, die formatiert und verschlüsselt werden soll, der FSTAB-Datei hinzufügen, bevor Sie den Verschlüsselungsvorgang initiieren. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll-Kriterien
Der Parameter durchläuft alle Partitionen und verschlüsselt sie, solange **alle** hier aufgeführten Kriterien erfüllt sind: 
- Ist keine Stamm-, Betriebssystem- oder Startpartition
- Ist nicht bereits verschlüsselt
- Ist kein BEK-Volume
- Ist kein RAID-Volume
- Ist kein LVM-Volume
- Ist bereitgestellt

Verschlüsselt die Datenträger, aus denen sich das RAID- oder LVM-Volume zusammensetzt, nicht das RAID- oder LVM-Volume selbst.

### <a name="bkmk_EFAPSH"> </a> Verwenden des Parameters „EncryptFormatAll“ mit der Azure-Befehlszeilenschnittstelle
Verwenden Sie den Befehl [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), um die Verschlüsselung auf einem ausgeführten virtuellen IaaS-Computer in Azure zu aktivieren.

-  **Verschlüsseln eines ausgeführten virtuellen Computers unter Verwendung von „EncryptFormatAll“:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Verwenden des Parameters EncryptFormatAll mit einem PowerShell-Cmdlet
Verwenden Sie das [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension)-Cmdlet mit dem [Parameter EncryptFormatAll](https://www.powershellgallery.com/packages/AzureRM/5.0.0). 

**Verschlüsseln eines ausgeführten virtuellen Computers unter Verwendung von „EncryptFormatAll“:** Als Beispiel werden mit dem unten angegebenen Skript Ihre Variablen initialisiert und das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ mit dem Parameter „EncryptFormatAll“ ausgeführt. Die Ressourcengruppe, der virtuelle Computer und der Schlüsseltresor wurden bereits bei der Vorbereitung erstellt. Ersetzen Sie „MySecureRg“, „MySecureVM“ und „MySecureVault“ durch Ihre eigenen Werte.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
   ```


### <a name="bkmk_EFALVM"> </a> Verwenden des Parameters EncryptFormatAll mit Logical Volume Manager (LVM) 
Wir empfehlen Ihnen, ein LVM-on-crypt-Setup zu verwenden. Ersetzen Sie für alle folgenden Beispiele den Gerätepfad und die Bereitstellungspunkte durch die Werte für Ihren jeweiligen Anwendungsfall. Dieses Setup kann wie folgt durchgeführt werden:

- Fügen Sie die Datenträger hinzu, aus denen sich die VM zusammensetzt.
- Formatieren Sie diese Datenträger, stellen Sie sie bereit, und fügen Sie sie der FSTAB-Datei hinzu.

    1. Formatieren Sie den neu hinzugefügten Datenträger. Hier verwenden wir von Azure generierte symlinks. Durch die Verwendung von symlinks werden Probleme in Bezug auf sich ändernde Gerätenamen vermieden. Weitere Informationen finden Sie im Artikel zur [Behandlung von Problemen mit Gerätenamen](../virtual-machines/linux/troubleshoot-device-names-problems.md).
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Stellen Sie die Datenträger bereit.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Fügen Sie sie der FSTAB-Datei hinzu.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Führen Sie das Cmdlet „Set-AzureRmVMDiskEncryptionExtension-PowerShell“ mit „-EncryptFormatAll“ aus, um diese Datenträger zu verschlüsseln.
         ```azurepowershell-interactive
         Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. Richten Sie LVM für diese neuen Datenträger ein. Beachten Sie, dass die verschlüsselten Laufwerke entsperrt werden, nachdem das Starten der VM abgeschlossen wurde. Die LVM-Bereitstellung muss nachfolgend also auch verzögert werden.


## <a name="bkmk_VHDpre"> </a> Neue virtuelle IaaS-Computer, die aus einer vom Kunden verschlüsselten VHD und mit Verschlüsselungsschlüsseln erstellt wurden
In diesem Szenario können Sie die Verschlüsselung mithilfe von PowerShell-Cmdlets oder CLI-Befehlen aktivieren. 

Verwenden Sie im Anhang die Anleitung zum Vorbereiten von vorverschlüsselten Images, die in Azure verwendet werden können. Nach dem Erstellen des Images können Sie die Schritte im nächsten Abschnitt ausführen, um eine verschlüsselte Azure-VM zu erstellen.

* [Vorbereiten einer vorverschlüsselten Windows-VHD](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vorbereiten einer vorverschlüsselten Linux-VHD](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Es ist obligatorisch, außerhalb von Azure Disk Encryption und vor der Aktivierung von Azure Disk Encryption eine Momentaufnahme bzw. Sicherung einer VM zu erstellen, die auf einem verwalteten Datenträger basiert. Sie können über das Portal eine Momentaufnahme des verwalteten Datenträgers erstellen oder [Azure Backup](../backup/backup-azure-vms-encryption.md) verwenden. Mit Sicherungen ist dafür gesorgt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Nach der Erstellung einer Sicherung kann das Set-AzureRmVMDiskEncryptionExtension-Cmdlet verwendet werden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Der Befehl „Set-AzureRmVMDiskEncryptionExtension“ führt für VMs, die auf verwalteten Datenträgern basieren, zu einem Fehler, bis eine Sicherung erstellt und dieser Parameter angegeben wurde. 
>
>Das Verschlüsseln bzw. Deaktivieren der Verschlüsselung kann dazu führen, dass die VM neu gestartet wird. 



### <a name="bkmk_VHDprePSH"> </a> Verwenden von Azure PowerShell zum Verschlüsseln von virtuellen IaaS-Computern mit vorverschlüsselten VHDs 
Sie können die Datenträgerverschlüsselung auf einer verschlüsselten VHD aktivieren, indem Sie das PowerShell-Cmdlet [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples) verwenden. Im folgenden Beispiel sind einige häufig verwendete Parameter angegeben. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Aktivieren der Verschlüsselung auf einem neu hinzugefügten Datenträger

Sie können einen neuen Datenträger hinzufügen, indem Sie den Befehl [az vm disk attach](../virtual-machines/linux/add-disk.md) oder das [Azure-Portal](../virtual-machines/linux/attach-disk-portal.md) verwenden. Bevor Sie die Verschlüsselung durchführen können, müssen Sie den neu angefügten Datenträger bereitstellen. Sie müssen die Verschlüsselung des Datenträgers anfordern, da das Laufwerk nicht genutzt werden kann, während die Verschlüsselung durchgeführt wird. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Aktivieren der Verschlüsselung für einen neu hinzugefügten Datenträger per Azure CLI

 Wenn der virtuelle Computer zuvor mit „All“ verschlüsselt war, sollte der Parameter „-VolumeType“ auf „All“ belassen werden. Bei „All“ sind sowohl Betriebssystem als auch Datenträger enthalten. Wenn der virtuelle Computer zuvor mit dem Volumetyp „OS“ verschlüsselt war, sollte der Parameter „-VolumeType“ in „All“ geändert werden, damit sowohl das Betriebssystem als auch der neue Datenträger enthalten sind. Wenn der virtuelle Computer nur mit dem Volumetyp „Data“ verschlüsselt war, kann er auf „Data“ belassen werden, wie es nachfolgend gezeigt ist. Das Hinzufügen und Anfügen eines neuen Datenträgers an einen virtuellen Computer ist keine ausreichende Vorbereitung für die Verschlüsselung. Der neu angefügte Datenträger muss außerdem formatiert und vor dem Aktivieren der Verschlüsselung ordnungsgemäß im virtuellen Computer bereitgestellt werden. Unter Linux muss der Datenträger in „/etc/fstab“ mit einem [persistenten Blockgerätenamen](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems) bereitgestellt werden.  

Im Gegensatz zur PowerShell-Syntax erfordert die Befehlszeilenschnittstelle vom Benutzer keine Angabe einer eindeutigen Sequenzversion beim Aktivieren der Verschlüsselung. Die Befehlszeilenschnittstelle wird automatisch generiert und verwendet einen eigenen eindeutigen Sequenzversionswert.

-  **Verschlüsseln von Datenvolumes eines ausgeführten virtuellen Computers:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Verschlüsseln von Datenvolumes eines ausgeführten virtuellen Computers mit KEK:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Aktivieren der Verschlüsselung für einen neu hinzugefügten Datenträger mit Azure PowerShell
 Bei Verwendung von PowerShell zum Verschlüsseln eines neuen Datenträgers für Linux muss eine neue Sequenzversion angegeben werden. Die Sequenzversion muss eindeutig sein. Mit dem unten angegebenen Skript wird eine GUID für die Sequenzversion generiert. 
 

-  **Verschlüsseln von Datenvolumes eines ausgeführten virtuellen Computers:** Das folgende Skript initialisiert Ihre Variablen und führt das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ aus. Die Ressourcengruppe, der virtuelle Computer und der Schlüsseltresor wurden bereits bei der Vorbereitung erstellt. Ersetzen Sie „MySecureRg“, „MySecureVM“ und „MySecureVault“ durch Ihre eigenen Werte. Zulässige Werte für den Parameter „-VolumeType“ sind „All“, „OS“ und „Data“. Wenn der virtuelle Computer zuvor mit dem Volumetyp „OS“ oder „All“ verschlüsselt war, sollte der Parameter „-VolumeType“ in „All“ geändert werden, damit sowohl das Betriebssystem als auch der neue Datenträger enthalten sind.

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **Verschlüsseln von Datenvolumes eines ausgeführten virtuellen Computers mit KEK:** Zulässige Werte für den Parameter „-VolumeType“ sind „All“, „OS“ und „Data“. Wenn der virtuelle Computer zuvor mit dem Volumetyp „OS“ oder „All“ verschlüsselt war, sollte der Parameter „-VolumeType“ in „All“ geändert werden, damit sowohl das Betriebssystem als auch der neue Datenträger enthalten sind.

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data';

     ```

    >[!NOTE]
    > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Deaktivieren der Verschlüsselung für virtuelle Linux-Computer
Sie können die Verschlüsselung mit Azure PowerShell, der Azure CLI oder einer Resource Manager-Vorlage deaktivieren. 

>[!IMPORTANT]
>Das Deaktivieren der Verschlüsselung mit Azure Disk Encryption auf Linux-VMs wird nur für Datenvolumes unterstützt. Dies wird nicht auf Daten- oder Betriebssystemvolumes unterstützt, wenn das Betriebssystemvolume verschlüsselt wurde.  

- **Deaktivieren der Datenträgerverschlüsselung mit Azure PowerShell:** Verwenden Sie das Cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption), um die Verschlüsselung zu deaktivieren. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Deaktivieren der Verschlüsselung mit der Azure CLI:** Verwenden Sie den Befehl [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable), um die Verschlüsselung zu deaktivieren. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Deaktivieren der Verschlüsselung mit einer Resource Manager-Vorlage:** Verwenden Sie zum Deaktivieren der Verschlüsselung die Vorlage zum [Deaktivieren der Verschlüsselung auf einem ausgeführten virtuellen Linux-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad).
     1. Klicken Sie auf Schaltfläche zum **Bereitstellen in Azure**.
     2. Wählen Sie das Abonnement, die Ressourcengruppe, den Standort, die VM, die rechtlichen Bedingungen und die Vereinbarung aus.
     3.  Klicken Sie auf **Kaufen**, um die Datenträgerverschlüsselung auf einer ausgeführten Windows-VM zu deaktivieren. 


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Windows](azure-security-disk-encryption-windows.md)

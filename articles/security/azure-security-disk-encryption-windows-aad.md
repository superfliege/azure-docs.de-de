---
title: Aktivieren von Azure Disk Encryption mit Azure AD-Apps für Windows-IaaS-VMs (früheres Release) | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Anleitung zur Aktivierung von Microsoft Azure Disk Encryption für virtuelle Windows-IaaS-Computer.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 10/04/2018
ms.openlocfilehash: 407ea9adaaae4df15054dc4da3391b870dd8dcc7
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622141"
---
#  <a name="enable-azure-disk-encryption-for-windows-iaas-vms-previous-release"></a>Aktivieren von Azure Disk Encryption für virtuelle Windows-IaaS-Computer (früheres Release)

**Beim neuen Release von Azure Disk Encryption muss kein Azure AD-Anwendungsparameter mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Sie müssen beim neuen Release während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen mit dem neuen Release und ohne die Azure AD-Anwendungsparameter verschlüsselt werden. Eine Anleitung zum Aktivieren der VM-Datenträgerverschlüsselung mit dem neuen Release finden Sie unter [Azure Disk Encryption für virtuelle Windows-Computer](azure-security-disk-encryption-windows.md). Virtuelle Computer, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der AAD-Syntax gepflegt werden.**


Es gibt viele Szenarien für die Aktivierung der Datenträgerverschlüsselung. Die Schritte können je nach Szenario variieren. In den folgenden Abschnitten werden diese Szenarien für virtuelle Windows-IaaS-Computer ausführlicher beschrieben. Bevor Sie die Datenträgerverschlüsselung verwenden können, müssen die [Voraussetzungen für Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites-aad.md) erfüllt sein. 

Erstellen Sie eine [Momentaufnahme](../virtual-machines/windows/snapshot-copy-managed-disk.md), und sichern Sie die Datenträger, bevor diese verschlüsselt werden. Durch Sicherungen wird sichergestellt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Für VMs mit verwalteten Datenträgern ist eine Sicherung erforderlich, bevor die Verschlüsselung durchgeführt wird. Nach der Erstellung einer Sicherung können Sie das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ verwenden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Weitere Informationen zum Sichern und Wiederherstellen von verschlüsselten VMs finden Sie im Artikel [Azure Backup](../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
>Um sicherzustellen, dass die Verschlüsselungsgeheimnisse die Regionsgrenzen nicht verlassen, müssen die Key Vault-Instanz und die VMs für Azure Disk Encryption in derselben Region angeordnet sein. Erstellen und verwenden Sie eine Key Vault-Instanz, die sich in derselben Region wie die zu verschlüsselnde VM befindet. 


## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Aktivieren der Verschlüsselung auf neuen IaaS-VMs, die über den Marketplace erstellt wurden.
Die Datenträgerverschlüsselung kann auf neuen virtuellen IaaS-Windows-VMs aus dem Azure Marketplace aktiviert werden, indem eine Resource Manager-Vorlage genutzt wird. Mit der Vorlage wird ein neuer verschlüsselter virtueller Windows-Computer erstellt, indem das Windows Server 2012-Katalogimage verwendet wird.

1. Klicken Sie in der [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) auf **Deploy to Azure** (In Azure bereitstellen).

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Ressourcengruppenstandort, die Parameter, die rechtlichen Bedingungen und die Vereinbarung aus. Klicken Sie auf **Kaufen**, um einen neuen virtuellen IaaS-Computer bereitzustellen, für den die Verschlüsselung aktiviert ist.

3. Nachdem Sie die Vorlage bereitgestellt haben, können Sie den Verschlüsselungsstatus der VM überprüfen, indem Sie Ihre bevorzugte Methode verwenden:
     - Führen Sie die Überprüfung mit der Azure CLI durch, indem Sie den Befehl [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) verwenden. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Führen Sie die Überprüfung mit Azure PowerShell durch, indem Sie das Cmdlet [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) verwenden. 

         ```azurepowershell-interactive
         Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     -  Wählen Sie die VM aus, und klicken Sie dann unter der Überschrift **Einstellungen** auf **Datenträger**, um den Verschlüsselungsstatus im Portal zu überprüfen. Im Diagramm unter **Verschlüsselung** wird angezeigt, ob die Verschlüsselung aktiviert ist. 
           ![Azure-Portal – Datenträgerverschlüsselung aktiviert](./media/azure-security-disk-encryption/disk-encryption-fig2.png) In der folgenden Tabelle sind die Parameter der Resource Manager-Vorlage für das Szenario mit neuen virtuellen Computern aus dem Marketplace mit Azure AD-Client-ID aufgeführt:

| Parameter | BESCHREIBUNG | 
| --- | --- |
| adminUserName | Administratorbenutzername für den virtuellen Computer. |
| adminPassword | Administratorkennwort für den virtuellen Computer. |
| newStorageAccountName | Name des Speicherkontos zum Speichern von Betriebssystem- und Daten-VHDs. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| virtualNetworkName | Der Name des VNET, dem die VM-NIC angehören soll. |
| subnetName | Name des Subnetzes im VNET, dem die VM-NIC angehören soll. |
| AADClientID | Client-ID der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| AADClientSecret | Clientgeheimnis der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| keyVaultURL | Die URL des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Sie können sie abrufen, indem Sie das Cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName).VaultURI` oder die Azure CLI `az keyvault show --name "MySecureVault" --query properties.vaultUri` verwenden. |
| keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird (optional). </br> </br>„KeyEncryptionKeyURL“ ist ein optionaler Parameter. Sie können Ihren eigenen KEK nutzen, um den Schutz für den Datenverschlüsselungsschlüssel (Passphrasegeheimnis) im Schlüsseltresor zu erhöhen. |
| keyVaultResourceGroup | Die Ressourcengruppe des Schlüsseltresors. |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |


## <a name="bkmk_RunningWinVM"></a> Aktivieren der Verschlüsselung auf vorhandenen oder ausgeführten virtuellen Windows-IaaS-Computern
In diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie eine Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten wird ausführlicher erläutert, wie Sie Azure Disk Encryption aktivieren. 

>[!IMPORTANT]
 >Es ist obligatorisch, außerhalb von Azure Disk Encryption und vor der Aktivierung von Azure Disk Encryption eine Momentaufnahme bzw. Sicherung einer VM zu erstellen, die auf einem verwalteten Datenträger basiert. Sie können über das Portal eine Momentaufnahme des verwalteten Datenträgers erstellen oder [Azure Backup](../backup/backup-azure-vms-encryption.md) verwenden. Mit Sicherungen ist dafür gesorgt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Nach der Erstellung einer Sicherung kann das Set-AzureRmVMDiskEncryptionExtension-Cmdlet verwendet werden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Der Befehl „Set-AzureRmVMDiskEncryptionExtension“ führt für VMs, die auf verwalteten Datenträgern basieren, zu einem Fehler, bis eine Sicherung erstellt und dieser Parameter angegeben wurde. 
>
>Das Verschlüsseln bzw. Deaktivieren der Verschlüsselung kann dazu führen, dass die VM neu gestartet wird. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Aktivieren der Verschlüsselung auf vorhandenen oder ausgeführten VMs mit Azure PowerShell 
Verwenden Sie das Cmdlet [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension), um die Verschlüsselung auf einem ausgeführten virtuellen IaaS-Computer in Azure zu aktivieren. Informationen dazu, wie die Verschlüsselung mit Azure Disk Encryption über PowerShell-Cmdlets aktiviert wird, finden Sie in den Blogbeiträgen [Explore Azure Disk Encryption with Azure PowerShell - Part 1](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1) und [Explore Azure Disk Encryption with Azure PowerShell - Part 2](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2).

-  **Verschlüsseln einer ausgeführten VM mit einem geheimen Clientschlüssel:** Mit dem unten angegebenen Skript werden Ihre Variablen initialisiert, und das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ wird ausgeführt. Die Ressourcengruppe, die VM, der Schlüsseltresor, die AAD-App und der geheime Clientschlüssel sollten bereits beim Erfüllen der Voraussetzungen erstellt worden sein. Ersetzen Sie „MySecureRg“, „MySecureVM“, „MySecureVault“, „My-AAD-client-ID“ und „My-AAD-client-secret“ durch Ihre Werte.
     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Verschlüsseln einer ausgeführten VM per KEK, um den geheimen Clientschlüssel zu umschließen:** Für Azure Disk Encryption können Sie einen vorhandenen Schlüssel in Ihrem Schlüsseltresor angeben, um Geheimnisse für die Datenträgerverschlüsselung zu umschließen, die beim Aktivieren der Verschlüsselung generiert wurden. Wenn ein Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) angegeben wird, wird dieser Schlüssel von Azure Disk Encryption verwendet, um die Verschlüsselungsgeheimnisse vor dem Schreiben in die Key Vault-Instanz zu umschließen. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Überprüfen der Datenträgerverschlüsselung:** Verwenden Sie das Cmdlet [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus), um den Verschlüsselungsstatus eines virtuellen IaaS-Computers zu überprüfen. 
     ```azurepowershell-interactive
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Deaktivieren der Datenträgerverschlüsselung:** Verwenden Sie das Cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption), um die Verschlüsselung zu deaktivieren. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a> Aktivieren der Verschlüsselung auf vorhandenen oder ausgeführten VMs mit der Azure CLI
Verwenden Sie den Befehl [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable), um die Verschlüsselung auf einem ausgeführten virtuellen IaaS-Computer in Azure zu aktivieren.

-  **Verschlüsseln einer ausgeführten VM mit einem geheimen Clientschlüssel:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Verschlüsseln einer ausgeführten VM per KEK zum Umschließen des geheimen Clientschlüssels:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Überprüfen der Datenträgerverschlüsselung:** Verwenden Sie den Befehl [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show), um den Verschlüsselungsstatus eines virtuellen IaaS-Computers zu überprüfen. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Deaktivieren der Verschlüsselung:** Verwenden Sie den Befehl [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable), um die Verschlüsselung zu deaktivieren. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Es ist obligatorisch, außerhalb von Azure Disk Encryption und vor der Aktivierung von Azure Disk Encryption eine Momentaufnahme bzw. Sicherung einer VM zu erstellen, die auf einem verwalteten Datenträger basiert. Sie können über das Portal eine Momentaufnahme des verwalteten Datenträgers erstellen oder [Azure Backup](../backup/backup-azure-vms-encryption.md) verwenden. Mit Sicherungen ist dafür gesorgt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Nach der Erstellung einer Sicherung kann das Set-AzureRmVMDiskEncryptionExtension-Cmdlet verwendet werden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Dieser Befehl führt für VMs, die auf verwalteten Datenträgern basieren, zu einem Fehler, bis eine Sicherung erstellt und dieser Parameter angegeben wurde. 
>
>Das Verschlüsseln bzw. Deaktivieren der Verschlüsselung kann dazu führen, dass die VM neu gestartet wird. 

### <a name="bkmk_RunningWinVMwRM"> </a>Verwenden der Resource Manager-Vorlage
Sie können die Datenträgerverschlüsselung auf vorhandenen oder ausgeführten virtuellen IaaS-Windows-VMs in Azure aktivieren, indem Sie die [Resource Manager-Vorlage zum Verschlüsseln eines ausgeführten virtuellen Windows-Computers](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm) verwenden.


1. Klicken Sie in der Azure-Schnellstartvorlage auf **Deploy to Azure** (In Azure bereitstellen).

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Ressourcengruppenstandort, die Parameter, die rechtlichen Bedingungen und die Vereinbarung aus. Klicken Sie auf **Kaufen**, um die Verschlüsselung auf dem vorhandenen oder ausgeführten virtuellen IaaS-Computer zu aktivieren.

In der folgenden Tabelle werden die Parameter der Resource Manager-Vorlage für vorhandene oder ausgeführte VMs aufgeführt, die eine Azure AD-Client-ID verwenden:

| Parameter | BESCHREIBUNG |
| --- | --- |
| AADClientID | Client-ID der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| AADClientSecret | Clientgeheimnis der Azure AD-Anwendung, die über Berechtigungen zum Schreiben von Geheimnissen in den Schlüsseltresor verfügt. |
| keyVaultName | Der Name des Schlüsseltresors, in den der BitLocker-Schlüssel hochgeladen werden soll. Sie können ihn abrufen, indem Sie das Cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` oder den folgenden Azure CLI-Befehl verwenden: az keyvault list --resource-group "MySecureGroup" |Convertfrom-JSON`|
|  keyEncryptionKeyURL | Die URL des KEK, der zum Verschlüsseln des generierten BitLocker-Schlüssels verwendet wird. Dieser Parameter ist optional, wenn Sie in der Dropdownliste „UseExistingKek“ die Option **nokek** auswählen. Wenn Sie in der Dropdownliste „UseExistingKek“ die Option **kek** auswählen, müssen Sie den Wert _keyEncryptionKeyURL_ eingeben. |
| volumeType | Der Typ des Volumes, auf dem der Verschlüsselungsvorgang durchgeführt wird. Gültige Werte sind _OS_, _Data_ und _All_. |
| sequenceVersion | Sequenzversion des BitLocker-Vorgangs. Diese Versionsnummer sollte jedes Mal erhöht werden, wenn auf demselben virtuellen Computer ein Vorgang für eine Datenträgerverschlüsselung durchgeführt wird. |
| vmName | Der Name des virtuellen Computers, auf dem der Verschlüsselungsvorgang durchgeführt wird. |


## <a name="bkmk_VHDpre"> </a>Neue virtuelle IaaS-Computer, die aus einer vom Kunden verschlüsselten VHD und mit Verschlüsselungsschlüsseln erstellt wurden
In diesem Szenario können Sie die Verschlüsselung aktivieren, indem Sie die Resource Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle verwenden. In den folgenden Abschnitten werden die Resource Manager-Vorlage und die CLI-Befehle ausführlicher erläutert. 

Verwenden Sie im Anhang die Anleitung zum Vorbereiten von vorverschlüsselten Images, die in Azure verwendet werden können. Nach dem Erstellen des Images können Sie die Schritte im nächsten Abschnitt ausführen, um eine verschlüsselte Azure-VM zu erstellen.

* [Vorbereiten einer vorverschlüsselten Windows-VHD](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vorbereiten einer vorverschlüsselten Linux-VHD](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Es ist obligatorisch, außerhalb von Azure Disk Encryption und vor der Aktivierung von Azure Disk Encryption eine Momentaufnahme bzw. Sicherung einer VM zu erstellen, die auf einem verwalteten Datenträger basiert. Sie können über das Portal eine Momentaufnahme des verwalteten Datenträgers erstellen oder [Azure Backup](../backup/backup-azure-vms-encryption.md) verwenden. Mit Sicherungen ist dafür gesorgt, dass eine Wiederherstellungsoption verfügbar ist, falls während der Verschlüsselung ein unerwarteter Fehler auftritt. Nach der Erstellung einer Sicherung kann das Set-AzureRmVMDiskEncryptionExtension-Cmdlet verwendet werden, um verwaltete Datenträger durch das Angeben des Parameters „-skipVmBackup“ zu verschlüsseln. Der Befehl „Set-AzureRmVMDiskEncryptionExtension“ führt für VMs, die auf verwalteten Datenträgern basieren, zu einem Fehler, bis eine Sicherung erstellt und dieser Parameter angegeben wurde. 
>
>Das Verschlüsseln bzw. Deaktivieren der Verschlüsselung kann dazu führen, dass die VM neu gestartet wird. 



### <a name="bkmk_VHDprePSH"> </a> Verschlüsseln von VMs mit vorverschlüsselten VHDs per Azure PowerShell
Sie können die Datenträgerverschlüsselung auf einer verschlüsselten VHD aktivieren, indem Sie das PowerShell-Cmdlet [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples) verwenden. Im folgenden Beispiel sind einige häufig verwendete Parameter angegeben. 

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```


### <a name="bkmk_VHDpreRM"> </a>Verschlüsseln von virtuellen IaaS-Computern mit vorverschlüsselten VHDs per Resource Manager-Vorlage 
Sie können die Datenträgerverschlüsselung auf einer verschlüsselten VHD aktivieren, indem die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm) nutzen.

1. Klicken Sie in der Azure-Schnellstartvorlage auf **Deploy to Azure** (In Azure bereitstellen).

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Ressourcengruppenstandort, die Parameter, die rechtlichen Bedingungen und die Vereinbarung aus. Klicken Sie auf **Erstellen**, um die Verschlüsselung auf dem neuen virtuellen IaaS-Computer zu aktivieren.

In der folgenden Tabelle werden die Parameter der Resource Manager-Vorlage für Ihre verschlüsselte VHD beschrieben:

| Parameter | BESCHREIBUNG |
| --- | --- |
| newStorageAccountName | Name des Speicherkontos zum Speichern des Betriebssystems auf einer verschlüsselten VHD. Dieses Speicherkonto sollte bereits in derselben Ressourcengruppe und an demselben Standort wie der virtuelle Computer erstellt worden sein. |
| osVhdUri | URI der Betriebssystem-VHD aus dem Speicherkonto. |
| osType | Betriebssystem-Produkttyp (Windows/Linux). |
| virtualNetworkName | Der Name des VNET, dem die VM-NIC angehören soll. Der Name sollte bereits in derselben Ressourcengruppe und an demselben Standort wie der virtuelle Computer erstellt worden sein. |
| subnetName | Name des Subnetzes im VNET, dem die VM-NIC angehören soll. |
| vmSize | Die Größe des virtuellen Computers. Derzeit werden nur die standardmäßigen A-, D- und G-Serien unterstützt. |
| keyVaultResourceID | Die ResourceID zum Identifizieren der Key Vault-Ressource im Azure Resource Manager. Sie können sie abrufen, indem Sie das PowerShell-Cmdlet `(Get-AzureRmKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` oder den Azure CLI-Befehl `az keyvault show --name "MySecureVault" --query id` verwenden.|
| keyVaultSecretUrl | Die URL des Datenträgerverschlüsselungsschlüssels, der im Schlüsseltresor eingerichtet ist. |
| keyVaultKekUrl | Die URL des KEK, der zum Verschlüsseln des generierten Datenträgerverschlüsselungsschlüssels verwendet wird. |
| vmName | Name des virtuellen IaaS-Computers. |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Aktivieren der Verschlüsselung auf einem neu hinzugefügten Datenträger
Sie können [einer Windows-VM einen neuen Datenträger mit PowerShell](../virtual-machines/windows/attach-disk-ps.md) oder [über das Azure-Portal](../virtual-machines/windows/attach-managed-disk-portal.md) hinzufügen. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Aktivieren der Verschlüsselung für einen neu hinzugefügten Datenträger mit Azure PowerShell
 Bei Verwendung von PowerShell zum Verschlüsseln eines neuen Datenträgers für Windows-VMs sollte eine neue Sequenzversion angegeben werden. Die Sequenzversion muss eindeutig sein. Mit dem unten angegebenen Skript wird eine GUID für die Sequenzversion generiert. In einigen Fällen kann ein neu hinzugefügter Datenträger ggf. automatisch über die Azure Disk Encryption-Erweiterung verschlüsselt werden. Die automatische Verschlüsselung tritt normalerweise auf, wenn der virtuelle Computer neu gestartet wird, nachdem die neue Festplatte online ist. Dies wird in der Regel dadurch verursacht, dass für den Datenträgertyp „Alle“ angegeben wurde, wenn die Festplattenverschlüsselung zuvor auf dem virtuellen Computer ausgeführt wurde. Wenn die automatische Verschlüsselung auf einem neu hinzugefügten Datenträger auftritt, wird empfohlen, das Cmdlet „Set-AzureRmVmDiskEncryptionExtension“ erneut mit der neuen Sequenzversion auszuführen. Wenn Ihr neuer Datenträger automatisch verschlüsselt ist und Sie diese Verschlüsselung nicht wünschen, entschlüsseln Sie zuerst alle Laufwerke und verschlüsseln Sie dann mit einer neuen Sequenzversion, die das Betriebssystem für den Datenträgertyp angibt. 
 

-  **Verschlüsseln einer ausgeführten VM mit einem geheimen Clientschlüssel:** Mit dem unten angegebenen Skript werden Ihre Variablen initialisiert, und das Cmdlet „Set-AzureRmVMDiskEncryptionExtension“ wird ausgeführt. Die Ressourcengruppe, die VM, der Schlüsseltresor, die AAD-App und der geheime Clientschlüssel sollten bereits beim Erfüllen der Voraussetzungen erstellt worden sein. Ersetzen Sie „MySecureRg“, „MySecureVM“, „MySecureVault“, „My-AAD-client-ID“ und „My-AAD-client-secret“ durch Ihre Werte. In diesem Beispiel wird „All“ für den Parameter „-VolumeType“ verwendet, wodurch Betriebssystem- und Datenvolumes enthalten sind. Wenn Sie nur das Betriebssystemvolume verschlüsseln möchten, verwenden Sie „OS“ für den Parameter „-VolumeType“. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;
    ```
- **Verschlüsseln einer ausgeführten VM per KEK, um den geheimen Clientschlüssel zu umschließen:** Für Azure Disk Encryption können Sie einen vorhandenen Schlüssel in Ihrem Schlüsseltresor angeben, um Geheimnisse für die Datenträgerverschlüsselung zu umschließen, die beim Aktivieren der Verschlüsselung generiert wurden. Wenn ein Schlüsselverschlüsselungsschlüssel angegeben wird, verwendet Azure Disk Encryption diesen, um Verschlüsselungsgeheimnisse vor dem Schreiben in Key Vault zu umschließen. In diesem Beispiel wird „All“ für den Parameter „-VolumeType“ verwendet, wodurch Betriebssystem- und Datenvolumes enthalten sind. Wenn Sie nur das Betriebssystemvolume verschlüsseln möchten, verwenden Sie „OS“ für den Parameter „-VolumeType“. 

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Die Syntax für den Wert des Parameters „disk-encryption-keyvault“ ist die vollständige Bezeichnerzeichenfolge: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Die Syntax für den Wert des Parameters „key-encryption-key“ ist der vollständige URI für den KEK, z.B.: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Aktivieren der Verschlüsselung für einen neu hinzugefügten Datenträger per Azure CLI
  Mit dem Azure CLI-Befehl wird automatisch eine neue Sequenzversion für Sie bereitgestellt, wenn Sie den Befehl zum Aktivieren der Verschlüsselung ausführen. Zulässige Werte für den Parameter „-VolumeType“ sind „All“, „OS“ und „Data“. Möglicherweise müssen Sie den Parameter „-VolumeType“ in „OS“ oder „Data“ ändern, wenn Sie nur einen Datenträgertyp für den virtuellen Computer verschlüsseln. In den Beispielen wird „All“ für den Parameter „-VolumeType“ verwendet. 

-  **Verschlüsseln einer ausgeführten VM mit einem geheimen Clientschlüssel:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Verschlüsseln einer ausgeführten VM per KEK zum Umschließen des geheimen Clientschlüssels:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "all"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Aktivieren Sie die Verschlüsselung, indem Sie die auf dem Azure AD-Clientzertifikat basierende Authentifizierung verwenden.
Sie können die Clientzertifikatauthentifizierung mit oder ohne KEK verwenden. Für die Skripts ist es erforderlich, dass die [Azure Disk Encryption-Voraussetzungen](azure-security-disk-encryption-prerequisites-aad.md) vollständig erfüllt sind. Bevor Sie die PowerShell-Skripts verwenden, sollten Sie das Zertifikat bereits in den Schlüsseltresor hochgeladen und auf der VM bereitgestellt haben. Wenn Sie auch einen KEK nutzen, sollte der KEK bereits vorhanden sein. Weitere Informationen finden Sie im Abschnitt [Zertifikatbasierte Authentifizierung für Azure AD](azure-security-disk-encryption-prerequisites-aad.md#bkmk_Cert) des Artikels mit den Voraussetzungen.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Aktivieren der Verschlüsselung mit der zertifikatbasierten Authentifizierung mit Azure PowerShell

```powershell
## Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = ‘MySecureVM’;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Aktivieren der Verschlüsselung mit der zertifikatbasierten Authentifizierung und einem KEK mit Azure PowerShell

```powershell
# Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Deaktivieren der Verschlüsselung
Sie können die Verschlüsselung mit Azure PowerShell, der Azure CLI oder einer Resource Manager-Vorlage deaktivieren. 

- **Deaktivieren der Datenträgerverschlüsselung mit Azure PowerShell:** Verwenden Sie das Cmdlet [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption), um die Verschlüsselung zu deaktivieren. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

- **Deaktivieren der Verschlüsselung mit der Azure CLI:** Verwenden Sie den Befehl [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable), um die Verschlüsselung zu deaktivieren. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Deaktivieren der Verschlüsselung mit einer Resource Manager-Vorlage:** 

    1. Klicken Sie in der Vorlage unter [Disable disk encryption on running Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) (Deaktivieren der Datenträgerverschlüsselung auf einer ausgeführten Windows-VM) auf **Deploy to Azure** (In Azure bereitstellen).
    2. Wählen Sie das Abonnement, die Ressourcengruppe, den Standort, die VM, die rechtlichen Bedingungen und die Vereinbarung aus.
    3.  Klicken Sie auf **Kaufen**, um die Datenträgerverschlüsselung auf einer ausgeführten Windows-VM zu deaktivieren. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aktivieren von Azure Disk Encryption für Linux](azure-security-disk-encryption-linux-aad.md)

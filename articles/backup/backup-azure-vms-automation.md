---
title: Bereitstellen und Verwalten von Sicherungen für mit Resource Manager bereitgestellte virtuelle Computer mithilfe von PowerShell
description: Verwenden Sie PowerShell zum Bereitstellen und Verwalten von Sicherungen in Azure für mit Resource Manager bereitgestellte virtuelle Computer.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c29a91a40df34ecd9270d5805209d361cf990754
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638034"
---
# <a name="use-powershell-to-back-up-and-restore-virtual-machines"></a>Verwenden von PowerShell zum Sichern und Wiederherstellen von virtuellen Computern

In diesem Artikel erfahren Sie, wie Sie eine Azure-VM mithilfe von Azure PowerShell-Cmdlets in einem Recovery Services-Tresor sichern und daraus wiederherstellen. Ein Recovery Services-Tresor ist eine Ressource von Azure Resource Manager, die für den Schutz von Daten und Objekten in den Diensten Azure Backup und Azure Site Recovery verwendet wird. 

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel ist für die Verwendung mit virtuellen Computern erstellt, die mit dem Resource Manager-Modell bereitgestellt wurden.
>
>

In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer mit PowerShell schützen und Daten von einem Wiederherstellungspunkt aus wiederherstellen.

## <a name="concepts"></a>Konzepte
Wenn Sie mit dem Dienst „Azure Backup“ noch nicht vertraut sind, können Sie sich im Artikel [Was ist Azure Backup?](backup-introduction-to-azure-backup.md) eine Übersicht über den Dienst verschaffen. Stellen Sie zunächst sicher, dass die erforderlichen Voraussetzungen für Azure Backup erfüllt und Ihnen die Einschränkungen der aktuellen VM-Sicherungslösung bekannt sind.

Damit Sie PowerShell effektiv nutzen können, ist es notwendig, dass Sie die Objekthierarchie verstehen und wissen, womit Sie beginnen sollten.

![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Sehen Sie sich die [Azure Backup - Recovery Services-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in der Azure-Bibliothek an, um die Cmdlet-Referenz „AzureRm.RecoveryServices.Backup PowerShell“ anzuzeigen.

## <a name="setup-and-registration"></a>Einrichtung und Registrierung

Vorbereitung:

1. [Laden Sie die neueste PowerShell-Version herunter](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (erforderliche Mindestversion: 1.4.0).

2. Ermitteln Sie die Azure-PowerShell-Cmdlets zur Datensicherung mithilfe des folgenden Befehls:
   
    ```powershell
    Get-Command *azurermrecoveryservices*
    ```    
    Die Aliase und Cmdlets für Azure Backup, Azure Site Recovery und den Recovery Services-Tresor werden angezeigt. Die folgende Abbildung ist ein Beispiel für die angezeigten Elemente. Dies ist nicht die vollständige Liste mit den Cmdlets.

    ![Recovery Services-Liste](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. Melden Sie sich mithilfe von **Connect-AzureRmAccount** bei Ihrem Azure-Konto an. Dieses Cmdlet ruft eine Webseite auf, die Sie zur Eingabe Ihrer Kontoanmeldeinformationen auffordert:

    * Alternativ können Sie die Kontoanmeldeinformationen als Parameter im Cmdlet **Connect-AzureRmAccount** mit dem Parameter **-Credential** einschließen.
    * Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primärem Mandantendomänennamen als Mandanten an. Beispiel: **Connect-AzureRmAccount -Tenant "fabrikam.com"**

4. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Falls Sie Azure Backup zum ersten Mal verwenden, müssen Sie das Cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** verwenden, um den Azure Recovery Service-Anbieter für Ihr Abonnement zu registrieren.

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Mithilfe der folgenden Befehle können Sie überprüfen, ob die Anbieter erfolgreich registriert wurden:
    ```powershell
    Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ``` 
    In der Befehlsausgabe sollte sich der **RegistrationState**-Wert in **Registriert** ändern. Ist dies nicht der Fall, führen Sie das oben genannte Cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** einfach erneut aus.

Die folgenden Aufgaben können mit PowerShell automatisiert werden:

* [Erstellen eines Recovery Services-Tresors](backup-azure-vms-automation.md#create-a-recovery-services-vault)
* [Sichern virtueller Azure-Computer](backup-azure-vms-automation.md#back-up-azure-vms)
* [Auslösen eines Sicherungsauftrags](backup-azure-vms-automation.md#trigger-a-backup-job)
* [Überwachen eines Sicherungsauftrags](backup-azure-vms-automation.md#monitoring-a-backup-job)
* [Wiederherstellen einer Azure-VM](backup-azure-vms-automation.md#restore-an-azure-vm)

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Mit den folgenden Schritten können Sie einen Recovery Services-Tresor erstellen. Ein Recovery Services-Tresor unterscheidet sich von einem Sicherungstresor.

1. Der Recovery Services-Tresor ist eine Resource Manager-Ressource. Deshalb müssen Sie ihn in eine Ressourcengruppe einfügen. Sie können eine vorhandene Ressourcengruppe verwenden oder eine Ressourcengruppe mit dem Cmdlet **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** erstellen. Wenn Sie eine Ressourcengruppe erstellen, geben Sie den Namen und den Speicherort für die Ressourcengruppe an.  

    ```powershell
    New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Erstellen Sie mithilfe des Cmdlets **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** den Recovery Services-Tresor. Stellen Sie sicher, dass Sie den gleichen Speicherort für den Tresor angeben, der für die Ressourcengruppe verwendet wurde.

    ```powershell
    New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geben Sie den Typ der zu verwendenden Speicherredundanz an – entweder [lokal redundanter Speicher (LRS)](../storage/common/storage-redundancy-lrs.md) oder [geografisch redundanter Speicher (GRS)](../storage/common/storage-redundancy-grs.md). Das folgende Beispiel zeigt, dass für die Option „BackupStorageRedundancy“ für „testVault“ der Wert auf „GeoRedundant“ festgelegt ist.

    ```powershell
    $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Viele Azure Backup-Cmdlets benötigen das Recovery Services-Tresorobjekt als Eingabe. Aus diesem Grund sollte das zur Sicherung verwendete Recovery Services-Tresorobjekt in einer Variablen gespeichert werden.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Tresoren in einem Abonnement

Verwenden Sie **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**, um alle Tresore des Abonnements anzuzeigen:

```powershell
Get-AzureRmRecoveryServicesVault
```

Die Ausgabe ähnelt dem folgenden Beispiel. Beachten Sie, dass die zugeordneten Elemente „ResourceGroupName“ und „Location“ angegeben sind.

```
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Sichern von Azure-VMs

Verwenden Sie einen Recovery Services-Tresor zum Schutz Ihrer virtuellen Computer. Bevor Sie den Schutz anwenden, legen Sie den Tresorkontext (den Typ der Daten, die im Tresor geschützt werden) fest, und überprüfen Sie die Schutzrichtlinie. Die Schutzrichtlinie stellt den Zeitplan für die Ausführung des Sicherungsauftrags dar, und für die Aufbewahrungsdauer jeder Sicherungsmomentaufnahme.

### <a name="set-vault-context"></a>Festlegen des Tresorskontexts

Verwenden Sie vor dem Aktivieren des Schutzes auf einem virtuellen Computer **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**, um den Tresorkontext festzulegen. Sobald der Tresorkontext festgelegt ist, gilt er für alle nachfolgenden-Cmdlets. Im folgenden Beispiel wird der Tresorkontext für den Tresor *testvault* festgelegt.

```powershell
Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Erstellen einer Schutzrichtlinie

Wenn Sie einen Recovery Services-Tresor erstellen, enthält er automatisch standardmäßige Schutz- und Aufbewahrungsrichtlinien. Die standardmäßige Schutzrichtlinie löst täglich zu einem angegebenen Zeitpunkt einen Sicherungsauftrag aus. Die standardmäßige Aufbewahrungsrichtlinie bewahrt den täglichen Wiederherstellungspunkt für 30 Tage auf. Mit der Standardrichtlinie können Sie Ihren virtuellen Computer schnell schützen und die Richtlinie später mit anderen Details bearbeiten.

Verwenden Sie **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**, um die im Tresor verfügbaren Schutzrichtlinien anzuzeigen. Sie können dieses Cmdlet zum Abrufen einer bestimmten Richtlinie verwenden oder um Richtlinien anzuzeigen, die einem bestimmten Workloadtyp zugeordnet sind. Im folgenden Beispiel werden Richtlinien für den Workloadtyp „AzureVM“ abgerufen.

```powershell
Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> Die Zeitzone des Feldes „BackupTime“ in PowerShell ist UTC. Wenn jedoch der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.
>
>

Eine Sicherungsschutzrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Die Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt in gespeichert wird, bevor er gelöscht wird. Verwenden Sie **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**, um die standardmäßige Aufbewahrungsrichtlinie anzuzeigen.  Auf ganz ähnliche Weise können Sie **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** verwenden, um die standardmäßige Zeitplanrichtlinie abzurufen. Das Cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** erstellt ein PowerShell-Objekt, das Informationen zu Sicherungsrichtlinien enthält. Die Zeitplan- und Aufbewahrungsrichtlinienobjekte werden als Eingaben für das Cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** verwendet. Im folgenden Beispiel werden die Zeitplanrichtlinie und die Aufbewahrungsrichtlinie in Variablen gespeichert. Im Beispiel werden diese Variablen verwendet, um die Parameter beim Erstellen einer Schutzrichtlinie,*NewPolicy*, zu definieren.

```powershell
$schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>Schutz aktivieren

Nachdem Sie die Schutzrichtlinie definiert haben, müssen Sie noch die Richtlinie für ein Element aktivieren. Verwenden Sie **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**, um den Schutz zu aktivieren. Zum Aktivieren des Schutzes sind zwei Objekte erforderlich – das Element und die Richtlinie. Sobald die Richtlinie mit dem Tresor verknüpft ist, wird der Sicherungsworkflow zu dem im Richtlinienzeitplan definierten Zeitpunkt gestartet.

Im folgenden Beispiel wird der Schutz für das Element „V2VM“ mithilfe der Richtlinie „NewPolicy“ aktiviert. Die Beispiele unterscheiden sich in Abhängigkeit davon, ob der virtuelle Computer verschlüsselt ist und welche Art von Verschlüsselung verwendet wird. 

Gehen Sie wie folgt vor, um den Schutz für **nicht verschlüsselte Resource Manager-VMs** zu aktivieren:

```powershell
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Um den Schutz auf **verschlüsselten VMs (mit BEK und KEK verschlüsselt)** zu aktivieren, müssen Sie dem Azure Backup-Dienst die Berechtigung zum Lesen von Schlüsseln und geheimen Schlüsseln aus dem Schlüsseltresor zuweisen.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Um den Schutz auf **verschlüsselten VMs (nur mit BEK verschlüsselt)** zu aktivieren, müssen Sie dem Azure Backup-Dienst die Berechtigung zum Lesen von Geheimnissen aus dem Schlüsseltresor zuweisen.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Wenn Sie die Azure Government-Cloud verwenden, verwenden Sie im Cmdlet [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) den Wert ff281ffe-705c-4f53-9f37-a40e6f2c68f3 für den Parameter **ServicePrincipalName**.
>
>

Aktivieren Sie den Schutz auf einem klassischen virtuellen Computer wie folgt:

```powershell
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Ändern einer Schutzrichtlinie

Verwenden Sie [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) zum Bearbeiten der Schutzrichtlinie, um die Objekte „SchedulePolicy“ oder „RetentionPolicy“ zu ändern.

Im folgenden Beispiel wird der Aufbewahrungswert von Wiederherstellungspunkten in 365 Tage geändert.

```powershell
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Auslösen einer Sicherung

Verwenden Sie **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**, um einen Sicherungsauftrag auszulösen. Wenn es die erste Sicherung ist, handelt es sich um eine vollständige Sicherung. Nachfolgende Sicherungen erstellen inkrementelle Kopien. Verwenden Sie vor dem Auslösen eines Sicherungsauftrags **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**, um den Tresorkontext festzulegen. Im folgenden Beispiel wird davon ausgegangen, dass der Tresorkontext bereits festgelegt wurde.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> Die Zeitzone der Felder „StartTime“ und „EndTime“ in PowerShell ist UTC. Wenn jedoch der Zeitpunkt im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.
>
>

## <a name="monitoring-a-backup-job"></a>Überwachen eines Sicherungsauftrags

Sie können Vorgänge mit langer Ausführungsdauer, z.B. Sicherungsaufträge, ohne das Azure-Portal überwachen. Mit dem Cmdlet **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** können Sie den Status eines Auftrags abrufen, der sich in Bearbeitung befindet. Dieses Cmdlet ruft die Sicherungsaufträge für einen bestimmten Tresor ab. Dieser Tresor ist im Tresorkontext angegeben. Im folgenden Beispiel wird der Status eines laufenden Auftrags als Array abgerufen und in der Variable „$joblist“ gespeichert.

```powershell
$joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
$joblist[0]
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Anstatt die Fertigstellung dieser Aufträge abzufragen – was unnötigen, zusätzlichen Code bedeutet –, verwenden Sie das Cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**. Dieses Cmdlet hält die Ausführung an, bis entweder der Auftrag abgeschlossen oder der angegebene Timeoutwert erreicht ist.

```powershell
Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Wiederherstellen eines virtuellen Azure-Computers

Es gibt einen wichtigen Unterschied zwischen dem Wiederherstellen eines virtuellen Computers mithilfe des Azure-Portals und dem Wiederherstellen eines virtuellen Computers mithilfe von PowerShell. Mit PowerShell ist der Wiederherstellungsvorgang mit der Erstellung der Datenträger und den Konfigurationsinformationen aus dem Wiederherstellungspunkt abgeschlossen. Beim Wiederherstellungsvorgang wird kein virtueller Computer erstellt. Informationen zum Erstellen eines virtuellen Computers aus einem Datenträger finden Sie im Abschnitt [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). Wenn Sie nicht den gesamten virtuellen Computer wiederherstellen möchten, sondern nur einige Dateien aus einer Azure-VM-Sicherung, helfen Ihnen die Informationen im [Abschnitt zur Dateiwiederherstellung](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup) weiter.

> [!Tip]
> Beim Wiederherstellungsvorgang wird kein virtueller Computer erstellt.
>
>

Die folgende Grafik zeigt die Objekthierarchie von „RecoveryServicesVault“ bis „BackupRecoveryPoint“.

![Die Recovery Services-Objekthierarchie zeigt die BackupContainer an](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Identifizieren Sie das gesicherte Element und den Wiederherstellungspunkt, der die Zeitpunktdaten enthält, um die gesicherten Daten wiederherstellen zu können. Verwenden Sie **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**, um Daten aus dem Tresor in Ihrem Konto wiederherzustellen.

Zum Wiederherstellen eines virtuellen Azure-Computers müssen Sie folgende grundlegende Schritte ausführen:

* Wählen Sie den virtuellen Computer aus.
* Wählen Sie einen Wiederherstellungspunkt aus.
* Stellen Sie die Datenträger wieder her.
* Erstellen Sie den virtuellen Computer über die gespeicherten Datenträger.

### <a name="select-the-vm"></a>Auswählen des virtuellen Computers

Zum Abrufen des PowerShell-Objekts, das das richtige Sicherungselement identifiziert, starten Sie vom Container im Tresor aus, und durchlaufen Sie die Objekthierarchie nach unten. Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**, und reichen Sie dieses an das Cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** weiter, um den Container auszuwählen, der den virtuellen Computer darstellt.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Auswählen eines Wiederherstellungspunkts

Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**, um alle Wiederherstellungspunkte für das Sicherungselement aufzulisten. Wählen Sie anschließend den zu wiederherstellenden Wiederherstellungspunkt. Wenn Sie nicht sicher sind, welchen Wiederherstellungspunkt Sie verwenden sollen, empfiehlt es sich, den letzten Punkt in der Liste auszuwählen, an dem sich RecoveryPointType = AppConsistent befand.

Im folgenden Skript ist die Variable **$rp** ein Array von Wiederherstellungspunkten für das ausgewählte Sicherungselement der letzten sieben Tage. Das Array wird in umgekehrter Reihenfolge sortiert. Der letzte Wiederherstellungspunkt liegt bei Index 0. Verwenden Sie die standardmäßige PowerShell-Arrayindizierung zum Auswählen des Wiederherstellungspunkts. Im Beispiel wählt „$rp[0]“ den letzten Wiederherstellungspunkt aus.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="restore-the-disks"></a>Wiederherstellung der Datenträger

Verwenden Sie das Cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**, um Daten und die Konfiguration eines Sicherungselements auf einem Wiederherstellungspunkt wiederherzustellen. Nachdem Sie einen Wiederherstellungspunkt ermittelt haben, können Sie ihn als Wert für den **-RecoveryPoint**-Parameter verwenden. Im obigen Beispiel wurde **$rp[0]** als Wiederherstellungspunkt verwendet. Im folgenden Beispielcode ist **$rp[0]** der Wiederherstellungspunkt, der zum Wiederherstellen des Datenträgers verwendet wird.

So stellen Sie den Datenträger und die Konfigurationsinformationen wieder her:

```powershell
$restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
$restorejob
```
#### <a name="restore-managed-disks"></a>Wiederherstellen von verwalteten Datenträgern

> [!NOTE]
> Wenn die gesicherte VM über verwaltete Datenträger verfügt und Sie diese als verwaltete Datenträger wiederherstellen möchten, können Sie die Funktion aus Azure PowerShell v6.7.0 oder höher nutzen.
>
>

Geben Sie den zusätzlichen Parameter **TargetResourceGroupName** an, um die RG festzulegen, in der die verwalteten Datenträger gespeichert werden.


```powershell
$restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -TargetResourceGroupName "DestRGforManagedDisks"
```

Die Datei **VMConfig.JSON** wird für das Speicherkonto wiederhergestellt, und die verwalteten Datenträger werden in der angegebenen Zielressourcengruppe wiederhergestellt.


Die Ausgabe sieht in etwa wie das folgende Beispiel aus:
```
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Verwenden Sie das Cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**, und warten Sie auf den Abschluss des Wiederherstellungsauftrags.

```powershell
Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Nachdem der Wiederherstellungsauftrag abgeschlossen ist, verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**, um die Details des Wiederherstellungsvorgangs abzurufen. Die JobDetails-Eigenschaft enthält die Informationen, die zum erneuten Erstellen des virtuellen Computers benötigt werden.

```powershell
$restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
$details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Nachdem Sie die Datenträger wiederhergestellt haben, fahren Sie mit dem nächsten Abschnitt fort, um den virtuellen Computer zu erstellen.

## <a name="create-a-vm-from-restored-disks"></a>Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern

Nachdem Sie die Datenträger wiederhergestellt haben, können Sie die folgenden Schritte zum Erstellen und Konfigurieren des virtuellen Computers vom Datenträger verwenden.

> [!NOTE]
> Zum Erstellen von verschlüsselten virtuellen Computern aus wiederhergestellten Datenträgern muss Ihre Azure-Rolle über die Berechtigung zum Ausführen der Aktion **Microsoft.KeyVault/vaults/deploy/action** verfügen. Wenn Ihre Rolle nicht über diese Berechtigung verfügt, erstellen Sie mit dieser Aktion eine benutzerdefinierte Rolle. Weitere Informationen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](../role-based-access-control/custom-roles.md).
>
>

1. Fragen Sie die Eigenschaften des wiederhergestellten Datenträgers für die Auftragsdetails ab.

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. Legen Sie den Azure-Speicherkontext fest, und stellen Sie die JSON-Konfigurationsdatei wieder her.

   ```powershell
   Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzureStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. Verwenden Sie die JSON-Konfigurationsdatei, um die Konfiguration des virtuellen Computers zu erstellen.

   ```powershell
   $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. Fügen Sie die Betriebssystemdatenträger und Datenträger hinzu. Dieser Schritt enthält Beispiele für verschiedene verwaltete und verschlüsselte VM-Konfigurationen. Verwenden Sie das Beispiel, das für Ihre VM-Konfiguration geeignet ist.

   * **Nicht verwaltete unverschlüsselte VMs**: Verwenden Sie das folgende Beispiel für nicht verwaltete unverschlüsselte VMs.

       ```powershell
       Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
       ```

   * **Nicht verwaltete, verschlüsselte VMs (nur BEK)**: Für nicht verwaltete, verschlüsselte VMs (nur mit BEK verschlüsselt) müssen Sie das Geheimnis für den Schlüsseltresor wiederherstellen, bevor Sie Datenträger anfügen können. Weitere Informationen finden Sie im Artikel [Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Azure Backup-Wiederherstellungspunkts](backup-azure-restore-key-secret.md). Im folgenden Beispiel wird gezeigt, wie man das Betriebssystem und die Datenträger für verschlüsselte VMs anfügt. Achten Sie beim Festlegen des Betriebssystemdatenträgers darauf, dass der relevante Betriebssystemtyp angegeben wird.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```
      Verwenden Sie den folgenden Befehl, um die Verschlüsselung für die Datenträger manuell zu aktivieren.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -VolumeType Data
      ```

   * **Nicht verwaltete, verschlüsselte VMs (BEK und KEK)**: Stellen Sie für nicht verwaltete, verschlüsselte VMs (per BEK und KEK verschlüsselt) den Schlüssel und das Geheimnis für den Schlüsseltresor wieder her, bevor Sie die Datenträger anfügen. Weitere Informationen finden Sie im Artikel [Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Azure Backup-Wiederherstellungspunkts](backup-azure-restore-key-secret.md). Im folgenden Beispiel wird gezeigt, wie man das Betriebssystem und die Datenträger für verschlüsselte VMs anfügt.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
       {
       $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
      ```

      Verwenden Sie den folgenden Befehl, um die Verschlüsselung für die Datenträger manuell zu aktivieren.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **Verwaltete, unverschlüsselte VMs**: Fügen Sie für verwaltete und unverschlüsselte VMs die wiederhergestellten verwalteten Datenträger an. Ausführliche Informationen finden Sie im Artikel [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md).

   * **Verwaltete, verschlüsselte VMs (nur BEK)**: Fügen Sie für verwaltete, verschlüsselte VMs (nur mit BEK verschlüsselt) die wiederhergestellten verwalteten Datenträger an. Ausführliche Informationen finden Sie im Artikel [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md).
   
      Verwenden Sie den folgenden Befehl, um die Verschlüsselung für die Datenträger manuell zu aktivieren.

       ```powershell
       Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
       ```

   * **Verwaltete, verschlüsselte VMs (BEK und KEK)**: Fügen Sie für verwaltete, verschlüsselte VMs (mit BEK und KEK verschlüsselt) die wiederhergestellten verwalteten Datenträger an. Ausführliche Informationen finden Sie im Artikel [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md). 

      Verwenden Sie den folgenden Befehl, um die Verschlüsselung für die Datenträger manuell zu aktivieren.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

5. Legen Sie die Netzwerkeinstellungen fest.

    ```powershell
    $nicName="p1234"
    $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzureRmVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzureRmVirtualNetwork
    $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Erstellen Sie den virtuellen Computer.

    ```powershell  
    New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern

Sie können nicht nur Datenträger, sondern auch einzelne Dateien aus einer Sicherung von virtuellen Azure-Computern wiederherstellen. Für die Funktionen zum Wiederherstellen von Dateien ist der Zugriff auf alle Dateien eines Wiederherstellungspunkts möglich. Verwalten Sie die Dateien über den Datei-Explorer, wie Sie dies für andere Dateien gewohnt sind.

Grundlegende Schritte zum Wiederherstellen einer Datei aus einer Sicherung von virtuellen Azure-Computern:

* Auswählen des virtuellen Computers
* Auswählen eines Wiederherstellungspunkts
* Bereitstellen der Datenträger des Wiederherstellungspunkts
* Kopieren der erforderlichen Dateien
* Aufheben der Bereitstellung des Datenträgers


### <a name="select-the-vm"></a>Auswählen des virtuellen Computers

Zum Abrufen des PowerShell-Objekts, das das richtige Sicherungselement identifiziert, starten Sie vom Container im Tresor aus, und durchlaufen Sie die Objekthierarchie nach unten. Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**, und reichen Sie dieses an das Cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** weiter, um den Container auszuwählen, der den virtuellen Computer darstellt.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Auswählen eines Wiederherstellungspunkts

Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**, um alle Wiederherstellungspunkte für das Sicherungselement aufzulisten. Wählen Sie anschließend den zu wiederherstellenden Wiederherstellungspunkt. Wenn Sie nicht sicher sind, welchen Wiederherstellungspunkt Sie verwenden sollen, empfiehlt es sich, den letzten Punkt in der Liste auszuwählen, an dem sich RecoveryPointType = AppConsistent befand.

Im folgenden Skript ist die Variable **$rp** ein Array von Wiederherstellungspunkten für das ausgewählte Sicherungselement der letzten sieben Tage. Das Array wird in umgekehrter Reihenfolge sortiert. Der letzte Wiederherstellungspunkt liegt bei Index 0. Verwenden Sie die standardmäßige PowerShell-Arrayindizierung zum Auswählen des Wiederherstellungspunkts. Im Beispiel wählt „$rp[0]“ den letzten Wiederherstellungspunkt aus.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>Bereitstellen der Datenträger des Wiederherstellungspunkts

Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)**, um das Skript zum Bereitstellen aller Datenträger des Wiederherstellungspunkts abzurufen.

> [!NOTE]
> Die Datenträger werden als angefügte iSCSI-Datenträger auf dem Computer bereitgestellt, auf dem das Skript ausgeführt wird. Die Bereitstellung wird sofort durchgeführt, und es fallen keine Gebühren an.
>
>

```powershell
Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

Führen Sie das Skript auf dem Computer aus, auf dem die Dateien wiederhergestellt werden sollen. Um das Skript auszuführen, müssen Sie das angegebene Kennwort eingeben. Nachdem die Datenträger angefügt wurden, können Sie mit dem Windows-Datei-Explorer die neuen Volumes und Dateien durchsuchen. Weitere Informationen finden Sie im Backup-Artikel [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](backup-azure-restore-files-from-vm.md).

### <a name="unmount-the-disks"></a>Aufheben der Bereitstellung von Datenträgern

Nachdem die erforderlichen Dateien kopiert wurden, können Sie **[Disable-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)** verwenden, um die Bereitstellung der Datenträger aufzuheben. Achten Sie darauf, dass die Aufhebung der Bereitstellung für die Datenträger durchgeführt wird, damit der Zugriff auf die Dateien des Wiederherstellungspunkts nicht mehr möglich ist.

```powershell
Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

## <a name="next-steps"></a>Nächste Schritte

Falls Sie PowerShell für die Arbeit mit Azure-Ressourcen vorziehen, hilft Ihnen der PowerShell-Artikel [Bereitstellen und Verwalten der Sicherung in Azure für Windows Server-/Windows-Clientcomputer mit PowerShell](backup-client-automation.md) weiter. Wenn Sie DPM-Sicherungen verwalten, finden Sie im Artikel [Bereitstellen und Verwalten der Sicherung für DPM](backup-dpm-automation.md) weitere Informationen. Diese beiden Artikel weisen eine Version für Resource Manager-Bereitstellungen und eine für klassische Bereitstellungen auf.  

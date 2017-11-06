---
title: Replizieren von Hyper-V-VMs mithilfe von PowerShell und Azure Resource Manager | Microsoft-Dokumentation
description: "Automatisieren Sie die Replikation von Hyper-V-VMs in Azure mit Azure Site Recovery über PowerShell und Azure Resource Manager."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: bsiva
ms.openlocfilehash: d93be3b958f85cd2892f92d84ed68996909a5d6b
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs mithilfe von PowerShell und Azure Resource Manager

[Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Azure-Computer und lokaler virtueller Computer sowie physischer Server aufeinander abgestimmt werden.

Dieser Artikel beschreibt, wie Sie Windows PowerShell zusammen mit Azure Resource Manager verwenden, um Hyper-V-VMs in Azure zu replizieren. Das Beispiel in diesem Artikel zeigt, wie Sie einen auf einem Hyper-V-Host ausgeführten einzelnen virtuellen Computer in Azure replizieren.

## <a name="overview"></a>Übersicht

Azure PowerShell stellt Cmdlets zum Verwalten von Azure mit Windows PowerShell zur Verfügung. Mit PowerShell-Cmdlets für Site Recovery, die mit Azure PowerShell für den Azure Resource Manager verfügbar sind, können Sie Ihre Server in Azure schützen und wiederherstellen.

Sie müssen kein PowerShell-Experte sein, um diesen Artikel verwenden zu können. Sie müssen jedoch mit den grundlegenden Konzepten wie Modulen, Cmdlets und Sitzungen vertraut sein. Lesen Sie [Erste Schritte mit Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx) und [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-Partner im CSP-Programm (Cloud Solution Provider, Cloud-Lösungsanbieter) können den Schutz der Server ihrer Kunden in den entsprechenden CSP-Abonnements (Mandantenabonnements) konfigurieren und verwalten.
>
>

## <a name="before-you-start"></a>Vorbereitung
Stellen Sie sicher, dass diese Voraussetzungen erfüllt werden:

* Ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. Darüber hinaus können Sie sich über die [Preisgestaltung für Azure Site Recovery-Manager](https://azure.microsoft.com/pricing/details/site-recovery/)informieren.
* Azure PowerShell 1.0. Informationen zu dieser Version, und wie Sie diese installieren, finden Sie unter [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* Die Module [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) und [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/). Sie erhalten die neuesten Versionen dieser Module im [PowerShell-Katalog](https://www.powershellgallery.com/)

Darüber hinaus gelten für das in diesem Artikel beschriebene Beispiel die folgenden Voraussetzungen:

* Ein Hyper-V-Host unter Windows Server 2012 R2 oder Microsoft Hyper-V Server 2012 R2 mit mindestens einem virtuellen Computer. Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.
* Die virtuellen Computer, die Sie replizieren möchten, sollten [diese Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.

## <a name="step-1-sign-in-to-your-azure-account"></a>Schritt 1: Anmelden bei Ihrem Azure-Konto

1. Öffnen Sie eine PowerShell-Konsole, und führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Konto anzumelden. Das Cmdlet ruft eine Webseite auf, die Sie zur Eingabe Ihrer Kontoanmeldeinformationen auffordert: **Login-AzureRmAccount**.
    - Alternativ können Sie die Kontoanmeldeinformationen als Parameter im Cmdlet **Login-AzureRmAccount** mit dem Parameter **-Credential** einschließen.
    - Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primärem Mandantendomänennamen als Mandanten an. Beispiel: **Login-AzureRmAccount -Tenant "fabrikam.com"**
2. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Stellen Sie mit den folgenden Befehlen sicher, dass Ihr Abonnement für die Verwendung der Azure-Anbieter für Recovery Services und Site Recovery registriert ist:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices``Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Vergewissern Sie sich in der Befehlsausgabe, dass **RegistrationState** auf **Registered** festgelegt ist. Anschließend können Sie mit Schritt 2 fortfahren. Wenn dies nicht der Fall ist, müssen Sie den fehlenden Anbieter in Ihrem Abonnement registrieren, indem Sie die folgenden Befehle ausführen:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery``Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Überprüfen Sie mithilfe der folgenden Befehle, ob die Anbieter erfolgreich registriert wurden:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>Schritt 2: Einrichten des Tresors

1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, in der der Tresor erstellt werden soll, oder verwenden Sie eine vorhandene Ressourcengruppe. Erstellen Sie wie folgt eine neue Ressourcengruppe. Die Variable „$ResourceGroupName“ enthält den Namen der zu erstellenden Ressourcengruppe und die Variable „$Geo“ die Azure-Region, in der die Ressourcengruppe erstellt wird (z.B. „Brasilien, Süden“).

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Führen Sie zum Abrufen einer Liste der Ressourcengruppen in Ihrem Abonnement das Cmdlet **Get-AzureRmResourceGroup** aus.
2. Erstellen Sie einen neuen Azure Recovery Services-Tresor wie folgt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Sie können eine Liste der vorhandenen Tresore mit dem Cmdlet **Get-AzureRmRecoveryServicesVault** abrufen.


## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Festlegen des Kontexts des Recovery Services-Tresors

Legen Sie den Tresorkontext wie folgt fest:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>Schritt 4: Erstellen eines Hyper-V-Standorts

1. Erstellen Sie einen neuen Hyper-V-Standort wie folgt:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Dieses Cmdlet startet einen Site Recovery-Auftrag, um den Standort zu erstellen, und gibt ein Site Recovery-Auftragsobjekt zurück. Warten Sie, bis der Auftrag abgeschlossen ist, und überprüfen Sie, ob der Auftrag erfolgreich abgeschlossen wurde.
3. Verwenden Sie das Cmdlet **Get-AzureRmSiteRecoveryJob** zum Abrufen des Auftragsobjekts, und überprüfen Sie den aktuellen Status des Auftrags.
4. Generieren Sie wie folgt einen Registrierungsschlüssel für den Standort, und laden Sie ihn herunter:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. Kopieren Sie den heruntergeladenen Schlüssel in den Hyper-V-Host. Sie benötigen den Schlüssel, um den Hyper-V-Host am Standort zu registrieren.

## <a name="step-5-install-the-provider-and-agent"></a>Schritt 5: Installieren des Anbieters und des Agents

1. Laden Sie bei [Microsoft](https://aka.ms/downloaddra)das Installationsprogramm für die neueste Version des Anbieters herunter.
2. Führen Sie das Installationsprogramm auf dem Hyper-V-Host aus.
3. Fahren Sie am Ende der Installation mit dem Registrierungsschritt fort.
4. Geben Sie den heruntergeladenen Schlüssel an, wenn Sie dazu aufgefordert werden, und schließen Sie die Registrierung des Hyper-V-Hosts ab.
5. Überprüfen Sie wie folgt, ob der Hyper-V-Host am Standort registriert wurde:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>Schritt 6: Erstellen einer Replikationsrichtlinie

Bevor Sie beginnen, muss sich das angegebene Speicherkonto in der gleichen Azure-Region wie Ihr Tresor befinden, und die Georeplikation muss aktiviert sein.

1. Erstellen Sie eine Replikationsrichtlinie wie folgt:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Überprüfen Sie den zurückgegebenen Auftrag, um sicherzustellen, dass die Replikationsrichtlinie erfolgreich erstellt wurde.

3. Rufen Sie den Schutzcontainer, der dem Standort entspricht, wie folgt ab:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Ordnen Sie den Schutzcontainers der Replikationsrichtlinie wie folgt zu:

     $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

4. Warten Sie, bis der Zuordnungsauftrag erfolgreich abgeschlossen wurde.

## <a name="step-7-enable-vm-protection"></a>Schritt 7: Aktivieren des Schutzes des virtuellen Computers

1. Rufen Sie die Schutzentität, die dem zu schützenden virtuellen Computer entspricht, folgendermaßen ab:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Schützen Sie den virtuellen Computer. Wenn dem geschützten virtuellen Computer mehrere Datenträger angefügt sind, geben Sie den Betriebssystemdatenträger mit dem *OSDiskName* -Parameter an.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. Warten Sie, bis die virtuellen Computer nach der ersten Replikation einen geschützten Zustand erreicht haben. Abhängig von Faktoren wie der Menge der zu replizierenden Daten und der verfügbare Upstreambandbreite zu Azure kann dies länger dauern. Die Aufträge „State“ und „StateDescription“ werden wie folgt aktualisiert, sobald der virtuelle Computer einen geschützten Zustand erreicht: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Aktualisieren Sie die Wiederherstellungseigenschaften (z.B. die Größe der VM-Rolle) und das Azure-Netzwerk, an das die NIC des virtuellen Computers nach dem Failover angefügt werden soll.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Schritt 8: Ausführen eines Testfailovers
1. Führen Sie ein Testfailover wie folgt aus:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Stellen Sie sicher, dass die Test-VM in Azure erstellt wird. Der Testfailoverauftrag wird nach dem Erstellen des virtuellen Testcomputers in Azure angehalten.
3. Führen Sie zum Bereinigen und Abschließen des Testfailovers Folgendes aus:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr](https://msdn.microsoft.com/library/azure/mt637930.aspx) über Azure Site Recovery mit PowerShell-Cmdlets für Azure Resource Manager.

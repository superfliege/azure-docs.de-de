---
title: "Replizieren von Hyper-V-VMs in VMM-Clouds an einen sekundären Standort mithilfe von PowerShell (Azure Resource Manager) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie virtuelle Hyper-V-Computer in VMM-Clouds mit PowerShell (Resource Manager) an einen sekundären VMM-Standort replizieren."
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Replizieren von Hyper-V-VMs an einen sekundären Standort mithilfe von PowerShell (Resource Manager)

In diesem Artikel wird das Automatisieren der Schritte für die Replikation von Hyper-V-VMs in System Center Virtual Machine Manager-Clouds (VMM) in eine VMM-Cloud an einem sekundären lokalen Standort mit [Azure Site Recovery](site-recovery-overview.md) beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Überprüfen Sie [Architektur und Komponenten des Szenarios](hyper-v-vmm-architecture.md).
- Überprüfen Sie die [Supportanforderungen](site-recovery-support-matrix-to-sec-site.md) für alle Komponenten.
- Stellen Sie sicher, dass VMM-Server und Hyper-V-Hosts den [Supportanforderungen](site-recovery-support-matrix-to-sec-site.md) entsprechen.
- Überprüfen Sie, ob die virtuellen Computer, die Sie replizieren möchten, die Anforderungen für die [Unterstützung replizierter Computer](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) erfüllen.


## <a name="prepare-for-network-mapping"></a>Bereiten Sie sich auf die Netzwerkzuordnung vor

Die [Netzwerkzuordnung](hyper-v-vmm-network-mapping.md) ist die Zuordnung zwischen lokalen VMM-VM-Netzwerken in Quell- und Zielclouds. Die Zuordnung ermöglicht Folgendes:

- Nach dem Failover werden Verbindungen von VMs mit geeigneten Ziel-VM-Netzwerken hergestellt. 
- Replikat-VMs werden optimal auf Ziel-Hyper-V-Hostservern platziert. 
- Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden nach einem Failover keine Verbindungen von Replikat-VMs mit einem VM-Netzwerk hergestellt.

Bereiten Sie VMM wie folgt vor:

1. Stellen Sie sicher, dass [logische VMM-Netzwerke](https://docs.microsoft.com/system-center/vmm/network-logical) auf den Quell- und Ziel-VMM-Servern vorhanden sind.
    - Das logische Netzwerk auf dem Quellserver sollte der Quellcloud zugeordnet sein, in der sich Hyper-V-Hosts befinden.
    - Das logische Netzwerk auf dem Zielserver sollte der Zielcloud zugeordnet werden.
1. Stellen Sie sicher, dass [VM-Netzwerke](https://docs.microsoft.com/system-center/vmm/network-virtual) auf den Quell- und Ziel-VMM-Servern vorhanden sind. VM-Netzwerke sollten an jedem Standort mit dem logischen Netzwerk verknüpft werden.
2. Stellen Sie die Verbindung der virtuellen Computer auf den Hyper-V-Quellhosts mit dem VM-Quellnetzwerk her. 

## <a name="prepare-for-powershell"></a>Vorbereiten von PowerShell

Stellen Sie sicher, dass Azure PowerShell einsatzbereit ist.

- Wenn Sie PowerShell bereits verwenden, müssen Sie auf Version 0.8.10 oder höher aktualisieren.  Informieren Sie sich über das [Einrichten von PowerShell](/powershell/azureps-cmdlets-docs).
- Nach Abschluss der Einrichtung und Konfiguration von PowerShell überprüfen Sie die [Dienst-Cmdlets](/powershell/azure/overview).
- Weitere Informationen zum Verwenden der Parameterwerte, Eingaben und Ausgaben in Azure PowerShell lesen Sie die Anleitung mit den [Ersten Schritten](/powershell/azure/get-started-azureps).

## <a name="set-up-a-subscription"></a>Einrichten eines Abonnements
1. Melden Sie sich in Azure PowerShell bei Ihrem Azure-Konto an:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Rufen Sie über die Abonnement-IDs eine Liste Ihrer Abonnements ab. Notieren Sie sich die ID des Abonnements, in dem Sie den Recovery Services-Tresor erstellen möchten.   

        Get-AzureRmSubscription
3. Richten Sie das Abonnement für den Tresor ein:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, falls noch keine vorhanden ist.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Erstellen Sie einen neuen Recovery Services-Tresor, und speichern Sie das Tresorobjekt in einer Variable, die später verwendet wird: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Sie können das Tresorobjekt auch nach der Erstellung abrufen, indem Sie das Cmdlet Get-AzureRMRecoveryServicesVault verwenden.

## <a name="set-the-vault-context"></a>Festlegen des Tresorkontexts
1. Rufen Sie einen vorhandenen Tresor ab:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Legen Sie den Tresorkontexts fest:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Installieren des Azure Site Recovery-Anbieters
1. Erstellen Sie ein Verzeichnis auf dem VMM-Computer durch Ausführen des folgenden Befehls:

       New-Item c:\ASR -type directory
2. Extrahieren Sie die Dateien mithilfe der heruntergeladenen Anbietersetupdatei: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installieren Sie den Anbieter, und warten Sie, bis die Installation abgeschlossen wurde:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Registrieren Sie den Server im Tresor:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Erstellen und Zuordnen einer Replikationsrichtlinie
1. Erstellen Sie wie folgt eine Replikationsrichtlinie (in diesem Fall für Hyper-V 2012 R2):

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > Die VMM-Cloud kann Hyper-V-Hosts enthalten, auf denen verschiedene Versionen von Windows Server ausgeführt werden, aber die Replikationsrichtlinie ist betriebssystemspezifisch. Wenn Sie über verschiedene Hosts verfügen, die unter unterschiedlichen Betriebssystemversionen ausgeführt werden, erstellen Sie getrennte Replikationsrichtlinien für jedes System. Wenn Sie z.B. fünf Hosts unter Windows Server 2012 und drei Hosts unter Windows Server 2012 R2 ausführen, erstellen Sie zwei Replikationsrichtlinien: jeweils eine Richtlinie für jeden Betriebssystemtyp.

2. Rufen Sie den primären Schutzcontainer (primäre VMM-Cloud) und den Schutzcontainer für die Wiederherstellung (VMM-Cloud für die Wiederherstellung) ab:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Rufen Sie die Replikationsrichtlinie, die Sie erstellt haben, mithilfe des Anzeigenamens ab:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Starten Sie die Zuordnung des Schutzcontainers (VMM-Cloud) zur Replikationsrichtlinie:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Warten Sie, bis der Auftrag der Richtlinienzuordnung abgeschlossen ist. Mithilfe des folgenden PowerShell-Codeausschnitts können Sie prüfen, ob der Auftrag abgeschlossen ist:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Nachdem die Verarbeitung des Auftrags abgeschlossen ist, führen Sie den folgenden Befehl aus:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor-activity) aus.

##  <a name="configure-network-mapping"></a>Konfigurieren der Netzwerkzuordnung
1. Verwenden Sie diesen Befehl, um Server für den aktuellen Tresor abzurufen. Der Befehl speichert die Azure Site Recovery-Server in der $Servers-Arrayvariable.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Führen Sie diesen Befehl aus, um die Netzwerke für den VMM-Quellserver und den VMM-Zielserver abzurufen.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Der VMM-Quellserver kann der erste oder zweite Server im Serverarray sein. Überprüfen Sie die Namen der VMM-Server, und rufen Sie die Netzwerke entsprechend ab.


3. Dieses Cmdlet erstellt eine Zuordnung zwischen dem primären Netzwerk und dem Wiederherstellungsnetzwerk. Es legt das primäre Netzwerk als erstes Element von $PrimaryNetworks und das Wiederherstellungsnetzwerk als erstes Element von $RecoveryNetworks fest.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Aktivieren des Schutzes für virtuelle Computer
Nach der korrekten Konfiguration der Server, Clouds und Netzwerke können Sie den Schutz für die virtuellen Computer in der Cloud aktivieren.

1. Führen Sie den folgenden Befehl aus, um den Schutzcontainer abzurufen und den Schutz zu aktivieren:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Rufen Sie die Schutzentität (VM) wie folgt ab:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Aktivieren Sie die Replikation für die VM:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Um Ihre Bereitstellung zu testen, können Sie ein Testfailover für einen einzelnen virtuellen Computer ausführen oder einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer umfasst, und ein Testfailover für diesen Plan durchführen. Das Test-Failover simuliert Ihre Failover- und Wiederherstellungsmechanismen in einem isolierten Netzwerk.

1. Rufen Sie den virtuellen Computer ab, auf den das Failover für die virtuellen Computer ausgeführt wird:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Führen Sie wie folgt ein Testfailover aus:
    - Für einen einzelnen virtuellen Computer

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Für einen Wiederherstellungsplan

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor-activity) aus.

## <a name="run-planned-and-unplanned-failovers"></a>Ausführen geplanter und ungeplanter Failover

1. Führen Sie wie folgt ein geplantes Failover aus:
    -  Für einen einzelnen virtuellen Computer

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
    - Für einen Wiederherstellungsplan

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan
2. Führen Sie wie folgt ein ungeplantes Failover aus:
    - Für einen einzelnen virtuellen Computer
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

    - Für einen Wiederherstellungsplan

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Überwachen der Aktivität
Verwenden Sie die folgenden Befehle zum Überwachen der Failoveraktivität. Beachten Sie, dass Sie zwischen den Aufträgen auf den Abschluss der Verarbeitung warten müssen.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](/powershell/module/azurerm.recoveryservices.backup/#recovery) über Site Recovery mit PowerShell-Cmdlets für Azure Resource Manager.

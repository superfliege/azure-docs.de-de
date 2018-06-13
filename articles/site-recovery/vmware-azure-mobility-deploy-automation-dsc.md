---
title: Bereitstellen von Site Recovery Mobility Service mithilfe von Azure Automation DSC | Microsoft-Dokumentation
description: Beschreibt, wie Sie Azure Automation DSC verwenden, um den Azure Site Recovery Mobility Service und den Azure-Agent automatisch für die Replikation von VMware-VMs und physischen Servern in Azure bereitzustellen.
services: site-recovery
author: krnese
manager: lorenr
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: krnese
ms.openlocfilehash: 0bf1b551ba578cd152201c131bd60470bdc9d86a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
ms.locfileid: "29811357"
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc"></a>Bereitstellen des Mobilitätsdiensts mithilfe von Azure Automation DSC

Sie stellen Mobility Service auf VMware-VMs und physischen Servern bereit, die Sie mit [Azure Site Recovery](site-recovery-overview.md) in Azure replizieren möchten.

Dieser Artikel beschreibt die Bereitstellung des Diensts auf Windows-Computern mithilfe von Azure Automation Desired State Configuration (DSC). Damit wird Folgendes sichergestellt:


## <a name="prerequisites"></a>Voraussetzungen

* Ein Repository zum Speichern des erforderlichen Setups
* Ein Repository zum Speichern der erforderlichen Passphrase zur Registrierung beim Verwaltungsserver. Eine eindeutige Passphrase ist für einen bestimmten Konfigurationsserver generiert. 
* Windows Management Framework (WMF) 5.0 sollte auf den Computern, für die Sie den Schutz aktivieren möchten, installiert sein. Dies ist eine Voraussetzung für Automation DSC.
Informationen zum Verwenden von DSC für Windows-Computer, auf denen WMF 4.0 installiert ist, finden Sie unter [Verwenden von DSC in getrennten Umgebungen](## Use DSC in disconnected environments).
 

## <a name="extract-binaries"></a>Extrahieren der Binärdateien

Der Mobility Service kann über die Befehlszeile installiert werden und akzeptiert mehrere Argumente. Sie müssen die Binärdateien (nachdem sie aus dem Setup extrahiert wurden) an einem Ort speichern, von dem Sie sie mit einer DSC-Konfiguration abrufen können.

1. Um die Dateien, die Sie für diese Konfiguration benötigen, zu extrahieren, wechseln Sie in das folgende Verzeichnis auf dem Verwaltungsserver: **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.
2. Überprüfen Sie in diesem Ordner, ob die MSI-Datei **Microsoft ASR_UA_Version_Windows_GA_Datum_Release.exe** vorhanden ist.
3. Verwenden Sie den folgenden Befehl, um das Installationsprogramm zu extrahieren: **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Wählen Sie alle Dateien aus, und senden Sie sie an einen komprimierten (gezippten) Ordner.

Jetzt haben Sie die Binärdateien, die Sie zum Automatisieren des Mobility Service-Setups mit Automation DSC benötigen.

## <a name="store-the-passphrase"></a>Speichern der Passphrase

Sie müssen festlegen, wo der gezippte Ordner gespeichert werden soll. Sie können ein Azure-Speicherkonto verwenden, wie weiter unten dargestellt, um die für das Setup erforderliche Passphrase zu speichern. Der Agent wird dann während des Prozesses beim Verwaltungsserver registriert.

1. Speichern Sie die Passphrase, die Sie beim Bereitstellen des Verwaltungsservers erhalten haben, in einer Textdatei namens „passphrase.txt“.
2. Legen Sie den ZIP-Ordner und die Passphrase in einem dedizierten Container im Azure-Speicherkonto ab.


Falls Sie diese Dateien lieber auf einer Freigabe in Ihrem Netzwerk speichern möchten, ist dies ebenfalls möglich. Sie müssen nur sicherstellen, dass die DSC-Ressource, die Sie später verwenden, Zugriff hat und das Setup sowie die Passphrase abrufen kann.

## <a name="create-the-dsc-configuration"></a>Erstellen der DSC-Konfiguration

Für das Setup ist WMF 5.0 erforderlich. Damit der Computer die Konfiguration erfolgreich über Automation DSC anwenden kann, muss WMF 5.0 installiert sein.

Die folgende DSC-Beispielkonfiguration wird in der Umgebung verwendet:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Die Konfiguration führt folgende Schritte aus:

* Die Variablen weisen die Konfiguration an, von wo die Binärdateien für den Mobility Service und den Azure-VM-Agent sowie die Passphrase abgerufen werden müssen und wo die Ausgabe gespeichert werden soll.
* Die Konfiguration importiert die DSC-Ressource „xPSDesiredStateConfiguration“, sodass Sie `xRemoteFile` zum Herunterladen der Dateien aus dem Repository verwenden können.
* Die Konfiguration erstellt ein Verzeichnis zum Speichern der Binärdateien.
* Die Ressource „Archive“ extrahiert die Dateien aus dem ZIP-Ordner.
* Die Ressource „Package Install“ installiert den Mobility Service über das Installationsprogramm UNIFIEDAGENT.EXE mit bestimmten Argumenten. (Die Variablen, aus denen die Argumente bestehen, müssen für Ihre Umgebung entsprechend geändert werden.)
* Die Ressource „Package AzureAgent“ installiert den Azure-VM-Agent, der auf jedem virtuellen Computer vorhanden sein sollte, der in Azure ausgeführt wird. Der Azure-VM-Agent ermöglicht außerdem das Hinzufügen von Erweiterungen zum virtuellen Computer nach einem Failover.
* Die Ressourcen „Service“ stellen sicher, dass die zugehörigen Mobility Services und Azure-Dienste immer ausgeführt werden.

Speichern Sie die Konfiguration als **ASRMobilityService**.

> [!NOTE]
> Denken Sie daran, die CSIP in Ihrer Konfiguration durch die IP-Adresse des tatsächlichen Verwaltungsservers zu ersetzen, damit der Agent unter Verwendung der richtigen Passphrase korrekt verbunden wird.
>
>

## <a name="upload-to-automation-dsc"></a>Hochladen in Automation DSC
Da die erstellte DSC-Konfiguration ein erforderliches DSC-Ressourcenmodul (xPSDesiredStateConfiguration) importiert, müssen Sie dieses Modul vor dem Hochladen der DSC-Konfiguration in Automation importieren.

1. Melden Sie sich bei Ihrem Automation-Konto an, navigieren Sie zu **Ressourcen** > **Module**, und klicken Sie auf **Katalog durchsuchen**.
2. Suchen Sie nach dem Modul, und importieren Sie es in Ihr Konto.
3. Importieren Sie anschließend auf dem Computer, auf dem die Azure Resource Manager-Module installiert sind, die neu erstellte DSC-Konfiguration.

## <a name="import-cmdlets"></a>Import-Cmdlets

1. Melden Sie sich in PowerShell bei Ihrem Azure-Abonnement an.
2. Ändern Sie die Cmdlets entsprechend Ihrer Umgebung, und erfassen Sie die Informationen für Ihr Automation-Konto in einer Variablen:

    ```powershell
    $AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
    ```

3. Laden Sie die Konfiguration mit dem folgenden Cmdlet in Automation DSC hoch:

    ```powershell
    $ImportArgs = @{
        SourcePath = 'C:\ASR\ASRMobilityService.ps1'
        Published = $true
        Description = 'DSC Config for Mobility Service'
    }
    $AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
    ```

## <a name="compile-the-configuration-in-automation-dsc"></a>Kompilieren der Konfiguration in Automation DSC

1. Kompilieren Sie die Konfiguration in Automation DSC, damit Sie mit dem Registrieren von Knoten beginnen können. Dazu führen Sie das folgende Cmdlet aus:

    ```    powershell
    $AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
    ```

Dies kann einige Minuten dauern, da Sie im Grunde die Konfiguration im gehosteten DSC-Pulldienst bereitstellen.

2. Nach dem Kompilieren der Konfiguration können Sie die Auftragsinformationen entweder mithilfe von PowerShell (Get-AzureRmAutomationDscCompilationJob) oder mit dem [Azure-Portal](https://portal.azure.com/)abrufen.

    ![Auftrag abrufen](./media/vmware-azure-mobility-deploy-automation-dsc/retrieve-job.png)

Damit haben Sie die DSC-Konfiguration erfolgreich in Automation DSC veröffentlicht und hochgeladen.

## <a name="onboard-machines-to-automation-dsc"></a>Integrieren von Computern in Automation DSC


1. Stellen Sie sicher, dass Ihre Windows-Computer mit der neuesten Version von WMF aktualisiert wurden. Sie können die richtige Version für Ihre Plattform aus dem [Download Center](https://www.microsoft.com/download/details.aspx?id=50395)herunterladen und installieren.
2. Erstellen Sie eine Metakonfiguration für DSC, die Sie auf die Knoten anwenden. Dazu müssen Sie die Endpunkt-URL und den Primärschlüssel für das ausgewählte Automation-Konto in Azure abrufen. Diese Werte finden Sie im Automation-Konto auf dem Blatt **Alle Einstellungen** unter **Schlüssel**.

    ![Schlüsselwerte](./media/vmware-azure-mobility-deploy-automation-dsc/key-values.png)

In diesem Beispiel verwenden Sie einen physischen Windows Server 2012 R2-Server, der mit Site Recovery geschützt werden soll.

## <a name="check-for-any-pending-file-rename-operations"></a>Überprüfen auf ausstehende Dateiumbenennungsvorgänge

Bevor Sie den Server dem Automation DSC-Endpunkt zuordnen, empfiehlt es sich, in der Registrierung zu überprüfen, ob ausstehende Dateiumbenennungsvorgänge vorliegen. Sie können verhindern, dass das Setup aufgrund eines ausstehenden Neustarts beendet wird.

1. Führen Sie das folgende Cmdlet aus, um sicherzustellen, dass auf dem Server kein Neustart aussteht:

    ```powershell
    Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
    ```
2. Wenn die Ausgabe leer ist, können Sie fortfahren. Andernfalls sollten Sie den Server während eines Wartungsfensters neu starten.
3. Starten Sie PowerShell Integrated Scripting Environment (ISE), und führen Sie das folgende Skript aus, um die Konfiguration auf dem Server anzuwenden. Dies ist im Grunde eine lokale DSC-Konfiguration, die die lokale Konfigurations-Manager-Engine anweist, die Registrierung beim Automation DSC-Dienst vorzunehmen und die spezifische Konfiguration (ASRMobilityService.localhost) abzurufen.

    ```powershell
    [DSCLocalConfigurationManager()]
    configuration metaconfig {
        param (
            $URL,
            $Key
        )
        node localhost {
            Settings {
                RefreshFrequencyMins = '30'
                RebootNodeIfNeeded = $true
                RefreshMode = 'PULL'
                ActionAfterReboot = 'ContinueConfiguration'
                ConfigurationMode = 'ApplyAndMonitor'
                AllowModuleOverwrite = $true
            }

            ResourceRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }

            ConfigurationRepositoryWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
                ConfigurationNames = 'ASRMobilityService.localhost'
            }

            ReportServerWeb AzureAutomationDSC {
                ServerURL = $URL
                RegistrationKey = $Key
            }
        }
    }
    metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'
    
    Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
    ```

Diese Konfiguration führt dazu, dass sich die lokale Konfigurations-Manager-Engine selbst bei Automation DSC registriert. Sie bestimmt auch, wie die Engine ausgeführt werden soll, was im Fall einer Konfigurationsabweichung geschehen soll (ApplyAndAutoCorrect) und wie die Konfiguration fortgesetzt werden soll, falls ein Neustart erforderlich ist.

1. Nachdem Sie dieses Skript ausgeführt haben, sollte der Knoten die Registrierung bei Automation DSC starten.

    ![Knotenregistrierung wird ausgeführt](./media/vmware-azure-mobility-deploy-automation-dsc/register-node.png)

2. Wenn Sie zum Azure-Portal zurückkehren, sehen Sie, dass der neu registrierte Knoten jetzt im Portal angezeigt wird.

    ![Registrierter Knoten im Portal](./media/vmware-azure-mobility-deploy-automation-dsc/registered-node.png)

3. Auf dem Server können Sie das folgende PowerShell-Cmdlet ausführen, um sicherzustellen, dass der Knoten korrekt registriert wurde:

    ```powershell
    Get-DscLocalConfigurationManager
    ```

4. Sobald die Konfiguration mithilfe von Pull übertragen und auf den Server angewendet wurde, können Sie den Konfigurationsstatus mit dem folgenden Cmdlet überprüfen:

    ```powershell
    Get-DscConfigurationStatus
    ```

Die Ausgabe zeigt, dass der Server seine Konfiguration erfolgreich abgerufen hat:

![Output](./media/vmware-azure-mobility-deploy-automation-dsc/successful-config.png)

Darüber hinaus verfügt das Mobility Service-Setup über ein eigenes Protokoll, das unter „*Systemlaufwerk*\ProgramData\ASRSetupLogs“ gespeichert wird.

Das ist alles. Sie haben nun erfolgreich den Mobility Service auf dem Computer bereitgestellt und registriert, den Sie mit Site Recovery schützen möchten. DSC stellt sicher, dass die erforderlichen Dienste immer ausgeführt werden.

![Erfolgreiche Bereitstellung](./media/vmware-azure-mobility-deploy-automation-dsc/successful-install.png)

Sobald die erfolgreiche Bereitstellung vom Verwaltungsserver erkannt wurde, können Sie mit Site Recovery auf dem Computer den Schutz konfigurieren und die Replikation aktivieren.

## <a name="use-dsc-in-disconnected-environments"></a>Verwenden von DSC in getrennten Umgebungen

Auch wenn Ihre Computer nicht mit dem Internet verbunden sind, können Sie DSC verwenden, um den Mobility Service für zu schützende Workloads bereitzustellen und zu konfigurieren.

Sie können Ihren eigenen DSC-Pullserver in Ihrer Umgebung instanziieren, um im Wesentlichen die gleichen Funktionen bereitzustellen, die Automation DSC bietet. Die Clients übertragen also die Konfiguration (nach der Registrierung) mithilfe von Pull an den DSC-Endpunkt. Eine weitere Option ist jedoch, die DSC-Konfiguration manuell mithilfe von Push auf Ihre Computer (lokal oder remote) zu übertragen.

Beachten Sie, dass in diesem Beispiel ein Parameter für den Computernamen hinzugefügt wurde. Die Remotedateien befinden sich jetzt auf einer Remotefreigabe, auf die die zu schützenden Computer Zugriff haben müssen. Am Ende des Skripts startet das Skript die Konfiguration und beginnt damit, die DSC-Konfiguration auf den Zielcomputer anzuwenden.

### <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass das PowerShell-Modul „xPSDesiredStateConfiguration“ installiert ist. Für Windows-Computer, auf denen WMF 5.0 installiert ist, können Sie das Modul „xPSDesiredStateConfiguration“ installieren, indem Sie das folgende Cmdlet auf den Zielcomputern ausführen:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Sie können das Modul auch herunterladen und speichern, falls Sie es an Windows-Computer verteilen müssen, die WMF 4.0 verwenden. Führen Sie dieses Cmdlet auf einem Computer aus, auf dem PowerShellGet (WMF 5.0) vorhanden ist:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Stellen Sie für WMF 4.0 zudem sicher, dass das [Windows 8.1-Update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) auf den Computern installiert ist.

Die folgende Konfiguration kann mithilfe von Push auf Windows-Computer mit WMF 5.0 und WMF 4.0 übertragen werden:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Informationen zum Instanziieren eines eigenen DSC-Pullservers innerhalb des Unternehmensnetzwerks, der die gleichen Funktionen wie Automation DSC bietet, finden Sie unter [Einrichten eines DSC-Webpullservers](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optional: Bereitstellen der DSC-Konfiguration mithilfe einer Azure Resource Manager-Vorlage
In diesem Artikel lag der Fokus bisher darauf, wie Sie Ihre eigene DSC-Konfiguration erstellen, um den Mobility Service und den Azure-VM-Agent automatisch bereitzustellen und sicherzustellen, dass sie auf den zu schützenden Computern ausgeführt werden. Es gibt auch eine Azure Resource Manager-Vorlage, die diese DSC-Konfiguration in einem neuen oder vorhandenen Azure Automation-Konto bereitstellt. Die Vorlage verwendet Eingabeparameter um Automation-Ressourcen zu erstellen, die Variablen für Ihre Umgebung enthalten.

Nach der Bereitstellung der Vorlage können Sie einfach mit Schritt 4 dieses Leitfadens fortfahren, um Ihre Computer zu integrieren.

Die Vorlage führt folgende Schritte aus:

1. Verwenden eines vorhandenen Automation-Kontos oder Erstellen eines neuen Kontos
2. Akzeptieren der Eingabeparameter für Folgendes:
   * ASRRemoteFile: Speicherort des Mobility Service-Setups
   * ASRPassphrase: Speicherort der Datei „passphrase.txt“
   * ASRCSEndpoint: IP-Adresse des Verwaltungsservers
3. Importieren des PowerShell-Moduls „xPSDesiredStateConfiguration“
4. Erstellen und Kompilieren der DSC-Konfiguration

Die obigen Schritte werden in der richtigen Reihenfolge ausgeführt, sodass Sie mit dem Integrieren Ihrer zu schützenden Computer beginnen können.

Die Vorlage mit Anleitungen für die Bereitstellung finden Sie auf [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Stellen Sie die Vorlage mithilfe von PowerShell bereit:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Mobility Service-Agents bereitgestellt haben, können Sie die [Replikation](vmware-azure-tutorial.md) für die virtuellen Computer aktivieren.

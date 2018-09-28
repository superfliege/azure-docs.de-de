---
title: Onboarding von Azure Monitor for VMs | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie das Onboarding und die Konfiguration von Azure Monitor for VMs durchführen, damit Sie erfahren können, wie gut die Leistung Ihrer verteilten Anwendung ist und welche Integritätsprobleme erkannt wurden.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: e5421ca791ae9d0059639000f33b77be57f4d891
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968032"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms"></a>Onboardingmethoden für den Azure Monitor for VMs 
In diesem Artikel wird beschrieben, wie Sie Azure Monitor for VMs für die Überwachung der Betriebssystemintegrität Ihrer Azure-VMs und für die Erkennung und Zuordnung von Anwendungsabhängigkeiten der auf ihnen ggf. gehosteten Anwendungen einrichten.  

Das Aktivieren von Azure Monitor for VMs erfolgt mithilfe einer der im Folgenden vorgestellten Methoden; Details zur Verwendung der einzelnen Methoden finden Sie weiter unten im Artikel.  

* Für eine einzelne Azure-VM durch Auswählen von **Insights (Vorschau)** direkt in der VM.
* Für mehrere Azure-VMs mithilfe von Azure Policy, um sicherzustellen, dass auf vorhandenen und neuen bewerteten VMs die erforderlichen Abhängigkeiten installiert und ordnungsgemäß konfiguriert sind.  Nicht konforme VMs werden gemeldet, so dass Sie je nachdem, wo die Konformität nicht eingehalten wird, entscheiden können, wie Sie den Zustand korrigieren möchten.  
* Für mehrere Azure-VMs oder VM-Skalierungsgruppen innerhalb eines angegebenen Abonnements oder einer Ressourcengruppe mithilfe von PowerShell.

## <a name="prerequisites"></a>Voraussetzungen
Achten Sie vor Beginn darauf, dass die in den folgenden Unterabschnitten beschriebenen Voraussetzungen erfüllt sind.

### <a name="log-analytics"></a>Log Analytics 

Ein Log Analytics-Arbeitsbereich wird aktuell in den folgenden Regionen unterstützt:

  - USA, Westen-Mitte  
  - USA (Ost)  
  - Europa, Westen  
  - Asien, Südosten<sup>1</sup>  

<sup>1</sup> Diese Region unterstützt derzeit das Integritätsfeature von Azure Monitor for VMs nicht.   

Wenn Sie nicht über einen Arbeitsbereich verfügen, können Sie ihn über den [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../log-analytics/log-analytics-quick-create-workspace.md) erstellen.  

Um die Lösung zu aktivieren, müssen Sie der Rolle „Log Analytics-Mitwirkender“ angehören. Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgenden Versionen der Betriebssysteme Windows und Linux werden in Kombination mit Azure Monitor for VMs offiziell unterstützt:

|Betriebssystemversion |Leistung |Karten |Health |  
|-----------|------------|-----|-------|  
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X| X |  
|Cent OS Linux 7, 6 | X | X| X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> Das Leistungsfeature von Azure Monitor for VMs steht nur in Azure Monitor zur Verfügung, nicht aber beim direkten Zugriff über den linken Bereich der Azure-VM.  

### <a name="hybrid-environment-connected-sources"></a>Verbundene Quellen in Hybridumgebungen
Das Zuordnungsfeature in Azure Monitor for VMs erhält seine Daten vom Microsoft Dependency-Agent. Der Dependency-Agent baut auf dem Log Analytics-Agent auf und ist auf dessen Verbindung mit Log Analytics angewiesen. Dies bedeutet, dass auf einem System der Log Analytics-Agent mit dem Dependency-Agent installiert und konfiguriert sein muss.  In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die vom Zuordnungsfeature in einer Hybridumgebung unterstützt werden.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
|:--|:--|:--|
| Windows-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Windows](../log-analytics/log-analytics-concept-hybrid.md) erfordern Windows-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| Linux-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Linux](../log-analytics/log-analytics-concept-hybrid.md) erfordern Linux-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| System Center Operations Manager-Verwaltungsgruppe | Nein  | |  

Unter Windows wird der Microsoft Monitoring Agent (MMA) von System Center Operations Manager und Log Analytics zum Erfassen und Senden von Überwachungsdaten verwendet. System Center Operations Manager und Log Analytics bieten unterschiedliche vorkonfigurierte Versionen des Agents. Jede dieser Versionen kann Berichte an System Center Operations Manager, Log Analytics oder beide senden.  

Unter Linux erfasst der Log Analytics-Agent für Linux Überwachungsdaten und sendet sie an Log Analytics.   

Wenn Ihre Windows- oder Linux-Computer nicht direkt mit dem Dienst verbunden werden können, müssen Sie den Log Analytics-Agent so konfigurieren, dass er die Verbindung mit Log Analytics über das OMS-Gateway herstellt. Weitere Informationen zum Bereitstellen und Konfigurieren des OMS-Gateways finden Sie unter [Verbinden von Computern ohne Internetzugriff über das OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).  

Der Dependency-Agent kann von den folgenden Speicherorten heruntergeladen werden.

| Datei | Betriebssystem | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.5.0 | 8B8FE0F6B0A9F589C4B7B52945C2C25DF008058EB4D4866DC45EE2485062C9D7 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.5.1 | 09D56EF43703A350FF586B774900E1F48E72FE3671144B5C99BB1A494C201E9E |

## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Azure Monitor-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Diensts sicherzustellen und zu verbessern. Zu den Daten vom Zuordnungsfeature gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Microsoft erfasst weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="performance-counters-enabled"></a>Aktivierte Leistungsindikatoren
Azure Monitor for VMs konfiguriert einen Log Analytics-Arbeitsbereich, um die von der Lösung verwendeten Leistungsindikatoren zu sammeln.  In der folgenden Tabelle sind die von der Lösung konfigurierten Objekte und Indikatoren aufgelistet, die alle 60 Sekunden erfasst werden.

### <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren

|Objektname |Name des Leistungsindikators |  
|------------|-------------|  
|Logischer Datenträger |% freier Speicher |  
|Logischer Datenträger |Durchschn. Datenträger s/gelesen |  
|Logischer Datenträger |Durchschn. Datenträger s/übertragen |  
|Logischer Datenträger |Durchschn. Datenträger s/geschrieben |  
|Logischer Datenträger |Datenträger Bytes/s |  
|Logischer Datenträger |Byte gelesen/s  |  
|Logischer Datenträger |Lesevorgänge/s  |  
|Logischer Datenträger |Übertragungen/s |  
|Logischer Datenträger |Byte geschrieben/s |  
|Logischer Datenträger |Schreibvorgänge/s |  
|Logischer Datenträger |Freie Megabytes |  
|Arbeitsspeicher |Verfügbare MB |  
|Netzwerkadapter |Empfangene Byte/Sek. |  
|Netzwerkadapter |Gesendete Byte/Sek. |  
|Prozessor |% Prozessorzeit |  

### <a name="linux-performance-counters"></a>Leistungsindikatoren von Linux

|Objektname |Name des Leistungsindikators |  
|------------|-------------|  
|Logischer Datenträger |% verwendeter Speicher |  
|Logischer Datenträger |Byte gelesen/s  |  
|Logischer Datenträger |Lesevorgänge/s  |  
|Logischer Datenträger |Übertragungen/s |  
|Logischer Datenträger |Byte geschrieben/s |  
|Logischer Datenträger |Schreibvorgänge/s |  
|Logischer Datenträger |Freie Megabytes |  
|Logischer Datenträger |Logischer Datenträger Bytes/s |  
|Arbeitsspeicher |Verfügbarer Speicher in MB |  
|Netzwerk |Summe empfangene Bytes |  
|Netzwerk |Summe übertragene Bytes |  
|Prozessor |% Prozessorzeit |  

## <a name="enable-from-the-azure-portal"></a>Aktivieren im Azure-Portal
Gehen Sie wie folgt vor, um die Überwachung Ihrer Azure-VM im Azure-Portal zu aktivieren:

1. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
2. Wählen Sie einen virtuellen Computer in der Liste aus. 
3. Wählen Sie auf der Seite der VM im Abschnitt **Überwachung** den Eintrag **Insights (Vorschau)** aus.
4. Wählen Sie auf der Seite **Insights (Vorschau)** **Jetzt testen** aus.

    ![Aktivieren von Azure Monitor for VMs für eine VM](./media/monitoring-vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Wenn Sie im selben Abonnement einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Azure Monitor Insights Onboarding** (Onboarding von Azure Monitor Insights) in der Dropdownliste aus.  Mit der Liste werden der Standardarbeitsbereich und der Speicherort, an dem die VM im Abonnement bereitgestellt wird, vorausgewählt. 

    >[!NOTE]
    >Wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus der VM erstellen möchten, befolgen Sie die Anweisungen in [Erstellen eines Log Analytics-Arbeitsbereichs](../log-analytics/log-analytics-quick-create-workspace.md). Achten Sie darauf, dass Sie den Arbeitsbereich in demselben Abonnement erstellen, in dem die VM bereitgestellt wird. 

Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätsmetriken für die VM angezeigt werden. 

![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](./media/monitoring-vminsights-onboard/onboard-vminsights-vm-portal-status.png)

## <a name="enable-using-azure-policy"></a>Aktivieren mithilfe von Azure Policy
Um die Lösung für mehrere Azure-VMs in einer Weise bereitzustellen, die durchgehende Compliance und automatische Aktivierung für neu bereitgestellte VMs sicherstellt, wird die Verwendung von [Azure Policy](../azure-policy/azure-policy-introduction.md) empfohlen.  Die Verwendung von Azure Policy mit den bereitgestellten Richtlinien bietet die folgenden Vorzüge für neue VMs:

* Azure Monitor for VMs wird für jede VM im definierten Bereich aktiviert
* Der Log Analytics-Agent wird bereitgestellt 
* Der Dependency-Agent wird bereitgestellt, um Anwendungsabhängigkeiten zu erkennen und in der Zuordnung darzustellen
* Es wird überwacht, ob das Betriebssystemimage Ihrer Azure-VM sich in der Richtliniendefinition in einer vordefinierten Liste befindet  
* Es wird überwacht, ob Ihre Azure-VM in einen anderen als den angegebenen Arbeitsbereich protokolliert
* Complianceergebnisse werden gemeldet 
* Für nicht konforme VMs wird Korrektur unterstützt

Die Aktivierung für Ihren Mandanten setzt Folgendes voraus:

- Konfigurieren eines Log Analytics-Arbeitsbereichs anhand der hier aufgeführten Schritte
- Importieren der Initiativendefinition in Ihren Mandanten (auf der Ebene der Verwaltungsgruppe oder des Abonnements)
- Zuweisen der Richtlinie zum gewünschten Bereich
- Überprüfen der Complianceergebnisse

### <a name="add-the-policies-and-initiative-to-your-subscription"></a>Hinzufügen von Richtlinie und Initiative zu Ihrem Abonnement
Um die Richtlinien zu verwenden, können Sie ein bereitgestelltes PowerShell-Skript – [Add-VMInsightsPolicy.ps1](https://www.powershellgallery.com/packages/Add-VMInsightsPolicy/1.2) – nutzen, das im Azure PowerShell-Katalog verfügbar ist. Das Skript fügt Ihrem Abonnement die Richtlinien und eine Initiative hinzu.  Führen Sie die folgenden Schritte aus, um Azure Policy in Ihrem Abonnement zu konfigurieren. 

1. Laden Sie das PowerShell-Skript in Ihr lokales Dateisystem herunter.

2. Verwenden Sie den folgenden PowerShell-Befehl im Ordner, um Richtlinien hinzuzufügen. Das Skript unterstützt die folgenden optionalen Parameter: 

    ```powershell
    -UseLocalPolicies [<SwitchParameter>]
      <Optional> Load the policies from a local folder instead of https://raw.githubusercontent.com/dougbrad/OnBoardVMInsights/Policy/Policy/

    -SubscriptionId <String>
      <Optional> SubscriptionId to add the Policies/Initiatives to
    -ManagementGroupId <String>
      <Optional> Management Group Id to add the Policies/Initiatives to

    -Approve [<SwitchParameter>]
      <Optional> Gives the approval to add the Policies/Initiatives without any prompt
    ```  

    >[!NOTE]
    >Hinweis: Wenn Sie beabsichtigen, die Initiative/Richtlinie mehreren Abonnements hinzuzufügen, müssen die Definitionen in der Verwaltungsgruppe gespeichert sein, die die Abonnements enthält, denen Sie die Richtlinie zuweisen möchten. Daher müssen Sie den Parameter „-ManagementGroupID“ verwenden.
    >
   
    Beispiel ohne Parameter: `.\Add-VMInsightsPolicy.ps1`

### <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung
Nach dem Ausführen des `Add-VMInsightsPolicy.ps1`-PowerShell-Skripts wurden die folgende Initiative und die folgenden Richtlinien hinzugefügt:

* **Bereitstellen von Log Analytics-Agent für Windows-VMs – Vorschau**
* **Bereitstellen von Log Analytics-Agent für Linux-VMs – Vorschau**
* **Bereitstellen von Dependency-Agent für Windows-VMs – Vorschau**
* **Bereitstellen von Dependency-Agent für Linux-VMs – Vorschau**
* **Überwachen der Log Analytics-Agent-Bereitstellung – nicht gelistetes VM-Betriebssystemimage – Vorschau**
* **Überwachen der Dependency-Agent-Bereitstellung – nicht gelistetes VM-Betriebssystemimage – Vorschau**

Der folgende Initiativenparameter wurde hinzugefügt:

- **Log Analytics-Arbeitsbereich** (Sie müssen die Ressourcen-ID des Arbeitsbereichs angeben, wenn Sie die Zuordnung mithilfe von PowerShell oder CLI vornehmen)

    Für VMs, die von der Überwachungsrichtlinie **VMs außerhalb des Betriebssystembereichs...** für nicht konform befunden wurden, beinhalten die Kriterien der Bereitstellungsrichtlinie nur VMs, die aus bekannten Azure-VM-Images bereitgestellt werden. Überprüfen Sie anhand der Dokumentation, ob das Betriebssystem der VM unterstützt wird oder nicht.  Ist das nicht der Fall, müssen Sie die Bereitstellungsrichtlinie duplizieren und sie aktualisieren/ändern, um das Image in den Bereich einzuschließen.

Die folgende eigenständige optionale Richtlinie wird hinzugefügt:

- **Die VM ist für einen nicht übereinstimmenden Log Analytics-Arbeitsbereich konfiguriert – Vorschau**

    Sie kann dazu verwendet werden, um VMs zu erkennen, die bereits mit der [Log Analytics-VM-Erweiterung](../virtual-machines/extensions/oms-windows.md), jedoch mit einem anderen als dem beabsichtigten Arbeitsbereich (wie in der Richtlinienzuweisung angegeben) konfiguriert wurden. Die Richtlinie nimmt einen Parameter für die WorkspaceID an.

In dieser ursprünglichen Version können Sie die Richtlinienzuweisung nur im Azure-Portal erstellen. Informationen zum Ausführen dieser Schritte finden Sie unter [Erstellen einer Richtlinienzuweisung im Azure-Portal](../azure-policy/assign-policy-definition.md).

## <a name="enable-with-powershell"></a>Aktivieren mithilfe von PowerShell
Um Azure Monitor for VMs für mehrere VMs oder VM-Skalierungsgruppen zu aktivieren, können Sie ein bereitgestelltes PowerShell-Skript – [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) – verwenden, das im Azure PowerShell-Katalog verfügbar ist.  Dieses Skript durchläuft alle virtuellen Computer und VM-Skalierungsgruppen in Ihrem Abonnement in der bereichsbezogenen Ressourcengruppe gemäß der Angabe in *ResourceGroup* oder eine einzelne VM oder Skalierungsgruppe, die in *Name* angegeben ist.  Für jede VM oder VM-Skalierungsgruppe überprüft das Skript, ob die VM-Erweiterung bereits installiert ist, und versucht, sie zu installieren, wenn das nicht der Fall ist.  Andernfalls fährt sie mit der Installation der Log Analytics- und Dependency-Agent-VM-Erweiterungen fort.   

Das Skript benötigt mindestens die Azure PowerShell-Modulversion 5.7.0. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Wenn Sie Hilfe zum Skript benötigen, können Sie durch Ausführen von `Get-Help` eine Liste mit Argumentdetails und Syntaxbeispielen anzeigen.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

Das folgende Beispiel veranschaulicht die Verwendung der PowerShell-Befehle im Ordner, um Azure Monitor for VMs zu aktivieren und die erwartete Ausgabe zu verstehen:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>Aktivieren für Hybridumgebungen
In diesem Abschnitt wird erläutert, wie das Onboarding von VMs oder physischen Computern, die in Ihrem Rechenzentrum oder einer Cloudumgebung gehostet sind, für die Überwachung mithilfe von Azure Monitor for VMs erfolgt.  

Der Dependency-Agent von Azure Monitor for VMs Map selbst überträgt keine Daten und erfordert keine Änderungen an Firewalls oder Ports. Die Daten in Map werden immer vom Log Analytics-Agent an den Azure Monitor-Dienst übertragen, entweder direkt oder über das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md), falls Ihre Richtlinien zur IT-Sicherheit Computern im Netzwerk keine Internetverbindungen erlauben.

Überprüfen Sie die Anforderungen und Bereitstellungsmethoden für den [Log Analytics-Linux- und -Windows-Agent](../log-analytics/log-analytics-concept-hybrid.md).

Zusammenfassung der Schritte:

1. Installieren Sie den Log Analytics-Agent für Windows oder Linux
2. Installieren Sie den Dependency-Agent von Azure Monitor for VMs Map
3. Aktivieren Sie die Erfassung von Leistungsindikatoren
4. Führen Sie das Onboarding von Azure Monitor for VMs durch

### <a name="install-the-dependency-agent-on-windows"></a>Installieren des Dependency-Agents unter Windows 
Der Dependency-Agent kann durch Ausführen von `InstallDependencyAgent-Windows.exe` manuell auf Windows-Computern installiert werden. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Setup-Assistent gestartet, mit dem Sie die Installation interaktiv durchführen können.  

>[!NOTE]
>Zum Installieren oder Deinstallieren des Agent sind Administratorrechte erforderlich.

Die folgende Tabelle hebt die spezifischen Parameter hervor, die von Setup für den Agent auf der Befehlszeile unterstützt werden.  

| Parameter | BESCHREIBUNG |
|:--|:--|
| /? | Gibt eine Liste der Befehlszeilenoptionen zurück. |
| /S | Führt eine automatische Installation ohne Benutzereingriff aus. |

Um das Installationsprogramm beispielsweise mit dem Parameter `/?` auszuführen, geben Sie `InstallDependencyAgent-Windows.exe /?` ein.

Dateien für den Windows Dependency-Agent werden standardmäßig in `C:\Program Files\Microsoft Dependency Agent` installiert.  Falls der Dependency-Agent nach dem Abschluss von Setup nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>Installieren des Dependency-Agents unter Linux
Der Dependency-Agent wird auf Linux-Servern mit `InstallDependencyAgent-Linux64.bin` installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit `sh` ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

>[!NOTE]
> Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.
> 

| Parameter | BESCHREIBUNG |
|:--|:--|
| -help | Ruft eine Liste der Befehlszeilenoptionen ab. |
| -s | Führt eine automatische Installation ohne Benutzereingaben aus. |
| --check | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Um das Installationsprogramm beispielsweise mit dem Parameter `-help` auszuführen, geben Sie `InstallDependencyAgent-Linux64.bin -help` ein.

Installieren Sie den Dependency-Agent für Linux mit Root-Berechtigungen, indem Sie den folgenden Befehl ausführen: `sh InstallDependencyAgent-Linux64.bin`.
    
Falls der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis `/var/opt/microsoft/dependency-agent/log`.

Dateien für den Dependency-Agent werden in den folgenden Verzeichnissen gespeichert:

| Dateien | Standort |
|:--|:--|
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren
Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Dies kann manuell erfolgen, wie [hier](../log-analytics/log-analytics-data-sources-performance-counters.md) beschrieben, oder durch Herunterladen und Ausführen eines PowerShell-Skripts, das im [Azure Powershell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.
 
### <a name="onboard-azure-monitor-for-vms"></a>Führen Sie das Onboarding von Azure Monitor for VMs durch
Diese Methode beinhaltet eine JSON-Vorlage, die die Konfiguration angibt, um die Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich zu aktivieren.  

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) 

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Erstellen und Ausführen von Vorlagen

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Speichern Sie diese Datei als **installsolutionsforvminsights.json** in einem lokalen Ordner.
3. Bearbeiten Sie die Werte für **WorkspaceName**, **ResourceGroupName** und **WorkspaceLocation**.  Der Wert für **WorkspaceName** ist die vollständige Ressourcen-ID des Log Analytics-Arbeitsbereichs, die den Namen des Arbeitsbereichs beinhaltet, und der Wert für **WorkspaceLocation** ist die Region, in der der Arbeitsbereich definiert ist.
4. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```
Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätsstatus und Metriken für den Hybridcomputer angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

Bei aktivierter Überwachung für Ihre VM stehen diese Informationen für die Analyse mit Azure Monitor for VMs zur Verfügung.  Informationen zur Verwendung des Integritätsfeatures finden Sie unter [View Azure Monitor for VMs Health](monitoring-vminsights-health.md) (Azure Monitor for VMs – Integrität anzeigen); Informationen zum Anzeigen entdeckter Anwendungsabhängigkeiten finden Sie unter [View Azure Monitor for VMs Map](monitoring-vminsights-maps.md) (Azure Monitor for VMs – Zuordnung anzeigen).  
---
title: Grundlegende Informationen zu den Agent-Überprüfungsergebnissen in der Azure-Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit dem Updateverwaltungs-Agent beheben können.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/10/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 1b258d115e7d9962ecab4b93dbcd98d13f5977c7
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956679"
---
# <a name="understand-the-agent-check-results-in-update-management"></a>Grundlegende Informationen zu den Agent-Überprüfungsergebnissen in der Updateverwaltung

Es kann zahlreiche Gründe dafür geben, dass Ihr Nicht-Azure-Computer in der Updateverwaltung nicht als **Bereit** angezeigt wird. In der Updateverwaltung können Sie die Integrität eines Hybrid Worker-Agents überprüfen, um das zugrunde liegende Problem zu ermitteln. Dieser Artikel erläutert, wie Sie die Problembehandlung im Azure-Portal und in Offlineszenarien ausführen.

## <a name="start-the-troubleshooter"></a>Starten der Problembehandlung

Indem Sie im Portal unter der Spalte **Update-Agent-Bereitschaft** auf den Link **Problembehandlung** klicken, starten Sie die Seite **Problembehandlung von Update-Agent**. Diese Seite zeigt Probleme mit dem Agent und einen Link zu einem Artikel, der Sie bei der Behandlung dieser Probleme unterstützt.

![Seite „VM-Liste“](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Für die Überprüfungen muss die VM ausgeführt werden. Wenn die VM nicht ausgeführt wird, wird eine Schaltfläche angezeigt, mit der Sie die **VM starten** können.

Klicken Sie auf der Seite **Problembehandlung von Update-Agent** auf **Überprüfungen ausführen**. Die Problembehandlung verwendet [Befehl ausführen](../../virtual-machines/windows/run-command.md), um ein Skript auf einem Computer auszuführen, mit dem die Abhängigkeiten überprüft werden, die der Agent aufweist. Wenn die Problembehandlung abgeschlossen ist, werden die Ergebnisse der Überprüfungen zurückgegeben.

![Seite „Problembehandlung“](../media/update-agent-issues/troubleshoot-page.png)

Nach Abschluss werden die Ergebnisse im Fenster zurückgegeben. Die verschiedenen [Überprüfungsabschnitte](#pre-requisistes-checks) liefern Informationen zu den Aspekten, die jede Überprüfung untersucht.

![Seite mit Überprüfungen des Update-Agents](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Voraussetzungsprüfungen

### <a name="operating-system"></a>Betriebssystem

Die Betriebssystemprüfung untersucht, ob der Hybrid Runbook Worker unter einem der folgenden Betriebssysteme ausgeführt wird:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Unterstützt nur Updatebewertungen.         |
|Windows Server 2008 R2 SP1 und höher     |.NET Framework 4.5 oder höher ist erforderlich. ([.NET Framework herunterladen](/dotnet/framework/install/guide-for-developers))<br/> WindowsPowerShell 4.0 oder höher ist erforderlich. ([WMF 4.0 herunterladen](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Für eine höhere Zuverlässigkeit wird Windows PowerShell 5.1 empfohlen.  ([WMF 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen. Für klassifizierungsbasiertes Patchen muss „yum“ Sicherheitsdaten zurückgeben, über die CentOS nicht standardmäßig verfügt.         |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 14.04 LTS und 16.04 LTS (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

### <a name="net-45"></a>.NET 4.5

Bei der .NET Framework-Überprüfung wird untersucht, ob auf dem System mindestens [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) vorhanden ist.

### <a name="wmf-51"></a>WMF 5.1

Bei der WMF-Überprüfung wird untersucht, ob auf dem System die erforderliche Version des Windows Management Framework installiert ist. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) ist die niedrigste unterstützte Version. Es wird empfohlen, [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) zu installieren, um die Zuverlässigkeit des Hybrid Runbook Workers zu erhöhen.

### <a name="tls-12"></a>TLS 1.2

Bei dieser Überprüfung wird untersucht, ob Sie zum Verschlüsseln der Kommunikation TLS 1.2 verwenden. TLS 1.0 wird von der Plattform nicht mehr unterstützt, und es wird empfohlen, dass Clients TLS 1.2 für die Kommunikation mit der Updateverwaltung verwenden.

## <a name="connectivity-checks"></a>Konnektivitätsprüfungen

### <a name="registration-endpoint"></a>Registrierungsendpunkt

Bei dieser Überprüfung wird untersucht, ob der Agent ordnungsgemäß mit dem Agent-Dienst kommunizieren kann.

Die Proxy- und Firewallkonfigurationen müssen die Kommunikation des Hybrid Runbook Worker-Agents mit dem Registrierungsendpunkt zulassen. Eine Liste der Adressen und zu öffnenden Ports finden Sie unter [Netzwerkplanung für Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="operations-endpoint"></a>Endpunkt für Vorgänge

Bei dieser Überprüfung wird untersucht, ob der Agent ordnungsgemäß mit dem Auftragsruntime-Datendienst kommunizieren kann.

Die Proxy- und Firewallkonfigurationen müssen die Kommunikation des Hybrid Runbook Worker-Agents mit dem Auftragsruntime-Datendienst zulassen. Eine Liste der Adressen und zu öffnenden Ports finden Sie unter [Netzwerkplanung für Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="vm-service-health-checks"></a>Integritätsprüfungen für den VM-Dienst

### <a name="monitoring-agent-service-status"></a>Status des Monitoring Agent-Diensts

Bei dieser Überprüfung wird untersucht, ob Microsoft Monitoring Agent, `HealthService`, auf dem Computer ausgeführt wird.

Weitere Informationen zur Behandlung von Problemen mit dem Dienst finden Sie unter [Der Microsoft Monitoring Agent wird nicht ausgeführt](hybrid-runbook-worker.md#mma-not-running).

Informationen zum erneuten Installieren von Microsoft Monitoring Agent finden Sie unter [Installieren und Konfigurieren von Microsoft Monitoring Agent](/log-analytics/log-analytics-concept-hybrid.md#install-and-configure-agent).

### <a name="monitoring-agent-service-events"></a>Ereignisse des Monitoring Agent-Diensts

Bei dieser Überprüfung wird untersucht, ob in den letzten 24 Stunden `4502`-Ereignisse im Operations Manager-Protokoll auf dem Computer aufgetreten sind.

Weitere Informationen zu diesem Ereignis finden Sie im [Leitfaden zur Problembehandlung](hybrid-runbook-worker.md#event-4502) für dieses Ereignis.

## <a name="access-permissions-checks"></a>Überprüfung von Zugriffsberechtigungen

### <a name="machinekeys-folder-access"></a>Zugriff auf MachineKeys-Ordner

Bei der Überprüfung des Zugriffs auf den Ordner „Crypto“ wird untersucht, ob das lokale Dateisystem auf `C:\ProgramData\Microsoft\Crypto\RSA` zugreifen kann.

## <a name="troubleshoot-offline"></a>Offlineproblembehandlung

Sie können die Problembehandlung offline auf einem Hybrid Runbook Worker ausführen, indem Sie das Skript lokal ausführen. Sie finden das Skript  [Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) im PowerShell-Katalog. Hier sehen Sie eine Beispielausgabe dieses Skripts:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .Net Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             : 
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Behandlung weiterer Probleme mit Ihren Hybrid Runbook Workern finden Sie unter [Problembehandlung für Hybrid Runbook Worker](hybrid-runbook-worker.md).

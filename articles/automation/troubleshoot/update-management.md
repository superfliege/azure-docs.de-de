---
title: Beheben von Fehlern mit Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit Updateverwaltung beheben können.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 22e3ea1c90946902fc2a16d947ff2884e5e0a44b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274585"
---
# <a name="troubleshooting-issues-with-update-management"></a>Behandeln von Problemen mit Updateverwaltung

In diesem Artikel werden Lösungen zum Beheben von Problemen beschrieben, die bei Verwendung der Updateverwaltung auftreten können.

Es gibt eine Agent-Problembehandlung für den Hybrid Worker-Agent, mit dem das zugrunde liegende Problem ermittelt werden kann. Informationen zur Problembehandlung finden Sie unter [Beheben von Problemen mit dem Update-Agent](update-agent-issues.md). Bei allen anderen Problemen finden Sie weiter unten ausführliche Informationen zu möglichen Problemen.

## <a name="general"></a>Allgemein

### <a name="components-enabled-not-working"></a>Szenario: Die Komponenten für die Lösung „Updateverwaltung“ wurden aktiviert, und diese VM wird nun konfiguriert.

#### <a name="issue"></a>Problem

Die folgende Meldung wird 15 Minuten nach dem Onboarding auf einer VM angezeigt:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. Die Kommunikation an das Automation-Konto wird blockiert.
2. Die VM, die integriert wird, stammt möglicherweise von einem geklonten Cloudcomputer, dessen System nicht mit dem installierten Microsoft Monitoring Agent vorbereitet wurde.

#### <a name="resolution"></a>Lösung

1. Sehen Sie sich den Abschnitt [Konfigurieren des Netzwerks](../automation-hybrid-runbook-worker.md#network-planning) an, um zu erfahren, welche Adressen und Ports zugelassen werden müssen, damit die Updateverwaltung funktioniert.
2. Wenn Sie ein geklontes Image verwenden:
   1. Entfernen Sie in Ihrem Log Analytics-Arbeitsbereich den virtuellen Computer aus der gespeicherten Suche für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates`, sofern er angezeigt wird. Gespeicherte Suchen finden Sie unter **Allgemein** in Ihrem Arbeitsbereich.
   2. Ausführen `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force`
   3. Führen Sie `Restart-Service HealthService` aus, um `HealthService` neu zu starten. Der Schlüssel wird erneut erstellt, und eine neue UUID wird generiert.
   4. Wenn dies nicht funktioniert, bereiten Sie mit Sysprep das Image zunächst vor, und installieren Sie den MMA-Agent anschließend.

### <a name="multi-tenant"></a>Szenario: Sie erhalten einen Fehler aufgrund eines verknüpften Abonnements, wenn Sie eine Updatebereitstellung für Computer auf einem anderen Azure-Mandanten erstellen.

#### <a name="issue"></a>Problem

Sie erhalten beim Versuch, eine Updatebereitstellung für Computer auf einem anderen Azure-Mandanten zu erstellen, die folgende Fehlermeldung:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt bei der Erstellung einer Updatebereitstellung auf, bei der virtuelle Azure-Computer eines anderen Mandanten in eine Updatebereitstellung einbezogen werden.

#### <a name="resolution"></a>Lösung

Sie müssen die folgende Problemumgehung nutzen, um die Planung durchzuführen. Verwenden Sie das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) mit dem Schalter `-ForUpdate`, um einen Zeitplan zu erstellen, und das Cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
), um die Computer aus dem anderen Mandanten an den Parameter `-NonAzureComputer` zu übergeben. Dies wird anhand des folgenden Beispiels veranschaulicht:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="nologs"></a>Szenario: Die Daten der Updateverwaltung für einen Computer werden nicht in Azure Monitor-Protokolle angezeigt

#### <a name="issue"></a>Problem

Bei einigen Ihrer Computer wird **Nicht bewertet** unter **Compliance** angezeigt, es werden aber Heartbeatdaten in Azure Monitor-Protokolle für den Hybrid Runbook Worker, jedoch nicht für die Updateverwaltung angezeigt.

#### <a name="cause"></a>Ursache

Der Hybrid Runbook Worker muss eventuell erneut registriert und installiert werden.

#### <a name="resolution"></a>Lösung

Führen Sie die Schritte unter [Bereitstellen eines Windows Hybrid Runbook Workers](../automation-windows-hrw-install.md) aus, um den Hybrid Worker für Windows neu zu installieren (bzw. [Bereitstellen eines Linux Hybrid Runbook Workers](../automation-linux-hrw-install.md) für Linux).

## <a name="windows"></a>Windows

Sollten Sie Probleme beim Integrieren der Lösung in einen virtuellen Computers haben, suchen Sie auf dem lokalen Computer im Ereignisprotokoll **Operations Manager** unter **Anwendungs- und Dienstprotokolle** nach Ereignissen mit der Ereignis-ID **4502** und einer Ereignismeldung, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthält.

Der folgende Abschnitt enthält spezifische Fehlermeldungen und passende Lösungsvorschläge. Informationen zu anderen Integrationsproblemen finden Sie unter [Problembehandlung bei der Integration von Lösungen](onboarding.md).

### <a name="machine-already-registered"></a>Szenario: Computer ist bereits bei einem anderen Konto registriert.

#### <a name="issue"></a>Problem

Sie erhalten die folgende Fehlermeldung:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Ursache

Der Computer ist bereits in einen anderen Arbeitsbereich für Updateverwaltung integriert.

#### <a name="resolution"></a>Lösung

Bereinigen Sie alte Artefakte auf dem Computer durch [Löschen der Hybrid-Runbook-Gruppe](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group), und wiederholen Sie den Vorgang.

### <a name="machine-unable-to-communicate"></a>Szenario: Computer kann nicht mit dem Dienst kommunizieren.

#### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Ursache

Es gibt möglicherweise einen Proxy, ein Gateway oder eine Firewall, durch den bzw. die die Netzwerkkommunikation blockiert wird.

#### <a name="resolution"></a>Lösung

Überprüfen Sie Ihr Netzwerk, und stellen Sie sicher, dass die entsprechenden Ports und Adressen zulässig sind. Unter [Netzwerkanforderungen](../automation-hybrid-runbook-worker.md#network-planning) finden Sie eine Liste der Ports und Adressen, die von Updateverwaltung und Hybrid Runbook Workers benötigt werden.

### <a name="unable-to-create-selfsigned-cert"></a>Szenario: Selbstsigniertes Zertifikat kann nicht erstellt werden.

#### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Ursache

Der Hybrid Runbook Worker konnte kein selbstsigniertes Zertifikat generieren.

#### <a name="resolution"></a>Lösung

Überprüfen Sie, ob das Systemkonto über Lesezugriff auf den Ordner **C:\ProgramData\Microsoft\Crypto\RSA** verfügt, und versuchen Sie es erneut.

### <a name="hresult"></a>Szenario: Computer wird als nicht bewertet mit einer HResult-Ausnahme angezeigt.

#### <a name="issue"></a>Problem

Für Ihren Computer wird **Nicht bewertet** unter **Konformität** und darunter wird eine Ausnahmemeldung angezeigt.

#### <a name="cause"></a>Ursache

Windows Update oder WSUS wurde auf dem Computer nicht richtig konfiguriert. Für die Updateverwaltung wird Windows Update oder WSUS benötigt, um die erforderlichen Updates, den Status des Patches und die Ergebnisse der bereitgestellten Patches bereitzustellen. Ohne diese Informationen kann die Updateverwaltung keine richtigen Meldungen für die Patches zurückgeben, die benötigt bzw. installiert werden.

#### <a name="resolution"></a>Lösung

Doppelklicken Sie auf die rot angezeigte Ausnahme, um die vollständige Ausnahmemeldung anzuzeigen. Suchen Sie in der folgenden Tabelle nach möglichen Lösungen oder Aktionen:

|Ausnahme  |Lösung oder Aktion  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Suchen Sie den entsprechenden Fehlercode in der [Windows Update-Fehlercodeliste](https://support.microsoft.com/help/938205/windows-update-error-code-list), um weitere Details zur Ursache der Ausnahme zu erfahren.        |
|`0x8024402C` oder `0x8024401C`     | Diese Fehler deuten auf Netzwerkkonnektivitätsprobleme hin. Stellen Sie sicher, dass Ihr Computer über die richtige Netzwerkkonnektivität für die Updateverwaltung verfügt. Im Abschnitt [Netzwerkplanung](../automation-update-management.md#ports) finden Sie eine Liste der erforderlichen Ports und Adressen.        |
|`0x8024402C`     | Wenn Sie einen WSUS-Server verwenden, stellen Sie sicher, dass die Registrierungswerte für `WUServer` und `WUStatusServer` unter dem Registrierungsschlüssel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` den richtigen WSUS-Server aufweisen.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Stellen Sie sicher, dass der Windows Update-Dienst (wuauserv) ausgeführt wird und nicht deaktiviert ist.        |
|Weitere generische Ausnahmen     | Suchen Sie im Internet nach möglichen Lösungen, und arbeiten Sie mit Ihrem lokalen IT-Support zusammen.         |

Die Überprüfung von `windowsupdate.log` kann Ihnen dabei helfen, auch die mögliche Ursache zu ermitteln. Weitere Informationen zum Lesen des Protokolls finden Sie unter [Lesen der Datei „Windowsupdate.log“](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Darüber hinaus können Sie die [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) herunterladen und ausführen, um zu überprüfen, ob auf dem Computer Probleme mit Windows Update bestehen.

> [!NOTE]
> Die [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) ist für Windows-Clients vorgesehen, sie funktioniert aber auch unter Windows Server.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Szenario: Ausführung eines Updates kann nicht gestartet werden.

#### <a name="issue"></a>Problem

Die Ausführung eines Updates kann auf einem Linux-Computer nicht gestartet werden.

#### <a name="cause"></a>Ursache

Der Linux Hybrid Worker ist fehlerhaft.

#### <a name="resolution"></a>Lösung

Erstellen Sie eine Kopie der folgenden Protokolldatei, und speichern Sie sie für Problembehandlungszwecke:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Szenario: Ausführung des Updates wird gestartet, aber es treten Fehler auf.

#### <a name="issue"></a>Problem

Die Ausführung eines Updates wird gestartet, wobei jedoch Fehler auftreten.

#### <a name="cause"></a>Ursache

Mögliche Ursachen:

* Der Paket-Manager ist fehlerhaft.
* Spezifische Pakete können cloudbasiertes Patchen beeinträchtigen.
* Andere Gründe

#### <a name="resolution"></a>Lösung

Wenn während der Ausführung eines Updates nach dem erfolgreichen Start unter Linux Fehler auftreten, überprüfen Sie die Auftragsausgabe des von der Ausführung betroffenen Computers. Möglicherweise finden Sie spezifische Fehlermeldungen des Paket-Managers auf Ihrem Computer, die Sie untersuchen können, um dann Maßnahmen zu ergreifen. Updateverwaltung setzt voraus, dass der Paket-Manager für eine erfolgreiche Bereitstellung von Updates fehlerfrei ist.

In einigen Fällen können Paketupdates Updateverwaltung beeinträchtigen und verhindern, dass die Bereitstellung eines Updates abgeschlossen wird. Wenn dies der Fall ist, müssen Sie diese Pakete entweder von künftigen Ausführungen von Updates ausschließen oder sie manuell installieren.

Wenn Sie ein Patchproblem nicht beheben können, erstellen Sie eine Kopie der folgenden Protokolldatei, und bewahren Sie sie zu Problembehandungszwecken auf, **bevor** die nächste Bereitstellung von Updates beginnt:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.

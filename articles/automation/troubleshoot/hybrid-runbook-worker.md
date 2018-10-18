---
title: Problembehandlung für Hybrid Runbook Worker von Azure Automation
description: Dieser Artikel bietet Informationen zur Problembehandlung für Hybrid Runbook Worker von Azure Automation.
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c8dc240172451118fd75b042ba267740999882d
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321766"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Problembehandlung für Hybrid Runbook Worker

Dieser Artikel bietet Informationen zur Problembehandlung für Hybrid Runbook Worker.

## <a name="general"></a>Allgemein

Der Hybrid Runbook Worker ist von einem Agent abhängig, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbookaufträge zu erhalten und den Status zu melden. Bei Windows ist dies der Microsoft Monitoring Agent. Bei Linux ist dies der OMS-Agent für Linux.

### <a name="runbook-execution-fails"></a>Szenario: Fehler bei der Runbook-Ausführung

#### <a name="issue"></a>Problem

Die Runbook-Ausführung wird mit folgender Fehlermeldung abgebrochen:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Kurz nachdem Sie versucht haben, Ihr Runbook dreimal auszuführen, wird es angehalten. Bestimmte Bedingungen können eine erfolgreiche Ausführung des Runbooks verhindern. Die Fehlermeldung enthält jedoch keine zusätzlichen Angaben zu den Gründen für den Abbruch.

#### <a name="cause"></a>Ursache

Dafür gibt es drei mögliche Ursachen:

* Die Runbooks können nicht bei lokalen Ressourcen authentifiziert werden

* Der Hybrid Worker befindet sich hinter einem Proxy oder einer Firewall.

* Die Runbooks können nicht bei lokalen Ressourcen authentifiziert werden

* Der für die Ausführung des Hybrid Runbook Worker-Features vorgesehene Computer erfüllt die Hardwaremindestanforderungen.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf „*.azure-automation.net“ über Port 443 verfügt.

Computer, auf denen der Hybrid Runbook Worker ausgeführt werden soll, müssen die Mindestanforderungen an die Hardware erfüllen, damit sie als Host für dieses Feature infrage kommen. Andernfalls kommt es – je nach Ressourcennutzung durch andere Hintergrundprozesse und möglichen Konflikten bei der Runbookausführung – zu einer übermäßigen Auslastung des Computers. Dies verursacht Verzögerungen und Timeouts bei Runbookaufträgen.

Vergewissern Sie sich, dass der für die Ausführung des Hybrid Runbook Worker-Features vorgesehene Computer die Hardwaremindestanforderungen erfüllt. Wenn dies der Fall ist, überwachen Sie die CPU- und Arbeitsspeicherauslastung, um Zusammenhänge zwischen der Leistung der Hybrid Runbook Worker-Prozesse und Windows zu erkennen. Falls die Leistungsgrenzen des Arbeitsspeichers oder der CPU fast erreicht werden, deutet dies u.U. darauf hin, dass Sie ein Prozessorupgrade oder weitere Prozessoren oder mehr Speicher benötigen, um den Ressourcenengpass zu beseitigen und den Fehler zu beheben. Alternativ können Sie auch eine andere Computeressource auswählen, die die Mindestanforderungen erfüllt. Bei entsprechend großem Workload skalieren Sie sie nach Bedarf.

Prüfen Sie, ob im Ereignisprotokoll **Microsoft-SMA** ein entsprechendes Ereignis mit der Beschreibung *Win32-Prozess wurde mit dem Code [4294967295] beendet*vorhanden ist. Die Ursache dieses Fehlers liegt darin, dass Sie in Ihren Runbooks die Authentifizierung nicht konfiguriert haben oder dass Sie die „Ausführen als“-Anmeldeinformationen für die Hybrid Worker-Gruppe nicht angegeben haben. Überprüfen Sie die [Runbookberechtigungen](../automation-hrw-run-runbooks.md#runbook-permissions), und vergewissern Sie sich, dass Sie die Authentifizierung für Ihre Runbooks ordnungsgemäß konfiguriert haben.

## <a name="linux"></a>Linux

Der Linux Hybrid Runbook Worker ist abhängig vom OMS-Agent für Linux, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbook-Aufträge zu erhalten und den Status zu melden. Wenn bei der Registrierung des Workers ein Fehler auftritt, gibt es mehrere mögliche Gründe dafür:

### <a name="oms-agent-not-running"></a>Szenario: Der OMS-Agent für Linux wird nicht ausgeführt

Wenn der OMS-Agent für Linux nicht ausgeführt wird, verhindert dies die Kommunikation zwischen dem Linux Hybrid Runbook Worker und Azure Automation. Überprüfen Sie, dass der Agent ausgeführt wird, indem Sie den folgenden Befehl eingeben: `ps -ef | grep python`. Die Ausgabe sollte in etwa folgendermaßen aussehen, die Python-Prozesse mit dem **nxautomation**-Benutzerkonto. Wenn die Lösungen Updateverwaltung oder Azure Automation nicht aktiviert sind, wird keiner der folgenden Prozesse ausgeführt.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Die folgende Liste enthält die Prozesse, die für einen Linux Hybrid Runbook Worker gestartet werden. Sie befinden sich alle im Verzeichnis `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: Dies ist der Worker-Manager-Prozess; er wird direkt von DSC gestartet.

* **worker.conf**: Dieser Prozess ist der automatisch registrierte Hybrid Worker-Prozess; er wird vom Worker-Manager gestartet. Dieser Prozess dient der Updateverwaltung und ist für den Benutzer transparent. Dieser Prozess ist nicht vorhanden, wenn die Lösung Updateverwaltung auf dem Computer nicht aktiviert ist.

* **diy/worker.conf**: Dieser Prozess ist der eigenständige Hybrid Worker-Prozess. Der eigenständige Hybrid Worker-Prozess wird verwendet, um Benutzerrunbooks auf dem Hybrid Runbook Worker auszuführen. Er unterscheidet sich vom automatisch registrierten Hybrid Worker-Prozess nur in einem wichtigsten Detail, nämlich, dass eine andere Konfiguration verwendet wird. Dieser Prozess ist nicht vorhanden, wenn die Lösung Azure Automation nicht aktiviert und der eigenständige Linux Hybrid Worker nicht registriert ist.

Wenn der OMS-Agent für Linux nicht ausgeführt wird, führen Sie zum Starten des Diensts den folgenden Befehl aus: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Szenario: Die angegebene Klasse ist nicht vorhanden

Wenn die Fehlermeldung **Die angegebene Klasse ist nicht vorhanden...** in `/var/opt/microsoft/omsconfig/omsconfig.log` angezeigt wird, muss der OMS-Agent für Linux aktualisiert werden. Führen Sie den folgenden Befehl aus, um den OMS-Agent neu zu installieren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Der Windows Hybrid Runbook Worker ist abhängig vom Microsoft Monitoring Agent, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbook-Aufträge zu empfangen und den Status zu melden. Wenn bei der Registrierung des Workers ein Fehler auftritt, gibt es mehrere mögliche Gründe dafür:

### <a name="mma-not-running"></a>Szenario: Der Microsoft Monitoring Agent wird nicht ausgeführt

#### <a name="issue"></a>Problem

Der Dienst `healthservice` wird nicht auf dem Computer mit dem Hybrid Runbook Worker ausgeführt.

#### <a name="cause"></a>Ursache

Wenn der Microsoft Monitoring Agent-Dienst von Windows nicht ausgeführt wird, verhindert dies die Kommunikation zwischen dem Hybrid Runbook Worker und Azure Automation.

#### <a name="resolution"></a>Lösung

Überprüfen Sie, dass der Agent ausgeführt wird, indem Sie den folgenden Befehl in PowerShell eingeben: `Get-Service healthservice`. Wenn der Dienst beendet wurde, geben Sie den folgenden Befehl in PowerShell ein, um ihn wieder zu starten: `Start-Service healthservice`.

### <a name="event-4502"></a> Ereignis 4502 im Operations Manager-Protokoll

#### <a name="issue"></a>Problem

Im Ereignisprotokoll **Anwendungs- und Dienstprotokolle\Operations Manager** werden das Ereignis 4502 sowie ein EventMessage-Element mit **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** und folgender Beschreibung angezeigt: *Das durch den Dienst „\<wsid\>.oms.opinsights.azure.com“ vorgelegte Zertifikat wurde nicht durch eine für Microsoft-Dienste verwendete Zertifizierungsstelle ausgestellt. Wenden Sie sich bitte an Ihren Netzwerkadministrator, um zu erfahren, ob ein Proxy ausgeführt wird, das die TLS/SSL-Kommunikation abfängt. Im Artikel KB3126513 sind weitere Informationen zur Behandlung von Verbindungsproblemen enthalten.*

#### <a name="cause"></a>Ursache

Dies kann daran liegen, dass Ihr Proxy oder Ihre Netzwerkfirewall die Kommunikation mit Microsoft Azure blockiert. Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf *.azure-automation.net über Port 443 verfügt.

#### <a name="resolution"></a>Lösung

Protokolle werden lokal auf jedem Hybridworker unter "C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes" gespeichert. Sie können überprüfen, ob Warnungen oder Fehlerereignisse in die Ereignisprotokolle **Anwendungs- und Dienstprotokolle\Microsoft-SMA\Operations** und **Anwendungs- und Dienstprotokolle\Operations Manager** eingetragen wurden, die darauf hinweisen, dass ein Verbindungsproblem oder ein anderes Problem das Onboarding der Rolle in Azure Automation beeinträchtigt oder ein Problem bei der Ausführung regulärer Vorgänge vorliegt.

[Runbookausgabe und -meldungen](../automation-runbook-output-and-messages.md) werden von Hybridworkern an Azure Automation gesendet, genau wie Runbookaufträge, die in der Cloud ausgeführt werden. Sie können auch die Verbose- und Progress-Streams auf dieselbe Weise aktivieren wie für andere Runbooks.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.

---
title: Beheben von Fehlern mit Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit Updateverwaltung beheben können.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 08/08/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 2e47320d5ad88edfa8ea6122f3a0abd104230974
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "42140327"
---
# <a name="troubleshooting-issues-with-update-management"></a>Behandeln von Problemen mit Updateverwaltung

In diesem Artikel werden Lösungen zur Behebung von Problemen beschrieben, die bei Verwendung der Updateverwaltung auftreten können.

## <a name="general"></a>Allgemein

### <a name="components-enabled-not-working"></a>Szenario: Die Komponenten für die Lösung „Updateverwaltung“ wurden aktiviert, und diese VM wird nun konfiguriert.

#### <a name="issue"></a>Problem

Die folgende Meldung wird 15 Minuten nach dem Onboarding auf einer VM angezeigt:

```
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. Die Kommunikation an das Automation-Konto wird blockiert.
2. Die VM, die integriert wird, stammt womöglich von einem Cloudcomputer, dessen System nicht mit dem installierten Microsoft Monitoring Agent vorbereitet wurde.

#### <a name="resolution"></a>Lösung

1. Sehen Sie sich den Abschnitt [Konfigurieren des Netzwerks](../automation-hybrid-runbook-worker.md#network-planning) an, um zu erfahren, welche Adressen und Ports zugelassen werden müssen, damit die Updateverwaltung funktioniert.
2. Wenn Sie ein geklontes Image verwenden, bereiten Sie mit Sysprep das Image zunächst vor, und installieren Sie den MMA-Agent anschließend.

## <a name="windows"></a>Windows

Sollten Sie Probleme beim Integrieren der Lösung in einen virtuellen Computers haben, suchen Sie auf dem lokalen Computer im Ereignisprotokoll **Operations Manager** unter **Anwendungs- und Dienstprotokolle** nach Ereignissen mit der Ereignis-ID **4502** und einer Ereignismeldung, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthält.

Der folgende Abschnitt enthält spezifische Fehlermeldungen und passende Lösungsvorschläge. Informationen zu anderen Integrationsproblemen finden Sie unter [Problembehandlung bei der Integration von Lösungen](onboarding.md).

### <a name="machine-already-registered"></a>Szenario: Computer bereits bei einem anderen Konto registriert

#### <a name="issue"></a>Problem

Sie erhalten die folgende Fehlermeldung:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Ursache

Der Computer ist bereits in einen anderen Arbeitsbereich für Updateverwaltung integriert.

#### <a name="resolution"></a>Lösung

Bereinigen Sie alte Artefakte auf dem Computer durch [Löschen der Hybrid-Runbook-Gruppe](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group), und wiederholen Sie den Vorgang.

### <a name="machine-unable-to-communicate"></a>Szenario: Computer kann nicht mit dem Dienst kommunizieren

#### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Ursache

Es gibt möglicherweise einen Proxy, ein Gateway oder eine Firewall, wodurch eine Blockierung der Netzwerkkommunikation verursacht wird.

#### <a name="resolution"></a>Lösung

Überprüfen Sie Ihr Netzwerk, und stellen Sie sicher, dass die entsprechenden Ports und Adressen zulässig sind. Unter [Netzwerkanforderungen](../automation-hybrid-runbook-worker.md#network-planning) finden Sie eine Liste der Ports und Adressen, die von Updateverwaltung und Hybrid Runbook Workers benötigt werden.

### <a name="unable-to-create-selfsigned-cert"></a>Szenario: Selbstsigniertes Zertifikat kann nicht erstellt werden

#### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Ursache

Der Hybrid Runbook Worker konnte kein selbstsigniertes Zertifikat generieren.

#### <a name="resolution"></a>Lösung

Überprüfen Sie, ob das Systemkonto über Lesezugriff auf den Ordner **C:\ProgramData\Microsoft\Crypto\RSA** verfügt, und versuchen Sie es erneut.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Szenario: Ausführung eines Updates kann nicht gestartet werden

#### <a name="issue"></a>Problem

Die Ausführung eines Updates kann auf einem Linux-Computer nicht gestartet werden.

#### <a name="cause"></a>Ursache

Der Linux Hybrid Worker ist fehlerhaft.

#### <a name="resolution"></a>Lösung

Erstellen Sie eine Kopie der folgenden Protokolldatei, und speichern Sie sie für Problembehandlungszwecke:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Szenario: Ausführung des Updates wird gestartet, es treten jedoch Fehler auf

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

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
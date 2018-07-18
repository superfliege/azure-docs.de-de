---
title: Beheben von Fehlern mit Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit Updateverwaltung beheben können.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b77d1210ff48a4bd30834fcbad64173bf77b1290
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064385"
---
# <a name="troubleshooting-issues-with-update-management"></a>Behandeln von Problemen mit Updateverwaltung

In diesem Artikel werden Lösungen zur Behebung von Problemen beschrieben, die bei Verwendung von Updateverwaltung auftreten können.

## <a name="windows"></a>Windows

Sollten Sie Probleme beim Integrieren der Lösung in einen virtuellen Computers haben, suchen Sie auf dem lokalen Computer im Ereignisprotokoll **Operations Manager** unter **Anwendungs- und Dienstprotokolle** nach Ereignissen mit der Ereignis-ID **4502** und einer Ereignismeldung, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthält.

Der folgende Abschnitt enthält spezifische Fehlermeldungen und passende Lösungsvorschläge. Informationen zu anderen Integrationsproblemen finden Sie unter [Problembehandlung bei der Integration von Lösungen](onboarding.md).

### <a name="machine-already-registered"></a>Szenario: Computer bereits bei einem anderen Konto registriert

#### <a name="issue"></a>Problem

Sie erhalten die folgende Fehlermeldung:

```error
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
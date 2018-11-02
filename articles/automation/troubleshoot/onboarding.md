---
title: Beheben von Fehlern beim Integrieren von Lösungen für Updateverwaltung, Änderungsnachverfolgung und Bestand
description: Erfahren Sie, wie Sie Fehler beim Integrieren von Lösungen für Updateverwaltung, Änderungsnachverfolgung und Bestand beheben.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 40a1955e88b23ecfb86412b388413b920dd2eb1a
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407604"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Beheben von Fehlern beim Integrieren von Lösungen

Beim Integrieren von Lösungen wie Updateverwaltung oder Änderungsnachverfolgung können Fehler auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="computer-grou-query-format-error"></a>Szenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problem

Dieser Fehlercode bedeutet, dass die Abfrage einer gespeicherten Suchcomputergruppe für die Ziellösung nicht ordnungsgemäß formatiert war. 

#### <a name="cause"></a>Ursache

Möglicherweise haben Sie die Abfrage geändert, oder sie wurde vom System geändert.

#### <a name="resolution"></a>Lösung

Sie können die Abfrage für diese Lösung löschen und die Lösung erneut anwenden, wodurch die Abfrage neu erstellt wird. Die Abfrage finden Sie in Ihrem Arbeitsbereich unter **Gespeicherte Suchvorgänge**. Der Name der Abfrage lautet **MicrosoftDefaultComputerGroup**, und die Kategorie der Abfrage ist der Name der mit dieser Abfrage verknüpften Lösung. Wenn mehrere Lösungen aktiviert sind, wird **MicrosoftDefaultComputerGroup** unter **Speicherte Suchvorgänge** mehrfach angezeigt.

### <a name="policy-violation"></a>Szenario: PolicyViolation

#### <a name="issue"></a>Problem

Dieser Fehlercode bedeutet, dass bei der Bereitstellung ein Fehler aufgrund einer oder mehrerer Richtlinienverletzungen aufgetreten ist.

#### <a name="cause"></a>Ursache 

Eine Richtlinie ist vorhanden, die den Abschluss des Vorgangs blockiert.

#### <a name="resolution"></a>Lösung

Für eine erfolgreiche Bereitstellung der Lösung müssen Sie eine Änderung der angegebenen Richtlinie in Betracht ziehen. Da viele verschiedene Richtlinientypen definiert werden können, hängen die erforderlichen Änderungen von der Richtlinie ab, gegen die verstoßen wurde. Beispiel: Wenn eine Richtlinie für eine Ressourcengruppe festgelegt wurde, die die Berechtigung zum Ändern der Inhalte bestimmter Ressourcentypen in dieser Ressourcengruppe verweigert hat, können Sie beispielsweise einen der folgenden Schritte ausführen:

* Entfernen Sie die Richtlinie vollständig.
* Versuchen Sie, sie für eine andere Ressourcengruppe festzulegen.
* Überarbeiten Sie die Richtlinie etwa wie folgt:
  * Weisen Sie die Richtlinie einer bestimmten Ressource neu zu (z.B. einem bestimmten Automation-Konto).
  * Überarbeiten Sie die Gruppe der Ressourcen, zu deren Ablehnung diese Richtlinie konfiguriert wurde.

Lesen Sie die Benachrichtigungen in der oberen rechten Ecke des Azure-Portals, oder navigieren Sie zum Anzeigen der fehlerhaften Bereitstellung zu der Ressourcengruppe, die Ihr Automation-Konto enthält, und wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus. Weitere Informationen zu Azure Policy finden Sie unter [Was ist Azure Policy](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>MMA-Erweiterungsfehler

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bei der Bereitstellung einer Lösung wird eine Vielzahl zugehöriger Ressourcen bereitgestellt. Eine dieser Ressourcen ist die Microsoft Monitoring Agent-Erweiterung oder der Log Analytics-Agent für Linux. Dies sind VM-Erweiterungen, die vom Gast-Agent des virtuellen Computers installiert werden. Dieser ist zuständig für die Kommunikation mit dem konfigurierten Log Analytics-Arbeitsbereich zum Zweck der späteren Koordination des Herunterladens von Binär- und anderen Dateien, von denen die von Ihnen integrierte Lösung abhängt, sobald die Ausführung beginnt.
Auf Fehler bei der Installation von MMA oder des Log Analytics-Agents für Linux werden Sie zumeist über eine Benachrichtigung im Notifications Hub aufmerksam. Wenn Sie auf diese Benachrichtigung klicken, erhalten Sie weitere Informationen zum jeweiligen Fehler. Durch Navigieren zur Ressource „Ressourcengruppen“ und dann zum darin enthaltenen Element „Bereitstellungen“ werden auch Details zu den aufgetretenen Bereitstellungsfehlern angezeigt.
Die Installation von MMA oder des Log Analytics-Agents für Linux kann aus verschiedenen Gründen fehlschlagen, wobei die Schritte zur Behebung dieser Fehler je nach Problem unterschiedlich sind. Es folgen spezifische Schritte zur Problembehandlung.

Im folgenden Abschnitt werden verschiedene Probleme beschrieben, die bei der Integration auftreten können und die einen Fehler bei der Bereitstellung der MMA-Erweiterung verursachen.

### <a name="webclient-exception"></a>Szenario: Ausnahme während einer WebClient-Anforderung

Die MMA-Erweiterung auf dem virtuellen Computer kann nicht mit externen Ressourcen kommunizieren, und die Bereitstellung schlägt fehl.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die zurückgegeben werden:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* In der VM ist ein Proxy konfiguriert, der nur bestimmte Ports zulässt.

* Eine Firewalleinstellung hat den Zugriff auf die benötigten Ports und Adressen blockiert.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass die ordnungsgemäßen Ports und Adressen für die Kommunikation geöffnet sind. Eine Liste der Ports und Adressen finden Sie unter [Planen Ihres Netzwerks](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Szenario: Fehler bei der Installation aufgrund vorübergehender Umgebungsprobleme

Die Installation der Microsoft Monitoring Agent-Erweiterung ist während der Installation aufgrund einer anderen Installation oder Aktion fehlgeschlagen, die die Installation blockiert.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die ggf. zurückgegeben werden:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Eine weitere Installation erfolgt gerade.
* Auf dem System wurde während der Vorlagenbereitstellung ein Neustart ausgelöst.

#### <a name="resolution"></a>Lösung

Dieser Fehler tritt nur vorübergehend auf. Wiederholen Sie die Bereitstellung, um die Erweiterung zu installieren.

### <a name="installation-timeout"></a>Szenario: Zeitlimit für Installation

Die Installation der MMA-Erweiterung wurde aufgrund eines Zeitlimits nicht abgeschlossen.

#### <a name="issue"></a>Problem

Im folgenden finden ein Beispiel einer Fehlermeldung, die zurückgegeben werden kann:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Ursache

Dieser Fehler ist darauf zurückzuführen, dass der virtuelle Computer während der Installation unter großer Last stand.

### <a name="resolution"></a>Lösung

Versuchen Sie, die MMA-Erweiterung zu installieren, wenn die VM unter geringerer Last steht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.

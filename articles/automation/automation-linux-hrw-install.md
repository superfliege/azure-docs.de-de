---
title: Azure Automation Hybrid Runbook Worker (Linux)
description: Dieser Artikel enthält Informationen zum Installieren eines Azure Automation-Hybrid Runbook Workers, mit dem Sie Runbooks auf Linux-basierten Computern in Ihrem lokalen Datencenter oder einer Cloudumgebung ausführen können.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: aca68b6e8d0e6b80a1504b16b9b3462f20fdc6c4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195657"
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Bereitstellen eines Linux-Hybrid Runbook Workers

Die Azure Automation-Funktion „Hybrid Runbook Worker“ ermöglicht das Ausführen von Runbooks direkt auf dem Computer, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Linux Hybrid Runbook Worker führt Runbooks als spezieller Benutzer mit erhöhten Rechten aus, um Befehle ausführen zu können, für die erhöhte Rechte erforderlich sind. Runbooks werden in Azure Automation gespeichert und verwaltet und an einen oder mehrere dafür vorgesehene Computer übermittelt. Dieser Artikel beschreibt, wie Sie den Hybrid Runbook Worker auf einem Linux-Computer installieren.

## <a name="supported-linux-operating-systems"></a>Unterstützte Linux-Betriebssysteme

Im folgenden finden Sie eine Liste unterstützter Linux-Distributionen:

* Amazon Linux 2012.09 bis 2015.09 (x86/x64)
* CentOS Linux 5, 6 und 7 (x86/x64)
* Oracle Linux 5, 6 und 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 und 7 (x86/x64)
* Debian GNU/Linux 6, 7 und 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 und 12 (x86/x64)

## <a name="installing-linux-hybrid-runbook-worker"></a>Installieren eines Linux Hybrid Runbook Worker

Um einen Hybrid Runbook Worker auf Ihrem Linux-Computer zu installieren und zu konfigurieren, führen Sie ein einfaches Verfahren zur manuellen Installation und Konfiguration der Rolle aus. Dazu ist die Aktivierung der Lösung **Automation Hybrid Worker** in Ihrem Log Analytics-Arbeitsbereich erforderlich sowie das Ausführen einiger Befehle, um den Computer als Worker zu registrieren und ihn einer neuen oder vorhandenen Gruppe hinzuzufügen.

Im folgenden finden Sie die Mindestanforderungen für Linux Hybrid Runbook Worker:

* Mindestens zwei Kerne
* Mindestens 4 GB RAM
* Port 443 (ausgehend)

### <a name="package-requirements"></a>Paketanforderungen

| **Erforderliches Paket** | **Beschreibung** | **Mindestversion**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C-Bibliothek| 2.5-12 |
|Openssl| OpenSSL-Bibliotheken | 0.9.8e oder 1.0|
|Curl | cURL-Webclient | 7.15.5|
|Python-ctypes | |
|PAM | Module für austauschbare Authentifizierung|

Bevor Sie den Vorgang fortsetzen, müssen Sie den Log Analytics-Arbeitsbereich, mit dem Ihr Automation-Konto verknüpft ist, sowie den Primärschlüssel für Ihr Automation-Konto notieren. Sie finden beides im Portal. Wählen Sie Ihr Automation-Konto aus, und klicken Sie für die Arbeitsbereichs-ID auf **Arbeitsbereich** und für den Primärschlüssel auf **Schlüssel**. Weitere Informationen zu Ports und Adressen, die für Hybrid Runbook Worker erforderlich sind, finden Sie unter [Konfigurieren des Netzwerks](automation-hybrid-runbook-worker.md#network-planning).

1. Aktivieren Sie die Lösung „Automation Hybrid Worker“ in Azure. Dies kann durch Folgendes geschehen:

   1. Fügen Sie die Lösung **Automation Hybrid Worker** Ihrem Abonnement mithilfe des Verfahrens unter [Hinzufügen von Azure Log Analytics-Verwaltungslösungen zu Ihrem Arbeitsbereich](../log-analytics/log-analytics-add-solutions.md) hinzu.
   1. Führen Sie das folgende Cmdlet aus:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Installieren Sie den OMS-Agent für Linux durch Ausführen des folgenden Befehls, wobei Sie \<WorkspaceID\> und \<WorkspaceKey\> durch die entsprechenden Werte Ihres Arbeitsbereichs ersetzen.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Führen Sie den folgenden Befehl aus. Ändern Sie die Werte für die Parameter *-w*, *-k*, *-g* und *-e*. Ersetzen Sie den Wert für den Parameter *-g* durch den Namen der Hybrid Runbook Worker-Gruppe, der der neue Linux Hybrid Runbook Worker beitreten soll. Wenn der Name in Ihrem Automation-Konto nicht bereits vorhanden ist, wird eine neue Hybrid Runbook Worker-Gruppe mit diesem Namen erstellt.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Nach Ausführung des Befehls werden auf der Seite „Hybrid Worker-Gruppen“ im Azure-Portal die neue Gruppe und die Anzahl von Mitgliedern angezeigt. Im Falle einer bereits vorhandenen Gruppe wird die Anzahl der Mitglieder erhöht. Sie können die Gruppe aus der Liste auf der Seite **Hybrid Worker-Gruppen** auswählen und dann auf die Kachel **Hybrid Worker** klicken. Auf der Seite **Hybrid Worker** werden die einzelnen Mitglieder der Gruppe aufgelistet.

## <a name="turning-off-signature-validation"></a>Deaktivieren der Signaturüberprüfung

Standardmäßig erfordern Linux Hybrid Runbook Worker eine Überprüfung der Signatur. Wenn Sie ein unsigniertes Runbook für einen Worker ausführen, wird ein „Fehler bei der Signaturüberprüfung“ angezeigt. Um die Signaturüberprüfung zu deaktivieren, führen Sie den folgenden Befehl aus, und ersetzen Sie dabei den zweiten Parameter durch Ihre Log Analytics-Arbeitsbereichs-ID:

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Unterstützte Runbooktypen

Linux Hybrid Runbook Worker unterstützen nicht alle Runbooktypen von Azure Automation.

Die folgenden Runbooktypen werden auf einem Linux Hybrid Worker unterstützt:

* Python 2
* PowerShell

Die folgenden Runbooktypen werden auf einem Linux Hybrid Worker nicht unterstützt:

* PowerShell-Workflow
* Grafisch
* Grafischer PowerShell-Workflow

## <a name="troubleshooting"></a>Problembehandlung

Der Linux Hybrid Runbook Worker ist abhängig vom OMS-Agent für Linux, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbook-Aufträge zu erhalten und den Status zu melden. Wenn die Registrierung des Workers fehlschlägt, gibt es mehrere mögliche Gründe dafür:

### <a name="the-oms-agent-for-linux-is-not-running"></a>Der OMS-Agent für Linux wird nicht ausgeführt

Wenn der OMS-Agent für Linux nicht ausgeführt wird, verhindert dies die Kommunikation zwischen dem Linux Hybrid Runbook Worker und Azure Automation. Überprüfen Sie, dass der Agent ausgeführt wird, indem Sie den folgenden Befehl eingeben: `ps -ef | grep python`. Die Ausgabe sollte in etwa folgendermaßen aussehen, die Python-Prozesse mit dem **nxautomation**-Benutzerkonto. Wenn die Updateverwaltung oder Azure Automation-Lösungen nicht aktiviert sind, wird keiner der folgenden Prozesse ausgeführt.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Die folgende Liste enthält die Prozesse, die für einen Linux Hybrid Runbook Worker gestartet werden. Sie befinden sich alle im Verzeichnis `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: Dies ist der Worker-Manager-Prozess; er wird direkt von DSC gestartet.

* **worker.conf**: Dieser Prozess ist der automatisch registrierte Hybrid Worker-Prozess; er wird vom Worker-Manager gestartet. Dieser Prozess dient der Updateverwaltung und ist für den Benutzer transparent. Dieser Prozess ist nicht vorhanden, wenn die Updateverwaltungslösung auf dem Computer nicht aktiviert ist.

* **diy/worker.conf**: Dieser Prozess ist der eigenständige Hybrid Worker-Prozess. Der eigenständige Hybrid Worker-Prozess wird verwendet, um Benutzerrunbooks auf dem Hybrid Runbook Worker auszuführen. Er unterscheidet sich vom automatisch registrierten Hybrid Worker-Prozess nur in einem wichtigsten Detail, nämlich, dass eine andere Konfiguration verwendet wird. Dieser Prozess ist nicht vorhanden, wenn die Azure Automation-Lösung nicht aktiviert und der eigenständige Linux Hybrid Worker nicht registriert ist.

Wenn der OMS-Agent für Linux nicht ausgeführt wird, führen Sie zum Starten des Diensts den folgenden Befehl aus: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-does-not-exist"></a>Die angegebene Klasse ist nicht vorhanden

Wenn die Fehlermeldung **Die angegebene Klasse ist nicht vorhanden...** in `/var/opt/microsoft/omsconfig/omsconfig.log` angezeigt wird, muss der OMS-Agent für Linux aktualisiert werden. Führen Sie den folgenden Befehl aus, um den OMS-Agent neu zu installieren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Zusätzliche Schritte zum Beheben von Problemen mit der Updateverwaltung finden Sie unter [Updateverwaltung – Problembehandlung](automation-update-management.md#troubleshooting)

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie sich [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.
* Anweisungen zum Entfernen von Hybrid Runbook Workern finden Sie unter [Entfernen von Azure Automation-Hybrid-Runbook Workern](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker).

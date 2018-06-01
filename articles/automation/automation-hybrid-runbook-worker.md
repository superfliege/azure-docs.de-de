---
title: Azure Automation – Hybrid-Runbook-Worker
description: In diesem Artikel werden Informationen zum Installieren und Verwenden der Azure Automation-Funktion „Hybrid Runbook Worker“ bereitgestellt, mit der Sie Runbooks auf Computern in Ihrem lokalen Rechenzentrum oder Cloudanbieter ausführen können.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 270990d5dea53c2467bf2d7df4695a14b3dbec8c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194620"
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatisieren von Ressourcen im Rechenzentrum oder in der Cloud mit Hybrid Runbook Worker

Runbooks in Azure Automation können möglicherweise nicht auf Ressourcen in anderen Clouds oder in Ihrer lokalen Umgebung zugreifen, da sie in der Azure-Cloud ausgeführt werden. Die Azure Automation-Funktion „Hybrid Runbook Worker“ ermöglicht das Ausführen von Runbooks direkt auf dem Computer, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Runbooks werden in Azure Automation gespeichert und verwaltet und an einen oder mehrere dafür vorgesehene Computer übermittelt.

Diese Funktionalität wird in der folgenden Abbildung veranschaulicht:

![Hybrid-Runbook-Worker – Übersicht](media/automation-hybrid-runbook-worker/automation.png)

## <a name="hybrid-runbook-worker-groups"></a>Hybrid-Runbook-Workergruppen

Jeder Hybrid-Runbook-Worker ist ein Mitglied einer Hybrid-Runbook-Workergruppe, die Sie beim Installieren des Agents angeben. Eine Gruppe kann einen einzelnen Agent umfassen, aber für Hochverfügbarkeit können Sie mehrere Agents installieren.

Wenn Sie ein Runbook auf einen Hybrid Runbook Worker starten, geben Sie die Gruppe an, in der das Runbook ausgeführt werden soll. Jeder Worker in der Gruppe ruft Azure Automation ab, um festzustellen, ob Aufträge verfügbar sind. Wenn ein Auftrag verfügbar ist, übernimmt ihn der erste verfügbare Worker. Sie können keinen bestimmten Worker angeben.

## <a name="installing-a-hybrid-runbook-worker"></a>Installieren eines Hybrid Runbook Worker

Der Prozess zur Installation des Hybrid Runbook Worker variiert je nach Betriebssystem. Die folgende Tabelle enthält Links zu den verschiedenen Methoden für die Installation eines Hybrid Runbook Worker. Zum Installieren und Konfigurieren eines Windows Hybrid Runbook Worker stehen zwei Methoden zur Verfügung. Die empfohlene Methode besteht darin, ein Automation-Runbook zu verwenden, um den Prozess, der zum Konfigurieren eines Windows-Computers erforderlich ist, vollständig zu automatisieren. Bei der zweiten Methode wird die Rolle Schritt für Schritt manuell installiert und konfiguriert. Bei Linux-Computern führen Sie zur Installation des Agents auf dem Computer ein Python-Skript aus.

|Betriebssystem  |Bereitstellungstypen  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manuell](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Um die Konfiguration der Server zu verwalten, die die Hybrid Runbook Worker-Rolle mit Desired State Configuration (DSC) unterstützen, müssen Sie die Server als DSC-Knoten hinzufügen. Weitere Informationen zu ihrer Integration für die Verwaltung durch DSC finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
>
>Wenn Sie die [Lösung für die Updateverwaltung](automation-update-management.md) aktivieren, werden alle mit dem Log Analytics-Arbeitsbereich verbundenen Computer automatisch als Hybrid Runbook Worker konfiguriert, um Runbooks zu unterstützen, die in dieser Lösung enthalten sind. Es wird aber keine Registrierung für Hybrid Worker-Gruppen durchgeführt, die in Ihrem Automation-Konto bereits definiert wurden. Der Computer kann einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzugefügt werden, um Automation-Runbooks zu unterstützen, solange Sie sowohl für die Lösung als auch die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

Bevor Sie mit der Bereitstellung eines Hybrid Runbook Worker beginnen, lesen Sie die [Informationen zum Planen Ihres Netzwerks](#network-planning). Lesen Sie sich nach dem erfolgreichen Bereitstellen eines Runbook Workers [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.

## <a name="removing-hybrid-runbook-worker"></a>Entfernen von Hybrid Runbook Worker

Je nach ihren Anforderungen können Sie eine oder mehrere Hybrid Runbook Worker aus einer Gruppe entfernen, oder Sie können die Gruppe entfernen, je nach Ihren Anforderungen. Wenn Sie einen Hybrid Runbook Worker von einem lokalen Computer entfernen möchten, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem Automation-Konto.
2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Schlüssel** aus, und notieren Sie sich die Werte für die Felder **URL** und **Primärer Zugriffsschlüssel**. Sie benötigen diese Informationen im nächsten Schritt.

### <a name="windows"></a>Windows

Öffnen Sie eine PowerShell-Sitzung im Administratormodus, und führen Sie den folgenden Befehl aus: Verwenden Sie den Schalter **-Verbose** , um ein ausführliches Protokoll zum Entfernungsvorgang zu erhalten.

```powershell
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Verwenden Sie zum Entfernen veralteter Computer aus Ihrer Hybrid Worker-Gruppe den optionalen Parameter `machineName`.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Dadurch wird nicht Microsoft Monitoring Agent von diesem Computer entfernt, sondern nur die Funktionen und die Konfiguration der Hybrid Runbook-Workerrolle.

## <a name="remove-hybrid-worker-groups"></a>Entfernen von Hybrid Worker-Gruppen

Um eine Gruppe zu entfernen, müssen Sie zuerst den Hybrid Runbook Worker von allen Computern entfernen, die Mitglied der Gruppe sind. Verwenden Sie dazu das weiter oben gezeigte Verfahren, und führen Sie dann die folgenden Schritte aus, um die Gruppe zu entfernen.

1. Wählen Sie im Azure-Portal das Automation-Konto aus.
1. Wählen Sie unter **Prozessautomatisierung** die Option **Hybrid Worker-Gruppen** aus. Wählen Sie die Gruppe aus, die Sie löschen möchten. Nach dem Auswählen der Gruppe wird die Seite mit den Eigenschaften der **Hybrid Worker-Gruppe** angezeigt.

   ![Seite „Hybrid Runbook Worker-Gruppe“](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Klicken Sie auf der Seite mit den Eigenschaften der ausgewählten Gruppe auf **Löschen**. Eine Meldung wird angezeigt, in der Sie aufgefordert werden, diese Aktion zu bestätigen. Wählen Sie **Ja**, wenn Sie sicher sind, dass Sie fortfahren möchten.

   ![Bestätigungsdialogfeld zum Löschen der Gruppe](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

## <a name="network-planning"></a>Konfigurieren des Netzwerks

### <a name="hybrid-worker-role"></a>Hybrid Worker-Rolle

Damit der Hybrid Runbook Worker eine Verbindung mit Log Analytics herstellen und sich bei dem Dienst registrieren kann, benötigt er Zugriff auf die in diesem Abschnitt angegebene Portnummer und die URLs. Dies gilt zusätzlich zu den [für Microsoft Monitoring Agent erforderlichen Ports und URLs](../log-analytics/log-analytics-agent-windows.md) für die Herstellung einer Verbindung mit Log Analytics.

Wenn Sie einen Proxyserver für die Kommunikation zwischen dem Agent und dem Log Analytics-Dienst verwenden, müssen Sie sicherstellen, dass auf die entsprechenden Ressourcen zugegriffen werden kann. Wenn Sie eine Firewall verwenden, um den Zugriff auf das Internet einzuschränken, müssen Sie die Firewall so konfigurieren, dass der Zugriff möglich ist.

Nachfolgend sind der Port und die URLs aufgeführt, die für die Kommunikation zwischen der Hybrid Runbook Worker-Rolle und Automation erforderlich sind:

* Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich.
* Globale URL: *.azure-automation.net
* Globale URL von „US Gov Virginia“: *.azure-automation.us
* Agent-Dienst: https://\<Arbeitsbereichs-ID\>.agentsvc.azure-automation.net

Wenn eines Ihrer Automation-Konten für eine bestimmte Region definiert ist, können Sie die Kommunikation mit diesem regionalen Rechenzentrum einschränken. Die folgende Tabelle enthält den DNS-Eintrag für jede Region.

| **Region** | **DNS-Eintrag** |
| --- | --- |
| USA, Westen-Mitte | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| USA Süd Mitte |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Indien, Mitte |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan, Osten |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australien, Südosten |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| UK, Süden | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Government, Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Wenn Sie eine Liste mit IP-Adressen der Regionen anstelle ihrer Namen benötigen, können Sie die XML-Datei mit den [IP-Adressen der Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) aus dem Microsoft Download Center herunterladen.

> [!NOTE]
> Die XML-Datei mit den IP-Adressen der Azure-Rechenzentren enthält die IP-Adressbereiche, die in den Microsoft Azure-Rechenzentren verwendet werden. Die Datei enthält die Bereiche für Compute, SQL und Storage.
>
>Eine aktualisierte Datei wird wöchentlich veröffentlicht. Die Datei enthält die derzeit bereitgestellten Bereichen und alle anstehenden Änderungen an den IP-Adressbereichen. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Rechenzentren verwendet.
>
> Sie sollten die neue XML-Datei jede Woche herunterladen. Führen Sie damit dann eine Aktualisierung an Ihrem Standort durch, um in Azure ausgeführte Dienste ordnungsgemäß zu identifizieren. Diese Datei zum Aktualisieren der BGP-Ankündigung (Border Gateway Protocol) von Azure-Bereichen wird Azure ExpressRoute-Benutzern jeweils in der ersten Woche des Monats angezeigt.

### <a name="update-management"></a>Updateverwaltung

Neben den Standardadressen und -ports, die der Hybrid Runbook Worker erfordert, sind folgende Adressen speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="troubleshooting"></a>Problembehandlung

Der Hybrid Runbook Worker ist von einem Agent abhängig, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbookaufträge zu erhalten und den Status zu melden. Bei Windows ist dies der Microsoft Monitoring Agent. Bei Linux ist dies der OMS-Agent für Linux. Wenn bei der Registrierung des Workers ein Fehler auftritt, gibt es mehrere mögliche Gründe dafür:

### <a name="the-hybrid-worker-is-behind-a-proxy-or-firewall"></a>Der Hybrid Worker befindet sich hinter einem Proxy oder einer Firewall.

Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf „*.azure-automation.net“ über Port 443 verfügt.

### <a name="the-computer-the-hybrid-worker-is-running-on-has-less-than-the-minimum-hardware-requirements"></a>Der Computer, auf dem der Hybrid Worker ausgeführt wird, erfüllt die Mindestanforderungen für die Hardware nicht vollständig.

Computer, auf denen der Hybrid Runbook Worker ausgeführt werden soll, müssen die Mindestanforderungen an die Hardware erfüllen, damit sie als Host für dieses Feature infrage kommen. Andernfalls kommt es – je nach Ressourcennutzung durch andere Hintergrundprozesse und möglichen Konflikten bei der Runbookausführung – zu einer übermäßigen Auslastung des Computers. Dies verursacht Verzögerungen und Timeouts bei Runbookaufträgen.

Vergewissern Sie sich, dass der für die Ausführung des Hybrid Runbook Worker-Features vorgesehene Computer die Hardwaremindestanforderungen erfüllt. Wenn dies der Fall ist, überwachen Sie die CPU- und Arbeitsspeicherauslastung, um Zusammenhänge zwischen der Leistung der Hybrid Runbook Worker-Prozesse und Windows zu erkennen. Falls die Leistungsgrenzen des Arbeitsspeichers oder der CPU fast erreicht werden, deutet dies u.U. darauf hin, dass Sie ein Prozessorupgrade oder weitere Prozessoren oder mehr Speicher benötigen, um den Ressourcenengpass zu beseitigen und den Fehler zu beheben. Alternativ können Sie auch eine andere Computeressource auswählen, die die Mindestanforderungen erfüllt. Bei entsprechend großem Workload skalieren Sie sie nach Bedarf.

Weitere Informationen zur Behandlung von Problemen bei einem bestimmten Betriebssystem finden Sie unter [Bereitstellen eines Hybrid Runbook Worker unter Linux](automation-linux-hrw-install.md#troubleshooting) oder [Bereitstellen eines Hybrid Runbook Worker unter Windows](automation-windows-hrw-install.md#troubleshooting).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie sich [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.

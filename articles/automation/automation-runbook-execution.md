---
title: Ausführen von Runbooks in Azure Automation
description: Beschreibt ausführlich, wie ein Runbook in Azure Automation verarbeitet wird.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2b1a6e2921fdaf9ede1184cfc02c3f61f63c60ac
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393762"
---
# <a name="runbook-execution-in-azure-automation"></a>Ausführen von Runbooks in Azure Automation

Wenn Sie ein Runbook in Azure Automation starten, wird ein Auftrag erstellt. Ein Auftrag ist eine einzelne Ausführungsinstanz eines Runbooks. Für die Ausführung jedes Auftrags wird ein Azure Automation-Worker zugewiesen. Wenngleich Worker von vielen Azure-Konten gemeinsam genutzt werden, sind die Aufträge von verschiedenen Automation-Konten voneinander isoliert. Sie können nicht steuern, welcher Worker die Anforderung für Ihren Auftrag verarbeitet. Für ein einzelnes Runbook können viele Aufträge gleichzeitig ausgeführt werden. Die Ausführungsumgebung für Aufträge aus dem gleichen Automation-Konto kann wiederverwendet werden. Wenn Sie die Liste der Runbooks im Azure-Portal anzeigen, wird der Status aller Aufträge aufgelistet, die für jedes Runbook gestartet wurden. Sie können die Liste der Aufträge für jedes Runbook anzeigen, um den Status der einzelnen Aufträge nachzuverfolgen. Eine Beschreibung der verschiedenen Auftragsstatusangaben finden Sie unter [Auftragsstatuswerte](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

Das folgende Diagramm zeigt den Lebenszyklus eines Runbookauftrags für [grafische Runbooks](automation-runbook-types.md#graphical-runbooks) und [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks).

![Auftragsstatus – PowerShell-Workflow](./media/automation-runbook-execution/job-statuses.png)

Das folgende Diagramm zeigt den Lebenszyklus eines Runbookauftrags für [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks).

![Auftragsstatus – PowerShell-Skript](./media/automation-runbook-execution/job-statuses-script.png)

Ihre Aufträge können auf Ihre Azure-Ressourcen zugreifen, indem sie eine Verbindung mit Ihrem Azure-Abonnement herstellen. Sie besitzen nur Zugriff auf Ressourcen in Ihrem Rechenzentrum, wenn aus der öffentlichen Cloud auf diese Ressourcen zugegriffen werden kann.

## <a name="job-statuses"></a>Auftragsstatuswerte

Die folgende Tabelle beschreibt die verschiedenen Status, die für einen Auftrag möglich sind. PowerShell unterscheidet zwei Arten von Fehlern: Fehler mit Abbruch und Fehler ohne Abbruch. Fehler mit Abbruch setzen den Runbookstatus auf **Fehler**, wenn sie auftreten. Fehler ohne Abbruch ermöglichen es dem Skript, auch nach ihrem Auftreten fortzufahren. Ein Beispiel für einen Fehler ohne Abbruch ist die Verwendung des Cmdlets `Get-ChildItem` mit einem Pfad, der nicht vorhanden ist. PowerShell erkennt, dass der Pfad nicht vorhanden ist, löst einen Fehler aus und fährt mit dem nächsten Ordner fort. Dieser Fehler würde den Runbookstatus nicht auf **Fehler** setzen und könnte als **Abgeschlossen** markiert werden. Um ein Runbook zu veranlassen, bei einem Fehler ohne Abbruch anzuhalten, können Sie `-ErrorAction Stop` für das Cmdlet verwenden.

| Status | BESCHREIBUNG |
|:--- |:--- |
| Abgeschlossen |Der Auftrag wurde erfolgreich abgeschlossen. |
| Fehler |Bei [grafischen und PowerShell-Workflow-Runbooks](automation-runbook-types.md)ist die Kompilierung des Runbooks fehlgeschlagen. Bei [PowerShell-Skript-Runbooks](automation-runbook-types.md) konnte das Runbook nicht gestartet werden, oder bei dem Auftrag ist eine Ausnahme aufgetreten. |
| Fehler, auf Ressourcen wird gewartet |Beim Auftrag ist ein Fehler aufgetreten, da der Auftrag dreimal den Grenzwert für die [gleichmäßige Verteilung](#fair-share) erreicht hat und jedes Mal vom gleichen Prüfpunkt oder vom Anfang des Runbooks gestartet wurde. |
| In Warteschlange |Der Auftrag wartet darauf, dass Ressourcen für einen Automation-Worker verfügbar werden, damit er gestartet werden kann. |
| Wird gestartet |Der Auftrag wurde einem Worker zugewiesen, und das System startet ihn. |
| Wird fortgesetzt |Das System setzt den Auftrag fort, nachdem er angehalten wurde. |
| Wird ausgeführt |Der Auftrag wird ausgeführt. |
| Wird ausgeführt, auf Ressourcen wird gewartet |Der Auftrag wurde entladen, da er den Grenzwert für die [gleichmäßige Verteilung](#fair-share) erreicht hat. Er wird in Kürze vom letzten Prüfpunkt wiederaufgenommen. |
| Beendet |Der Auftrag wurde vom Benutzer beendet, bevor er abgeschlossen wurde. |
| Wird beendet |Das System beendet den Auftrag. |
| Ausgesetzt |Der Auftrag wurde vom Benutzer, vom System oder von einem Befehl im Runbook angehalten. Wenn ein Runbook keinen Prüfpunkt aufweist, wird der Vorgang am Anfang des Runbooks gestartet. Wenn ein Prüfpunkt vorhanden ist, kann es erneut starten und den Vorgang bei seinem letzten Prüfpunkt fortsetzen. Das Runbook wird nur vom System angehalten, wenn eine Ausnahme auftritt. Standardmäßig ist ErrorActionPreference auf **Continue** festgelegt. Dies bedeutet, dass der Auftrag bei einem Fehler weiterhin ausgeführt wird. Wenn diese Einstellungsvariable auf **Stop** festgelegt wird, wird der Auftrag bei einem Fehler angehalten. Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md) . |
| Wird angehalten |Das System versucht, den Auftrag auf Anforderung des Benutzers anzuhalten. Das Runbook muss den nächsten Prüfpunkt erreichen, bevor es angehalten werden kann. Wenn der letzte Prüfpunkt bereits verstrichen ist, wird das Runbook abgeschlossen, bevor es angehalten werden kann. Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md) . |

## <a name="viewing-job-status-from-the-azure-portal"></a>Anzeigen des Auftragsstatus im Azure-Portal

Sie können einen zusammengefassten Status aller Runbookaufträge anzeigen oder im Azure-Portal weitere Details zu einem bestimmten Runbookauftrag aufführen. Sie können auch die Integration mit Ihrem Log Analytics-Arbeitsbereich konfigurieren, um den Status von Runbookaufträgen und Auftragsdatenströmen weiterzuleiten. Weitere Informationen zur Integration in Log Analytics finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Automation-Runbookaufträge als Zusammenfassung

Rechts in Ihrem ausgewählten Automation-Konto sehen Sie auf der Kachel **Auftragsstatistik** die Zusammenfassung aller Runbookaufträge.

![Die Kachel „Auftragsstatistik“](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Auf dieser Kachel wird die Anzahl aller ausgeführten Jobs neben einer grafischen Darstellung des Auftragsstatus angezeigt.

Wenn Sie auf die Kachel klicken, wird die Seite **Aufträge** angezeigt, die eine zusammengefasste Liste aller ausgeführten Aufträge enthält. Auf dieser Seite werden Status, Start- und Abschlusszeiten angezeigt.

![Die Seite „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Sie können die Auftragsliste filtern, indem Sie die Option **Filteraufträge** auswählen. Aufträge lassen sich zum Durchsuchen nach bestimmten Runbooks, dem Auftragsstatus oder, über die Dropdownliste und den Uhrzeitbereich filtern.

![Status von Filteraufträgen](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternativ können Sie für ein bestimmtes Runbook eine Übersicht anzeigen, indem Sie in Ihrem Automation-Konto zuerst auf die Seite **Runbooks** das Runbook und dann die Kachel **Aufträge** auswählen. Diese Aktion stellt die Seite **Aufträge** dar. Wenn Sie dort auf einen Auftragsdatensatz klicken, werden Details und die Ausgabe angezeigt.

![Die Seite „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>API-Zusammenfassung

Sie können eine Liste aller Aufträge, die für ein bestimmtes Runbook erstellt wurden, sowie deren aktuellen Status anzeigen. Sie können diese Liste nach Auftragsstatus und dem Datumsbereich für die letzte Änderung des Auftrags filtern. Um ausführliche Informationen und die Auftragsausgabe anzuzeigen, klicken Sie auf den Namen eines Auftrags. Die Detailansicht des Auftrags enthält die Werte für die Runbookparameter, die für diesen Auftrag bereitgestellt wurden.

Zeigen Sie die Aufträge für ein Runbook mithilfe der folgenden Schritte an.

1. Wählen Sie im Azure-Portal die Option **Automation** aus, und wählen Sie anschließend den Namen eines Automation-Kontos aus.
2. Wählen Sie im Hub die Option **Runbooks** aus, und wählen Sie dann auf der Seite **Runbooks** ein Runbook in der Liste aus.
3. Klicken Sie auf der Seite für das ausgewählte Runbook auf die Kachel **Aufträge**.
4. Wenn Sie auf einen der Aufträge in der Liste klicken, können Sie auf der Seite für die Runbook-Auftragsdetails die Details und die Ausgabe anzeigen.

## <a name="retrieving-job-status-using-windows-powershell"></a>Abrufen des Auftragsstatus mithilfe von Windows PowerShell

Sie können [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) verwenden, um die für ein Runbook erstellten Aufträge und die Details zu einem bestimmten Auftrag anzuzeigen. Wenn Sie ein Runbook über Windows PowerShell mithilfe des Befehls [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook)starten, wird der resultierende Auftrag zurückgegeben. Verwenden Sie [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput), um die Ausgabe eines Auftrags abzurufen.

Die folgenden Beispielbefehle rufen den letzten Auftrag für ein Beispielrunbook ab und zeigen seinen Status, die für die Runbookparameter bereitgestellten Werte und die Ausgabe des Auftrags an.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Das folgende Beispiel ruft die Ausgabe für einen bestimmten Auftrag ab und gibt die einzelnen Datensätze zurück. Sollte für einen der Datensätze eine Ausnahme aufgetreten sein, wird anstelle des Werts die Ausnahme ausgegeben. Dieses Verhalten ist hilfreich, da Ausnahmen zusätzliche Informationen liefern können, die bei der Ausgabe normalerweise nicht protokolliert werden.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Abrufen von Details aus Aktivitätsprotokoll

Weitere Details wie die Person oder das Konto, das das Runbook gestartet hat, können aus dem Aktivitätsprotokoll des Automation-Kontos abgerufen werden. Das folgende PowerShell-Beispiel gibt den letzten Benutzer an, um das betreffende Runbook auszuführen:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>gleichmäßige Verteilung

Damit Ressourcen von allen Runbooks in der Cloud verwendet werden können, beendet oder entlädt Azure Automation jeden Auftrag vorübergehend, der für mehr als drei Stunden ausgeführt wurde. Aufträge für [PowerShell-basierte Runbooks](automation-runbook-types.md#powershell-runbooks) und [Python-Runbooks](automation-runbook-types.md#python-runbooks) werden beendet und nicht neu gestartet, und der Auftragsstatus zeigt „Beendet“.

Für Aufgaben mit langer Ausführungszeit wird empfohlen, einen [Hybrid Runbook Worker](automation-hrw-run-runbooks.md#job-behavior) zu verwenden. Hybrid Runbook Worker unterliegen nicht der gleichmäßigen Verteilung und keiner Einschränkung hinsichtlich der Ausführungszeit eines Runbooks. Die anderen [Grenzwerte](../azure-subscription-service-limits.md#automation-limits) für Aufträge gelten sowohl für Azure-Sandboxes als auch für Hybrid Runbook Workers. Während Hybrid Runbook Worker nicht durch die 3-stündige Begrenzung für die gleichmäßige Verteilung begrenzt sind, sollten Runbooks, die auf Hybrid Runbook Workers laufen, weiterhin entwickelt werden, um das Neustartverhalten bei unerwarteten lokalen Infrastrukturproblemen zu unterstützen.

Eine weitere Möglichkeit ist das Optimieren des Runbooks durch die Verwendung von untergeordneten Runbooks. Wenn Ihr Runbook für mehrere Ressourcen eine Schleife durch die gleiche Funktion durchführt, z. B. für einen Datenbankvorgang in mehreren Datenbanken, können Sie diese Funktion in ein [untergeordnetes Runbook](automation-child-runbooks.md) verschieben und mit dem Cmdlet [Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) aufrufen. Jedes dieser untergeordneten Runbooks wird parallel in separaten Prozessen ausgeführt, sodass die Gesamtdauer für die Ausführung des übergeordneten Runbooks verringert wird. Mit dem Cmdlet [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) in Ihrem Runbook können Sie den Auftragsstatus für jedes untergeordnete Element überprüfen, wenn Vorgänge vorhanden sind, die nach Abschluss des untergeordneten Runbooks durchgeführt werden müssen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verschiedenen Methoden, die zum Starten eines Runbooks in Azure Automation verwendet werden können, finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).

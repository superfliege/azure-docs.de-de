---
title: Zeitpläne in Azure Automation
description: Automation-Zeitpläne werden verwendet, um die automatische Ausführung von Runbooks in Azure Automation zu planen. Beschreibt die Erstellung und Verwaltung eines Zeitplans, sodass ein Runbook automatisch zu einer bestimmten Uhrzeit oder nach einem sich wiederholenden Zeitplan gestartet wird.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3d8492d2a8982c9c85bfc91867f7eb6c2da04e58
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294763"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Planen eines Runbooks in Azure Automation

Um ein Runbook in Azure-Automation für die Ausführung zu einer bestimmten Uhrzeit zu planen, müssen Sie es mit einem oder mehreren Zeitplänen verknüpfen. Ein Zeitplan kann so konfiguriert werden, dass er entweder einmalig oder nach einem sich wiederholenden stündlichen oder täglichen Zeitplan für Runbooks im Azure-Portal ausgeführt wird. Sie können Zeitpläne auch wöchentlich, monatlich, für bestimmte Wochentage oder Tage des Monats oder einen bestimmten Tag des Monats festlegen. Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein.

> [!NOTE]
> Zeitpläne unterstützen derzeit keine Azure Automation DSC-Konfigurationen.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten von Zeitplänen mit Windows PowerShell in Azure Automation verwendet. Sie sind im [Azure PowerShell-Modul](/powershell/azure/overview)enthalten.

| Cmdlets | BESCHREIBUNG |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Ruft einen Zeitplan ab. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Erstellt einen neuen Zeitplan. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Entfernt einen Zeitplan. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Legt die Eigenschaften für einen vorhandenen Zeitplan fest. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Ruft geplante Runbooks ab. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Ordnet ein Runbook einem Zeitplan zu. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Hebt die Zuordnung eines Runbook zu einem Zeitplan auf. |

## <a name="creating-a-schedule"></a>Erstellen eines Zeitplans

Sie können einen neuen Zeitplan für Runbooks im Azure-Portal oder mit Windows PowerShell erstellen.

> [!NOTE]
> In Azure Automation werden die neuesten Module in Ihrem Automation-Konto verwendet, wenn ein neuer geplanter Auftrag ausgeführt wird.  Zum Vermeiden von Auswirkungen auf Ihre Runbooks und die Prozesse, die Sie automatisieren, testen Sie zuerst alle Runbooks mit verknüpften Zeitplänen mit einem für Testzwecke vorgesehenen Automation-Konto.  Dadurch wird überprüft, ob Ihre geplanten Runbooks weiterhin einwandfrei funktionieren. Sollte dies nicht der Fall sein, können Sie alle zur Problembehandlung erforderlichen Änderungen vornehmen, bevor Sie die aktualisierte Runbookversion zur Produktion migrieren.
> Ihr Automation-Konto erhält nicht automatisch die neuen Versionen der Module. Dazu müssen Sie sie manuell aktualisieren, indem Sie unter **Module** die Option [Azure-Module aktualisieren](automation-update-azure-modules.md) auswählen.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>So erstellen Sie einen neuen Zeitplan im Azure-Portal

1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Freigegebene Ressourcen** links auf **Zeitpläne**.
1. Klicken Sie oben auf der Seite auf **Zeitplan hinzufügen**.
1. Geben Sie im Bereich **Neuer Zeitplan** einen Wert für **Name** und optional eine **Beschreibung** für den neuen Zeitplan ein.
1. Wählen Sie aus, ob der Zeitplan einmalig oder wiederholt ausgeführt werden soll, indem Sie **Einmalig** oder **Wiederholt** auswählen. Geben Sie bei Auswahl von **Einmalig** eine **Startzeit** an, und klicken Sie auf **Erstellen**. Geben Sie bei Auswahl von **Wiederholt** eine **Startzeit** an, und wählen Sie für das **Wiederholungsintervall** die Häufigkeit für die wiederholte Ausführung des Runbooks aus: **Stunde**, **Tag**, **Woche** oder **Monat**.
    1. Wenn Sie **Woche** auswählen, wird eine Liste der zur Auswahl stehenden Wochentage angezeigt. Wählen Sie beliebig viele Tage aus. Die erste Ausführung Ihres Zeitplans erfolgt am ersten Tag, den Sie nach der Startzeit ausgewählt haben.
    2. Bei Auswahl von **Monat** werden verschiedene Optionen angezeigt. Wählen Sie für die Option **Monatliche Vorkommen** entweder **Tage im Monat** oder **Wochentage** aus. Bei Auswahl von **Tage im Monat** wird ein Kalender angezeigt, in dem Sie beliebig viele Tage auswählen können. Wenn Sie einen Tag wie den 31. auswählen, der im aktuellen Monat nicht vorkommt, wird der Zeitplan nicht ausgeführt. Wenn der Zeitplan am letzten Tag ausgeführt werden soll, wählen Sie unter **Am letzten Tag des Monats ausführen** die Option **Ja** aus. Bei Auswahl von **Wochentage** wird die Option **Wiederholen alle** angezeigt. Wählen Sie **Erster**, **Zweiter**, **Dritter**, **Vierter** oder **Letzter** aus. Wählen Sie schließlich einen Tag für die Wiederholung aus.
1. Klicken Sie abschließend auf **Erstellen**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>So erstellen Sie einen neuen Zeitplan mit Windows PowerShell

Verwenden Sie das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule), um Zeitpläne zu erstellen. Geben Sie die Startzeit für den Zeitplan und die Häufigkeit der Ausführung an.

Mit den folgenden Beispielbefehlen wird veranschaulicht, wie Sie einen Zeitplan für den 15. und 30. jedes Monats mit einem Azure Resource Manager-Cmdlet erstellen.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Verknüpfen eines Zeitplans mit einem Runbook

Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein. Wenn ein Runbook über Parameter verfügt, können Sie Werte für diese bereitstellen. Sie müssen Werte für alle verbindlichen Parameter angeben; für optionale Parameter können Sie Werte angeben. Diese Werte werden jedes Mal verwendet, wenn das Runbook durch diesen Zeitplan gestartet wird. Sie können das gleiche Runbook einem anderen Zeitplan zuordnen und dabei andere Parameterwerte festlegen.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit dem klassischen Azure-Portal

1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Prozessautomatisierung** links auf **Runbooks**.
2. Klicken Sie auf den Namen des Runbooks, das Sie mit einem Zeitplan verknüpfen möchten.
3. Wenn das Runbook gegenwärtig nicht mit einem Zeitplan verknüpft ist, stehen Ihnen die Optionen zum Erstellen eines neuen Zeitplans oder Verknüpfen mit einem vorhandenen Zeitplan zur Verfügung.
4. Falls das Runbook über Parameter verfügt, können Sie die Option **Ausführungseinstellungen ändern (Standard: Azure)** wählen. Der Bereich **Parameter** wird angezeigt, in dem Sie die Informationen entsprechend eingeben können.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit Windows PowerShell

Mit dem Cmdlet [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) können Sie einen Zeitplan verknüpfen. Sie können Werte für die Runbookparameter im Parameter "Parameters" angeben. Weitere Informationen zum Angeben von Parameterwerten finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan mit einem Runbook verknüpfen, indem Sie ein Azure Resource Manager-Cmdlet mit Parametern verwenden.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Häufigeres Planen von Runbooks

Das am häufigsten auftretende Intervall, mit dem ein Zeitplan in Azure Automation konfiguriert werden kann, beträgt eine Stunde. Wenn Sie Zeitpläne häufiger ausführen müssen, stehen Ihnen zwei Optionen zur Verfügung:

* Erstellen Sie einen [Webhook](automation-webhooks.md) für das Runbook, und rufen Sie mit dem [Azure Scheduler](../scheduler/scheduler-get-started-portal.md) den Webhook auf. Der Azure Scheduler bietet eine differenziertere Granularität bei der Definition eines Zeitplans.

* Erstellen Sie vier Zeitpläne, die einmal pro Stunde alle mit einem Abstand von 15 Minuten voneinander beginnen. In diesem Szenario kann das Runbook alle 15 Minuten mit unterschiedlichen Zeitplänen ausgeführt werden.

## <a name="disabling-a-schedule"></a>Deaktivieren eines Zeitplans

Wenn Sie einen Zeitplan deaktivieren, werden sämtliche damit verknüpfte Runbooks nicht mehr nach diesem Zeitplan ausgeführt. Sie können einen Zeitplan manuell deaktivieren oder während der Erstellung für Zeitpläne eine Ablaufzeit festlegen. Wenn der Ablaufzeitpunkt erreicht ist, wird der Zeitplan deaktiviert.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>So deaktivieren Sie einen Zeitplan über das Azure-Portal

1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Freigegebene Ressourcen** links auf **Zeitpläne**.
1. Klicken Sie auf den Namen eines Zeitplans, um den Bereich mit den Details zu öffnen.
1. Ändern Sie **Aktiviert** in **Nein**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>So deaktivieren Sie einen Zeitplan mit Windows PowerShell

Mit dem Cmdlet [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) können Sie die Eigenschaften eines vorhandenen Zeitplans ändern. Um den Zeitplan zu deaktivieren, legen Sie für den Parameter **IsEnabled** den Wert **false** fest.

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan für ein Runbook deaktivieren, indem Sie ein Azure Resource Manager-Cmdlet verwenden.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den ersten Schritten mit Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md)

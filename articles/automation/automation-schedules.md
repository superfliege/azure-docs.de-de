---
title: Zeitpläne in Azure Automation
description: Automation-Zeitpläne werden verwendet, um die automatische Ausführung von Runbooks in Azure Automation zu planen. Beschreibt die Erstellung und Verwaltung eines Zeitplans, sodass ein Runbook automatisch zu einer bestimmten Uhrzeit oder nach einem sich wiederholenden Zeitplan gestartet wird.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ae7378c6d0f85e71c9522a6678adcb1f2e4b01f1
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Planen eines Runbooks in Azure Automation
Um ein Runbook in Azure-Automation für die Ausführung zu einer bestimmten Uhrzeit zu planen, müssen Sie es mit einem oder mehreren Zeitplänen verknüpfen. Ein Zeitplan kann so konfiguriert werden, dass er entweder einmalig oder nach einem sich wiederholenden stündlichen oder täglichen Zeitplan für Runbooks im Azure-Portal ausgeführt wird. Sie können Zeitpläne auch wöchentlich, monatlich, für bestimmte Wochentage oder Tage des Monats oder einen bestimmten Tag des Monats festlegen.  Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein.

> [!NOTE]
> Zeitpläne unterstützen derzeit keine Azure Automation DSC-Konfigurationen.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets
Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten von Zeitplänen mit Windows PowerShell in Azure Automation verwendet. Sie sind im [Azure PowerShell-Modul](/powershell/azure/overview)enthalten.

| Cmdlets | BESCHREIBUNG |
|:--- |:--- |
| **Azure Resource Manager-Cmdlets** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Ruft einen Zeitplan ab. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Erstellt einen neuen Zeitplan. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Entfernt einen Zeitplan. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Legt die Eigenschaften für einen vorhandenen Zeitplan fest. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Ruft geplante Runbooks ab. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Ordnet ein Runbook einem Zeitplan zu. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Hebt die Zuordnung eines Runbook zu einem Zeitplan auf. |
| **Azure-Dienstverwaltungs-Cmdlets** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Ruft einen Zeitplan ab. |
| [New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Erstellt einen neuen Zeitplan. |
| [Remove-AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Entfernt einen Zeitplan. |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Legt die Eigenschaften für einen vorhandenen Zeitplan fest. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Ruft geplante Runbooks ab. |
| [Register-AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Ordnet ein Runbook einem Zeitplan zu. |
| [Unregister-AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Hebt die Zuordnung eines Runbook zu einem Zeitplan auf. |

## <a name="creating-a-schedule"></a>Erstellen eines Zeitplans
Sie können einen neuen Zeitplan für Runbooks im Azure-Portal oder mit Windows PowerShell erstellen. Außerdem haben Sie die Möglichkeit, einen neuen Zeitplan zu erstellen, wenn Sie ein Runbook über das klassische Azure-Portal oder das Azure-Portal mit einem Zeitplan verknüpfen.

> [!NOTE]
> In Azure Automation werden die neuesten Module in Ihrem Automation-Konto verwendet, wenn ein neuer geplanter Auftrag ausgeführt wird.  Zum Vermeiden von Auswirkungen auf Ihre Runbooks und die Prozesse, die Sie automatisieren, testen Sie zuerst alle Runbooks mit verknüpften Zeitplänen mit einem für Testzwecke vorgesehenen Automation-Konto.  Dadurch wird überprüft, ob Ihre geplanten Runbooks weiterhin einwandfrei funktionieren. Sollte dies nicht der Fall sein, können Sie alle zur Problembehandlung erforderlichen Änderungen vornehmen, bevor Sie die aktualisierte Runbookversion zur Produktion migrieren.  
>  Ihr Automation-Konto erhält nicht automatisch die neuen Versionen der Module. Dazu müssen Sie sie manuell aktualisieren, indem Sie unter **Module** die Option [Aktualisieren von Azure-Modulen](automation-update-azure-modules.md) auswählen. 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>So erstellen Sie einen neuen Zeitplan im Azure-Portal
1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Freigegebene Ressourcen** links auf **Zeitpläne**. 
2. Klicken Sie oben auf der Seite auf **Zeitplan hinzufügen**.
4. Geben Sie im Bereich **Neuer Zeitplan** einen Wert für **Name** und optional eine **Beschreibung** für den neuen Zeitplan ein.
5. Wählen Sie aus, ob der Zeitplan einmalig oder nach einem Zeitplan häufiger ausgeführt werden soll, indem Sie **Einmalig** oder **Wiederholung** angeben.  Geben Sie bei Auswahl von **Einmalig** eine **Startzeit** an, und klicken Sie auf **Erstellen**.  Geben Sie bei Auswahl von **Wiederholung** eine **Startzeit** und den Wert an, der festlegt, wie oft die Ausführung des Runbooks wiederholt werden soll: **Stunde**, **Tag**, **Woche** oder **Monat**.  Wenn Sie in der Dropdownliste die Option **Woche** oder **Monat** auswählen, wird im Bereich die Option **Wiederholung** angezeigt. Bei Auswahl dieser Option wird der Bereich mit der Option **Wiederholung** angezeigt, und Sie können den Wochentag auswählen, wenn Sie zuvor **Woche** gewählt haben.  Falls Sie **Monat** ausgewählt haben, können Sie **Wochentage** oder bestimmte Tage des Monats im Kalender auswählen. Außerdem können Sie angeben, ob die Ausführung am letzten Tag des Monats durchgeführt werden soll. Klicken Sie anschließend auf **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>So erstellen Sie einen neuen Zeitplan mit Windows PowerShell
Sie können das [New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0)-Cmdlet verwenden, um in Azure Automation einen neuen Zeitplan für klassische Runbooks zu erstellen, oder das [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule)-Cmdlet für Runbooks im Azure-Portal. Sie müssen die Startzeit für den Zeitplan und die Häufigkeit der Ausführung angeben.

Mit den folgenden Beispielbefehlen wird veranschaulicht, wie Sie einen Zeitplan für den 15. und 30. jedes Monats mit einem Azure Resource Manager-Cmdlet erstellen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Die folgenden Beispielbefehle zeigen, wie ein neuer Zeitplan erstellt wird, der ab 20. Januar 2015 jeden Tag um 15:30 mit einem Azure-Dienstverwaltungs-Cmdlet ausgeführt wird.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Verknüpfen eines Zeitplans mit einem Runbook
Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein. Wenn ein Runbook über Parameter verfügt, können Sie Werte für diese bereitstellen. Sie müssen Werte für alle verbindlichen Parameter angeben; für optionale Parameter können Sie Werte angeben.  Diese Werte werden jedes Mal verwendet, wenn das Runbook durch diesen Zeitplan gestartet wird.  Sie können das gleiche Runbook einem anderen Zeitplan zuordnen und dabei andere Parameterwerte festlegen.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit dem klassischen Azure-Portal
1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Prozessautomatisierung** links auf **Runbooks**.
2. Klicken Sie auf den Namen des Runbooks, das Sie mit einem Zeitplan verknüpfen möchten.
3. Wenn das Runbook gegenwärtig nicht mit einem Zeitplan verknüpft ist, stehen Ihnen die Optionen zum Erstellen eines neuen Zeitplans oder Verknüpfen mit einem vorhandenen Zeitplan zur Verfügung.  
4. Falls das Runbook über Parameter verfügt, können Sie die Option **Ausführungseinstellungen ändern (Standard: Azure)** wählen. Der Bereich **Parameter** wird angezeigt, in dem Sie die Informationen entsprechend eingeben können.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit Windows PowerShell
Sie können [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) verwenden, um einen Zeitplan mit einem klassischen Runbook zu verknüpfen, oder das [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook)-Cmdlet für Runbooks im Azure-Portal.  Sie können Werte für die Runbookparameter im Parameter "Parameters" angeben. Weitere Informationen zum Angeben von Parameterwerten finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) .

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan mit einem Runbook verknüpfen, indem Sie ein Azure Resource Manager-Cmdlet mit Parametern verwenden.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan verknüpfen, indem Sie ein Azure-Dienstverwaltungs-Cmdlet mit Parametern verwenden.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Deaktivieren eines Zeitplans
Wenn Sie einen Zeitplan deaktivieren, werden sämtliche damit verknüpften Runbooks nicht mehr nach diesem Zeitplan ausgeführt. Sie können einen Zeitplan manuell deaktivieren oder während der Erstellung für Zeitpläne eine Ablaufzeit festlegen. Wenn der Ablaufzeitpunkt erreicht ist, wird der Zeitplan deaktiviert.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>So deaktivieren Sie einen Zeitplan über das Azure-Portal
1. Klicken Sie im Azure-Portal in Ihrem Automation-Konto im Abschnitt **Freigegebene Ressourcen** links auf **Zeitpläne**.
2. Klicken Sie auf den Namen eines Zeitplans, um den Bereich mit den Details zu öffnen.
3. Ändern Sie **Aktiviert** in **Nein**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>So deaktivieren Sie einen Zeitplan mit Windows PowerShell
Sie können das [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx)-Cmdlet verwenden, um die Eigenschaften eines vorhandenen Zeitplans für ein klassisches Runbook zu ändern, oder das [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule)-Cmdlet für Runbooks im Azure-Portal. Um den Zeitplan zu deaktivieren, legen Sie für den Parameter **IsEnabled** den Wert **false** fest.

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan für ein Runbook deaktivieren, indem Sie ein Azure Resource Manager-Cmdlet verwenden.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan deaktivieren, indem Sie das Azure-Dienstverwaltungs-Cmdlet verwenden.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den ersten Schritten mit Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) 


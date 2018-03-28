---
title: Mein erstes PowerShell-Workflowrunbook in Azure Automation
description: Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen Textrunbooks unter Verwendung eines PowerShell-Workflows vertraut machen können.
keywords: PowerShell-Workflow, Beispiele für Powershell-Workflows, Workflow PowerShell
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: 61191e78ebd2f0d7a960dfb0c74a1d8260331212
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="my-first-powershell-workflow-runbook"></a>Mein erstes PowerShell-Workflow-Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

Dieses Tutorial führt Sie durch die Erstellung eines [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) in Azure Automation. Sie beginnen mit einem einfachen Runbook, das Sie testen und veröffentlichen. Dabei wird erläutert, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend wird das Runbook geändert, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Abschließend fügen Sie Runbookparameter hinzu, um die Stabilität des Runbooks zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen.  Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, sollte es sich nicht um einen virtuellen Computer in der Produktionsumgebung handeln.

## <a name="step-1---create-new-runbook"></a>Schritt 1: Erstellen eines neuen Runbooks
Erstellen Sie zunächst ein einfaches Runbook, das den Text *Hello World* ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.

   Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites)erwähnte Anmeldeinformationsobjekt befinden.

1. Klicken Sie unter **Prozessautomatisierung** auf **Runbooks**, um die Liste mit den Runbooks zu öffnen.
2. Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **+ Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen** klicken.
3. Nennen Sie das Runbook *MyFirstRunbook-Workflow*.
4. Da Sie hier ein [PowerShell-Workflowrunbook](automation-runbook-types.md#powershell-workflow-runbooks) erstellen, wählen Sie als **Runbooktyp** die Option **PowerShell-Workflow** aus.
5. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="step-2---add-code-to-the-runbook"></a>Schritt 2 – Hinzufügen von Code zum Runbook
Sie können entweder direkt Code in das Runbook eingeben, oder Sie wählen Cmdlets, Runbooks und Objekte aus dem Bibliotheksteuerelement aus und fügen diese mit entsprechenden Parametern zum Runbook hinzu. In dieser exemplarischen Vorgehensweise geben Sie den Code direkt in das Runbook ein.

1. Ihr Runbook ist zurzeit leer und enthält lediglich das erforderliche Schlüsselwort *workflow*, den Namen des Runbooks und die geschweiften Klammern, die den gesamten Workflow umschließen.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Geben Sie zwischen den geschweiften Klammern *Write-Output "Hello World."* ein.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="step-3---test-the-runbook"></a>Schritt 3: Testen des Runbooks
Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie sich vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die **Entwurfsversion** des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich** , um den Testbereich zu öffnen.
2. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
3. Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.

   Der Auftrag besitzt zunächst den Status *In der Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  

1. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In Ihrem Fall sollte *Hello World* angezeigt werden.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
2. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks
Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Sie müssen das Runbook veröffentlichen, um es in der Produktionsumgebung ausführen zu können. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.
2. Wenn Sie nun nach links scrollen, um das Runbook im Bereich **Runbooks** anzuzeigen, wird der **Erstellungsstatus** des Runbooks als **Veröffentlicht** angezeigt.
3. Führen Sie wieder einen Bildlauf nach rechts durch, um den Bereich für **MyFirstRunbook-Workflow**anzuzeigen.  
   Mit den Optionen am oberen Rand können wir das Runbook starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um den Start über einen HTTP-Aufruf zu ermöglichen.
4. Sie möchten das Runbook einfach starten. Klicken Sie daher auf **Starten** und anschließend auf **Ja**.<br><br> ![Runbook starten](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Dieser Bereich kann zwar geschlossen werden, lassen Sie ihn in diesem Fall aber geöffnet, um den Status des Auftrags verfolgen zu können.
6. Der Auftragsstatus wird unter **Auftragszusammenfassung** angezeigt und entspricht den Statusoptionen, die Sie bereits beim Testen des Runbooks gesehen haben.<br><br> ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Wenn der Runbookstatus *Abgeschlossen*lautet, klicken Sie auf **Ausgabe**. Der Ausgabebereich wird geöffnet, und der Text *Hello World* wird angezeigt.<br><br> ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Schließen Sie den Ausgabebereich.
9. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabedatenstrom sollte nur *Hello World* angezeigt werden. Hier können aber auch andere Datenströme für einen Runbookauftrag (wie etwa „Ausführlich“ und „Fehler“) angezeigt werden, sofern das Runbook Schreibvorgänge dafür durchführt.<br><br> ![API-Zusammenfassung](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich „MyFirstRunbook“ zurückzukehren.
11. Klicken Sie auf **Aufträge** , um den Auftragsbereich für dieses Runbook zu öffnen. Dadurch werden alle von diesem Runbook erstellten Aufträge aufgeführt. Hier ist nur ein einzelner Auftrag aufgeführt, da Sie den Auftrag bislang erst einmal ausgeführt haben.<br><br> ![Aufträge](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den Sie sich beim Starten des Runbooks angesehen haben. So können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Schritt 5: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen
Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Sie möchten damit ja eigentlich Azure-Ressourcen verwalten. Dazu ist jedoch eine Authentifizierung mit den Anmeldeinformationen erforderlich, die in den [Voraussetzungen](#prerequisites)genannt sind. Zu diesem Zweck verwenden Sie das Cmdlet **Add-AzureRMAccount**.

1. Öffnen Sie den Text-Editor, indem Sie im Bereich „MyFirstRunbook-Workflow“ auf **Bearbeiten** klicken.
2. Die Zeile **Write-Output** wird nicht mehr benötigt, also löschen Sie sie.
3. Positionieren Sie den Cursor in einer leeren Zeile zwischen den geschweiften Klammern.
4. Geben Sie den folgenden Code ein (oder fügen Sie ihn ein). Dieser Code ist für die Authentifizierung bei Ihrem ausführenden Konto für Automation zuständig:

   ```powershell-interactive
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Klicken Sie auf den **Testbereich**, um das Runbook zu testen.
6. Klicken Sie auf **Starten** , um den Test zu starten. Nach Abschluss des Tests erhalten Sie in der Regel eine Ausgabe wie in der Abbildung unten mit allgemeinen Informationen aus Ihrem Konto. Auf diese Weise wird bestätigt, dass die Anmeldeinformationen gültig sind.<br><br> ![Authentifizieren](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Schritt 6: Hinzufügen von Code zum Starten eines virtuellen Computers
Nachdem das Runbook jetzt in Ihrem Azure-Abonnement authentifiziert ist, können Sie Ressourcen verwalten. Sie fügen einen Befehl zum Starten eines virtuellen Computers hinzu. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen. Sie werden den Namen vorerst im Runbook hartcodieren.

1. Geben Sie nach *Add-AzureRmAccount* den Code *Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'* ein, und geben Sie den Namen und den Ressourcengruppennamen des virtuellen Computers an, der gestartet werden soll.  

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Speichern Sie das Runbook, und klicken Sie dann auf den **Testbereich**, um es zu testen.
3. Klicken Sie auf **Starten** , um den Test zu starten. Vergewissern Sie sich nach Abschluss des Tests, dass der virtuelle Computer gestartet wurde.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Schritt 7: Hinzufügen eines Eingabeparameters zum Runbook
Ihr Runbook startet zwar jetzt den virtuellen Computer, den Sie im Runbook hartcodiert haben, es wäre aber praktischer, wenn Sie den virtuellen Computer beim Start des Runbooks angeben könnten. Zu diesem Zweck fügen Sie dem Runbook Eingabeparameter hinzu.

1. Fügen Sie dem Runbook Parameter für *VMName* und *ResourceGroupName* hinzu, und verwenden Sie diese Variablen mit dem Cmdlet **Start-AzureRmVM**, wie im folgenden Beispiel gezeigt.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Sie können nun Werte für die beiden Eingabevariablen angeben, die sich im Testbereich befinden.
3. Schließen Sie den Testbereich.
4. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
5. Beenden Sie den virtuellen Computer, den Sie im vorherigen Schritt gestartet haben.
6. Klicken Sie auf **Starten** , um das Runbook zu starten. Geben Sie **VMName** und **ResourceGroupName** für den virtuellen Computer ein, den Sie starten möchten.<br><br> ![Start Runbook](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Vergewissern Sie sich nach Abschluss des Runbooks, dass der virtuelle Computer gestartet wurde.  

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md)
* Erste Schritte mit PowerShell-Runbooks werden in [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md)
* Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
* Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

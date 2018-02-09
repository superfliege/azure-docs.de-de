---
title: Planen von Azure-SSIS-Integration Runtimes | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Planung des Startens und Beendens einer Azure-SSIS-Integration Runtime mithilfe von Azure Automation und Data Factory.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 01/25/2018
ms.author: spelluru
ms.openlocfilehash: 60a4afdb8a78cffdc7eb1ee82c7daf3b06e5fe15
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-schedule-starting-and-stopping-of-an-azure-ssis-integration-runtime"></a>Planen des Startens und Beendens einer Azure SSIS-Integration Runtime 
Das Ausführen einer Azure SSIS (SQL Server Integration Services)-Integration Runtime (IR) ist mit einer Gebühr verbunden. Sie sollten die IR daher nur ausführen, wenn Sie SSIS-Pakete in Azure ausführen müssen, und sie andernfalls beenden. Sie können die Data Factory-Benutzeroberfläche oder Azure PowerShell zum [manuellen Starten oder Beenden einer Azure SSIS-IR](manage-azure-ssis-integration-runtime.md) verwenden. Dieser Artikel beschreibt die Planung des Startens und Beendens einer Azure-SSIS-Integration Runtime (IR) mithilfe von Azure Automation und Azure Data Factory. Folgende allgemeine Schritte werden in diesem Artikel beschrieben:

1. **Erstellen und Testen eines Azure Automation-Runbooks.** In diesem Schritt erstellen Sie ein PowerShell-Runbook mit dem Skript, das eine Azure SSIS-IR startet oder beendet. Anschließend testen Sie das Runbook in Start- und Stoppszenarien und bestätigen, dass IR gestartet oder beendet wird. 
2. **Erstellen von zwei Zeitplänen für das Runbook.** Für den ersten Zeitplan konfigurieren Sie das Runbook mit „Start“ als Vorgang. Für den zweiten Zeitplan konfigurieren Sie das Runbook mit „Stopp“ als Vorgang. Geben Sie für beide Zeitpläne den Rhythmus an, in dem das Runbook ausgeführt werden soll. Beispielsweise empfiehlt es sich, das erste für die tägliche Ausführung um 8 Uhr und das zweite für die tägliche Ausführung um 23 Uhr zu planen. Wenn das erste Runbook ausgeführt wird, startet es die Azure SSIS-IR. Wenn das zweite Runbook ausgeführt wird, beendet es die Azure SSIS-IR. 
3. **Erstellen Sie zwei Webhooks für das Runbook** – einen für den Startvorgang und den anderen für den Stoppvorgang. Sie verwenden die URLs dieser Webhooks beim Konfigurieren von Webaktivitäten in einer Data Factory-Pipeline. 
4. **Erstellen einer Data Factory-Pipeline.** Die erstellte Pipeline umfasst vier Aktivitäten. Die erste **Web**-Aktivität ruft den ersten Webhook auf, um die Azure SSIS-IR zu starten. Die **Warten**-Aktivität wartet 30 Minuten (1.800 Sekunden) darauf, dass die Azure SSIS-IR gestartet wird. Die **Gespeicherte Prozedur**-Aktivität führt ein SQL­-Skript aus, das das SSIS-Paket ausführt. Die zweite **Web**-Aktivität beendet die Azure SSIS-IR. Weitere Informationen zum Aufrufen eines SSIS-Pakets aus einer Data Factory-Pipeline mit der „Gespeicherte Prozedur“-Aktivität finden Sie unter [Aufrufen eines SSIS-Pakets](how-to-invoke-ssis-package-stored-procedure-activity.md). Anschließend erstellen Sie einen Zeitplantrigger zum Planen der Pipeline für die Ausführung im angegebenen Rhythmus.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, die allgemein verfügbar (GA) ist, lesen Sie [Invoke SSIS packages using stored procedure activity in version 1](v1/how-to-invoke-ssis-package-stored-procedure-activity.md) (Aufrufen von SSIS-Paketen mithilfe einer Aktivität einer gespeicherten Prozedur in Version 1).

 
## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie noch keine Azure SSIS-Integration Runtime bereitgestellt haben, stellen Sie sie mithilfe der folgenden Anweisungen im [Tutorial](tutorial-create-azure-ssis-runtime-portal.md) bereit. 

## <a name="create-and-test-an-azure-automation-runbook"></a>Erstellen und Testen eines Azure Automation-Runbooks
In diesem Abschnitt führen Sie die folgenden Schritte aus: 

1. Erstellen eines Azure Automation-Kontos
2. Erstellen des PowerShell-Runbooks in dem Azure Automation-Konto Das mit dem Runbook verbundene PowerShell-Skript startet oder beendet eine Azure SSIS-IR basierend auf dem Befehl, den Sie für den „VORGANG“-Parameter angeben. 
3. Testen Sie das Runbook in Start- und Stoppszenarien, um zu bestätigen, dass es funktioniert. 

### <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos
Wenn Sie nicht über ein Azure Automation-Konto verfügen, erstellen Sie eines anhand der Anweisungen in diesem Schritt. Eine ausführliche Anleitung finden Sie unter [Erstellen eines Azure Automation-Kontos](../automation/automation-quickstart-create-account.md). Im Rahmen dieses Schritts erstellen Sie ein **ausführendes Azure-Konto** (einen Dienstprinzipal in Ihrem Azure Active Directory) und fügen es der Rolle **Mitwirkender** Ihres Azure-Abonnements hinzu. Stellen Sie sicher, dass es sich um das Abonnement handelt, das die Data Factory mit der Azure SSIS-IR enthält. Azure Automation verwendet dieses Konto für die Authentifizierung bei Azure Resource Manager und die Verarbeitung Ihrer Ressourcen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.    
2. Wählen Sie im linken Menü **Neu** aus, dann **Überwachung + Verwaltung** und schließlich **Automatisierung**. 

    ![Neu -> Überwachung + Verwaltung -> Automatisierung](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. Gehen Sie im Fenster **Automation-Konto hinzufügen** folgendermaßen vor: 

    1. Geben Sie einen **Namen** für das Automation-Konto an. 
    2. Wählen Sie das **Abonnement** aus, das die Data Factory mit der Azure SSIS-IR enthält. 
    3. Wählen Sie für **Ressourcengruppe** die Option **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen, oder wählen Sie **Vorhandene verwenden** aus, um eine vorhandene Ressourcengruppe auszuwählen. 
    4. Wählen Sie einen **Standort** für das Automation-Konto aus. 
    5. Vergewissern Sie sich, dass **Ausführendes Konto erstellen** auf **Ja** festgelegt ist. Ein Dienstprinzipal wird in Ihrem Azure Active Directory erstellt. Er wird der Rolle **Mitwirkender** des Azure-Abonnements hinzugefügt.
    6. Wählen Sie **An Dashboard anheften** aus, damit er auf dem Dashboard des Portals angezeigt wird. 
    7. Klicken Sie auf **Erstellen**. 

        ![Neu -> Überwachung + Verwaltung -> Automatisierung](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. Sie sehen den **Bereitstellungsstatus** auf dem Dashboard und in den Benachrichtigungen. 
    
    ![Bereitstellen von Automation](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. Die Startseite für das Automation-Konto wird angezeigt, nachdem dieses erstellt wurde. 

    ![Automation-Startseite](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Importieren von Data Factory-Modulen

1. Wählen Sie im linken Menü **Module** im Bereich **Freigegebene Ressourcen** aus, und überprüfen Sie, ob **AzureRM.Profile** und **AzureRM.DataFactoryV2** in der Liste der Module aufgeführt werden. Wenn sie nicht vorhanden sind, wählen Sie auf der Symbolleiste **Katalog durchsuchen** aus.

    ![Automation-Startseite](./media/how-to-schedule-azure-ssis-integration-runtime/automation-modules.png)
2. Geben Sie im Fenster **Katalog durchsuchen** im Suchfeld **AzureRM.Profile** ein, und drücken Sie die **EINGABETASTE**. Wählen Sie in der Liste **AzureRM.Profile** aus. Klicken Sie dann auf der Symbolleiste auf **Importieren**. 

    ![Auswählen von „AzureRM.Profile“](./media/how-to-schedule-azure-ssis-integration-runtime/select-azurerm-profile.png)
1. Wählen Sie im Fenster **Importieren** die Option **Ich stimme zu, alle Azure-Module zu aktualisieren** aus, und klicken Sie auf **OK**.  

    ![Importieren von „AzureRM.Profile“](./media/how-to-schedule-azure-ssis-integration-runtime/import-azurerm-profile.png)
4. Schließen Sie die Fenster, um zurück zum Fenster **Module** zu gelangen. Der Status des Imports sollte in der Liste angezeigt werden. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. Warten Sie, bis als **Status** der Wert **Verfügbar** angezeigt wird.

    ![Importstatus](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-profile.png)
1. Wiederholen Sie die Schritte zum Importieren des Moduls **AzureRM.DataFactoryV2**. Vergewissern Sie sich, dass der Status dieses Moduls auf **Verfügbar** festgelegt ist, bevor Sie fortfahren. 

    ![Abschließender Importstatus](./media/how-to-schedule-azure-ssis-integration-runtime/module-list-with-azurerm-datafactoryv2.png)

### <a name="create-a-powershell-runbook"></a>Erstellen eines PowerShell-Runbooks
Das folgende Verfahren bietet Schritte zum Erstellen eines PowerShell-Runbooks. Das mit dem Runbook verbundene Skript startet/beendet eine Azure SSIS-IR basierend auf dem Befehl, den Sie für den **VORGANG**-Parameter angeben. Dieser Abschnitt enthält nicht alle Details zum Erstellen eines Runbooks. Weitere Informationen finden Sie im Artikel [Erstellen eines Runbooks](../automation/automation-quickstart-create-runbook.md).

1. Wechseln Sie zur Registerkarte **Runbooks**, und wählen Sie auf der Symbolleiste **+ Runbook hinzufügen** aus. 

    ![Schaltfläche „Runbook hinzufügen“](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Wählen Sie **Neues Runbook erstellen** aus, und führen Sie die folgenden Schritte aus: 

    1. Geben Sie für **Name** die Zeichenfolge **StartStopAzureSsisRuntime** ein.
    2. Wählen Sie als **Runbooktyp** den Typ **PowerShell** aus.
    3. Klicken Sie auf **Erstellen**.
    
        ![Schaltfläche „Runbook hinzufügen“](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Kopieren Sie das folgende Skript, und fügen Sie es in das Runbook-Skriptfenster ein. Speichern und veröffentlichen Sie das Runbook mithilfe der Schaltflächen **Speichern** und **Veröffentlichen** auf der Symbolleiste. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

    ![Bearbeiten eines PowerShell-Runbooks](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
5. Testen Sie das Runbook, indem Sie auf der Symbolleiste die Schaltfläche **Starten** auswählen. 

    ![Schaltfläche zum Starten eines Runbooks](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. Führen Sie im Fenster **Runbook starten** die folgenden Schritte aus: 

    1. Geben Sie unter **RESSOURCENGRUPPENNAME** den Namen der Ressourcengruppe mit der Data Factory, in der sich die Azure SSIS-IR befindet, ein. 
    2. Geben Sie unter **DATA FACTORY-NAME** den Namen der Data Factory ein, in der sich die Azure SSIS-IR befindet. 
    3. Geben Sie unter **AZURESSISNAME** den Namen der Azure SSIS-IR ein. 
    4. Geben Sie unter **VORGANG** die Zeichenfolge **START** ein. 
    5. Klicken Sie auf **OK**.  

        ![Fenster „Runbook starten“](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. Wählen Sie im Auftragsfenster die Kachel **Ausgabe** aus. Warten Sie im Fenster „Ausgabe“ des Auftrags, bis die Meldung **##### Abgeschlossen #####** angezeigt wird, nachdem zuvor **##### Wird gestartet #####** angezeigt wurde. Das Starten einer Azure SSIS-IR kann etwa 20 Minuten dauern. Schließen Sie das Fenster **Auftrag**, und kehren Sie zum Fenster **Runbook** zurück.

    ![Azure SSIS-IR – gestartet](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Wiederholen Sie die vorherigen beiden Schritte, verwenden Sie dabei jedoch **STOPP** als Wert für **VORGANG**. Starten Sie das Runbook erneut, indem Sie auf der Symbolleiste die Schaltfläche **Starten** auswählen. Geben Sie den Namen der Ressourcengruppe, den Namen der Data Factory und den Namen der Azure SSIS-IR ein. Geben Sie unter **VORGANG** die Zeichenfolge **STOPP** ein. 

    Warten Sie im Fenster „Ausgabe“ des Auftrags, bis die Meldung **##### Abgeschlossen #####** angezeigt wird, nachdem zuvor **##### Wird beendet #####** angezeigt wurde. Das Beenden einer Azure SSIS-IR dauert nicht so lange wie das Starten der Azure SSIS-IR. Schließen Sie das Fenster **Auftrag**, und kehren Sie zum Fenster **Runbook** zurück.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Erstellen von Zeitplänen für das Runbook zum Starten/Beenden der Azure SSIS-IR
Im vorherigen Abschnitt haben Sie ein Azure Automation-Runbook erstellt, das eine Azure SSIS-IR starten oder beenden kann. In diesem Abschnitt erstellen Sie zwei Zeitpläne für das Runbook. Beim Konfigurieren des ersten Zeitplans geben Sie „START“ für den „VORGANG“-Parameter ein. Gleichermaßen geben Sie beim Konfigurieren des zweiten Zeitplans „STOPP“ für „VORGANG“ an. Ausführliche Schritte zum Erstellen von Zeitplänen finden Sie unter [Erstellen eines Zeitplans](../automation/automation-schedules.md#creating-a-schedule).

1. Wählen Sie im Fenster **Runbook** die Option **Zeitpläne** und dann auf der Symbolleiste **+ Zeitplan hinzufügen** aus. 

    ![Azure SSIS-IR – gestartet](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. Führen Sie im Fenster **Runbook planen** die folgenden Schritte aus: 

    1. Wählen Sie **Zeitplan mit Runbook verknüpfen** aus. 
    2. Wählen Sie **Neuen Zeitplan erstellen** aus.
    3. Geben Sie im Fenster **Neuer Zeitplan** unter **Name** die Zeichenfolge **Start IR daily** ein. 
    4. Geben Sie im Abschnitt **Start** eine Zeit wenige Minuten nach dem aktuellen Zeitpunkt ein. 
    5. Wählen Sie unter **Wiederholung** die Option **Serie** aus. 
    6. Wählen Sie im Abschnitt **Wiederholungsintervall** die Option **Tag** aus. 
    7. Klicken Sie auf **Erstellen**. 

        ![Zeitplan für den Start der Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Wechseln Sie zur Registerkarte **Parameter und Ausführungseinstellungen**. Geben Sie den Namen der Ressourcengruppe, den Namen der Data Factory und den Namen der Azure SSIS-IR ein. Geben Sie unter **VORGANG** die Zeichenfolge **START** ein. Klicken Sie auf **OK**. Wählen Sie erneut **OK** aus, um den Zeitplan auf der Seite **Zeitpläne** des Runbooks anzuzeigen. 

    ![Zeitplan zum Starten der Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Wiederholen Sie die vorherigen beiden Schritte, um einen Zeitplan mit dem Namen **Stop IR daily** zu erstellen. Geben Sie hierbei als Zeit einen Zeitpunkt an, der mindestens 30 Minuten nach dem im Zeitplan **Start IR daily** liegt. Geben Sie unter **VORGANG** die Option **STOPP** an. 
5. Wählen Sie im Fenster **Runbook** im linken Menü **Aufträge** aus. Die über die Zeitpläne zu den angegebenen Zeitpunkten erstellten Aufträge sollten nun mit ihrem Status angezeigt werden. Sie können die Details zum Auftrag wie etwa die Ausgabe anzeigen, ähnlich dem Ablauf beim Testen des Runbooks. 

    ![Zeitplan zum Starten der Azure SSIS-IR](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Deaktivieren Sie nach Abschluss der Tests die Zeitpläne, indem Sie sie bearbeiten und für **aktiviert** die Option **NEIN** auswählen. Wählen Sie im linken Menü **Zeitpläne** aus, wählen Sie **Start IR daily/Stop IR daily** aus, und wählen Sie für **Aktiviert** die Option **NEIN** aus. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Erstellen von Webhooks zum Starten und Beenden der Azure SSIS-IR
Befolgen Sie die Anweisungen unter [Erstellen von Webhooks](../automation/automation-webhooks.md#creating-a-webhook), um zwei Webhooks für das Runbook zu erstellen. Geben Sie als „VORGANG“ für den ersten „START“ und für den zweiten „STOPP“ an. Speichern Sie die URLs für beide Webhooks an einer beliebigen Stelle (z.B. in einer Textdatei oder einem OneNote-Notizbuch). Sie verwenden diese URLs beim Konfigurieren von Webaktivitäten in der Data Factory-Pipeline. Die folgende Abbildung zeigt ein Beispiel für die Erstellung eines Webhooks, der die Azure SSIS-IR startet:

1. Wählen Sie im **Runbook** im linken Menü **Webhooks** aus, und wählen Sie auf der Symbolleiste **+ Webhook hinzufügen** aus. 

    ![Webhooks -> Webhook hinzufügen](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. Wählen Sie im Fenster **Webhook hinzufügen** die Option **Neuen Webhook erstellen** aus, und führen Sie die folgenden Aktionen aus: 

    1. Geben Sie unter **Name** die Zeichenfolge **StartAzureSsisIR** ein. 
    2. Vergewissern Sie sich, dass **Aktiviert** auf **Ja** festgelegt ist. 
    3. Kopieren Sie die **URL**, und speichern Sie sie an einer beliebigen Stelle. Dieser Schritt ist wichtig. Die URL wird später nicht mehr angezeigt. 
    4. Klicken Sie auf **OK**. 

        ![Fenster „Neuer Webhook“](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Wechseln Sie zur Registerkarte **Parameter und Ausführungseinstellungen**. Geben Sie den Namen der Ressourcengruppe, den Namen der Data Factory und die Azure SSIS-IR ein. Geben Sie unter **VORGANG** die Zeichenfolge **START** ein. Klicken Sie auf **OK**. Klicken Sie dann auf **Erstellen**. 

    ![Webhook – Parameter und Ausführungseinstellungen](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Wiederholen Sie die vorangegangenen drei Schritte, um einen anderen Webhook mit dem Namen **StopAzureSsisIR** zu erstellen. Vergessen Sie nicht, die URL zu kopieren. Wenn Sie die Parameter und Ausführungseinstellungen angeben, geben Sie **STOPP** für **VORGANG** ein. 

Sie sollten zwei URLs haben, eine für den Webhook **StartAzureSsisIR** und eine für den Webhook **StopAzureSsisIR**. Sie können an diese URLs eine HTTP POST-Anforderung senden, um Ihre Azure SSIS-IR zu starten und zu beenden. 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Erstellen und Planen einer Data Factory-Pipeline zum Starten/Beenden der IR
In diesem Abschnitt wird veranschaulicht, wie Sie mit einer Webaktivität die Webhooks aufrufen, die Sie im vorherigen Abschnitt erstellt haben.

Die erstellte Pipeline umfasst vier Aktivitäten. 

1. Die erste **Web**-Aktivität ruft den ersten Webhook auf, um die Azure SSIS-IR zu starten. 
2. Die **Warten**-Aktivität wartet 30 Minuten (1.800 Sekunden) darauf, dass die Azure SSIS-IR gestartet wird. 
3. Die **Gespeicherte Prozedur**-Aktivität führt ein SQL­-Skript aus, das das SSIS-Paket ausführt. Die zweite **Web**-Aktivität beendet die Azure SSIS-IR. Weitere Informationen zum Aufrufen eines SSIS-Pakets aus einer Data Factory-Pipeline mit der „Gespeicherte Prozedur“-Aktivität finden Sie unter [Aufrufen eines SSIS-Pakets](how-to-invoke-ssis-package-stored-procedure-activity.md). 
4. Die zweite **Web**-Aktivität ruft den Webhook auf, um die Azure SSIS-IR zu beenden. 

Nachdem Sie die Pipeline erstellt und getestet haben, erstellen Sie einen Zeitplantrigger und ordnen diesen der Pipeline zu. Der Zeitplantrigger definiert einen Zeitplan für die Pipeline. Angenommen, Sie erstellen einen Trigger, der täglich um 23 Uhr ausgeführt werden soll. Der Trigger führt die Pipeline täglich um 23 Uhr aus. Die Pipeline startet die Azure SSIS-IR, führt das SSIS-Paket aus und beendet dann die Azure SSIS-IR. 

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.    
2. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **MyAzureSsisDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>MyAzureSsisDataFactory“), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
      Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. Die Liste enthält nur Standorte, die für die Erstellung von Data Factorys geeignet sind.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
   ![Data Factory-Startseite](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten.

### <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus. 

   ![Seite für die ersten Schritte](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, ziehen Sie die Aktivität **Web** auf die Oberfläche des Pipeline-Designers, und legen Sie sie ab. Ändern Sie im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** den Namen der Aktivität in **StartIR**.

   ![Erste Webaktivität – Registerkarte „Allgemein“](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Einstellungen**, und führen Sie die folgenden Aktionen aus: 

    1. Fügen Sie unter **URL** die URL für den Webhook ein, der die Azure SSIS-IR startet. 
    2. Wählen Sie unter **Methode** die Option **POST** aus. 
    3. Geben Sie unter **Text** die Zeichenfolge `{"message":"hello world"}` ein. 
   
        ![Erste Webaktivität – Registerkarte „Einstellungen“](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)
4. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Iteration & Conditionals** (Iteration + Bedingungen), ziehen Sie die Aktivität **ForEach** in die Oberfläche des Pipeline-Designers, und legen Sie sie dort ab. Ändern Sie auf der Registerkarte **Allgemein** den Namen der Aktivität in **WaitFor30Minutes**. 
5. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Einstellungen**. Geben Sie unter **Wartezeit in Sekunden** den Wert **1800** ein. 
6. Verbinden Sie die **Web**-Aktivität und die **Warten**-Aktivität. Um die Verbindung herzustellen, ziehen Sie das grüne quadratische Feld, das mit der Web-Aktivität verbunden ist, zur Warten-Aktivität. 

    ![Verbinden der Aktivitäten „Web“ und „Warten“](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-wait.png)
5. Ziehen Sie die „Gespeicherte Prozedur“-Aktivität aus dem Bereich **Allgemein** der Toolbox **Aktivitäten**, und legen Sie sie ab. Legen Sie den Namen der Aktivität auf **RunSSISPackage** fest. 
6. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **SQL-Konto**. 
7. Klicken Sie unter **Verknüpfter Dienst** auf **+ Neu**.
8. Führen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die folgenden Aktionen aus: 

    1. Wählen Sie **Azure SQL-Datenbank** als **Typ** aus.
    2. Wählen Sie für das Feld **Servername** den Azure SQL-Server aus, der die Datenbank **SSISDB** hostet. Der Azure SSIS-IR-Bereitstellungsprozesses erstellt einen SSIS-Katalog (Datenbank „SSISDB“) im angegebenen Azure SQL-Server.
    3. Wählen Sie **SSISDB** als **Datenbankname** aus.
    4. Geben Sie unter **Benutzername** den Namen des Benutzers ein, der Zugriff auf die Datenbank hat.
    5. Geben Sie unter **Kennwort** das Kennwort des Benutzers ein. 
    6. Testen Sie die Verbindung mit der Datenbank, indem Sie auf die Schaltfläche **Verbindung testen** klicken.
    7. Speichern Sie den verknüpften Dienst, indem Sie auf die Schaltfläche **Speichern** klicken.
1. Wechseln Sie im Fenster **Eigenschaften** von der Registerkarte **SQL-Konto** zur Registerkarte **Gespeicherte Prozedur**, und führen Sie folgende Schritte durch: 

    1. Wählen Sie für **Name der gespeicherten Prozedur** die Option **Bearbeiten** aus, und geben Sie **sp_executesql** ein. 
    2. Wählen Sie im Abschnitt **Parameter der gespeicherten Prozedur** die Option **+ Neu** aus. 
    3. Geben Sie unter **Name** des Parameters **stmt** ein. 
    4. Geben Sie unter **Typ** des Parameters **Zeichenfolge** ein. 
    5. Geben Sie unter **Wert** des Parameters die folgende SQL-Abfrage ein:

        Geben Sie in der SQL-Abfrage die entsprechenden Werte für die Parameter **folder_name**, **project_name** und **package_name** ein. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END   
        ```
10. Verbinden Sie die **Warten**-Aktivität mit der **Gespeicherte Prozedur**-Aktivität. 

    ![Verbinden der Aktivitäten „Warten“ und „Gespeicherte Prozedur“](./media/how-to-schedule-azure-ssis-integration-runtime/connect-wait-sproc.png)
11. Ziehen Sie die **Web**-Aktivität rechts neben die **Gespeicherte Prozedur**-Aktivität, und legen Sie sie dort ab. Legen Sie den Namen der Aktivität auf **StopIR** fest. 
12. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Einstellungen**, und führen Sie die folgenden Aktionen aus: 

    1. Fügen Sie unter **URL** die URL für den Webhook ein, der die Azure SSIS-IR beendet. 
    2. Wählen Sie unter **Methode** die Option **POST** aus. 
    3. Geben Sie unter **Text** die Zeichenfolge `{"message":"hello world"}` ein.  
4. Verbinden Sie die **Gespeicherte Prozedur**-Aktivität mit der letzten **Web**-Aktivität.

    ![Vollständige Pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
5. Klicken Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste auf **Überprüfen**. Schließen Sie den **Pipelineüberprüfungsbericht**, indem Sie auf die Schaltfläche **>>** klicken. 

    ![Überprüfen der Pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>Ausführen eines Testlaufs für die Pipeline

1. Wählen Sie auf der Symbolleiste für die Pipeline die Option **Testlauf** aus. Die Ausgabe wird im Fenster **Ausgabe** im unteren Bereich angezeigt. 

    ![Testlauf](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. Auf der Seite **Runbook** Ihres Azure Automation-Kontos können Sie sich vergewissern, dass die Aufträge zum Starten und Beenden der Azure SSIS-IR ausgeführt wurden. 

    ![Runbook-Aufträge](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. Starten Sie SQL Server Management Studio. Führen Sie im Fenster **Mit Server verbinden** die folgenden Aktionen aus: 

    1. Geben Sie unter **Servername** die Zeichenfolge **&lt;Ihre Azure SQL-Datenbank&gt;.database.windows.net** ein.
    2. Wählen Sie **Optionen >>** aus.
    3. Wählen Sie unter **Mit Datenbank verbinden** die Option **SSISDB** aus.
    4. Wählen Sie **Verbinden**aus. 
    5. Erweitern Sie **Integration Services-Kataloge** -> **SSISDB** -> Ihr Ordner -> **Projekte** -> Ihr SSIS-Projekt -> **Pakete** . 
    6. Klicken Sie mit der rechten Maustaste auf das SSIS-Paket, und wählen Sie **Berichte** -> **Standardberichte** -> **Alle Ausführungen** aus. 
    7. Vergewissern Sie sich, dass das SSIS-Paket ausgeführt wurde. 

        ![Überprüfen der Ausführung des SSIS-Pakets](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>Planen der Pipeline 
Nun, da die Pipeline wie erwartet funktioniert, können Sie einen Trigger zum Ausführen dieser Pipeline in einem festgelegten Rhythmus erstellen. Informationen zum Zuordnen eines Zeitplantriggers zu einer Pipeline finden Sie unter [Auslösen der Pipeline nach einem Zeitplan](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Wählen Sie auf der Symbolleiste für die Pipeline die Option **Testlauf** und dann **Neu/Bearbeiten** aus. 

    ![Trigger -> „Neu/Bearbeiten“](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. Wählen Sie im Fenster **Add Triggers** (Trigger hinzufügen) die Option **+ Neu** aus.

    ![„Add Triggers“ (Trigger hinzufügen) – „Neu“](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. Führen Sie unter **Neuer Trigger** die folgenden Aktionen durch: 

    1. Geben Sie unter **Name** einen Namen für den Trigger an. Im folgenden Beispiel ist **Run daily** der Name des Triggers. 
    2. Wählen Sie unter **Typ** die Option **Zeitplan** aus. 
    3. Wählen Sie unter **Startdatum** ein Startdatum und eine Startzeit aus. 
    4. Geben Sie unter **Wiederholung** den Rhythmus für den Trigger an. Im folgenden Beispiel ist dies einmal täglich. 
    5. Unter **Ende** können Sie das Datum und die Uhrzeit angeben, indem Sie die Option **Festes Datum** auswählen. 
    6. Wählen Sie **Aktiviert** aus. Der Trigger wird sofort nach dem Veröffentlichen der Lösung in Data Factory aktiviert. 
    7. Klicken Sie auf **Weiter**.

        ![Trigger -> „Neu/Bearbeiten“](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. Überprüfen Sie auf der Seite **Trigger Run Parameters** (Ausführungsparameter für Trigger) die Warnung, und wählen Sie **Fertig stellen** aus. 
5. Veröffentlichen Sie die Projektmappe in Data Factory, indem Sie im linken Bereich **Alle veröffentlichen** auswählen. 

    ![Alle veröffentlichen](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)
6. Verwenden Sie zum Überwachen der Ausführung von Triggern und Pipelines die Registerkarte **Monitor** auf der linken Seite. Ausführliche Schritte finden Sie unter [Überwachen der Pipeline](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![Pipelineausführungen](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
7. Wenn Sie mit einer Pipelineausführung verbundene Aktivitätsausführungen anzeigen möchten, wählen Sie in der Spalte **Aktionen** den ersten Link (**View Activity Runs**, Aktivitätsausführungen anzeigen) aus. Jeder Aktivität in der Pipeline (erste Web-Aktivität, Warten-Aktivität, Gespeicherte Prozedur-Aktivität und zweite Web-Aktivität) sind vier Aktivitätsausführungen zugeordnet. Wählen Sie den Link **Pipelines** ganz oben aus, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln.

    ![Aktivitätsausführungen](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
8. Sie können die Triggerausführungen auch anzeigen, indem Sie in der Dropdownliste neben der Option **Pipeline Runs** (Pipelineausführungen) ganz oben die Option **Trigger runs** (Triggerausführungen) auswählen. 

    ![Triggerausführungen](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

## <a name="next-steps"></a>Nächste Schritte
Informationen finden Sie in den folgenden Artikeln der SSIS-Dokumentation: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Bereitstellen, Ausführen und Überwachen eines SSIS-Pakets in Azure)   
- [Connect to the SSISDB Catalog database on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Herstellen einer Verbindung mit der SSISDB-Katalogdatenbank in Azure)
- [Schedule the execution of an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Planen der Ausführung eines SSIS-Pakets in Azure)
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung)
---
title: Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks
description: Es wird beschrieben, wie Sie bei Auslösung einer Azure-Warnung ein Runbook auslösen.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 98de7a84dc388f74c64d7c265d2ce8ed32995a5a
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48784774"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks

Sie können [Azure Monitor](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) verwenden, um in Azure grundlegende Metriken und Protokolle für die meisten Dienste zu überwachen. Sie können Azure Automation-Runbooks aufrufen, indem Sie [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) oder klassische Warnungen verwenden, um Aufgaben basierend auf Warnungen zu automatisieren. In diesem Artikel erfahren Sie, wie Sie ein Runbook mit Warnungen konfigurieren und ausführen.

## <a name="alert-types"></a>Warnungstypen

Sie können Automation-Runbooks mit drei Warnungstypen verwenden:
* Klassische Metrikwarnungen
* Aktivitätsprotokollwarnungen
* Near Real-Time Metric Alerts

Wenn eine Warnung ein Runbook aufruft, erfolgt der eigentliche Aufruf in Form einer HTTP POST-Anforderung an den Webhook. Der Text der POST-Anforderung enthält ein JSON-formatiertes Objekt, das nützliche Eigenschaften im Zusammenhang mit der Warnung enthält. In der folgenden Tabelle sind Links zu den Nutzlastschemas der einzelnen Warnungstypen angegeben:

|Warnung  |BESCHREIBUNG|Nutzlast und Schema  |
|---------|---------|---------|
|[Klassische Metrikwarnung](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Sendet eine Benachrichtigung, wenn eine beliebige Metrik auf Plattformebene eine bestimmte Bedingung erfüllt. Beispiel: Der Wert für **CPU in %** auf einer VM liegt seit fünf Minuten über dem Wert **90**.| [Nutzlastschema vom Typ „Klassenmetrikwarnung“](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Aktivitätsprotokollwarnung](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Sendet eine Benachrichtigung, wenn ein beliebiges neues Ereignis im Azure-Aktivitätsprotokoll bestimmte Bedingungen erfüllt. Beispiel: Wenn ein `Delete VM`-Vorgang in **myProductionResourceGroup** auftritt oder wenn ein neues Azure Service Health-Ereignis mit dem Status **Aktiv** angezeigt wird.| [Nutzlastschema vom Typ „Aktivitätsprotokollwarnung“](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md)        |
|[Near Real-Time Metric Alerts](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Sendet eine Benachrichtigung schneller als Metrikwarnungen, wenn mindestens eine Metrik auf Plattformebene bestimmte Bedingungen erfüllt. Beispiel: Wenn der Wert für **CPU in %** auf einer VM größer als **90** ist und der Wert für **Netzwerk eingehend** in den letzten fünf Minuten über **500 MB** gelegen hat.| [Nutzlastschema vom Typ „Near Real-Time Metric Alert“](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Da sich die Daten unterscheiden, die von den einzelnen Typen von Warnungen bereitgestellt werden, wird jeder Warnungstyp anders behandelt. Im nächsten Abschnitt erfahren Sie, wie Sie ein Runbook erstellen, um verschiedene Warnungstypen zu behandeln.

## <a name="create-a-runbook-to-handle-alerts"></a>Erstellen eines Runbooks zum Behandeln von Warnungen

Um bei Warnungen Automation einzusetzen, benötigen Sie ein Runbook, das über geeignete Logik verfügt, um die an das Runbook übergebene JSON-Nutzlast der Warnungen zu verarbeiten. Das folgende Beispielrunbook muss von einer Azure-Warnung aufgerufen werden.

Wie im vorherigen Abschnitt beschrieben, weist jeder Typ von Warnung ein anderes Schema auf. Mit dem Skript werden die Webhookdaten im Runbook-Eingabeparameter`WebhookData` einer Warnung verwendet. Anschließend wertet das Skript die JSON-Nutzlast aus, um zu bestimmen, welcher Warnungstyp verwendet wurde.

In diesem Beispiel wird eine Warnung von einem virtuellen Computer verwendet. Es ruft die VM-Daten aus der Nutzlast ab und verwendet diese Informationen dann, um den virtuellen Computer zu beenden. Die Verbindung muss in dem Automation-Konto eingerichtet sein, unter dem das Runbook ausgeführt wird. Wenn Warnungen zum Auslösen von Runbooks verwendet werden, muss unbedingt der Status der Warnung im ausgelösten Runbook überprüft werden. Das Runbook wird bei jeder Statusänderung der Warnung ausgelöst. Warnungen haben mehrere Status, die beiden häufigsten sind `Activated` und `Resolved`. Prüfen Sie in Ihrer Runbook-Logik auf diesen Status, um sicherzustellen, dass Ihr Runbook nur einmal ausgeführt wird. Das Beispiel in diesem Artikel zeigt, wie Sie die Suche auf `Activated`-Warnungen einschränken.

Das Runbook verwendet das [ausführende Konto](automation-create-runas-account.md) **AzureRunAsConnection** für die Authentifizierung bei Azure, um die Verwaltungsaktion für die VM durchzuführen.

Verwenden Sie dieses Beispiel, um ein Runbook mit dem Namen **Stop-AzureVmInResponsetoVMAlert** zu erstellen. Sie können das PowerShell-Skript ändern und mit vielen verschiedenen Ressourcen nutzen.

1. Wechseln Sie zu Ihrem Azure Automation-Konto.
1. Wählen Sie unter **PROZESSAUTOMATISIERUNG** die Option **Runbooks**.
1. Wählen Sie oben in der Liste mit den Runbooks die Option **Runbook hinzufügen**. 
1. Wählen Sie auf der Seite **Runbook hinzufügen** die Option **Schnellerfassung**.
1. Geben Sie als Runbookname **Stop-AzureVmInResponsetoVMAlert** ein. Wählen Sie als Runbooktyp **PowerShell** aus. Wählen Sie anschließend **Erstellen**.  
1. Kopieren Sie das folgende PowerShell-Beispiel in den Bereich **Bearbeiten**. 

    ```powershell-interactive
    <#
    .SYNOPSIS
    This runbook stops a resource management VM in response to an Azure alert trigger.

    .DESCRIPTION
    This runbook stops a resource management VM in response to an Azure alert trigger.
    The input is alert data that has the information required to identify which VM to stop.
    
    DEPENDENCIES
    - The runbook must be called from an Azure alert via a webhook.
    
    REQUIRED AUTOMATION ASSETS
    - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
    - An Automation certificate asset called "AzureRunAsCertificate".

    .PARAMETER WebhookData
    Optional. (The user doesn't need to enter anything, but the service always passes an object.)
    This is the data that's sent in the webhook that's triggered from the alert.

    .NOTES
    AUTHOR: Azure Automation Team
    LASTEDIT: 2017-11-22
    #>

    [OutputType("PSAzureOperationResponse")]

    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )

    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData.
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema).
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # The schema isn't supported.
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if ($status -eq "Activated")
        {
            $ResourceType = $AlertContext.resourceType
            $ResourceGroupName = $AlertContext.resourceGroupName
            $SubId = $AlertContext.subscriptionId
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Use this only if this is a resource management VM.
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is the VM.
                Write-Verbose "This is a resource management VM." -Verbose

                # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the VM.
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType isn't supported.
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated', so no action taken.
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```
1. Wählen Sie die Option **Veröffentlichen**, um das Runbook zu speichern und zu veröffentlichen.

## <a name="create-an-action-group"></a>Erstellen einer Aktionsgruppe

Eine Aktionsgruppe ist eine Sammlung von Aktionen, die über eine Warnung ausgelöst werden. Runbooks stellen lediglich eine der vielen Aktionen dar, die Sie mit Aktionsgruppen verwenden können.

1. Wählen Sie im Azure-Portal die Option **Überwachen** > **EINSTELLUNGEN** > **Aktionsgruppen**.
1. Wählen Sie **Aktionsgruppe hinzufügen**, und geben Sie dann die erforderlichen Informationen ein:  
    1. Geben Sie im Feld **Name der Aktionsgruppe** einen Namen ein.
    1. Geben Sie im Feld **Kurzname** einen Namen ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Aktionsgruppe gesendet werden.
    1. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingefügt. Dies ist das Abonnement, unter dem die Aktionsgruppe gespeichert wird.
    1. Wählen Sie die Ressourcengruppe aus, in der die Aktionsgruppe gespeichert ist.

Für dieses Beispiel erstellen Sie zwei Aktionen: eine Runbookaktion und eine Benachrichtigungsaktion.

### <a name="runbook-action"></a>Runbookaktion

Gehen Sie wie folgt vor, um in der Aktionsgruppe eine Runbookaktion zu erstellen:

1. Geben Sie unter **Aktionen** > **AKTIONSNAME** einen Namen für die Aktion ein. Wählen Sie als **AKTIONSTYP** die Option **Automation-Runbook**.
1. Wählen Sie unter **DETAILS** die Option **Details bearbeiten**.  
1. Wählen Sie auf der Seite **Runbook konfigurieren** unter **Runbook-Quelle** die Option **Benutzer**.  
1. Wählen Sie Ihr **Abonnement** und **Automation-Konto** und anschließend das Runbook **Stop-AzureVmInResponsetoVMAlert** aus.  
1. Wählen Sie **OK**, wenn Sie fertig sind.

### <a name="notification-action"></a>Benachrichtigungsaktion

Gehen Sie wie folgt vor, um in der Aktionsgruppe eine Benachrichtigungsaktion zu erstellen:

1. Geben Sie unter **Aktionen** > **AKTIONSNAME** einen Namen für die Aktion ein. Wählen Sie unter **AKTIONSTYP** die Option **E-Mail**.  
1. Wählen Sie unter **DETAILS** die Option **Details bearbeiten**.  
1. Geben Sie auf der Seite **E-Mail** die E-Mail-Adresse ein, die für die Benachrichtigungen verwendet werden soll, und wählen Sie anschließend **OK**. Das Hinzufügen einer E-Mail-Adresse zusätzlich zum Runbook als Aktion ist hilfreich. Sie werden benachrichtigt, wenn das Runbook gestartet wird.  

    Ihre Aktionsgruppe sollte nun so aussehen wie in der folgenden Abbildung:

   ![Seite „Aktionsgruppe hinzufügen“](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

Sie können diese Aktionsgruppe in den [Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) und [Near Real-Time Alerts](../monitoring-and-diagnostics/monitoring-overview-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) verwenden, die Sie erstellen.

## <a name="classic-alert"></a>Klassische Warnung

Klassische Warnungen basieren auf Metriken, und es werden keine Aktionsgruppen verwendet. Sie können aber basierend auf einer klassischen Warnung eine Runbookaktion einrichten. 

Gehen Sie wie folgt vor, um eine klassische Warnung zu erstellen:

1. Wählen Sie **Metrikwarnung hinzufügen** aus.
1. Geben Sie Ihrer metrischen Warnung den Namen **myVMCPUAlert**. Geben Sie eine kurze Beschreibung für die Warnung ein.
1. Wählen Sie als Metrikwarnungsbedingung die Option **Größer als**. Wählen Sie als **Schwellenwert** den Wert **10**. Wählen Sie für **Zeitraum** den Wert **Over the last five minutes** (Letzte fünf Minuten).
1. Wählen Sie unter **Aktion ausführen** die Option **Runbook von dieser Warnung aus ausführen**.
1. Wählen Sie auf der Seite **Runbook konfigurieren** als **Runbook-Quelle** die Option **Benutzer**. Wählen Sie Ihr Automation-Konto und dann das Runbook **Stop-AzureVmInResponsetoVMAlert** aus. Klicken Sie auf **OK**.
1. Wählen Sie **OK**, um die Warnungsregel zu speichern.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Starten eines Automation-Runbooks per Webhook finden Sie unter [Starten eines Azure Automation-Runbooks mit einem Webhook](automation-webhooks.md).
* Weitere Informationen über die verschiedenen Methoden zum Starten eines Runbooks finden Sie unter [Starten eines Runbooks](automation-starting-a-runbook.md).
* Weitere Informationen zum Erstellen von Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Informationen zum Erstellen von Near Real-Time Alerts finden Sie unter [Erstellen einer Warnungsregel im Azure-Portal](../monitoring-and-diagnostics/alert-metric.md?toc=/azure/azure-monitor/toc.json).

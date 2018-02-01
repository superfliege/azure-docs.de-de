---
title: Reagieren auf Azure-Warnungen mit einem Automation-Runbook | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie bei auftretenden Azure-Warnungen die Ausführung eines Runbooks auslösen."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Reagieren auf Azure-Warnungen mit einem Automation-Runbook

[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) stellt grundlegende Metriken und Protokolle für die meisten Dienste in Microsoft Azure bereit. Azure Automation-Runbooks können mithilfe von [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) oder aus klassischen Warnungen aufgerufen werden, um auf den Warnungen basierende Aufgaben zu automatisieren. In diesem Artikel erfahren Sie, wie ein Runbook auf der Grundlage dieser Warnungen konfiguriert und ausgeführt wird.

## <a name="alert-types"></a>Warnungstypen

Runbooks sind unterstützte Aktionen für alle drei Warnungstypen. Wenn eine Warnung das Runbook aufruft, erfolgt der eigentliche Aufruf in Form einer HTTP POST-Anforderung an den Webhook. Der Text der POST-Anforderung enthält ein JSON-formatiertes Objekt, das nützliche Eigenschaften im Zusammenhang mit der Warnung enthält. Die folgende Tabelle enthält Verknüpfungen mit dem Nutzlastschema für die einzelnen Warnungstypen:

|Warnung  |BESCHREIBUNG|Nutzlastschema  |
|---------|---------|---------|
|[Klassische Metrikwarnung](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Sie erhalten eine Benachrichtigung, wenn eine beliebige Metrik auf Plattformebene eine bestimmte Bedingung erfüllt (z.B. CPU-Prozentsatz von mehr als 90 auf einer VM während der letzten fünf Minuten).| [Nutzlastschema](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Aktivitätsprotokollwarnung](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Sie erhalten eine Benachrichtigung, wenn neue Ereignisse im Azure-Aktivitätsprotokoll bestimmte Bedingungen erfüllen (z.B. beim Vorgang „VM löschen“ in „myProductionResourceGroup“ oder bei Anzeige eines neuen Service Health-Ereignisses mit dem Status „Aktiv“).| [Nutzlastschema](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Near Real-Time Metric Alerts](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Erhalten Sie eine Benachrichtigung schneller als Metrikwarnungen, wenn mindestens eine Metrik auf Plattformebene angegebene Bedingungen erfüllt (z. B. die CPU-Auslastung auf einem virtuellen Computer innerhalb der letzten 5 Minuten größer als 90 % und der Dateneingangsverkehr im Netzwerk größer als 500 MB war).| [Nutzlastschema](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Da die von den einzelnen Warnungen bereitgestellten Daten verschieden sind, muss jede Warnung anders behandelt werden. Im nächsten Abschnitt erfahren Sie, wie Sie ein Runbook erstellen, um diese verschiedenen Warnungstypen zu behandeln.

## <a name="create-a-runbook-to-handle-alerts"></a>Erstellen eines Runbooks zum Behandeln von Warnungen

Um bei Warnungen Automatisierung einzusetzen, benötigen Sie ein Runbook, das über Logik verfügt, um die an das Runbook übergebene JSON-Nutzlast der Warnungen zu verarbeiten. Das folgende Beispielrunbook muss von einer Azure-Warnung aufgerufen werden. Wie im vorhergehenden Abschnitt beschrieben, weist jeder Warnungstyp ein anderes Schema auf. Das Skript übernimmt die Webhookdaten im Eingabeparameter `WebhookData` des Runbooks aus einer Warnung und wertet die JSON-Nutzlast aus, um zu bestimmen, welcher Typ verwendet wurde. Das folgende Beispiel würde für eine Warnung von einem virtuellen Computer verwendet. Es ruft die VM-Daten aus der Nutzlast ab und verwendet diese Informationen, um den virtuellen Computer zu beenden. Die Verbindung muss im Automation-Konto eingerichtet sein, unter dem das Runbook ausgeführt wird.

Das Runbook verwendet das [ausführende Konto](automation-create-runas-account.md) **AzureRunAsConnection** für die Authentifizierung bei Azure, um die Verwaltungsaktion für die VM auszuführen.

Das folgende PowerShell-Skript kann für die Verwendung mit vielen verschiedenen Ressourcen abgeändert werden.

Erstellen Sie unter Verwendung des folgenden PowerShell-Beispiels ein Runbook mit dem Namen **Stop-AzureVmInResponsetoVMAlert**.

1. Öffnen Sie Ihr Automation-Konto.
1. Klicken Sie unter **PROZESSAUTOMATISIERUNG** auf **Runbooks**. Die Liste mit den Runbooks wird angezeigt.
1. Klicken Sie oben in der Liste auf die Schaltfläche **Runbook hinzufügen**. Wählen Sie auf der Seite **Runbook hinzufügen** die Option **Schnellerfassung** aus.
1. Geben Sie als **Namen** des Runbooks "Stop-AzureVmInResponsetoVMAlert" ein, und wählen Sie als **Runbooktyp****PowerShell** aus. Klicken Sie auf **Create**.
1. Kopieren Sie das folgende PowerShell-Beispiel in den Bearbeitungsbereich. Klicken Sie auf **Veröffentlichen**, um zu speichern und das Runbook zu veröffentlichen.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

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
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
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
        # Schema not supported
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

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

            # Authenticate to Azure with service principal and certificate and set subscription
            Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
            $ConnectionAssetName = "AzureRunAsConnection"
            Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Verbose "Authenticating to Azure with service principal." -Verbose
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Write-Verbose "Setting subscription to work against: $SubId" -Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Stop the VM
            Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
            Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
            # [OutputType(PSAzureOperationResponse")]
        }
        else {
            # ResourceType not supported
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    }
    else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Erstellen einer Aktionsgruppe

Eine Aktionsgruppe ist eine Sammlung von Aktionen, die auf der Grundlage einer Warnung ergriffen werden. Runbooks stellen lediglich eine der vielen Aktionen dar, die für Aktionsgruppen verfügbar sind.

1. Wählen Sie im Portal **Monitor** aus.

1. Wählen Sie unter **EINSTELLUNGEN** **Aktionsgruppen** aus.

1. Wählen Sie **Aktionsgruppe hinzufügen**, und füllen Sie die Felder aus.

1. Geben Sie im Namensfeld der Aktionsgruppe einen Namen und ebenfalls einen Namen im Feld „Kurzname“ ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

1. In das Feld „Abonnement“ wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert.
Wählen Sie die Ressourcengruppe aus, in der die Aktionsgruppe gespeichert wird.

Für dieses Beispiel erstellen Sie zwei Aktionen, eine Runbookaktion und eine Benachrichtigungsaktion.

### <a name="runbook-action"></a>Runbookaktion

Mit den folgenden Schritten wird eine Runbookaktion innerhalb der Aktionsgruppe erstellt.

1. Geben Sie der Aktion unter **Aktionen** einen Namen.

1. Wählen Sie als **Aktionstyp** **Automation-Runbook** aus.

1. Wählen Sie unter **Details** **Details bearbeiten** aus.

1. Wählen Sie auf der Seite **Runbook konfigurieren** unter **Runbook-Quelle** **Benutzer** aus.

1. Wählen Sie Ihr **Abonnement** und Ihr **Automation-Konto** und schließlich das Runbook mit dem Namen "Stop-AzureVmInResponsetoVMAlert" aus, das Sie im vorhergehenden Schritt erstellt haben.

1. Klicken Sie auf **OK**, wenn Sie fertig sind.

### <a name="notification-action"></a>Benachrichtigungsaktion

Mit den folgenden Schritten wird eine Benachrichtigungsaktion innerhalb der Aktionsgruppe erstellt.

1. Geben Sie der Aktion unter **Aktionen** einen Namen.

1. Wählen Sie als **Aktionstyp** **E-Mail** aus.

1. Wählen Sie unter **Details** **Details bearbeiten** aus.

1. Geben Sie auf der Seite **E-Mail** die zu benachrichtigende E-Mail-Adresse ein, und klicken Sie auf **OK**. Das Hinzufügen sowohl einer E-Mail-Adresse als auch des Runbooks als Aktion ist nützlich, da Sie beim Start des Runbooks benachrichtigt werden. Ihre Aktionsgruppe sollte nun so aussehen wie in der folgenden Abbildung:

   ![Seite „Aktionsgruppe hinzufügen“](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Klicken Sie auf **OK**, um die Aktionsgruppe zu erstellen.

Nach dem Erstellen der Aktionsgruppe können Sie [Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) oder [Near Real-Time Alerts](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) erstellen und die erstellte Aktionsgruppe verwenden.

## <a name="classic-alert"></a>Klassische Warnung

Klassische Warnungen basieren auf Metriken und verwenden keine Aktionsgruppen, können aber als Grundlage von Runbookaktionen dienen. Verwenden Sie die folgenden Schritte, um eine klassische Warnung zu erstellen:

1. Wählen Sie **Metrikwarnung hinzufügen** (Plussymbol) aus.

1. Benennen Sie die Metrikwarnung „myVMCPUAlert“, und geben Sie eine kurze Beschreibung der Warnung ein.

1. Legen Sie die Bedingung für die Metrikwarnung als „Größer als“, den Schwellenwert als „10“ und den Zeitraum als „In den letzten 5 Minuten“ fest.

1. Wählen Sie unter **Aktion ausführen** **Runbook von dieser Warnung aus ausführen** aus.

1. Wählen Sie auf der Seite **Runbook konfigurieren** als **Runbook-Quelle** **Benutzer** aus. Wählen Sie Ihr Automation-Konto und das Runbook **Stop-AzureVmInResponsetoVMAlert** aus. Klicken Sie auf **OK** und dann nochmals auf **OK**, um die Warnungsregel zu speichern.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Starten von Automation-Runbooks mithilfe von Webhooks finden Sie unter [Starten eines Runbooks über einen Webhook](automation-webhooks.md).
* Informationen zu verschiedenen Methoden zum Starten eines Runbooks finden Sie unter [Starten eines Runbooks](automation-starting-a-runbook.md).
* Weitere Informationen zum Erstellen von Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Informationen zum Erstellen eines Near Real-Time Alerts finden Sie unter [Erstellen einer Warnungsregel mit dem Azure-Portal](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).
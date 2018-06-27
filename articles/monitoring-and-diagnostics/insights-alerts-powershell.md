---
title: Verwenden von PowerShell zum Erstellen von klassischen Warnungen für Azure-Dienste | Microsoft-Dokumentation
description: E-Mails oder Benachrichtigungen bzw. URLs (Webhooks) von Websites oder Automatisierung werden ausgelöst bzw. aufgerufen, wenn die von Ihnen angegebenen Bedingungen erfüllt sind.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: b08a8f66add45d64085ac05605fe3c7d7f91b705
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286198"
---
# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Verwenden von PowerShell zum Erstellen von Warnungen für Azure-Dienste
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt jetzt [neuere, bessere Metrikwarnungen](monitoring-near-real-time-metric-alerts.md). Diese Warnungen ermöglichen die Überwachung mehrerer Metriken sowie Warnungen für dimensionale Metriken. PowerShell-Unterstützung für neuere Metrikwarnungen wird bald verfügbar sein.
>
>

In diesem Artikel erfahren Sie, wie Sie klassische Azure-Metrikwarnungen mit PowerShell einrichten können.  

Möglicherweise erhalten Sie auf Metriken basierende Warnungen für Ihre Azure-Dienste, oder Sie können Warnungen für Ereignisse empfangen, die in Azure auftreten können.

* **Metrikwerte**: Die Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen von Ihnen festgelegten Schwellenwert in beliebiger Richtung passiert. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch dann, wenn diese Bedingung nicht mehr erfüllt wird.    
* **Aktivitätsprotokollereignisse**: Eine Warnung kann für *jedes* Ereignis ausgelöst werden, oder dann, wenn bestimmte Ereignisse auftreten. Weitere Informationen zu Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen (klassisch)](monitoring-activity-log-alerts.md).

Sie können konfigurieren, dass bei Auslösung einer klassischen Metrikwarnung folgende Aufgaben ausgeführt werden:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mails an weitere von Ihnen angegebene Adressen.
* Aufrufen eines Webhooks
* Starten der Ausführung eines Azure-Runbooks (nur über das Azure-Portal).

Sie können von folgenden Standorten aus Warnungsregeln konfigurieren und zugehörige Informationen abrufen: 

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Um weitere Informationen zu erhalten, geben Sie ```Get-Help``` gefolgt von dem PowerShell-Befehl ein, zu dem Sie Hilfe benötigen.

## <a name="create-alert-rules-in-powershell"></a>Erstellen von Warnregeln in PowerShell
1. Melden Sie sich bei Azure an.   

    ```PowerShell
    Connect-AzureRmAccount

    ```
2. Rufen Sie eine Liste der Abonnements ab, die Ihnen zur Verfügung stehen. Stellen Sie sicher, dass Sie das richtige Abonnement verwenden. Falls nicht, rufen Sie mithilfe der Ausgabe von `Get-AzureRmSubscription` das richtige Abonnement ab.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden Sie den folgenden Befehl:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Um eine Regel zu erstellen, benötigen Sie zunächst verschiedene wichtige Informationen.

    - Die **Ressourcen-ID** der Ressource, für die Sie eine Warnung festlegen möchten.
    - Die für diese Ressource verfügbaren **Metrikdefinitionen**.

     Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Portal aus. Wählen Sie dann auf dem nächsten Blatt im Abschnitt **Einstellungen** die Option **Eigenschaften** aus. **RESSOURCEN-ID** ist ein Feld auf dem nächsten Blatt. 
     
     Sie können auch mithilfe des [Azure-Ressourcen-Explorers](https://resources.azure.com/) die Ressourcen-ID abrufen.

     Es folgt ein Beispiel einer Ressourcen-ID für eine Web-App:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Sie können `Get-AzureRmMetricDefinition` zum Anzeigen der Liste aller Definitionen von Metriken für eine bestimmte Ressource verwenden:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Im folgenden Beispiel wird eine Tabelle mit Namen und Einheit einer Metrik generiert:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Um eine vollständige Liste der verfügbaren Optionen für Get-AzureRmMetricDefinition zu erhalten, führen Sie `Get-Help Get-AzureRmMetricDefinition -Detailed` aus.
5. Im folgenden Beispiel wird eine Warnung für eine Websiteressource eingerichtet. Die Warnung wird ausgelöst, wenn fünf Minuten durchgängig Datenverkehr empfangen wird. Sie wird erneut ausgelöst, wenn fünf Minuten lang kein Datenverkehr empfangen wird.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Um einen Webhook zu erstellen oder E-Mail zu senden, wenn eine Warnung ausgelöst wird, müssen Sie zunächst die E-Mail-Adresse oder den Webhook erstellen. Erstellen Sie anschließend sofort die Regel mit dem „-Actions“-Tag (wie im folgenden Beispiel gezeigt). Sie können Webhooks oder E-Mail-Adressen nicht bereits erstellten Regeln zuordnen.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Betrachten Sie die einzelnen Regeln, um zu überprüfen, ob die Warnungen ordnungsgemäß erstellt wurden.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Löschen Sie Ihre Warnungen. Mit diesen Befehlen werden die zuvor in diesem Artikel erstellten Regeln gelöscht.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Azure-Überwachung](monitoring-overview.md) einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Weitere Informationen zum [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](insights-webhooks-alerts.md).
* Weitere Informationen zum [Erstellen von Aktivitätsprotokollwarnungen (klassisch)](monitoring-activity-log-alerts.md).
* Erfahren Sie mehr zu [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Verschaffen Sie sich einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md) , um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.

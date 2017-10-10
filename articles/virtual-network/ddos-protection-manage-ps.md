---
title: Verwalten des Azure DDoS-Schutzstandards mithilfe von Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Azure DDoS-Schutzstandard mithilfe von Azure PowerShell verwalten.
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: a1a3688d4ff215d05d2f78cdfa7d402e3fc20be2
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Verwalten des Azure DDoS-Schutzstandards mithilfe von Azure PowerShell

>[!IMPORTANT]
>Der Azure DDoS-Schutzstandard (DDoS-Schutz) befindet sich derzeit in der Vorschauversion. Der DDoS-Schutz wird von einer begrenzten Anzahl von Azure-Ressourcen und in ausgewählten Regionen unterstützt. Sie müssen sich während der eingeschränkten Vorschau [für den Dienst registrieren](http://aka.ms/ddosprotection), um den DDoS-Schutz für Ihr Abonnement zu aktivieren. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung, um Sie durch den Aktivierungsprozess zu leiten. Der DDoS-Schutz ist in den Regionen „USA, Osten“, „USA, Westen“ und „USA, Westen-Mitte“ verfügbar. Während der Vorschauphase entstehen für Sie für die Nutzung des Diensts keine Kosten.

In diesem Artikel erfahren Sie, wie Sie Azure PowerShell für die Aktivierung und Deaktivierung des DDoS-Schutzes und für die Nutzung der Telemetrie zur Entschärfung eines Angriffs verwenden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. Wenn Sie PowerShell installieren oder aktualisieren müssen, finden Sie weitere Informationen unter [Install Azure PowerShell module (Installieren des Azure PowerShell-Moduls)](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection"></a>Aktivieren des DDoS-Schutzes

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Erstellen eines neuen virtuellen Netzwerks und Aktivieren des DDoS-Schutzes

Führen Sie das folgende Beispiel aus, um ein virtuelles Netzwerk mit aktiviertem DDoS-Schutz zu erstellen:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6 -EnableDDoSProtection
```

Dieses Beispiel erstellt ein virtuelles Netzwerk mit zwei Subnetzen und zwei DNS-Servern. Als Auswirkung der Angabe von DNS-Servern im virtuellen Netzwerk erben die in diesem virtuellen Netzwerk bereitgestellten NICs/VMs die DNS-Server als Standards. Der DDoS-Schutz ist für alle geschützten Ressourcen im virtuellen Netzwerk aktiviert.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Aktivieren des DDoS-Schutzes in einem vorhandenen virtuellen Netzwerk

Führen Sie das folgende Beispiel aus, um den DDoS-Schutz in einem vorhandenen virtuellen Netzwerk zu aktivieren:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks
```

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Deaktivieren des DDoS-Schutzes in einem virtuellen Netzwerk

Führen Sie das folgende Beispiel aus, um den DDoS-Schutz in einem virtuellen Netzwerk zu deaktivieren:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks"
```

## <a name="review-the-ddos-protection-status-of-virtual-networks"></a>Überprüfen des DDoS-Schutzstatus von virtuellen Netzwerken 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>Verwenden der DDoS-Schutztelemetrie

Die Telemetrie für einen Angriff wird in Echtzeit durch Azure Monitor bereitgestellt. Die Telemetrie ist nur so lange verfügbar, wie eine öffentliche IP-Adresse der Entschärfung unterliegt. Vor oder nach der Entschärfung einer Attacke wird Ihnen keine Telemetrie angezeigt.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Konfigurieren von Warnungen in DDoS-Schutzmetriken

Durch die Nutzung der Warnungskonfiguration von Azure Monitor können Sie jede der verfügbaren DDoS-Schutzmetriken zum Warnen auswählen, wenn während eines Angriffes eine aktive Entschärfung vorhanden ist.

#### <a name="configure-email-alert-rules-via-azure-powershell"></a>Konfigurieren von E-Mail-Warnungsregeln über Azure PowerShell

1. Rufen Sie eine Liste der Abonnements ab, die Ihnen zur Verfügung stehen. Stellen Sie sicher, dass Sie das richtige Abonnement verwenden. Falls nicht, legen Sie mithilfe der Ausgabe von „Get-AzureRmSubscription“ das richtige Abonnement fest. 

    ```powershell
    Get-AzureRmSubscription 
    Get-AzureRmContext 
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

2. Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden Sie den folgenden Befehl: 

    ```powershell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

3. Um eine Regel zu erstellen, benötigen Sie zunächst verschiedene wichtige Informationen. 

    - Die Ressourcen-ID der Ressource, für die Sie eine Warnung festlegen möchten
    - Die für diese Ressource verfügbaren Metrikdefinitionen. Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Azure-Portal aus. Klicken Sie anschließend auf der nächsten Seite im Abschnitt *Einstellungen* auf *Eigenschaften*. Die **Ressourcen-ID** ist ein Feld auf der nächsten Seite. Eine andere Möglichkeit ist der [Azure-Ressourcen-Explorer](https://resources.azure.com/). Ein Beispiel einer Ressourcen-ID für eine IP ist `/subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`.

    Das folgende Beispiel richtet eine Warnung auf einer öffentlichen IP ein, die einem virtuellen Netzwerk zugeordnet ist. Die Metrik, für die ein Wert erstellt werden soll, steht **Unter DDoS-Angriff oder nicht**. Dies ist ein boolescher Wert 1 oder 0. **1** bedeutet, dass Sie angegriffen werden. **0** bedeutet, dass Sie nicht angegriffen werden. Die Warnung wird erstellt, wenn Sie innerhalb der letzten fünf Minuten dem Angriff ausgesetzt waren.

    Sie müssen zunächst die E-Mail und/oder Webhooks erstellen, um bei der Erstellung einer Warnung einen Webhook zu erstellen oder eine E-Mail zu senden. Erstellen Sie anschließend sofort die Regel mit dem „-Actions“-Tag (wie im folgenden Beispiel gezeigt). Sie können Webhooks oder E-Mails nicht mit bereits erstellten Regeln zuordnen, indem Sie PowerShell verwenden.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail-Description "Under DDoS Attack" 
    ```

4. Überprüfen Sie, ob die Warnungen ordnungsgemäß erstellt wurden, indem Sie die einzelnen Regeln untersuchen.

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

Sie können auch mehr über das [Konfigurieren von Webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) und über [Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) für die Erstellung von Warnungen erfahren.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfigurieren der Protokollierung in DDoS-Schutzmetriken

Weitere Informationen über den Zugriff und das Konfigurieren der Azure-Diagnoseprotokollierung über PowerShell finden Sie unter [PowerShell-Schnellstartbeispiele](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

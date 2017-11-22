---
title: Verwalten von Azure DDoS-Schutz Standard mithilfe von Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Azure DDoS-Schutz Standard mithilfe von Azure PowerShell verwalten.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: baac97db61b84000557e7150a64ffb64d81ce00c
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Verwalten von Azure DDoS-Schutz Standard mithilfe von Azure PowerShell

Erfahren Sie, wie Sie den DDoS-Schutz (Distributed Denial of Service) aktivieren oder deaktivieren und die Telemetrie von Azure DDoS-Schutz Standard verwenden, um einen DDoS-Angriff zu entschärfen. Der DDoS-Schutz Standard schützt Azure-Ressourcen wie virtuelle Computer, Lastenausgleichsmodule und Anwendungsgateways, denen eine [öffentliche Azure-IP-Adresse](virtual-network-public-ip-address.md) zugewiesen ist. Weitere Informationen zum DDoS-Schutz Standard und seinen Funktionen finden Sie unter [Übersicht über den DDoS-Schutz Standard](ddos-protection-overview.md). 

>[!IMPORTANT]
>Der Azure DDoS-Schutz Standard (DDoS-Schutz) befindet sich derzeit in der Vorschauversion. Der DDoS-Schutz wird von einer begrenzten Anzahl von Azure-Ressourcen unterstützt und ist nur in ausgewählten Regionen verfügbar. Eine Liste der verfügbaren Regionen finden Sie unter [Übersicht über den DDoS-Schutz Standard](ddos-protection-overview.md). Sie müssen sich während der eingeschränkten Vorschau [für den Dienst registrieren](http://aka.ms/ddosprotection), um den DDoS-Schutz für Ihr Abonnement zu aktivieren. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung und führt Sie durch den Aktivierungsprozess.


## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. Wenn Sie PowerShell installieren oder aktualisieren müssen, finden Sie weitere Informationen unter [Install Azure PowerShell module (Installieren des Azure PowerShell-Moduls)](/powershell/azure/install-azurerm-ps).

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>Aktivieren von DDoS-Schutz Standard – Neues virtuelles Netzwerk

Führen Sie das folgende Beispiel aus, um ein virtuelles Netzwerk mit aktiviertem DDoS-Schutz zu erstellen:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -EnableDDoSProtection
```

Dieses Beispiel erstellt ein virtuelles Netzwerk mit zwei Subnetzen und zwei DNS-Servern. Als Auswirkung der Angabe von DNS-Servern im virtuellen Netzwerk erben die in diesem virtuellen Netzwerk bereitgestellten NICs/VMs die DNS-Server als Standards. Der DDoS-Schutz ist für alle geschützten Ressourcen im virtuellen Netzwerk aktiviert.

> [!WARNING]
> Beim Auswählen einer Region wählen Sie eine unterstützte Region aus der Liste in der [Übersicht über den Azure DDoS-Schutz Standard](ddos-protection-overview.md) aus.

## <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Aktivieren des DDoS-Schutzes in einem vorhandenen virtuellen Netzwerk

Führen Sie das folgende Beispiel aus, um den DDoS-Schutz in einem vorhandenen virtuellen Netzwerk zu aktivieren:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks -Force
```

> [!WARNING]
> Das virtuelle Netzwerk muss in einer unterstützten Region vorhanden sein. Eine Liste der unterstützten Regionen finden Sie unter [Übersicht über den Azure DDoS-Schutz Standard](ddos-protection-overview.md).

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Deaktivieren des DDoS-Schutzes in einem virtuellen Netzwerk

Führen Sie das folgende Beispiel aus, um den DDoS-Schutz in einem virtuellen Netzwerk zu deaktivieren:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks" -Force
```

## <a name="review-the-ddos-protection-status-of-a-virtual-network"></a>Überprüfen des DDoS-Schutzstatus eines virtuellen Netzwerks 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>Verwenden der DDoS-Schutztelemetrie

Die Telemetrie für einen Angriff wird in Echtzeit durch Azure Monitor bereitgestellt. Die Telemetrie ist nur so lange verfügbar, wie eine öffentliche IP-Adresse der Entschärfung unterliegt. Vor oder nach der Entschärfung einer Attacke wird Ihnen keine Telemetrie angezeigt.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Konfigurieren von Warnungen in DDoS-Schutzmetriken

Durch die Nutzung der Warnungskonfiguration von Azure Monitor können Sie jede der verfügbaren DDoS-Schutzmetriken zum Warnen auswählen, wenn während eines Angriffes eine aktive Entschärfung vorhanden ist.

#### <a name="configure-email-alert-rules-via-azure"></a>Konfigurieren von E-Mail-Warnungsregeln über Azure

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

3. Zum Erstellen einer Regel benötigen Sie zuerst die folgenden Informationen: 

    - Die Ressourcen-ID der Ressource, für die Sie eine Warnung festlegen möchten
    - Die für diese Ressource verfügbaren Metrikdefinitionen. Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Azure-Portal aus. Klicken Sie anschließend auf der nächsten Seite im Abschnitt *Einstellungen* auf *Eigenschaften*. Die **Ressourcen-ID** ist ein Feld auf der nächsten Seite. Eine andere Möglichkeit ist der [Azure-Ressourcen-Explorer](https://resources.azure.com/). Ein Beispiel einer Ressourcen-ID für eine IP ist `/subscriptions/<Id>/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`.

    Im folgenden Beispiel wird eine Warnung für eine öffentliche IP-Adresse erstellt, die einer Ressource in einem virtuellen Netzwerk zugeordnet ist. Die Metrik, für die ein Wert erstellt werden soll, steht **Unter DDoS-Angriff oder nicht**. Dies ist ein boolescher Wert 1 oder 0. **1** bedeutet, dass Sie angegriffen werden. **0** bedeutet, dass Sie nicht angegriffen werden. Die Warnung wird erstellt, wenn ein Angriff innerhalb der letzten 5 Minuten gestartet wurde.

    Um beim Erstellen einer Warnung einen Webhook zu erstellen oder eine E-Mail zu senden, muss zunächst die E-Mail und/oder der Webhook erstellt werden. Nachdem Sie die E-Mail oder den Webhook erstellt haben, erstellen Sie sofort die Regel mit dem `-Actions`-Tag, wie es im folgenden Beispiel gezeigt ist. Sie können Webhooks oder E-Mails keine vorhandenen Regeln mithilfe von PowerShell zuordnen.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/<Id>/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail -Description "Under DDoS Attack"
    ```

4. Überprüfen Sie, ob die Warnung ordnungsgemäß erstellt wurde, indem Sie sich die Regel ansehen:

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    ```

Sie können auch mehr über das [Konfigurieren von Webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und über [Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) für die Erstellung von Warnungen erfahren.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfigurieren der Protokollierung in DDoS-Schutzmetriken

Weitere Informationen über den Zugriff und das Konfigurieren der Azure-Diagnoseprotokollierung über PowerShell finden Sie unter [PowerShell-Schnellstartbeispiele](../monitoring-and-diagnostics/insights-powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
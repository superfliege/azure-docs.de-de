---
title: Azure PowerShell-Skriptbeispiel – Einschränken des Webdatenverkehrs | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen Sie ein Anwendungsgateway mit einer Web Application Firewall und einer VM-Skalierungsgruppe, die OWASP-Regeln verwendet, um den Datenverkehr einzuschränken.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d6f8083e0070435e6ad8bb0dc0ff520b58c7eead
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="restrict-web-traffic-using-azure-powershell"></a>Einschränken von Webdatenverkehr mithilfe von Azure PowerShell

Dieses Skript erstellt ein Anwendungsgateway mit einer Web Application Firewall, das eine VM-Skalierungsgruppe als Back-End-Server verwendet. Die Web Application Firewall schränkt den Webdatenverkehr anhand von OWASP-Regeln ein. Nach dem Ausführen des Skripts können Sie das Anwendungsgateway mit dessen öffentlicher IP-Adresse testen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss-waf.ps1 "Create application gateway with WAF")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt die Subnetzkonfiguration. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt das virtuelle Netzwerk mit den Subnetzkonfigurationen. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Erstellt die öffentliche IP-Adresse für das Anwendungsgateway. |
| [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) | Erstellt die Konfiguration, die einem Subnetz das Anwendungsgateway zuordnet. |
| [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) | Erstellt die Konfiguration, die dem Anwendungsgateway eine öffentliche IP-Adresse zuordnet. |
| [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) | Weist einen Port zu, der für den Zugriff auf das Anwendungsgateway verwendet wird. |
| [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) | Erstellt einen Back-End-Pool für ein Anwendungsgateway. |
| [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) | Konfiguriert Einstellungen für einen Back-End-Pool. |
| [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) | Erstellt einen Listener. |
| [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) | Erstellt eine Routingregel. |
| [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) | Angeben von Ebene und Kapazität für ein Anwendungsgateway. |
| [New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewaywebapplicationfirewallconfiguration) | Erstellt die Web Application Firewall-Konfiguration. |
| [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) | Erstellen eines Anwendungsgateways |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Erstellen eines Speicherprofils für die Skalierungsgruppe. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Definieren des Betriebssystems für die Skalierungsgruppe. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Definieren der Netzwerkschnittstelle für die Skalierungsgruppe. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) | Erstellen einer VM-Skalierungsgruppe. |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Erstellt ein Speicherkonto. |
| [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) | Konfiguriert die Diagnose, um Daten aufzuzeichnen. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Ruft die öffentliche IP-Adresse für ein Anwendungsgateway ab. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. | 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele zu Anwendungsgateways finden Sie in der [Dokumentation zu Azure Application Gateway](../powershell-samples.md).

---
title: 'Azure PowerShell-Beispiele: Zonenredundante Skalierungsgruppe | Microsoft-Dokumentation'
description: Azure PowerShell-Beispiele
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b121684b3b9a5d03fe89e0892179140e9f5de80f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>Erstellen einer zonenredundanten VM-Skalierungsgruppe mit PowerShell
Dieses Skript erstellt eine VM-Skalierungsgruppe unter Windows Server 2016, die sich über mehrere Verfügbarkeitszonen erstreckt. Nach dem Ausführen des Skripts können Sie per RDP auf den virtuellen Computer zugreifen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1 "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt ein Konfigurationsobjekt, mit dem das Subnetz des virtuellen Netzwerks definiert wird. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt ein virtuelles Netzwerk und ein Subnetz. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Erstellt eine statische öffentliche IP-Adresse. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Erstellt ein Konfigurationsobjekt, mit dem das Front-End für den Lastenausgleich (einschließlich der öffentlichen IP-Adresse) definiert wird. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Erstellt ein Konfigurationsobjekt, mit dem das Back-End für den Lastenausgleich definiert wird. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Erstellt einen Lastenausgleich auf der Grundlage der Front-End- und Back-End-Konfigurationsobjekte. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Erstellt einen Integritätstest für den Lastenausgleich, um den Datenverkehr für TCP-Port 80 zu überwachen. Wenn innerhalb eines Intervalls von 15 Sekunden zwei aufeinander folgende Fehler auftreten, wird der Endpunkt als fehlerhaft angesehen. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Erstellt ein Konfigurationsobjekt zum Definieren von Lastenausgleichsregeln, die dafür sorgen, dass Datenverkehr am TCP-Port 80 vom Front-End-Pool an den Back-End-Pool verteilt wird. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Aktualisiert den Lastenausgleich mit dem Integritätstest und Lastenausgleichsregeln. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Erstellt ein Konfigurationsobjekt, mit dem eine Netzwerksicherheitsgruppen-Regel definiert wird, um Datenverkehr am TCP-Port 80 zuzulassen. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Erstellt eine Netzwerksicherheitsgruppe und eine dazugehörige Regel. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Aktualisiert das Subnetz des virtuellen Netzwerks, das der Netzwerksicherheitsgruppe zugeordnet werden soll. Alle VM-Instanzen, die mit dem Subnetz verbunden sind, erben die Netzwerksicherheitsgruppen-Regeln. |
| [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) | Aktualisiert das virtuelle Netzwerk mit dem Subnetz und der Konfiguration der Netzwerksicherheitsgruppe. |
| [New-AzureRmVmssIpConfig](/powershell/module/azurerm.compute/new-azurermvmssipconfig) | Erstellt ein Konfigurationsobjekt, mit dem die IP-Adressinformationen der VM-Skalierungsgruppe definiert werden, um für die VM-Instanzen eine Verbindung mit dem Back-End-Pool und dem NAT-Eingangspool des Lastenausgleichs herzustellen.
| [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) | Erstellt ein Konfigurationsobjekt, mit dem die Anzahl von VM-Instanzen in einer Skalierungsgruppe, die Größe der VM-Instanzen und der Upgraderichtlinienmodus definiert werden. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Erstellt ein Konfigurationsobjekt, mit dem das VM-Image für die VM-Instanzen definiert wird. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Erstellt ein Konfigurationsprofil, mit dem der VM-Instanzname und die Benutzeranmeldeinformationen definiert werden. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Erstellt ein Konfigurationsobjekt, mit dem die virtuelle Netzwerkschnittstellenkarte für die einzelnen VM-Instanzen definiert wird. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Kombiniert alle Konfigurationsobjekte, um die VM-Skalierungsgruppe zu erstellen. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Ruft Informationen zu einer VM-Skalierungsgruppe ab. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Fügt eine VM-Erweiterung für das benutzerdefinierte Skript hinzu, um eine einfache Webanwendung zu installieren. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Aktualisiert das Modell der VM-Skalierungsgruppe, um die VM-Erweiterung anzuwenden. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Ruft Informationen zu der zugewiesenen öffentlichen IP-Adresse ab, die vom Lastenausgleich verwendet wird. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für VM-Skalierungsgruppen finden Sie in der [Dokumentation zu Azure-VM-Skalierungsgruppen](../powershell-samples.md).
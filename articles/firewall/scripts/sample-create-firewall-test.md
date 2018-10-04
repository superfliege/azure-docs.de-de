---
title: 'Azure PowerShell-Skriptbeispiel: Erstellen einer Azure Firewall-Testumgebung'
description: 'Azure PowerShell-Skriptbeispiel: Erstellen einer Azure Firewall-Testumgebung'
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 63b34b6ddc1809031dc66fb3e41fa4a22d9f4a03
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182789"
---
# <a name="create-an-azure-firewall-test-environment"></a>Erstellen einer Azure Firewall-Testumgebung

Dieses Skriptbeispiel erstellt eine Firewall und eine Netzwerkumgebung zu Testzwecken. Das Netzwerk umfasst ein einzelnes VNet mit drei Subnetzen: *AzureFirewallSubnet*, *ServersSubnet* und *JumpboxSubnet*. „ServersSubnet“ und „JumpboxSubnet“ enthalten jeweils einen Windows Server mit zwei Kernen.

Die Firewall befindet sich in „AzureFirewallSubnet“ und wird mit einer Anwendungsregelsammlung mit einer einzelnen Regel konfiguriert, die den Zugriff auf www.microsoft.com ermöglicht.

Es wird eine benutzerdefinierte Route erstellt, die den Netzwerkverkehr von „ServersSubnet“ durch die Firewall leitet, wo die Firewallregeln angewendet werden.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/powershell) oder über eine lokale PowerShell-Installation ausführen. 

Wenn Sie PowerShell lokal ausführen, müssen Sie für dieses Skript die aktuelle Version des AzureRM PowerShell-Moduls (6.9.0 oder höher) verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. 

Sollte ein Upgrade erforderlich sein, können Sie `PowerShellGet` verwenden (in Windows 10 und Windows Server 2016 integriert).

> [!NOTE]
>Bei Verwendung einer anderen Windows-Version müssen Sie `PowerShellGet` installieren, um es verwenden zu können. Durch Ausführen von `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` können Sie ermitteln, ob es auf Ihrem System installiert ist. Ist die Ausgabe leer, installieren Sie die neueste Version von [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Weitere Informationen finden Sie unter [Installieren von Azure PowerShell unter Windows mit PowerShellGet](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0).

Vorhandene Azure PowerShell-Installationen, die über den Webplattform-Installer durchgeführt wurden, führen zu einem Konflikt mit der PowerShellGet-Installation und müssen entfernt werden.

Zur Erinnerung: Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure zu erstellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt ein Subnetzkonfigurationsobjekt |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Erstellt Sicherheitsregeln, die einer Netzwerksicherheitsgruppe zugewiesen werden sollen |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Erstellt NSG-Regeln, die bestimmte Ports bei bestimmten Subnetzen zulassen oder blockieren. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Ordnet NSGs Subnetzen zu. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Erstellt virtuelle Netzwerkschnittstellen und fügt sie an das Front-End- und Back-End-Subnetz des virtuellen Netzwerks an. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Name, Betriebssystem und Administratoranmeldeinformationen. Die Konfiguration wird während der VM-Erstellung verwendet. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Erstellen Sie eine VM. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |
|[New-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewall?view=azurermps-6.9.0)| Erstellt eine neue Azure Firewall.|
|[Get-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermfirewall?view=azurermps-6.9.0)|Ruft ein Azure Firewall-Objekt ab.|
|[New-AzureRmFirewallApplicationRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewallapplicationrule?view=azurermps-6.9.0)|Erstellt eine neue Azure Firewall-Anwendungsregel.|
|[Set-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermfirewall?view=azurermps-6.9.0)|Führt ein Commit für die Änderungen am Azure Firewall-Objekt aus.|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).


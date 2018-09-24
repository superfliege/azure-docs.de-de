---
title: Bereitstellen von Azure Firewall mit einer Vorlage
description: Bereitstellen von Azure Firewall mit einer Vorlage
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991333"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Bereitstellen von Azure Firewall mit einer Vorlage

Diese Vorlage erstellt eine Firewall und eine Testnetzwerkumgebung. Das Netzwerk verfügt über ein VNET mit drei Subnetzen: *AzureFirewallSubnet*, *ServersSubnet* und *JumpboxSubnet*. ServersSubnet und JumpboxSubnet enthalten jeweils einen Windows Server mit zwei Kernen.

Die Firewall befindet sich in „AzureFirewallSubnet“ und wird mit einer Anwendungsregelsammlung mit einer einzelnen Regel konfiguriert, die den Zugriff auf www.microsoft.com ermöglicht.

Es wird eine benutzerdefinierte Route erstellt, die den Netzwerkverkehr von „ServersSubnet“ durch die Firewall leitet, wo die Firewallregeln angewendet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="template-location"></a>Speicherort der Vorlage

Die Vorlage befindet sich unter:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Lesen Sie die Einführung, und wenn Sie mit der Bereitstellung beginnen möchten, klicken Sie auf **Bereitstellung in Azure**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Untersuchen Sie zuerst die Ressourcen, die mit der Firewall erstellt wurden. Sie können dann den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe, die Firewall und alle dazugehörigen Ressourcen zu entfernen, wenn diese nicht mehr benötigt werden.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie die Azure Firewall-Protokolle überwachen:

- [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)


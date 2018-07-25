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
ms.openlocfilehash: 1a732e22d72c36afe11030e42bae529baa35df1a
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992511"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Bereitstellen von Azure Firewall mit einer Vorlage

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

In den Beispielen in den Azure Firewall-Artikeln wird davon ausgegangen, dass Sie bereits die Public Preview von Azure Firewall aktiviert haben. Weitere Informationen finden Sie unter [Aktivieren der Public Preview von Azure Firewall](public-preview.md).

Diese Vorlage erstellt eine Firewall und eine Testnetzwerkumgebung. Das Netzwerk verfügt über ein VNET mit drei Subnetzen: *AzureFirewallSubnet*, *ServersSubnet* und *JumpboxSubnet*. ServersSubnet und JumpboxSubnet enthalten jeweils einen Windows Server mit zwei Kernen.

Die Firewall befindet sich im AzureFirewallSubnet und wird mit einer Anwendungsregelsammlung konfiguriert, die eine einzelne Regel umfasst, die den Zugriff auf www.microsoft.com ermöglicht.

Es wird eine benutzerdefinierte Route erstellt, die den Netzwerkverkehr vom ServerSubnet durch die Firewall leitet, wo die Firewallregeln angewendet werden.

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


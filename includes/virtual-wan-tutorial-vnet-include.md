---
title: Includedatei
description: Includedatei
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 660bbf50e1a8ae73bd7bbe1f7c42691ed62d276a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552970"
---
Falls Sie noch nicht über ein VNET verfügen, können Sie mit PowerShell schnell eins erstellen. Sie können auch über das Azure-Portal ein VNET erstellen.

* Stellen Sie sicher, dass sich der Adressraum für das von Ihnen erstellte VNET nicht mit den Adressbereichen anderer VNETs, mit denen Sie eine Verbindung herstellen möchten, oder mit Ihren Adressräumen des lokalen Netzwerks überlappt. 
* Wenn Sie bereits über ein VNET verfügen, sollten Sie sich vergewissern, dass es die erforderlichen Kriterien erfüllt und dass kein Gateway für virtuelle Netzwerke verwendet wird.

Sie können Ihr VNET leicht erstellen, indem Sie in diesem Artikel die Option zum Ausprobieren verwenden, um eine PowerShell-Konsole zu öffnen. Passen Sie die Werte an, und kopieren Sie die Befehle dann in das Konsolenfenster.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Passen Sie die PowerShell-Befehle an, und erstellen Sie anschließend eine Ressourcengruppe.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Erstellen eines VNET

Passen Sie die PowerShell-Befehle an, um das VNET zu erstellen, das für Ihre Umgebung kompatibel ist.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```
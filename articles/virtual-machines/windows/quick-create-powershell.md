---
title: 'Schnellstart: Erstellen eines virtuellen Windows-Computers mit Azure PowerShell | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure PowerShell einen virtuellen Windows-Computer erstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b199d031d4aaef4c1a707199f5692c86543e6360
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38767203"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-azure-with-powershell"></a>Schnellstart: Erstellen eines virtuellen Windows-Computers mit PowerShell in Azure

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit dem Azure PowerShell-Modul einen virtuellen Computer unter Windows Server 2016 in Azure bereitstellen. Wenn Sie den virtuellen Computer in Aktion sehen möchten, stellen Sie anschließend eine RDP-Verbindung mit dem virtuellen Computer her und installieren den IIS-Webserver.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) einen virtuellen Computer. Geben Sie Namen für die einzelnen Ressourcen an. Das Cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) erstellt sie dann für Sie, falls sie noch nicht vorhanden sind.

Geben Sie bei entsprechender Aufforderung einen Benutzernamen und ein Kennwort als Anmeldeinformationen für den virtuellen Computer ein:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389
```

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Stellen Sie nach Abschluss der Bereitstellung eine RDP-Verbindung mit dem virtuellen Computer her. Wenn Sie den virtuellen Computer in Aktion sehen möchten, muss dann der IIS-Webserver installiert werden.

Verwenden Sie zum Anzeigen der öffentlichen IP-Adresse des virtuellen Computers das Cmdlet [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress):

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung. Ersetzen Sie die IP-Adresse durch die öffentliche IP-Adresse Ihres virtuellen Computers. Geben Sie bei entsprechender Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Installieren des Webservers

Installieren Sie den IIS-Webserver, um den virtuellen Computer in Aktion zu sehen. Öffnen Sie eine PowerShell-Eingabeaufforderung auf dem virtuellen Computer, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Beenden Sie anschließend die RDP-Verbindung mit dem virtuellen Computer.

## <a name="view-the-web-server-in-action"></a>Anzeigen des Webservers in Aktion

Nachdem Sie IIS installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie einen Webbrowser Ihrer Wahl verwenden, um die IIS-Standardwillkommensseite anzuzeigen. Verwenden Sie die öffentliche IP-Adresse Ihres virtuellen Computers, die Sie in einem vorherigen Schritt abgerufen haben. Im folgenden Beispiel ist die IIS-Standardwebsite dargestellt:

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen nicht mehr benötigen, können Sie das Cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um sie zu entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen virtuellen Computer bereitgestellt, einen Netzwerkport für den Webdatenverkehr geöffnet und einen einfachen Webserver installiert. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)

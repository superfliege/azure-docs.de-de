---
title: Tutorial – Installieren von Anwendungen auf einem virtuellen Windows-Computer in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie die benutzerdefinierte Skripterweiterung zum Ausführen von Skripts und Bereitstellen von Anwendungen auf virtuellen Windows-Computern in Azure verwenden.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ab4c5c744733ac25243f0edbc7c9a760fc0682f6
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54883040"
---
# <a name="tutorial---deploy-applications-to-a-windows-virtual-machine-in-azure-with-the-custom-script-extension"></a>Tutorial: Bereitstellen von Anwendungen auf einem virtuellen Windows-Computer in Azure mit der benutzerdefinierten Skripterweiterung

Zum Konfigurieren von virtuellen Computern (VMs) auf schnelle und einheitliche Weise ist meist eine Form der Automatisierung erwünscht. Eine gängige Methode zum Anpassen von Windows-VMs ist die Verwendung der [benutzerdefinierten Skripterweiterung für Windows](extensions-customscript.md). In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="custom-script-extension-overview"></a>Übersicht über benutzerdefinierte Skripterweiterungen
Die benutzerdefinierte Skripterweiterung lädt Skripts auf Azure-VMs herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden.

Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden.

Sie können die benutzerdefinierte Skripterweiterung mit Windows- und Linux-VMs verwenden.


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) zuerst den Benutzernamen und das Kennwort des VM-Administrators fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Nun können nun mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) die VM erstellen. Im folgenden Beispiel wird eine VM mit dem Namen *myVM* für den Standort *EastUS* erstellt. Falls sie nicht bereits vorhanden sind, werden die Ressourcengruppe *myResourceGroupAutomate* und unterstützende Netzwerkressourcen erstellt. Um Webdatenverkehr zuzulassen, öffnet das Cmdlet auch Port *80*.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Credential $cred
```

Die Erstellung der Ressourcen und VM dauert einige Minuten.


## <a name="automate-iis-install"></a>Automatisieren der Installation von IIS
Verwenden Sie [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) zum Installieren der benutzerdefinierten Skripterweiterung. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus und aktualisiert dann die Seite *Default.htm* mit dem Hostnamen der VM:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroupAutomate" `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location "EastUS" `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
```


## <a name="test-web-site"></a>Testen der Website
Rufen Sie mit [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIPAddress* abgerufen, die wir zuvor erstellt haben:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myPublicIPAddress" | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einem Webbrowser ein. Die Website wird mit dem Hostnamen der VM angezeigt, an die der Load Balancer den Datenverkehr wie im folgenden Beispiel verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die IIS-Installation auf einem virtuellen Computer automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie benutzerdefinierte VM-Images erstellen.

> [!div class="nextstepaction"]
> [Erstellen von benutzerdefinierten VM-Images](./tutorial-custom-images.md)

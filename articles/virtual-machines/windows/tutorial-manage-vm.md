---
title: 'Tutorial: Erstellen und Verwalten von virtuellen Windows-Computern mit Azure PowerShell | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen und Verwalten virtueller Windows-Computer in Azure verwenden.
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
ms.date: 08/10/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ae29108aad2a538bb90484a048742be0b5c4764a
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094908"
---
# <a name="tutorial-create-and-manage-windows-vms-with-azure-powershell"></a>Tutorial: Erstellen und Verwalten von virtuellen Windows-Computern mit Azure PowerShell

Virtuelle Azure-Computer bieten eine vollständig konfigurierbare und flexible Computerumgebung. In diesem Tutorial werden grundlegende Vorgänge bei der Bereitstellung von virtuellen Azure-Computern behandelt, z.B. Auswählen einer VM-Größe, Auswählen eines VM-Images und Bereitstellen eines virtuellen Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers und Herstellen einer Verbindung mit ihm
> * Auswählen und Verwenden von VM-Images
> * Anzeigen und Verwenden bestimmter VM-Größen
> * Ändern der Größe eines virtuellen Computers
> * Anzeigen und Verstehen des Status von virtuellen Computern

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe.

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor dem virtuellen Computer muss eine Ressourcengruppe erstellt werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupVM* in der Region *EastUS* erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

Die Ressourcengruppe wird beim Erstellen oder Ändern eines virtuellen Computers angegeben und ist im gesamten Tutorial zu sehen.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Beim Erstellen eines virtuellen Computers stehen mehrere Optionen zur Verfügung, z.B. Betriebssystemimage, Netzwerkkonfiguration und Administratoranmeldeinformationen. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVM* erstellt, auf dem die neueste Standardversion von Windows Server 2016 Datacenter ausgeführt wird.

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="connect-to-vm"></a>Herstellen einer Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

Führen Sie die folgenden Befehle aus, um die öffentliche IP-Adresse des virtuellen Computers zurückzugeben. Notieren Sie sich diese IP-Adresse. Sie wird in einem späteren Schritt über den Browser aufgerufen, um die Webkonnektivität zu testen.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroupVM"  | Select IpAddress
```

Führen Sie auf Ihrem lokalen Computer den folgenden Befehl aus, um eine Remotedesktopsitzung mit dem virtuellen Computer zu erstellen. Ersetzen Sie die IP-Adresse mit dem *publicIPAddress*-Wert des virtuellen Computers. Geben Sie nach Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```powershell
mstsc /v:<publicIpAddress>
```

Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie für den virtuellen Computer erstellt haben, und klicken Sie dann auf **OK**.

## <a name="understand-vm-images"></a>Allgemeines zu VM-Images

Der Azure Marketplace umfasst viele VM-Images, die zum Erstellen eines neuen virtuellen Computers verwendet werden können. In den vorherigen Schritten wurde ein virtueller Computer mit dem Windows Server 2016 Datacenter-Image erstellt. In diesem Schritt wird der Marketplace mithilfe des PowerShell-Moduls nach weiteren Windows-Images durchsucht, die ebenfalls als Grundlage für neue virtuelle Computer verwendet werden können. Dieser Prozess umfasst die Suche nach dem Herausgeber, dem Angebot, der SKU und optional einer Versionsnummer zur [Identifizierung](cli-ps-findimage.md#terminology) des Images.

Führen Sie den Befehl [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) aus, um eine Liste mit Imageherausgebern abzurufen:

```azurepowershell-interactive
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Führen Sie den Befehl [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) aus, um eine Liste mit Imageangeboten abzurufen. Mit diesem Befehl wird die zurückgegebene Liste nach dem angegebenen Herausgeber gefiltert:

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          --------
Windows-HUB       MicrosoftWindowsServer EastUS
WindowsServer     MicrosoftWindowsServer EastUS
WindowsServer-HUB MicrosoftWindowsServer EastUS
```

Mit dem Befehl [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) wird dann nach dem Herausgeber und dem Angebotsnamen gefiltert und eine Liste mit Imagenamen zurückgegeben.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Mithilfe dieser Informationen kann ein virtueller Computer mit einem spezifischen Image bereitgestellt werden. In diesem Beispiel wird ein virtueller Computer mit der neuesten Version eines Images vom Typ „Windows Server 2016 mit Containern“bereitgestellt.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM2" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress2" `
    -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
    -Credential $cred `
    -AsJob
```

Mit dem Parameter `-AsJob` wird die VM als Hintergrundaufgabe erstellt, sodass die PowerShell-Aufforderungen für Sie zurückgegeben werden. Sie können die Details von Hintergrundaufträgen mit dem `Get-Job`-Cmdlet anzeigen.

## <a name="understand-vm-sizes"></a>Grundlegendes zu VM-Größen

Die Größe eines virtuellen Computers bestimmt die Menge an Computeressourcen (CPU, GPU, Arbeitsspeicher und Ähnliches), die für den virtuellen Computer zur Verfügung gestellt werden. Virtuelle Computer müssen mit einer angemessenen Größe für die erwartete Workload erstellt werden. Bei einer Zunahme der Workload kann die Größe eines vorhandenen virtuellen Computers geändert werden.

### <a name="vm-sizes"></a>VM-Größen

In der folgenden Tabelle sind Größen in Anwendungsfällen kategorisiert.  
| Typ                     | Gängige Größen           |    BESCHREIBUNG       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Allgemeiner Zweck](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Entwicklung und Tests, kleine bis mittlere Anwendungen und Datenlösungen.  |
| [Computeoptimiert](sizes-compute.md)   | Fs, F             | Hohes Verhältnis von CPU zu Arbeitsspeicher. Geeignet für Anwendungen, Network Appliances und Batch-Prozesse mit mittlerer Auslastung.        |
| [Arbeitsspeicheroptimiert](sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Hohes Verhältnis von Speicher zu Kern. Hervorragend geeignet für relationale Datenbanken, mittlere bis große Caches und In-Memory-Analysen.                 |
| [Speicheroptimiert](sizes-storage.md)      | Ls                | Datenträgerdurchsatz und -E/A auf hohem Niveau. Ideal für Big Data sowie SQL- und NoSQL-Datenbanken.                                                         |
| [GPU](sizes-gpu.md)          | NV, NC            | Spezialisierte virtuelle Computer für aufwendiges Grafikrendering und aufwendige Videobearbeitung.       |
| [Hohe Leistung](sizes-hpc.md) | H, A8-11          | Unsere virtuellen Computer mit den leistungsfähigsten CPUs, die optional über Netzwerkschnittstellen mit hohem Durchsatz (RDMA) verfügen. |

### <a name="find-available-vm-sizes"></a>Abrufen der verfügbaren VM-Größen

Eine Liste der in einer bestimmten Region verfügbaren VM-Größen können Sie mit dem Befehl [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) abrufen.

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

## <a name="resize-a-vm"></a>Ändern der Größe eines virtuellen Computers

Nach der Bereitstellung eines virtuellen Computers kann dessen Größe geändert werden, um die Ressourcenzuordnung zu erhöhen oder zu verringern.

Prüfen Sie vor der Größenänderung eines virtuellen Computers, ob die gewünschte Größe im aktuellen VM-Cluster verfügbar ist. Mit dem Befehl [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) wird eine Liste der Größen zurückgegeben.

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName "myResourceGroupVM" -VMName "myVM"
```

Wenn die gewünschte Größe verfügbar ist, kann die Größe des virtuellen Computers im eingeschalteten Zustand geändert werden, er muss jedoch während des Vorgangs neu gestartet werden.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
```

Wenn die gewünschte Größe im aktuellen Cluster nicht verfügbar ist, muss die Zuordnung des virtuellen Computers aufgehoben werden, damit die Größenänderung erfolgen kann. Beachten Sie, dass beim Wiedereinschalten des virtuellen Computers alle Daten auf dem temporären Datenträger entfernt werden und sich die öffentliche IP-Adresse ändert, sofern keine statische IP-Adresse verwendet wird.

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -Name $vm.name
```

## <a name="vm-power-states"></a>Betriebszustände von virtuellen Computern

Ein virtueller Azure-Computer kann einen von mehreren Betriebszuständen aufweisen. Dieser Zustand entspricht dem aktuellen Zustand des virtuellen Computers im Hypervisor.

### <a name="power-states"></a>Betriebszustände

| Betriebszustand | BESCHREIBUNG
|----|----|
| Wird gestartet | Gibt an, dass der virtuelle Computer gestartet wird. |
| Wird ausgeführt | Gibt an, dass der virtuelle Computer ausgeführt wird. |
| Wird beendet | Gibt an, dass der virtuelle Computer beendet wird. |
| Beendet | Gibt an, dass der virtuelle Computer beendet ist. Beachten Sie, dass für virtuelle Computer in beendetem Zustand weiterhin Computegebühren anfallen.  |
| Zuordnung wird aufgehoben | Gibt an, dass die Zuordnung des virtuellen Computers aufgehoben wird. |
| Zuordnung aufgehoben | Gibt an, dass der virtuelle Computer vollständig aus dem Hypervisor entfernt, auf Steuerungsebene jedoch weiterhin verfügbar ist. Für virtuelle Computer, deren Zuordnung aufgehoben ist, fallen keine Computegebühren an. |
| - | Gibt an, dass der Betriebszustand des virtuellen Computers nicht bekannt ist. |

### <a name="find-power-state"></a>Abrufen des Betriebszustands

Verwenden Sie zum Abrufen des Zustands eines bestimmten virtuellen Computers den Befehl [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm). Achten Sie darauf, dass Sie einen gültigen Namen für einen virtuellen Computer und eine Ressourcengruppe angeben.

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Ausgabe:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Verwaltungsaufgaben

Während der Lebensdauer eines virtuellen Computers können Sie Verwaltungsaufgaben wie das Starten, Beenden oder Löschen eines virtuellen Computers ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von wiederkehrenden oder komplexen Aufgaben zu erstellen. Mit Azure PowerShell können viele allgemeine Verwaltungsaufgaben über die Befehlszeile oder in Skripts ausgeführt werden.

### <a name="stop-virtual-machine"></a>Beenden des virtuellen Computers

Verwenden Sie [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm), um einen virtuellen Computer zu beenden und freizugeben:

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
```

Wenn der virtuelle Computer bereitgestellt bleiben soll, verwenden Sie den Parameter „-StayProvisioned“.

### <a name="start-virtual-machine"></a>Starten des virtuellen Computers

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM"
```

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroupVM" -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zur grundlegenden Erstellung und Verwaltung von virtuellen Computern erhalten, darunter:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers und Herstellen einer Verbindung mit ihm
> * Auswählen und Verwenden von VM-Images
> * Anzeigen und Verwenden bestimmter VM-Größen
> * Ändern der Größe eines virtuellen Computers
> * Anzeigen und Verstehen des Status von virtuellen Computern

Im nächsten Tutorial erhalten Sie Informationen zu VM-Datenträgern.  

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von VM-Datenträgern](./tutorial-manage-data-disk.md)

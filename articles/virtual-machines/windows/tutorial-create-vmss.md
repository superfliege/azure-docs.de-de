---
title: Tutorial – Erstellen einer VM-Skalierungsgruppe für Windows in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen und Bereitstellen einer hoch verfügbaren Anwendung auf virtuellen Windows-Computern mithilfe einer VM-Skalierungsgruppe verwenden.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 49754fd4409b1fbc6b15577d37e216290582ef2b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows-with-azure-powershell"></a>Tutorial: Erstellen einer VM-Skalierungsgruppe und Bereitstellen einer hoch verfügbaren App unter Windows mit Azure PowerShell
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Tutorial stellen Sie eine Skalierungsgruppe für virtuelle Computer bereit. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Definieren einer IIS-Website zur Skalierung
> * Erstellen eines Lastenausgleichs für Ihre Skalierungsgruppe
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="scale-set-overview"></a>Übersicht über Skalierungsgruppen
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Virtuelle Computer in einer Skalierungsgruppe werden in einer oder mehreren *Platzierungsgruppen* auf logische Fehler- und Updatedomänen verteilt. Hierbei handelt es sich um Gruppen ähnlich konfigurierter virtueller Computer, vergleichbar mit [Verfügbarkeitsgruppen](tutorial-availability-sets.md).

Virtuelle Computer werden nach Bedarf in einer Skalierungsgruppe erstellt. Sie können Regeln für die automatische Skalierung definieren, um zu steuern, wie und wann virtuelle Computer in der Skalierungsgruppe hinzugefügt oder entfernt werden. Diese Regeln können basierend auf Metriken wie CPU-Auslastung, Speicherauslastung oder Netzwerkdatenverkehr ausgelöst werden.

Bei Verwendung eines Azure-Plattformimages unterstützen Skalierungsgruppen bis zu 1.000 virtuelle Computer. Für Workloads mit beträchtlichen Installations- oder VM-Anpassungsanforderungen können Sie nach Wunsch ein [benutzerdefiniertes VM-Image erstellen](tutorial-custom-images.md). Bei Verwendung eines benutzerdefinierten Images können Sie bis zu 300 virtuelle Computer in einer Skalierungsgruppe erstellen.


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Erstellen Sie mit [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, für die das Plattformimage *Windows Server 2016 Datacenter* verwendet wird. Die Azure-Netzwerkressourcen für virtuelles Netzwerk, öffentliche IP-Adresse und Lastenausgleich werden automatisch erstellt. Geben Sie Ihre gewünschten Administratoranmeldeinformationen für die VM-Instanzen in der Skalierungsgruppe an, wenn Sie dazu aufgefordert werden:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic"
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.


## <a name="deploy-sample-application"></a>Bereitstellen der Beispielanwendung
Installieren Sie eine einfache Webanwendung, um Ihre Skalierungsgruppe zu testen. Die benutzerdefinierte Skripterweiterung von Azure wird zum Herunterladen und Ausführen eines Skripts verwendet, das IIS auf den VM-Instanzen installiert. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](extensions-customscript.md).

Verwenden Sie die benutzerdefinierte Skripterweiterung, um einen einfachen IIS-Webserver zu installieren. Wenden Sie die benutzerdefinierte Skripterweiterung, mit der IIS installiert wird, wie folgt an:

```azurepowershell-interactive
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroupScaleSet" `
            -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-scale-set"></a>Testen Ihrer Skalierungsgruppe
Um Ihre Skalierungsgruppe in Aktion zu sehen, rufen Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myPublicIPAddress" | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die Web-App wird mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-create-vmss/running-iis-site.png)

Um die Skalierungsgruppe in Aktion zu sehen, führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle virtuellen Computer in der Skalierungsgruppe verteilt, auf denen Ihre App ausgeführt wird.


## <a name="management-tasks"></a>Verwaltungsaufgaben
Während des Lebenszyklus der Skalierungsgruppe müssen Sie möglicherweise eine oder mehrere Verwaltungsaufgaben ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von verschiedenen Aufgaben im Lebenszyklus zu erstellen. Azure PowerShell bietet eine schnelle Möglichkeit, diese Aufgaben auszuführen. Im Folgenden sind einige allgemeine Aufgaben aufgeführt.

### <a name="view-vms-in-a-scale-set"></a>Anzeigen von virtuellen Computern in einer Skalierungsgruppe
Verwenden Sie [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) wie folgt, um in einer Skalierungsgruppe eine Liste mit den VM-Instanzen anzuzeigen:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet"
```

Die folgende Beispielausgabe zeigt zwei VM-Instanzen in der Skalierungsgruppe:

```powershell
ResourceGroupName                 Name Location             Sku InstanceID ProvisioningState
-----------------                 ---- --------             --- ---------- -----------------
MYRESOURCEGROUPSCALESET   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUPSCALESET   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Wenn Sie zusätzliche Informationen zu einer bestimmten VM-Instanz anzeigen möchten, fügen Sie [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) den Parameter `-InstanceId` hinzu. Im folgenden Beispiel werden Informationen zur VM-Instanz *1* angezeigt:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


### <a name="increase-or-decrease-vm-instances"></a>VM-Instanzen erhöhen oder verringern
Verwenden Sie [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss), und führen Sie eine Abfrage nach *sku.capacity* durch, um die Anzahl der zurzeit in einer Skalierungsgruppe vorhandenen Instanzen anzuzeigen:

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -VMScaleSetName "myScaleSet" | `
    Select -ExpandProperty Sku
```

Sie können dann die Anzahl der virtuellen Computer in der Skalierungsgruppe mit [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) manuell erhöhen oder verringern. Im folgenden Beispiel wird die Anzahl der virtuellen Computer in der Skalierungsgruppe auf *3* festgelegt:

```azurepowershell-interactive
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $scaleset
```

Es dauert einige Minuten, bis die angegebene Anzahl von Instanzen in der Skalierungsgruppe aktualisiert ist.


### <a name="configure-autoscale-rules"></a>Konfigurieren von Regeln zur automatischen Skalierung
Anstatt die Anzahl von Instanzen in Ihrer Skalierungsgruppe manuell zu skalieren, definieren Sie Regeln zur automatischen Skalierung. Diese Regeln überwachen die Instanzen in Ihrer Skalierungsgruppe und reagieren entsprechend basierend auf von Ihnen festgelegten Metriken und Schwellenwerten. Im folgenden Beispiel wird die Anzahl der Instanzen um eine Instanz horizontal hochskaliert, wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten höher als 60 % ist. Wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten unter 30 % fällt, werden die Instanzen um eine Instanz horizontal herunterskaliert:

```azurepowershell-interactive
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription)[0].Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
$myScaleSetId = (Get-AzureRmVmss -ResourceGroupName $myResourceGroup -VMScaleSetName $myScaleSet).Id 

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId $myScaleSetId `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5-minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId $myScaleSetId `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId $myScaleSetId `
  -AutoscaleProfiles $myScaleProfile
```

Weitere Entwurfsinformationen zur Verwendung der automatischen Skalierung finden Sie unter [Autoscaling](/azure/architecture/best-practices/auto-scaling) (Automatische Skalierung).


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine Skalierungsgruppe für virtuelle Computer bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden der Benutzerdefinierten Skripterweiterung zum Definieren einer IIS-Website zur Skalierung
> * Erstellen eines Lastenausgleichs für Ihre Skalierungsgruppe
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung

Im nächsten Tutorial erhalten Sie weitere Informationen zu den Konzepten des Lastenausgleichs für virtuelle Computer.

> [!div class="nextstepaction"]
> [Lastenausgleich für virtuelle Computer](tutorial-load-balancer.md)

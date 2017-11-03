---
title: Bereitstellen einer Anwendung in einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Anwendungen auf Instanzen von virtuellen Linux- und Windows-Computern in einer Skalierungsgruppe bereitstellen.
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: 0ff8a178d883e3b51294485e556e65da52dbf327
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Bereitstellen der App in VM-Skalierungsgruppen
Zum Ausführen von Anwendungen auf VM-Instanzen in einer Skalierungsgruppe müssen Sie zuerst die Anwendungskomponenten und erforderlichen Dateien installieren. In diesem Artikel werden Möglichkeiten zum Erstellen eines benutzerdefinierten VM-Image für Instanzen in einer Skalierungsgruppe oder zum automatischen Ausführen von Installationsskripts auf vorhandenen VM-Instanzen aufgezeigt. Außerdem erfahren Sie, wie Sie Anwendungs- oder Betriebssystemupdates für eine Skalierungsgruppe verwalten.


## <a name="build-a-custom-vm-image"></a>Erstellen eines benutzerdefinierten VM-Image
Wenn Sie eines der Azure-Plattformimages zum Erstellen der Instanzen in Ihrer Skalierungsgruppe erstellen, wird keine zusätzliche Software installiert oder konfiguriert. Sie können die Installation dieser Komponenten automatisieren, aber dies erhöht den Zeitaufwand für die Bereitstellung von VM-Instanzen in Ihren Skalierungsgruppen. Wenn Sie viele Konfigurationsänderungen auf die VM-Instanzen anwenden, fällt Verwaltungsaufwand für diese Konfigurationsskripts und -aufgaben an.

Zum Reduzieren der Konfigurationsverwaltung und der Zeit für die Bereitstellung einer VM können Sie ein benutzerdefiniertes VM-Image erstellen, das für die Ausführung Ihrer Anwendung bereitsteht, sobald in der Skalierungsgruppe eine Instanz bereitgestellt wird. Der gesamte Prozess zum Erstellen eines benutzerdefinierten VM-Image für Skalierungsgruppeninstanzen lautet wie folgt:

1. Zum Erstellen eines benutzerdefinierten VM-Image für Ihre Skalierungsgruppeninstanzen erstellen Sie eine VM und melden sich daran an und führen anschließend die Installation und Konfiguration für die Anwendung durch. Sie können Packer verwenden, um ein [Linux](../virtual-machines/linux/build-image-with-packer.md)- oder [Windows](../virtual-machines/windows/build-image-with-packer.md)-VM-Image zu definieren und zu erstellen. Oder Sie können die VM manuell erstellen und konfigurieren:

    - Erstellen Sie eine Linux-VM mit [Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md), [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md) oder über das [Portal](../virtual-machines/linux/quick-create-portal.md).
    - Erstellen Sie eine Windows-VM mit [Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md), [Azure CLI 2.0](../virtual-machines/windows/quick-create-cli.md) oder über das [Portal](../virtual-machines/windows/quick-create-portal.md).
    - Melden Sie sich an einer [Linux](../virtual-machines/linux/mac-create-ssh-keys.md#use-the-ssh-key-pair)- oder [Windows](../virtual-machines/windows/connect-logon.md)-VM an.
    - Installieren und konfigurieren Sie die erforderlichen Anwendungen und Tools. Wenn Sie bestimmte Versionen einer Bibliothek oder Laufzeit benötigen, können Sie mit einem benutzerdefinierten VM-Image eine Version definieren. 

2. Erfassen Sie Ihre VM mit [Azure CLI 2.0](../virtual-machines/linux/capture-image.md) oder [Azure PowerShell](../virtual-machines/windows/capture-image.md). In diesem Schritt wird das benutzerdefinierte VM-Image erstellt, mit dem dann Instanzen in einer Skalierungsgruppe bereitgestellt werden.

3. [Erstellen Sie eine Skalierungsgruppe](virtual-machine-scale-sets-create.md), und geben Sie das benutzerdefinierte VM-Image an, das in den vorherigen Schritten erstellt wurde.


## <a name="already-provisioned"></a>Installieren einer App mit der benutzerdefinierten Skripterweiterung
Die benutzerdefinierte Skripterweiterung lädt Skripts auf Azure-VMs herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden.

Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden. 

Weitere Informationen finden Sie unter [Übersicht über benutzerdefinierte Skripterweiterungen](../virtual-machines/windows/extensions-customscript.md).


### <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
In PowerShell wird eine Hashtabelle zum Speichern der herunterzuladenden Tabelle und des auszuführenden Befehls verwendet. Das folgende Beispiel:

- Die VM-Instanzen werden angewiesen, ein Skript aus GitHub herunterzuladen: *https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1*
- Die Erweiterung wird so festgelegt, dass ein Installationsskript ausgeführt wird: `powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1`
- Mit [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) werden Informationen zu einer Skalierungsgruppe abgerufen.
- Mit [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) wird die Erweiterung auf die VM-Instanzen angewendet.

Die benutzerdefinierte Skripterweiterung wird auf die *myScaleSet*-VM-Instanzen in der Ressourcengruppe mit dem Namen *myResourceGroup* angewendet. Geben Sie Ihre eigenen Namen wie folgt ein:

```powershell
# Define the script for your Custom Script Extension to run
$customConfig = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Wenn als Upgraderichtlinie für Ihre Skalierungsgruppe *manuell* festgelegt ist, sollten Sie Ihre VM-Instanzen mit [Update-AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance) aktualisieren. Mit diesem Cmdlet wird die aktualisierte Konfiguration für die Skalierungsgruppe auf die VM-Instanzen angewendet und Ihre Anwendung installiert.


### <a name="use-azure-cli-20"></a>Verwenden von Azure CLI 2.0
Zum Verwenden der benutzerdefinierten Skripterweiterung mit der Azure CLI erstellen Sie eine JSON-Datei, mit der definiert wird, welche Dateien beschafft und welche Befehle ausgeführt werden sollen. Diese JSON-Definitionen können für die Bereitstellungen von Skalierungsgruppen übergreifend wiederverwendet werden, um einheitliche Anwendungsinstallationen zu erhalten.

Erstellen Sie in der aktuellen Shell eine Datei namens *customConfig.json*, und fügen Sie die folgende Konfiguration ein. Erstellen Sie die Datei beispielsweise in Cloud Shell, nicht auf dem lokalen Computer. Dazu können Sie einen beliebigen Editor verwenden. Geben Sie `sensible-editor cloudConfig.json` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Wenden Sie die Konfiguration der benutzerdefinierten Skripterweiterung auf die VM-Instanzen in Ihrer Skalierungsgruppe an, indem Sie [az vmss extension set](/cli/azure/vmss/extension#set) verwenden. Im folgenden Beispiel wird die Konfiguration *customConfig.json* auf die *myScaleSet*-VM-Instanzen in der Ressourcengruppe *myResourceGroup* angewendet. Geben Sie Ihre eigenen Namen wie folgt ein:

```azurecli
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfig.json
```

Wenn als Upgraderichtlinie für Ihre Skalierungsgruppe *manuell* festgelegt ist, sollten Sie Ihre VM-Instanzen mit [az vmss update-instances](/cli/azure/vmss#update-instances) aktualisieren. Mit diesem Cmdlet wird die aktualisierte Konfiguration für die Skalierungsgruppe auf die VM-Instanzen angewendet und Ihre Anwendung installiert.


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Installieren einer App auf einer Windows-VM mit PowerShell DSC
[PowerShell DSC (Desired State Configuration)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) ist eine Verwaltungsplattform zum Definieren der Konfiguration von Zielcomputern. DSC-Konfigurationen definieren, was auf einem Computer installiert und wie der Host konfiguriert werden soll. Ein LCM-Modul (Local Configuration Manager) wird auf jedem Zielknoten ausgeführt, der angeforderte Aktionen auf der Grundlage gepushter Konfigurationen verarbeitet.

Mit der Erweiterung PowerShell DSC können Sie VM-Instanzen in einer Skalierungsgruppe mit PowerShell anpassen. Das folgende Beispiel:

- Die VM-Instanzen werden angewiesen, ein DSC-Paket von GitHub herunterzuladen: *https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip*
- Die Erweiterung wird so festgelegt, dass ein Installationsskript ausgeführt wird: `configure-http.ps1`
- Mit [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) werden Informationen zu einer Skalierungsgruppe abgerufen.
- Mit [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) wird die Erweiterung auf die VM-Instanzen angewendet.

Die DSC-Erweiterung wird auf die *myScaleSet*-VM-Instanzen in der Ressourcengruppe mit dem Namen *myResourceGroup* angewendet. Geben Sie Ihre eigenen Namen wie folgt ein:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Wenn als Upgraderichtlinie für Ihre Skalierungsgruppe *manuell* festgelegt ist, sollten Sie Ihre VM-Instanzen mit [Update-AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance) aktualisieren. Mit diesem Cmdlet wird die aktualisierte Konfiguration für die Skalierungsgruppe auf die VM-Instanzen angewendet und Ihre Anwendung installiert.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Installieren einer App auf einer Linux-VM mit „cloud-init“
[Cloud-init](https://cloudinit.readthedocs.io/latest/) ist ein weit verbreiteter Ansatz zum Anpassen einer Linux-VM beim ersten Start. Sie können mit cloud-init Pakete installieren und Dateien schreiben oder Benutzer und Sicherheit konfigurieren. Da cloud-init während des ersten Startvorgangs ausgeführt wird, müssen Sie keine zusätzlichen Schritte oder erforderlichen Agents auf Ihre Konfiguration anwenden.

Cloud-init funktioniert auch Distributionen übergreifend. Verwenden Sie z.B. nicht **apt-get install** oder **yum install**, um ein Paket zu installieren. Stattdessen können Sie eine Liste mit zu installierenden Paketen definieren. „cloud-init“ verwendet automatisch das native Paketverwaltungstool für die ausgewählte Distribution.

Weitere Informationen sowie die Beispieldatei *cloud-init.txt* finden Sie unter [Anpassen einer Linux-VM in Azure mit cloud-init](../virtual-machines/linux/using-cloud-init.md).

Fügen Sie zum Erstellen einer Skalierungsgruppe und Verwenden einer cloud-init-Datei dem Befehl [az vmss create](/cli/azure/vmss#create) den Parameter `--custom-data` hinzu, und geben Sie den Namen einer cloud-init-Datei an. Im folgenden Beispiel wird eine Skalierungsgruppe mit dem Namen *myScaleSet* in *myResourceGroup* erstellt, und VM-Instanzen werden mit einer Datei mit dem Namen *cloud-init.txt* konfiguriert. Geben Sie Ihre eigenen Namen wie folgt ein:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="install-applications-as-a-set-scales-out"></a>Installieren von Anwendungen beim horizontalen Hochskalieren einer Gruppe
Mit Skalierungsgruppen können Sie die Anzahl von VM-Instanzen erhöhen, von denen Ihre Anwendung ausgeführt wird. Dieser Prozess des horizontalen Hochskalierens kann manuell oder automatisch basierend auf Metriken, z.B. CPU- oder Arbeitsspeichernutzung, gestartet werden.

Wenn Sie eine benutzerdefinierte Skripterweiterung auf die Skalierungsgruppe angewendet haben, wird die Anwendung auf jeder neuen VM-Instanz installiert. Falls die Skalierungsgruppe auf einem benutzerdefinierten Image mit einer Vorinstallation der Anwendung basiert, wird jede neue VM-Instanz in einem verwendungsbereiten Zustand bereitgestellt. 

Wenn es sich bei den VM-Instanzen der Skalierungsgruppe um Containerhosts handelt, können Sie die benutzerdefinierte Skripterweiterung verwenden, um die benötigten Containerimages zu pullen und auszuführen. Mit der benutzerdefinierten Skripterweiterung kann die neue VM-Instanz auch bei einem Orchestrator, z.B. Azure Container Service, registriert werden.


## <a name="deploy-application-updates"></a>Bereitstellen von Anwendungsupdates
Wenn Sie den Anwendungscode, Bibliotheken oder Pakete aktualisieren, können Sie den aktuellen Anwendungszustand auf VM-Instanzen in einer Skalierungsgruppe pushen. Bei Verwendung der benutzerdefinierten Skripterweiterung werden Updates Ihrer Anwendung nicht automatisch bereitgestellt. Ändern Sie die Konfiguration des benutzerdefinierten Skripts beispielsweise so, dass auf ein Installationsskript verwiesen wird, das über einen aktualisierten Versionsnamen verfügt. Im vorherigen Beispiel wird für die benutzerdefinierte Skripterweiterung wie folgt ein Skript mit dem Namen *automate_nginx.sh* verwendet:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Alle Updates, die Sie an Ihrer Anwendung vornehmen, werden nur dann für die benutzerdefinierte Skripterweiterung verfügbar gemacht, wenn sich das Installationsskript ändert. Ein Ansatz besteht darin, eine Versionsnummer einzubinden, die gemäß den Releases Ihrer Anwendung erhöht wird. Die benutzerdefinierte Skripterweiterung kann nun wie folgt auf *automate_nginx_v2.sh* verweisen:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

Die benutzerdefinierte Skripterweiterung wird jetzt für die VM-Instanzen ausgeführt, um die aktuellen Anwendungsupdates anzuwenden.


### <a name="install-applications-with-os-updates"></a>Installieren von Anwendungen mit Betriebssystemupdates
Wenn neue Releases von Betriebssystemen verfügbar sind, können Sie ein neues benutzerdefiniertes Image verwenden oder erstellen und [Betriebssystemupgrades](virtual-machine-scale-sets-upgrade-scale-set.md) für eine Skalierungsgruppe bereitstellen. Jede VM-Instanz wird auf das aktuelle Image aktualisiert, das Sie angegeben haben. Sie können ein benutzerdefiniertes Image mit der vorinstallierten Anwendung, der benutzerdefinierten Skripterweiterung oder PowerShell DSC verwenden, damit die Anwendung automatisch verfügbar ist, wenn Sie das Upgrade durchführen. Unter Umständen müssen Sie die Anwendungswartung planen, wenn Sie diesen Prozess durchführen, um sicherzustellen, dass keine Probleme aufgrund der Kompatibilität von Versionen auftreten.

Wenn Sie ein benutzerdefiniertes VM-Image mit vorinstallierter Anwendung verwenden, können Sie die Anwendungsupdates in eine Bereitstellungspipeline integrieren, um die neuen Images zu erstellen und Betriebssystemupgrades für die gesamte Skalierungsgruppe bereitzustellen. Bei diesem Ansatz kann die Pipeline die aktuellen Builds der Anwendung nutzen, ein VM-Image erstellen und überprüfen und anschließend die VM-Instanzen in der Skalierungsgruppe aktualisieren. Zum Ausführen einer Bereitstellungspipeline, mit der Anwendungsupdates über benutzerdefinierte VM-Images hinweg erstellt und bereitgestellt werden, können Sie [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Spinnaker](https://www.spinnaker.io/) oder [Jenkins](https://jenkins.io/) verwenden.


## <a name="next-steps"></a>Nächste Schritte
Beim Erstellen und Bereitstellen von Anwendungen in Ihren Skalierungsgruppen helfen Ihnen die Informationen unter [Überlegungen zum Entwurf von Skalierungsgruppen](virtual-machine-scale-sets-design-overview.md) weiter. Weitere Informationen zum Verwalten Ihrer Skalierungsgruppe finden Sie unter [Manage a virtual machine scale set with Azure PowerShell](virtual-machine-scale-sets-windows-manage.md) (Verwalten einer VM-Skalierungsgruppe mit Azure PowerShell).

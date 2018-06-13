---
title: Erstellen einer Windows-VM-Skalierungsgruppe mit einer Azure-Vorlage | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit einer Azure Resource Manager-Vorlage schnell eine Windows-VM-Skalierungsgruppe erstellen, mit der eine Beispiel-App bereitgestellt wird und Regeln für die automatische Skalierung konfiguriert werden.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 1632411b0cfc2f8fa59f323436ee386e763a1ae0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2018
ms.locfileid: "30201069"
---
# <a name="create-a-windows-virtual-machine-scale-set-with-an-azure-template"></a>Erstellen einer Windows-VM-Skalierungsgruppe mit einer Azure-Vorlage
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Artikel zu den ersten Schritten erstellen Sie mit einer Azure Resource Manager-Vorlage eine VM-Skalierungsgruppe. Sie können eine Skalierungsgruppe auch per [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md), [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) oder mit dem [Azure-Portal](virtual-machine-scale-sets-create-portal.md) erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 4.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="define-a-scale-set-in-a-template"></a>Definieren einer Skalierungsgruppe in einer Vorlage
Mit Azure Resource Manager-Vorlagen können Sie Gruppen mit verwandten Ressourcen bereitstellen. Vorlagen werden in JSON (JavaScript Object Notation) geschrieben und definieren die gesamte Azure-Infrastrukturumgebung für Ihre Anwendung. In einer einzelnen Vorlage können Sie die VM-Skalierungsgruppe erstellen, Anwendungen installieren und Regeln für die automatische Skalierung konfigurieren. Bei Nutzung von Variablen und Parametern kann diese Vorlage wiederverwendet werden, um vorhandene Skalierungsgruppen zu aktualisieren oder zusätzliche zu erstellen. Sie können Vorlagen über das Azure-Portal, Azure CLI 2.0 oder Azure PowerShell sowie über CI/CD-Pipelines (Continuous Integration/Continuous Delivery) bereitstellen.

Weitere Informationen zu Vorlagen finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment).

Mit einer Vorlage wird die Konfiguration für die einzelnen Ressourcentypen definiert. Der Ressourcentyp einer VM-Skalierungsgruppe ist mit einer individuellen VM vergleichbar. Dies sind die wichtigsten Bestandteile des Ressourcentyps einer VM-Skalierungsgruppe:

| Eigenschaft                     | Beschreibung der Eigenschaft                                  | Beispiel für Vorlagenwert                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| type                         | Zu erstellender Azure-Ressourcentyp                            | Microsoft.Compute/virtualMachineScaleSets |
| name                         | Name der Skalierungsgruppe                                       | myScaleSet                                |
| location                     | Standort für die Erstellung der Skalierungsgruppe                     | USA (Ost)                                   |
| sku.name                     | VM-Größe für die einzelnen Skalierungsgruppeninstanzen                  | Standard_A1                               |
| sku.capacity                 | Anzahl von VM-Instanzen für die anfängliche Erstellung           | 2                                         |
| upgradePolicy.mode           | Upgrademodus für VM-Instanz bei Auftreten von Änderungen              | Automatisch                                 |
| imageReference               | Plattform- oder benutzerdefiniertes Image für die VM-Instanzen | Microsoft Windows Server 2016 Datacenter  |
| osProfile.computerNamePrefix | Namenspräfix für die einzelnen VM-Instanzen                     | myvmss                                    |
| osProfile.adminUsername      | Benutzername für die einzelnen VM-Instanzen                        | azureuser                                 |
| osProfile.adminPassword      | Kennwort für die einzelnen VM-Instanzen                        | P@ssw0rd!                                 |

 Im folgenden Beispiel wird die grundlegende Ressourcendefinition der Skalierungsgruppe veranschaulicht. Zu Anpassen einer Skalierungsgruppenvorlage können Sie die VM-Größe oder anfängliche Kapazität ändern oder ein anderes Plattformimage bzw. ein benutzerdefiniertes Image verwenden.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

 Die Konfiguration der virtuellen Netzwerkschnittstellenkarte (NIC) ist nicht angegeben, um das Beispiel möglichst kurz zu halten. Auch zusätzliche Komponenten, z.B. ein Lastenausgleichsmodul, werden nicht angezeigt. Eine vollständige Skalierungsgruppenvorlage ist [am Ende dieses Artikels](#deploy-the-template) angegeben.


## <a name="install-an-application"></a>Installieren einer Anwendung
Beim Bereitstellen einer Skalierungsgruppe können über VM-Erweiterungen Aufgaben für die Konfiguration und Automatisierung nach der Bereitstellung, z.B. das Installieren einer App, bereitgestellt werden. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden. Fügen Sie dem obigen Ressourcenbeispiel den Abschnitt *extensionProfile* hinzu, um eine Erweiterung auf Ihre Skalierungsgruppe anzuwenden. Im Erweiterungsprofil werden normalerweise die folgenden Eigenschaften definiert:

- Erweiterungstyp
- Erweiterungsherausgeber
- Erweiterungsversion
- Speicherort der Konfiguration bzw. der Installationsskripts
- Auf den VM-Instanzen auszuführende Befehle

In der Beispielvorlage für eine [ASP.NET-Anwendung unter Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) wird die PowerShell DSC-Erweiterung verwendet, um eine ASP.NET MVC-App zu installieren, die in IIS ausgeführt wird. 

Ein Installationsskript wird wie unter *url* definiert von GitHub heruntergeladen. Anschließend führt die Erweiterung *InstallIIS* über das Skript *IISInstall.ps1* aus, wie in *function* und *Script* definiert. Die eigentliche ASP.NET-App wird als Web Deploy-Paket bereitgestellt, das auch von GitHub heruntergeladen wird, wie in *WebDeployPackagePath* definiert:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Powershell.DSC",
      "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.9",
        "autoUpgradeMinorVersion": true,
        "forceUpdateTag": "1.0",
        "settings": {
          "configuration": {
            "url": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/DSC/IISInstall.ps1.zip",
            "script": "IISInstall.ps1",
            "function": "InstallIIS"
          },
          "configurationArguments": {
            "nodeName": "localhost",
            "WebDeployPackagePath": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/WebDeploy/DefaultASPWebApp.v1.0.zip"
          }
        }
      }
    }
  ]
}
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage
Sie können die Vorlage für die [ASP.NET MVC-Anwendung unter Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mit der folgenden Schaltfläche **In Azure bereitstellen** bereitstellen. Diese Schaltfläche bewirkt Folgendes: Das Azure-Portal wird geöffnet, die vollständige Vorlage wird geladen, und es werden einige Parameter abgefragt, z.B. der Name der Skalierungsgruppe, die Instanzanzahl und die Administratoranmeldeinformationen.

[![In Azure bereitstellen](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-windows-webapp-dsc-autoscale%2Fazuredeploy.json)

Sie können auch Azure PowerShell verwenden, um die ASP.NET-Anwendung unter Windows wie folgt mit [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) zu installieren:

```azurepowershell-interactive
# Create a resource group
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS

# Deploy template into resource group
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName myResourceGroup `
    -TemplateFile https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/azuredeploy.json

# Update the scale set and apply the extension
Update-AzureRmVmss `
    -ResourceGroupName myResourceGroup `
    -VmScaleSetName myVMSS `
    -VirtualMachineScaleSet $vmssConfig
```

Folgen Sie den Aufforderungen zum Angeben des Namens einer Skalierungsgruppe und der Administratoranmeldeinformationen für die VM-Instanzen. Es kann 10 bis 15 Minuten dauern, bis die Skalierungsgruppe erstellt und die Erweiterung zum Konfigurieren der App angewendet wurde.


## <a name="test-your-sample-application"></a>Testen der Beispielanwendung
Um Ihre App in Aktion zu sehen, rufen Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) wie folgt die öffentliche IP-Adresse Ihres Lastenausgleichsmoduls ab:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Geben Sie die öffentliche IP-Adresse des Lastenausgleichsmoduls in einem Webbrowser im Format *http://publicIpAddress/MyApp* ein. Das Lastenausgleichsmodul verteilt den Datenverkehr auf eine Ihrer VM-Instanzen. Dies ist im folgenden Beispiel dargestellt:

![Ausführen der IIS-Website](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie können [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen wie folgt zu entfernen, sofern diese nicht mehr benötigt werden:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel zu den ersten Schritten haben Sie mit einer Azure-Vorlage eine Windows-Skalierungsgruppe erstellt und die PowerShell DSC-Erweiterung verwendet, um eine grundlegende ASP.NET-App auf den VM-Instanzen zu installieren. Erweitern Sie Ihre Skalierungsgruppe, um die Möglichkeiten in Bezug auf die Skalierbarkeit und Automatisierung zu erhöhen, indem Sie die folgenden Anleitungen verwenden:

- [Bereitstellen der App in VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md)
- Automatisches Skalieren per [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) oder [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-automatic-upgrade.md)

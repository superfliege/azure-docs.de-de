---
title: "Erstellen einer Azure-Skalierungsgruppe, die Verfügbarkeitszonen verwendet (Vorschauversion) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Skalierungsgruppen für virtuelle Azure Computer erstellt werden, die Verfügbarkeitszonen verwenden, um bessere Redundanz bei Ausfällen bereitzustellen."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Erstellen einer VM-Skalierungsgruppe, die Verfügbarkeitszonen verwendet (Vorschauversion)
Um Ihre VM-Skalierungsgruppen vor Fehlern auf Rechenzentrumsebene zu schützen, können Sie in einer Verfügbarkeitszone eine Skalierungsgruppe erstellen. Azure-Regionen, die Verfügbarkeitszonen unterstützen, weisen mindestens drei getrennte Zonen auf, mit je eigener, unabhängiger Stromquelle, Netzwerk und Kühlung. Weitere Informationen finden Sie in der [Overview of Availability Zones (Übersicht über Verfügbarkeitszonen)](../availability-zones/az-overview.md).

Damit Verfügbarkeitszonen verwendet werden können, muss Ihre Skalierungsgruppe in einer [unterstützten Azure-Region](../availability-zones/az-overview.md#regions-that-support-availability-zones) erstellt werden. Sie können mit einer der folgenden Methoden eine Skalierungsgruppe erstellen, die Verfügbarkeitszonen verwendet:

- [Azure-Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure-Ressourcen-Manager-Vorlagen](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-portal.md) ausführlich beschrieben wird. Wenn Sie eine unterstützte Azure-Region auswählen, können Sie eine Skalierungsgruppe in einer der Verfügbarkeitszonen erstellen, wie im folgenden Beispiel gezeigt:

![Erstellen einer Skalierungsgruppe in einer einzelnen Verfügbarkeitszone](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Die Skalierungsgruppe und die unterstützenden Ressourcen, wie etwa der Azure Load Balancer und die öffentliche IP-Adresse, werden in der von Ihnen angegebenen einzelnen Zone erstellt.


## <a name="use-the-azure-cli-20"></a>Verwenden von Azure CLI 2.0
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-cli.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen. Fügen Sie dem [az vmss create](/cli/azure/vmss#az_vmss_create)-Befehl den `--zones`-Parameter hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*). Im folgenden Beispiel wird eine Skalierungsgruppe namens *myScaleSet* in Zone *1* erstellt:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe in der von Ihnen angegebenen Zone dauert einige Minuten.


## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-powershell.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen. Fügen Sie dem [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig)-Befehl den `-Zone`-Parameter hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*). Im folgenden Beispiel wird eine Konfiguration für eine Skalierungsgruppe mit dem Namen *vmssConfig* in Zone *1* von *USA, Osten 2* erstellt:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Zum Erstellen der eigentlichen Skalierungsgruppe führen Sie die zusätzlichen Schritte aus, wie im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-powershell.md) ausführlich beschrieben.


## <a name="use-azure-resource-manager-templates"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im Artikel „Erste Schritte“ für [Linux](virtual-machine-scale-sets-create-template-linux.md) oder [Windows](virtual-machine-scale-sets-create-template-windows.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen. Fügen Sie in Ihrer Vorlage dem Ressourcentyp *Microsoft.Compute/virtualMachineScaleSets* die Eigenschaft `zones` hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*). Im folgenden Beispiel wird eine Linux-Skalierungsgruppe namens *myScaleSet* in Zone *1* von *USA, Osten 2* erstellt:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
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
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
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

Zum Erstellen der eigentlichen Skalierungsgruppe führen Sie die zusätzlichen Schritte aus, wie im Artikel „Erste Schritte“ für [Linux](virtual-machine-scale-sets-create-template-linux.md) oder [Windows](virtual-machine-scale-sets-create-template-windows.md) ausführlich beschrieben.


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie jetzt eine Skalierungsgruppe in einer Verfügbarkeitszone erstellt haben, können Sie lernen, wie das [Bereitstellen von Anwendungen auf VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md) oder das [Verwenden von automatischer Skalierung bei VM-Skalierungsgruppen](virtual-machine-scale-sets-autoscale-overview.md) funktioniert.

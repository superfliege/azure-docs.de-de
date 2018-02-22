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
ms.date: 01/11/2018
ms.author: iainfou
ms.openlocfilehash: 2de214f604469025a8a4accde44359fea0ded7e9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Erstellen einer VM-Skalierungsgruppe, die Verfügbarkeitszonen verwendet (Vorschauversion)
Um Ihre VM-Skalierungsgruppen vor Fehlern auf Rechenzentrumsebene zu schützen, können Sie eine Skalierungsgruppe über mehrere Verfügbarkeitszonen erstellen. Azure-Regionen, die Verfügbarkeitszonen unterstützen, weisen mindestens drei getrennte Zonen auf, mit je eigener, unabhängiger Stromquelle, Netzwerk und Kühlung. Weitere Informationen finden Sie in der [Overview of Availability Zones (Übersicht über Verfügbarkeitszonen)](../availability-zones/az-overview.md).

[!INCLUDE [availability-zones-preview-statement.md](../../includes/availability-zones-preview-statement.md)]


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Skalierungsgruppen mit einer Zone und zonenredundante Skalierungsgruppen
Wenn Sie eine VM-Skalierungsgruppe bereitstellen, können eine einzelne Verfügbarkeitszone in einer Region oder mehrere Zonen verwenden.

Wenn Sie eine Skalierungsgruppe in einer einzelnen Zone erstellen, können Sie steuern, in welcher Zone alle diese VM-Instanzen ausgeführt werden. Die Skalierungsgruppe wird dann nur innerhalb dieser Zone verwaltet und automatisch skaliert. Durch eine zonenredundante Skalierungsgruppe können Sie eine einzelne Skalierungsgruppe erstellen, die sich über mehrere Zonen erstreckt. Während der Erstellung werden VM-Instanzen standardmäßig gleichmäßig auf Zonen verteilt. Wenn in einer der Zonen eine Unterbrechung auftritt, wird eine Skalierungsgruppe nicht automatisch horizontal hochskaliert, um die Kapazität zu erhöhen. Eine bewährte Methode besteht darin, Regeln für die automatische Skalierung anhand der CPU- oder Arbeitsspeicherauslastung zu konfigurieren. Über diese Regeln für automatische Skalierung könnte die Skalierungsgruppe auf den Verlust der VM-Instanzen in dieser einen Zone reagieren, indem neue Instanzen in den verbleibenden betriebsbereiten Zonen horizontal hochskaliert werden.

Damit Verfügbarkeitszonen verwendet werden können, muss Ihre Skalierungsgruppe in einer [unterstützten Azure-Region](../availability-zones/az-overview.md#regions-that-support-availability-zones) erstellt werden. Sie müssen sich außerdem [für die Vorschau von Verfügbarkeitszonen registrieren](http://aka.ms/azenroll). Sie können mit einer der folgenden Methoden eine Skalierungsgruppe erstellen, die Verfügbarkeitszonen verwendet:

- [Azure portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure-Ressourcen-Manager-Vorlagen](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-portal.md) ausführlich beschrieben wird. Achten Sie darauf, dass Sie sich [für die Vorschau von Verfügbarkeitszonen registriert haben](http://aka.ms/azenroll). Wenn Sie eine unterstützte Azure-Region auswählen, können Sie eine Skalierungsgruppe in einer der Verfügbarkeitszonen erstellen, wie im folgenden Beispiel gezeigt:

![Erstellen einer Skalierungsgruppe in einer einzelnen Verfügbarkeitszone](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Die Skalierungsgruppe und die unterstützenden Ressourcen, wie etwa der Azure Load Balancer und die öffentliche IP-Adresse, werden in der von Ihnen angegebenen einzelnen Zone erstellt.


## <a name="use-the-azure-cli-20"></a>Verwenden von Azure CLI 2.0
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-cli.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen und [für die Vorschau von Verfügbarkeitszonen registriert sein](http://aka.ms/azenroll).

Fügen Sie dem [az vmss create](/cli/azure/vmss#az_vmss_create)-Befehl den `--zones`-Parameter hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*). Im folgenden Beispiel wird eine Skalierungsgruppe mit einer Zone namens *myScaleSet* in Zone *1* erstellt:

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
Ein vollständiges Beispiel einer Skalierungsgruppe mit einer einzelnen Zone und Netzwerkressourcen finden Sie in [diesem Beispielskript für die Befehlszeilenschnittstelle](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh.).

### <a name="zone-redundant-scale-set"></a>Zonenredundante Skalierungsgruppe
Um eine zonenredundante Skalierungsgruppe zu erstellen, verwenden Sie die öffentliche IP-Adresse einer *Standard*-SKU und einen Lastenausgleich. Für eine verbesserte Redundanz erstellt die *Standard*-SKU zonenredundante Netzwerkressourcen. Weitere Informationen finden Sie unter [Übersicht über Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md). Wenn Sie erstmalig eine zonenredundante Skalierungsgruppe oder einen Lastenausgleich erstellen, müssen Sie die folgenden Schritte ausführen, um Ihr Konto für diese Vorschaufeatures zu registrieren.

1. Registrieren Sie Ihr Konto wie folgt mit [az feature register](/cli/azure/feature#az_feature_register) für die Features für zonenredundante Skalierungsgruppen und Netzwerkfunktionen:

    ```azurecli
    az feature register --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. Die Registrierung für diese Features dauert möglicherweise einige Minuten. Sie können den Status des Vorgangs mithilfe des Befehls [az feature show](/cli/azure/feature#az_feature_show) überprüfen:

    ```azurecli
    az feature show --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```

    Das folgende Beispiel zeigt den gewünschten Status des Features als *Registriert*:
    
    ```json
    "properties": {
          "state": "Registered"
       },
    ```

3. Wenn die zonenredundante Skalierungsgruppe und die Netzwerkressourcen den Status *Registriert* melden, registrieren Sie die Anbieter für *Compute* und *Netzwerk* wie folgt mit [az provider register](/cli/azure/provider#az_provider_register) erneut:

    ```azurecli
    az provider register --namespace Microsoft.Compute
    az provider register --namespace Microsoft.Network
    ```

Geben Sie zum Erstellen einer zonenredundanten Skalierungsgruppe mehrere Zonen mit dem Parameter `--zones` an. Im folgenden Beispiel wird eine zonenredundante Skalierungsgruppe namens *myScaleSet* in den Zonen *1, 2 und 3* erstellt:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones {1,2,3}
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe in den von Ihnen angegebenen Zonen dauert einige Minuten. Ein vollständiges Beispiel einer zonenredundanten Skalierungsgruppe mit Netzwerkressourcen finden Sie in [diesem Beispielskript für die Befehlszeilenschnittstelle](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh).


## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im [Artikel „Erste Schritte“](virtual-machine-scale-sets-create-powershell.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen und [für die Vorschau von Verfügbarkeitszonen registriert sein](http://aka.ms/azenroll). Fügen Sie dem [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig)-Befehl den `-Zone`-Parameter hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*). 

Im folgenden Beispiel wird eine Konfiguration für eine Skalierungsgruppe mit einer einzelnen Zone namens *vmssConfig* in Zone *1* von *USA, Osten 2* erstellt:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Ein vollständiges Beispiel einer Skalierungsgruppe mit einer einzelnen Zone und Netzwerkressourcen finden Sie in [diesem Beispielskript für PowerShell](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1).

### <a name="zone-redundant-scale-set"></a>Zonenredundante Skalierungsgruppe
Um eine zonenredundante Skalierungsgruppe zu erstellen, verwenden Sie die öffentliche IP-Adresse einer *Standard*-SKU und einen Lastenausgleich. Für eine verbesserte Redundanz erstellt die *Standard*-SKU zonenredundante Netzwerkressourcen. Weitere Informationen finden Sie unter [Übersicht über Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md). Wenn Sie erstmalig eine zonenredundante Skalierungsgruppe oder einen Lastenausgleich erstellen, müssen Sie die folgenden Schritte ausführen, um Ihr Konto für diese Vorschaufeatures zu registrieren.

1. Registrieren Sie Ihr Konto wie folgt mit [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) für die Features für zonenredundante Skalierungsgruppen und Netzwerkfunktionen:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. Die Registrierung für diese Features dauert möglicherweise einige Minuten. Sie können den Status des Vorgangs mithilfe des Befehls [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) überprüfen:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute 
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Das folgende Beispiel zeigt den gewünschten Status des Features als *Registriert*:
    
    ```powershell
    RegistrationState
    -----------------
    Registered
    ```

3. Wenn die zonenredundante Skalierungsgruppe und die Netzwerkressourcen den Status *Registriert* melden, registrieren Sie die Anbieter für *Compute* und *Netzwerk* wie folgt mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) erneut:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```

Geben Sie zum Erstellen einer zonenredundanten Skalierungsgruppe mehrere Zonen mit dem Parameter `-Zone` an. Im folgenden Beispiel wird eine Konfiguration für eine zonenredundante Skalierungsgruppe namens *myScaleSet* in den Zonen *1, 2 und 3* von *USA, Osten 2* erstellt:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1", "2", "3"
```

Geben Sie beim Erstellen einer öffentlichen IP-Adresse mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) oder eines Lastenausgleichsmoduls mit [New-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/New-AzureRmLoadBalancer) den Zusatz *-SKU "Standard"* an, um die zonenredundanten Netzwerkressourcen zu erstellen. Sie müssen außerdem eine Netzwerksicherheitsgruppe und Regeln zum Zulassen von Datenverkehr erstellen. Weitere Informationen finden Sie unter [Übersicht über Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).

Ein vollständiges Beispiel einer zonenredundanten Skalierungsgruppe mit Netzwerkressourcen finden Sie in [diesem Beispielskript für PowerShell](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1).


## <a name="use-azure-resource-manager-templates"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen
Der Erstellungsprozess einer Skalierungsgruppe, die eine Verfügbarkeitszone verwendet, ist der gleiche, der im Artikel „Erste Schritte“ für [Linux](virtual-machine-scale-sets-create-template-linux.md) oder [Windows](virtual-machine-scale-sets-create-template-windows.md) ausführlich beschrieben wird. Damit Verfügbarkeitszonen verwendet werden können, müssen Sie Ihre Skalierungsgruppe in einer unterstützten Azure-Region erstellen und [für die Vorschau von Verfügbarkeitszonen registriert sein](http://aka.ms/azenroll). Fügen Sie in Ihrer Vorlage dem Ressourcentyp *Microsoft.Compute/virtualMachineScaleSets* die Eigenschaft `zones` hinzu, und geben Sie die zu verwendende Zone an (z. B. Zone *1*, *2* oder *3*).

Im folgenden Beispiel wird eine Linux-Skalierungsgruppe mit einer einzelnen Zone namens *myScaleSet* in Zone *1* von *USA, Osten 2* erstellt:

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

Ein vollständiges Beispiel einer Skalierungsgruppe mit einer einzelnen Zone und Netzwerkressourcen finden Sie in [dieser Resource Manager-Beispielvorlage](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json).

### <a name="zone-redundant-scale-set"></a>Zonenredundante Skalierungsgruppe
Um eine zonenredundante Skalierungsgruppe zu erstellen, geben Sie in der `zones`-Eigenschaft für den Ressourcentyp *Microsoft.Compute/virtualMachineScaleSets* mehrere Werte an. Im folgenden Beispiel wird eine zonenredundante Skalierungsgruppe namens *myScaleSet* in den Zonen *1, 2 und 3* von *USA, Osten 2* erstellt:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": [
        "1",
        "2",
        "3"
      ]
}
```

Wenn Sie eine öffentliche IP-Adresse oder einen Lastenausgleich erstellen, geben Sie die Eigenschaft *"sku": { "name": "Standard" }"* an, um zonenredundante Netzwerkressourcen zu erstellenden. Sie müssen außerdem eine Netzwerksicherheitsgruppe und Regeln zum Zulassen von Datenverkehr erstellen. Weitere Informationen finden Sie unter [Übersicht über Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).

Ein vollständiges Beispiel einer zonenredundanten Skalierungsgruppe mit Netzwerkressourcen finden Sie in [dieser Resource Manager-Beispielvorlage](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json).


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie jetzt eine Skalierungsgruppe in einer Verfügbarkeitszone erstellt haben, können Sie lernen, wie das [Bereitstellen von Anwendungen auf VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md) oder das [Verwenden von automatischer Skalierung bei VM-Skalierungsgruppen](virtual-machine-scale-sets-autoscale-overview.md) funktioniert.

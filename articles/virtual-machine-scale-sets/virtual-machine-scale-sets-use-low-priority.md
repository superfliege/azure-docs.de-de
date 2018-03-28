---
title: Erstellen einer Azure-Skalierungsgruppe, die VMs mit niedriger Priorität verwendet (Vorschauversion) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-VM-Skalierungsgruppen erstellen, die VMs mit niedriger Priorität verwenden, um Kosten zu sparen.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mmccrory
manager: rajraj
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: memccror
ms.openlocfilehash: 9e4970ecc538caab537281931b89bfd57d994cfa
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>VMs mit niedriger Priorität in Skalierungsgruppen (Vorschau)

Mithilfe von VMs mit niedriger Priorität in Skalierungsgruppen können Sie unsere ungenutzte Kapazität mit signifikanten Kosteneinsparungen nutzen. Wenn Azure die Kapazität wieder benötigt, erzwingt die Azure-Infrastruktur die Zurücksetzung der VMs mit niedriger Priorität. Aus diesem Grund eignen sich VMs mit niedriger Priorität hervorragend für Workloads, die Unterbrechungen verkraften können, z.B. Batchverarbeitungsaufträge, Entwicklungs- und Testumgebungen, große Berechnungsarbeitslasten etc.

Die Menge der verfügbaren ungenutzten Kapazität kann abhängig von Größe, Region, Tageszeit etc. variieren. Beim Bereitstellen von VMs mit niedriger Priorität in Skalierungsgruppen weist Azure die virtuellen Computer zu, wenn Kapazität verfügbar ist, aber es gibt keine SLA für diese virtuellen Computer. Eine Skalierungsgruppe mit niedriger Priorität wird in einer einzelnen Fehlerdomäne bereitgestellt und bietet keine Garantien für hohe Verfügbarkeit.

> [!NOTE]
> Skalierungsgruppen mit niedriger Priorität befinden sich in der Vorschau und sind für Ihre Entwicklungs- und Testszenarios bereit. 

## <a name="eviction-policy"></a>Entfernungsrichtlinie

Wenn die VMs aus Ihrer Skalierungsgruppe mit niedriger Priorität entfernt werden, werden sie standardmäßig in den Zustand „Beendet“ (Zuordnung aufgehoben) verschoben. Mit dieser Entfernungsrichtlinie können Sie entfernte Instanzen erneut bereitstellen, aber es gibt keine Garantie, dass die Zuordnung erfolgreich ist. Die beendeten virtuellen Computer werden auf Ihr Kontingent an Skalierungsgruppeninstanzen angerechnet, und die zugrunde liegenden Datenträger werden Ihnen in Rechnung gestellt. 

Wenn Sie möchten, dass die virtuellen Computer in Ihrer Skalierungsgruppe mit niedriger Priorität gelöscht werden, wenn sie entfernt werden, können Sie die Entfernungsrichtlinie zum Löschen in Ihrer [Azure Resource Manager-Vorlage](#use-azure-resource-manager-templates) festlegen. Wenn die Entfernungsrichtlinie zum Löschen festgelegt ist, können Sie neue VMs durch Heraufsetzen der Skalierungsgruppeninstanzenanzahl-Eigenschaft erstellen. Die entfernten VMs werden zusammen mit ihren zugrunde liegenden Datenträgern gelöscht, und darum fallen keine Kosten für ihre Speicherung an. Sie können auch die Funktion zur automatischen Skalierung von Skalierungsgruppen verwenden, um zu versuchen, entfernte VMs automatisch zu kompensieren, es gibt jedoch keine Garantie, dass die Zuordnung erfolgreich ist. Sie sollten die Funktion für automatische Skalierung nur dann auf Skalierungsgruppen mit niedriger Priorität anwenden, wenn Sie die Entfernungsrichtlinie zum Löschen festlegen, um die Kosten Ihrer Datenträger und das Erreichen von Kontingentgrenzen zu vermeiden. 

> [!NOTE]
> Während der Vorschau können Sie Ihre Entfernungsrichtlinie mithilfe von [Azure Resource Manager-Vorlagen](#use-azure-resource-manager-templates) festlegen. 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Bereitstellen von VMs mit niedriger Priorität in Skalierungsgruppen

Zum Bereitstellen von VMs mit niedriger Priorität in Skalierungsgruppen können Sie das neue *Priorität*-Flag auf *Niedrig* setzen. Für alle virtuellen Computer in Ihrer Skalierungsgruppe wird niedrige Priorität festgelegt. Verwenden Sie zum Erstellen einer Skalierungsgruppe mit niedriger Priorität eine der folgenden Methoden:
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure-Ressourcen-Manager-Vorlagen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-cli-20"></a>Verwenden von Azure CLI 2.0

Der Prozess zum Erstellen einer Skalierungsgruppe mit VMs mit niedriger Priorität wird im [Artikel zum Einstieg](virtual-machine-scale-sets-create-cli.md) ausführlich beschrieben. Fügen Sie einfach den „--Priority“-Parameter dem CLI-Aufruf hinzu, und setzen Sie ihn auf *Low*, wie im folgenden Beispiel gezeigt:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Low
```

## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

Der Prozess zum Erstellen einer Skalierungsgruppe mit VMs mit niedriger Priorität wird im [Artikel zum Einstieg](virtual-machine-scale-sets-create-powershell.md) ausführlich beschrieben.
Fügen Sie einfach den „--Priority“-Parameter [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) hinzu, und setzen Sie ihn auf *Low*, wie im folgenden Beispiel gezeigt:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Verwenden von Azure Resource Manager-Vorlagen

Der Prozess zum Erstellen einer Skalierungsgruppe mit VMs mit niedriger Priorität wird im Artikel zum Einstieg für [Linux](virtual-machine-scale-sets-create-template-linux.md) oder [Windows](virtual-machine-scale-sets-create-template-windows.md) ausführlich beschrieben. Fügen Sie die Eigenschaft „Priority“dem *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile*-Ressourcentyp in Ihrer Vorlage hinzu, und geben Sie *Low* als Wert an. Achten Sie darauf, dass Sie die API-Version *2017-10-30-preview* oder höher verwenden. 

Um die Entfernungsrichtlinie auf Löschen festzulegen, fügen Sie den Parameter „evictionPolicy“ hinzu, und setzen Sie ihn auf *delete*.

Das folgende Beispiel erstellt eine Linux-Skalierungsgruppe mit niedriger Priorität mit der Bezeichnung *myScaleSet* in *USA, Westen-Mitte*, die die virtuellen Computer in der Skalierungsgruppe beim Entfernen *löscht*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Low",
       "evictionPolicy": "delete",
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
## <a name="next-steps"></a>Nächste Schritte
Da Sie nun eine Skalierungsgruppe mit virtuellen Computern mit niedriger Priorität erstellt haben, versuchen Sie, unsere [Vorlage für automatische Skalierung mit niedriger Priorität](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri) bereitzustellen.

Detaillierte Preisinformationen finden Sie auf der Seite [Linux VM-Skalierungsgruppen – Preise ](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/).
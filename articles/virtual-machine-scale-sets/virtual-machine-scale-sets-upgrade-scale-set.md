---
title: Ändern einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation
description: Ändern einer Azure-VM-Skalierungsgruppe
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: fcca912a8120a51d2f0a454ef0a6341cd5882015
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Ändern einer VM-Skalierungsgruppe
In diesem Artikel wird beschrieben, wie Sie eine vorhandene VM-Skalierungsgruppe ändern. Sie erfahren unter anderem, wie Sie die Konfiguration der Skalierungsgruppe oder die Konfiguration der in der Skalierungsgruppe ausgeführten Anwendungen ändern, wie Sie die Verfügbarkeit verwalten und vieles mehr.

## <a name="fundamental-concepts"></a>Grundlegende Konzepte

### <a name="scale-set-model"></a>Skalierungsgruppenmodell

Eine Skalierungsgruppe enthält ein Modell, das den *gewünschten* Zustand der gesamten Skalierungsgruppe erfasst. Zum Abfragen des Modells für eine Skalierungsgruppe können Sie Folgendes verwenden:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
   
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).

* PowerShell:

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}`
   
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure-Befehlszeilenschnittstelle: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Sie können auch den [Azure-Ressourcen-Explorer (Vorschauversion)](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen des Modells für eine Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der Azure CLI:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Wie Sie sehen, gelten diese Eigenschaften für die gesamte Skalierungsgruppe.



### <a name="scale-set-instance-view"></a>Instanzenansicht für die Skalierungsgruppe

Eine Skalierungsgruppe verfügt außerdem über eine Instanzenansicht, die den aktuellen *Runtimestatus* der gesamten Skalierungsgruppe erfasst. Zum Abfragen der Instanzenansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` 
   
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` 
  
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure-Befehlszeilenschnittstelle: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Sie können auch den [Azure-Ressourcen-Explorer (Vorschauversion)](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen der Instanzansicht für eine Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der Azure CLI:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
  .
  .
  .
}
```

Wie Sie sehen können, bieten diese Eigenschaften eine Zusammenfassung des aktuellen Runtimestatus der VMs in der Skalierungsgruppe. Die Zusammenfassung enthält den Status der Erweiterungen, die auf die Skalierungsgruppe angewendet werden (aus Gründen der Übersichtlichkeit weggelassen).



### <a name="scale-set-vm-model-view"></a>VM-Modellansicht der Skalierungsgruppe

Nicht nur die Skalierungsgruppe verfügt über eine Modellansicht, sondern auch jeder darin enthaltene virtuelle Computer. Zum Abfragen der Modellansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` 
  
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
  
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure-Befehlszeilenschnittstelle: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Sie können auch den [Azure-Ressourcen-Explorer (Vorschauversion)](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen des Modells für eine VM in einer Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der Azure CLI:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Wie Sie sehen, beschreiben diese Eigenschaften die Konfiguration des virtuellen Computers selbst, nicht die Konfiguration der gesamten Skalierungsgruppe. Beispielsweise besitzt das Skalierungsgruppenmodell die Eigenschaft `overprovision`, das Modell für einen virtuellen Computer in einer Skalierungsgruppe besitzt diese Eigenschaft jedoch nicht. Der Grund für diesen Unterschied liegt darin, dass die Überbereitstellung eine Eigenschaft für die gesamte Skalierungsgruppe und nicht für einzelne VMs in der Skalierungsgruppe ist. (Weitere Informationen zu Überbereitstellung finden Sie unter [Überlegungen zum Entwurf von Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="scale-set-vm-instance-view"></a>VM-Instanzenansicht für die Skalierungsgruppe

Nicht nur die Skalierungsgruppe verfügt über eine Instanzenansicht, sondern auch jeder darin enthaltene virtuelle Computer. Zum Abfragen der Instanzenansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` 
 
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` 
  
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure-Befehlszeilenschnittstelle: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Sie können auch den [Azure-Ressourcen-Explorer (Vorschauversion)](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen der Instanzansicht für eine VM in einer Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der Azure CLI:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
  .
  .
  .
}
```

Wie Sie sehen, beschreiben diese Eigenschaften den aktuellen Runtimestatus der VM selbst. Der Status enthält alle Erweiterungen, die auf die Skalierungsgruppe angewendet werden (aus Gründen der Übersichtlichkeit weggelassen).




## <a name="techniques-for-updating-global-scale-set-properties"></a>Verfahren zum Aktualisieren globaler Eigenschaften der Skalierungsgruppe

Wenn Sie eine globale Skalierungsgruppeneigenschaft aktualisieren möchten, müssen Sie die Eigenschaft im Skalierungsgruppenmodell aktualisieren. Für die Aktualisierung können Sie Folgendes verwenden:

* REST-API: 

  `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
  
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

  Alternativ können Sie unter Verwendung der Eigenschaften aus der REST-API eine Azure Resource Manager-Vorlage bereitstellen, um globale Skalierungsgruppeneigenschaften zu aktualisieren.

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` 
  
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).

* Azure-Befehlszeilenschnittstelle:

  * Ändern einer Eigenschaft: `az vmss update --set {propertyPath}={value}` 
  
  * Hinzufügen eines Objekts zu einer Listeneigenschaft in einer Skalierungsgruppe: `az vmss update --add {propertyPath} {JSONObjectToAdd}` 
  
  * Entfernen eines Objekts aus einer Listeneigenschaft in einer Skalierungsgruppe: `az vmss update --remove {propertyPath} {indexToRemove}` 
  
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update). 
  
  Falls Sie die Skalierungsgruppe zuvor mithilfe des Befehls `az vmss create` bereitgestellt haben, können Sie alternativ erneut den Befehl `az vmss create` ausführen, um die Skalierungsgruppe zu aktualisieren. Stellen Sie dabei sicher, dass – mit Ausnahme der zu ändernden Eigenschaften – alle Eigenschaften im Befehl `az vmss create` den vorherigen Eigenschaften entsprechen.



Sie können auch den [Azure-Ressourcen-Explorer (Vorschauversion)](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Aktualisieren des Modells für eine Skalierungsgruppe verwenden.

Nach der Aktualisierung des Skalierungsgruppenmodells gilt die neue Konfiguration für alle in der Skalierungsgruppe erstellten neuen virtuellen Computer. Die Modelle für die vorhandenen virtuellen Computer in der Skalierungsgruppe müssen aber dennoch auf das aktuelle Skalierungsgruppenmodell aktualisiert werden. Die Modelle der einzelnen virtuellen Computer enthalten eine boolesche Eigenschaft namens `latestModelApplied`, die angibt, ob ein virtueller Computer auf das aktuelle Skalierungsgruppenmodell aktualisiert wurde. (`true` bedeutet, dass der virtuelle Computer auf das aktuelle Modell aktualisiert wurde.)




## <a name="techniques-for-bringing-vms-up-to-date-with-the-latest-scale-set-model"></a>Verfahren zum Aktualisieren von VMs auf das aktuelle Skalierungsgruppenmodell

Für Skalierungsgruppen gilt eine *Upgraderichtlinie*, die festlegt, wie virtuelle Computer auf das aktuelle Skalierungsgruppenmodell aktualisiert werden. Die drei Modi für die Upgraderichtlinie lauten wie folgt:

- **Automatisch**: In diesem Modus gibt die Skalierungsgruppe keine Garantie in Bezug auf die Reihenfolge, in der virtuelle Computer heruntergefahren werden. Die Skalierungsgruppe fährt unter Umständen alle virtuellen Computer gleichzeitig herunter. 
- **Parallel**: In diesem Modus führt die Skalierungsgruppe ein Rollout des Updates in Batches mit optionaler Pause zwischen den Batches aus.
- **Manuell**: In diesem Modus geschieht beim Aktualisieren des Skalierungsgruppenmodells nichts mit den vorhandenen virtuellen Computern. Zum Aktualisieren vorhandener virtueller Computer müssen Sie für jeden einzelnen ein manuelles Upgrade ausführen. Sie können das manuelle Upgrade über folgende Komponenten ausführen:

  - REST-API: 
  
    `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` 
    
    Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).

  - PowerShell: 
  
    `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
    
    Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

  - Azure-Befehlszeilenschnittstelle: 
  
    `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` 
    
    Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).

  Sie können auch die [Azure SDKs](https://azure.microsoft.com/downloads/) verwenden, um ein manuelles Upgrade für einen virtuellen Computer in einer Skalierungsgruppe auszuführen.

>[!NOTE]
> Für Azure Service Fabric-Cluster kann nur der Modus „Automatisch“ verwendet werden, das Update wird jedoch anders gehandhabt. Weitere Informationen zu Service Fabric-Updates finden Sie in der [Service Fabric-Dokumentation](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

Eine der Änderungen, die an globalen Skalierungsgruppeneigenschaften vorgenommen werden, entspricht nicht der Upgraderichtlinie: Änderungen an dem Betriebssystemprofil der Skalierungsgruppe. (Beispiele sind Benutzername und Kennwort des Administrators.) Diese Eigenschaften können nur in API-Version 2017-12-01 oder höher geändert werden. Diese Änderungen gelten nur für virtuelle Computer, die nach der Änderung im Skalierungsgruppenmodell erstellt wurden. Um vorhandene virtuelle Computer zu aktualisieren, müssen Sie ein Reimaging aller vorhandenen virtuellen Computer durchführen. Für das Reimaging einer VM können Sie Folgendes verwenden:

* REST-API: 

  `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` 
  
  Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).

* PowerShell: 

  `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` 
  
  Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).

* Azure-Befehlszeilenschnittstelle: 

  `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` 
  
  Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).

Sie können auch die [Azure SDKs](https://azure.microsoft.com/downloads/) verwenden, um ein Reimaging für einen virtuellen Computer in einer Skalierungsgruppe durchzuführen.




## <a name="properties-with-restrictions-on-modification"></a>Eigenschaften mit Einschränkungen bei der Änderung

### <a name="create-time-properties"></a>Eigenschaften zur Erstellungszeit

Einige Eigenschaften können nur beim anfänglichen Erstellen der Skalierungsgruppe festgelegt werden. Zu diesen Eigenschaften zählen folgende:

- Zones
- Imagereferenzherausgeber
- Imagereferenzangebot

### <a name="properties-that-can-be-changed-based-on-the-current-value-only"></a>Eigenschaften, die nur auf Grundlage des aktuellen Werts geändert werden können

Einige Eigenschaften können mit Ausnahmen je nach aktuellem Wert geändert werden. Zu diesen Eigenschaften zählen folgende:

- `singlePlacementGroup`: Wenn `singlePlacementGroup` „true“ ist, kann diese Eigenschaft auf „false“ geändert werden. Wenn `singlePlacementGroup` jedoch „false“ lautet, kann sie *nicht* in „true“ geändert werden.
- `subnet`: Das Subnetz einer Skalierungsgruppe kann geändert werden, solange sich das ursprüngliche Subnetz und das neue Subnetz im gleichen virtuellen Netzwerk befinden.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschaften, für die zur Änderung die Aufhebung der Zuordnung erforderlich ist

Einige Eigenschaften können nur in bestimmte Werte geändert werden, wenn die Zuordnung der virtuellen Computer in der Skalierungsgruppe aufgehoben wird. Zu diesen Eigenschaften zählen folgende:

- `sku name`: Wird die neue VM-SKU nicht auf der Hardware unterstützt, auf der sich die Skalierungsgruppe derzeit befindet, müssen Sie die Zuordnung der virtuellen Computer in der Skalierungsgruppe aufheben, bevor Sie `sku name` ändern können. Weitere Informationen zum Ändern der Größe von virtuellen Computern finden Sie in [diesem Azure-Blogbeitrag](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM-spezifische Updates

Bestimmte Änderungen können statt auf die globalen Skalierungsgruppeneigenschaften auf spezifische virtuelle Computer angewendet werden. Derzeit wird als VM-spezifische Aktualisierung nur das Anfügen von Datenträgern an virtuelle Computer in der Skalierungsgruppe bzw. das Trennen dieser Datenträger unterstützt. Dieses Feature befindet sich in der Vorschauphase. Weitere Informationen finden Sie in der [Vorschaudokumentation](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios"></a>Szenarien

### <a name="application-updates"></a>Anwendungsupdates

Wenn eine Anwendung über Erweiterungen in einer Skalierungsgruppe bereitgestellt wird, wird durch eine Aktualisierung der Erweiterungskonfiguration eine Aktualisierung der Anwendung gemäß der Upgraderichtlinie ausgelöst. Beispiel: Wenn Sie eine neue Version eines Skripts für die Ausführung in einer benutzerdefinierten Skripterweiterung besitzen, können Sie die `fileUris`-Eigenschaft aktualisieren, um auf das neue Skript zu verweisen. 

In einigen Fällen möchten Sie ein Update erzwingen, obwohl die Konfiguration der Erweiterung unverändert ist. (Beispielsweise haben Sie das Skript aktualisiert, ohne die URI des Skripts zu ändern.) In diesen Fällen können Sie `forceUpdateTag` ändern, um eine Aktualisierung zu erzwingen. Die Azure-Plattform interpretiert diese Eigenschaft nicht. Die Änderung ihres Werts hat daher keine Auswirkung darauf, wie die Erweiterung ausgeführt wird. Die Änderung erzwingt lediglich eine erneute Ausführung der Erweiterung. 

Weitere Informationen zu `forceUpdateTag` finden Sie in der [REST-API-Dokumentation für Erweiterungen](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Anwendungen werden häufig auch über ein benutzerdefiniertes Image bereitgestellt. Dieses Szenario wird im folgenden Abschnitt behandelt.

### <a name="os-updates"></a>Betriebssystemupdates

Wenn Sie Plattformimages verwenden, können Sie die Images aktualisieren, indem Sie `imageReference` ändern. Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

>[!NOTE]
> Bei Plattformimages wird als Imagereferenzversion häufig „aktuelle Version“ angegeben. Das bedeutet, dass beim Erstellen, horizontalen Hochskalieren und Durchführen eines Reimagings die virtuellen Computer mit der neuesten verfügbaren Version erstellt werden. Es bedeutet jedoch *nicht*, dass das Betriebssystemimage im Laufe der Zeit automatisch aktualisiert wird, wenn neue Imageversionen veröffentlicht werden. Dies ist eine separate Funktion, die zurzeit als Vorschau verfügbar ist. Weitere Informationen finden Sie unter [Automatische Betriebssystemupgrades](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Wenn Sie benutzerdefinierte Images verwenden, können Sie die Images aktualisieren, indem Sie die `imageReference`-ID aktualisieren. Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="examples"></a>Beispiele

### <a name="update-the-os-image-for-your-scale-set"></a>Aktualisieren des Betriebssystemimages für Ihre Skalierungsgruppe

Nehmen wir an, Sie haben eine Skalierungsgruppe mit einer alten Version von Ubuntu LTS 16.04. Sie möchten auf eine neuere Version von Ubuntu LTS 16.04 aktualisieren (z.B. Version 16.04.201801090). Die Eigenschaft für die Imagereferenzversion ist nicht Teil einer Liste, daher können Sie diese Eigenschaften direkt mithilfe der folgenden Befehle ändern:

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

* Azure-Befehlszeilenschnittstelle: 

  `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="update-the-load-balancer-for-your-scale-set"></a>Aktualisieren des Lastenausgleichs für Ihre Skalierungsgruppe

Angenommen, Sie besitzen eine Skalierungsgruppe mit einem Azure-Lastenausgleich und möchten den Lastenausgleich durch ein Azure Anwendungsgateway ersetzen. Die Eigenschaften des Lastenausgleichs und Anwendungsgateways für eine Skalierungsgruppe sind Teil einer Liste. Sie können also die Befehle zum Entfernen und Hinzufügen von Listenelementen verwenden, anstatt die Eigenschaften direkt zu ändern.

PowerShell:
```
# Get the current model of the scale set and store it in a local PowerShell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure load balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local PowerShell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

Azure-Befehlszeilenschnittstelle:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # Remove the load balancer back-end pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # Remove the load balancer back-end pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # Add the application gateway back-end pool to the scale set model
```

>[!NOTE]
> Bei diesen Befehlen wird davon ausgegangen, dass für die Skalierungsgruppe nur eine IP-Konfiguration und ein Lastenausgleichsmodul festgelegt sind. Sind mehrere vorhanden, müssen Sie möglicherweise einen Listenindex ungleich 0 verwenden.
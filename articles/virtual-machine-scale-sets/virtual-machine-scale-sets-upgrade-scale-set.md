---
title: "Ändern einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation"
description: "Ändern einer Azure-VM-Skalierungsgruppe"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: cdd1015f63e80b7ec51565c18f3440ce1828fb03
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Ändern einer VM-Skalierungsgruppe
In diesem Artikel wird beschrieben, wie Sie eine vorhandene Skalierungsgruppe ändern. Sie erfahren unter anderem, wie Sie die Konfiguration der Skalierungsgruppe oder die Konfiguration der in der Skalierungsgruppe ausgeführten Anwendungen ändern, wie Sie die Verfügbarkeit verwalten und vieles mehr.

## <a name="fundamental-concepts"></a>Grundlegende Konzepte

### <a name="the-scale-set-model"></a>Skalierungsgruppenmodell

Eine Skalierungsgruppe enthält ein Skalierungsgruppenmodell, das den *gewünschten* Zustand der gesamten Skalierungsgruppe erfasst. Zum Abfragen des Modells für eine Skalierungsgruppe können Sie Folgendes verwenden:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).)

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).)

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).)

Sie können auch [resources.azure.com](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen des Modells für eine Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der CLI:

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



### <a name="the-scale-set-instance-view"></a>Instanzenansicht für die Skalierungsgruppe

Eine Skalierungsgruppe verfügt außerdem über eine Skalierungsgruppen-Instanzenansicht, die den aktuellen *Laufzeitstatus* der gesamten Skalierungsgruppe erfasst. Zum Abfragen der Instanzenansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).)

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).)

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).)

Sie können auch [resources.azure.com](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen der Instanzenansicht für eine Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der CLI:

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

Wie Sie sehen, bieten diese Eigenschaften eine Übersicht über den aktuellen Laufzeitstatus der virtuellen Computer in der Skalierungsgruppe, einschließlich des Status der auf die Skalierungsgruppe angewendeten Erweiterungen (aus Platzgründen weggelassen).



### <a name="the-scale-set-vm-model-view"></a>VM-Modellansicht der Skalierungsgruppe

Nicht nur die Skalierungsgruppe verfügt über eine Modellansicht, sondern auch jeder darin enthaltene virtuelle Computer. Zum Abfragen der Modellansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).)

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).)

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).)

Sie können auch [resources.azure.com](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen des Modells für einen virtuellen Computer in einer Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der CLI:

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

Wie Sie sehen, beschreiben diese Eigenschaften die Konfiguration des virtuellen Computers selbst, nicht die Konfiguration der gesamten Skalierungsgruppe. Beispielsweise besitzt das Skalierungsgruppenmodell die Eigenschaft `overprovision`, das Modell für einen virtuellen Computer in einer Skalierungsgruppe besitzt diese Eigenschaft jedoch nicht. Dieser Unterschied ist darauf zurückzuführen, dass die Überbereitstellung eine Eigenschaft für die Skalierungsgruppe als Ganzes und nicht für einzelne virtuelle Computer in der Skalierungsgruppe ist. (Weitere Informationen zur Überbereitstellung finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="the-scale-set-vm-instance-view"></a>VM-Instanzenansicht für die Skalierungsgruppe

Nicht nur die Skalierungsgruppe verfügt über eine Instanzenansicht, sondern auch jeder darin enthaltene virtuelle Computer. Zum Abfragen der Instanzenansicht für eine Skalierungsgruppe können Sie Folgendes verwenden:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).)

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).)

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).)

Sie können auch [resources.azure.com](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Abfragen der Instanzenansicht für einen virtuellen Computer in einer Skalierungsgruppe verwenden.

Die genaue Darstellung der Ausgabe hängt von den für den Befehl angegebenen Optionen ab. Nachfolgend sehen Sie eine Beispielausgabe der CLI:

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

Wie Sie sehen, beschreiben diese Eigenschaften den aktuellen Laufzeitstatus des virtuellen Computers selbst, einschließlich aller auf die Skalierungsgruppe angewendeten Erweiterungen (aus Platzgründen weggelassen).




## <a name="how-to-update-global-scale-set-properties"></a>Aktualisieren von globalen Skalierungsgruppeneigenschaften

Wenn Sie eine globale Skalierungsgruppeneigenschaft aktualisieren möchten, müssen Sie die Eigenschaft im Skalierungsgruppenmodell aktualisieren. Für die Aktualisierung können Sie Folgendes verwenden:

REST-API: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).)

Resource Manager-Vorlagen: Alternativ können Sie unter Verwendung der Eigenschaften aus der REST-API eine Resource Manager-Vorlage bereitstellen, um globale Skalierungsgruppeneigenschaften zu aktualisieren.

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).)

CLI. Ändern einer Eigenschaft: `az vmss update --set {propertyPath}={value}`. Hinzufügen eines Objekts zu einer Listeneigenschaft in einer Skalierungsgruppe: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Entfernen eines Objekts aus einer Listeneigenschaft in einer Skalierungsgruppe: `az vmss update --remove {propertyPath} {indexToRemove}`. (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update).) Falls Sie die Skalierungsgruppe zuvor mithilfe des Befehls `az vmss create` bereitgestellt haben, können Sie alternativ erneut den Befehl `az vmss create` ausführen, um die Skalierungsgruppe zu aktualisieren. Dabei müssen Sie sicherstellen, dass – mit Ausnahme der zu ändernden Eigenschaften – alle Eigenschaften im Befehl `az vmss create` den vorherigen Eigenschaften entsprechen.



Sie können auch [resources.azure.com](https://resources.azure.com) oder die [Azure SDKs](https://azure.microsoft.com/downloads/) zum Aktualisieren des Skalierungsgruppenmodells verwenden.

Nach der Aktualisierung des Skalierungsgruppenmodells gilt die neue Konfiguration für alle in der Skalierungsgruppe erstellten neuen virtuellen Computer. Die Modelle für die vorhandenen virtuellen Computer in der Skalierungsgruppe müssen aber dennoch auf das aktuelle Skalierungsgruppenmodell aktualisiert werden. Die Modelle der einzelnen virtuellen Computer enthalten eine boolesche Eigenschaften namens `latestModelApplied`, die angibt, ob ein virtueller Computer auf das aktuelle Skalierungsgruppenmodell aktualisiert wurde. (`true` bedeutet, dass der virtuelle Computer auf das aktuelle Modell aktualisiert wurde.)




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Aktualisieren von virtuellen Computer auf das aktuelle Skalierungsgruppenmodell

Für Skalierungsgruppen gilt eine „Upgraderichtlinie“, die festlegt, wie virtuelle Computer auf das aktuelle Skalierungsgruppenmodell aktualisiert werden. Die drei Modi für die Upgraderichtlinie lauten wie folgt:

- Automatisch: In diesem Modus gibt die Skalierungsgruppe keine Garantie in Bezug auf die Reihenfolge, in der virtuelle Computer heruntergefahren werden. Die Skalierungsgruppe fährt unter Umständen alle virtuellen Computer gleichzeitig herunter. 
- Parallel: In diesem Modus führt die Skalierungsgruppe ein Rollout des Updates in Batches mit optionaler Pause zwischen den Batches aus.
- Manuell: In diesem Modus geschieht beim Aktualisieren des Skalierungsgruppenmodells nichts mit den vorhandenen virtuellen Computern. Um vorhandene virtuelle Computer zu aktualisieren, müssen Sie für jeden virtuellen Computer ein manuelles Upgrade ausführen. Sie können das manuelle Upgrade über folgende Komponenten ausführen:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).)

PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).)

CLI: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).)

Sie können auch die [Azure SDKs](https://azure.microsoft.com/downloads/) verwenden, um ein manuelles Upgrade für einen virtuellen Computer in einer Skalierungsgruppe auszuführen.

>[!NOTE]
> Für Service Fabric-Cluster kann nur der Modus „Automatisch“ verwendet werden, das Update wird jedoch anders gehandhabt. Weitere Informationen zu Service Fabric-Updates finden Sie in der [Service Fabric-Dokumentation](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Eine der Änderungen, die an globalen Skalierungsgruppeneigenschaften vorgenommen werden, entspricht nicht der Upgraderichtlinie. Hierbei handelt es sich um Änderungen am Betriebssystemprofil der Skalierungsgruppe (etwa am Administratorbenutzernamen und am Kennwort). Diese Eigenschaften können nur in API-Version 2017-12-01 oder höher geändert werden. Diese Änderungen gelten nur für virtuelle Computer, die nach der Änderung im Skalierungsgruppenmodell erstellt wurden. Um vorhandene virtuelle Computer zu aktualisieren, müssen Sie ein Reimaging aller vorhandenen virtuellen Computer durchführen. Für dieses Reimaging können Sie Folgendes verwenden:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).)

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Weitere Informationen finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).)

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).)

Sie können auch die [Azure SDKs](https://azure.microsoft.com/downloads/) verwenden, um ein Reimaging für einen virtuellen Computer in einer Skalierungsgruppe durchzuführen.




## <a name="properties-with-restrictions-on-modification"></a>Eigenschaften mit Einschränkungen bei der Änderung

### <a name="create-time-properties"></a>Eigenschaften zur Erstellungszeit

Einige Eigenschaften können nur beim anfänglichen Erstellen der Skalierungsgruppe festgelegt werden. Zu diesen Eigenschaften zählen folgende:

- Zonen
- Imagereferenzherausgeber
- Imagereferenzangebot

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Eigenschaften, die nur auf Grundlage des aktuellen Werts geändert werden können

Einige Eigenschaften können mit Ausnahmen je nach aktuellem Wert geändert werden. Zu diesen Eigenschaften zählen folgende:

- singlePlacementGroup: Wenn für „singlePlacementGroup“ die Option „true“ festgelegt ist, kann diese in „false“ geändert werden. Wenn für „singlePlacementGroup“ jedoch „false“ festgelegt ist, kann die Einstellung **nicht** in „true“ geändert werden.
- subnet: Das Subnetz einer Skalierungsgruppe kann geändert werden, solange sich das ursprüngliche Subnetz und das neue Subnetz im gleichen virtuellen Netzwerk befinden.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschaften, für die zur Änderung die Aufhebung der Zuordnung erforderlich ist

Einige Eigenschaften können nur in bestimmte Werte geändert werden, wenn die Zuordnung der virtuellen Computer in der Skalierungsgruppe aufgehoben wird. Zu diesen Eigenschaften zählen folgende:

- SKU-Name: Wird die neue VM-SKU auf der Hardware nicht unterstützt, auf der sich die Skalierungsgruppe derzeit befindet, müssen Sie die Zuordnung der virtuellen Computer in der Skalierungsgruppe aufheben, bevor Sie den SKU-Namen ändern können. Weitere Informationen zum Ändern der Größe von virtuellen Computern finden Sie in [diesem Azure-Blogbeitrag](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM-spezifische Updates

Bestimmte Änderungen können statt auf die globalen Skalierungsgruppeneigenschaften auf spezifische virtuelle Computer angewendet werden. Derzeit wird als VM-spezifische Aktualisierung nur das Anfügen von Datenträgern an virtuelle Computer in der Skalierungsgruppe bzw. das Trennen dieser Datenträger unterstützt. Dieses Feature befindet sich in der Vorschauphase. Weitere Informationen finden Sie in der [Vorschaudokumentation](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Szenarien: Anwendungsupdates, Betriebssystemupdates usw.

### <a name="application-updates"></a>Anwendungsupdates

Wenn eine Anwendung über Erweiterungen in einer Skalierungsgruppe bereitgestellt wird, wird durch eine Aktualisierung der Erweiterungskonfiguration eine Aktualisierung der Anwendung gemäß der Upgraderichtlinie ausgelöst. Beispiel: Wenn Sie eine neue Version eines Skripts für die Ausführung in einer benutzerdefinierten Skripterweiterung besitzen, können Sie die fileUris-Eigenschaft aktualisieren, um auf das neue Skript zu verweisen. In einigen Fällen möchten Sie jedoch eine Aktualisierung erzwingen, auch wenn die Erweiterungskonfiguration unverändert bleibt. (Beispiel: Sie haben das Skript aktualisiert, ohne den URI des Skripts zu ändern.) In diesen Fällen können Sie „forceUpdateTag“ ändern, um eine Aktualisierung zu erzwingen. Die Azure-Plattform interpretiert diese Eigenschaft nicht. Die Änderung ihres Werts hat daher keine Auswirkung darauf, wie die Erweiterung ausgeführt wird. Die Änderung erzwingt lediglich eine erneute Ausführung der Erweiterung. Weitere Informationen zu „forceUpdateTag“ finden Sie in der [REST-API-Dokumentation für Erweiterungen](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Anwendungen werden häufig auch über ein benutzerdefiniertes Image bereitgestellt. Dieses Szenario wird im folgenden Abschnitt (Betriebssystemupdates) behandelt.

### <a name="os-updates"></a>Betriebssystemupdates

Bei Verwendung von Plattformimages können Sie das Image durch Änderung von „imageReference“ aktualisieren. (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).)

>[!NOTE]
> Bei Plattformimages wird als Imagereferenzversion häufig „aktuelle Version“ angegeben. Das bedeutet, dass beim Erstellen, horizontalen Hochskalieren und Durchführen eines Reimagings die virtuellen Computer mit der neuesten verfügbaren Version erstellt werden. Es bedeutet jedoch **nicht**, dass das Betriebssystemimage im Laufe der Zeit automatisch aktualisiert wird, wenn neue Imageversionen veröffentlicht werden. Dies ist eine separate Funktion, die zurzeit als Vorschau verfügbar ist. Weitere Informationen finden Sie in der [Dokumentation zu automatischen Betriebssystemupgrades](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Bei Verwendung von benutzerdefinierten Images können Sie das Image durch Aktualisierung der Imagereferenz-ID aktualisieren. (Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).)

## <a name="examples"></a>Beispiele

### <a name="updating-the-os-image-for-your-scale-set"></a>Aktualisieren des Betriebssystemimages für Ihre Skalierungsgruppe

Angenommen, Sie besitzen eine Skalierungsgruppe mit einer alten Version von Ubuntu LTS 16.04 und möchten nun ein Update auf eine neuere Version von Ubuntu LTS 16.04 (etwa Version 16.04.201801090) ausführen. Die Eigenschaft für die Imagereferenzversion ist nicht Teil einer Liste, daher können Sie diese Eigenschaften direkt mit den folgenden Befehlen ändern:

Powershell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

CLI: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Aktualisieren des Lastenausgleichs für Ihre Skalierungsgruppe

Angenommen, Sie besitzen eine Skalierungsgruppe mit Azure Load Balancer und möchten Azure Load Balancer durch Azure Application Gateway ersetzen. Die Eigenschaften des Lastenausgleichsmoduls und des Anwendungsgateways für eine Skalierungsgruppe sind Teil einer Liste, daher können Sie die Befehle zum Entfernen und Hinzufügen von Listenelementen verwenden, statt die Eigenschaften direkt zu ändern:

Powershell:
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Bei diesen Befehlen wird davon ausgegangen, dass für die Skalierungsgruppe nur eine IP-Konfiguration und ein Lastenausgleichsmodul festgelegt sind. Sind mehrere vorhanden, müssen Sie möglicherweise einen Listenindex ungleich 0 verwenden.
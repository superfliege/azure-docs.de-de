---
title: Skalierungsgruppen für an virtuelle Azure-Computer angefügte Datenträger | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie angefügte Datenträger mit VM-Skalierungsgruppen verwenden.
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: ec11a2d66530129fb61d97681e6882b887c8654c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Azure-VM-Skalierungsgruppen und angefügte Datenträger
Zur Erweiterung des verfügbaren Speicherplatzes unterstützen [Azure-VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/) VM-Instanzen mit angefügten Datenträgern. Datenträger können einer Skalierungsgruppe beim Erstellen der Skalierungsgruppe oder zu einem späteren Zeitpunkt hinzugefügt werden.

> [!NOTE]
>  Wenn Sie eine Skalierungsgruppe mit angefügten Datenträgern erstellen, müssen Sie die Datenträger über einen virtuellen Computer einbinden und formatieren, um sie verwenden zu können – genau wie bei eigenständigen virtuellen Azure-Computern. Eine einfache Möglichkeit ist die Verwendung einer benutzerdefinierten Skripterweiterung, die ein Skript aufruft, um alle Datenträger auf einem virtuellen Computer zu partitionieren und zu formatieren. Beispiele hierzu finden Sie unter [Azure CLI 2.0](tutorial-use-disks-cli.md#prepare-the-data-disks) bzw. unter [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Erstellen und Verwalten von Datenträgern in einer Skalierungsgruppe
Ausführliche Informationen zum Erstellen einer Skalierungsgruppe mit angefügten Datenträgern, zum Vorbereiten und Formatieren sowie zum Hinzufügen und Entfernen von Datenträgern finden Sie in den folgenden Tutorials:

- [Azure CLI 2.0](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

Im weiteren Verlauf dieses Artikels werden bestimmte Verwendungsfälle behandelt – beispielsweise Service Fabric-Cluster, die Datenträger benötigen, oder das Anfügen vorhandener Datenträger mit Inhalt an eine Skalierungsgruppe.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Erstellen eines Service Fabric-Clusters mit angefügten Datenträgern
Jedem [Knotentyp](../service-fabric/service-fabric-cluster-nodetypes.md) in einem in Azure ausgeführten [Service Fabric](/azure/service-fabric)-Cluster liegt eine VM-Skalierungsgruppe zugrunde.  Mit einer Azure Resource Manager-Vorlage können Sie Datenträger an die Skalierungsgruppen anfügen, aus denen sich der Service Fabric-Cluster zusammensetzt. Sie können eine [vorhandene Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates) als Ausgangspunkt verwenden. Fügen Sie in der Vorlage im Speicherprofil (_storageProfile_) der Ressourcen vom Typ _Microsoft.Compute/virtualMachineScaleSets_ einen Abschnitt namens _dataDisks_ ein, und stellen Sie die Vorlage bereit. Im folgenden Beispiel wird ein Datenträger mit einer Kapazität von 128 GB angefügt:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Die Datenträger können automatisch partitioniert, formatiert und eingebunden werden, wenn der Cluster bereitgestellt wird.  Fügen Sie dem Erweiterungsprofil (_extensionProfile_) des VM-Profils (_virtualMachineProfile_) der Skalierungsgruppen eine benutzerdefinierte Skripterweiterung hinzu.

Fügen Sie Folgendes hinzu, um die Datenträger in einem Windows-Cluster automatisch vorzubereiten:

```json
{
    "name": "customScript",    
    "properties": {    
        "publisher": "Microsoft.Compute",    
        "type": "CustomScriptExtension",    
        "typeHandlerVersion": "1.8",    
        "autoUpgradeMinorVersion": true,    
        "settings": {    
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Fügen Sie Folgendes hinzu, um die Datenträger in einem Linux-Cluster automatisch vorzubereiten:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Hinzufügen von vorab aufgefüllten Datenträgern zu einer vorhandenen Skalierungsgruppe 
> Wenn Sie Datenträger zu einem vorhandenen Skalierungsgruppenmodell hinzufügen, wird der Datenträger standardmäßig immer leer erstellt. (Dieses Szenario umfasst auch neue von der Skalierungsgruppe erstellte Instanzen.) Das Verhalten ist darin begründet, dass die Skalierungsgruppendefinition einen leeren Datenträger enthält. Wählen Sie eine der folgenden beiden Optionen, um vorab aufgefüllte Datenträger für eine vorhandene Skalierungsgruppe zu erstellen:

* Kopieren Sie Daten vom virtuellen Instanz 0-Computer auf die Datenträger in anderen virtuellen Computern, indem Sie ein benutzerdefiniertes Skript ausführen.
* Erstellen Sie ein verwaltetes Image mit dem Betriebssystemdatenträger sowie dem Datenträger (mit den erforderlichen Daten), und erstellen Sie dann eine neue Skalierungsgruppe mit dem Image. Auf diese Weise enthält jeder neue virtuelle Computer, der erstellt wird, einen Datenträger, der in der Definition der Skalierungsgruppe bereitgestellt wird. Da diese Definition auf ein Image mit einem Datenträger mit benutzerdefinierten Daten verweist, gelten für jeden virtuellen Computer in der Skalierungsgruppe automatisch diese Änderungen.

> Informationen zum Erstellen eines benutzerdefinierten Images finden Sie unter [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](/azure/virtual-machines/windows/capture-image-resource/). 

> Der Benutzer muss den virtuellen Instanz 0-Computer mit den erforderlichen Daten erfassen und dann diese VHD für die Imagedefinition verwenden.


## <a name="additional-notes"></a>Zusätzliche Hinweise
Unterstützung für Azure Managed Disks und Skalierungsgruppen, die an Datenträger angefügt sind, ist über die API-Version [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) oder höher der Microsoft.Compute-API verfügbar.

In der ursprünglichen Implementierung der Unterstützung angefügter Datenträger für Skalierungsgruppen ist es nicht möglich, Datenträger zu einzelnen virtuellen Computer hinzuzufügen oder davon zu entfernen.

Die Unterstützung des Azure-Portals für angefügte Datenträger in Skalierungsgruppen ist anfangs beschränkt. Je nach Ihren Anforderungen können Sie Azure-Vorlagen, CLI, PowerShell, SDKs und REST API zum Verwalten angefügter Datenträger verwenden.



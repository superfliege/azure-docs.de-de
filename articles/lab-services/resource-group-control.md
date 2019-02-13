---
title: Angeben einer Ressourcengruppe für virtuelle Computer in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Ressourcengruppe für virtuelle Computer in einem Lab in Azure DevTest Labs angeben.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2019
ms.author: spelluru
ms.openlocfilehash: ddda9ef2b9bb716f7cdd33aa8fe9233f6c7d8e82
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55748999"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Angeben einer Ressourcengruppe für virtuelle Labcomputer in Azure DevTest Labs
Als Labbesitzer können Sie Ihre Lab-VMs so konfigurieren, dass sie in einer bestimmten Ressourcengruppe erstellt werden. Verwenden Sie diese Funktion, damit Sie in Ihrem Azure-Abonnement keine Ressourcengruppenlimits erreichen. Diese Funktion ermöglicht Ihnen auch, Ihre Labressourcen in einer einzelnen Ressourcengruppe zusammenzufassen. Außerdem vereinfacht sie das Nachverfolgen dieser Ressourcen und das Anwenden von [Richtlinien](../governance/policy/overview.md), um sie auf Ressourcengruppenebene zu verwalten.

Mit dieser Funktion können Sie mithilfe eines Skripts eine neue oder vorhandene Ressourcengruppe in Ihrem Azure-Abonnement für alle Ihre Lab-VMs angeben. DevTest Labs unterstützt diese Funktion derzeit über eine API. 

## <a name="api-to-configure-a-resource-group-for-labs-vms"></a>API zum Konfigurieren einer Ressourcengruppe für virtuelle Labcomputer
Nun betrachten wir die Optionen, die Sie bei der Verwendung dieser API als Labbesitzer haben: 

- Sie können die **Ressourcengruppe des Labs** für alle virtuellen Computer auswählen.
- Sie können eine andere **vorhandene Ressourcengruppe** als die Ressourcengruppe des Labs für alle virtuellen Computer auswählen.
- Sie können den Namen einer **neuen Ressourcengruppe** für alle virtuellen Computer eingeben.
- Sie können das bestehende Verhalten fortsetzen, d.h., für jeden virtuellen Computer im Lab wird eine Ressourcengruppe erstellt.
 
Diese Einstellung gilt für neue virtuelle Computer, die im Lab erstellt werden. Die älteren virtuellen Computer in Ihrem Lab, die in eigenen Ressourcengruppen erstellt wurden, sind davon nicht betroffen. Sie können diese virtuellen Computer jedoch aus ihren jeweiligen Ressourcengruppen zur gemeinsamen Ressourcengruppe migrieren, sodass sich alle Ihre Lab-VMs in dieser befinden. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe](../azure-resource-manager/resource-group-move-resources.md). In Ihrem Lab erstellte Umgebungen verbleiben weiterhin in ihren eigenen Ressourcengruppen.

### <a name="how-to-use-this-api"></a>Verwenden dieser API:
- Verwenden Sie Version **2018_10_15_preview** dieser API. 
- Wenn Sie eine neue Ressourcengruppe angeben, stellen Sie sicher, dass Sie über **Schreibberechtigungen für Ressourcengruppen** in Ihrem Abonnement verfügen. Ohne Schreibberechtigungen führt das Erstellen neuer virtueller Computer in der angegebenen Ressourcengruppe zu einem Fehler. 
- Übergeben Sie bei Verwendung der API die **vollständige Ressourcengruppen-ID**. Beispiel: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Stellen Sie sicher, dass sich die Ressourcengruppe im selben Abonnement wie die Umgebung befindet. 

## <a name="use-powershell"></a>Verwenden von PowerShell 
Das folgende Beispiel beschreibt, wie alle Lab-VMs mithilfe eines PowerShell-Skripts in einer neuen Ressourcengruppe erstellt werden.

```PowerShell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Rufen Sie das Skript mit dem folgenden Befehl auf („ResourceGroup.ps1“ ist die Datei mit dem vorhergehenden Skript): 

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Wenn Sie das Lab mit einer Azure Resource Manager-Vorlage erstellen, verwenden Sie die **vmCreationResourceGroupId**-Eigenschaft im Abschnitt mit den Labeigenschaften der Resource Manager-Vorlage, wie im folgenden Beispiel gezeigt:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "6e6f668f-992b-435c-bac3-d328b745cd25"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln: 

- [Festlegen von Richtlinien für ein Lab](devtest-lab-get-started-with-lab-policies.md)
- [Häufig gestellte Fragen](devtest-lab-faq.md)

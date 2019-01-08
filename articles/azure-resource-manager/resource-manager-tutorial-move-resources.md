---
title: Verschieben von Azure-Ressourcen in eine andere Ressourcengruppe | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen aus einer Ressourcengruppe in eine andere Ressourcengruppe oder aus einem Abonnement in ein anderes Abonnement verschieben können.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/19/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0ae29146b1b44f3017d37b3cebf7ec4cf39115d0
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731755"
---
# <a name="tutorial-move-azure-resources-to-another-resource-group-or-subscription"></a>Tutorial: Verschieben von Azure-Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement

Erfahren Sie, wie Sie Azure-Ressourcen aus einer Ressourcengruppe in eine andere Ressourcengruppe verschieben. Sie können auch Azure-Ressourcen aus einem Azure-Abonnement in ein anderes Azure-Abonnement verschieben. In diesem Tutorial verwenden Sie eine Resource Manager-Vorlage, um zwei Ressourcengruppen und ein Speicherkonto bereitzustellen. Danach verschieben Sie das Speicherkonto aus einer Ressourcengruppe in die andere.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten der Ressourcen
> * Überprüfen, ob die Ressource verschoben werden kann
> * Checkliste vor dem Verschieben der Ressource
> * Überprüfen des Verschiebevorgangs
> * Verschieben der Ressource
> * Bereinigen der Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prepare-the-resources"></a>Vorbereiten der Ressourcen

Eine Vorlage wurde erstellt und in einem [freigegebenem Speicherkonto](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json) abgelegt. Die Vorlage definiert zwei Ressourcengruppen und ein Speicherkonto. Wenn Sie die Vorlage bereitstellen, müssen Sie einen Projektnamen angeben. Der Projektname wird verwendet, um eindeutige Ressourcennamen zu generieren.  Die folgende JSON-Code ist aus der Vorlage extrahiert:

```json
"variables": {
  "resourceGroupSource": {
    "name": "[concat(parameters('projectName'), 'rg1')]",
    "location": "eastus"
  },
  "resourceGroupDestination": {
    "name": "[concat(parameters('projectName'), 'rg2')]",
    "location": "westus"      
  },
  "storageAccount": {
    "name": "[concat(parameters('projectName'), 'store')]",
    "location": "eastus"
  }
},
```

Beachten Sie die im JSON-Code angegebenen Standorte. Die beiden Ressourcengruppen befinden sich „USA, Osten“ und „USA, Westen“. Das Speicherkonto befindet sich in „USA, Osten“. Wenn Sie eine Ressource in eine andere Ressourcengruppe mit einem anderen Speicherort verschieben, ändert der Verschiebevorgang nicht den Speicherort der Ressource.

Wählen Sie **Ausprobieren** aus, um die Cloud Shell zu öffnen, und führen Sie dann das PowerShell-Skript in der Cloud Shell aus:

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
New-AzureRmDeployment `
    -Name $projectname `
    -Location "centralus" `
    -TemplateUri "https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json" `
    -projectName $projectName
```

Warten Sie, bis das Skript erfolgreich abgeschlossen wurde, öffnen Sie dann das [Azure-Portal](https://portal.azure.com), und vergewissern Sie sich, dass die Ressourcengruppen und das Speicherkonto erwartungsgemäß bereitgestellt werden.

> [!NOTE]
> Da die Vorlage zwei Ressourcengruppe definiert, wird diese Bereitstellung als Bereitstellung auf Abonnementebene angesehen. Die Portalvorlagenbereitstellung unterstützt keine Bereitstellungen auf Abonnementebene. Daher wird Azure PowerShell in diesem Tutorial verwendet. Die Azure-Befehlszeilenschnittstelle unterstützt auch Bereitstellungen auf Abonnementebene. Weitere Informationen finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen für ein Azure-Abonnement](./deploy-to-subscription.md).

## <a name="verify-the-resource-can-be-moved"></a>Überprüfen, ob die Ressource verschoben werden kann

Nicht alle Ressource können verschoben werden. Die in diesem Tutorial verwendete Ressource ist ein Speicherkonto, das verschoben werden kann. Um zu überprüfen, ob eine Ressource verschoben werden kann, lesen Sie [Dienste, die verschoben werden können](./resource-group-move-resources.md#services-that-can-be-moved).

## <a name="checklist-before-moving-resources"></a>Checkliste vor dem Verschieben von Ressourcen

Dieser Schritt ist optional für dieses Tutorial, weil er bereits ausgeführt wurde.

Vor dem Verschieben einer Ressource müssen einige wichtige Schritte ausgeführt werden. Weitere Informationen finden Sie unter [Checkliste vor dem Verschieben von Ressourcen](./resource-group-move-resources.md#checklist-before-moving-resources).

## <a name="validate-the-move"></a>Überprüfen der Verschiebung

Überprüfen der Verschiebung ist optional für dieses Tutorial, weil dieser Schritt bereits ausgeführt wurde.

Der Vorgang zum Überprüfen der Verschiebung ermöglicht Ihnen, Ihr Verschiebungsszenario zu testen, ohne tatsächlich Ressourcen zu verschieben. Verwenden Sie diesen Vorgang, um zu ermitteln, ob die Verschiebung erfolgreich ausgeführt wird. Weitere Informationen finden Sie unter [Überprüfen der Verschiebung](./resource-group-move-resources.md#validate-move).

## <a name="move-the-resource"></a>Verschieben der Ressource

Das Speicherkonto befindet sich in der Quellressourcengruppe (rg1). Führen Sie das folgende PowerShell-Skript aus, um die Ressource in die Zielressourcengruppe (rg2) zu verschieben. Verwenden Sie unbedingt genau den Projektnamen, den Sie beim Bereitstellen der Ressourcen verwendet haben.

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
$resourceGroupSource = $projectName + "rg1"
$resourceGroupDestination = $projectName + "rg2"
$storageAccountName = $projectName + "store"

$storageAccount = Get-AzureRmResource -ResourceGroupName $resourceGroupSource -ResourceName $storageAccountName
Move-AzureRmResource -DestinationResourceGroupName $resourceGroupDestination -ResourceId $storageAccount.ResourceId
```

Öffnen Sie das [Azure-Portal](https://portal.azure.com), überprüfen Sie, ob das Speicherkonto in die andere Ressourcengruppe verschoben wurde, und überprüfen Sie ebenfalls, ob der Standort des Speicherkontos weiterhin „USA, Osten“ ist.

Beim Verschieben von Ressourcen werden die Quell- und Zielgruppe für die Dauer des Vorgangs gesperrt. Schreib- und Löschvorgänge in den Ressourcengruppen werden bis zum Abschluss der Verschiebung blockiert. Diese Sperre bedeutet, dass Sie in Ressourcengruppen keinen Ressourcen hinzufügen, aktualisieren oder löschen können. Es heißt aber nicht, dass die Ressourcen eingefroren sind. Wenn Sie beispielsweise eine SQL Server-Instanz und ihre Datenbank in eine neue Ressourcengruppe verschieben, weist eine Anwendung, die die Datenbank nutzt, keine Ausfallzeiten auf. Sie hat weiterhin Lese- und Schreibzugriff auf die Datenbank.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Quellressourcengruppe aus.  
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.
5. Wählen Sie den Namen der Zielressourcengruppe aus.  
6. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie ein Speicherkonto aus einer Ressourcengruppe in eine andere Ressourcengruppe verschoben werden kann. Bisher haben Sie sich mit einem Speicherkonto oder mit mehreren Speicherkontoinstanzen beschäftigt. Im nächsten Tutorial entwickeln Sie eine Vorlage mit mehreren Ressourcen und Ressourcentypen. Einige der Ressourcen verfügen über abhängige Ressourcen.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

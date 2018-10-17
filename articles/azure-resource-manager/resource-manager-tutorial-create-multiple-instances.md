---
title: Erstellen mehrerer Ressourceninstanzen mit Azure Resource Manager | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, um mehrere Azure-Ressourceninstanzen bereitzustellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/10/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 63a18a6ae0ee4c6e0a01bd7ac4a26a4fb89746c2
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419487"
---
# <a name="tutorial-create-multiple-resource-instances-using-resource-manager-templates"></a>Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen

Erfahren Sie, wie Sie die Azure Resource Manager-Vorlage durchlaufen können, um mehrere Instanzen einer Azure-Ressource zu erstellen. Im letzten Tutorial haben Sie eine vorhandene Vorlage zum Erstellen eines verschlüsselten Azure Storage-Kontos geändert. In diesem Tutorial ändern Sie die gleiche Vorlage, um drei Speicherkontoinstanzen zu erstellen.

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/).
* Die Erweiterung „Azure Resource Manager-Tools“. Informationen zur Installation finden unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Die in dieser Schnellstartanleitung verwendete Vorlage heißt [Standardspeicherkonto erstellen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Die Vorlage definiert eine Azure Storage-Kontoressource.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Das Ziel dieses Tutorials ist, mit der Ressourceniteration drei Speicherkonten zu erstellen.  Die Beispielvorlage erstellt nur ein einzelnes Speicherkonto. 

Nehmen Sie von Visual Studio Code aus die folgenden vier Änderungen vor:

![Erstellen mehrerer Instanzen im Azure Resource Manager](./media/resource-manager-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Fügen Sie der Ressourcendefinition des Speicherkontos ein `copy`-Element hinzu. Im copy-Element geben Sie die Anzahl von Iterationen und einen Namen für diese Schleife an. Der count-Wert muss eine positive ganze Zahl sein und darf 800 nicht überschreiten.
2. Die `copyIndex()`-Funktion gibt die aktuelle Iteration in der Schleife zurück. `copyIndex()` ist nullbasiert. Zum Versetzen des Indexwerts können Sie einen Wert in der copyIndex()-Funktion übergeben. Beispiel: *copyIndex(1)*.
3. Löschen Sie das **variables**-Element, da es nicht mehr verwendet wird.
4. Löschen Sie das **outputs**-Element.

Die fertige Vorlage sieht folgendermaßen aus:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Weitere Informationen zum Erstellen mehrerer Instanzen finden Sie unter [Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in Azure Resource Manager-Vorlagen](./resource-group-create-multiple.md).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie in der Visual Studio Code-Schnellstartanleitung im Abschnitt [Bereitstellen der Vorlage](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template).

Lassen Sie zum Auflisten aller drei Speicherkonten den --name-Parameter aus:

# <a name="clitabcli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)
```cli
az storage account list --resource-group <ResourceGroupName>
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

```powershell
Get-AzureRmStorageAccount -ResourceGroupName <ResourceGroupName>
```

---

Vergleichen Sie die Speicherkontennamen mit der Namensdefinition in der Vorlage.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, mehrere Instanzen von Speicherkonten zu erstellen. Bisher haben Sie ein Speicherkonto oder mehrere Instanzen von Speicherkonten erstellt. Im nächsten Tutorial entwickeln Sie eine Vorlage mit mehreren Ressourcen und Ressourcentypen. Einige der Ressourcen verfügen über abhängige Ressourcen.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

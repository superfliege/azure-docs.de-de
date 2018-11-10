---
title: Erstellen verknüpfter Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Informationen zum Erstellen verknüpfter Azure Resource Manager-Vorlagen zum Erstellen virtueller Computer
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b08013941c1cf83b3eb006543d699eb7e1356ff0
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239983"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Tutorial: Erstellen verknüpfter Azure Resource Manager-Vorlagen

Informationen zum Erstellen verknüpfter Azure Resource Manager-Vorlagen. Mit verknüpften Vorlagen können Sie eine Vorlage durch eine andere Vorlage aufrufen lassen. Dies eignet sich hervorragend zum Modularisieren von Vorlagen. In diesem Tutorial verwenden Sie die gleiche Vorlage wie in [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./resource-manager-tutorial-create-multiple-instances.md), die einen virtuellen Computer, ein virtuelles Netzwerk und andere abhängige Ressourcen einschließlich eines Speicherkontos erstellt. Sie separieren die Speicherkontoressource in einer verknüpften Vorlage.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Erstellen der verknüpften Vorlage
> * Hochladen der verknüpften Vorlage
> * Verknüpfen mit der verknüpften Vorlage
> * Konfigurieren der Abhängigkeit
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der [Erweiterung „Azure Resource Manager-Tools“](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers). Dieselbe Vorlage wird in [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./resource-manager-tutorial-create-multiple-instances.md) verwendet. Sie speichern zwei Kopien der gleichen Vorlage, die verwendet werden sollen als:

* **Die Hauptvorlage**: Erstellen aller Ressourcen mit Ausnahme des Speicherkontos.
* **Die verknüpfte Vorlage**: Erstellen des Speicherkontos.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Die Vorlage definiert fünf Ressourcen:

    * `Microsoft.Storage/storageAccounts`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts). 
    * `Microsoft.Network/publicIPAddresses`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses). 
    * `Microsoft.Network/virtualNetworks`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks). 
    * `Microsoft.Network/networkInterfaces`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces). 
    * `Microsoft.Compute/virtualMachines`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
5. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.
6. Wählen Sie **Datei**>**Speichern unter** aus, um eine andere Kopie der Datei mit dem Namen **linkedTemplate.json** zu erstellen.

## <a name="create-the-linked-template"></a>Erstellen der verknüpften Vorlage

Die verknüpfte Vorlage erstellt ein Speicherkonto. Die verknüpfte Vorlage ist fast identisch mit der eigenständigen Vorlage, die ein Speicherkonto erstellt. In diesem Tutorial muss die verknüpfte Vorlage einen Wert an die Hauptvorlage zurückgeben. Dieser Wert wird im `outputs`-Element definiert.

1. Öffnen Sie „linkedTemplate.json“ in Visual Studio Code, falls die Datei noch nicht geöffnet ist.
2. Nehmen Sie die folgenden Änderungen vor:

    * Entfernen Sie alle Ressourcen mit Ausnahme des Speicherkontos. Sie entfernen insgesamt vier Ressourcen.
    * Aktualisieren Sie das **outputs**-Element, sodass es so aussieht:

        ```json
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
        ```
        Die VM-Ressourcendefinition in der Hauptvorlage benötigt **storageUri**.  Sie geben den Wert als Ausgabewert an die Hauptvorlage zurück.
    * Entfernen Sie die Parameter, die nie verwendet werden. Diese Parameter sind mit einer grünen Wellenlinie unterlegt. Es sollte nur links ein Parameter namens **location** aufgerufen sein.
    * Entfernen Sie das Element **variables**. Sie sind in diesem Tutorial nicht erforderlich.
    * Fügen Sie einen Parameter namens **storageAccountName** hinzu. Der Name des Speicherkontos wird aus der Hauptvorlage als Parameter an die verknüpfte Vorlage übergeben.

    Wenn Sie fertig sind, sollte die Vorlage etwa so aussehen:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountName":{
            "type": "string",
            "metadata": {
              "description": "Azure Storage account name."
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
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
          }
        ],
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
    }
    ```
3. Speichern Sie die Änderungen.

## <a name="upload-the-linked-template"></a>Hochladen der verknüpften Vorlage

Von dort aus, wo Sie die Bereitstellung ausführen, muss der Zugriff auf die Vorlagen möglich sein. Dieser Speicherort könnte ein Azure-Speicherkonto, Github oder Dropbox sein. Wenn Ihre Vorlagen sensible Informationen enthalten, stellen Sie sicher, dass Sie den Zugriff darauf schützen. In diesem Tutorial verwenden Sie die Cloud Shell-Bereitstellungsmethode wie in [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./resource-manager-tutorial-create-multiple-instances.md). Die Hauptvorlage (azuredeploy.json) wird in die Shell hochgeladen. Die verknüpfte Vorlage (linkedTemplate.json) muss an einer beliebigen Stelle freigegeben werden.  Um die Aufgaben in diesem Tutorial zu reduzieren, wurde die im vorherigen Abschnitt definierte verknüpfte Vorlage in ein [Azure-Speicherkonto](https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json) hochgeladen.

## <a name="call-the-linked-template"></a>Aufrufen der verknüpften Vorlage

Die Hauptvorlage hat den Namen „azuredeploy.json“.

1. Öffnen Sie „azuredeploy.json“ in Visual Studio Code, falls die Datei noch nicht geöffnet ist.
2. Löschen Sie die Ressourcendefinition des Speicherkontos aus der Vorlage.
3. Fügen Sie dort, wo sich die Definition des Speicherkontos befand, den folgenden JSON-Codeausschnitt hinzu:

    ```json
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Berücksichtigen Sie dabei folgende Details:

    * Eine `Microsoft.Resources/deployments`-Ressource in der Hauptvorlage wird zum Verknüpfen mit einer anderen Vorlage verwendet.
    * Die `deployments`-Ressource hat den Namen `linkedTemplate`. Dieser Name wird zum [Konfigurieren der Abhängigkeit](#configure-dependency) verwendet.  
    * Beim Aufrufen verknüpfter Vorlagen können Sie nur den Bereitstellungsmodus [Inkrementell](./deployment-modes.md) verwenden.
    * `templateLink/uri` enthält den URI der verknüpften Vorlage. Die verknüpfte Vorlage wurde in ein freigegebenes Speicherkonto hochgeladen. Sie können den URI aktualisieren, wenn Sie die Vorlage zu einem anderen Speicherort im Internet hochladen.
    * Übergeben Sie mit `parameters` Werte aus der Hauptvorlage an die verknüpfte Vorlage.
4. Speichern Sie die Änderungen.

## <a name="configure-dependency"></a>Konfigurieren der Abhängigkeit

Wie Sie aus [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./resource-manager-tutorial-create-multiple-instances.md) wissen, hängt die VM-Ressource vom Speicherkonto ab:

![Abhängigkeitsdiagramm für Azure Resource Manager-Vorlagen](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Da das Speicherkonto nun in der verknüpften Vorlage definiert ist, müssen Sie die folgenden zwei Elemente der `Microsoft.Compute/virtualMachines`-Ressource aktualisieren.

* Konfigurieren Sie das `dependOn`-Element neu. Die Definition des Speicherkontos wird in die verknüpfte Vorlage verschoben.
* Konfigurieren Sie das `properties/diagnosticsProfile/bootDiagnostics/storageUri`-Element neu. In [Verknüpfte Vorlage erstellen](#create-the-linked-template) haben Sie einen Ausgabewert hinzugefügt:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    Die Hauptvorlage benötigt diesen Wert.

1. Öffnen Sie „azuredeploy.json“ in Visual Studio Code, falls die Datei noch nicht geöffnet ist.
2. Erweitern Sie die VM-Ressourcendefinition, aktualisieren Sie **dependsOn** wie im folgenden Screenshot gezeigt:

    ![Azure Resource Manager: verknüpfte Vorlagen – Abhängigkeit konfigurieren ](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate* ist der Name der Ressource der Bereitstellungen.  
3. Aktualisieren Sie **properties/diagnosticsProfile/bootDiagnostics/storageUri** wie im vorherigen Screenshot dargestellt.

Weitere Informationen finden Sie unter [Verwenden von verknüpften und geschachtelten Vorlagen bei der Bereitstellung von Azure-Ressourcen](./resource-group-linked-templates.md).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template). Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Siehe [Voraussetzungen](#prerequisites).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verknüpfte Vorlage entwickelt und bereitgestellt. Informationen zum Ausführen von Aufgaben nach der Bereitstellung mithilfe von VM-Erweiterungen finden Sie unter

> [!div class="nextstepaction"]
> [Tutorial: Verwenden des Azure-Bereitstellungs-Managers mit Resource Manager-Vorlagen (private Vorschau)](./deployment-manager-tutorial.md).

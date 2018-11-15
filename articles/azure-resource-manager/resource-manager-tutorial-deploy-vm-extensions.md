---
title: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 2f5cffa0b31d46d76564565001aaed678eb1a649
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613575"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Tutorial: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen

Hier erfahren Sie, wie Sie [Azure-VM-Erweiterungen](../virtual-machines/extensions/features-windows.md) verwenden, um nach der Bereitstellung Konfigurations- und Automatisierungsaufgaben für virtuelle Azure-Computer auszuführen. Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. In diesem Tutorial stellen Sie eine benutzerdefinierte Skripterweiterung aus einer Resource Manager-Vorlage bereit, um auf einem virtuellen Windows-Computer ein PowerShell-Skript auszuführen.  Das Skript installiert einen Webserver auf dem virtuellen Computer.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten eines PowerShell-Skripts
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“.  Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-a-powershell-script"></a>Vorbereiten eines PowerShell-Skripts

Ein PowerShell-Skript mit folgendem Inhalt wird über ein [Azure Storage-Konto mit öffentlichem Zugriff](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1) freigegeben:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wenn Sie die Datei an Ihrem eigenen Standort veröffentlichen möchten, müssen Sie später in diesem Tutorial das Element [fileUri] in der Vorlage aktualisieren.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Es gibt fünf Ressourcen, die von der Vorlage definiert werden:

    * `Microsoft.Storage/storageAccounts`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
5. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Fügen Sie der vorhandenen Vorlage eine VM-Erweiterungsressource mit folgendem Inhalt hinzu:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Weitere Informationen zu dieser Ressourcendefinition finden Sie bei Bedarf in der [Erweiterungsreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions). Im Anschluss sind einige zentrale Elemente aufgeführt:

* **name**: Da es sich bei der Erweiterungsressource um eine untergeordnete Ressource des VM-Objekts handelt, muss der Name mit dem VM-Namenspräfix versehen werden. Weitere Informationen finden Sie unter [Untergeordnete Ressourcen](./resource-manager-templates-resources.md#child-resources).
* **dependsOn**: Die Erweiterungsressource muss nach der Erstellung des virtuellen Computers erstellt werden.
* **fileUris**: Die Speicherorte der Skriptdateien. Falls Sie sich gegen die Verwendung der bereitgestellten Datei entscheiden, müssen Sie die Werte aktualisieren.
* **commandToExecute**: Der Befehl zum Aufrufen des Skripts.  

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template). Es empfiehlt sich, ein generiertes Kennwort für das Administratorkonto des virtuellen Computers zu verwenden. Siehe [Voraussetzungen](#prerequisites).

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Wählen Sie im Portal den virtuellen Computer aus. Verwenden Sie in der Übersicht des virtuellen **Computers die Kopieroption**rechts neben der IP-Adresse, um die IP-Adresse zu kopieren, und fügen Sie sie in eine Browserregisterkarte ein. Die Standardbegrüßungsseite für IIS wird geöffnet und sollte wie folgt aussehen:

![Azure Resource Manager: Bereitstellen von VM-Erweiterungen – benutzerdefiniertes Skript – IIS-Webserver](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen virtuellen Computer und eine VM-Erweiterung bereitgestellt. Die Erweiterung hat den IIS-Webserver auf dem virtuellen Computer installiert. Informationen zum Importieren einer BACPAC-Datei unter Verwendung der SQL-Datenbank-Erweiterung finden Sie hier:

> [!div class="nextstepaction"]
> [](./resource-manager-tutorial-deploy-vm-extensions.md)

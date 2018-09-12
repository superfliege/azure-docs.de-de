---
title: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage mit mehreren Ressourcen erstellen und über das Azure-Portal bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: e5ced038d5f1ab57939221a0392ab436560c348d
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160509"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen

Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, um mehrere Ressourcen bereitzustellen.  Nach der Erstellung der Vorlage stellen Sie sie mithilfe von Cloud Shell über das Azure-Portal bereit.

Einige der Ressourcen können erst bereitgestellt werden, wenn eine andere Ressource vorhanden ist. Sie können den virtuellen Computer beispielsweise erst erstellen, wenn sein Speicherkonto und seine Netzwerkschnittstelle vorhanden sind. Diese Beziehung definieren Sie, indem Sie eine Ressource von den anderen Ressourcen abhängig machen. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Weitere Informationen finden Sie unter [Definieren der Reihenfolge für die Bereitstellung von Ressourcen in Azure Resource Manager-Vorlagen](./resource-group-define-dependencies.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Untersuchen der Vorlage
> * Bereitstellen der Vorlage

Anhand der Anweisungen in diesem Tutorial werden ein virtueller Computer, ein virtuelles Netzwerk und einige andere abhängige Ressourcen erstellt. 

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/).
* Die Erweiterung „Azure Resource Manager-Tools“.  Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="explore-the-template"></a>Untersuchen der Vorlage

Sehen Sie sich die Vorlage in diesem Abschnitt an, und versuchen Sie, die folgenden Fragen zu beantworten:

- Wie viele Azure-Ressourcen sind in dieser Vorlage definiert?
- Bei einer der Ressourcen handelt es sich um ein Azure-Speicherkonto.  Sieht die Definition wie die Definition im letzten Tutorial aus?
- Finden Sie die Vorlagenreferenzen für die in dieser Vorlage definierten Ressourcen?
- Finden Sie die Abhängigkeiten der Ressourcen?

1. Reduzieren Sie in Visual Studio Code die Elemente, bis unter **resources** nur die Elemente der ersten und zweiten Ebene angezeigt werden:

    ![Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Es gibt fünf Ressourcen, die von der Vorlage definiert werden.
2. Erweitern Sie die erste Ressource. Es handelt sich um ein Speicherkonto. Die Definition muss mit der Definition identisch sein, die Sie zu Beginn des letzten Tutorials verwendet haben.

    ![Azure Resource Manager-Vorlagen in Visual Studio Code: Speicherkontodefinition](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Erweitern Sie die zweite Ressource. Der Ressourcentyp lautet **Microsoft.Network/publicIPAddresses**. Navigieren Sie zum Ermitteln der Vorlagenreferenz zu [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/), und geben Sie **öffentliche IP-Adresse** oder **öffentliche IP-Adressen** ins Feld **Nach Titel filtern** ein. Vergleichen Sie die Ressourcendefinition mit der Vorlagenreferenz.

    ![Azure Resource Manager-Vorlagen in Visual Studio Code: Definition der öffentlichen IP-Adresse](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Wiederholen Sie den letzten Schritt, um die Vorlagenreferenzen für die anderen in dieser Vorlage definierten Ressourcen zu ermitteln.  Vergleichen Sie die Ressourcendefinitionen mit den Referenzen.
5. Erweitern Sie die vierte Ressource:

    ![„dependsOn“ für Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    Das Element „dependsOn“ bietet die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. In diesem Beispiel handelt es sich bei dieser Ressource um eine Netzwerkschnittstelle (networkInterface).  Sie hängt von zwei weitere Ressourcen ab:

    * publicIPAddress
    * virtualNetwork

6. Erweitern Sie die fünfte Ressource. Diese Ressource ist ein virtueller Computer. Sie hängt von zwei weiteren Ressourcen ab:

    * storageAccount
    * networkInterface

Das folgende Diagramm veranschaulicht die Ressourcen und die Abhängigkeitsinformationen für diese Vorlage:

![Abhängigkeitsdiagramm für Azure Resource Manager-Vorlagen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Durch die Angabe der Abhängigkeiten wird die Lösung von Resource Manager effizient bereitgestellt. Mit Resource Manager werden das Speicherkonto, die öffentliche IP-Adresse und das virtuelle Netzwerk parallel bereitgestellt, da sie über keine Abhängigkeiten verfügen. Nach der Bereitstellung der öffentlichen IP-Adresse und des virtuellen Netzwerks wird die Netzwerkschnittstelle erstellt. Wenn alle anderen Ressourcen bereitgestellt wurden, stellt Resource Manager den virtuellen Computer bereit.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Es gibt viele Methoden zum Bereitstellen von Vorlagen.  In diesem Tutorial verwenden Sie Cloud Shell aus dem Azure-Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
2. Wählen Sie oben rechts **Cloud Shell** aus, wie in der folgenden Abbildung dargestellt:

    ![Azure-Portal, Cloud Shell](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
3. Klicken Sie in Cloud Shell oben links auf **PowerShell**.  In diesem Tutorial können Sie PowerShell verwenden.
4. Klicken Sie auf **Neu starten**.
5. Klicken Sie in Cloud Shell auf **Datei hochladen**:

    ![Azure-Portal, Cloud Shell, Datei hochladen](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
6. Wählen Sie die Datei aus, die Sie zuvor im Tutorial gespeichert haben. Der Standardname lautet **azuredeploy.json**.  Falls Sie eine Datei mit dem gleichen Dateinamen besitzen, wird die alte Datei ohne Benachrichtigung überschrieben.
7. Führen Sie in Cloud Shell den folgenden Befehl aus, um zu überprüfen, ob die Datei erfolgreich hochgeladen wurde. 

    ```shell
    ls
    ```

    ![Azure-Portal, Cloud Shell, Datei auflisten](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    Der Dateiname im Screenshot lautet „azuredeploy.json“.

8. Führen Sie in Cloud Shell den folgenden Befehl aus, um den Inhalt der JSON-Datei zu überprüfen:

    ```shell
    cat azuredeploy.json
    ```
9. Führen Sie in Cloud Shell die folgenden PowerShell-Befehle aus:

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $location = "<Enter the Azure location>"
    $vmAdmin = "<Enter the admin username>"
    $vmPassword = "<Enter the password>"
    $dnsLabelPrefix = "<Enter the prefix>"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    $vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
    New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
        -dnsLabelPrefix $dnsLabelPrefix
    ```
    Hier ist der Screenshot einer Beispiel-Bereitstellung:

    ![Azure-Portal, Cloud Shell, Vorlage bereitstellen](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-deploy-template.png)

    Im Screenshot werden diese Werte verwendet:

    * **$resourceGroupName**: myresourcegroup0710. 
    * **$location**: eastus2
    * **&lt;DeployName>**: mydeployment0710
    * **&lt;TemplateFile>**: azuredeploy.json
    * **Vorlagenparameter:**

        * **adminUsername**: JohnDole
        * **adminPassword**: Pass@word123
        * **dnsLabelPrefix**: myvm0710

10. Führen Sie den folgenden PowerShell-Befehl zum Auflisten des neu erstellen virtuellen Computers aus:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName <ResourceGroupName>
    ```

    Der Name des virtuellen Computers ist in der Vorlage als **SimpleWinVM** hartcodiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial entwickeln Sie eine Vorlage und stellen sie bereit, um einen virtuellen Computer, ein virtuelles Netzwerk und die abhängigen Ressourcen zu erstellen. Weitere Informationen zu Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](./resource-group-authoring-templates.md).
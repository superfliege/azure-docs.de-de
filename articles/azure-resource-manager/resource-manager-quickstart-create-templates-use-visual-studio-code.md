---
title: Verwenden von Visual Studio Code für die Erstellung einer Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: Verwenden Sie Visual Studio Code und die Azure Resource Manager-Tools-Erweiterung für Resource Manager-Vorlagen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 08/24/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 540aabc9164e43776d2166926430f4512dd23f49
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106048"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code

Erfahren Sie, wie Sie Azure Resource Manager-Vorlagen mit Visual Studio Code und der Erweiterung „Azure Resource Manager-Tools“ erstellen. Sie können Resource Manager-Vorlagen in Visual Studio Code auch ohne die Erweiterung erstellen, aber die Erweiterung verfügt über Optionen für die automatische Vervollständigung, die Ihnen die Entwicklung von Vorlagen vereinfachen. Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- [Visual Studio Code](https://code.visualstudio.com/).
- Die Erweiterung „Azure Resource Manager-Tools“. Zum Installieren gehen Sie wie folgt vor:

    1. Öffnen Sie Visual Studio Code.
    2. Drücken Sie **STRG+UMSCHALT+X**, um den Bereich "Erweiterungen" zu öffnen.
    3. Suchen Sie nach **Azure Resource Manager-Tools**, und wählen Sie die Option **Installieren** aus.
    4. Wählen Sie die Option **Erneut laden**, um die Installation der Erweiterung abzuschließen.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Anstatt eine Vorlage von Grund auf neu zu erstellen, können Sie auch eine Vorlage aus [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) öffnen. „Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen.

Die in dieser Schnellstartanleitung verwendete Vorlage heißt [Standardspeicherkonto erstellen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Die Vorlage definiert eine Azure Storage-Kontoressource.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um die Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Um zu lernen, wie man eine Vorlage mit Visual Studio Code bearbeitet, fügen Sie ein weiteres Element in den Ausgabebereich ein.

1. Fügen Sie aus Visual Studio Code eine weitere Ausgabe zur exportierten Vorlage hinzu:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Wenn Sie fertig sind, sieht der Ausgabeabschnitt folgendermaßen aus:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Wenn Sie den Code in Visual Studio Code kopiert und eingefügt haben, versuchen Sie, das Element **value** erneut einzugeben, um die IntelliSense-Funktion der Erweiterung „Resource Manager-Tools“ zu testen.

    ![Resource Manager-Vorlagen, Visual Studio Code, IntelliSense](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Wählen Sie **Datei**>**Speichern** aus, um die Datei zu speichern.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Es gibt viele Methoden zum Bereitstellen von Vorlagen.  In dieser Schnellstartanleitung verwenden Sie Azure Cloud Shell aus dem Azure-Portal. Cloud Shell unterstützt die Azure-Befehlszeilenschnittstelle (CLI) und Azure PowerShell. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
2. Wählen Sie oben rechts **Cloud Shell** aus, wie in der folgenden Abbildung dargestellt:

    ![Azure-Portal, Cloud Shell](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell.png)

    Cloud Shell wird am unteren Bildschirmrand geöffnet.

3. Klicken Sie in Cloud Shell oben links auf **PowerShell** oder **Bash**. Zur Verwendung der CLI müssen Sie eine Bash-Sitzung öffnen. Zur Ausführung von PowerShell müssen Sie eine PowerShell-Sitzung öffnen. Klicken Sie zum Wechseln auf den Pfeil nach unten, und wählen Sie dann den Interpreter aus. In der folgenden Abbildung ist der Wechsel von PowerShell zu Bash dargestellt:

    ![Azure-Portal, Cloud Shell, CLI](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    Bei einem Wechsel ist ein Neustart der Shell erforderlich.
4. Wählen Sie **Dateien hochladen/herunterladen** und dann **Hochladen** aus.

    ![Azure-Portal, Cloud Shell, Datei hochladen](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    Sie müssen die Vorlagendatei hochladen, bevor Sie sie über die Shell bereitstellen können.
5. Wählen Sie die Datei aus, die Sie zuvor in der Schnellstartanleitung gespeichert haben. Der Standardname lautet **azuredeploy.json**.
6. Führen Sie aus Cloud Shell den Befehl **ls** aus, um zu überprüfen, ob die Datei erfolgreich hochgeladen wurde. Sie können auch den Befehl **Cat** verwenden, um den Inhalt der Vorlage zu überprüfen. In der folgenden Abbildung ist die Ausführung des Befehls über Bash dargestellt.  In einer PowerShell-Sitzung verwenden Sie die gleichen Befehle.

    ![Azure-Portal, Cloud Shell, Datei auflisten](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
7. Führen Sie in Cloud Shell die folgenden Befehle aus. Klicken Sie auf die Registerkarte, um den PowerShell-Code oder den CLI-Code anzuzeigen.

    # <a name="clitabcli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)
    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```powershell
    New-AzureRmResourceGroup -Name <ResourceGroupName> -Location <AzureLocation>

    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroupName> -TemplateFile <TemplateFileName>
    ```
    
    ---

    Der folgende Screenshot zeigt eine CLI-Beispielausführung:

    ![Azure-Portal, Cloud Shell, Vorlage bereitstellen](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    Im Screenshot werden diese Werte verwendet:

    - **&lt;ResourceGroupName>**: myresourcegroup0709. Es gibt zwei Darstellungen des Parameters.  Stellen Sie sicher, dass der gleiche Wert verwendet wird.
    - **&lt;AzureLocation>**: eastus2
    - **&lt;DeployName>**: mydeployment0709
    - **&lt;TemplateFile>**: azuredeploy.json

    In der Screenshotausgabe lautet der Name des Speicherkontos *3tqebj3slyfyestandardsa*. 

7. Führen Sie den folgenden CLI- oder PowerShell-Befehl zum Auflisten des neu erstellen Speicherkontos aus:

    # <a name="clitabcli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)
    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```powershell
    Get-AzureRmStorageAccount -ResourceGroupName <ResourceGroupName> -Name <StorageAccountName>
    ```
    
    ---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial geht es in erster Linie um die Bearbeitung einer vorhandenen Azure-Schnellstartvorlage mithilfe von Visual Studio Code. Darüber hinaus haben Sie gelernt, wie Sie die Vorlage über Azure Cloud Shell mithilfe der CLI oder PowerShell bereitstellen. Die Azure-Schnellstartvorlagen decken unter Umständen nicht alle Ihre Anforderungen ab. Im nächsten Tutorial wird gezeigt, wie Sie die Informationen in der Vorlagenreferenz suchen, um ein verschlüsseltes Azure Storage-Konto erstellen zu können.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer Azure Resource Manager-Vorlage für die Bereitstellung eines verschlüsselten Speicherkontos](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

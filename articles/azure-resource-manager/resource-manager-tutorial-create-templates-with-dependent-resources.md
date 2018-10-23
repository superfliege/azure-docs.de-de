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
ms.date: 10/09/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 50f1c81f08787181de2fe3a9f6fb97a96a2bd882
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114311"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen

Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, um mehrere Ressourcen bereitzustellen.  Nach der Erstellung der Vorlage stellen Sie sie mithilfe von Cloud Shell über das Azure-Portal bereit.

In diesem Tutorial erstellen Sie ein Speicherkonto, eine VM, ein virtuelles Netzwerk und einige andere abhängigen Ressourcen. Einige der Ressourcen können erst bereitgestellt werden, wenn eine andere Ressource vorhanden ist. Sie können den virtuellen Computer beispielsweise erst erstellen, wenn sein Speicherkonto und seine Netzwerkschnittstelle vorhanden sind. Diese Beziehung definieren Sie, indem Sie eine Ressource von den anderen Ressourcen abhängig machen. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Weitere Informationen finden Sie unter [Definieren der Reihenfolge für die Bereitstellung von Ressourcen in Azure Resource Manager-Vorlagen](./resource-group-define-dependencies.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten der Key Vault-Instanz
> * Öffnen einer Schnellstartvorlage
> * Untersuchen der Vorlage
> * Bearbeiten der Parameterdatei
> * Bereitstellen der Vorlage

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“.  Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="prepare-key-vault"></a>Vorbereiten von Key Vault

Zum Verhindern von Kennwort-Spray-Angriffen wird empfohlen, ein automatisch generiertes Kennwort für das Administratorkonto des virtuellen Computers und Key Vault zum Speichern des Kennworts zu verwenden. Mit dem folgenden Verfahren werden eine Key Vault-Instanz und ein Geheimnis zum Speichern des Kennworts erstellt. Außerdem werden erforderlichen Berechtigungen für den Zugriff auf das in Key Vault gespeicherte Geheimnis durch die Vorlagenbereitstellung konfiguriert. Wenn sich die Key Vault-Instanz in einem anderen Azure-Abonnement befindet, sind zusätzliche Zugriffsrichtlinien erforderlich. Weitere Details finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](./resource-manager-keyvault-parameter.md).

1. Melden Sie sich bei [Azure Cloud Shell](https://shell.azure.com) an.
2. Wechseln Sie über die Option links oben zu Ihrer bevorzugten Umgebung, entweder **PowerShell** oder **Bash**.
3. Führen Sie den folgenden Azure PowerShell- oder Azure CLI-Befehl aus:  

    ```azurecli-interactive
    keyVaultName='<your-unique-vault-name>'
    resourceGroupName='<your-resource-group-name>'
    location='Central US'
    userPrincipalName='<your-email-address-associated-with-your-subscription>'
    
    # Create a resource group
    az group create --name $resourceGroupName --location $location
    
    # Create a Key Vault
    keyVault=$(az keyvault create \
      --name $keyVaultName \
      --resource-group $resourceGroupName \
      --location $location \
      --enabled-for-template-deployment true)
    keyVaultId=$(echo $keyVault | jq -r '.id')
    az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

    # Create a secret
    password=$(openssl rand -base64 32)
    az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: $keyVaultId."
    ```

    ```azurepowershell-interactive
    $keyVaultName = "<your-unique-vault-name>"
    $resourceGroupName="<your-resource-group-name>"
    $location='Central US'
    $userPrincipalName="<your-email-address-associated-with-your-subscription>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
    # Create a Key Vault
    $keyVault = New-AzureRmKeyVault `
      -VaultName $keyVaultName `
      -resourceGroupName $resourceGroupName `
      -Location $location `
      -EnabledForTemplateDeployment
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
      
    # Create a secret
    $password = openssl rand -base64 32
    
    $secretValue = ConvertTo-SecureString $password -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretValue
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: " $keyVault.ResourceID
    ```
4. Notieren Sie die Ausgabewerte. Sie werden später im Tutorial benötigt.

> [!NOTE]
> Jeder Azure-Dienst hat bestimmte Kennwortanforderungen. Die Anforderungen des virtuellen Azure-Computers finden Sie beispielsweise unter „Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?“

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.
5. Wiederholen Sie die Schritte 1 bis 4, um **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json** öffnen, und speichern Sie die Datei anschließend als **azuredeploy.parameters.json**.

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

## <a name="edit-the-parameters-file"></a>Bearbeiten der Parameterdatei

Die Vorlagendatei muss nicht geändert werden. Sie müssen jedoch die Parameterdatei ändern, um das Administratorkennwort aus Key Vault abzurufen.

1. Öffnen Sie **azuredeploy.parameters.json** in Visual Studio Code, falls die Datei noch nicht geöffnet ist.
2. Aktualisieren Sie den Parameter **adminPassword** wie folgt:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```
    Ersetzen Sie die **ID** durch die Ressourcen-ID Ihrer in der letzten Prozedur erstellen Key Vault-Instanz. Diese ist in den Ausgaben enthalten. 

    ![Integrieren von Key Vault und Resource Manager-Vorlage: VM-Bereitstellung – Parameterdatei](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Geben Sie Werte für Folgendes an:

    - **adminUsername**: Geben Sie das Administratorkonto des virtuellen Computers an.
    - **dnsLabelPrefix**: Geben Sie das Präfix für die DNS-Bezeichnung an.
4. Speichern Sie die Änderungen.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Es gibt viele Methoden zum Bereitstellen von Vorlagen.  In diesem Tutorial verwenden Sie Cloud Shell aus dem Azure-Portal.

1. Melden Sie sich bei [Cloud Shell](https://shell.azure.com) an. Sie können sich auch beim [Azure-Portal](https://portal.azure.com) anmelden und dann oben rechts **Cloud Shell** auswählen, wie in der folgenden Abbildung dargestellt:

    ![Azure-Portal, Cloud Shell](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
2. Wählen Sie links oben in der Cloudshell die Option **PowerShell** aus, und wählen Sie dann **Bestätigen**.  In diesem Tutorial können Sie PowerShell verwenden.
3. Klicken Sie in Cloud Shell auf **Datei hochladen**:

    ![Azure-Portal, Cloud Shell, Datei hochladen](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Wählen Sie die Dateien aus, die Sie zuvor im Tutorial gespeichert haben. Die Standardnamen sind **azuredeploy.json** und **azuredeploy.paraemters.json**.  Falls Sie Dateien mit den gleichen Dateinamen haben, werden die alten Dateien ohne Benachrichtigung überschrieben.
5. Führen Sie in Cloud Shell den folgenden Befehl aus, um zu überprüfen, ob die Datei erfolgreich hochgeladen wurde. 

    ```bash
    ls
    ```

    ![Azure-Portal, Cloud Shell, Datei auflisten](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    Der Dateiname im Screenshot lautet „azuredeploy.json“.

6. Führen Sie in Cloud Shell den folgenden Befehl aus, um den Inhalt der JSON-Datei zu überprüfen:

    ```bash
    cat azuredeploy.json
    cat azuredeploy.parameters.json
    ```
7. Führen Sie in Cloud Shell die folgenden PowerShell-Befehle aus. Das Beispielskript verwendet die für Key Vault erstellte Ressourcengruppe ebenfalls. Die Verwendung der gleichen Ressourcengruppe vereinfacht die Bereinigung der Ressourcen.

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $deploymentName = "<Enter a deployment name>"

    New-AzureRmResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json `
        -TemplateparameterFile azuredeploy.parameters.json
    ```
8. Führen Sie den folgenden PowerShell-Befehl zum Auflisten des neu erstellen virtuellen Computers aus:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    Der Name des virtuellen Computers ist in der Vorlage als **SimpleWinVM** hartcodiert.

9. Melden Sie sich am virtuellen Computer an, um die Anmeldeinformationen des Administrators zu testen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial entwickeln Sie eine Vorlage und stellen sie bereit, um einen virtuellen Computer, ein virtuelles Netzwerk und die abhängigen Ressourcen zu erstellen. Informationen zum Bereitstellen von Azure-Ressourcen auf der Grundlage von Bedingungen finden Sie hier:


> [!div class="nextstepaction"]
> [Verwenden von Bedingungen](./resource-manager-tutorial-use-conditions.md)


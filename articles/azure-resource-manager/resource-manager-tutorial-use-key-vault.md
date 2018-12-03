---
title: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie bei der Resource Manager-Vorlagenbereitstellung sichere Parameterwerte mithilfe von Azure Key Vault übergeben.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0bcff6e0cec234f17b0aaab9828602eb4a194d85
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334209"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung

Hier erfahren Sie, wie Sie bei der Resource Manager-Bereitstellung geheime Werte aus Azure Key Vault abrufen und als Parameter übergeben. Der Wert wird nie verfügbar gemacht, da lediglich die Key Vault-ID als Verweis verwendet wird. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](./resource-manager-keyvault-parameter.md).

Im Tutorial [Festlegen der Reihenfolge für die Ressourcenbereitstellung](./resource-manager-tutorial-create-templates-with-dependent-resources.md) erstellen Sie einen virtuellen Computer, ein virtuelles Netzwerk und einige andere abhängige Ressourcen. In diesem Tutorial passen Sie die Vorlage an, um das Administratorkennwort für den virtuellen Computer aus Azure Key Vault abzurufen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten der Key Vault-Instanz
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Parameterdatei
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der [Erweiterung „Azure Resource Manager-Tools“](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-the-key-vault"></a>Vorbereiten der Key Vault-Instanz

In diesem Abschnitt erstellen Sie mithilfe einer Resource Manager-Vorlage eine Key Vault-Instanz und ein Geheimnis. Diese Vorlage umfasst Folgendes:

* Erstellen einer Key Vault-Instanz mit aktivierter `enabledForTemplateDeployment`-Eigenschaft. Diese Eigenschaft muss auf „true“ festgelegt sein, damit der Bereitstellungsprozess auf die in dieser Key Vault-Instanz definierten Geheimnisse zugreifen kann.
* Sie fügt der Key Vault-Instanz ein Geheimnis hinzu.  Das Geheimnis enthält das Administratorkennwort für den virtuellen Computer.

Wenn Sie (als der Benutzer, der die VM-Vorlage bereitstellt) nicht der Besitzer oder Mitwirkende für die Key Vault-Instanz sind, muss der Besitzer oder Mitwirkende für die Key Vault-Instanz Ihnen Zugriff auf die Berechtigung „Microsoft.KeyVault/vaults/deploy/action“ für die Key Vault-Instanz gewähren. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](./resource-manager-keyvault-parameter.md).

Ihre Azure AD-Benutzerobjekt-ID wird von der Vorlage zum Konfigurieren von Berechtigungen benötigt. Die folgende Prozedur ruft die Objekt-ID (GUID) ab und generiert das Administratorkennwort. Zur Verhinderung von Kennwort-Spray-Angriffen empfiehlt es sich, generierte Kennwörter zu verwenden.

1. Führen Sie den folgenden Azure PowerShell- oder Azure CLI-Befehl aus:  

    ```azurecli-interactive
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    openssl rand -base64 32
    ```
    ```azurepowershell-interactive
    $upn = Read-Host -Prompt "Input your user principal name (email address) used to sign in to Azure"
    (Get-AzureADUser -ObjectId $upn).ObjectId
    openssl rand -base64 32
    ```
2. Notieren Sie sich sowohl die Objekt-ID als auch das generierte Kennwort. Diese werden später benötigt.
3. Vergewissern Sie sich, dass das generierte Kennwort die VM-Kennwortanforderungen erfüllt. Jeder Azure-Dienst hat bestimmte Kennwortanforderungen. Informationen zu den Kennwortanforderungen für virtuelle Computer finden Sie unter [Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

So erstellen Sie eine Key Vault-Instanz:

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Die Vorlage erstellt eine Key Vault-Instanz und ein Key Vault-Geheimnis.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="./media/resource-manager-tutorial-use-key-vault/deploy-to-azure.png" /></a>

2. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.  Klicken Sie nach Angabe der Werte nicht auf **Kaufen**.

    ![Resource Manager-Vorlage: Key Vault-Integration – Bereitstellen (Portal)](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)

    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Weisen Sie einen eindeutigen Namen zu. Notieren Sie sich diesen Namen. Die gleiche Ressourcengruppe wird in der nächsten Sitzung zum Bereitstellen des virtuellen Computers verwendet. Wenn Sie sowohl die Key Vault-Instanz als auch den virtuellen Computer in der gleichen Ressourcengruppe platzieren, lässt sich die Ressource am Ende des Tutorials leichter bereinigen.
    * **Standort**: Wählen Sie einen Standort aus.  Standardstandort: **USA, Mitte**.
    * **Schlüsseltresorname**: Weisen Sie einen eindeutigen Namen zu. 
    * **Mandanten-ID**: Die Vorlagenfunktion ruft automatisch Ihre Mandanten-ID ab.  Lassen Sie den Standardwert unverändert.
    * **Ad User Id** (AD-Benutzer-ID): Geben Sie Ihre Azure AD-Benutzerobjekt-ID ein, die Sie in der vorherigen Prozedur abgerufen haben.
    * **Geheimnisname**: Der Standardname lautet **vmAdminPassword**. Wenn Sie den Geheimnisnamen hier ändern, müssen Sie ihn beim Bereitstellen des virtuellen Computers aktualisieren.
    * **Geheimniswert**: Geben Sie Ihr Geheimnis ein.  Das Geheimnis ist das Kennwort für die Anmeldung bei dem virtuellen Computer. Es empfiehlt sich, das generierte Kennwort zu verwenden, das Sie in der vorherigen Prozedur erstellt haben.
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.
3. Klicken Sie im oberen Bereich auf **Parameter bearbeiten**, und sehen Sie sich die Vorlage an.
4. Navigieren Sie zu Zeile 28 der JSON-Vorlagendatei. Hierbei handelt es sich um die Definition der Key Vault-Ressource.
5. Navigieren Sie zu Zeile 35:

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment` ist eine Key Vault-Eigenschaft. Diese Eigenschaft muss auf „true“ festgelegt sein, um bei der Bereitstellung die Geheimnisse aus dieser Key Vault-Instanz abrufen zu können.
6. Navigieren Sie zu Zeile 89. Hierbei handelt es sich um die Definition des Key Vault-Geheimnisses.
7. Klicken Sie unten auf der Seite auf **Verwerfen**. Sie haben keine Änderungen vorgenommen.
8. Vergewissern Sie sich anhand des vorherigen Screenshots, dass Sie alle Werte angegeben haben, und klicken Sie anschließend unten auf der Seite auf **Kaufen**.
9. Klicken Sie im oberen Bereich der Seite auf das Glockensymbol (Benachrichtigung), um den Bereich **Benachrichtigungen** zu öffnen. Warten Sie, bis die Ressource erfolgreich bereitgestellt wurde.
10. Klicken Sie im Bereich **Benachrichtigungen** auf **Zu Ressourcengruppe wechseln**. 
11. Klicken Sie auf den Schlüsseltresornamen, um die Key Vault-Instanz zu öffnen.
12. Klicken Sie im linken Bereich auf **Zugriffsrichtlinien**. Der Name „Active Directory“ sollte aufgeführt werden. Falls nicht, sind Sie nicht berechtigt, auf den Schlüsseltresor zuzugreifen.
13. Wählen Sie **Klicken Sie, um erweiterte Zugriffsrichtlinien anzuzeigen.** aus. Wie Sie sehen, ist **Zugriff auf Azure Resource Manager für Vorlagenbereitstellung aktivieren** aktiviert. Hierbei handelt es sich um eine weitere Bedingung, die erfüllt sein muss, damit die Key Vault-Integration funktioniert.

    ![Resource Manager-Vorlage: Key Vault-Integration – Zugriffsrichtlinien](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
14. Klicken Sie im linken Bereich auf **Eigenschaften**.
15. Kopieren Sie die **Ressourcen-ID**. Diese ID wird beim Bereitstellen des virtuellen Computers benötigt.  Die Ressourcen-ID hat das folgende Format:

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen. Hierbei handelt es sich um das gleiche Szenario, das auch im [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md) verwendet wird.
4. Es gibt fünf Ressourcen, die von der Vorlage definiert werden:

    * `Microsoft.Storage/storageAccounts`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
5. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.
6. Wiederholen Sie die Schritte 1 bis 4, um die folgende URL zu öffnen, und speichern Sie die Datei anschließend als **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Bearbeiten der Parameterdatei

Die Vorlagendatei muss nicht geändert werden.

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
    Ersetzen Sie die **ID** durch die Ressourcen-ID Ihrer in der letzten Prozedur erstellen Key Vault-Instanz.  

    ![Integrieren von Key Vault und Resource Manager-Vorlage: VM-Bereitstellung – Parameterdatei](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Geben Sie Werte für Folgendes an:

    * **adminUsername**: Geben Sie das Administratorkonto des virtuellen Computers an.
    * **dnsLabelPrefix**: Geben Sie das Präfix für die DNS-Bezeichnung an.
4. Speichern Sie die Änderungen.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage gemäß der Anleitung unter [Bereitstellen der Vorlage](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bereit. Laden Sie sowohl **azuredeploy.json** als auch **azuredeploy.parameters.json** in Cloud Shell hoch, und stellen Sie die Vorlage anschließend mithilfe des folgenden PowerShell-Skripts bereit:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name of the Key Vault"
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

Verwenden Sie beim Bereitstellen der Vorlage die gleiche Ressourcengruppe wie für die Key Vault-Instanz. Das erleichtert später die Bereinigung der Ressourcen. Sie müssen dann nicht zwei Ressourcengruppen löschen, sondern nur eine.

## <a name="valid-the-deployment"></a>Überprüfen der Bereitstellung

Testen Sie nach erfolgreicher Bereitstellung des virtuellen Computers die Anmeldung unter Verwendung des in der Key Vault-Instanz gespeicherten Kennworts.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Ressourcengruppen**/**Name Ihrer Ressourcengruppe>**/**simpleWinVM**.
3. Klicken Sie im oberen Bereich auf **Verbinden**.
4. Klicken Sie auf **RDP-Datei herunterladen**, und melden Sie sich gemäß den Anweisungen unter Verwendung des in der Key Vault-Instanz gespeicherten Kennworts bei dem virtuellen Computer an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Geheimnis aus Azure Key Vault abgerufen und im Rahmen Ihrer Vorlagenbereitstellung verwendet.  Informationen zum Erstellen verknüpfter Vorlagen finden Sie unter:

> [!div class="nextstepaction"]
> [Erstellen verknüpfter Vorlagen](./resource-manager-tutorial-create-linked-templates.md)

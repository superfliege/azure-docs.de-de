---
title: Erstellen und Veröffentlichen einer verwalteten Azure-Dienstkataloganwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: a2e6e78268f97136533b4f72ce28373642b6c394
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801266"
---
# <a name="create-and-publish-a-managed-application-definition"></a>Erstellen und Veröffentlichen einer Definition für die verwaltete Anwendung

Sie können [verwaltete Azure-Anwendungen](overview.md) erstellen und veröffentlichen, die für Mitglieder Ihrer Organisation vorgesehen sind. Eine IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Standards der Organisation erfüllen. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Um eine verwaltete Anwendung für den Dienstkatalog zu veröffentlichen, gehen Sie folgendermaßen vor:

* Erstellen Sie eine Vorlage, die definiert, welche Ressourcen mit der verwalteten Anwendung bereitgestellt werden.
* Definieren Sie die Elemente der Benutzeroberfläche für das Portal, wenn Sie die verwaltete Anwendung bereitstellen.
* Erstellen Sie ein ZIP-Paket, das die erforderlichen Vorlagendateien enthält.
* Entscheiden Sie, welche Benutzer, Gruppen oder Anwendungen Zugriff auf die Ressourcengruppe im Benutzerabonnement benötigen.
* Erstellen Sie die verwaltete Anwendungsdefinition, die auf das ZIP-Paket verweist und Zugriff auf die Identität anfordert.

Für diesen Artikel verfügt die verwaltete Anwendung nur über ein Speicherkonto. Es soll die Schritte zum Veröffentlichen einer verwalteten Anwendung veranschaulichen. Vollständige Beispiele finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).

Für die PowerShell-Beispiele in diesem Artikel wird mindestens die Azure PowerShell-Version 6.2 benötigt. [Aktualisieren Sie Ihre Version](/powershell/azure/install-azurerm-ps), sofern erforderlich.

## <a name="create-the-resource-template"></a>Erstellen der Ressourcenvorlage

Jede Definition einer verwalteten Anwendung enthält eine Datei namens **mainTemplate.json**. Darin definieren Sie die bereitzustellenden Azure-Ressourcen. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen.

Erstellen Sie eine Datei mit dem Namen **mainTemplate.json**. Bei dem Namen wird die Groß-/Kleinschreibung unterschieden.

Fügen Sie Ihrer Datei den folgenden JSON-Code hinzu. Er definiert die Parameter für das Erstellen eines Speicherkontos und legt die Eigenschaften des Speicherkontos fest.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

Speichern Sie die Datei „mainTemplate.json“.

## <a name="create-the-user-interface-definition"></a>Erstellen der Benutzeroberflächendefinition

Das Azure-Portal verwendet die **createUiDefinition.json**-Datei zum Generieren der Benutzeroberfläche für Benutzer, die die verwaltete Anwendung erstellen. Sie legen fest, wie Benutzer die Eingaben für die einzelnen Parameter bereitstellen. Sie können Optionen wie eine Dropdownliste, ein Textfeld, ein Kennwortfeld und andere Eingabetools verwenden. Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).

Erstellen Sie eine Datei mit dem Namen **createUiDefinition.json**. Bei dem Namen wird die Groß-/Kleinschreibung unterschieden.

Fügen Sie der Datei den folgenden JSON-Code hinzu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Speichern Sie die Datei „createUIDefinition.json“.

## <a name="package-the-files"></a>Erstellen eines Pakets aus den Dateien

Fügen Sie die beiden Dateien einer ZIP-Datei mit dem Namen „app.zip“ hinzu. Die zwei Dateien müssen sich auf der Stammebene der ZIP-Datei befinden. Wenn Sie sie in einem Ordner speichern, erhalten Sie beim Erstellen der Definition der verwalteten Anwendung eine Fehlermeldung mit dem Hinweis, dass die erforderlichen Dateien nicht vorhanden sind. 

Laden Sie das Paket an einen zugänglichen Speicherort hoch, wo es verwendet werden kann. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Erstellen einer Azure Active Directory-Benutzergruppe oder -Anwendung

Der zweite Schritt besteht darin, eine Benutzergruppe oder Anwendung für das Verwalten der Ressourcen für den Kunden zu erstellen. Diese Benutzergruppe oder Anwendung verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Es kann sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle (Role-Based Access Control, RBAC) wie „Besitzer“ oder „Mitwirkender“ handeln. Sie können einem einzelnen Benutzer auch Berechtigungen zum Verwalten der Ressourcen zuweisen, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Weitere Informationen zum Erstellen einer neuen Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Sie benötigen zum Verwalten der Ressourcen die Objekt-ID der Benutzergruppe. 

```powershell
$groupID=(Get-AzureRmADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel verwenden Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Falls Sie noch nicht über eine Ressourcengruppe zum Speichern der Definition Ihrer verwalteten Anwendung verfügen, erstellen Sie jetzt eine:

```powershell
New-AzureRmResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung.

```powershell
$blob = Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzureRmManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

### <a name="make-sure-users-can-see-your-definition"></a>Sicherstellen, dass Benutzer Ihre Definition sehen können

Sie haben zwar Zugriff auf die Definition der verwalteten Anwendung, möchten aber sicherstellen, dass andere Benutzer in Ihrer Organisation darauf zugreifen können. Erteilen Sie ihnen mindestens die Leserrolle für die Definition. Diese Zugriffsebene haben sie möglicherweise vom Abonnement oder von der Ressourcengruppe geerbt. Informationen zum Überprüfen, wer Zugriff auf die Definition hat, sowie zum Hinzufügen von Benutzern oder Gruppen finden Sie unter [Manage access using RBAC and the Azure portal](../role-based-access-control/role-assignments-portal.md) (Verwalten des Zugriffs mithilfe von RBAC und Azure-Portal).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Veröffentlichen der verwalteten Anwendung im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](publish-marketplace-app.md).
* Informationen zum Bereitstellen einer Instanz der verwalteten Anwendung finden Sie unter [Deploy service catalog app through Azure portal](deploy-service-catalog-quickstart.md) (Bereitstellen einer Dienstkatalog-App über das Azure-Portal).
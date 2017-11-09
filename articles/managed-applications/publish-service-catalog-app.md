---
title: "Erstellen und Veröffentlichen einer verwalteten Azure-Dienstkataloganwendung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: fd2c60cbc237f6d302616723c745563a3e1afecb
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Veröffentlichen einer verwalteten Anwendung für die interne Nutzung

Sie können [verwaltete Azure-Anwendungen](overview.md) erstellen und veröffentlichen, die für Mitglieder Ihrer Organisation vorgesehen sind. Die IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Konformität mit Ihren Unternehmensstandards gewährleisten. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Um eine verwaltete Anwendung für den Dienstkatalog zu veröffentlichen, gehen Sie folgendermaßen vor:

* Erstellen Sie eine Vorlage, die definiert, welche Ressourcen mit der verwalteten Anwendung bereitgestellt werden.
* Definieren Sie die Elemente der Benutzeroberfläche für das Portal, wenn Sie die verwaltete Anwendung bereitstellen.
* Erstellen Sie ein ZIP-Paket, das die erforderlichen Vorlagendateien enthält.
* Entscheiden Sie, welche Benutzer, Gruppen oder Anwendungen Zugriff auf die Ressourcengruppe im Benutzerabonnement benötigen.
* Erstellen Sie die verwaltete Anwendungsdefinition, die auf das ZIP-Paket verweist und Zugriff auf die Identität anfordert.

Für diesen Artikel enthält die verwaltete Anwendung nur ein Speicherkonto. Es soll die Schritte zum Veröffentlichen einer verwalteten Anwendung veranschaulichen. Vollständige Beispiele finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).

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
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString('storage'))]"
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

Fügen Sie die beiden Dateien einer ZIP-Datei mit dem Namen „app.zip“ hinzu. Die zwei Dateien müssen sich auf der Stammebene der ZIP-Datei befinden. Wenn Sie sie in einem Ordner speichern, erhalten Sie, wenn Sie die Definition der verwalteten Anwendung erstellen, eine Fehlermeldung, dass die erforderlichen Dateien nicht vorhanden sind. 

Laden Sie das Paket an einen zugänglichen Speicherort hoch, wo es verwendet werden kann. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Erstellen einer Azure Active Directory-Benutzergruppe oder -Anwendung

Der zweite Schritt besteht darin, eine Benutzergruppe oder Anwendung für das Verwalten der Ressourcen für den Kunden zu erstellen. Diese Benutzergruppe oder Anwendung verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Es kann sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle (Role-Based Access Control, RBAC) wie „Besitzer“ oder „Mitwirkender“ handeln. Sie können einem einzelnen Benutzer auch Berechtigungen zum Verwalten der Ressourcen zuweisen, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Weitere Informationen zum Erstellen einer neuen Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Sie benötigen zum Verwalten der Ressourcen die Objekt-ID der Benutzergruppe. 

![Abrufen der Gruppen-ID](./media/publish-service-catalog-app/get-group-id.png)

### <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel verwenden Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Wenn Sie noch über keine Ressourcengruppe für das Speichern der Definition Ihrer verwalteten Anwendung verfügen, erstellen Sie sie jetzt:

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
  -Authorization "<group-id>:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="create-the-managed-application-by-using-the-portal"></a>Erstellen der verwalteten Anwendung mit dem Portal

Jetzt verwenden wir das Portal zum Bereitstellen der verwalteten Anwendung. Sie sehen die Benutzeroberfläche, die Sie im Paket erstellt haben.

1. Öffnen Sie das Azure-Portal. Wählen Sie **+ Neu**, und suchen Sie nach **Dienstkatalog**.

   ![Suchen nach „Dienstkatalog“](./media/publish-service-catalog-app/select-new.png)

1. Wählen Sie **Verwaltete Dienstkataloganwendung**.

   ![Wählen des Dienstkatalogs](./media/publish-service-catalog-app/select-service-catalog.png)

1. Klicken Sie auf **Erstellen**.

   ![Klicken auf „Erstellen“](./media/publish-service-catalog-app/select-create.png)

1. Suchen Sie die verwaltete Anwendung, die Sie erstellen möchten, in der Liste der verfügbaren Lösungen, und wählen Sie sie aus. Klicken Sie auf **Erstellen**.

   ![Suchen der verwalteten Anwendung](./media/publish-service-catalog-app/find-application.png)

1. Geben Sie grundlegende Informationen an, die für die verwaltete Anwendung erforderlich sind. Geben Sie das Abonnement und eine neue Ressourcengruppe an, die die verwaltete Anwendung enthalten sollen. Wählen Sie als Standort **USA, Westen-Mitte** aus. Wählen Sie dann die Option **OK**.

   ![Bereitstellen der Parameter für verwaltete Anwendungen](./media/publish-service-catalog-app/provide-basics.png)

1. Geben Sie Werte an, die für die Ressourcen in der verwalteten Anwendung spezifisch sind. Wählen Sie dann die Option **OK**.

   ![Bereitstellen von Ressourcenparametern](./media/publish-service-catalog-app/provide-resource-values.png)

1. Die Vorlage überprüft die bereitgestellten Werte. Wählen Sie bei erfolgreicher Validierung **OK** aus, um die Bereitstellung zu starten.

   ![Überprüfen der verwalteten Anwendung](./media/publish-service-catalog-app/validate.png)

Nachdem die Bereitstellung abgeschlossen ist, ist die verwaltete Anwendung in einer Ressourcengruppe mit dem Namen applicationGroup vorhanden. Das Speicherkonto ist in einer Ressourcengruppe mit dem Namen applicationGroup plus einem Hashzeichenfolgenwert vorhanden.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](overview.md).
* Beispielprojekte finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](publish-marketplace-app.md).
* Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
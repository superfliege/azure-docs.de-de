---
title: Erstellen einer Azure-Blaupause mit der REST-API
description: Erstellen, Definieren und Bereitstellen von Artefakten mithilfe von Azure Blueprint.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/07/2018
ms.topic: quickstart
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: b600eeff0482944a8b9b18ad39c23ee6ea4700ce
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283545"
---
# <a name="define-and-assign-an-azure-blueprint-with-rest-api"></a>Definieren und Zuweisen einer Azure-Blaupause mit der REST-API

Wenn Sie mit der Erstellung und Zuweisung von Blaupausen vertraut sind, können Sie allgemeine Muster definieren, um wiederverwendbare und schnell bereitstellbare Konfigurationen zu entwickeln, die auf Resource Manager-Vorlagen, Richtlinien, Sicherheit usw. basieren. In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Blueprint einige allgemeine Aufgaben im Zusammenhang mit der organisationsweiten Erstellung, Veröffentlichung und Zuweisung einer Blaupause ausführen:

> [!div class="checklist"]
> - Erstellen einer neuen Blaupause und Hinzufügen verschiedener unterstützter Artefakte
> - Ändern einer vorhandenen Blaupause, die sich noch im **Entwurf** befindet
> - Markieren einer Blaupause als bereit für die Zuweisung mit **Veröffentlicht**
> - Zuweisen einer Blaupause zu einem vorhandenen Abonnement
> - Überprüfen von Status und Fortschritt einer zugewiesenen Blaupause
> - Entfernen einer Blaupause, die einem Abonnement zugewiesen wurde

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="getting-started-with-rest-api"></a>Erste Schritte mit der REST-API

Sollten Sie noch nicht mit der REST-API vertraut sein, finden Sie unter [Azure REST-API-Referenz](/rest/api/azure/) eine allgemeine Übersicht über die REST-API (insbesondere über den Anforderungs-URI und den Anforderungstext). In diesem Artikel dienen diese Konzepte als Grundlage für die Anweisungen zur Verwendung von Azure Blueprint. Aus diesem Grund werden entsprechende Grundkenntnisse vorausgesetzt. Mit Tools wie [ARMClient](https://github.com/projectkudu/ARMClient) kann die Autorisierung automatisch durchgeführt werden. Diese Tools werden für Anfänger empfohlen.

Die Spezifikationen zur Blaupause finden Sie unter [Azure Blueprints REST API](/rest/api/blueprints/) (REST-API für Azure-Blaupausen).

### <a name="rest-api-and-powershell"></a>REST-API und PowerShell

Wenn Sie noch über kein Tool für REST-API-Aufrufe verfügen, können Sie PowerShell für diese Anweisungen verwenden. Im Folgenden finden Sie ein Beispielheader für die Authentifizierung bei Azure. Generieren Sie einen Authentifizierungsheader, der manchmal als **Bearertoken** bezeichnet wird, und geben Sie den REST-API-URI für die Verbindung mit Parametern oder einen **Anforderungstext** an:

```powershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Invoke the REST API
$restUri = 'https://management.azure.com/subscriptions/{subscriptionId}?api-version=2016-06-01'
$response = Invoke-RestMethod -Uri $restUri -Method Get -Headers $authHeader
```

Ersetzen Sie `{subscriptionId}` in der Variable **$restUri** oben, um Informationen zu Ihrem Abonnement abzurufen. Die Variable „$response“ enthält das Ergebnis des Cmdlets `Invoke-RestMethod`, das mit Cmdlets wie z.B. [ConvertFrom-Json](/powershell/module/microsoft.powershell.utility/convertfrom-json) analysiert werden kann. Wenn im REST-API-Dienstendpunkt ein **Anforderungstext** erwartet wird, geben Sie eine JSON-formatierte Variable für den Parameter `-Body` von `Invoke-RestMethod` an.

## <a name="create-a-blueprint"></a>Erstellen einer Blaupause

Im ersten Schritt beim Definieren eines Standardmusters für die Konformität wird eine Blaupause aus den verfügbaren Ressourcen erstellt. Wir erstellen eine Blaupause namens „MyBlueprint“, um Rollen- und Richtlinienzuweisungen für das Abonnement zu konfigurieren. Anschließend fügen wir eine Ressourcengruppe, eine Resource Manager-Vorlage und eine Rollenzuweisung für die Ressourcengruppe hinzu.

> [!NOTE]
> Bei Verwendung der REST-API wird zuerst das _blueprint_-Objekt erstellt. Für jedes hinzugefügte _Artefakt_, das über Parameter verfügt, müssen die Parameter vorab in der anfänglichen _Blaupause_ definiert werden.

In jedem REST-API-URI gibt es Variablen, die Sie durch Ihre eigenen Werte ersetzen müssen:

- Ersetzen Sie `{YourMG}` durch den Namen Ihrer Verwaltungsgruppe.
- Ersetzen Sie `{subscriptionId}` durch Ihre Abonnement-ID.

1. Erstellen Sie das anfängliche _blueprint_-Objekt. Der **Anforderungstext** enthält Eigenschaften der Blaupause, alle zu erstellenden Ressourcengruppen und alle Parameter auf Blaupausenebene. Die Parameter werden im Rahmen der Zuweisung festgelegt und von den in späteren Schritten hinzugefügten Artefakten verwendet.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "properties": {
             "description": "This blueprint sets tag policy and role assignment on the subscription, creates a ResourceGroup, and deploys a resource template and role assignment to that ResourceGroup.",
             "targetScope": "subscription",
             "parameters": {
                 "storageAccountType": {
                     "type": "string",
                     "metadata": {
                         "displayName": "storage account type.",
                         "description": null
                     }
                 },
                 "tagName": {
                     "type": "string",
                     "metadata": {
                         "displayName": "The name of the tag to provide the policy assignment.",
                         "description": null
                     }
                 },
                 "tagValue": {
                     "type": "string",
                     "metadata": {
                         "displayName": "The value of the tag to provide the policy assignment.",
                         "description": null
                     }
                 },
                 "contributors": {
                     "type": "array",
                     "metadata": {
                         "description": "List of AAD object IDs that is assigned Contributor role at the subscription"
                     }
                 },
                 "owners": {
                     "type": "array",
                     "metadata": {
                         "description": "List of AAD object IDs that is assigned Owner role at the resource group"
                     }
                 }
             },
             "resourceGroups": {
                 "storageRG": {
                     "description": "Contains the resource template deployment and a role assignment."
                 }
             }
         }
     }
     ```

1. Fügen Sie eine Rollenzuweisung für das Abonnement hinzu. Der **Anforderungstext** definiert die _Art_ des Artefakts sowie die Eigenschaften, die am Rollendefinitionsbezeichner ausgerichtet sind. Die Prinzipalbezeichner werden als Array von Werten übergeben. Im Beispiel unten ist für die Prinzipalbezeichner, denen die angegebene Rolle zugewiesen wird, ein Parameter konfiguriert, der bei der Blaupausenzuweisung festgelegt wird.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleContributor?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "kind": "roleAssignment",
         "properties": {
             "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
             "principalIds": "[parameters('contributors')]"
         }
     }
     ```

1. Fügen Sie eine Richtlinienzuweisung für das Abonnement hinzu. Der **Anforderungstext** definiert die _Art_ des Artefakts sowie die Eigenschaften, die sich an einer Richtlinie oder Initiativdefinition orientieren. Er konfiguriert außerdem die Richtlinienzuweisung für die Verwendung der definierten Blaupausenparameter, die im Rahmen der Blaupausenzuweisung konfiguriert werden.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyTags?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "kind": "policyAssignment",
         "properties": {
             "description": "Apply tag and its default value to resource groups",
             "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
             "parameters": {
                 "tagName": {
                     "value": "[parameters('tagName')]"
                 },
                 "tagValue": {
                     "value": "[parameters('tagValue')]"
                 }
             }
         }
     }
     ```

1. Fügen Sie eine weitere Richtlinienzuweisung für das Storage-Tag (unter Wiederverwendung des Parameters _storageAccountType_) für das Abonnement hinzu. Dieses zusätzliche Artefakt der Richtlinienzuweisung veranschaulicht, dass ein für die Blaupause definierter Parameter von mehreren Artefakten verwendet werden kann. In dem Beispiel wird **storageAccountType** verwendet, um ein Tag für die Ressourcengruppe festzulegen. Dieser Wert stellt Informationen zum im nächsten Schritt erstellten Speicherkonto bereit.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "kind": "policyAssignment",
         "properties": {
             "description": "Apply storage tag and the parameter also used by the template to resource groups",
             "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
             "parameters": {
                 "tagName": {
                     "value": "StorageType"
                 },
                 "tagValue": {
                     "value": "[parameters('storageAccountType')]"
                 }
             }
         }
     }
     ```

1. Fügen Sie eine Vorlage unter der Ressourcengruppe hinzu. Der **Anforderungstext** für eine Resource Manager-Vorlage enthält die normale JSON-Komponente der Vorlage und definiert die Zielressourcengruppe mit **properties.resourceGroup**. Darüber hinaus verwendet die Vorlage auch die Vorlagenparameter **storageAccountType**, **tagName** und **tagValue** wieder, indem sie jeweils an die Vorlage übergeben werden. Die Blaupausenparameter sind für die Vorlage durch die Definition von **properties.parameters** verfügbar, und innerhalb des JSON-Codes der Vorlage wird der Wert mithilfe dieses Schlüssel-Wert-Paars eingefügt. Die Namen der Blaupausen- und Vorlagenparameter können identisch sein, sind hier jedoch unterschiedlich, um zu veranschaulichen, wie sie jeweils von der Blaupause an das Vorlagenartefakt übergeben werden.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/templateStorage?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "kind": "template",
         "properties": {
             "template": {
                 "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                 "contentVersion": "1.0.0.0",
                 "parameters": {
                     "storageAccountTypeFromBP": {
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
                     "tagNameFromBP": {
                         "type": "string",
                         "defaultValue": "NotSet",
                         "metadata": {
                             "description": "Tag name from blueprint"
                         }
                     },
                     "tagValueFromBP": {
                         "type": "string",
                         "defaultValue": "NotSet",
                         "metadata": {
                             "description": "Tag value from blueprint"
                         }
                     }
                 },
                 "variables": {
                     "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
                 },
                 "resources": [{
                     "type": "Microsoft.Storage/storageAccounts",
                     "name": "[variables('storageAccountName')]",
                     "apiVersion": "2016-01-01",
                     "tags": {
                        "[parameters('tagNameFromBP')]": "[parameters('tagValueFromBP')]"
                     },
                     "location": "[resourceGroup().location]",
                     "sku": {
                         "name": "[parameters('storageAccountTypeFromBP')]"
                     },
                     "kind": "Storage",
                     "properties": {}
                 }],
                 "outputs": {
                     "storageAccountSku": {
                         "type": "string",
                         "value": "[variables('storageAccountName')]"
                     }
                 }
             },
             "resourceGroup": "storageRG",
             "parameters": {
                 "storageAccountTypeFromBP": {
                     "value": "[parameters('storageAccountType')]"
                 },
                 "tagNameFromBP": {
                     "value": "[parameters('tagName')]"
                 },
                 "tagValueFromBP": {
                     "value": "[parameters('tagValue')]"
                 }
             }
         }
     }
     ```

1. Fügen Sie eine Rollenzuweisung unter der Ressourcengruppe hinzu. Ähnlich wie beim vorherigen Rollenzuweisungseintrag wird im folgenden Beispiel der Definitionsbezeichner für die Rolle **Besitzer** verwendet und dafür ein anderer Parameter aus der Blaupause angegeben.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "kind": "roleAssignment",
         "properties": {
             "resourceGroup": "storageRG",
             "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
             "principalIds": "[parameters('owners')]"
         }
     }
     ```

## <a name="publish-a-blueprint"></a>Veröffentlichen einer Blaupause

Nach dem Hinzufügen der Artefakte zur Blaupause kann diese veröffentlicht werden. Durch die Veröffentlichung kann sie einem Abonnement zugewiesen werden.

- REST-API-URI

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/versions/{BlueprintVersion}?api-version=2017-11-11-preview
  ```

Der Wert für `{BlueprintVersion}` ist eine Zeichenfolge mit Buchstaben, Zahlen und Bindestrichen (keine Leerzeichen oder Sonderzeichen) mit einer maximalen Länge von 20 Zeichen. Verwenden Sie einen eindeutigen und aussagekräftigen Wert, z.B. **v20180622-135541**.

## <a name="assign-a-blueprint"></a>Zuweisen einer Blaupause

Nach dem Veröffentlichen einer Blaupause mit der REST-API kann sie einem Abonnement zugewiesen werden. Weisen Sie die erstellte Blaupause einem der Abonnements unter Ihrer Verwaltungsgruppenhierarchie zu. Der **Anforderungstext** gibt die zuzuweisende Blaupause, den Namen und Standort für alle Ressourcengruppen in der Blaupausendefinition sowie alle Parameter an, die in der Blaupause definiert sind und von einem oder mehreren angefügten Artefakten verwendet werden.

1. Geben Sie für den Azure Blueprint-Dienstprinzipal die Rolle **Besitzer** für das Zielabonnement an. Die AppId ist statisch (`f71766dc-90d9-4b7d-bd9d-4499c4331c3f`), aber die Dienstprinzipal-IDs variieren je nach Mandant. Details für Ihren Mandanten können mit der folgenden REST-API angefordert werden. Sie verwendet die [Azure Active Directory Graph-API](../../active-directory/develop/active-directory-graph-api.md), die eine andere Autorisierung aufweist.

   - REST-API-URI

     ```http
     GET https://graph.windows.net/{tenantId}/servicePrincipals?api-version=1.6&$filter=appId eq 'f71766dc-90d9-4b7d-bd9d-4499c4331c3f'
     ```

1. Führen Sie die Blaupausenbereitstellung aus, indem Sie sie einem Abonnement zuweisen. Da für die Parameter **Mitwirkende** und **Besitzer** ein Array von Objekt-IDs der Prinzipale, denen die Rollenzuweisung erteilt wird, erforderlich ist, verwenden Sie die [Azure Active Directory Graph-API](../../active-directory/develop/active-directory-graph-api.md) zum Sammeln der Objekt-IDs zur Verwendung im **Anforderungstext** für Ihre eigenen Benutzer, Gruppen oder Dienstprinzipale.

   - REST-API-URI

     ```http
     PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2017-11-11-preview
     ```

   - Anforderungstext

     ```json
     {
         "properties": {
             "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint",
             "resourceGroups": {
                 "storageRG": {
                     "name": "StorageAccount",
                     "location": "eastus2"
                 }
             },
             "parameters": {
                 "storageAccountType": {
                     "value": "Standard_GRS"
                 },
                 "tagName": {
                     "value": "CostCenter"
                 },
                 "tagValue": {
                     "value": "ContosoIT"
                 },
                 "contributors": {
                     "value": [
                         "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                         "38833b56-194d-420b-90ce-cff578296714"
                     ]
                 },
                 "owners": {
                     "value": [
                         "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                         "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                     ]
                 }
             }
         },
         "identity": {
             "type": "systemAssigned"
         },
         "location": "westus"
     }
     ```

## <a name="unassign-a-blueprint"></a>Aufheben der Zuweisung einer Blaupause

Sie können eine Blaupause aus einem Abonnement entfernen. Dieser Schritt wird häufig ausgeführt, wenn die Artefaktressourcen nicht mehr benötigt werden. Wenn eine Blaupause entfernt wird, werden die als Teil der Blaupause zugewiesenen Artefakte beibehalten. Verwenden Sie den folgenden REST-API-Vorgang, um eine Blaupausenzuweisung zu entfernen:

- REST-API-URI

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2017-11-11-preview
  ```

## <a name="delete-a-blueprint"></a>Löschen einer Blaupause

Verwenden Sie den folgenden REST-API-Vorgang, um die Blaupause zu löschen:

- REST-API-URI

  ```http
  DELETE https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2017-11-11-preview
  ```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](./concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](./concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blueprints](./concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blueprints sperren](./concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](./how-to/update-existing-assignments.md).
- Beheben Sie Probleme bei der Blueprintzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](./troubleshoot/general.md).
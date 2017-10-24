---
title: "Erstellen und Veröffentlichen einer verwalteten Azure-Dienstkataloganwendung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/09/2017
ms.author: gauravbh
ms.openlocfilehash: 3ff2108d08bacc4bc5f79a768b9c131aa7e6fceb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Veröffentlichen einer verwalteten Anwendung für die interne Nutzung

Sie können [verwaltete Azure-Anwendungen](managed-application-overview.md) erstellen und veröffentlichen, die für Mitglieder Ihrer Organisation vorgesehen sind. Die IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Konformität mit Ihren Unternehmensstandards gewährleisten. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Um eine verwaltete Anwendung für den Dienstkatalog zu veröffentlichen, gehen Sie folgendermaßen vor:

* Erstellen Sie ein ZIP-Paket, das die zwei erforderlichen Vorlagendateien enthält.
* Entscheiden Sie, welche Benutzer, Gruppen oder Anwendungen Zugriff auf die Ressourcengruppe im Benutzerabonnement benötigen.
* Erstellen Sie die verwaltete Anwendungsdefinition, die auf das ZIP-Paket verweist und Zugriff auf die Identität anfordert.

## <a name="create-a-managed-application-package"></a>Erstellen eines Pakets für eine verwaltete Anwendung

Im ersten Schritt werden die zwei erforderlichen Vorlagendateien erstellt. Komprimieren Sie die beiden Dateien zu einer ZIP-Datei, und laden Sie sie an einen zugänglichen Speicherort hoch, z.B. ein Speicherkonto. Sie übergeben einen Link zu dieser ZIP-Datei, wenn Sie die Definition der verwalteten Anwendung erstellen.

* **mainTemplate.json:** Diese Datei definiert die Azure-Ressourcen, die als Teil der verwalteten Anwendung bereitgestellt werden. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen. Beispielsweise enthält die Datei „mainTemplate.json“ zum Erstellen eines Speicherkontos mithilfe einer verwalteten Anwendung Folgendes:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]",
            "apiVersion": "2016-01-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {}
  }
  ```

* **createUiDefinition.json:** Das Azure-Portal verwendet diese Datei zum Generieren der Benutzeroberfläche für Benutzer, die die verwaltete Anwendung erstellen. Sie legen fest, wie Benutzer die Eingaben für die einzelnen Parameter bereitstellen. Sie können Optionen wie eine Dropdownliste, ein Textfeld, ein Kennwortfeld und andere Eingabetools verwenden. Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).

  Das folgende Beispiel zeigt eine „createUiDefinition.json“-Datei, die es Benutzern ermöglicht, das Namenspräfix für das Speicherkonto über ein Textfeld einzugeben.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {
                "name": "storageAccounts",
                "type": "Microsoft.Common.TextBox",
                "label": "Storage account name prefix",
                "defaultValue": "storage",
                "toolTip": "Provide a value that is used for the prefix of your storage account. Limit to 11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-11 characters long."
                },
                "visible": true
            }
        ],
        "steps": [],
        "outputs": {
            "storageAccountNamePrefix": "[basics('storageAccounts')]"
        }
    }
  }
  ```

Nachdem alle erforderlichen Dateien bereit sind, packen Sie sie als eine ZIP-Datei. Die zwei Dateien müssen sich auf der Stammebene der ZIP-Datei befinden. Wenn Sie sie in einem Ordner speichern, erhalten Sie, wenn Sie die Definition der verwalteten Anwendung erstellen, eine Fehlermeldung, dass die erforderlichen Dateien nicht vorhanden sind. Laden Sie das Paket an einen zugänglichen Speicherort hoch, wo es verwendet werden kann. In diesem Artikel wird davon ausgegangen, dass die ZIP-Datei in einem öffentlich zugänglichen Storage-Blobcontainer verfügbar ist.

Derzeit können Sie eine verwaltete Anwendung für den Dienstkatalog entweder über die Azure CLI oder das Azure-Portal erstellen. In diesem Artikel werden beide Ansätze beschrieben.

## <a name="create-managed-application-with-azure-cli"></a>Erstellen Sie eine verwaltete Anwendung mit Azure-CLI

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Erstellen einer Azure Active Directory-Benutzergruppe oder -Anwendung

Der zweite Schritt besteht darin, eine Benutzergruppe oder Anwendung für das Verwalten der Ressourcen für den Kunden zu erstellen. Diese Benutzergruppe oder Anwendung verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Es kann sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle (Role-Based Access Control, RBAC) wie „Besitzer“ oder „Mitwirkender“ handeln. Sie können einem einzelnen Benutzer auch Berechtigungen zum Verwalten der Ressourcen zuweisen, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Weitere Informationen zum Erstellen einer neuen Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Sie benötigen zum Verwalten der Ressourcen die Objekt-ID der Benutzergruppe. Das folgende Beispiel zeigt, wie die Objekt-ID vom Anzeigenamen der Gruppe abgerufen wird:

```azurecli-interactive
az ad group show --group exampleGroupName
```

Der Beispielbefehl gibt die folgende Ausgabe zurück:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

Verwenden Sie zum Abrufen der Objekt-ID Folgendes:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

### <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel verwenden Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```azurecli-interactive
az role definition list --name owner
```

Der Befehl gibt die folgende Ausgabe zurück:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access to resources.",
      "permissions": [
        {
          "actions": [
            "*"
         ],
         "notActions": []
        }
      ],
      "roleName": "Owner",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
}
```

Sie benötigen den Wert der Eigenschaft „name“ aus dem vorhergehenden Beispiel. Sie können nur diese Eigenschaft wie folgt abrufen:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

### <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Wenn Sie noch über keine Ressourcengruppe für das Speichern der Definition Ihrer verwalteten Anwendung verfügen, erstellen Sie sie jetzt:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung.

```azurecli-interactive
az managedapp definition create \
  --name storageApp \
  --location "westcentralus" \
  --resource-group managedApplicationGroup \
  --lock-level ReadOnly \
  --display-name myteststorageapp \
  --description storageapp \
  --authorizations "$groupid:$roleid" \
  --package-file-uri <uri-path-to-zip-file>
```

Die im vorhergehenden Beispiel verwendeten Parameter sind:

* **resource-group:** Der Name der Ressourcengruppe, in der die Definition für die verwaltete Anwendung erstellt wird.
* **lock-level**: Der Typ der Sperre, der auf die verwaltete Ressourcengruppe angewendet wird. Er verhindert, dass der Kunde unerwünschte Vorgänge für diese Ressourcengruppe durchführt. Derzeit ist „ReadOnly“ die einzige unterstützte Sperrebene. Wenn „ReadOnly“ festgelegt wird, sind die in der verwalteten Ressourcengruppe vorhandenen Ressourcen für den Kunden schreibgeschützt.
* **authorizations**: Beschreibt die Prinzipal-ID und die Rollendefinitions-ID, die zum Erteilen von Berechtigungen für die verwaltete Ressourcengruppe verwendet werden. Diese ist im Format `<principalId>:<roleDefinitionId>` angegeben. Für diese Eigenschaft können auch mehrere Werte angegeben werden. Wenn mehrere Werte erforderlich sind, sollten sie in diesem Format angegeben werden: `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Mehrere Werte werden durch ein Leerzeichen voneinander getrennt.
* **package-file-uri:** Der Speicherort des Pakets für die verwaltete Anwendung mit den Vorlagendateien. Dabei kann es sich um Azure Storage Blob handeln.

## <a name="create-managed-application-with-portal"></a>Erstellen Sie eine verwaltete Anwendung mit dem Portal

1. Erstellen Sie bei Bedarf eine Azure Active Directory-Benutzergruppe, um die Ressourcen zu verwalten. Weitere Informationen dazu finden Sie in [Erstellen einer Gruppe in Azure Active Directory und Hinzufügen von Mitgliedern](../active-directory/active-directory-groups-create-azure-portal.md).
1. Klicken Sie in der oberen linken Ecke auf **+ Neu**.

   ![Neuer Dienst](./media/managed-application-publishing/new.png)

1. Suchen Sie nach **Dienstkatalog**.

1. Scrollen Sie in den Ergebnissen zu **Service Catalog Managed Application Definition (Definition für die verwaltete Dienstkataloganwendung)**. Wählen Sie dies aus.

   ![Suchen Sie nach Definitionen für die verwaltete Anwendung](./media/managed-application-publishing/select-managed-apps-definition.png)

1. Wählen Sie **Erstellen** aus, um den Erstellungsprozess der Definition für die verwaltete Anwendung zu starten.

   ![Erstellen der Definition für die verwaltete Anwendung](./media/managed-application-publishing/create-definition.png)

1. Geben Sie Werte für Name, Anzeigename, Beschreibung, Speicherort, Abonnement und Ressourcengruppe ein. Geben Sie für den Paketdatei-URI den Pfad zur ZIP-Datei an, die Sie erstellt haben.

   ![Geben Sie die Werte ein](./media/managed-application-publishing/fill-application-values.png)

1. Wenn Sie zur Authentifizierung und zum Sperrebene-Abschnitt gelangen, wählen Sie **Autorisierung hinzufügen** aus.

   ![Autorisierung hinzufügen](./media/managed-application-publishing/add-authorization.png)

1. Wählen Sie bei Bedarf eine Azure Active Directory-Gruppe aus, um die Ressourcen zu verwalten, und wählen Sie **OK** aus.

   ![Autorisierungsgruppe hinzufügen](./media/managed-application-publishing/add-auth-group.png)

1. Wenn Sie alle Werte eingegeben haben, wählen Sie **Erstellen**.

   ![Verwaltete Anwendung erstellen](./media/managed-application-publishing/create-app.png)

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](managed-application-overview.md).
* Beispiele für die Dateien finden Sie unter [Beispiele für verwaltete Anwendungen](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Informationen zum Nutzen einer verwalteten Dienstkataloganwendung, finden Sie unter [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).
* Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).
* Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).

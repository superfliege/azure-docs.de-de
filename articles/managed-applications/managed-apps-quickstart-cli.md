---
title: Erstellen einer verwalteten Azure-Anwendung per Azure CLI | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist."
services: azure-resource-manager
author: tfitzmac
manager: timlt
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 10/20/2017
ms.author: tomfitz
ms.openlocfilehash: e104778db445d078ffca7a9ddb9d28fc84a93c81
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="create-and-deploy-an-azure-managed-application-with-azure-cli"></a>Erstellen und Bereitstellen einer verwalteten Azure-Anwendung per Azure CLI

Dieser Artikel enthält eine Einführung in die Verwendung von verwalteten Anwendungen. Sie fügen einem internen Katalog für die Benutzer Ihrer Organisation eine Definition für die verwaltete Anwendung hinzu. Anschließend stellen Sie diese verwaltete Anwendung für Ihr Abonnement bereit. Wir haben die Dateien für Ihre verwaltete Anwendung bereits erstellt, um die Einführung zu vereinfachen. Mit einer Datei wird die Infrastruktur für Ihre Lösung definiert. Mit einer zweiten Datei wird die Benutzeroberfläche für die Bereitstellung der verwalteten Anwendung über das Portal definiert. Diese Dateien sind über GitHub verfügbar. Im Tutorial [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md) erfahren Sie, wie Sie diese Dateien erstellen.

Nach Abschluss des Vorgangs verfügen Sie über drei Ressourcengruppen, die unterschiedliche Teile der verwalteten Anwendung enthalten.

| Ressourcengruppe | Contains | Beschreibung |
| -------------- | -------- | ----------- |
| appDefinitionGroup | Die Definition der verwalteten Anwendung. | Der Herausgeber erstellt diese Ressourcengruppe und die Definition der verwalteten Anwendung. Alle Benutzer, die Zugriff auf die Definition der verwalteten Anwendung haben, können sie bereitstellen. |
| applicationGroup | Die Instanz der verwalteten Anwendung. | Der Consumer erstellt diese Ressourcengruppe und die Instanz der verwalteten Anwendung. Der Consumer kann die verwaltete Anwendung über diese Instanz aktualisieren. |
| infrastructureGroup | Die Ressourcen für die verwaltete Anwendung. | Diese Ressourcengruppe wird automatisch erstellt, wenn die verwaltete Anwendung erstellt wird. Der Herausgeber hat Zugriff auf diese Ressourcengruppe, um die Anwendung zu verwalten. Der Consumer verfügt über eingeschränkten Zugriff auf die Ressourcengruppe. |

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Erstellen einer Ressourcengruppe für die Definition

Die Definition Ihrer verwalteten Anwendung ist in einer Ressourcengruppe enthalten. Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe zu erstellen:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Beim Definieren der verwalteten Anwendung wählen Sie einen Benutzer, eine Gruppe oder eine Anwendung für die Verwaltung der Ressourcen im Namen des Consumers aus. Diese Identität verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Normalerweise erstellen Sie eine Azure Active Directory-Gruppe, um die Ressourcen zu verwalten. Verwenden Sie für diesen Artikel aber Ihre eigene Identität.

Geben Sie im folgenden Befehl Ihren Benutzerprinzipalnamen an, um die Objekt-ID Ihrer Identität zu erhalten:

```azurecli-interactive
userid=$(az ad user show --upn-or-object-id example@contoso.org --query objectId --output tsv)
```

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer gewähren möchten. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung. Die verwaltete Anwendung enthält nur ein Speicherkonto.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"
```

Nachdem die Ausführung des Befehls abgeschlossen ist, verfügen Sie in Ihrer Ressourcengruppe über eine Definition für die verwaltete Anwendung. 

Im vorigen Beispiel werden u.a. folgende Parameter verwendet:

* **resource-group:** Der Name der Ressourcengruppe, in der die Definition für die verwaltete Anwendung erstellt wird.
* **lock-level**: Der Typ der Sperre, der auf die verwaltete Ressourcengruppe angewendet wird. Er verhindert, dass der Kunde unerwünschte Vorgänge für diese Ressourcengruppe durchführt. Derzeit ist „ReadOnly“ die einzige unterstützte Sperrebene. Wenn „ReadOnly“ festgelegt wird, sind die in der verwalteten Ressourcengruppe vorhandenen Ressourcen für den Kunden schreibgeschützt.
* **authorizations**: Beschreibt die Prinzipal-ID und die Rollendefinitions-ID, die zum Erteilen von Berechtigungen für die verwaltete Ressourcengruppe verwendet werden. Diese ist im Format `<principalId>:<roleDefinitionId>` angegeben. Für diese Eigenschaft können auch mehrere Werte angegeben werden. Wenn mehrere Werte erforderlich sind, sollten sie in diesem Format angegeben werden: `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Mehrere Werte werden durch ein Leerzeichen voneinander getrennt.
* **package-file-uri**: Der Speicherort eines ZIP-Pakets, das die erforderlichen Dateien enthält. Das Paket enthält mindestens die Dateien **mainTemplate.json** und **createUiDefinition.json**. Die Datei **mainTemplate.json** definiert die Azure-Ressourcen, die als Teil der verwalteten Anwendung bereitgestellt werden. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen. Die Datei **createUiDefinition.json** generiert die Benutzeroberfläche für Benutzer, die die verwaltete Anwendung über das Portal erstellen.

## <a name="create-resource-group-for-managed-application"></a>Erstellen der Ressourcengruppe für die verwaltete Anwendung

Es ist nun alles zum Bereitstellen der verwalteten Anwendung bereit. 

Sie benötigen eine Ressourcengruppe für die bereitgestellte verwaltete Anwendung. Verwenden Sie **westcentralus** als Standort.

```azurecli-interactive
az group create --name applicationGroup --location westcentralus
```

## <a name="deploy-the-managed-application"></a>Bereitstellen der verwalteten Anwendung

Sie stellen die Anwendung mit den folgenden Befehlen bereit:

```azurecli-interactive
appid=$(az managedapp definition show --name ManagedStorage --resource-group appDefinitionGroup --query id --output tsv)
subid=$(az account show --query id --output tsv)
managedGroupId=/subscriptions/$subid/resourceGroups/infrastructureGroup

az managedapp create \
  --name storageApp \
  --location "westcentralus" \
  --kind "Servicecatalog" \
  --resource-group applicationGroup \
  --managedapp-definition-id $appid \
  --managed-rg-id $managedGroupId \
  --parameters "{\"storageAccountNamePrefix\": {\"value\": \"<your-prefix>\"}, \"storageAccountType\": {\"value\": \"Standard_LRS\"}}"
```

Im vorigen Beispiel werden u.a. folgende Parameter verwendet:

* **managedapp-definition-id**: Die ID der Definition, die Sie weiter oben in diesem Artikel erstellt haben.
* **managed-rg-id**: Die ID der Ressourcengruppe für die Ressourcen, die der verwalteten Anwendung zugeordnet sind. Mit dem Befehl wird diese Ressourcengruppe erstellt. **Bevor der Befehl ausgeführt wird, darf sie noch nicht vorhanden sein**. Diese Ressourcengruppe wird vom Herausgeber verwaltet. 
* **resource-group**: Die Ressourcengruppe, in der die Ressource für die verwaltete Anwendung erstellt wird.
* **parameters**: Die erforderlichen Parameter für die Ressourcen, die der verwalteten Anwendung zugeordnet sind.

Nach dem erfolgreichen Abschluss der Bereitstellung sehen Sie, dass die verwaltete Anwendung in applicationGroup erstellt wurde. Die Ressource storageAccount wird in infrastructureGroup erstellt.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](overview.md).
* Beispiele für die Dateien finden Sie unter [Beispiele für verwaltete Anwendungen](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Azure Marketplace finden Sie unter [Erstellen einer Marketplace-Anwendung](publish-marketplace-app.md).
* Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).

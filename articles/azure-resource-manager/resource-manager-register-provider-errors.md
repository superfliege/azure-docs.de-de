---
title: Fehler bei der Azure-Ressourcenanbieterregistrierung | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie Fehler bei der Azure-Ressourcenanbieterregistrierung beheben.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: d6a99917e732a3439a31cafa5608348694014054
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Beheben von Fehlern bei der Ressourcenanbieterregistrierung

In diesem Artikel werden die Fehler beschrieben, die bei Verwendung eines Ressourcenanbieters auftreten können, den Sie in Ihrem Abonnement zuvor noch nicht genutzt haben.

## <a name="symptom"></a>Symptom

Beim Bereitstellen von Ressourcen werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Oder Sie erhalten eine ähnliche Meldung, die Folgendes besagt:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

## <a name="cause"></a>Ursache

Für diese Fehler gibt es drei Gründe:

1. Der Ressourcenanbieter wurde für Ihr Abonnement nicht registriert.
1. Die API-Version wird für den Ressourcentyp nicht unterstützt.
1. Der Standort wird für den Ressourcentyp nicht unterstützt.

## <a name="solution"></a>Lösung

Die Fehlermeldung enthält in der Regel Vorschläge für die unterstützten Standorte und API-Versionen. Sie können Ihre Vorlage in einen der vorgeschlagenen Werte ändern. Die meisten Anbieter werden automatisch über das Azure-Portal oder die verwendete Befehlszeilenschnittstelle registriert. Wenn Sie zuvor noch keinen bestimmten Ressourcenanbieter verwendet haben, müssen Sie diesen Anbieter unter Umständen registrieren. Weitere Informationen zu Ressourcenanbietern erhalten Sie in PowerShell oder der Azure-Befehlszeilenschnittstelle.

### <a name="solution-1"></a>Lösung 1

Für PowerShell verwenden Sie zum Anzeigen des Registrierungsstatus **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Verwenden Sie **Register-AzureRmResourceProvider** , um einen Anbieter zu registrieren, und geben Sie den Namen des zu registrierenden Ressourcenanbieters an.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Verwenden Sie zum Abrufen der unterstützten Standorte für einen bestimmten Ressourcentyp Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Verwenden Sie zum Abrufen der unterstützten API-Versionen für einen bestimmten Ressourcentyp Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

### <a name="solution-2"></a>Lösung 2

**Azure-Befehlszeilenschnittstelle**

Mit dem Befehl `az provider list` können Sie ermitteln, ob der Anbieter registriert ist.

```azurecli-interactive
az provider list
```

Verwenden Sie den Befehl `az provider register` , um einen Ressourcenanbieter zu registrieren, und geben Sie den zu registrierenden *Namespace* an.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Verwenden Sie zum Anzeigen der unterstützten Standorte und API-Versionen für einen Ressourcentyp Folgendes:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

### <a name="solution-3"></a>Lösung 3

Sie können den Registrierungsstatus sehen und einen Ressourcenanbieter-Namespace über das Portal registrieren.

1. Wählen Sie für Ihr Abonnement die Option **Ressourcenanbieter** aus.

   ![Ressourcenanbieter auswählen](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Betrachten Sie die Liste der Ressourcenanbieter, und wählen Sie bei Bedarf den Link **Registrieren** aus, um den Ressourcenanbietertyp zu registrieren, den Sie bereitstellen möchten.

   ![Liste der Ressourcenanbieter](./media/resource-manager-register-provider-errors/list-resource-providers.png)

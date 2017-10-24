---
title: "Fehler vom Typ „Azure-Ressource nicht gefunden“ | Microsoft-Dokumentation"
description: "Hier wird erläutert, wie Sie Fehler beheben, wenn eine Ressource nicht gefunden wird."
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
ms.openlocfilehash: c76c965c43ca8217faa9488c01975ce09a21daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Beheben von Fehlern aufgrund nicht gefundener Azure-Ressourcen

In diesem Artikel werden die Fehler beschrieben, die auftreten können, wenn eine Ressource während der Bereitstellung nicht gefunden wird. 

## <a name="symptom"></a>Symptom

Wenn Ihre Vorlage einen Ressourcennamen enthält, der nicht aufgelöst werden kann, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Wenn Sie versuchen, die Funktionen [reference](resource-group-template-functions-resource.md#reference) oder [listKeys](resource-group-template-functions-resource.md#listkeys) mit einer Ressource zu verwenden, die nicht aufgelöst werden kann, erhalten Sie folgenden Fehler:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Ursache

Resource Manager muss die Eigenschaften für eine Ressource abrufen, kann die Ressource in Ihrem Abonnement jedoch nicht identifizieren.

## <a name="solution"></a>Lösung

### <a name="solution-1"></a>Lösung 1

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitzustellen, prüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden, um eine Abhängigkeit zu der anderen Ressource herzustellen. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Beim Versuch, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie verhindern diesen Fehler, indem Sie die Abhängigkeit in der Web-App festlegen.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Vorschläge zur Problembehandlung für Abhängigkeitsfehler finden Sie unter [Überprüfen der Bereitstellungssequenz](resource-manager-troubleshoot-tips.md#check-deployment-sequence).

### <a name="solution-2"></a>Lösung 2

Wenn die Ressource in einer anderen Ressourcengruppe als der Ressourcengruppe enthalten ist, die gerade bereitgestellt wird, rufen Sie mit [resourceId function](resource-group-template-functions-resource.md#resourceid) den vollqualifizierten Namen der Ressource ab.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

### <a name="solution-3"></a>Lösung 3

Suchen Sie nach einem Ausdruck, der die [reference](resource-group-template-functions-resource.md#reference)-Funktion enthält. Die von Ihnen bereitgestellten Werte variieren basierend darauf, ob sich die Ressource in der gleichen Vorlage und Ressourcengruppe und im gleichen Abonnement befindet. Überprüfen Sie, ob die erforderlichen Parameterwerte für Ihr Szenario angegeben wurden. Befindet sich die Ressource in einer anderen Ressourcengruppe, geben Sie die vollständige Ressourcen-ID an. Verwenden Sie zum Verweisen auf ein Speicherkonto in einer anderen Ressourcengruppe beispielsweise Folgendes:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```
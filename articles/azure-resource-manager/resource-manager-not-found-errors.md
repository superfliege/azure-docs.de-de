---
title: Fehler vom Typ „Azure-Ressource nicht gefunden“ | Microsoft-Dokumentation
description: Hier wird erläutert, wie Sie Fehler beheben, wenn eine Ressource nicht gefunden wird.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: f5da2a74b3a399c60c518f386ccf2e60a617aeda
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Beheben von Fehlern aufgrund nicht gefundener Azure-Ressourcen

In diesem Artikel werden die Fehler beschrieben, die auftreten können, wenn eine Ressource während der Bereitstellung nicht gefunden wird.

## <a name="symptom"></a>Symptom

Wenn Ihre Vorlage einen Ressourcennamen enthält, der nicht aufgelöst werden kann, erhalten Sie in etwa folgende Fehlermeldung:

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

Resource Manager muss die Eigenschaften für eine Ressource abrufen, kann die Ressource in Ihrem Abonnement aber nicht identifizieren.

## <a name="solution-1---set-dependencies"></a>Lösung 1: Festlegen von Abhängigkeiten

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitzustellen, sollten Sie prüfen, ob Sie eine Abhängigkeit hinzufügen müssen. Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Beim Versuch, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie verhindern diesen Fehler, indem Sie die Abhängigkeit in der Web-App festlegen.

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

Es ist ratsam, das Einrichten von Abhängigkeiten zu vermeiden, die nicht benötigt werden. Wenn Sie über nicht benötigte Abhängigkeiten verfügen, verlängern Sie die Dauer der Bereitstellung, indem Sie verhindern, dass nicht voneinander abhängige Ressourcen parallel bereitgestellt werden. Außerdem besteht die Gefahr, dass Sie Ringabhängigkeiten erstellen, die die Bereitstellung blockieren. Mit der Funktion [reference](resource-group-template-functions-resource.md#reference) wird eine implizite Abhängigkeit von der angegebenen Ressource erstellt, wenn diese Ressource in derselben Vorlage bereitgestellt wird. Aus diesem Grund verfügen Sie ggf. über eine höhere Zahl von Abhängigkeiten als in der **dependsOn**-Eigenschaft angegeben. Mit der Funktion [resourceId](resource-group-template-functions-resource.md#resourceid) wird keine implizite Abhängigkeit erstellt und nicht überprüft, ob die Ressource vorhanden ist.

Wenn Abhängigkeitsprobleme auftreten, benötigen Sie Informationen zur Reihenfolge der Ressourcenbereitstellung. So zeigen Sie die Reihenfolge der Vorgänge bei der Bereitstellung an:

1. Wählen Sie den Bereitstellungsverlauf für die Ressourcengruppe aus.

   ![Bereitstellungsverlauf auswählen](./media/resource-manager-not-found-errors/select-deployment.png)

2. Wählen Sie aus dem Verlauf eine Bereitstellung und **Ereignisse** aus.

   ![Bereitstellungsereignisse auswählen](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Überprüfen Sie die Abfolge von Ereignissen für jede Ressource. Achten Sie auf den Status der einzelnen Vorgänge. Die folgende Abbildung zeigt beispielsweise drei parallel bereitgestellte Speicherkonten. Beachten Sie, dass die drei Speicherkonten zur selben Zeit gestartet werden.

   ![Parallele Bereitstellung](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   Die nächste Abbildung zeigt drei Speicherkonten, die nicht parallel bereitgestellt werden. Das zweite Speicherkonto ist vom ersten Speicherkonto abhängig, und das dritte Speicherkonto ist vom zweiten Speicherkonto abhängig. Das erste Speicherkonto wird gestartet, akzeptiert und abgeschlossen, bevor das nächste gestartet wird.

   ![Sequenzielle Bereitstellung](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Lösung 2: Abrufen der Ressource aus einer anderen Ressourcengruppe

Wenn die Ressource in einer anderen Ressourcengruppe als der Ressourcengruppe enthalten ist, die gerade bereitgestellt wird, rufen Sie mit [resourceId function](resource-group-template-functions-resource.md#resourceid) den vollqualifizierten Namen der Ressource ab.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Lösung 3: Überprüfen der reference-Funktion

Suchen Sie nach einem Ausdruck, der die [reference](resource-group-template-functions-resource.md#reference)-Funktion enthält. Die von Ihnen bereitgestellten Werte variieren basierend darauf, ob sich die Ressource in der gleichen Vorlage und Ressourcengruppe und im gleichen Abonnement befindet. Überprüfen Sie, ob die erforderlichen Parameterwerte für Ihr Szenario angegeben wurden. Befindet sich die Ressource in einer anderen Ressourcengruppe, geben Sie die vollständige Ressourcen-ID an. Verwenden Sie zum Verweisen auf ein Speicherkonto in einer anderen Ressourcengruppe beispielsweise Folgendes:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```
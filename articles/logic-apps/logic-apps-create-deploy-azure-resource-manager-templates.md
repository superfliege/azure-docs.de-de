---
title: Erstellen von Logik-Apps auf der Grundlage von Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Erstellen und Bereitstellen von Logik-App-Workflows mit Azure Resource Manager-Vorlagen
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: LADocs; mandia
ms.openlocfilehash: e30ed8b1b8e2241bbebab1d7c5f337fabf37e1dd
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="create-and-deploy-logic-apps-with-azure-resource-manager-templates"></a>Erstellen und Bereitstellen von Logik-Apps mit Azure Resource Manager-Vorlagen

Azure Logic Apps bietet Azure Resource Manager-Vorlagen, mit denen Sie nicht nur Logik-Apps zur Automatisierung von Workflows erstellen, sondern auch die für die Bereitstellung verwendeten Ressourcen und Parameter definieren können. Die Vorlage kann für eigene Unternehmensszenarien verwendet oder an Ihre individuellen Anforderungen angepasst werden. Weitere Informationen finden Sie in der [Resource Manager-Vorlage für Logic-Apps](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) sowie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

## <a name="define-the-logic-app"></a>Definieren der Logik-App

Diese Beispieldefinition einer Logik-App wird stündlich ausgeführt und pingt den im Parameter `testUri` angegebenen Speicherort.
Die Vorlage verwendet Parameterwerte für den Logik-App-Namen (```logicAppName```) und den Speicherort, der zu Testzwecken gepingt wird (```testUri```). Weitere Informationen zum Definieren dieser Parameter in Ihrer Vorlage finden Sie [hier](#define-parameters). Die Vorlage legt auch den Speicherort für die Logik-App auf den Speicherort der Azure-Ressourcengruppe fest. 

``` json
{
    "type": "Microsoft.Logic/workflows",
    "apiVersion": "2016-06-01",
    "name": "[parameters('logicAppName')]",
    "location": "[resourceGroup().location]",
    "tags": {
        "displayName": "LogicApp"
    },
    "properties": {
        "definition": {
            "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "testURI": {
                "type": "string",
                "defaultValue": "[parameters('testUri')]"
                }
            },
            "triggers": {
                "Recurrence": {
                    "type": "Recurrence",
                    "recurrence": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            },
            "actions": {
                "Http": {
                    "type": "Http",
                    "inputs": {
                        "method": "GET",
                        "uri": "@parameters('testUri')"
                    },
                    "runAfter": {}
                }
            },
            "outputs": {}
        },
        "parameters": {}
    }
}
``` 

<a name="define-parameters"></a>

### <a name="define-parameters"></a>Definieren von Parametern

[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Im Anschluss werden die Parameter in der Vorlage beschrieben:

| Parameter | Beschreibung | Beispiel für JSON-Definition | 
| --------- | ----------- | ----------------------- | 
| `logicAppName` | Definiert den Namen der Logik-App, die durch diese Vorlage erstellt wird. | "logicAppName": { "type": "string", "metadata": { "description": "myExampleLogicAppName" } } |
| `testUri` | Definiert den Speicherort, der zu Testzwecken gepingt werden soll. | "testUri": { "type": "string", "defaultValue": "http://azure.microsoft.com/status/feed/"} | 
||||

Bei Bedarf können Sie sich ausführlicher über die [REST-API für Logic Apps-Workflowdefinition und -eigenschaften](https://docs.microsoft.com/rest/api/logic/workflows) sowie über das [Erstellen von Workflowdefinitionen für Logik-Apps per JSON-Code](logic-apps-author-definitions.md) informieren.

## <a name="deploy-logic-apps-automatically"></a>Automatisches Bereitstellen von Logik-Apps

Wenn Sie eine Logik-App erstellen und automatisch in Azure bereitstellen möchten, klicken Sie hier auf **In Azure bereitstellen**:

[![In Azure bereitstellen](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Dadurch werden Sie beim Azure-Portal angemeldet, wo Sie die Details zu Ihrer Logik-App angeben und die Vorlage oder die Parameter nach Bedarf ändern können. Im Azure-Portal werden beispielsweise folgende Details angefordert:

* Name des Azure-Abonnements
* Gewünschte Ressourcengruppe
* Speicherort der Logik-App
* Name für Ihre Logik-App
* Test-URI
* Annahme der angegebenen Bedingungen

## <a name="deploy-logic-apps-with-commands"></a>Bereitstellen von Logik-Apps mithilfe von Befehlen

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup
``` 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überwachen von Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
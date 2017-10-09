---
title: "Entwickeln von Vorlagen für Azure Stack | Microsoft-Dokumentation"
description: "Informationen zu bewährten Methoden für Azure Stack-Vorlagen"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ffad7bfd4ffcd9159dea23b70640f0ee761fbae0
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="azure-resource-manager-template-considerations"></a>Aspekte zu Azure Resource Manager-Vorlagen

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Bei der Entwicklung Ihrer Anwendung ist es wichtig, die Portabilität von Vorlagen zwischen Azure und Azure Stack sicherzustellen.  In diesem Thema werden Aspekte beim Entwickeln von Azure Resource Manager-[Vorlagen](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) behandelt, sodass Sie Prototypen Ihrer Anwendung erstellen und die Bereitstellung in Azure ohne Zugriff auf eine Azure Stack-Umgebung testen können.

## <a name="public-namespaces"></a>Öffentliche Namespaces
Da Azure Stack in Ihrem Rechenzentrum gehostet wird, weist die Lösung andere Dienstendpunkt-Namespaces als die öffentliche Azure-Cloud auf. Daher verursachen hartcodierte öffentliche Endpunkte in Resource Manager-Vorlagen Fehler, wenn Sie versuchen, sie in Azure Stack bereitzustellen. Stattdessen können Sie die Funktionen *reference* und *concatenate* verwenden, um den Dienstendpunkt basierend auf Werten dynamisch zu erstellen, die während der Bereitstellung vom Ressourcenanbieter abgerufen wurden. Statt beispielsweise *blob.core.windows.net* in Ihrer Vorlage anzugeben, rufen Sie [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) ab, um den Endpunkt *osDisk.URI* dynamisch festzulegen:

     "osDisk": {"name": "osdisk","vhd": {"uri": 
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>API-Versionsverwaltung
Azure Service-Versionen können zwischen Azure und Azure Stack unterschiedlich sein. Jede Ressource erfordert das Attribut „apiVersion“, das die gebotenen Fähigkeiten definiert. Um API-Versionskompatibilität in Azure Stack sicherzustellen, sind die folgenden API-Versionen für jeden Ressourcenanbieter gültig:

| Ressourcenanbieter | apiVersion |
| --- | --- |
| Compute |`'2015-06-15'` |
| Netzwerk |`'2015-06-15'`, `'2015-05-01-preview'` |
| Speicher |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |
| MySQL |`'2015-09-01'` |
| SQL |`'2014-04-01-preview'` |

## <a name="template-functions"></a>Funktionen von Azure-Ressourcen-Manager-Vorlagen
Ressourcen-Manager-[Funktionen](../../azure-resource-manager/resource-group-template-functions.md) ermöglichen das Erstellen dynamischer Vorlagen. Beispielsweise können Sie Funktionen für Aufgaben wie die folgenden verwenden:

* Verketten oder Kürzen von Zeichenfolgen 
* Verweisen auf Werte aus anderen Ressourcen
* Durchlaufen von Ressourcen zur Bereitstellung mehrerer Instanzen 

Beachten Sie beim Erstellen Ihrer Vorlagen, dass einige Funktionen im Azure Stack Development Kit nicht verfügbar sind und nicht verwendet werden dürfen. Dabei handelt es sich um diese Funktionen:

* Skip
* Take

## <a name="resource-location"></a>Speicherort von Ressourcen
Resource Manager-Vorlagen nutzen das Attribut „location“ zum Angeben des Speicherorts von Ressourcen während der Bereitstellung. In Azure verweisen Speicherorte auf eine Region wie USA, Westen oder Südamerika. In Azure Stack sind Speicherorte anders, weil Azure Stack sich in Ihrem Rechenzentrum befindet.  Um sicherzustellen, dass Vorlagen zwischen Azure und Azure Stack übertragbar sind, müssen Sie auf den Speicherort der Ressourcengruppe verweisen, während Sie einzelne Ressourcen bereitstellen. Nutzen Sie hierfür `[resourceGroup().Location]`, um sicherzustellen, dass alle Ressourcen den Speicherort der Ressourcengruppe erben.  Der folgende Auszug aus einer Resource Manager-Vorlage ist ein Beispiel dieser Funktion beim Bereitstellen eines Speicherkontos:

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]


## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)
* [Bereitstellen von Vorlagen mit der Azure CLI](azure-stack-deploy-template-command-line.md)
* [Bereitstellen von Vorlagen mithilfe von Visual Studio](azure-stack-deploy-template-visual-studio.md)



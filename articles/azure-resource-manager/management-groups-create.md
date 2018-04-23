---
title: Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-Verwaltungsgruppen zum Verwalten mehrerer Ressourcen erstellen.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: 48af0bd566f8250e47e03ae45c8ba1c06e5a6792
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen
Bei Verwaltungsgruppen handelt es sich um Container, mit denen Sie Zugriff, Richtlinien und Konformität abonnementübergreifend verwalten können. Erstellen Sie diese Container, um eine effektive und effiziente Hierarchie zu erstellen, die mit [Azure Policy](../azure-policy/azure-policy-introduction.md) und mit der [rollenbasierten Zugriffssteuerung (Role-Based Access Controls, RBAC) von Azure](../role-based-access-control/overview.md) verwendet werden kann. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen). 

Das Feature für Verwaltungsgruppen ist in einer öffentlichen Vorschauversion verfügbar. Melden Sie sich zur Verwendung von Verwaltungsgruppen beim [Azure-Portal](https://portal.azure.com) an. Alternativ können Sie [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), die [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available) oder die [REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) verwenden, um Verwaltungsgruppen zu erstellen.   

Es kann bis zu 15 Minuten dauern, bis die Erstellung der ersten Verwaltungsgruppe im Verzeichnis abgeschlossen ist. Bei der ersten Erstellung werden Prozesse zum Einrichten des Verwaltungsgruppendiensts in Azure für Ihr Verzeichnis ausgeführt. Sie erhalten eine Benachrichtigung, wenn der Vorgang abgeschlossen ist.  

## <a name="how-to-create-a-management-group"></a>Erstellen einer Verwaltungsgruppe
Die Verwaltungsgruppe kann über das Portal, mithilfe von PowerShell oder mithilfe der Azure CLI erstellt werden.

### <a name="create-in-portal"></a>Erstellen im Portal

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.
3. Klicken Sie auf der Hauptseite auf **New Management group** (Neue Verwaltungsgruppe). 

    ![Erstellen einer Gruppe](media/management-groups/create_main.png) 
4.  Füllen Sie das Feld für die Verwaltungsgruppen-ID aus. 
    - Die **ID der Verwaltungsgruppe** ist der eindeutige Bezeichner des Verzeichnisses, der zum Übermitteln von Befehlen für diese Verwaltungsgruppe verwendet wird. Dieser Bezeichner kann nach der Erstellung nicht bearbeitet werden, da er im gesamten Azure-System zum Identifizieren dieser Gruppe verwendet wird. 
    - Im Feld für den Anzeigenamen wird der Name angegeben, der im Azure-Portal angezeigt wird. Beim Erstellen der Verwaltungsgruppe gibt es ein optionales Feld für einen separaten Anzeigenamen, der jederzeit geändert werden kann.  

    ![Erstellen](media/management-groups/create_context_menu.png)  
5.  Wählen Sie **Speichern** aus.


### <a name="create-in-powershell"></a>Erstellen in PowerShell
In PowerShell verwenden Sie die Cmdlets „Add-AzureRmManagementGroups“.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
**GroupName** ist ein eindeutiger Bezeichner, der erstellt wird. Diese ID wird von anderen Befehlen zum Verweisen auf diese Gruppe verwendet. Sie kann später nicht geändert werden.

Falls für die Verwaltungsgruppe im Azure-Portal ein anderer Name angezeigt werden soll, fügen Sie den Parameter **DisplayName** zur Zeichenfolge hinzu. Beispiel: Wenn Sie eine Verwaltungsgruppe mit dem Gruppennamen „Contoso“ und dem Anzeigenamen „Contoso Group“ erstellen möchten, verwenden Sie das folgende Cmdlet: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Verwenden Sie den Parameter **ParentId**, um diese Verwaltungsgruppe unter einer anderen Verwaltung zu erstellen.  

### <a name="create-in-azure-cli"></a>Erstellen in der Azure CLI
Verwenden Sie bei der Azure CLI den Befehl „az account management-group create“. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links: 
- [Organize your resources with Azure management groups ](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](management-groups-manage.md)
- [Installieren des Azure Powershell-Moduls](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Spezifikationen zur REST-API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

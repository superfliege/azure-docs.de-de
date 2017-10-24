---
title: Verwenden der rollenbasierten Zugriffssteuerung in Azure API Management | Microsoft-Dokumentation
description: Informationen zum Verwenden der integrierten Rollen und zum Erstellen benutzerdefinierter Rollen in Azure API Management
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: apimpm
ms.openlocfilehash: a3872aae3a9eb8da0b881ec9388f54546e84b08b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Verwenden der rollenbasierten Zugriffssteuerung in Azure API Management
Azure API Management basiert auf der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure, um eine differenzierte Zugriffsverwaltung für API Management-Dienste und -Entitäten (etwa APIs und Richtlinien) zu ermöglichen. Dieser Artikel bietet Ihnen einen Überblick über die integrierten und benutzerdefinierten Rollen in API Management. Weitere Informationen zur Zugriffsverwaltung im Azure-Portal finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Integrierte Rollen
API Management bietet zurzeit drei integrierte Rollen, und es werden in naher Zukunft zwei weitere Rollen hinzugefügt. Diese Rollen können auf verschiedenen Ebenen zugewiesen werden, darunter Abonnement, Ressourcengruppe und einzelne API Management-Instanz. Beispiel: Wenn einem Benutzer die Leserolle für den Azure API Management-Dienst (Azure API Management Service Reader) auf Ressourcengruppenebene zugewiesen wird, besitzt der Benutzer innerhalb der Ressourcengruppe Lesezugriff auf alle Instanzen von API Management. 

Die folgende Tabelle enthält kurze Beschreibungen der integrierten Rollen. Sie können diese Rollen über das Azure-Portal oder andere Tools zuweisen, beispielsweise Azure [PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), die [Azure CLI](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli) oder die [REST-API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Informationen zum Zuweisen integrierter Rollen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

| Rolle          | Lesezugriff<sup>[1]</sup> | Schreibzugriff<sup>[2]</sup> | Dienst erstellen, löschen, skalieren, Konfiguration von VPN und benutzerdefinierter Domäne | Zugriff auf das Legacy-Herausgeberportal | Beschreibung
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Mitwirkender des Azure API Management-Diensts | ✓ | ✓  | ✓  | ✓ | Administrator. Besitzt CRUD-Vollzugriff auf API Management-Dienste und -Entitäten (z.B. APIs, Richtlinien). Besitzt Zugriff auf das Legacy-Herausgeberportal. |
| Leser des Azure API Management-Diensts | ✓ | | || Besitzt schreibgeschützten Zugriff auf API Management-Dienste und -Entitäten. |
| Operator des Azure API Management-Diensts | ✓ | | ✓  | | Kann API Management-Dienste, jedoch keine Entitäten verwalten.|
| Editor des Azure API Management-Diensts<sup>*</sup> | ✓ | ✓  | |  | Kann API Management-Entitäten, jedoch keine Dienste verwalten.|
| Azure API Management-Inhalts-Manager<sup>*</sup> | ✓ | | | ✓  | Kann das Entwicklerportal verwalten. Schreibgeschützter Zugriff auf Dienste und Entitäten.|

<sup>[1] Lesezugriff auf API Management-Dienste und -Entitäten (z.B. APIs, Richtlinien)</sup>

<sup>[2] Schreibzugriff auf API Management-Dienste und -Entitäten mit Ausnahme der folgenden Vorgänge: Erstellen, Löschen und Skalieren von Instanzen; VPN-Konfiguration; Einrichten benutzerdefinierter Domänen</sup>

<sup>\* Die Rolle des Dienst-Editors steht zur Verfügung, nachdem die gesamte Administrator-UI vom vorhandenen Herausgeberportal zum Azure-Portal migriert wurde. Die Inhalts-Manager-Rolle ist verfügbar, sobald das Herausgeberportal so umgestaltet wurde, dass darin nur noch Funktionen im Zusammenhang mit der Verwaltung des Entwicklerportals enthalten sind.</sup>  

## <a name="custom-roles"></a>Benutzerdefinierte Rollen
Wenn keine der integrierten Rollen Ihre Anforderungen erfüllt, können benutzerdefinierte Rollen erstellt werden, um eine detailliertere Zugriffsverwaltung für API Management-Entitäten bereitzustellen. Beispielsweise können Sie eine benutzerdefinierte Rolle erstellen, die schreibgeschützten Zugriff auf einen API Management-Dienst, aber nur Schreibzugriff für eine bestimmte API besitzt. Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Erstellen von benutzerdefinierten Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles). 

Wenn Sie eine benutzerdefinierte Rolle erstellen, ist es einfacher, mit einer der integrierten Rollen zu beginnen. Bearbeiten Sie die Attribute, und fügen Sie **Actions**, **NotActions** oder **AssignableScopes** hinzu. Speichern Sie die Änderungen anschließend als neue Rolle. Im folgenden Beispiel wird von der Leserolle für den Azure API Management-Dienst ausgegangen und eine benutzerdefinierte Rolle namens „Calculator API Editor“ (Rechner-API-Editor) erstellt. Sie können die benutzerdefinierte Rolle einer bestimmten API zuweisen. Daher hat diese Rolle nur Zugriff auf diese API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

Der Artikel [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement) enthält die Liste der Berechtigungen, die auf API Management-Ebene gewährt werden können.

## <a name="watch-a-video-overview"></a>Überblicksvideo ansehen

Das Video [Role-Based Access Control in API Management](https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player) (Rollenbasierte Zugriffssteuerung in API Management) enthält weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure finden Sie in den folgenden Artikeln:
  * [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Erstellen von benutzerdefinierten Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles)
  * [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement)


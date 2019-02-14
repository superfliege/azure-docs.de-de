---
title: Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements | Microsoft Docs
description: Erfahren Sie, wie Sie einem Benutzer oder Dienstprinzipal ermöglichen können, programmgesteuert Azure Enterprise-Abonnements zu erstellen.
services: azure-resource-manager
author: adpick
manager: adpick
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/05/2018
ms.author: adpick
ms.openlocfilehash: 7a2397328f715dbf63246e8d4aaa789b5986b3b4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112562"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements (Vorschau)

Als Azure-Kunde mit einem [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) können Sie einem anderen Benutzer oder Dienstprinzipal die Berechtigung zum Erstellen von Abonnements erteilen, die Ihrem Konto in Rechnung gestellt werden. In diesem Artikel erfahren Sie, wie Sie [rollenbasierte Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-configure.md) verwenden können, um anderen Benutzern das Erstellen von Abonnements zu ermöglichen, und wie Sie die Erstellung von Abonnements überwachen können. Sie müssen für das Konto, das Sie freigeben möchten, über die Rolle „Besitzer“ verfügen.

Weitere Informationen zum Erstellen eines Abonnements finden Sie unter [Programmgesteuertes Erstellen von Azure Enterprise-Abonnements (Vorschau)](programmatically-create-subscription.md).

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Delegieren des Zugriffs auf ein Registrierungskonto mithilfe von RBAC

Um einem anderen Benutzer oder Dienstprinzipal die Möglichkeit zu geben, Abonnements für ein bestimmtes Konto zu erstellen, [weisen Sie ihm eine RBAC-Besitzerrolle im Bereich des Registrierungskontos zu](../active-directory/role-based-access-control-manage-access-rest.md). Im folgenden Beispiel wird einem Benutzer im Mandanten mit der `principalId` `<userObjectId>` (für SignUpEngineering@contoso.com) eine Besitzerrolle für das Registrierungskonto zugewiesen. Informationen zum ermitteln der Registrierungskonto-ID und der Prinzipal-ID finden Sie unter [Programmgesteuertes Erstellen von Azure Enterprise-Abonnements (Vorschau)](programmatically-create-subscription.md).

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>"
  }
}
```

Wenn die Besitzerrolle im Bereich des Anmeldekontos erfolgreich zugewiesen wurde, antwortet Azure mit Informationen zur Rollenzuweisung:

```json
{
  "properties": {
    "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
    "principalId": "<userObjectId>",
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "createdOn": "2018-03-05T08:36:26.4014813Z",
    "updatedOn": "2018-03-05T08:36:26.4014813Z",
    "createdBy": "<assignerObjectId>",
    "updatedBy": "<assignerObjectId>"
  },
  "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "<roleAssignmentGuid>"
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie [New-AzRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md), um einem anderen Benutzer Besitzerzugriff auf Ihr Registrierungskonto zu gewähren.

```azurepowershell-interactive
New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie [az role assignment create](../active-directory/role-based-access-control-manage-access-azure-cli.md), um einem anderen Benutzer Besitzerzugriff auf Ihr Registrierungskonto zu gewähren.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

----

Sobald ein Benutzer ein RBAC-Besitzer für Ihr Registrierungskonto ist, kann er darunter programmgesteuert Abonnements erstellen. Ein Abonnement, das von einem delegierten Benutzer erstellt wurde, weist weiterhin den ursprünglichen Kontobesitzer als Dienstadministrator auf, verfügt aber standardmäßig auch über den delegierten Benutzer als Besitzer. 

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Überwachen mit Aktivitätsprotokollen, wer Abonnements erstellt hat

Verwenden Sie zum Nachverfolgen der über diese API erstellten Abonnements die [Mandantenaktivitätsprotokoll-API](/rest/api/monitor/tenantactivitylogs). Zurzeit ist es nicht möglich, PowerShell, die CLI oder das Azure-Portal zum Nachverfolgen der Abonnementerstellung zu verwenden.

1. Als Mandantenadministrator des Azure AD-Mandanten [erhöhen Sie die Zugriffsrechte](../active-directory/role-based-access-control-tenant-admin-access.md), und weisen Sie dann dem überwachenden Benutzer eine Leserrolle über den Bereich `/providers/microsoft.insights/eventtypes/management` zu.
1. Rufen Sie als überwachender Benutzer die [Mandantenaktivitätsprotokoll-API](/rest/api/monitor/tenantactivitylogs) auf, um Aktivitäten zum Erstellen von Abonnements anzuzeigen. Beispiel:

```
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'" 
```

> [!NOTE]
> Um diese API bequem über die Befehlszeile aufzurufen, verwenden Sie [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Nächste Schritte

* Da der Benutzer oder Dienstprinzipal nun über die Berechtigung verfügt, ein Abonnement zu erstellen, können Sie diese Identität verwenden, um [programmgesteuert Azure Enterprise-Abonnements zu erstellen](programmatically-create-subscription.md).
* Ein Beispiel zum Erstellen von Abonnements mithilfe von .NET finden Sie im [Beispielcode auf GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Weitere Informationen zu Azure Resource Manager und den zugehörigen APIs finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).
* Einen umfassenden Leitfaden mit bewährten Methoden für große Organisationen zur Abonnementgovernance finden Sie unter [Azure Enterprise-Gerüst: Präskriptive Abonnementgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).

---
title: Programmgesteuertes Erstellen von Azure Enterprise-Abonnements | Microsoft Docs
description: Erfahren Sie, wie Sie zusätzliche Azure Enterprise- oder Enterprise Dev/Test-Abonnements programmgesteuert erstellen können.
services: azure-resource-manager
author: jlian
manager: jlian
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/18/2018
ms.author: jlian
ms.openlocfilehash: 8d495bf89697a5e14ff79953ab98f241ef8972e8
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programmgesteuertes Erstellen von Azure Enterprise-Abonnements (Vorschau)

Als Azure-Kunde mit [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) können Sie EA- (MS-AZR-0017P) und EA Dev/Test-Abonnements (MS-AZR-0148P) programmgesteuert erstellen. Um einem anderen Benutzer oder Dienstprinzipal die Berechtigung zum Erstellen von Abonnements zu erteilen, die über Ihr Konto abgerechnet werden, weisen Sie ihm [RBAC-Zugriff (rollenbasierte Zugriffssteuerung)](../active-directory/role-based-access-control-configure.md) auf Ihr Registrierungskonto zu. 

> [!IMPORTANT]
> Die über diese API erstellten Azure-Abonnements unterliegen der Vereinbarung, nach der Sie Microsoft Azure-Dienste von Microsoft oder einem autorisierten Handelspartner erhalten haben. Weitere Informationen finden Sie unter [Rechtliche Informationen zu Microsoft Azure](https://azure.microsoft.com/support/legal/).

In diesem Artikel werden die folgenden Themen behandelt:

> [!div class="checklist"]
> * Informationen zum programmgesteuerten Erstellen von Abonnements mithilfe von Azure Resource Manager
> * Grundlegendes zum Verwenden von RBAC zum Erstellen von Abonnements, die über Ihr EA-Konto abgerechnet werden

Der [.NET Beispielcode ist auf GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core) verfügbar.

## <a name="ask-your-ea-enrollment-admin-to-add-you-as-account-owner"></a>Hinzufügen Ihrer Person als Kontobesitzer durch Ihren EA-Registrierungsadministrator

Bitten Sie Ihren Registrierungsadministrator im ersten Schritt, [Sie als Kontobesitzer über das EA-Portal hinzuzufügen](https://ea.azure.com/helpdocs/addNewAccount) (Anmeldung erforderlich). Folgen Sie den Anweisungen in der Einladungs-E-Mail, die Sie erhalten, um manuell ein anfängliches Abonnement zu erstellen.

> [!IMPORTANT]
> Sie müssen den Kontobesitz bestätigen und manuell ein anfängliches EA-Abonnement erstellen, bevor Sie mit dem nächsten Schritt fortfahren können. Es reicht nicht aus, das Konto einfach zur Registrierung hinzuzufügen.

## <a name="find-accounts-you-have-access-to"></a>Ermitteln der Konten, auf die Sie Zugriff besitzen

Nachdem Sie einer Azure-EA-Registrierung als Kontobesitzer hinzugefügt wurden, verwendet Azure die Beziehung zwischen dem Konto und der Registrierung zum Ermitteln, wo die Abonnementgebühren angerechnet werden sollen. Zum Erstellen von Abonnements müssen Sie zunächst feststellen, auf welche Registrierungskonten Zugriff besteht. Wenn Sie zurzeit ein EA-Kontobesitzer sind und versuchen, diese API verwenden, überprüft Azure die folgenden Bedingungen:

- Ihr Konto wurde einer EA-Registrierung hinzugefügt.
- Sie verfügen über mindestens ein EA- oder EA Dev/Test-Abonnement, haben also den manuellen Registrierungsvorgang mindestens ein Mal durchlaufen.
- Sie sind im *Basisverzeichnis* des Kontobesitzers angemeldet. Dies ist das Verzeichnis, in dem Abonnements standardmäßig erstellt werden.

Wenn die oben genannten zwei Bedingungen erfüllt sind, wird eine `enrollmentAccount`-Ressource zurückgegeben, und Sie können mit dem Erstellen von Abonnements unter diesem Kontos beginnen. Alle Abonnements, die unter dem Konto erstellt werden, werden über die EA-Registrierung abgerechnet, in der sich das Konto befindet.

# <a name="resttabrest"></a>[REST](#tab/rest)

Anforderung zum Auflisten aller Registrierungskonten:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure gibt eine Liste aller Registrierungskonten zurück, auf die Sie zugreifen können:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileOnboardingEng@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileBackendEng@contoso.com"
      }
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Verwenden Sie den Befehl [Get-Get-AzureRmEnrollmentAccount](/powershell/module/azurerm.billing/get-azurermenrollmentaccount) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurepowershell-interactive
Get-AzureRmEnrollmentAccount
```

Azure gibt eine Liste der Objekt-IDs und E-Mail-Adressen von Konten zurück.

```azurepowershell
ObjectId                               | PrincipalName
<enrollmentAccountId>   | MobileOnboardingEng@contoso.com
<enrollmentAccountId>   | MobileBackendEng@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Verwenden Sie den Befehl [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurecli-interactive 
az billing enrollment-account list
```
Azure gibt eine Liste der Objekt-IDs und E-Mail-Adressen von Konten zurück.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileOnboardingEng@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
      "name": "<enrollmentAccountId>",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "MobileBackendEng@contoso.com"
      }
    }
  ]
}
```

---

Verwenden Sie die `principalName`-Eigenschaft zum Identifizieren des Kontos, in dem Abonnements abgerechnet werden sollen. Verwenden Sie die `id` als den `enrollmentAccount`-Wert, den Sie zum Erstellen des Abonnements im nächsten Schritt verwenden.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Erstellen von Abonnements unter einem bestimmten Registrierungskonto 

Das folgende Beispiel erstellt eine Anforderung zum Erstellen eines Abonnements namens *Dev Team Subscription*. Das Abonnementangebot lautet *MS-AZR-0017P* (reguläres EA-Abonnement). Das Registrierungskonto ist `<enrollmentAccountId>`, also das Registrierungskonto für MobileOnboardingEng@contoso.com. Es werden optional auch zwei Benutzer als RBAC-Besitzer für das Abonnement hinzugefügt.

# <a name="resttabrest"></a>[REST](#tab/rest)

Verwenden Sie die `id` des `enrollmentAccount` im Pfad der Anforderung, um das Abonnement zu erstellen.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Nein       | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `offerType`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owners`      | Nein        | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |

In der Antwort wird ein `subscriptionOperation`-Objekt für die Überwachung zurückgegeben. Nach Abschluss der Erstellung des Abonnements gibt das `subscriptionOperation`-Objekt ein `subscriptionLink`-Objekt zurück, das die Abonnement-ID enthält.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Um dieses Vorschaumodul zu verwenden, installieren Sie es zunächst durch Ausführen von `Install-Module AzureRM.Subscription -AllowPrerelease`. Um sicherzustellen, dass `-AllowPrerelease` funktioniert, installieren Sie eine aktuelle Version von PowerShellGet aus [PowerShellGet-Modul abrufen](/powershell/gallery/psget/get_psget_module).

Verwenden Sie [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview) zusammen mit dem `enrollmentAccount`-Namen als `EnrollmentAccountObjectId`-Parameter, um ein neues Abonnement zu erstellen. 

```azurepowershell-interactive
New-AzureRmSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountId> -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Nein       | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `OfferType`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `OwnerObjectId`      | Nein        | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `OwnerSignInName`    | Nein        | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden.|
| `OwnerApplicationId` | Nein        | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden.| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [New-AzureRmSubscription](/powershell/module/azurerm.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Um diese Vorschauerweiterung zu verwenden, installieren Sie sie zunächst durch Ausführen von `az extension add --name subscription`.

Verwenden Sie [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) zusammen mit dem `enrollmentAccount`-Namen als `enrollment_account_name`-Parameter, um ein neues Abonnement zu erstellen.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-name "<enrollmentAccountId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Nein       | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `offer-type`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `owner-object-id`      | Nein        | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `owner-upn`    | Nein        | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden.|
| `owner-spn` | Nein        | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden.| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="delegate-access-to-an-enrollment-account-using-rbac"></a>Delegieren des Zugriffs auf ein Registrierungskonto mithilfe von RBAC

Um einem anderen Benutzer oder Dienstprinzipal die Möglichkeit zu geben, Abonnements für ein bestimmtes Konto zu erstellen, [weisen Sie ihm eine RBAC-Besitzerrolle im Bereich des Registrierungskontos zu](../active-directory/role-based-access-control-manage-access-rest.md). Im folgenden Beispiel wird einem Benutzer im Mandanten mit der `principalId` `<userObjectId>` (für MobileOnboardingEng@contoso.com) eine Besitzerrolle für das Registrierungskonto zugewiesen. 

# <a name="resttabrest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

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
    "scope": "/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>",
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

Verwenden Sie [New-AzureRmRoleAssignment](../active-directory/role-based-access-control-manage-access-powershell.md), um einem anderen Benutzer Besitzerzugriff auf Ihr Registrierungskonto zu gewähren.

```azurepowershell-interactive
New-AzureRmRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Verwenden Sie [az role assignment create](../active-directory/role-based-access-control-manage-access-azure-cli.md), um einem anderen Benutzer Besitzerzugriff auf Ihr Registrierungskonto zu gewähren.

```azurecli-interactive 
az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountId>
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

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Einschränkungen der Azure Enterprise-Abonnementerstellungs-API

- Nur Azure Enterprise-Abonnements können mithilfe dieser API erstellt werden.
- Es sind maximal 50 Abonnements pro Konto möglich. Weitere Abonnements können nur mithilfe des Kontocenters erstellt werden.
- Es muss mindestens ein EA- oder EA Dev/Test-Abonnement unter dem Konto vorhanden sein. Dies bedeutet, dass der Kontobesitzer den manuellen Registrierungsvorgang mindestens ein Mal durchlaufen hat.
- Benutzer, die keine Kontobesitzer sind, jedoch einem Registrierungskonto über RBAC hinzugefügt wurden, können keine Abonnements mithilfe des Kontocenters erstellen.
- Sie können den Mandanten nicht auswählen, in dem das Abonnement erstellt werden soll. Das Abonnement wird immer im Basismandanten des Kontobesitzers erstellt. Wenn Sie das Abonnement in einen anderen Mandanten verschieben möchten, finden Sie weitere Informationen unter [Ändern des Abonnementmandanten](..\active-directory\active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Nächste Schritte

* Ein Beispiel zum Erstellen von Abonnements mithilfe von .NET finden Sie im [Beispielcode auf GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Weitere Informationen zu Azure Resource Manager und den zugehörigen APIs finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).
* Einen umfassenden Leitfaden mit bewährten Methoden für große Organisationen zur Abonnementgovernance finden Sie unter [Azure Enterprise-Gerüst: Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).

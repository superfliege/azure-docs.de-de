---
title: Programmgesteuertes Erstellen von Azure Enterprise-Abonnements | Microsoft Docs
description: Erfahren Sie, wie Sie zusätzliche Azure Enterprise- oder Enterprise Dev/Test-Abonnements programmgesteuert erstellen können.
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
ms.openlocfilehash: 1b772fdbda8e58db9414e09ef3ef7c98fc9f86b8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486978"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programmgesteuertes Erstellen von Azure Enterprise-Abonnements (Vorschau)

Als Azure-Kunde mit [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) können Sie EA- (MS-AZR-0017P) und EA Dev/Test-Abonnements (MS-AZR-0148P) programmgesteuert erstellen. Dieser Artikel enthält Informationen zum programmgesteuerten Erstellen von Abonnements mithilfe von Azure Resource Manager.

Wenn Sie über diese API Azure-Abonnements erstellen, unterliegen diese der Vereinbarung, nach der Sie Microsoft Azure-Dienste von Microsoft oder einem autorisierten Handelspartner erhalten haben. Weitere Informationen finden Sie unter [Rechtliche Informationen zu Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine Rolle „Besitzer“ oder „Mitwirkender“ in dem Registrierungskonto, unter dem Sie Abonnements erstellen möchten. Es gibt zwei Möglichkeiten, diese Rollen zu erhalten:

* Der Registrierungsadministrator kann [Sie zum Kontobesitzer machen](https://ea.azure.com/helpdocs/addNewAccount) (Anmeldung erforderlich), d.h. zum Besitzer des Registrierungskontos. Folgen Sie den Anweisungen in der Einladungs-E-Mail, die Sie erhalten, um manuell ein anfängliches Abonnement zu erstellen. Bestätigen Sie den Kontobesitz, und erstellen Sie manuell ein anfängliches EA-Abonnement, bevor Sie mit dem nächsten Schritt fortfahren. Es reicht nicht aus, das Konto einfach zur Registrierung hinzuzufügen.

* Ein vorhandener Besitzer des Registrierungskontos kann [Ihnen Zugriff gewähren](grant-access-to-create-subscription.md). Ebenso müssen Sie, wenn Sie einen Dienstprinzipal verwenden möchten, um das EA-Abonnement zu erstellen, [diesem Dienstprinzipal die Berechtigung zum Erstellen von Abonnements erteilen](grant-access-to-create-subscription.md).

## <a name="find-accounts-you-have-access-to"></a>Ermitteln der Konten, auf die Sie Zugriff besitzen

Nachdem Sie einer Azure-EA-Registrierung als Kontobesitzer hinzugefügt wurden, verwendet Azure die Beziehung zwischen dem Konto und der Registrierung zum Ermitteln, wo die Abonnementgebühren angerechnet werden sollen. Alle Abonnements, die unter dem Konto erstellt werden, werden über die EA-Registrierung abgerechnet, in der sich das Konto befindet. Um Abonnements zu erstellen, müssen Sie Werte zum Registrierungskonto und den Benutzerprinzipalen als Besitzer des Abonnements übergeben. 

Um die folgenden Befehle ausführen zu können, müssen Sie im *Basisverzeichnis* des Kontobesitzers angemeldet sein. Dies ist das Verzeichnis, in dem Abonnements standardmäßig erstellt werden.

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
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie das Cmdlet [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure gibt eine Liste der Objekt-IDs und E-Mail-Adressen von Konten zurück.

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den Befehl [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) zum Auflisten aller Registrierungskonten, auf die Sie zugreifen können.

```azurecli-interactive 
az billing enrollment-account list
```

Azure gibt eine Liste der Objekt-IDs und E-Mail-Adressen von Konten zurück.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

---

Verwenden Sie die `principalName`-Eigenschaft zum Identifizieren des Kontos, in dem Abonnements abgerechnet werden sollen. Verwenden Sie die `id` als den `enrollmentAccount`-Wert, den Sie zum Erstellen des Abonnements im nächsten Schritt verwenden.

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Erstellen von Abonnements unter einem bestimmten Registrierungskonto 

Das folgende Beispiel erstellt eine Anforderung zum Erstellen eines Abonnements namens *Dev Team Subscription*. Das Abonnementangebot lautet *MS-AZR-0017P* (reguläres EA-Abonnement). Das Registrierungskonto ist `747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx` (Platzhalterwert, dieser Wert ist eine GUID), also das Registrierungskonto für SignUpEngineering@contoso.com. Es werden optional auch zwei Benutzer als RBAC-Besitzer für das Abonnement hinzugefügt.

# <a name="resttabrest"></a>[REST](#tab/rest)

Verwenden Sie die `id` des `enrollmentAccount` im Pfad der Anforderung, um das Abonnement zu erstellen.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

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

Um dieses Vorschaumodul zu verwenden, installieren Sie es zunächst durch Ausführen von `Install-Module Az.Subscription -AllowPrerelease`. Um sicherzustellen, dass `-AllowPrerelease` funktioniert, installieren Sie eine aktuelle Version von PowerShellGet aus [PowerShellGet-Modul abrufen](/powershell/gallery/installing-psget).

Verwenden Sie [New-AzSubscription](/powershell/module/az.subscription) zusammen mit der Objekt-ID `enrollmentAccount` als Parameter `EnrollmentAccountObjectId`, um ein neues Abonnement zu erstellen. 

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId 747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx -OwnerObjectId <userObjectId>,<servicePrincipalObjectId>
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Nein       | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `OfferType`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `EnrollmentAccountObjectId`      | Ja       | Zeichenfolge | Die Objekt-ID des Registrierungskontos, unter dem das Abonnement erstellt und abgerechnet wird. Dieser Wert ist eine GUID, die Sie von `Get-AzEnrollmentAccount` abrufen. |
| `OwnerObjectId`      | Nein        | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `OwnerSignInName`    | Nein        | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden.|
| `OwnerApplicationId` | Nein        | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `OwnerObjectId` verwenden. Wenn Sie diesen Parameter verwenden zu können, benötigt das Dienstprinzipal [Lesezugriff auf das Verzeichnis](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [New-AzSubscription](/powershell/module/az.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um diese Vorschauerweiterung zu verwenden, installieren Sie sie zunächst durch Ausführen von `az extension add --name subscription`.

Verwenden Sie [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) zusammen mit der Objekt-ID `enrollmentAccount` als `enrollment-account-object-id`-Parameter, um ein neues Abonnement zu erstellen.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Elementname  | Erforderlich | Typ   | BESCHREIBUNG                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Nein       | Zeichenfolge | Der Anzeigename des Abonnements. Wenn kein Anzeigename angegeben wird, wird er auf den Namen des Angebots festgelegt, z.B. auf „Microsoft Azure Enterprise“.                                 |
| `offer-type`   | Ja      | Zeichenfolge | Das Angebot des Abonnements. Die beiden Optionen für EA sind [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (Verwendung in der Produktion) und [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (Dev/Test, muss [mit dem EA-Portal aktiviert werden](https://ea.azure.com/helpdocs/DevOrTestOffer)).                |
| `enrollment-account-object-id`      | Ja       | Zeichenfolge | Die Objekt-ID des Registrierungskontos, unter dem das Abonnement erstellt und abgerechnet wird. Dieser Wert ist eine GUID, die Sie von `az billing enrollment-account list` abrufen. |
| `owner-object-id`      | Nein        | Zeichenfolge | Die Objekt-ID eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird.  |
| `owner-upn`    | Nein        | Zeichenfolge | Die E-Mail-Adresse eines Benutzers, den Sie als RBAC Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden.|
| `owner-spn` | Nein        | Zeichenfolge | Die Anwendungs-ID eines Dienstprinzipals, den Sie als RBAC-Besitzer für das Abonnement hinzufügen möchten, wenn es erstellt wird. Sie können diesen Parameter anstelle von `owner-object-id` verwenden. Wenn Sie diesen Parameter verwenden zu können, benötigt das Dienstprinzipal [Lesezugriff auf das Verzeichnis](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole).| 

Wenn Sie eine vollständige Liste aller Parameter anzeigen möchten, finden Sie weitere Informationen unter [az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

----

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Einschränkungen der Azure Enterprise-Abonnementerstellungs-API

- Nur Azure Enterprise-Abonnements können mithilfe dieser API erstellt werden.
- Es sind maximal 50 Abonnements pro Konto möglich. Weitere Abonnements können nur mithilfe des Kontocenters erstellt werden.
- Es muss mindestens ein EA- oder EA Dev/Test-Abonnement unter dem Konto vorhanden sein. Dies bedeutet, dass der Kontobesitzer den manuellen Registrierungsvorgang mindestens ein Mal durchlaufen hat.
- Benutzer, die keine Kontobesitzer sind, jedoch einem Registrierungskonto über RBAC hinzugefügt wurden, können keine Abonnements mithilfe des Kontocenters erstellen.
- Sie können den Mandanten nicht auswählen, in dem das Abonnement erstellt werden soll. Das Abonnement wird immer im Basismandanten des Kontobesitzers erstellt. Wenn Sie das Abonnement in einen anderen Mandanten verschieben möchten, finden Sie weitere Informationen unter [Ändern des Abonnementmandanten](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Nächste Schritte

* Ein Beispiel zum Erstellen von Abonnements mithilfe von .NET finden Sie im [Beispielcode auf GitHub](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Da Sie nun ein Abonnement erstellt haben, können Sie diese Möglichkeit auch für andere Benutzer und Dienstprinzipale eröffnen. Weitere Informationen finden Sie unter [Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements (Vorschau) ](grant-access-to-create-subscription.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](management-groups-overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).

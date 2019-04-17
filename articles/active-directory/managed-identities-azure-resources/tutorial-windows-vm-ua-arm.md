---
title: Zugreifen auf Azure Resource Manager mithilfe einer benutzerseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers
description: In diesem Tutorial wird erläutert, wie Sie eine benutzerseitig zugewiesene verwaltete Identität auf einem virtuellen Windows-Computer verwenden, um auf Azure Resource Manager zuzugreifen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6fc7e13cf5ea3991f81d53edcabe0980d799cb0
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2019
ms.locfileid: "59520863"
---
# <a name="tutorial-use-a-user-assigned-managed-identity-on-a-windows-vm-to-access-azure-resource-manager"></a>Tutorial: Zugreifen auf Azure Resource Manager mithilfe einer benutzerseitig zugewiesenen verwalteten Identität auf einem virtuellen Windows-Computer

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine benutzerseitig zugewiesene Identität erstellen, diese einem virtuellen Windows-Computer zuweisen und anschließend verwenden, um auf die Azure Resource Manager-API zuzugreifen. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet. Sie ermöglichen die Authentifizierung von Diensten mit Unterstützung für die Azure AD-Authentifizierung, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer benutzerseitig zugewiesenen verwalteten Identität
> * Zuweisen einer benutzerseitig zugewiesenen Identität zu Ihrem virtuellen Windows-Computer
> * Gewähren des Zugriffs auf eine Ressourcengruppe in Azure Resource Manager für die benutzerseitig zugewiesene Identität 
> * Abrufen eines Zugriffstokens mithilfe der benutzerseitig zugewiesenen Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager 
> * Lesen der Eigenschaften einer Ressourcengruppe

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

- [Anmelden beim Azure-Portal](https://portal.azure.com)

- [Erstellen eines virtuellen Windows-Computers](/azure/virtual-machines/windows/quick-create-portal)

- Ihr Konto muss über die Berechtigung „Besitzer“ für den geeigneten Bereich (Ihr Abonnement oder Ihre Ressourcengruppe) verfügen, um die in diesem Tutorial erforderlichen Schritte für die Ressourcenerstellung und Rollenverwaltung durchführen zu können. Wenn Sie Unterstützung bei der Rollenzuweisung benötigen, finden Sie weitere Informationen unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](/azure/role-based-access-control/role-assignments-portal).
- [Installieren Sie die aktuelle Version des Azure PowerShell-Moduls.](/powershell/azure/install-az-ps) 
- Führen Sie zum Starten `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.
- Installieren Sie die [neueste Version von PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
- Führen Sie `Install-Module -Name PowerShellGet -AllowPrerelease` aus, um die Vorabversion des `PowerShellGet`-Moduls abzurufen (möglicherweise müssen Sie `Exit` in der aktuellen PowerShell-Sitzung ausführen, nachdem Sie diesen Befehl zum Installieren des `Az.ManagedServiceIdentity`-Moduls ausgeführt haben).
- Führen Sie `Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease` aus, um die Vorabversion des `Az.ManagedServiceIdentity`-Moduls zu installieren und die Vorgänge für benutzerseitig zugewiesene Identitäten in diesem Artikel auszuführen.

## <a name="create-a-user-assigned-identity"></a>Erstellen einer benutzerseitig zugewiesenen Identität

Eine benutzerseitig zugewiesene Identität wird als eigenständige Azure-Ressource erstellt. Mit [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/get-azuserassignedidentity) erstellt Azure eine Identität in Ihrem Azure AD-Mandanten, die einer oder mehreren Azure-Dienstinstanzen zugewiesen werden kann.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Die Antwort enthält Details zu der erstellten benutzerseitig zugewiesenen Identität, ähnlich dem folgenden Beispiel. Notieren Sie die Werte `Id` und `ClientId` für Ihre benutzerseitig zugewiesene Identität, da sie in den nachfolgenden Schritten verwendet werden:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Zuweisen der benutzerseitig zugewiesenen Identität zu einem virtuellen Windows-Computer

Eine benutzerseitig zugewiesene Identität kann von Clients für mehrere Azure-Ressourcen verwendet werden. Verwenden Sie die folgenden Befehle, um einem einzelnen virtuellen Computer die benutzerseitig zugewiesene Identität zuzuweisen. Verwenden Sie die Eigenschaft `Id`, die im vorherigen Schritt für den Parameter `-IdentityID` zurückgegeben wird.

```azurepowershell-interactive
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Gewähren des Zugriffs auf eine Ressourcengruppe in Azure Resource Manager für die benutzerseitig zugewiesene Identität 

Verwaltete Identitäten für Azure-Ressourcen stellen Identitäten dar, mit denen Ihr Code Zugriffstoken zur Authentifizierung bei Ressourcen-APIs anfordern kann, die die Azure AD-Authentifizierung unterstützen. In diesem Tutorial greift Ihr Code auf die Azure Resource Manager-API zu. 

Bevor Ihr Code auf die API zugreifen kann, müssen Sie der Identität Zugriff auf eine Ressource im Azure Resource Manager gewähren. In diesem Fall ist diese die Ressourcengruppe, in der die VM enthalten ist. Aktualisieren Sie abhängig von Ihrer Umgebung den Wert für `<SUBSCRIPTION ID>`.

```azurepowershell-interactive
$spID = (Get-AzUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Die Antwort enthält Details zu der erstellten Rollenzuweisung, ähnlich wie im folgenden Beispiel dargestellt wird:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwendung zum Aufrufen von Resource Manager 

Für den Rest des Tutorials arbeiten Sie von der VM aus, die wir zuvor erstellt haben.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

2. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zum virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**.

3. Geben Sie den **Benutzernamen** und das **Kennwort** ein, die Sie beim Erstellen der Windows-VM verwendet haben.

4. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt **PowerShell** in der Remotesitzung.

5. Erstellen Sie mithilfe des PowerShell-Befehls `Invoke-WebRequest` eine Anforderung an den lokalen Endpunkt für die verwalteten Identitäten für Azure-Ressourcen, um ein Zugriffstoken für Azure Resource Manager zu erhalten.  Der Wert `client_id` wurde zurückgegeben, als Sie [die benutzerseitig zugewiesene verwaltete Identität erstellt haben](#create-a-user-assigned-identity).

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=af825a31-b0e0-471f-baea-96de555632f9&resource=https://management.azure.com/' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Lesen der Eigenschaften einer Ressourcengruppe

Verwenden Sie das im vorherigen Schritt abgerufene Zugriffstoken, um auf Azure Resource Manager zuzugreifen, und lesen Sie die Eigenschaften der Ressourcengruppe, für die Sie der benutzerseitig zugewiesenen Identität den Zugriff gewährt haben. Ersetzen Sie `<SUBSCRIPTION ID>` durch die Abonnement-ID Ihrer Umgebung.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Die Antwort enthält die jeweiligen Informationen zur Ressourcengruppe, ähnlich wie im folgenden Beispiel dargestellt wird:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/myResourceGroupVM","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine benutzerseitig zugewiesene Identität erstellen und an einen virtuellen Azure-Computer anfügen, um auf die Azure Resource Manager-API zuzugreifen.  Weitere Informationen zu Azure Resource Manager finden Sie hier:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

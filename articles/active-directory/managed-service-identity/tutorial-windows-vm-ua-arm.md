---
title: Zugreifen auf den Azure Resource Manager mithilfe einer vom Benutzer zugewiesenen MSI einer Windows-VM
description: In diesem Tutorial wird erläutert, wie Sie eine vom Benutzer zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) auf einer Windows-VM verwenden, um auf den Azure Resource Manager zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: b81d4b669898a7acc428fe22a070ccd76dc5e546
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33932462"
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Zugreifen auf den Azure Resource Manager mithilfe einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) auf einer Windows-VM

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine vom Benutzer zugewiesene Identität erstellen, diese einem virtuellen Windows-Computer (VM) zuweisen und anschließend verwenden, um auf die Azure Resource Manager-API zuzugreifen. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet. Sie ermöglichen die Authentifizierung von Diensten mit Unterstützung für die Azure AD-Authentifizierung, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Windows-Computers 
> * Erstellen einer vom Benutzer zugewiesenen Identität
> * Zuweisen einer vom Benutzer zugewiesenen Identität zu Ihrer Windows-VM
> * Gewähren des Zugriffs auf eine Ressourcengruppe im Azure Resource Manager für die vom Benutzer zugewiesene Identität 
> * Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen Identität und Verwenden dieses Zugriffstokens zum Aufrufen des Azure Resource Manager 
> * Lesen der Eigenschaften einer Ressourcengruppe

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit der verwalteten Dienstidentität vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen vom System und vom Benutzer zugewiesenen Identitäten](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Ihr Konto muss über die Berechtigung „Besitzer“ für den geeigneten Bereich (Ihr Abonnement oder Ihre Ressourcengruppe) verfügen, um die in diesem Tutorial erforderlichen Schritte für die Ressourcenerstellung und Rollenverwaltung durchführen zu können. Wenn Sie Unterstützung bei der Rollenzuweisung benötigen, finden Sie weitere Informationen unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](/azure/role-based-access-control/role-assignments-portal).

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 5.7 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupVM* in der Region *EastUS* erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Nachdem die Ressourcengruppe erstellt wurde, erstellen Sie eine Windows-VM.

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```azurepowershell-interactive
$cred = Get-Credential
```
Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="create-a-user-assigned-identity"></a>Erstellen einer vom Benutzer zugewiesenen Identität

Eine durch den Benutzer zugewiesene Identität wird als eigenständige Azure-Ressource erstellt. Mit [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) erstellt Azure eine Identität in Ihrem Azure AD-Mandanten, die einer oder mehreren Azure-Dienstinstanzen zugewiesen werden kann.

> [!IMPORTANT]
> Für die Erstellung von Identitäten, die vom Benutzer zugewiesen werden, werden nur alphanumerische Zeichen und Bindestriche („0-9“, „a-Z“ bzw. „A-Z“ oder „-“) unterstützt. Darüber hinaus sollten Namen max. 24 Zeichen enthalten, damit die Zuordnung zur VM/VMSS ordnungsgemäß funktioniert. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen. Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Die Antwort enthält Details zu der erstellten vom Benutzer zugewiesenen Identität, ähnlich wie im folgenden Beispiel dargestellt wird. Notieren Sie sich den Wert `Id` Ihrer vom Benutzer zugewiesenen Identität, da er im nächsten Schritt verwendet wird:

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

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Zuweisen der vom Benutzer zugewiesenen Identität zu einer Windows-VM

Eine vom Benutzer zugewiesene Identität kann von Clients für mehrere Azure-Ressourcen verwendet werden. Verwenden Sie die folgenden Befehle, um einer einzelnen VM die vom Benutzer zugewiesene Identität zuzuweisen. Verwenden Sie die Eigenschaft `Id`, die im vorherigen Schritt für den Parameter `-IdentityID` zurückgegeben wird.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Gewähren des Zugriffs auf eine Ressourcengruppe im Azure Resource Manager für die vom Benutzer zugewiesene MSI 

Verwaltete Dienstidentitäten (Managed Service Identities, MSI) stellen Identitäten dar, mit denen Ihr Code Zugriffstoken zur Authentifizierung bei Ressourcen-APIs anfordern kann, die die Azure AD-Authentifizierung unterstützen. In diesem Tutorial greift Ihr Code auf die Azure Resource Manager-API zu. 

Bevor Ihr Code auf die API zugreifen kann, müssen Sie der Identität Zugriff auf eine Ressource im Azure Resource Manager gewähren. In diesem Fall ist diese die Ressourcengruppe, in der die VM enthalten ist. Aktualisieren Sie abhängig von Ihrer Umgebung den Wert für `<SUBSCRIPTION ID>`.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
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

5. Erstellen Sie mithilfe des PowerShell-Befehls `Invoke-WebRequest` eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für den Azure Resource Manager zu erhalten.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Lesen der Eigenschaften einer Ressourcengruppe

Verwenden Sie das im vorherigen Schritt abgerufene Zugriffstoken, um auf den Azure Resource Manager zuzugreifen, und lesen Sie die Eigenschaften der Ressourcengruppe, für die Sie der vom Benutzer zugewiesenen Identität den Zugriff gewährt haben. Ersetzen Sie <SUBSCRIPTION ID> durch die Abonnement-ID Ihrer Umgebung.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Die Antwort enthält die jeweiligen Informationen zur Ressourcengruppe, ähnlich wie im folgenden Beispiel dargestellt wird:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](overview.md).

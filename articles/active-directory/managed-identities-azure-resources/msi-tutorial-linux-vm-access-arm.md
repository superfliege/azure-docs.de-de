---
title: Zugreifen auf Azure Resource Manager mithilfe einer benutzerseitig zugewiesenen verwalteten Identität eines virtuellen Linux-Computers
description: In diesem Tutorial wird erläutert, wie Sie eine benutzerseitig zugewiesene verwaltete Identität auf einem virtuellen Linux-Computer verwenden, um auf Azure Resource Manager zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d9683c95c050826143e3e30931c808a5b464d7e5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431995"
---
# <a name="tutorial-use-a-user-assigned-managed-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Tutorial: Zugreifen auf Azure Resource Manager mithilfe einer benutzerseitig zugewiesenen verwalteten Identität auf einem virtuellen Linux-Computer

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine benutzerseitig zugewiesene verwaltete Identität erstellen, diese einem virtuellen Linux-Computer zuweisen und anschließend verwenden, um auf die Azure Resource Manager-API zuzugreifen. Verwaltete Identitäten für Azure-Ressourcen werden von Azure automatisch verwaltet. Sie ermöglichen die Authentifizierung von Diensten mit Unterstützung für die Azure AD-Authentifizierung, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer benutzerseitig zugewiesenen verwalteten Identität
> * Zuweisen der benutzerseitig zugewiesenen verwalteten Identität zu einem virtuellen Linux-Computer 
> * Gewähren des Zugriffs auf eine Ressourcengruppe in Azure Resource Manager für die benutzerseitig zugewiesene verwaltete Identität 
> * Abrufen eines Zugriffstokens mithilfe der benutzerseitig zugewiesenen verwalteten Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

- [Anmelden beim Azure-Portal](https://portal.azure.com)

- [Erstellen eines virtuellen Linux-Computers](/azure/virtual-machines/linux/quick-create-portal)

- Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

1. Wenn Sie (anstelle einer Azure Cloud Shell-Sitzung) die CLI-Konsole verwenden, melden Sie sich zunächst bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die neue benutzerseitig zugewiesene verwaltete Identität erstellen möchten:

    ```azurecli
    az login
    ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine benutzerseitig zugewiesene verwaltete Identität. Der Parameter `-g` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene verwaltete Identität erstellt wird, und der Parameter `-n` gibt den Namen an. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<UAMI NAME>` durch Ihre eigenen Werte:
    
[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <UAMI NAME>
```

Die Antwort enthält Details zu der erstellten benutzerseitig zugewiesenen verwalteten Identität, ähnlich dem folgenden Beispiel. Notieren Sie sich den Wert `id` Ihrer benutzerseitig zugewiesenen verwalteten Identität, da er im nächsten Schritt verwendet wird:

```json
{
"clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
"clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<UAMI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
"id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<UAMI NAME>",
"location": "westcentralus",
"name": "<UAMI NAME>",
"principalId": "9012",
"resourceGroup": "<RESOURCE GROUP>",
"tags": {},
"tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
"type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

## <a name="assign-a-user-assigned-managed-identity-to-your-linux-vm"></a>Zuweisen einer benutzerseitig zugewiesenen verwalteten Identität zu einem virtuellen Linux-Computer

Eine benutzerseitig zugewiesene verwaltete Identität kann von Clients für mehrere Azure-Ressourcen verwendet werden. Verwenden Sie die folgenden Befehle, um einem einzelnen virtuellen Computer die benutzerseitig zugewiesene verwaltete Identität zuzuweisen. Verwenden Sie die Eigenschaft `Id`, die im vorherigen Schritt für den Parameter `-IdentityID` zurückgegeben wird.

Weisen Sie mit [az vm identity assign](/cli/azure/vm#az-vm-identity-assign) Ihrem virtuellen Linux-Computer die benutzerseitig zugewiesene verwaltete Identität zu. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. Verwenden Sie die Eigenschaft `id`, die im vorherigen Schritt für den Parameterwert `--identities` zurückgegeben wird.

```azurecli-interactive
az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<UAMI NAME>"
```

## <a name="grant-your-user-assigned-managed-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Gewähren des Zugriffs auf eine Ressourcengruppe in Azure Resource Manager für die benutzerseitig zugewiesene verwaltete Identität 

Verwaltete Identitäten für Azure-Ressourcen stellen Identitäten dar, mit denen Ihr Code Zugriffstoken zur Authentifizierung bei Ressourcen-APIs anfordern kann, die die Azure AD-Authentifizierung unterstützen. In diesem Tutorial greift Ihr Code auf die Azure Resource Manager-API zu.  

Bevor Ihr Code auf die API zugreifen kann, müssen Sie der Identität Zugriff auf eine Ressource im Azure Resource Manager gewähren. In diesem Fall ist diese die Ressourcengruppe, in der die VM enthalten ist. Aktualisieren Sie abhängig von Ihrer Umgebung die Werte für `<SUBSCRIPTION ID>` und `<RESOURCE GROUP>`. Ersetzen Sie `<UAMI PRINCIPALID>` durch die Eigenschaft `principalId`, die unter [Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](#create-a-user-assigned-managed-identity) vom Befehl `az identity create` zurückgegeben wurde:

```azurecli-interactive
az role assignment create --assignee <UAMI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

Die Antwort enthält Details zu der erstellten Rollenzuweisung, ähnlich wie im folgenden Beispiel dargestellt wird:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwendung zum Aufrufen von Resource Manager 

Für den Rest des Tutorials arbeiten wir von dem virtuellen Computer aus, den wir zuvor erstellt haben.

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. 

1. Melden Sie sich beim Azure-[Portal](https://portal.azure.com) an.
2. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zum virtuellen Linux-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**. Kopieren Sie die Zeichenfolge, um eine Verbindung mit Ihrem virtuellen Computer herzustellen.
3. Stellen Sie eine Verbindung zwischen der VM und dem gewünschten SSH-Client her. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, lesen Sie die Informationen unter [Vorgehensweise: Verwenden von SSH-Schlüsseln mit Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
4. Erstellen Sie im Terminalfenster mit CURL eine Anforderung an den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) zum Abrufen eines Zugriffstokens für den Azure Resource Manager.  

   Im folgenden Beispiel wird die cURL-Anforderung zum Abrufen eines Zugriffstokens angezeigt. Ersetzen Sie `<CLIENT ID>` durch die Eigenschaft `clientId`, die unter [Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](#create-a-user-assigned-managed-identity) vom Befehl `az identity create` zurückgegeben wurde: 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<UAMI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > Der Wert des `resource`-Parameters muss exakt mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden. 
    
    Die Antwort enthält das Zugriffstoken, das Sie für den Zugriff auf Azure Resource Manager benötigen. 
    
    Beispielantwort:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

5. Verwenden Sie dieses Zugriffstoken, um auf Azure Resource Manager zuzugreifen und die Eigenschaften der Ressourcengruppe zu lesen, für die Sie der benutzerseitig zugewiesenen verwalteten Identität zuvor den Zugriff gewährt haben. Ersetzen Sie `<SUBSCRIPTION ID>` und `<RESOURCE GROUP>` durch die Werte, die Sie zuvor angegeben haben, und `<ACCESS TOKEN>` durch den im vorherigen Schritt zurückgegebenen Token.

    > [!NOTE]
    > Bei der URL wird die Groß-/Kleinschreibung beachtet. Achten Sie daher darauf, dieselbe Groß- und Kleinschreibung zu verwenden wie zuvor beim Benennen der Ressourcengruppe, einschließlich des Großbuchstaben „G“ in `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Die Antwort enthält die jeweiligen Informationen zur Ressourcengruppe, ähnlich wie im folgenden Beispiel dargestellt wird: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine benutzerseitig zugewiesene verwaltete Identität erstellen und an einen virtuellen Linux-Computer anfügen, um auf die Azure Resource Manager-API zuzugreifen.  Weitere Informationen zu Azure Resource Manager finden Sie hier:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)


---
title: Zugreifen auf Azure Resource Manager mithilfe einer vom Benutzer zugewiesenen verwalteten Dienstidentität eines virtuellen Linux-Computers
description: In diesem Tutorial wird erläutert, wie Sie eine vom Benutzer zugewiesene verwaltete Dienstidentität auf einem virtuellen Linux-Computer verwenden, um auf Azure Resource Manager zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
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
ms.openlocfilehash: 98be87d62861295536a75201ff93e809c9ba120b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886851"
---
# <a name="tutorial-use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Tutorial: Zugreifen auf den Azure Resource Manager mithilfe einer vom Benutzer zugewiesenen Identität auf einem virtuellen Linux-Computer

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine vom Benutzer zugewiesene Identität erstellen, diese einem virtuellen Linux-Computer (VM) zuweisen und anschließend verwenden, um auf die Azure Resource Manager-API zuzugreifen. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet. Sie ermöglichen die Authentifizierung von Diensten mit Unterstützung für die Azure AD-Authentifizierung, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet. Sie ermöglichen die Authentifizierung von Diensten mit Unterstützung für die Azure AD-Authentifizierung, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen.

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer vom Benutzer zugewiesenen Identität
> * Zuweisen der vom Benutzer zugewiesenen Identität zu einer Linux-VM 
> * Gewähren des Zugriffs auf eine Ressourcengruppe im Azure Resource Manager für die vom Benutzer zugewiesene Identität 
> * Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen Identität und Verwenden dieses Zugriffstokens zum Aufrufen des Azure Resource Manager 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[Anmelden beim Azure-Portal](https://portal.azure.com)

[Erstellen eines virtuellen Linux-Computers](/azure/virtual-machines/linux/quick-create-portal)

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-user-assigned-identity"></a>Erstellen einer vom Benutzer zugewiesenen Identität

1. Wenn Sie (anstelle einer Azure Cloud Shell-Sitzung) die CLI-Konsole verwenden, melden Sie sich zunächst bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die neue vom Benutzer zugewiesene Identität erstellen möchten:

    ```azurecli
    az login
    ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine vom Benutzer zugewiesene Identität. Mit dem Parameter `-g` wird die Ressourcengruppe angegeben, in der die verwaltete Dienstidentität erstellt wird, und mit dem Parameter `-n` wird ihr Name angegeben. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <MSI NAME>
```

Die Antwort enthält Details zu der erstellten vom Benutzer zugewiesenen Identität, ähnlich wie im folgenden Beispiel dargestellt wird. Notieren Sie sich den Wert `id` Ihrer vom Benutzer zugewiesenen Identität, da er im nächsten Schritt verwendet wird:

```json
{
"clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
"clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
"id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
"location": "westcentralus",
"name": "<MSI NAME>",
"principalId": "9012",
"resourceGroup": "<RESOURCE GROUP>",
"tags": {},
"tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
"type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Zuweisen einer vom Benutzer zugewiesenen Identität zu Ihrer Linux-VM

Eine vom Benutzer zugewiesene Identität kann von Clients für mehrere Azure-Ressourcen verwendet werden. Verwenden Sie die folgenden Befehle, um einer einzelnen VM die vom Benutzer zugewiesene Identität zuzuweisen. Verwenden Sie die Eigenschaft `Id`, die im vorherigen Schritt für den Parameter `-IdentityID` zurückgegeben wird.

Weisen Sie mit [az vm assign-identity](/cli/azure/vm#az-vm-assign-identity) die vom Benutzer zugewiesene verwaltete Dienstidentität Ihrem virtuellen Linux-Computer zu. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. Verwenden Sie die Eigenschaft `id`, die im vorherigen Schritt für den Parameterwert `--identities` zurückgegeben wird.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Gewähren des Zugriffs auf eine Ressourcengruppe im Azure Resource Manager für die vom Benutzer zugewiesene Identität 

Verwaltete Dienstidentitäten (Managed Service Identities, MSI) stellen Identitäten dar, mit denen Ihr Code Zugriffstoken zur Authentifizierung bei Ressourcen-APIs anfordern kann, die die Azure AD-Authentifizierung unterstützen. In diesem Tutorial greift Ihr Code auf die Azure Resource Manager-API zu.  

Bevor Ihr Code auf die API zugreifen kann, müssen Sie der Identität Zugriff auf eine Ressource im Azure Resource Manager gewähren. In diesem Fall ist diese die Ressourcengruppe, in der die VM enthalten ist. Aktualisieren Sie abhängig von Ihrer Umgebung die Werte für `<SUBSCRIPTION ID>` und `<RESOURCE GROUP>`. Ersetzen Sie `<MSI PRINCIPALID>` durch die Eigenschaft `principalId`, die unter [Erstellen einer vom Benutzer zugewiesenen MSI](#create-a-user-assigned-msi) vom Befehl `az identity create` zurückgegeben wurde:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
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

   Im folgenden Beispiel wird die cURL-Anforderung zum Abrufen eines Zugriffstokens angezeigt. Ersetzen Sie `<CLIENT ID>` durch die Eigenschaft `clientId`, die unter [Erstellen einer vom Benutzer zugewiesenen Identität](#create-a-user-assigned-msi) vom Befehl `az identity create` zurückgegeben wurde: 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > Der Wert des `resource`-Parameters muss exakt mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden. 
    
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

5. Verwenden Sie dieses Zugriffstoken, um auf den Azure Resource Manager zuzugreifen und die Eigenschaften der Ressourcengruppe zu lesen, für die Sie der vom Benutzer zugewiesenen Identität zuvor den Zugriff gewährt haben. Ersetzen Sie `<SUBSCRIPTION ID>` und `<RESOURCE GROUP>` durch die Werte, die Sie zuvor angegeben haben, und `<ACCESS TOKEN>` durch den im vorherigen Schritt zurückgegebenen Token.

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

In diesem Tutorial haben Sie erfahren, wie Sie eine vom Benutzer zugewiesene Identität erstellen und an einen virtuellen Linux-Computer anfügen, um auf die Azure Resource Manager-API zuzugreifen.  Weitere Informationen zu Azure Resource Manager finden Sie hier:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)


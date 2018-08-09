---
title: Konfigurieren von system- und benutzerzugewiesenen Identitäten auf einem virtuellen Azure-Computer mit der Azure CLI
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten auf einem virtuellen Azure-Computer mit der Azure CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: e12cc37c579c10d3b59197d126589d36e80a8451
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444520"
---
# <a name="configure-managed-service-identity-on-an-azure-vm-using-azure-cli"></a>Konfigurieren der verwalteten Dienstidentität auf einem virtuellen Azure-Computer mit der Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mit der Azure CLI die folgenden „Verwaltete Dienstidentität“-Vorgänge (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer (Azure-VM) ausführen können:
- Aktivieren und Deaktivieren der systemzugewiesenen Identität auf einem virtuellen Azure-Computer
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität auf einem virtuellen Azure-Computer

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität für eine Azure-VM zu aktivieren bzw. zu entfernen.
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen.
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM zuzuweisen bzw. davon zu entfernen.
- Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:
    - Verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
    - Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
    - [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie die systemzugewiesene Identität auf einem virtuellen Azure-Computer mit der Azure CLI aktivieren und deaktivieren können.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Aktivieren einer systemzugewiesenen Identität beim Erstellen eines virtuellen Azure-Computers

So erstellen Sie einen virtuellen Azure-Computer mit aktivierter systemzugewiesener Identität:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Erstellen Sie eine [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen des virtuellen Computers und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az-group-create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm/#az-vm-create). Im folgenden Beispiel wird ein virtueller Computer namens *myVM* mit einer systemzugewiesenen Identität erstellt, wie dies vom `--assign-identity`-Parameter gefordert wird. Der `--admin-username`-Parameter und der `--admin-password`-Parameter geben den Namen und das Kennwort des Administratorbenutzers für die Anmeldung am virtuellen Computer an. Aktualisieren Sie diese Werte ggf. mit den entsprechenden Werten für Ihre Umgebung: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Aktivieren der systemzugewiesenen Identität auf einem vorhandenen virtuellen Azure-Computer

Wenn Sie die systemzugewiesene Identität auf einem vorhandenen virtuellen Computer aktivieren müssen, gehen Sie wie folgt vor:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie [az vmss identity assign](/cli/azure/vm/identity/#az-vm-identity-assign) mit dem `identity assign`-Befehl, um die systemzugewiesene Identität auf einem vorhandenen virtuellen Computer zu aktivieren:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Deaktivieren der systemzugewiesenen Identität von einem virtuellen Azure-Computer

Wenn Sie über einen virtuellen Computer verfügen, der nicht mehr die systemzugewiesene Identität, jedoch weiterhin benutzerzugewiesene Identitäten benötigt, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Bei einem virtuellen Computer, der nicht mehr die vom System zugewiesene Identität benötigt und über keine vom Benutzer zugewiesenen Identitäten verfügt, verwenden Sie den folgenden Befehl:

> [!NOTE]
> Für den Wert `none` müssen Sie die Groß-/Kleinschreibung beachten. Er darf nur aus Kleinbuchstaben bestehen. 

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none"
```

Um die VM-Erweiterung der verwalteten Dienstidentität zu entfernen, verwenden Sie den Schalter `-n ManagedIdentityExtensionForWindows` oder `-n ManagedIdentityExtensionForLinux` (abhängig vom Typ des virtuellen Computers) mit [az vm extension delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe der Azure CLI eine benutzerzugewiesene Identität zu einem virtuellen Azure-Computer hinzufügen oder aus ihm entfernen.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Zuweisen einer benutzerzugewiesenen Identität beim Erstellen eines virtuellen Azure-Computers

Dieser Abschnitt führt Sie durch das Erstellen eines virtuellen Computers samt Zuweisung einer benutzerzugewiesenen Identität. Wenn Sie bereits über eine VM verfügen, die Sie verwenden möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit dem nächsten Abschnitt fort.

1. Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie verwenden möchten. Erstellen Sie mit [az group create](/cli/azure/group/#az-group-create) eine [Ressourcengruppe](~/articles/azure-resource-manager/resource-group-overview.md#terminology) zum Einschließen und Bereitstellen Ihrer verwalteten Dienstidentität. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<LOCATION>` durch Ihre eigenen Werte. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine benutzerzugewiesene Identität.  Mit dem Parameter `-g` wird die Ressourcengruppe angegeben, in der die benutzerzugewiesene Identität erstellt wird, und mit dem Parameter `-n` wird deren Name angegeben.    
    
   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g myResourceGroup -n myUserAssignedIdentity
   ```
   Die Antwort enthält Details zu der erstellten benutzerzugewiesenen Identität, ähnlich dem folgenden Beispiel. Der Ressourcen-ID-Wert, der der benutzerzugewiesenen Identität zugewiesen ist, wird im nächsten Schritt verwendet.

   ```json
   {
       "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
       "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
       "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
       "location": "westcentralus",
       "name": "<MSI NAME>",
       "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
       "resourceGroup": "<RESOURCE GROUP>",
       "tags": {},
       "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
       "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm/#az-vm-create). Im folgenden Beispiel wird ein virtueller Computer erstellt, der mit der neuen benutzerzugewiesenen Identität verknüpft ist, wie dies durch den Parameter `--assign-identity` angegeben ist. Ersetzen Sie die Parameter`<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` und `<MSI ID>` durch Ihre eigenen Werte. Verwenden Sie für `<MSI ID>` die Ressourcen-`id`-Eigenschaft der benutzerzugewiesenen Identität, die im vorherigen Schritt erstellt wurde: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Zuweisen einer benutzerzugewiesenen Identität zu einer vorhandenen Azure-VM

1. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine benutzerzugewiesene Identität.  Mit dem Parameter `-g` wird die Ressourcengruppe angegeben, in der die benutzerzugewiesene Identität erstellt wird, und mit dem Parameter `-n` wird deren Name angegeben. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:

    > [!IMPORTANT]
    > Das Erstellen einer benutzerzugewiesenen Identität mit Sonderzeichen (d. h. Unterstrich) im Namen wird derzeit nicht unterstützt. Verwenden Sie alphanumerische Zeichen. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.  Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Die Antwort enthält Details zu der erstellten verwalteten Identität, ähnlich dem folgenden Beispiel. Der Ressourcen-`id`-Wert, der der benutzerzugewiesenen Identität zugewiesen ist, wird im nächsten Schritt verwendet.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Verwenden Sie [az vm identity assign](/cli/azure/vm#az-vm-identity-assign), um Ihrem virtuellen Computer die benutzerzugewiesene Identität zuzuweisen. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. Die `<MSI ID>` ist die im vorherigen Schritt erstellte Ressourcen-`id`-Eigenschaft der benutzerzugewiesenen Identität:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Entfernen einer benutzerzugewiesenen Identität von einem virtuellen Azure-Computer

Um eine vom Benutzer zugewiesene Identität aus einer VM zu entfernen, verwenden Sie [az vm identity remove](/cli/azure/vm#az-vm-identity-remove). Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. `<MSI NAME>` ist die `name`-Eigenschaft der benutzerzugewiesen Identität. Diese Eigenschaft kann mit `az vm identity show` aus dem Identitätsabschnitt des virtuellen Computers abgerufen werden:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```

Wenn Ihre VM keine vom System zugewiesene Identität hat, und Sie alle vom Benutzer zugewiesenen Identitäten entfernen möchten, verwenden Sie den folgenden Befehl:

> [!NOTE]
> Für den Wert `none` müssen Sie die Groß-/Kleinschreibung beachten. Er darf nur aus Kleinbuchstaben bestehen.

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Wenn Ihr virtueller Computer sowohl system- als auch benutzerzugewiesene Identitäten hat, können Sie alle benutzerzugewiesene Identitäten entfernen, indem Sie in den Modus wechseln, in dem nur die systemzugewiesene Identität verwendet wird. Verwenden Sie den folgenden Befehl:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>Verwandte Inhalte
- [Übersicht über verwaltete Dienstidentitäten](overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter: 
  - [Erstellen eines virtuellen Windows-Computers mit der CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Erstellen eines virtuellen Linux-Computers mit der CLI](../../virtual-machines/linux/quick-create-cli.md) 


















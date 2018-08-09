---
title: Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mit der Azure CLI
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mit der Azure CLI.
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
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 6474b34abeceb58c2eff9e7a2d2237ec47e61933
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447522"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Konfigurieren einer MSI (Managed Service Identity, verwaltete Dienstidentität) für eine VM-Skalierungsgruppe mit der Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mit der Azure CLI die folgenden Vorgänge für verwaltete Dienstidentitäten (Managed Service Identity, MSI) für eine Azure VM-Skalierungsgruppe (Virtual Machine Scale Set, VMSS) ausführen können:
- Aktivieren und Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität in einer Azure VM-Skalierungsgruppe


## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM-Skalierungsgruppe zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität auf einer VM-Skalierungsgruppe zu aktivieren bzw. daraus zu entfernen.
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen.
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM-Skalierungsgruppe zuzuweisen bzw. daraus zu entfernen.
- Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:
    - Verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
    - Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
    - [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie die systemzugewiesene Identität für eine Azure VM-Skalierungsgruppe mit der Azure CLI aktivieren und deaktivieren können.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität bei der Erstellung einer Azure VM-Skalierungsgruppe

So erstellen Sie eine VM-Skalierungsgruppe samt aktivierter systemzugewiesener Identität:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die VM-Skalierungsgruppe bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Erstellen Sie eine [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen der VM-Skalierungsgruppe und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az-group-create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Erstellen Sie mit [az vmss create](/cli/azure/vmss/#az-vmss-create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine VM-Skalierungsgruppe namens *myVMSS* mit einer systemzugewiesenen Identität erstellt, wie dies durch den Parameter `--assign-identity` gefordert wird. Der `--admin-username`-Parameter und der `--admin-password`-Parameter geben den Namen und das Kennwort des Administratorbenutzers für die Anmeldung am virtuellen Computer an. Aktualisieren Sie diese Werte ggf. mit den entsprechenden Werten für Ihre Umgebung: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität in einer vorhandenen Azure VM-Skalierungsgruppe

Wenn Sie die systemzugewiesene Identität in einer vorhandenen Azure VM-Skalierungsgruppe aktivieren müssen:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, das die VM-Skalierungsgruppe enthält.

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie den Befehl [az vmss identity assign](/cli/azure/vmss/identity/#az-vmss-identity-assign), um eine systemzugewiesene Identität für einen vorhandenen virtuellen Computer zu aktivieren:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

Wenn Sie über eine VM-Skalierungsgruppe verfügen, die nicht mehr die vom System zugewiesene Identität, jedoch weiterhin vom Benutzer zugewiesene Identitäten benötigt, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Bei einem virtuellen Computer, der nicht mehr die vom System zugewiesene Identität benötigt und über keine vom Benutzer zugewiesene Identitäten verfügt, verwenden Sie den folgenden Befehl:

> [!NOTE]
> Für den Wert `none` müssen Sie die Groß-/Kleinschreibung beachten. Er darf nur aus Kleinbuchstaben bestehen. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

Möchten Sie die MSI-Erweiterung für virtuelle Computer entfernen, verwenden Sie den Befehl [az vmss identity remove](/cli/azure/vmss/identity/#az-vmss-remove-identity), um die systemzugewiesene Identität aus einer VM-Skalierungsgruppe zu entfernen:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie mithilfe der Azure CLI eine benutzerzugewiesene Identität aktivieren und entfernen.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Zuweisen einer benutzerzugewiesenen Identität beim Erstellen einer Azure VM-Skalierungsgruppe

Dieser Abschnitt führt Sie durch das Erstellen einer VM-Skalierungsgruppe und das Zuweisen einer benutzerzugewiesenen Identität zu der VM-Skalierungsgruppe. Wenn Sie bereits eine VM-Skalierungsgruppe (VMSS) haben, die Sie verwenden möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit dem nächsten Abschnitt fort.

1. Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie verwenden möchten. Erstellen Sie mit [az group create](/cli/azure/group/#az-group-create) eine [Ressourcengruppe](~/articles/azure-resource-manager/resource-group-overview.md#terminology) zum Einschließen und Bereitstellen Ihrer benutzerzugewiesenen Identität. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<LOCATION>` durch Ihre eigenen Werte. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine benutzerzugewiesene Identität.  Mit dem Parameter `-g` wird die Ressourcengruppe angegeben, in der die benutzerzugewiesene Identität erstellt wird, und mit dem Parameter `-n` wird deren Name angegeben. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Die Antwort enthält Details zu der erstellten benutzerzugewiesenen Identität, ähnlich dem folgenden Beispiel. Der Ressourcen-`id`-Wert, der der benutzerzugewiesenen Identität zugewiesen ist, wird im nächsten Schritt verwendet.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Erstellen Sie mit [az vmss create](/cli/azure/vmss/#az-vmss-create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine VM-Skalierungsgruppe erstellt, die mit der neuen benutzerzugewiesenen Identität verknüpft ist, wie dies durch den Parameter `--assign-identity` angegeben ist. Ersetzen Sie die Parameter`<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` und `<USER ASSIGNED IDENTITY ID>` durch Ihre eigenen Werte. Verwenden Sie für `<USER ASSIGNED IDENTITY ID>` die Ressourcen-`id`-Eigenschaft der benutzerzugewiesenen Identität, die im vorherigen Schritt erstellt wurde: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Zuweisen einer benutzerzugewiesenen Identität zu einer vorhandenen Azure-VM

1. Erstellen Sie mit [az identity create](/cli/azure/identity#az-identity-create) eine benutzerzugewiesene Identität.  Mit dem Parameter `-g` wird die Ressourcengruppe angegeben, in der die benutzerzugewiesene Identität erstellt wird, und mit dem Parameter `-n` wird deren Name angegeben. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

    > [!IMPORTANT]
    > Das Erstellen einer benutzerzugewiesenen Identität mit Sonderzeichen (d. h. Unterstrich) im Namen wird derzeit nicht unterstützt. Verwenden Sie alphanumerische Zeichen. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.  Weitere Informationen finden Sie unter [FAQs und bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory](known-issues.md).

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Die Antwort enthält Details zu der erstellten benutzerzugewiesenen Identität, ähnlich dem folgenden Beispiel. Der Ressourcen-`id`-Wert, der der benutzerzugewiesenen Identität zugewiesen ist, wird im nächsten Schritt verwendet.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Verwenden Sie [az vmss identity assign](/cli/azure/vmss/identity#az-vm-assign-identity), um Ihrer VM-Skalierungsgruppe die benutzerzugewiesene Identität zuzuweisen. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VMSS NAME>` durch Ihre eigenen Werte. Die `<USER ASSIGNED IDENTITY ID>` ist die im vorherigen Schritt erstellte Ressourcen-`id`-Eigenschaft der benutzerzugewiesenen Identität:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Entfernen einer vom Benutzer zugewiesenen Identität aus einer Azure-VM-Skalierungsgruppe

Zum Entfernen einer vom Benutzer zugewiesenen Identität aus einer VM-Skalierungsgruppe verwenden Sie [az vmss identity remove](/cli/azure/vmss/identity#az-vmss-identity-remove). Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VMSS NAME>` durch Ihre eigenen Werte. `<MSI NAME>` ist die `name`-Eigenschaft der benutzerzugewiesen Identität. Diese Eigenschaft kann mit `az vmss identity show` aus dem Identitätsabschnitt des virtuellen Computers abgerufen werden:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

Wenn Ihre VM-Skalierungsgruppe keine vom System zugewiesene Identität hat und Sie alle vom Benutzer zugewiesenen Identitäten entfernen möchten, verwenden Sie den folgenden Befehl:

> [!NOTE]
> Für den Wert `none` müssen Sie die Groß-/Kleinschreibung beachten. Er darf nur aus Kleinbuchstaben bestehen.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Wenn die VM-Skalierungsgruppe sowohl vom System als auch vom Benutzer zugewiesene Identitäten aufweist, können Sie alle vom Benutzer zugewiesenen Identitäten entfernen, indem Sie in den Modus wechseln, in dem nur die vom System zugewiesene Identität verwendet wird. Verwenden Sie den folgenden Befehl:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über verwaltete Dienstidentitäten](overview.md)
- Die vollständige Schnellstartanleitung für die Erstellung einer Azure-VM-Skalierungsgruppe finden Sie hier: 

  - [Erstellen einer VM-Skalierungsgruppe und Bereitstellen einer hoch verfügbaren App unter Linux](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)


















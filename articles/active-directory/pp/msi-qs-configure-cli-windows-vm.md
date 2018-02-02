---
title: "Vorgehensweise zum Konfigurieren einer vom Benutzer zugewiesenen MSI für eine Azure-VM unter Verwendung der Azure CLI"
description: "Schritt-für-Schritt-Anweisungen für die Konfiguration einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) für eine Azure-VM unter Verwendung der Azure CLI."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 98683af2ca35b687f918647602a561d37dd42b11
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-azure-cli"></a>Konfigurieren einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) für eine VM unter Verwendung der Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Die verwaltete Dienstidentität (Managed Service Identity, MSI) stellt Azure-Dienste mit einer verwalteten Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure CLI eine vom Benutzer zugewiesene MSI für eine Azure-VM aktivieren und entfernen können.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche „Ausprobieren“, die sich in der oberen rechten Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. Melden Sie sich dann mit [az login](/cli/azure/#az_login) bei Azure an. Verwenden Sie ein Konto, das mit dem Azure-Abonnement verknüpft ist, unter dem Sie die vom Benutzer zugewiesene MSI und VM bereitstellen möchten:

   ```azurecli
   az login
   ```

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

Dieser Abschnitt führt Sie durch die Erstellung der VM und die Zuweisung der vom Benutzer zugewiesenen MSI zur VM. Wenn Sie bereits über eine VM verfügen, die Sie verwenden möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit dem nächsten Abschnitt fort.

1. Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie verwenden möchten. Erstellen Sie mithilfe von [az group create](/cli/azure/group/#az_group_create) eine [Ressourcengruppe](~/articles/azure-resource-manager/resource-group-overview.md#terminology) zum Einschließen und Bereitstellen Ihrer MSI. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<LOCATION>` durch Ihre eigenen Werte. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az_identity_create) eine vom Benutzer zugewiesene MSI.  Der Parameter `-g` gibt die Ressourcengruppe an, in der die MSI erstellt wurde, während der Parameter `-n` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Die Antwort enthält Details zu der erstellten vom Benutzer zugewiesenen MSI, ähnlich wie im Folgenden dargestellt wird. Im folgenden Schritt wird der Wert der Ressource `id` verwendet, der der MSI zugewiesen ist.

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

3. Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm/#az_vm_create). Im folgenden Beispiel wird eine durch den Parameter `--assign-identity` angegebene VM erstellt, die mit der neuen vom Benutzer zugewiesenen MSI verknüpft ist. Ersetzen Sie `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` und `<`MSI-ID>` parameter values with your own values. For `<MSI ID>`, use the user-assigned MSI's resource `id“-Eigenschaft, die Sie im vorangegangenen Schritt erstellt haben: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

1. Erstellen Sie mit [az identity create](/cli/azure/identity#az_identity_create) eine vom Benutzer zugewiesene MSI.  Der Parameter `-g` gibt die Ressourcengruppe an, in der die MSI erstellt wurde, während der Parameter `-n` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Die Antwort enthält Details zu der erstellten vom Benutzer zugewiesenen MSI, ähnlich wie im Folgenden dargestellt wird. Im folgenden Schritt wird der Wert der Ressource `id` verwendet, der der MSI zugewiesen ist.

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

2. Weisen Sie mit [az vm assign-identity](/cli/azure/vm#az_vm_assign_identity) die vom Benutzer zugewiesene MSI Ihrer VM zu. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. `<MSI ID>` ist die vom Benutzer zugewiesene Ressourceneigenschaft `id` der MSI, die im vorherigen Schritt erstellt wurde:

    ```azurecli-interactive
    az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

1. Entfernen Sie mithilfe von [az vm remove-identity](/cli/azure/vm#az_vm_remove_identity) die vom Benutzer zugewiesene MSI aus Ihrer VM. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. `<MSI NAME>` ist die vom Benutzer zugewiesene Eigenschaft `name` der MSI, die im Befehl `az identity create` angegeben wurde (siehe Beispiele in den vorherigen Abschnitten):

   ```azurecli-interactive
   az vm remove-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)
- Die vollständigen Schnellstarts zum Erstellen virtueller Azure-Computer finden Sie unter: 

  - [Erstellen eines virtuellen Windows-Computers mit der CLI](~/articles/virtual-machines/windows/quick-create-cli.md)  
  - [Erstellen eines virtuellen Linux-Computers mit der CLI](~/articles/virtual-machines/linux/quick-create-cli.md) 

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

















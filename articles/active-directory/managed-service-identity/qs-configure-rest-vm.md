---
title: Konfigurieren von system- und benutzerzugewiesenen Identitäten auf einem virtuellen Azure-Computer mit REST
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten auf einem virtuellen Azure-Computer mithilfe von CURL zum Durchführen von REST-API-Aufrufen.
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
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: 825f34d174c37c2ee5d4187048f7a31fbaeef226
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215928"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>Konfigurieren einer verwalteten Identität auf einem virtuellen Azure-Computer mithilfe von REST-API-Aufrufen

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Identität stellt für Azure-Dienste eine automatisch verwaltete Systemidentität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt die folgenden Vorgänge für verwaltete Identitäten auf einem virtuellen Azure-Computer ausführen können:

- Aktivieren und Deaktivieren der systemzugewiesenen Identität auf einem virtuellen Azure-Computer
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität auf einem virtuellen Azure-Computer

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität für eine Azure-VM zu aktivieren bzw. zu entfernen
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM zuzuweisen bzw. davon zu entfernen
- Wenn Sie Windows verwenden, installieren Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) im Azure-Portal.
- [Installieren Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle](/azure/install-azure-cli), wenn Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder eine [Linux-Distribution](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) verwenden.
- Wenn Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle verwenden, melden Sie sich über `az login` bei Azure mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, für das Sie system- oder benutzerzugewiesene Identitäten verwalten möchten.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt eine systemzugewiesene Identität auf einem virtuellen Azure-Computer aktivieren und deaktivieren können.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Aktivieren einer systemzugewiesenen Identität beim Erstellen eines virtuellen Azure-Computers

Zum Erstellen eines virtuellen Azure-Computers mit aktivierter systemzugewiesener Identität müssen Sie einen virtuellen Computer erstellen und ein Zugriffstoken abrufen, um CURL zum Aufrufen des Resource Manager-Endpunkts mit dem Wert für den systemzugewiesenen Identitätstyp zu verwenden.

1. Erstellen Sie eine [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen des virtuellen Computers und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az_group_create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Erstellen einer [Netzwerkschnittstelle](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) für den virtuellen Computer:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Erstellen Sie einen virtuellen Computer mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts. Im folgenden Beispiel wird ein virtueller Computer namens *myVM* mit einer systemzugewiesenen Identität erstellt, wie es im Anforderungstext durch den Wert `"identity":{"type":"SystemAssigned"}` angegeben ist. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Aktivieren der systemzugewiesenen Identität auf einem vorhandenen virtuellen Azure-Computer

Um die systemzugewiesene Identität auf einem vorhandenen virtuellen Computer zu aktivieren, müssen Sie ein Zugriffstoken abrufen und dann CURL verwenden, um den Resource Manager-REST-Endpunkt zum Aktualisieren des Identitätstyps aufzurufen.

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Verwenden Sie den folgenden CURL-Befehl zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um die systemzugewiesene Identität auf dem virtuellen Computer zu aktivieren, wie es im Anforderungstext durch den Wert `{"identity":{"type":"SystemAssigned"}` für einen virtuellen Computer namens *myVM* angegeben ist.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
   
   > [!IMPORTANT]
   > Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die dem virtuellen Computer zugewiesen sind, müssen Sie die benutzerzugewiesenen Identitäten mit dem folgenden CURL-Befehl auflisten: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"` Wenn dem virtuellen Computer benutzerzugewiesene Identitäten zugewiesen sind, wie es im `identity`-Wert in der Antwort angegeben ist, fahren Sie mit Schritt 3 fort. Dort wird gezeigt, wie Sie benutzerzugewiesene Identitäten beibehalten und gleichzeitig die systemzugewiesene Identität auf dem virtuellen Computer aktivieren.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Zum Aktivieren der systemzugewiesenen Identität auf einem virtuellen Computer mit vorhandenen benutzerzugewiesenen Identitäten müssen Sie `SystemAssigned` zum `type`-Wert hinzufügen.  
   
   Wenn dem virtuellen Computer beispielsweise die benutzerzugewiesenen Identitäten `ID1` und `ID2` zugewiesen sind, und Sie dem virtuellen Computer die systemzugewiesene Identität hinzufügen möchten, verwenden Sie den folgenden CURL-Aufruf. Ersetzen Sie `<ACCESS TOKEN>` und `<SUBSCRIPTION ID>` durch Werte entsprechend Ihrer Umgebung.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Deaktivieren einer systemzugewiesenen Identität auf einem virtuellen Azure-Computer

Um eine systemzugewiesene Identität auf einem vorhandenen virtuellen Computer zu deaktivieren, müssen Sie ein Zugriffstoken abrufen und dann CURL verwenden, um den Resource Manager-REST-Endpunkt zum Aktualisieren des Identitätstyps in `None` aufzurufen.

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Aktualisieren Sie den virtuellen Computer mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um die systemzugewiesene Identität zu deaktivieren.  Im folgenden Beispiel wird eine systemzugewiesene Identität deaktiviert, wie es im Anforderungstext durch den Wert `{"identity":{"type":"None"}}` von einem virtueller Computer namens *myVM* angegeben ist.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.

   > [!IMPORTANT]
   > Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die dem virtuellen Computer zugewiesen sind, müssen Sie die benutzerzugewiesenen Identitäten mit dem folgenden CURL-Befehl auflisten: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"` Wenn dem virtuellen Computer benutzerzugewiesene Identitäten zugewiesen sind, wie es im `identity`-Wert in der Antwort angegeben ist, fahren Sie mit Schritt 3 fort. Dort wird gezeigt, wie Sie benutzerzugewiesene Identitäten beibehalten und gleichzeitig die systemzugewiesene Identität auf dem virtuellen Computer deaktivieren.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Zum Entfernen der systemzugewiesenen Identität von einem virtuellen Computer mit benutzerzugewiesenen Identitäten entfernen Sie `SystemAssigned` aus dem `{"identity":{"type:" "}}`-Wert, während Sie den `UserAssigned`-Wert und den `identityIds`-Array beibehalten, der definiert, welche benutzerzugewiesenen Identitäten dem virtuellen Computer zugewiesen sind.

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt eine benutzerzugewiesene Identität auf einem virtuellen Computer hinzufügen und entfernen können.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Zuweisen einer benutzerzugewiesenen Identität beim Erstellen eines virtuellen Azure-Computers

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Erstellen einer [Netzwerkschnittstelle](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) für den virtuellen Computer:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Erstellen Sie eine benutzerzugewiesene Identität anhand der Anweisungen unter [Erstellen einer benutzerzugewiesenen verwalteten Identität](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Erstellen Sie einen virtuellen Computer mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts. Im folgenden Beispiel wird ein virtueller Computer namens *myVM* in der Ressourcengruppe *myResourceGroup* mit einer benutzerzugewiesenen Identität `ID1` erstellt, wie es im Anforderungstext durch den Wert `"identity":{"type":"UserAssigned"}` angegeben ist. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Zuweisen einer benutzerzugewiesenen Identität zu einer vorhandenen Azure-VM

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Erstellen Sie eine benutzerzugewiesene Identität anhand der Anweisungen unter [Erstellen einer benutzerzugewiesenen verwalteten Identität](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Um sicherzustellen, dass Sie keine vorhandenen benutzer- oder systemzugewiesenen verwalteten Identitäten löschen, die dem virtuellen Computer zugewiesen sind, müssen Sie die Identitätstypen, die dem virtuellen Computer zugewiesen sind, mit dem folgenden CURL-Befehl auflisten. Sollten der VM-Skalierungsgruppe verwaltete Identitäten zugewiesen sein, sind diese unter dem `identity`-Wert aufgelistet.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Wenn dem virtuellen Computer benutzer- oder systemzugewiesene Identitäten zugewiesen sind, wie es im `identity`-Wert in der Antwort angegeben ist, fahren Sie mit Schritt 5 fort. Dort wird gezeigt, wie Sie benutzerzugewiesene Identitäten beibehalten und gleichzeitig die systemzugewiesene Identität auf dem virtuellen Computer aktivieren.

4. Wenn dem virtuellen Computer keine benutzerzugewiesenen Identitäten zugewiesen sind, verwenden Sie den folgenden CURL-Befehl zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um dem virtuellen Computer die erste benutzerzugewiesene Identität zuzuweisen.  Wenn dem virtuellen Computer benutzerzugewiesene Identitäten zugewiesen sind, fahren Sie mit dem nächsten Schritt fort. Dort wird gezeigt, wie Sie einem virtuellen Computer mehrere benutzerzugewiesene Identitäten hinzufügen.

   Im folgenden Beispiel wird einem virtuellen Computer namens *myVM* in der Ressourcengruppe *myResourceGroup* eine benutzerzugewiesene Identität `ID1` zugewiesen.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Wenn dem virtuellen Computer benutzer- oder systemzugewiesene Identitäten zugewiesen sind, müssen Sie die neue benutzerzugewiesene Identität zum `identityIDs`-Array hinzufügen und gleichzeitig die benutzer- und systemzugewiesenen Identitäten beibehalten, die dem virtuellen Computer derzeit zugewiesen sind.

   Wenn dem virtuellen Computer derzeit beispielsweise die systemzugewiesene Identität und die benutzerzugewiesene Identität `ID1` zugewiesen sind, und Sie die benutzerzugewiesene Identität `ID2` hinzuzufügen möchten, verwenden Sie den folgenden CURL-Befehl. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie mithilfe der Schritte zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den `<SUBSCRIPTION ID>`-Wert entsprechend Ihrer Umgebung.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Entfernen einer benutzerzugewiesenen Identität von einem virtuellen Azure-Computer

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um den virtuellen Computer mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die dem virtuellen Computer weiterhin zugewiesen sein sollen, und auch nicht die systemzugewiesene Identität entfernen, müssen Sie die verwalteten Identitäten mit dem folgenden CURL-Befehl auflisten: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Sollten dem virtuellen Computer verwaltete Identitäten zugewiesen sein, sind diese in der Antwort im `identity`-Wert aufgelistet.

   Wenn dem virtuellen Computer beispielsweise die benutzerzugewiesenen Identitäten `ID1` und `ID2` zugewiesen sind und Sie nur `ID1` und die systemzugewiesene Identität beibehalten möchten, verwenden Sie den gleichen CURL-Befehl wie zum Zuweisen einer benutzerzugewiesenen verwalteten Identität zu einem virtuellen Computer, wobei Sie nur den Wert `ID1` und den Wert `SystemAssigned` beibehalten. Dadurch wird die benutzerzugewiesene Identität `ID2` vom virtuellen Computer entfernt, während die systemzugewiesene Identität beibehalten wird.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Wenn Ihr virtueller Computer sowohl system- als auch benutzerzugewiesene Identitäten aufweist, können Sie alle benutzerzugewiesenen Identitäten entfernen, indem Sie mithilfe des folgenden Befehls in den Modus wechseln, in dem nur die systemzugewiesene Identität verwendet wird:

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Wenn der virtuelle Computer nur benutzerzugewiesene Identitäten enthält und sie alle entfernen möchten, verwenden Sie den folgenden Befehl:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen, Auflisten oder Löschen von benutzerzugewiesenen Identitäten mithilfe von REST finden Sie unter:

- [Erstellen, Auflisten oder Löschen einer benutzerzugewiesenen Identität mithilfe von REST-API-Aufrufen](how-to-manage-ua-identity-rest.md)
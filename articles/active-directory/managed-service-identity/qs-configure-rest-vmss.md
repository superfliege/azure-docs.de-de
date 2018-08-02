---
title: Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mithilfe von REST
description: Schrittweise Anweisungen für ein Konfigurieren von system- und benutzerzugewiesenen Identitäten in einer Azure VM-Skalierungsgruppe mithilfe von CURL zum Durchführen von REST-API-Aufrufen.
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
ms.openlocfilehash: e6c0fc981030b934cd1b8568946344262936e18c
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215706"
---
# <a name="configure-managed-identity-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Konfigurieren einer verwalteten Identität in einer VM-Skalierungsgruppe mithilfe von REST-API-Aufrufen

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Identität stellt für Azure-Dienste eine automatisch verwaltete Systemidentität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt die folgenden Vorgänge für verwaltete Identitäten in einer VM-Skalierungsgruppe ausführen können:

- Aktivieren und Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe
- Hinzufügen und Entfernen einer benutzerzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit „Verwaltete Dienstidentität“ vertraut sind, helfen Ihnen die Informationen in dieser [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Rollenzuweisungen:
    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM-Skalierungsgruppe zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität für eine VM-Skalierungsgruppe zu aktivieren bzw. zu entfernen
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene Identität zu erstellen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene Identität einer VM-Skalierungsgruppe zuzuweisen bzw. daraus zu entfernen.
- Wenn Sie Windows verwenden, installieren Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) im Azure-Portal.
- [Installieren Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle](/azure/install-azure-cli), wenn Sie das [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) oder eine [Linux-Distribution](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) verwenden.
- Wenn Sie die lokale Konsole für die Azure-Befehlszeilenschnittstelle verwenden, melden Sie sich über `az login` bei Azure mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, für das Sie system- oder benutzerzugewiesene Identitäten verwalten möchten.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Systemzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt eine systemzugewiesene Identität in einer VM-Skalierungsgruppe aktivieren und deaktivieren können.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität bei der Erstellung einer VM-Skalierungsgruppe

Zum Erstellen einer VM-Skalierungsgruppe mit aktivierter systemzugewiesener Identität müssen Sie eine VM-Skalierungsgruppe erstellen und ein Zugriffstoken abrufen, um CURL zum Aufrufen des Resource Manager-Endpunkts mit dem Wert für den systemzugewiesenen Identitätstyp zu verwenden.

1. Erstellen Sie eine [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen der VM-Skalierungsgruppe und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az_group_create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Erstellen Sie eine [Netzwerkschnittstelle](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) für die VM-Skalierungsgruppe:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Erstellen Sie eine VM-Skalierungsgruppe mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts. Im folgenden Beispiel wird eine VM-Skalierungsgruppe namens *myVMSS* in *myResourceGroup* mit einer systemzugewiesenen Identität erstellt, wie es im Anforderungstext durch den Wert `"identity":{"type":"SystemAssigned"}` angegeben ist. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
 
    ```bash   
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Aktivieren der systemzugewiesenen Identität in einer vorhandenen Azure VM-Skalierungsgruppe

Um die systemzugewiesene Identität in einer vorhandenen VM-Skalierungsgruppe zu aktivieren, müssen Sie ein Zugriffstoken abrufen und dann CURL verwenden, um den Resource Manager-REST-Endpunkt zum Aktualisieren des Identitätstyps aufzurufen.

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Verwenden Sie den folgenden CURL-Befehl zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um die systemzugewiesene Identität in der VM-Skalierungsgruppe zu aktivieren, wie es im Anforderungstext durch den Wert `{"identity":{"type":"SystemAssigned"}` für eine VM-Skalierungsgruppe namens *myVMSS* angegeben ist.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
   
   > [!IMPORTANT]
   > Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die der VM-Skalierungsgruppe zugewiesen sind, müssen Sie die benutzerzugewiesenen Identitäten mit dem folgenden CURL-Befehl auflisten: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"` Wenn der VM-Skalierungsgruppe benutzerzugewiesene Identitäten zugewiesen sind, wie es im `identity`-Wert in der Antwort angegeben ist, fahren Sie mit Schritt 3 fort. Dort wird gezeigt, wie Sie benutzerzugewiesene Identitäten beibehalten und gleichzeitig die systemzugewiesene Identität in der VM-Skalierungsgruppe aktivieren.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Zum Aktivieren der systemzugewiesenen Identität in einer VM-Skalierungsgruppe mit vorhandenen benutzerzugewiesenen Identitäten müssen Sie `SystemAssigned` zum `type`-Wert hinzufügen.  
   
   Wenn der VM-Skalierungsgruppe beispielsweise die benutzerzugewiesenen Identitäten `ID1` und `ID2` zugewiesen sind, und Sie der VM-Skalierungsgruppe die systemzugewiesene Identität hinzufügen möchten, verwenden Sie den folgenden CURL-Aufruf. Ersetzen Sie `<ACCESS TOKEN>` und `<SUBSCRIPTION ID>` durch Werte entsprechend Ihrer Umgebung.
   
   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deaktivieren der systemzugewiesenen Identität in einer Azure VM-Skalierungsgruppe

Um die systemzugewiesene Identität in einer vorhandenen VM-Skalierungsgruppe zu deaktivieren, müssen Sie ein Zugriffstoken abrufen und dann CURL verwenden, um den Resource Manager-REST-Endpunkt zum Aktualisieren des Identitätstyps in `None` aufzurufen.

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Aktualisieren Sie die VM-Skalierungsgruppe mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um die systemzugewiesene Identität zu deaktivieren.  Im folgenden Beispiel wird die systemzugewiesene Identität in einer VM-Skalierungsgruppe namens *myVMSS* deaktiviert, wie es im Anforderungstext durch den Wert `{"identity":{"type":"None"}}` angegeben ist.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.

   > [!IMPORTANT]
   > Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die der VM-Skalierungsgruppe zugewiesen sind, müssen Sie die benutzerzugewiesenen Identitäten mit dem folgenden CURL-Befehl auflisten: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"` Wenn der VM-Skalierungsgruppe benutzerzugewiesene Identitäten zugewiesen sind, fahren Sie mit Schritt 3 fort. Dort wird gezeigt, wie Sie die benutzerzugewiesenen Identitäten beibehalten und gleichzeitig die systemzugewiesene Identität aus der VM-Skalierungsgruppe entfernen.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Zum Entfernen der systemzugewiesenen Identität aus einer VM-Skalierungsgruppe mit benutzerzugewiesenen Identitäten entfernen Sie `SystemAssigned` aus dem `{"identity":{"type:" "}}`-Wert, während Sie den `UserAssigned`-Wert und den `identityIds`-Array beibehalten, der definiert, welche benutzerzugewiesenen Identitäten der VM-Skalierungsgruppe zugewiesen sind.

## <a name="user-assigned-identity"></a>Benutzerzugewiesene Identität

In diesem Abschnitt erfahren Sie, wie Sie unter Verwendung von CURL für Aufrufe an den Azure Resource Manager-REST-Endpunkt eine benutzerzugewiesene Identität in einer VM-Skalierungsgruppe hinzufügen und entfernen können.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Zuweisen einer benutzerzugewiesenen Identität bei der Erstellung einer VM-Skalierungsgruppe

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Erstellen Sie eine [Netzwerkschnittstelle](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) für die VM-Skalierungsgruppe:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Erstellen Sie eine benutzerzugewiesene Identität anhand der Anweisungen unter [Erstellen einer benutzerzugewiesenen verwalteten Identität](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Erstellen Sie eine VM-Skalierungsgruppe mithilfe von CURL zum Aufrufen des Azure Resource Manager-REST-Endpunkts. Im folgenden Beispiel wird eine VM-Skalierungsgruppe namens *myVMSS* in der Ressourcengruppe *myResourceGroup* mit einer benutzerzugewiesenen Identität `ID1` erstellt, wie es im Anforderungstext durch den Wert `"identity":{"type":"UserAssigned"}` angegeben ist. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Zuweisen einer benutzerzugewiesenen Identität zu einer vorhandenen Azure VM-Skalierungsgruppe

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Erstellen Sie eine benutzerzugewiesene Identität anhand der Anweisungen unter [Erstellen einer benutzerzugewiesenen verwalteten Identität](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Um sicherzustellen, dass Sie keine vorhandenen benutzer- oder systemzugewiesenen verwalteten Identitäten löschen, die der VM-Skalierungsgruppe zugewiesen sind, müssen Sie die Identitätstypen, die der VM-Skalierungsgruppe zugewiesen sind, mit dem folgenden CURL-Befehl auflisten. Sollten der VM-Skalierungsgruppe verwaltete Identitäten zugewiesen sein, sind diese unter dem `identity`-Wert aufgelistet.
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

4. Wenn der VM-Skalierungsgruppe keine benutzer- oder systemzugewiesenen Identitäten zugewiesen sind, verwenden Sie den folgenden CURL-Befehl zum Aufrufen des Azure Resource Manager-REST-Endpunkts, um der VM-Skalierungsgruppe die erste benutzerzugewiesene Identität zuzuweisen.  Wenn der VM-Skalierungsgruppe benutzer- oder systemzugewiesene Identitäten zugewiesen sind, fahren Sie mit Schritt 5 fort. Dort wird gezeigt, wie Sie einer VM-Skalierungsgruppe mehrere benutzerzugewiesenen Identitäten hinzufügen und gleichzeitig die systemzugewiesene Identität beibehalten.

   Im folgenden Beispiel wird einer VM-Skalierungsgruppe namens *myVMSS* in der Ressourcengruppe *myResourceGroup* eine benutzerzugewiesene Identität `ID1` zugewiesen.  Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie im vorherigen Schritt zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den Wert `<SUBSCRIPTION ID>` entsprechend Ihrer Umgebung.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Wenn der VM-Skalierungsgruppe benutzer- oder systemzugewiesene Identitäten zugewiesen sind, müssen Sie die neue benutzerzugewiesene Identität zum `identityIDs`-Array hinzufügen und gleichzeitig die benutzer- und systemzugewiesenen Identitäten beibehalten, die der VM-Skalierungsgruppe derzeit zugewiesen sind.

   Wenn der VM-Skalierungsgruppe derzeit beispielsweise die systemzugewiesene Identität und die benutzerzugewiesene Identität `ID1` zugewiesen sind, und Sie die benutzerzugewiesene Identität `ID2` hinzuzufügen möchten, verwenden Sie den folgenden CURL-Befehl. Ersetzen Sie `<ACCESS TOKEN>` durch den Wert, den Sie mithilfe der Schritte zum Anfordern eines Bearer-Zugriffstokens erhalten haben, und ersetzen Sie den `<SUBSCRIPTION ID>`-Wert entsprechend Ihrer Umgebung.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Entfernen einer benutzerzugewiesenen Identität aus einer VM-Skalierungsgruppe

1. Rufen Sie ein Bearer-Zugriffstoken ab, das Sie dann im nächsten Schritt im Autorisierungsheader verwenden, um die VM-Skalierungsgruppe mit einer systemzugewiesenen verwalteten Identität zu erstellen.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Um sicherzustellen, dass Sie keine vorhandenen benutzerzugewiesenen verwalteten Identitäten löschen, die der VM-Skalierungsgruppe weiterhin zugewiesen sein sollen, und auch nicht die systemzugewiesene Identität entfernen, müssen Sie die verwalteten Identitäten mit dem folgenden CURL-Befehl auflisten: 
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```
   
   Sollten dem virtuellen Computer verwaltete Identitäten zugewiesen sein, sind diese in der Antwort im `identity`-Wert aufgelistet. 
    
   Wenn der VM-Skalierungsgruppe beispielsweise die benutzerzugewiesenen Identitäten `ID1` und `ID2` zugewiesen sind und Sie nur `ID1` und die systemzugewiesene Identität beibehalten möchten, verwenden Sie den gleichen CURL-Befehl wie zum Zuweisen einer benutzerzugewiesenen verwalteten Identität zu einer VM-Skalierungsgruppe, wobei Sie nur den Wert `ID1` und den Wert `SystemAssigned` beibehalten. Dadurch wird die benutzerzugewiesene Identität `ID2` aus der VM-Skalierungsgruppe entfernt, während die systemzugewiesene Identität beibehalten wird.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Wenn die VM-Skalierungsgruppe sowohl system- als auch benutzerzugewiesene Identitäten aufweist, können Sie alle benutzerzugewiesenen Identitäten entfernen, indem Sie mit dem folgenden Befehl in den Modus wechseln, in dem nur die systemzugewiesene Identität verwendet wird:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Wenn die VM-Skalierungsgruppe nur benutzerzugewiesene Identitäten enthält und sie alle entfernen möchten, verwenden Sie den folgenden Befehl:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen, Auflisten oder Löschen von benutzerzugewiesenen Identitäten mithilfe von REST finden Sie unter:

- [Erstellen, Auflisten oder Löschen einer benutzerzugewiesenen Identität mithilfe von REST-API-Aufrufen](how-to-manage-ua-identity-rest.md)
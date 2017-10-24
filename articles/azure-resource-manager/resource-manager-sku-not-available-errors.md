---
title: "Fehler: Azure SKU nicht verfügbar | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie den Fehler vom Typ „SKU nicht verfügbar“ während der Bereitstellung beheben."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 25cea4ae23471d182105ca3f720aaf74f81bf8c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-sku-not-available"></a>Beheben des Fehlers vom Typ „SKU nicht verfügbar“

In diesem Artikel wird beschrieben, wie Sie den Fehler **SkuNotAvailable** beheben.

## <a name="symptom"></a>Symptom

Beim Bereitstellen einer Ressource (in der Regel ein virtueller Computer) werden der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Ursache

Sie erhalten diesen Fehler, wenn die ausgewählte Ressourcen-SKU (z.B. die Größe des virtuellen Computers) für den ausgewählten Standort nicht verfügbar ist.

## <a name="solution"></a>Lösung

Zum Beheben dieses Problems müssen Sie ermitteln, welche SKUs in einer Region verfügbar sind. Sie können PowerShell, das Portal oder einen REST-Vorgang verwenden, um nach verfügbaren SKUs zu suchen.

### <a name="solution-1"></a>Lösung 1

Verwenden Sie in PowerShell den Befehl [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku). Filtern Sie die Ergebnisse nach Standort. Für diesen Befehl benötigen Sie die aktuelle Version von PowerShell.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
```

Die Ergebnisse enthalten eine Liste von SKUs für den Standort und alle Einschränkungen für diese SKU.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

### <a name="solution-2"></a>Lösung 2

Verwenden Sie in der Azure CLI den Befehl `az vm list-skus`. Anschließend können Sie zum Filtern der Ausgabe `grep` oder ein ähnliches Hilfsprogramm verwenden.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

### <a name="solution-3"></a>Lösung 3

Verwenden Sie das [Portal](https://portal.azure.com). Melden Sie sich beim Portal an, und fügen Sie über die Oberfläche eine Ressource hinzu. Beim Festlegen der Werte werden die verfügbaren SKUs für die Ressource angezeigt. Sie müssen die Bereitstellung nicht abschließen.

![Verfügbare SKUs](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Lösung 4

Verwenden Sie die REST-API für virtuelle Computer. Senden Sie die folgende Anforderung:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Verfügbare SKUs und Regionen werden im folgenden Format zurückgegeben:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Wenn Sie keine geeignete SKU in dieser Region oder einer anderen Region finden, die Ihre Geschäftsanforderungen erfüllt, übermitteln Sie eine [SKU-Anforderung](https://aka.ms/skurestriction) an den Azure-Support.
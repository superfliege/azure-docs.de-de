---
title: Azure Kontingentfehler | Microsoft-Dokumentation
description: Beschreibt das Beheben von Kontingentfehlern bei Ressourcen.
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
ms.openlocfilehash: 6a8024a12c4a79e92e37df0a56b6e6bd0cb6a8d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Das Beheben von Fehlern bei Ressourcenkontingenten

Dieser Artikel beschreibt Kontingentfehler, die möglicherweise auftreten, wenn Sie Ressourcen bereitstellen.

## <a name="symptom"></a>Symptom

Wenn Sie eine Vorlage bereitstellen, die Ressourcen erstellt, die Ihre Azure-Kontingente übersteigen, erhalten Sie einen Bereitstellungsfehler, ähnlich dem folgenden Fehler:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Oder Sie sehen möglicherweise:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Ursache

Kontingente werden pro Ressourcengruppe, Abonnements, Konten und anderen Bereichen angewendet. Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr als der zulässigen Anzahl von Kernen bereitzustellen, erhalten Sie eine Fehlermeldung, die darauf hinweist, dass das Kontingent überschritten wurde.
Die vollständigen Kontingentinformationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](../azure-subscription-service-limits.md).

## <a name="solution"></a>Lösung

### <a name="solution-1"></a>Lösung 1

Verwenden Sie für Azure CLI den `az vm list-usage`-Befehl, um Kontingente für virtuelle Computer zu finden.

```azurecli
az vm list-usage --location "South Central US"
```

Ausgabe des Befehls:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Lösung 2

Verwenden Sie für PowerShell den Befehl **Get-AzureRmVMUsage**, um Kontingente für virtuelle Computer zu finden.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Ausgabe des Befehls:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Lösung 3

Wenn Sie eine Kontingentgrenze erhöhen müssen, navigieren Sie zum Portal, und reichen Sie ein Supportproblem ein, um Ihr Kontingent für die Region, in der Sie bereitstellen möchten, zu erhöhen.

> [!NOTE]
> Denken Sie daran, dass für Ressourcengruppen das Kontingent für jede einzelne Region und nicht für das gesamte Abonnement gilt. Wenn Sie 30 Kerne in der Region "USA, Westen" bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne für "USA, Westen" anfordern. Wenn Sie 30 Kerne in einer der Regionen bereitstellen möchten, auf die Sie Zugriff haben, sollten Sie 30 Resource Manager-Kerne in allen Regionen anfordern.
>
>
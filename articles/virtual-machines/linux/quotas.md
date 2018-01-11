---
title: "vCPU-Kontingente für Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über vCPU-Kontingente für Azure."
keywords: 
services: virtual-machines-linux
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: 8b99fd92d9031b7172e698cf5db1f776387bdfb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>vCPU-Kontingente für virtuelle Computer

Die vCPU-Kontingente für virtuelle Computer und die VM-Skalierungsgruppen werden bei jedem Abonnement und in jeder Region in zwei Ebenen angeordnet. „Regionale vCPUs gesamt“ bilden die erste Ebene, die zweite Ebene besteht aus den Kernen der verschiedenen VM-Größenfamilien (z. B. vCPUs der Familie „Standard_D“). Bei jeder Bereitstellung eines neuen virtuellen Computers dürfen die vCPUs für den neu bereitgestellten virtuellen Computer das vCPU-Kontingent für die bestimmte VM-Größenfamilie bzw. das regionale vCPU-Gesamtkontingent nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen. Es gibt auch ein Kontingent für die Gesamtanzahl der virtuellen Computer in der Region. Die Details zu den einzelnen Kontingenten können Sie entweder im [Azure-Portal](https://portal.azure.com) auf der Seite **Abonnement** im Bereich **Nutzung + Kontingente** anzeigen oder die Werte mithilfe der Azure-Befehlszeilenschnittstelle abfragen.


## <a name="check-usage"></a>Überprüfen der Nutzung

Sie können die Nutzung Ihres Kontingents mithilfe von [az vm list-usage](/cli/azure/vm#az_vm_list_usage) überprüfen.

```azurecli-interactive
az vm list-usage --location "East US"
[
  …
  {
    "currentValue": 4,
    "limit": 260,
    "name": {
      "localizedValue": "Total Regional vCPUs",
      "value": "cores"
    }
  },
  {
    "currentValue": 4,
    "limit": 10000,
    "name": {
      "localizedValue": "Virtual Machines",
      "value": "virtualMachines"
    }
  },
  {
    "currentValue": 1,
    "limit": 2000,
    "name": {
      "localizedValue": "Virtual Machine Scale Sets",
      "value": "virtualMachineScaleSets"
    }
  },
  {
    "currentValue": 1,
    "limit": 10,
    "name": {
      "localizedValue": "Standard B Family vCPUs",
      "value": "standardBFamily"
    }
  },
```
## <a name="reserved-vm-instances"></a>Reservierte VM-Instanzen
Reservierte VM-Instanzen, die einem Einzelabonnement zugeordnet sind, erweitern vCPU-Kontingente um einen neuen Aspekt. Diese Werte beschreiben die Anzahl der Instanzen der angegebenen Größe, die im Abonnement bereitstellbar sein muss. Sie fungieren im Kontingentsystem als Platzhalter, um sicherzustellen, dass ein Kontingent reserviert ist. Dadurch wird gewährleistet, dass reservierte Instanzen im Abonnement bereitgestellt werden können. Wenn beispielsweise ein bestimmtes Abonnement über 10 reservierte Instanzen der Größe „Standard_D1“ verfügt, ist „10“ der Nutzungsgrenzwert für reservierte Standard_D1-Instanzen. Dadurch wird Azure veranlasst sicherzustellen, dass immer mindestens 10 vCPUs im regionalen vCPU-Gesamtkontingent sowie 10 vCPUs im vCPU-Kontingent für die Standard_D-Familie verfügbar sind, die für Standard_D1-Instanzen verwendet werden sollen.

Wenn eine Erhöhung des Kontingents erforderlich ist, z. B. um ein Einzelabonnement für eine reservierte Instanz (RI) zu erwerben, können Sie für Ihr Abonnement eine [Erhöhung des Kontingents anfordern](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Abrechnung und Kontingenten finden Sie unter [Grenzwerte für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).

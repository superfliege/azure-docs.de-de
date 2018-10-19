---
title: Auswählen von VM-Größen für Azure Batch-Pools | Microsoft-Dokumentation
description: Wählen aus den verfügbaren VM-Größen für Computeknoten in Azure Batch-Pools
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: danlep
ms.openlocfilehash: 987cbcc642152a4077cc895ad06e43ac56113497
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45544063"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Auswählen einer VM-Größe für Computeknoten in einem Azure Batch-Pool

Wenn Sie eine Knotengröße für einen Azure Batch-Pool wählen, können Sie aus fast allen in Azure verfügbaren VM-Größen wählen. Azure bietet eine Reihe von Größen für virtuelle Linux- und Windows-Computer für verschiedene Workloads. 

Bei der Auswahl einer VM-Größe gelten einige Ausnahmen und Einschränkungen:
* Einige VM-Familien oder -Größen werden in Batch nicht unterstützt. 
* Einige VM-Größen sind eingeschränkt und müssen explizit aktiviert werden, damit sie zugeordnet werden können.


## <a name="supported-vm-families-and-sizes"></a>Unterstützte VM-Familien und -Größen

### <a name="pools-in-virtual-machine-configuration"></a>Pools in der Konfiguration des virtuellen Computers

Batch-Pools in der Konfiguration des virtuellen Computers unterstützen alle VM-Größen ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *außer* den folgenden:

| Familie  | Nicht unterstützte Größen  |
|---------|---------|
| Serie „Basic A“ | Basic_A0 (A0) |
| A-Serie | Standard_A0 |
| B-Serie | Alle |

Virtuelle Computer der M-Serie werden nur für Knoten mit niedriger Priorität unterstützt.


### <a name="pools-in-cloud-service-configuration"></a>Pools in der Clouddienstkonfiguration

Batch-Pools in der Clouddienstkonfiguration unterstützen alle [VM-Größen für Cloud Services](../cloud-services/cloud-services-sizes-specs.md) *außer* den folgenden:

| Familie  | Nicht unterstützte Größen  |
|---------|---------|
| A-Serie | Sehr klein |
| Av2-Serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Eingeschränkte VM-Familien
Die folgenden VM-Familien können in Batch-Pools zugewiesen werden, Sie müssen jedoch eine bestimmte Kontingenterhöhung anfordern (siehe [dieser Artikel](batch-quota-limit.md#increase-a-quota)):
* NCv2-Serie
* NCv3-Serie
* ND-Serie

Diese Größen können nur in Pools in der Konfiguration des virtuellen Computers verwendet werden.

## <a name="size-considerations"></a>Überlegungen zu Größen

* **Anwendungsanforderungen**: Berücksichtigen Sie die Merkmale und Anforderungen der Anwendungen, die auf den Knoten ausgeführt werden sollen. Die Beantwortung der Fragen, ob es sich beispielsweise um eine Multithreadanwendung handelt und wie viel Arbeitsspeicher sie beansprucht, kann Ihnen dabei behilflich sein, die am besten geeignete und kostengünstigste Knotengröße zu bestimmen. Ziehen Sie für [MPI-Workloads](batch-mpi.md) mit mehreren Instanzen oder CUDA-Anwendungen spezielle [HPC](../virtual-machines/linux/sizes-hpc.md)- bzw. [GPU-fähige ](../virtual-machines/linux/sizes-gpu.md) VM-Größen in Betracht. (Informationen dazu finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).) 

* **Aufgaben pro Knoten**: Die Knotengröße wird normalerweise unter der Annahme ausgewählt, dass jeweils nur eine Aufgabe auf einem Knoten ausgeführt wird. Es kann jedoch von Vorteil sein, mehrere Aufgaben (und somit mehrere Anwendungsinstanzen) während der Auftragsausführung auf Computeknoten [parallel zu nutzen](batch-parallel-node-tasks.md). In diesem Fall wird häufig eine Knotengröße mit mehreren Kernen gewählt, um den höheren Bedarf an parallelen Aufgabenausführungen decken zu können.

* **Auslastungsgrade für verschiedene Aufgaben**: Alle Knoten in einem Pool haben die gleiche Größe. Wenn Sie Anwendungen mit unterschiedlichen Systemanforderungen bzw. Auslastungsgraden ausführen möchten, empfehlen wir Ihnen die Verwendung von separaten Pools. 

* **Regionale Verfügbarkeit**: Eine VM-Familie oder -Größe ist in den Regionen, in denen Sie die Batch-Konten erstellen, möglicherweise nicht verfügbar. Informationen dazu, welche Größen verfügbar sind, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

* **Kontingente**: Die [Kernkontingente](batch-quota-limit.md#resource-quotas) in Ihrem Batch-Konto können die Anzahl der Knoten einer bestimmten Größe beschränken, die Sie einem Batch-Pool hinzufügen können. Informationen zum Anfordern einer Kontingenterhöhung finden Sie in [diesem Artikel](batch-quota-limit.md#increase-a-quota). 

* **Poolkonfiguration**: Im Allgemeinen stehen Ihnen mehr VM-Größenoptionen zur Verfügung, wenn Sie einen Pool in der Konfiguration des virtuellen Computers anstatt in der Clouddienstkonfiguration erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
* Informationen zur Verwendung von rechenintensiven VM-Größen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md). 



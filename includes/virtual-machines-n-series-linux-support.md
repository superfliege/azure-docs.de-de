---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: df7d8815eeb588e4e99041844642b6721e25dad7
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52585707"
---
## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA-Treiber

NVIDIA CUDA-Treiber für VMs der Serien NC, NCv2, NCv3, ND und NDv2 (optional für NV-Serie) werden nur unter den in der folgenden Tabelle aufgeführten Linux-Distributionen unterstützt. Die CUDA-Treiberinformationen sind zum Zeitpunkt der Veröffentlichung aktuell. Die aktuellsten CUDA-Treiber finden Sie auf der [NVIDIA](https://developer.nvidia.com/cuda-zone)-Website. Stellen Sie sicher, dass Sie eine Installation der bzw. ein Upgrade auf die neuesten CUDA-Treiber für Ihre Distribution ausgeführt haben. 

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Linux-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Ubuntu 16.04 LTS oder CentOS 7.4 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.

| Distribution | Treiber |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/> 7.3 oder 7.4 (CentOS-basiert), 7.4 HPC (CentOS-basiert) | NVIDIA CUDA 10.0, Treiberbranch R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle Computer der NV- und NVv2-Serie, die als virtuelle Arbeitsstationen oder für virtuelle Anwendungen verwendet werden. Installieren Sie nur diese GRID-Treiber auf Azure NV-VMs, und zwar nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten. Sie müssen keinen NVIDIA vGPU-Softwarelizenzserver einrichten.

| Distribution | Treiber |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/>7.3 oder 7.4 (CentOS-basiert) | NVIDIA GRID 7, Treiberbranch R410|

> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>

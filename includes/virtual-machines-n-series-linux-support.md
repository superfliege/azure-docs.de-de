---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/03/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: e925dba3805ec8994aeba730e325c407468a5c87
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2018
---
## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC-, NCv2-, NCv3- und ND-Serie – NVIDIA CUDA-Treiber

Die CUDA-Treiberinformationen in der folgenden Tabelle sind zum Zeitpunkt der Veröffentlichung aktuell. Die aktuellsten CUDA-Treiber finden Sie auf der [NVIDIA](https://developer.nvidia.com/cuda-zone)-Website. Stellen Sie sicher, dass Sie eine Installation der bzw. ein Upgrade auf die neuesten CUDA-Treiber für Ihre Distribution ausgeführt haben. 

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Linux-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Ubuntu 16.04 LTS oder CentOS 7.4 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.

| Distribution | Treiber |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/> CentOS 7.3 oder 7.4, 7.4 HPC (CentOS-basiert) | NVIDIA CUDA 9.1, Treiberbranch R390 |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-Serie – NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle NV-Computer weiter. Installieren Sie auf virtuellen Azure NV-Computern nur diese GRID-Treiber. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten.

| Distribution | Treiber |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>7.3 (CentOS-basiert) | NVIDIA GRID 6.0, Treiberbranch R390|



> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>

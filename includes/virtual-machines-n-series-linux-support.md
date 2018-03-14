---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 22d37ca30f1319f46a52b96be1c527f6f56719ab
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC-, NCv2-, NCv3- und ND-Serie – NVIDIA CUDA-Treiber
| Distribution | Treiber |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/> CentOS 7.3 oder 7.4 | NVIDIA CUDA 9.1, Treiberbranch R390 |

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie eine Installation der bzw. ein Upgrade auf die neuesten CUDA-Treiber für Ihre Distribution ausgeführt haben. Bei Treibern mit einer älteren Version als R390 können Probleme mit aktualisierten Linux-Kernels auftreten.
>

### <a name="nv-series---nvidia-grid-drivers"></a>NV-Serie – NVIDIA GRID-Treiber

| Distribution | Treiber |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>7.3 (CentOS-basiert) | NVIDIA GRID 5.2, Treiberbranch R384|

> [!NOTE]
> Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle NV-Computer weiter. Installieren Sie auf virtuellen Azure NV-Computern nur diese GRID-Treiber. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten.
>

> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>

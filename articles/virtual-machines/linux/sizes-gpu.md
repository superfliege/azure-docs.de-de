---
title: "Größen von virtuellen Azure Linux-Computern – GPU | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren GPU-optimierten Größen für virtuelle Linux-Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jonbeck
ms.openlocfilehash: 9e8b682d8d7e409f97144d344ec58e710327df7a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2017
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Für GPU optimierte VM-Größen

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Informationen zur Installation von Treibern und zu Überprüfungsschritten finden Sie unter [N-series driver setup for Linux](n-series-driver-setup.md) (Einrichtung von Treibern für die N-Serie unter Linux).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Sie sollten nicht X-Server oder andere Systeme installieren, die den `Nouveau`-Treiber auf Ubuntu-NC-VMs verwenden. Vor der Installation von NVIDIA-GPU-Treibern müssen Sie den `Nouveau`-Treiber deaktivieren.  

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [High Performance Computing](sizes-hpc.md)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
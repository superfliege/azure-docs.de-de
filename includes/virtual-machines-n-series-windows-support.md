---
title: Includedatei
description: Includedatei
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/03/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 689e45f96ed5e7aaea3aecd1193e9cd58f10e80d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC-, NCv2-, NCv3- und ND-Serie – NVIDIA Tesla-Treiber (CUDA)

Die in der folgenden Tabelle bereitgestellten Links zum Herunterladen der Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Windows Server-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Windows Server 2016 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.


| Betriebssystem | Treiber |
| -------- |------------- |
| Windows Server 2016 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2016-international.exe) (EXE-Datei) |
| Windows Server 2012 R2 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (EXE-Datei) |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-Serie – NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle NV-Computer weiter. Installieren Sie auf virtuellen Azure NV-Computern nur diese GRID-Treiber. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten.

| Betriebssystem | Treiber |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [GRID 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836843) (EXE-Datei) |
| Windows Server 2012 R2 | [GRID 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836844) (EXE-Datei)  |
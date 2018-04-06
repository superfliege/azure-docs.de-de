---
title: Einrichtung von Treibern für die Azure N-Serie unter Windows | Microsoft-Dokumentation
description: Einrichtung von NVIDIA-GPU-Treibern für virtuelle Computer der N-Serie mit Windows in Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/20/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61bcbe23ec3afd7091084dd0102010a9b8ef0b7c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server
Um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit Windows Server 2016 oder Windows Server 2012 R2 nutzen zu können, müssen NVIDIA-Grafiktreiber installiert werden. In diesem Artikel werden die Treiberinstallationsschritte beschrieben, die Sie nach der Bereitstellung eines virtuellen Computers der N-Serie ausführen müssen. Informationen zur Einrichtung von Treibern stehen auch für [Linux-VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zur Verfügung.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Windows-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Treiberinstallation

1. Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der N-Serie her.

2. Laden Sie den unterstützten Treiber für Ihr Windows-Betriebssystem herunter, entpacken und installieren Sie ihn.

Auf virtuellen Azure NV-Computern ist nach der Treiberinstallation ein Neustart erforderlich. Für virtuelle NC-Computer ist kein Neustart erforderlich.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Tesla K80-Karte auf einem virtuellen Azure-NC-Computer.

![Eigenschaften des GPU-Treibers](./media/n-series-driver-setup/GPU_driver_properties.png)

Führen Sie zum Abfragen des GPU-Gerätestatus das mit dem Treiber installierte Befehlszeilenprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus.

1. Öffnen Sie eine Eingabeaufforderung, und wechseln Sie in das Verzeichnis **C:\Programme\NVIDIA Corporation\NVSMI**.

2. Führen Sie `nvidia-smi`aus. Wenn der Treiber installiert wurde, wird eine Ausgabe ähnlich wie die folgende angezeigt. Beachten Sie, dass eine **GPU-Auslastung** von **0 %** angezeigt wird, sofern gerade keine GPU-Auslastung auf dem virtuellen Computer ausgeführt wird. Die Treiberversion und die GPU-Informationen weichen möglicherweise von den angezeigten Informationen ab.

![NVIDIA-Gerätestatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA-Netzwerkkonnektivität

RDMA-Netzwerkkonnektivität kann auf virtuellen Computern der N-Serie (z.B. NC24r) aktiviert werden, die RDMA-fähig sind und in derselben Verfügbarkeitsgruppe bzw. VM-Skalierungsgruppe bereitgestellt werden. Die HpcVmDrivers-Erweiterung muss hinzugefügt werden, um Windows-Netzwerkgerätetreiber zu installieren, die für RDMA-Konnektivität benötigt werden. Verwenden Sie [Azure PowerShell](/powershell/azure/overview)-Cmdlets für Azure Resource Manager, um einem virtuellen RDMA-fähigen Computer der N-Serie die VM-Erweiterung hinzuzufügen.

So installieren Sie die neueste Version (1.1) der HpcVMDrivers-Erweiterung auf einem vorhandenen, RDMA-fähigen virtuellen Computer namens myVM in der Region USA, Westen:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Das RDMA-Netzwerk unterstützt Message Passing Interface-Datenverkehr (MPI) für Anwendungen mit [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) oder Intel MPI 5.x. 


## <a name="next-steps"></a>Nächste Schritte

* Entwickler, die GPU-beschleunigte Anwendungen für die NVIDIA Tesla-GPUs erstellen, können auch das [CUDA Toolkit 9.1](https://developer.nvidia.com/cuda-downloads) herunterladen und installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).



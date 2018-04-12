---
title: Größen von virtuellen Azure Windows-Computern – HPC | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren Größen für virtuelle Windows HPC-Computer (High Performance Computing) in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: e402fd3ac95cac4816b9442f7c08aeaf7c108886
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="high-performance-compute-vm-sizes"></a>Größen von virtuellen HPC-Computern (High Performance Computing)

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Betriebssystem**: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** : Microsoft MPI (MS-MPI) 2012 R2 oder höher, Intel MPI Library 5.x

  Unterstützte MPI-Implementierungen verwenden die Microsoft Network Direct-Schnittstelle für die Kommunikation zwischen Instanzen. 

* **RDMA-Netzwerkadressbereich:** Für das RDMA-Netzwerk in Azure wird der Adressbereich 172.16.0.0/16 reserviert. Wenn Sie MPI-Anwendungen auf Instanzen ausführen möchten, die in einem virtuellen Azure-Netzwerk bereitgestellt wurden, vergewissern Sie sich, dass der Adressraum des virtuellen Netzwerks sich nicht mit dem RDMA-Netzwerk überschneidet.

* **VM-Erweiterung HpcVmDrivers**: Fügen Sie auf RDMA-fähigen VMs die Erweiterung HpcVmDrivers hinzu, um Windows-Netzwerkgerätetreiber für RDMA-Verbindungen zu installieren. (In bestimmten Bereitstellungen von A8- und A9-Instanzen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt.) Um die VM-Erweiterung einer VM hinzuzufügen, können Sie [Azure PowerShell](/powershell/azure/overview)-Cmdlets verwenden. 

  
  Der folgende Befehl installiert die neueste Version 1.1 der Erweiterung HpcVMDrivers auf einer vorhandenen RDMA-fähigen VM mit dem Namen *myVM*, die in der Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Westen* bereitgestellt ist:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Erweiterungen können auch für virtuelle Computer verwendet werden, die mit dem [klassischen Bereitstellungsmodell](classic/manage-extensions.md) bereitgestellt wurden.


## <a name="using-hpc-pack"></a>Verwenden von HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts kostenlose HPC-Cluster- und Auftragsverwaltungslösung, stellt eine Option zum Erstellen eines Computeclusters in Azure zum Ausführen Windows-basierter MPI-Anwendungen und anderer HPC-Workloads dar. HPC Pack 2012 R2 und neuere Versionen enthalten eine Laufzeitumgebung für MS-MPI, die das Azure RDMA-Netzwerk bei der Bereitstellung auf RDMA-fähigen VMs verwendet.



## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md)
- [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)

## <a name="next-steps"></a>Nächste Schritte

- Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows Server finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Informationen zur Verwendung rechenintensiver Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../../batch/batch-mpi.md).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.





---
title: "Größen von virtuellen Azure Linux-Computern – HPC | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren Größen für virtuelle Linux HPC-Computer (High Performance Computing) in Azure."
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
ms.date: 10/10/2017
ms.author: jonbeck
ms.openlocfilehash: fecc0656264f1c1d44aa8ad3aea321aa8cc4a0c8
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Größen von virtuellen Linux HPC-Computern (High Performance Computing)

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-fähige Instanzen
Eine Teilmenge der rechenintensiven Instanzen (H16r, H16mr, A8 und A9) verfügt über eine Netzwerkschnittstelle für RDMA-Verbindungen (Remote Direct Memory Access). Diese Schnittstelle steht zusätzlich zur standardmäßigen Azure-Netzwerkschnittstelle anderer VM-Größen zur Verfügung. 
  
Über diese Schnittstelle können die RDMA-fähigen Instanzen über ein InfiniBand-Netzwerk kommunizieren. Dabei können FDR-Raten (virtuelle Computer der Größe H16r oder H16mr) bzw. QDR-Raten (virtuelle Computer der Größe A8 oder A9) verwendet werden. Durch diese RDMA-Funktionen können Skalierbarkeit und Leistung von MPI-Anwendungen (Message Passing Interface), die unter Intel MPI 5.x oder einer höheren Version ausgeführt werden, gesteigert werden.

Stellen Sie die RDMA-fähigen VMs in der gleichen Verfügbarkeitsgruppe (bei Verwendung des Azure Resource Manager-Bereitstellungsmodells) oder im gleichen Clouddienst (bei Verwendung des klassischen Bereitstellungsmodells) bereit. Es folgen weitere Anforderungen für RDMA-fähige Linux-VMs für den Zugriff auf das Azure RDMA-Netzwerk.

### <a name="distributions"></a>Verteilungen
 
Stellen Sie eine computeintensive VM über eines der Images im Azure Marketplace bereit, das RDMA-Konnektivität unterstützt:
  
* **Ubuntu** – Ubuntu Server 16.04 LTS. Konfigurieren Sie RDMA-Treiber auf der VM, und registrieren Sie sich bei Intel, um Intel MPI herunterzuladen:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** – SLES 12 SP3 for HPC, SLES 12 SP3 for HPC (Premium), SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium). Auf der VM werden RDMA-Treiber installiert und Intel MPI-Pakete verteilt. Installieren Sie MPI mit dem folgenden Befehl:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **HPC (CentOS-basiert)** – 7.3 HPC (CentOS-basiert), 7.1 HPC (CentOS-basiert), 6.8 HPC (CentOS-basiert) oder 6.5 HPC (CentOS-basiert) (wird für die H-Serie Version 7.1 oder höher empfohlen). Auf der VM sind RDMA-Treiber und Intel MPI 5.1 installiert.  
 
  > [!NOTE]
  > Bei den CentOS-basierten HPC-Images sind Kernel-Updates in der **yum** -Konfigurationsdatei deaktiviert. Der Grund: Die Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.
  > 
 
### <a name="cluster-configuration"></a>Clusterkonfiguration 
    
Wenn Sie MPI-Aufträge auf gruppierten virtuellen Computer ausführen möchten, sind zusätzliche Konfigurationsschritte erforderlich. So müssen Sie in einem Cluster mit virtuellen Computern beispielsweise die Vertrauensstellung zwischen den Computeknoten einrichten. Informationen zu typischen Einstellungen finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Überlegungen zur Netzwerktopologie
* Auf RDMA-fähigen virtuellen Linux-Computern in Azure ist „Eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.

* In Azure wird IP over InfiniBand (IB) nicht unterstützt. Nur RDMA over IB wird unterstützt.

## <a name="using-hpc-pack"></a>Verwenden von HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)ist eine kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft und bietet eine Option für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack unterstützen das Ausführen von mehreren Linux-Distributionen auf Serverknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Mit RDMA-fähigen Linux-Computeknoten, auf denen Intel MPI ausgeführt wird, kann HPC Pack Linux-basierte MPI-Anwendungen mit Zugriff auf das RDMA-Netzwerk planen und ausführen. Informationen finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Größen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.





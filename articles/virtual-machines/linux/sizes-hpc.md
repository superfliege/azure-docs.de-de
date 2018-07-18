---
title: Größen von virtuellen Azure Linux-Computern – HPC | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren Größen für virtuelle Linux HPC-Computer (High Performance Computing) in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 748cb4612b2b5aed26ba8197cfad0782f2645e1e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37902128"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>High Performance Computing-VM-Größen

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Nur Intel MPI 5.x-Versionen werden unterstützt. Höhere Versionen der Intel MPI-Laufzeitbibliothek (2017, 2018) sind mit den Azure-Linux-RDMA-Treibern nicht kompatibel.


### <a name="distributions"></a>Verteilungen
 
Stellen Sie eine computeintensive VM über eines der Images im Azure Marketplace bereit, das RDMA-Konnektivität unterstützt:
  
* **Ubuntu** – Ubuntu Server 16.04 LTS. Konfigurieren Sie RDMA-Treiber auf der VM, und registrieren Sie sich bei Intel, um Intel MPI herunterzuladen:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** – SLES 12 SP3 for HPC, SLES 12 SP3 for HPC (Premium), SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium). Auf der VM werden RDMA-Treiber installiert und Intel MPI-Pakete verteilt. Installieren Sie MPI mit dem folgenden Befehl:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **CentOS-basiertes HPC** – CentOS-basiertes HPC der Version 6.5 oder höher (für die H-Reihe wird Version 7.1 oder höher empfohlen). Auf der VM sind RDMA-Treiber und Intel MPI 5.1 installiert.  
 
  > [!NOTE]
  > Bei den CentOS-basierten HPC-Images sind Kernel-Updates in der **yum** -Konfigurationsdatei deaktiviert. Der Grund: Die Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.
  > 
 
### <a name="cluster-configuration"></a>Clusterkonfiguration 
    
Wenn Sie MPI-Aufträge auf gruppierten virtuellen Computer ausführen möchten, sind zusätzliche Konfigurationsschritte erforderlich. So müssen Sie in einem Cluster mit virtuellen Computern beispielsweise die Vertrauensstellung zwischen den Computeknoten einrichten. Informationen zu typischen Einstellungen finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Überlegungen zur Netzwerktopologie
* Auf RDMA-fähigen virtuellen Linux-Computern in Azure ist „Eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.

* Das RDMA-Netzwerk in Azure reserviert sich den Adressbereich 172.16.0.0/16. 


## <a name="using-hpc-pack"></a>Verwenden von HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)ist eine kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft und bietet eine Option für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack unterstützen das Ausführen von mehreren Linux-Distributionen auf Serverknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Mit RDMA-fähigen Linux-Computeknoten, auf denen Intel MPI ausgeführt wird, kann HPC Pack Linux-basierte MPI-Anwendungen mit Zugriff auf das RDMA-Netzwerk planen und ausführen. Informationen finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Größen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.





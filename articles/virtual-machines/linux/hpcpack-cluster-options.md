---
title: Linux HPC Pack-Clusteroptionen in Azure | Microsoft-Dokumentation
description: Informationen zu Optionen mit Microsoft HPC Pack, um einen Linux-HPC-Cluster (High Performance Computing) in der Azure-Cloud zu erstellen und zu verwalten
services: virtual-machines-linux,cloud-services
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 281867e30c78c7ed36ac739c8ae1a902463199cd
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34166453"
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>Optionen zum Erstellen und Verwalten eines Clusters für Linux-HPC-Workloads in Azure mit HPC Pack
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel konzentriert sich auf Azure-Optionen zum Ausführen von Linux-Workloads unter Verwendung von HPC Pack. Es gibt auch Optionen zum Ausführen von [Windows-HPC-Workloads mit HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack-Cluster in virtuellen Azure-Computern und VM-Skalierungsgruppen
### <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen
* (GitHub) [HPC Pack 2016 Update 1 (Clustervorlagen)](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2 Clustervorlagen](https://github.com/MsHpcPack/HPCPack2012R2)
* (Schnellstart) [Create HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn) (Erstellen eines HPC-Clusters mit Linux-Computeknoten)

### <a name="azure-vm-images"></a>Azure VM-Images
Navigieren Sie zu [HPC Pack-Images im Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22), wenn Sie Ihren eigenen HPC Pack-Cluster in Azure erstellen möchten.

## <a name="hpc-pack-2012-r2-cluster-in-classic-deployment-model"></a>HPC Pack 2012 R2-Cluster mit dem klassischen Bereitstellungsmodell
* [Erstellen eines Linux-HPC-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von NAMD mit Microsoft HPC Pack auf Linux-Computeknoten in Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von STAR-CCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="job-submisssion"></a>Auftragsübermittlung
* [Übermitteln von Aufträgen an einen HPC Pack-Cluster in Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)





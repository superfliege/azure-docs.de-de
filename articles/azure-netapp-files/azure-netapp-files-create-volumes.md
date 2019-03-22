---
title: Create a volume for Azure NetApp Files | Microsoft Docs
description: Describes how to create a volume for Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: b-juche
ms.openlocfilehash: 0d4629651e17c917720196c1e5f6152c50c2a33a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770265"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Create a volume for Azure NetApp Files

A volume's capacity consumption counts against its pool's provisioned capacity.  You can create multiple volumes in a capacity pool, but the volumes' total capacity consumption must not exceed the pool size. 

## <a name="before-you-begin"></a>Before you begin 
You must have already set up a capacity pool.   
[Set up a capacity pool](azure-netapp-files-set-up-capacity-pool.md)   
A subnet must be delegated to Azure NetApp Files.  
[Delegate a subnet to Azure NetApp Files](azure-netapp-files-delegate-subnet.md)


## <a name="steps"></a>Steps 
1.  Click the **Volumes** blade from the Manage Capacity Pools blade. 
2.  Click **+ Add volume** to create a volume.  
    The New Volume window appears.

3.  In the New Volume window, click **Create** and provide information for the following fields:   
    * **Name**      
        Specify the name for the volume that you are creating.   

        The name must be unique within a resource group. It must be at least three characters long.  It can use any alphanumeric characters.

    * **File path**  
        Specify the file path that will be used to create the export path for the new volume. The export path is used to mount and access the volume.   
     
        The file path name can contain letters, numbers, and hyphens ("-") only. It must be between 16 and 40 characters in length.  

    * **Quota**  
        Specify the amount of logical storage that is allocated to the volume.  

        The **Available quota** field shows the amount of unused space in the chosen capacity pool that you can use towards creating a new volume. The size of the new volume must not exceed the available quota.  

    * **Virtual network**  
        Specify the Azure virtual network (Vnet) from which you want to access the volume.  

        The Vnet you specify must have a subnet delegated to Azure NetApp Files. The Azure NetApp Files service can be accessed only from the same Vnet or from a Vnet that is in the same region as the volume through Vnet peering. You can also access the volume from  your on-premise network through Express Route.   

    * **Subnet**  
        Specify the subnet that you want to use for the volume.  
        The subnet you specify must be delegated to Azure NetApp Files. 
        
        If you have not delegated a subnet, you can click **Create new** on the Create a Volume page. Then in the Create Subnet page, specify the subnet information, and select **Microsoft.NetApp/volumes** to delegate the subnet for Azure NetApp Files. Note that in each Vnet, only one subnet can be delegated to Azure NetApp Files.   
 
        ![New volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Create subnet](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)


4.  Click **OK**. 
 
A volume inherits subscription, resource group, location attributes from its capacity pool. To monitor the volume deployment status, you can use the Notifications tab.

## <a name="next-steps"></a>Next steps  
* [Configure export policy for a volume (optional)](azure-netapp-files-configure-export-policy.md)
* [Learn about virtual network integration for Azure services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


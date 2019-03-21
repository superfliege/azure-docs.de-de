---
title: Delegieren eines Subnetzes an Azure NetApp Files | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Delegieren eines Subnetzes an Azure NetApp Files.
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
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: 6c1a6bf4e7042c28239f57af6b39c0822b63b5e8
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768075"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Delegieren eines Subnetzes an Azure NetApp Files 

Sie müssen ein Subnetze an Azure NetApp Files delegieren.   Wenn Sie ein Volume erstellen, müssen Sie das delegierte Subnetz angeben.

## <a name="about-this-task"></a>Informationen zu dieser Aufgabe
* Der Assistent zum Erstellen eines neuen Subnetzes verwendet standardmäßig eine /24-Netzwerkmaske, die 251 IP-Adressen bereitstellt. Die Verwendung einer /28-Netzwerkmaske, die 16 nutzbare IP-Adressen bereitstellt, ist für den Dienst ausreichend.
* Sie können im delegierten Subnetz keine Netzwerksicherheitsgruppe und keinen Dienstendpunkt festlegen. Dadurch würde bei der Subnetzdelegierung ein Fehler auftreten.
* In jedem virtuellen Azure-Netzwerk (VNET) kann nur ein Subnetz an Azure NetApp Files delegiert werden.
* Der Zugriff auf einem Volume aus einem virtuellen Peeringnetzwerk wird zurzeit nicht unterstützt.

## <a name="steps"></a>Schritte 
1.  Wechseln Sie im Azure-Portal zum Blatt **Virtuelles Netzwerk**, und wählen Sie das virtuelle Netzwerk aus, das Sie für Azure NetApp Files verwenden möchten.    

1. Wählen Sie auf dem Blatt „Virtuelles Netzwerk“ die Option **Subnetze** aus, und klicken Sie auf die Schaltfläche **+Subnetz**. 

1. Erstellen Sie ein neues Subnetz, das für Azure NetApp Files verwendet werden soll, indem Sie die folgenden erforderlichen Felder auf der Seite „Subnetz hinzufügen“ ausfüllen:
    * **Name**: Geben Sie den Namen des Subnetzes an.
    * **Adressbereich**: Geben Sie den IP-Adressbereich an.
    * **Subnetzdelegierung**: Wählen Sie **Microsoft.NetApp/volumes** aus. 

      ![Subnetzdelegierung](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Sie können ein Subnetz auch beim [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md) erstellen und delegieren. 

## <a name="next-steps"></a>Nächste Schritte  
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erfahren Sie mehr über die Integration virtueller Netzwerke für Azure-Dienste](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)



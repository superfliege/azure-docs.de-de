---
title: Einrichten eines Kapazitätspools für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Einrichten eines Kapazitätspools für die Volumeerstellung.
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
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 55a1d16ce1617ecf7bc28c7c62de8557ceeea311
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412910"
---
# <a name="set-up-a-capacity-pool"></a>Einrichten eines Kapazitätspools
Nach dem Einrichten eines Kapazitätspools können Sie Volumes in diesem Pool erstellen.  

## <a name="before-you-begin"></a>Voraussetzungen 
Sie müssen bereits ein NetApp-Konto erstellt haben.   

[Erstellen eines NetApp-Kontos](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Schritte 

1. Navigieren Sie zum Blatt für die Verwaltung Ihres NetApp-Kontos, und klicken Sie anschließend im Navigationsbereich auf **Kapazitätspools**.

2. Klicken Sie auf **+ Pools hinzufügen**, um einen neuen Kapazitätspool zu erstellen.   
    Das Fenster für den neuen Kapazitätspool wird angezeigt.

3. Geben Sie für den neuen Kapazitätspool folgende Informationen an:  
  * **Name**  
    Geben Sie den Namen für den Kapazitätspool an.  
    Der Name des Kapazitätspools muss für jedes NetApp-Konto eindeutig sein.

  * **Servicelevel**   
    In diesem Feld wird die gewünschte Leistung des Kapazitätspools angezeigt.  
    Aktuell steht nur das Servicelevel „Premium“ zur Verfügung. 

  *  **Größe**     
      Geben Sie die Größe des Kapazitätspools an, den Sie erwerben.        
      Die Mindestgröße eines Kapazitätspools beträgt 4 TiB. Die Poolgröße kann auf ein Vielfaches von 4 TiB festgelegt werden.   
      
      ![Neuer Kapazitätspool](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Klicken Sie auf **OK**.

## <a name="next-steps"></a>Nächste Schritte 

[Delegieren eines Subnetzes für Azure NetApp Files](azure-netapp-files-delegate-subnet.md)



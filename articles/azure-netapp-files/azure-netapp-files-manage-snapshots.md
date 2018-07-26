---
title: Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure NetApp Files bei Bedarf eine Momentaufnahme für ein Volume erstellen oder aus einer Momentaufnahme auf einem neuen Volume wiederherstellen.
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
ms.topic: how-to-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 48cb88b9815ba723d93c18caf63f33b50eea850c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011978"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files
Mithilfe von Azure NetApp Files können Sie eine Momentaufnahme bei Bedarf für ein Volume erstellen oder aus einer Momentaufnahme auf einem neuen Volume wiederherstellen.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Erstellen einer Momentaufnahme bei Bedarf für ein Volume
Momentaufnahmen können nur bei Bedarf erstellt werden.  Richtlinien für Momentaufnahmen werden derzeit nicht unterstützt.  
1.  Klicken Sie im Blatt „Volume verwalten“ auf **Momentaufnahmen** und anschließend auf **+ Momentaufnahme hinzufügen**, um eine bedarfsgesteuerte Momentaufnahme für ein Volume zu erstellen.

2.  Geben Sie im Fenster „Neue Momentaufnahme“ der neu erstellten Momentaufnahme einen Namen an.   

3. Klicken Sie auf **OK**. 


## <a name="restore-a-snapshot-to-a-new-volume"></a>Wiederherstellen einer Momentaufnahme auf einem neuen Volume
Derzeit können Sie eine Momentaufnahme nur auf einem neuen Volume wiederherstellen. 
1. Wechseln Sie vom Blatt „Volume“ zum Blatt **Momentaufnahmen verwalten**, um die Liste der Momentaufnahmen anzuzeigen. 
2. Wählen Sie die Momentaufnahme aus, die wiederhergestellt werden soll.  
3. Klicken Sie mit der rechten Maustaste auf den Namen der Momentaufnahme, und wählen Sie im Menü die Option **Auf neuem Volume wiederherstellen** aus.  

    ![Wiederherstellen von Momentaufnahme auf neuem Volume](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Geben Sie im Fenster „Neues Volume“ die folgenden Informationen für das neue Volume an:  
    * **Name**   
        Geben Sie den Namen für das neu erstellte Volume an.  
        
        Der Name muss innerhalb einer Ressourcengruppe eindeutig sein. Er muss mindestens drei Zeichen umfassen.  Er darf alle alphanumerischen Zeichen enthalten.

    * **Dateipfad**     
        Geben Sie den Dateipfad an, der zum Erstellen des Exportpfads für das neue Volume verwendet wird. Der Exportpfad dient zum Einbinden und Zugreifen auf das Volume.   
        
        Das Einbindungsziel ist der Endpunkt der IP-Adresse des NFS-Diensts. Es wird automatisch generiert.   
        
        Der Dateipfadname darf nur Buchstaben, Zahlen und Bindestriche („-“) enthalten. Er muss 16 bis 40 Zeichen umfassen. 

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt die Menge an nicht verwendetem Speicherplatz im ausgewählten Kapazitätspool an, die Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.

    *   **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNet) an, von dem aus Sie auf das Volume zugreifen möchten. 
        
        Das angegebene Vnet muss für Azure NetApp Files konfiguriert sein. Auf den Azure NetApp Files-Dienst kann nur von einem Vnet aus zugegriffen werden, das sich am gleichen Speicherort wie das Volume befindet.  

    ![Wiederhergestelltes neues Volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
    
5. Klicken Sie auf **OK**.   
    Das neue Volume, auf dem die Momentaufnahme wiederhergestellt wurde, wird auf dem Blatt „Volumes“ angezeigt.


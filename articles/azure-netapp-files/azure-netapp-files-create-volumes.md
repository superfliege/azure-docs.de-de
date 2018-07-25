---
title: Erstellen eines Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Erstellen eines Volumes für Azure NetApp Files.
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
ms.openlocfilehash: 42e475f4da2102bb8b010daec6e6451ba7f13848
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011091"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Erstellen eines Volumes für Azure NetApp Files

Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet.  In einem Kapazitätspool können mehrere Volumes erstellt werden, der gesamte Kapazitätsverbrauch der Volumes darf jedoch die Poolgröße nicht übersteigen. 

## <a name="before-you-begin"></a>Voraussetzungen 
Sie müssen bereits einen Kapazitätspool eingerichtet haben.  

[Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)

## <a name="steps"></a>Schritte 
1.  Klicken Sie auf dem Blatt zum Verwalten von Kapazitätspools auf **Volumes**. 
2.  Klicken Sie auf **+ Volume hinzufügen**, um ein Volume zu erstellen.  
    Das Fenster „Neues Volume“ wird angezeigt.

3.  Klicken Sie im Fenster „Neues Volume“ auf **Erstellen**, und geben Sie Informationen für die folgenden Felder an:   
    * **Name**      
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   
        Der Name muss innerhalb einer Ressourcengruppe eindeutig sein. Er muss mindestens drei Zeichen umfassen.  Er darf beliebige alphanumerische Zeichen enthalten.

    * **Dateipfad**  
        Geben Sie den Dateipfad zum Erstellen des Exportpfads für das neue Volume an. Der Exportpfad dient zum Einbinden und Zugreifen auf das Volume.   
        Das Einbindungsziel ist der Endpunkt der IP-Adresse des NFS-Diensts. Es wird automatisch generiert.    
        Der Dateipfadname darf nur Buchstaben, Zahlen und Bindestriche („-“) enthalten. Er muss 16 bis 40 Zeichen umfassen.  

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  
        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.  

    * **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNet) an, von dem aus Sie auf das Volume zugreifen möchten. Für das angegebene VNet muss Azure NetApp Files konfiguriert sein. Auf den Azure NetApp Files-Dienst kann nur von einem VNet aus zugegriffen werden, das sich am gleichen Ort befindet wie das Volume.   

    ![Neues Volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)

4.  Klicken Sie auf **OK**. 
 
Ein Volume erbt Abonnement-, Ressourcengruppen- und Standortattribute aus dem Kapazitätspool. Den Volumebereitstellungsstatus können Sie auf der Benachrichtigungsregisterkarte überwachen.

## <a name="next-steps"></a>Nächste Schritte  
[Konfigurieren der Exportrichtlinie für ein Volume (optional)](azure-netapp-files-configure-export-policy.md)


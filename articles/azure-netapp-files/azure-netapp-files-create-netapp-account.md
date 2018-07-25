---
title: Erstellen eines NetApp-Kontos für den Zugriff auf Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt das Zugreifen auf Azure NetApp Files sowie das Erstellen eines NetApp-Kontos, um einen Kapazitätspool einrichten und ein Volume erstellen zu können.
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
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010343"
---
# <a name="create-a-netapp-account"></a>Erstellen eines NetApp-Kontos
Nach der Erstellung eines NetApp-Kontos können Sie einen Kapazitätspool einrichten und anschließend ein Volume erstellen. Neue NetApp-Konten werden über das Blatt „Azure NetApp Files“ erstellt.

## <a name="before-you-begin"></a>Voraussetzungen
Sie müssen in die Whitelist für den Zugriff auf den Azure-Ressourcenanbieter „Microsoft.NetApp“ aufgenommen und für die Verwendung des Azure NetApp Files-Diensts konfiguriert worden sein.  

[Registrierungsseite für die Public Preview von Azure NetApp Files](https://aka.ms/nfspublicpreview) 

## <a name="steps"></a>Schritte 

1. Melden Sie sich über die Azure-Portal-Vorschau-URL aus Ihrer Vorschaueinladung beim Portal an. 
2.  Greifen Sie mithilfe einer der folgenden Methoden auf das Blatt „Azure NetApp Files“ zu:  
  * Suchen Sie im Suchfeld des Azure-Portals nach **Azure NetApp Files**.  
  * Klicken Sie im Navigationsbereich auf **Alle Dienste**, und filtern Sie nach „Azure NetApp Files“.  

  Sie können das Blatt „Azure NetApp Files“ als Favorit speichern, indem Sie daneben auf das Sternsymbol klicken. 

3. Klicken Sie auf **+ Hinzufügen**, um ein neues NetApp-Konto zu erstellen.  
  Das Fenster für das neue NetApp-Konto wird angezeigt.  

4. Geben Sie für Ihr NetApp-Konto folgende Informationen an: 
  * **Kontoname**  
    Geben Sie einen eindeutigen Namen für das Abonnement an.
  *  **Abonnement**  
    Wählen Sie eines der vorhandenen Abonnements aus.
  * **Ressourcengruppe**   
    Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue.
  * **Standort**  
    Wählen Sie die Region aus, in der sich das Konto und dessen untergeordnete Ressourcen befinden sollen.  
    Momentan wird der Azure NetApp Files-Dienst nur in der Region „USA, Osten“ unterstützt.  

    ![Neues NetApp-Konto](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Klicken Sie auf **Create**.     
  Das erstellte NetApp-Konto wird nun auf dem Blatt „Azure NetApp Files“ angezeigt. 

## <a name="next-steps"></a>Nächste Schritte  

1. [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
2. [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Konfigurieren der Exportrichtlinie für ein Volume (optional)](azure-netapp-files-configure-export-policy.md)

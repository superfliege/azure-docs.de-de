---
title: Einrichten von Azure NetApp Files und Erstellen eines Volumes | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie auf schnelle Weise Azure NetApp Files einrichten und ein Volume erstellen.
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
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: b-juche
ms.openlocfilehash: 634f23cf3161fff09f21c79fd8300cb269dcc5b7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546575"
---
# <a name="set-up-azure-netapp-files-and-create-a-volume"></a>Einrichten von Azure NetApp Files und Erstellen eines Volumes 

In diesem Artikel erfahren Sie, wie Sie auf schnelle Weise Azure NetApp Files einrichten und ein Volume erstellen. 

## <a name="before-you-begin"></a>Voraussetzungen 

Sie müssen am Public Preview-Programm teilnehmen und auf der Whitelist für den Zugriff auf den Ressourcenanbieter „Microsoft.NetApp“ stehen. Nähere Informationen zur Teilnahme am Public Preview-Programm finden Sie auf der [Registrierungsseite für Azure NetApp Files Public Preview](https://aka.ms/nfspublicpreview). 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registrieren für Azure NetApp Files und NetApp-Ressourcenanbieter

1. Klicken Sie im Azure-Portal in der rechten oberen Ecke auf das Azure Cloud Shell-Symbol.

      ![Azure Cloud Shell-Symbol](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Geben Sie das Abonnement an, das für Azure NetApp Files in die Whitelist aufgenommen wurde:
    
        az account set --subscription <subscriptionId>

3. Registrieren des Azure-Ressourcenanbieters: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    Der Registrierungsvorgang kann einige Zeit in Anspruch nehmen.

## <a name="create-a-netapp-account"></a>Erstellen eines NetApp-Kontos

1. Geben Sie im Suchfeld des Azure-Portals **Azure NetApp Files** ein, und wählen Sie in der daraufhin angezeigten Liste die Option **Azure NetApp Files (Vorschauversion)** aus.

      ![Auswählen von „Azure NetApp Files“](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Klicken Sie auf **+ Hinzufügen**, um ein neues NetApp-Konto zu erstellen.

     ![Erstellen eines neuen NetApp-Kontos](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. Geben Sie im Fenster für das neue NetApp-Konto folgende Informationen an: 
   1. Geben Sie **myaccount1** als Kontoname ein. 
   2. Wählen Sie Ihr Abonnement aus.
   3. Wählen Sie **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen. Geben Sie **myRG1** als Ressourcengruppenname ein. Klicken Sie auf **OK**. 
   4. Wählen Sie den Standort Ihres Kontos aus.  

      ![Fenster für das neue NetApp-Konto](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Ressourcengruppenfenster](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Klicken Sie auf **Erstellen**, um Ihr neues NetApp-Konto zu erstellen.

## <a name="set-up-a-capacity-pool"></a>Einrichten eines Kapazitätspools

1. Wählen Sie auf dem Azure NetApp Files-Verwaltungsblatt Ihr NetApp-Konto (**myaccount1**) aus.

    ![Auswählen des NetApp-Kontos](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. Klicken Sie auf dem Azure NetApp Files-Verwaltungsblatt Ihres NetApp-Kontos auf **Kapazitätspools**.

    ![Klicken auf Kapazitätspools](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Klicken Sie auf **+ Pools hinzufügen**. 

    ![Klicken auf „Pools hinzufügen“](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Geben Sie Informationen für den Kapazitätspool an: 
    1. Geben Sie **mypool1** als Poolname ein.
    2. Wählen Sie für den Servicelevel die Option **Premium** aus. 
    3. Geben Sie eine Poolgröße von **4 (TiB)** an. 

5. Klicken Sie auf **OK**.

## <a name="create-a-volume-for-azure-netapp-files"></a>Erstellen eines Volumes für Azure NetApp Files

1. Klicken Sie auf dem Azure NetApp Files-Verwaltungsblatt Ihres NetApp-Kontos auf **Volumes**.

    ![Klicken auf „Volumes“](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Klicken Sie auf **+ Volume hinzufügen**.

    ![Klicken auf „Volume hinzufügen“](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Geben Sie im Fenster „Volume erstellen“ Informationen für das Volume an: 
   1. Geben Sie **myvol1** als Volumename ein. 
   2. Geben Sie **myfilepath1** als Dateipfad zum Erstellen des Exportpfads für das Volume an.
   3. Wählen Sie Ihren Kapazitätspool (**mypool1**) aus.
   4. Übernehmen Sie für das Kontingent den Standardwert. 
   5. Klicken Sie unter „Virtuelles Netzwerk“ auf **Neu erstellen**, um ein neues virtuelles Azure-Netzwerk (VNET) zu erstellen.  Geben Sie anschließend folgende Informationen an:
       * Geben Sie **myvnet1** als VNET-Name ein.
       * Geben Sie einen Adressraum für Ihre Einstellung an (beispielsweise 10.7.0.0/16).
       * Geben Sie **myANFsubnet** als Subnetzname ein.
       * Geben Sie den Subnetzadressbereich an (beispielsweise 10.7.0.0/24). Beachten Sie, dass Sie das dedizierte Subnetz nicht für andere Ressourcen freigeben können.
       * Wählen Sie für die Subnetzdelegierung die Option **Microsoft.NetApp/volumes** aus.
       * Klicken Sie auf **OK**, um das VNET zu erstellen.
   6. Wählen Sie unter „Subnetz“ das neu erstellte VNET (**myvnet1**) als Delegatsubnetz aus.

      ![Fenster „Volume erstellen“](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Fenster „Virtuelles Netzwerk erstellen“](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Klicken Sie auf **Überprüfen + erstellen**.

    ![Fenster „Überprüfen + erstellen“](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Überprüfen Sie die Informationen für das Volume, und klicken Sie anschließend auf **Erstellen**.  
    Das erstellte Volume wird auf dem Blatt „Volumes“ angezeigt.

    ![Volume erstellt](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="next-steps"></a>Nächste Schritte  

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Verwalten von Volumes mithilfe von Azure NetApp Files](azure-netapp-files-manage-volumes.md) 

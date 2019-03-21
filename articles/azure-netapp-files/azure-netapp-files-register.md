---
title: Registrieren für Azure NetApp Files | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie eine Anforderung zum Registrieren im Azure NetApp Files-Dienst übermitteln.
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
ms.date: 01/04/2018
ms.author: b-juche
ms.openlocfilehash: 86c016a5dbcc0d78378e59bc6b3606ddf2c54f64
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58122334"
---
# <a name="register-for-azure-netapp-files"></a>Registrieren für Azure NetApp Files
Bevor Sie Azure NetApp Files nutzen können, müssen Sie eine Anforderung zum Registrieren im Azure NetApp Files-Dienst übermitteln.  Danach registrieren Sie sich für die Verwendung des Diensts.

## <a name="request-to-enroll-in-the-service"></a>Anforderung zum Registrieren im Dienst
Sie müssen am Public Preview-Programm teilnehmen und auf der Whitelist für den Zugriff auf den Ressourcenanbieter „Microsoft.NetApp“ stehen. Nähere Informationen zur Teilnahme am Public Preview-Programm finden Sie auf der [Registrierungsseite für Azure NetApp Files Public Preview](https://aka.ms/nfspublicpreview). 


## <a name="register-the-netapp-resource-provider"></a>Registrieren des NetApp-Ressourcenanbieters

Um den Dienst zu verwenden, müssen Sie den Azure-Ressourcenanbieter für Azure NetApp Files registrieren. 

1. Klicken Sie im Azure-Portal in der rechten oberen Ecke auf das Symbol für die Azure Cloud Shell:

      ![Azure Cloud Shell-Symbol](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Wenn Sie in Ihrem Azure-Konto über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, das auf der Whitelist für Azure NetApp Files steht:
    
        az account set --subscription <subscriptionId>

3. Geben Sie in der Azure Cloud Shell-Konsole den folgenden Befehl ein, um zu überprüfen, ob Ihr Abonnement auf der Whitelist steht:
    
        az feature list | grep NetApp

   Die Ausgabe des Befehls sieht wie folgt aus:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/publicPreviewADC",  
       "name": "Microsoft.NetApp/publicPreviewADC" 
       
   `<SubID>` ist Ihre Abonnement-ID.

4. Geben Sie in der Azure Cloud Shell-Konsole den folgenden Befehl ein, um den Azure-Ressourcenanbieter zu registrieren: 
    
        az provider register --namespace Microsoft.NetApp --wait

   Der `--wait`-Parameter weist die Konsole an, auf den Abschluss der Registrierung zu warten. Der Registrierungsvorgang kann einige Zeit in Anspruch nehmen.

5. Geben Sie in der Azure Cloud Shell-Konsole den folgenden Befehl ein, um zu überprüfen, ob der Azure-Ressourcenanbieter registriert wurde: 
    
        az provider show --namespace Microsoft.NetApp

   Die Ausgabe des Befehls sieht wie folgt aus:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   `<SubID>` ist Ihre Abonnement-ID.  Der Parameterwert `state` gibt `Registered` an.

6. Klicken Sie im Azure-Portal auf das Blatt **Abonnements**.
7. Klicken Sie auf dem Blatt „Abonnements“ auf Ihre Abonnement-ID. 
8. Klicken Sie in den Einstellungen des Abonnements auf **Ressourcenanbieter**, um zu überprüfen, ob der Microsoft.NetApp-Anbieter den Status „Registriert“ aufweist: 

      ![Registrierte Microsoft.NetApp](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Nächste Schritte  

[Erstellen eines NetApp-Kontos](azure-netapp-files-create-netapp-account.md)
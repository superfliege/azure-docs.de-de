---
title: Includedatei
description: Includedatei
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 04/13/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: 210698ffdc523f59220d4215a982f77361c9b6a4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen CDN-Profils

Ein CDN-Profil ist ein Container für CDN-Endpunkte und gibt einen Tarif an.

1. Klicken Sie links oben im Azure-Portal auf **Ressource erstellen**.
    
    Der Bereich **Neu** wird angezeigt.
   
2. Klicken Sie auf **Web + Mobil** und anschließend auf **CDN**.
   
    ![Klicken auf die Ressource „CDN“](./media/cdn-create-profile/cdn-new-resource.png)

    Der Bereich **CDN-Profil** wird angezeigt.

    Verwenden Sie die Einstellungen aus der Tabelle unter der folgenden Abbildung.
   
    ![Neues CDN-Profil](./media/cdn-create-profile/cdn-new-profile.png)

    | Einstellung  | Wert |
    | -------- | ----- |
    | **Name** | Geben Sie *my-cdn-profile-123* als Profilname ein. Dieser Name muss global eindeutig sein. Sollte er bereits verwendet werden, können Sie einen anderen Namen eingeben. |
    | **Abonnement** | Wählen Sie in der Dropdownliste ein Azure-Abonnement aus.|
    | **Ressourcengruppe** | Klicken Sie auf **Neu erstellen**, und geben Sie *my-resource-group-123* als Ressourcengruppennamen ein. Dieser Name muss global eindeutig sein. Sollte er bereits verwendet werden, können Sie einen anderen Namen eingeben. | 
    | **Ressourcengruppenstandort** | Wählen Sie in der Dropdownliste die Option **USA, Mitte** aus. |
    | **Preisstufe** | Wählen Sie in der Dropdownliste die Option **Verizon Standard** aus. |
    | **Jetzt neuen CDN-Endpunkt erstellen** | Lassen Sie das Kontrollkästchen deaktiviert. |  
   
3. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, um das erstellte Profil in Ihrem Dashboard zu speichern.
    
4. Wählen Sie **Erstellen**, um das Profil zu erstellen. 

    Bei **Azure CDN Standard von Microsoft** ist die Profilerstellung in der Regel in zwei Stunden abgeschlossen. 


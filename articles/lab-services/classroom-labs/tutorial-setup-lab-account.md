---
title: Einrichten eines Lab-Kontos mit Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie ein Lab-Konto mit Azure Lab Services ein.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: 72acc4fe0febaa09d6deb82043795b6003af683f
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706773"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Tutorial: Einrichten eines Lab-Kontos mit Azure Lab Services
In Azure Lab Services fungiert ein Lab-Konto als zentrales Konto, unter dem die Labs Ihrer Organisation verwaltet werden. In Ihrem Lab-Konto können Sie anderen Benutzern die Berechtigung zum Erstellen von Labs erteilen und Richtlinien festlegen, die für alle Labs unter dem Lab-Konto gelten. In diesem Tutorial erfahren Sie, wie Sie als Lab-Administrator ein Lab-Konto erstellen. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Lab-Kontos
> * Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
> * Angeben eines für Lab-Besitzer verfügbaren Marketplace-Images

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
Die folgenden Schritte veranschaulichen, wie Sie Azure-Portal verwenden, um ein Lab-Konto in Azure Lab Services zu erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Hauptmenü auf der linken Seite **Ressource erstellen** aus.
3. Suchen Sie im Azure Marketplace nach **Lab Services**, und wählen Sie in der Dropdownliste **Lab Services** aus. 
4. Wählen Sie in der gefilterten Liste der Dienste **Lab Services (Vorschauversion)** aus. 
1. Wählen Sie im Fenster **Lab-Konto erstellen** die Option **Erstellen** aus.
2. Führen Sie im Fenster **Lab-Konto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Wenn die Seite für das Lab-Konto nicht angezeigt wird, klicken Sie auf die Schaltfläche **Benachrichtigungen** und dann in den Benachrichtigungen auf die Schaltfläche **Zu Ressource wechseln**. 

    ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, können Sie diesen Schritt überspringen. Führen Sie die folgenden Schritte aus, um zum Erstellen eines Classroom-Labs ein anderes Benutzerkonto zu verwenden: 

Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle **Lab-Ersteller** hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und klicken Sie auf der Symbolleiste auf **+ Hinzufügen**. 

    ![Seite des Lab-Kontos](../media/tutorial-setup-lab-account/access-control.png)
2. Wählen Sie auf der Seite **Berechtigungen hinzufügen** als **Rolle** die Option **Lab-Ersteller** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Lab-Ersteller“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Festlegen von Marketplace-Images für Lab-Besitzer
Als Lab-Kontobesitzer können Sie die Marketplace-Images festlegen, die Lab-Ersteller zum Erstellen von Labs im Lab-Konto verwenden können. 

1. Wählen Sie links im Menü die Option **Marketplace-Images** aus. Standardmäßig wird die vollständige Liste der Images (sowohl aktivierte als auch deaktivierte) angezeigt. Sie können die Liste filtern, um nur aktivierte bzw. deaktivierte Images anzuzeigen. Wählen Sie dazu in der Dropdownliste oben die Option **Enabled only**/**Disabled only** (Nur aktivierte/Nur deaktivierte). 
    
    ![Seite mit Marketplace-Images](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Die Marketplace-Images, die in der Liste angezeigt werden, sind die einzigen, die die folgenden Bedingungen erfüllen:
        
    - Erstellt einen einzelnen virtuellen Computer
    - Verwendet den Azure Resource Manager zum Bereitstellen von virtuellen Computern
    - Erfordert nicht den Erwerb eines zusätzlichen Lizenzplans
2. Um ein aktiviertes Marketplace-Image zu **deaktivieren**, führen Sie eine der folgenden Aktionen aus: 
    1. Klicken Sie in der letzten Spalte auf die Ellipse (**...**) und dann auf **Disable image** (Image deaktivieren). 

        ![Deaktivieren eines Images](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Wählen Sie mindestens ein Image in der Liste aus, indem Sie die Kontrollkästchen vor den Imagenamen in der Liste aktivieren, und wählen Sie **Ausgewählte Images deaktivieren** aus. 

        ![Deaktivieren mehrerer Images](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Ebenso führen Sie zum **Aktivieren** eines Marketplace-Images eine der folgenden Aktionen aus: 
    1. Klicken Sie in der letzten Spalte auf die Ellipse (**...**) und dann auf **Enable image** (Image aktivieren). 
    2. Wählen Sie mindestens ein Image in der Liste aus, indem Sie die Kontrollkästchen vor den Imagenamen in der Liste aktivieren, und wählen Sie **Ausgewählte Images aktivieren** aus. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab-Konto erstellt. Um zu erfahren, wie Sie als beruflicher Benutzer ein Classroom-Lab erstellen, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)


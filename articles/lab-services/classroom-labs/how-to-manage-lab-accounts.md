---
title: Verwalten von Lab-Konten in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in einem Azure-Abonnement ein Lab-Konto erstellen, alle Lab-Konten anzeigen oder ein Lab-Konto löschen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: c672634e79cbc0850edfe7f7fbdb0a880d5cf0d8
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707128"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Verwalten von Lab-Konten in Azure Lab Services 
In Azure Lab Services ist ein Lab-Konto ein Container für verwaltete Labs (beispielsweise Classroom-Labs). Ein Administrator richtet ein Lab-Konto mit Azure Lab Services ein und gewährt Lab-Besitzern Zugriff, sodass sie Labs unter dem Konto erstellen können. In diesem Artikel erfahren Sie, wie Sie ein Lab-Konto erstellen, alle Lab-Konten anzeigen oder ein Lab-Konto löschen.

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Hauptmenü auf der linken Seite **Ressource erstellen** aus.
3. Suchen Sie im Azure Marketplace nach **Lab Services**, und wählen Sie in der Dropdownliste **Lab Services** aus. 
4. Wählen Sie in der gefilterten Liste der Dienste **Lab Services (Vorschauversion)** aus. 
5. Wählen Sie im Fenster **Lab-Konto erstellen** die Option **Erstellen** aus.
7. Führen Sie im Fenster **Lab-Konto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Wenn die Seite für das Lab-Konto nicht angezeigt wird, klicken Sie auf die Schaltfläche **Benachrichtigungen** und dann in den Benachrichtigungen auf die Schaltfläche **Zu Ressource wechseln**. 

    ![Fenster zum Erstellen eines Lab-Kontos](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, können Sie diesen Schritt überspringen. Führen Sie die folgenden Schritte aus, um zum Erstellen eines Classroom-Labs ein anderes Benutzerkonto zu verwenden: 

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

## <a name="view-lab-accounts"></a>Anzeigen von Lab-Konten
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü **Alle Ressourcen** aus. 
3. Wählen Sie für **Typ** die Option **Lab Services**. 
    Sie können auch nach Abonnement, Ressourcengruppe, Speicherorten und Tags filtern. 

## <a name="delete-a-lab-account"></a>Löschen eines Lab-Kontos
Befolgen Sie die Anleitung im vorherigen Abschnitt, um Lab-Konten in einer Liste anzuzeigen. Gehen Sie folgendermaßen vor, um ein Lab-Konto zu löschen: 

1. Wählen Sie das **Lab-Konto** aus, das Sie löschen möchten. 
2. Klicken Sie auf der Symbolleiste auf **Löschen**. 
3. Geben Sie zur Bestätigung **Ja** ein.
4. Klicken Sie auf **Löschen**. 

## <a name="view-and-manage-labs-in-the-lab-account"></a>Anzeigen und Verwalten von Labs im Labkonto

1. Wählen Sie auf der Seite **Labkonto** im Menü auf der linken Seite die Option **Labs**.

    ![Labs im Konto](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Im Konto wird eine **Liste mit Labs** mit den folgenden Informationen angezeigt: 
    1. Name des Labs
    2. Datum, an dem das Lab erstellt wurde 
    3. E-Mail-Adresse des Benutzers, der das Lab erstellt hat 
    4. Maximale Anzahl von Benutzern, die für das Lab zulässig ist 
    5. Status des Labs 

## <a name="delete-a-lab-in-the-lab-account"></a>Löschen eines Labs im Labkonto
Befolgen Sie die Anleitung im vorherigen Abschnitt, um im Labkonto eine Liste mit den Labs anzuzeigen.

1. Wählen Sie **...** (Auslassungszeichen) und dann **Löschen**. 

    ![Lab löschen – Schaltfläche](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Wählen Sie in der Warnmeldung die Option **Ja**. 



## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
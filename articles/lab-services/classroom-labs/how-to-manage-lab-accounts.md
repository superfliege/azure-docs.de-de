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
ms.date: 02/07/2018
ms.author: spelluru
ms.openlocfilehash: 7e3142e0274f2328d3e0c8a3e6f9a2e4c3d45d87
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55959136"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Verwalten von Lab-Konten in Azure Lab Services 
In Azure Lab Services ist ein Lab-Konto ein Container für verwaltete Labs (beispielsweise Classroom-Labs). Ein Administrator richtet ein Lab-Konto mit Azure Lab Services ein und gewährt Lab-Besitzern Zugriff, sodass sie Labs unter dem Konto erstellen können. In diesem Artikel erfahren Sie, wie Sie ein Lab-Konto erstellen, alle Lab-Konten anzeigen oder ein Lab-Konto löschen.

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
Die folgenden Schritte veranschaulichen, wie Sie Azure-Portal verwenden, um ein Lab-Konto in Azure Lab Services zu erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus. Wählen Sie im Abschnitt **DEVOPS** die Option **Labkonten**. Wenn Sie das Sternchen (`*`) neben **Labkonten** auswählen, wird die Option im linken Menü dem Abschnitt **FAVORITEN** hinzugefügt. Beim nächsten Mal wählen Sie **Labkonten** unter **FAVORITEN** aus.

    ![Alle Dienste -> Labkonten](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Klicken Sie auf der Seite **Labkonten** auf der Symbolleiste auf **Hinzufügen**. 

    ![Auswählen von „Hinzufügen“ auf der Seite „Labkonten“](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Führen Sie auf der Seite **Labkonto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Wählen Sie auf der Symbolleiste das **Glockensymbol** (**Benachrichtigungen**), überprüfen Sie, ob die Bereitstellung erfolgreich war, und wählen Sie dann **Zu Ressource wechseln**. 

    Wählen Sie alternativ auf der Seite **Labkonten** die Option **Aktualisieren**, und wählen Sie anschließend das von Ihnen erstellte Labkonto aus. 

    ![Fenster zum Erstellen eines Lab-Kontos](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Zum Einrichten eines Classroom-Labs in einem Labkonto muss der Benutzer Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen des Labkontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Wenn Sie zum Erstellen eines Classroom-Labs das gleiche Benutzerkonto verwenden möchten, können Sie diesen Schritt überspringen. Führen Sie die folgenden Schritte aus, um zum Erstellen eines Classroom-Labs ein anderes Benutzerkonto zu verwenden: 

Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle **Lab-Ersteller** hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und klicken Sie auf der Symbolleiste auf **+ Rollenzuweisung hinzufügen**. 

    ![Zugriffssteuerung > Schaltfläche „Rollenzuweisung hinzufügen“](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** als **Rolle** die Option **Ersteller des Labs** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Ersteller des Labs“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen des Lab-Erstellers](../media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="specify-marketplace-images-available-to-lab-creators"></a>Angeben von für Lab-Ersteller verfügbare Marketplace-Images
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
3. Wählen Sie für **Typ** die Option **Labkonten**. 
    Sie können auch nach Abonnement, Ressourcengruppe, Speicherorten und Tags filtern. 

    ![Alle Ressourcen -> Labkonten](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)


## <a name="delete-a-lab-account"></a>Löschen eines Lab-Kontos
Befolgen Sie die Anleitung im vorherigen Abschnitt, um Lab-Konten in einer Liste anzuzeigen. Gehen Sie folgendermaßen vor, um ein Lab-Konto zu löschen: 

1. Wählen Sie das **Lab-Konto** aus, das Sie löschen möchten. 
2. Klicken Sie auf der Symbolleiste auf **Löschen**. 

    ![Labkonten -> Schaltfläche „Löschen“](../media/how-to-manage-lab-accounts/delete-button.png)
1. Geben Sie zur Bestätigung **Ja** ein.
1. Klicken Sie auf **Löschen**. 

    ![Labkonto löschen – Bestätigung](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

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

    ![Bestätigen des Löschvorgangs](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
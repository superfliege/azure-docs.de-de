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
ms.date: 04/20/2018
ms.author: spelluru
ms.openlocfilehash: 09303a4d4fc730aae6fc7c04148d64b0f57e69ac
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Verwalten von Lab-Konten in Azure Lab Services (früher Azure DevTest Labs)
In diesem Artikel erfahren Sie, wie Sie ein Lab-Konto erstellen, alle Lab-Konten anzeigen oder ein Lab-Konto löschen.

## <a name="prerequisites"></a>Voraussetzungen
Azure Lab Services befindet sich derzeit in einer abgegrenzten Vorschauphase. Um ein Lab-Konto zu erstellen, [registrieren Sie sich für die Vorschauversion](https://aka.ms/azlabspreviewsignup).

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Hauptmenü auf der linken Seite oben in der Liste den Eintrag **Ressource erstellen** aus, zeigen Sie auf **Entwicklertools**, und klicken Sie auf **Lab Services (Vorschau)**.
1. Wählen Sie im Fenster **Lab-Konto erstellen** die Option **Erstellen** aus.
2. Führen Sie im Fenster **Lab-Konto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Wenn die Seite für das Lab-Konto nicht angezeigt wird, klicken Sie auf die Schaltfläche **Benachrichtigungen** und dann in den Benachrichtigungen auf die Schaltfläche **Zu Ressource wechseln**. 

    ![Fenster zum Erstellen eines Lab-Kontos](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Veranstaltungen zu erteilen, fügen Sie sie zur Rolle „Lab-Ersteller“ hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und klicken Sie auf der Symbolleiste auf **+ Hinzufügen**. 

    ![Seite des Lab-Kontos](./media/tutorial-setup-lab-account/access-control.png)
2. Wählen Sie auf der Seite **Berechtigungen hinzufügen** als **Rolle** die Option **Lab-Ersteller** aus. Wählen Sie den Benutzer aus, den Sie der Rolle „Lab-Ersteller“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


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

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)
- [Einrichten eines benutzerdefinierten Labs](tutorial-create-custom-lab.md)

---
title: Einrichten eines Lab-Kontos mit Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie ein Lab-Konto mit Azure Lab Services ein (früher als DevTest Labs bezeichnet).
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
ms.date: 04/26/2018
ms.author: spelluru
ms.openlocfilehash: 5af348bfdccb9392948af962cf582e33ebd40872
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services-formerly-azure-devtest-labs"></a>Tutorial: Einrichten eines Lab-Kontos mit Azure Lab Services ein (früher als Azure DevTest Labs bezeichnet)
In diesem Tutorial agieren Sie als Lab-Administrator, um ein Lab-Konto mit Azure Lab Services zu erstellen. Danach erteilen Sie Lehrkräften die Berechtigung, in diesem Lab-Konto Labs für ihre Klassen zu erstellen. Eine Lehrkraft kann über die [Azure Lab Services-Website](https://labs.azure.com) ein Lab für eine Klasse einrichten.   

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Lab-Kontos
> * Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Azure Lab Services befindet sich derzeit in einer geschlossenen Vorschau. Um ein Lab-Konto zu erstellen, [registrieren Sie sich für die Vorschauversion](https://aka.ms/azlabspreviewsignup).

## <a name="create-a-lab-account"></a>Erstellen eines Lab-Kontos
Die folgenden Schritte veranschaulichen, wie Sie Azure-Portal verwenden, um ein Lab-Konto in Azure Lab Services zu erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Hauptmenü auf der linken Seite oben in der Liste den Eintrag **Ressource erstellen** aus, zeigen Sie auf **Entwicklertools**, und klicken Sie auf **Lab Services (Vorschau)**.
1. Wählen Sie im Fenster **Lab-Konto erstellen** die Option **Erstellen** aus.
2. Führen Sie im Fenster **Lab-Konto** die folgenden Aktionen aus: 
    1. Geben Sie einen **Lab-Kontonamen** ein. 
    2. Wählen Sie das **Azure-Abonnement** aus, in dem Sie das Lab-Konto erstellen möchten.
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    4. Wählen Sie als **Standort** einen Standort oder eine Region aus, in dem bzw. der das Lab-Konto erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 

        ![Fenster zum Erstellen eines Lab-Kontos](./media/tutorial-setup-lab-account/lab-account-settings.png)
5. Wenn die Seite für das Lab-Konto nicht angezeigt wird, klicken Sie auf die Schaltfläche **Benachrichtigungen**, und klicken Sie dann in den Benachrichtigungen auf die Schaltfläche **Zu Ressource wechseln**. 

    ![Fenster zum Erstellen eines Lab-Kontos](./media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Folgende Seite für das **Lab-Konto** wird angezeigt:

    ![Seite des Lab-Kontos](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“
Um Lehrkräften die Berechtigung zum Erstellen von Labs für ihre Klassen zu erteilen, fügen Sie sie zur Rolle „Lab-Ersteller“ hinzu:

1. Wählen Sie auf der Seite **Lab-Konto** die Option **Zugriffssteuerung (IAM)** aus, und klicken Sie auf der Symbolleiste auf **+ Hinzufügen**. 

    ![Seite des Lab-Kontos](./media/tutorial-setup-lab-account/access-control.png)
2. Wählen Sie auf der Seite **Berechtigungen hinzufügen** als **Rolle** die Option **Lab-Ersteller** aus, wählen Sie den Benutzer aus, den Sie der Rolle „Lab-Ersteller“ hinzufügen möchten, und klicken Sie auf **Speichern**. 

    ![Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab-Konto erstellt. Um zu erfahren, wie Sie als beruflicher Benutzer ein Classroom-Lab erstellen, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)


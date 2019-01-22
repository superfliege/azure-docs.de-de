---
title: Verwalten von Teams und untergeordneten Teams in der Content Moderator-API – Content Moderator
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Teams und untergeordnete Teams in der Content Moderator API für Cognitive Services verwenden.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e45e06bdcc69dc88d8c979bfbb5a0974f7284b22
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266228"
---
# <a name="manage-review-teams-and-subteams"></a>Verwalten von Überprüfungsteams und untergeordneten Überprüfungsteams

Vor der Verwendung von Content Moderator müssen Sie ein Team erstellen. Wenn Sie Ihr Team registrieren und benennen, ist dieses Team Ihr Standardteam. Das Prüfungstool kann nur ein Team enthalten. Sie können jedoch mehrere untergeordnete Teams erstellen. Untergeordnete Teams sind nützlich für die Erstellung von Eskalationsteams oder Teams, die sich mit der Überprüfung bestimmter Inhaltskategorien befassen. Beispielsweise können Sie Inhalte für Erwachsene zur zusätzlichen Überprüfung an ein anderes Team senden.

In diesem Thema wird erklärt, wie Sie untergeordnete Teams erstellen und Überprüfungen schnell und einfach zuweisen können. Sie können jedoch [Workflows](workflows.md) verwenden, um Überprüfungen anhand bestimmter Kriterien zuzuweisen.

## <a name="go-to-the-teams-setting"></a>Aufrufen der Teameinstellungen

Um mit der Erstellung eines untergeordneten Teams zu beginnen, wählen Sie unter „Einstellungen“ die Option **Teams**.

![Teameinstellungen](images/0-teams-1.png)

## <a name="create-subteams"></a>Erstellen von untergeordneten Teams

Das Standardteam enthält alle potenziellen Prüfer; untergeordnete Teams sind Teilmengen des Standardteams. Sie können keine Person einem untergeordneten Team zuweisen, wenn diese Person nicht bereits Mitglied des Standardteams ist, daher müssen Sie jetzt dem Standardteam weitere Prüfer hinzufügen. Klicken Sie auf der Teamseite auf die Schaltfläche „Einladung“.

![Benutzer einladen](images/invite-users.png)

### <a name="create-a-subteam"></a>Erstellen eines untergeordnetes Teams
Scrollen Sie auf der Teamseite nach unten zum Abschnitt „Untergeordnetes Team“. Klicken Sie auf die Schaltfläche „Untergeordnetes Team hinzufügen“. 

![Untergeordnetes Team hinzufügen](images/1-teams-1.png)

### <a name="name-your-subteam"></a>Benennen des untergeordneten Teams
Geben Sie den Namen Ihres untergeordneten Teams im Dialogfeld ein, und klicken Sie auf „Speichern“.

![Name des untergeordneten Teams](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>Fügen Sie Mitglieder zu Ihrem Standardteam hinzu.
Klicken Sie auf „Mitglied hinzufügen“, um Mitglieder Ihres Standardteam zu einem oder mehreren untergeordneten Teams hinzuzufügen. Sie können nur vorhandene Benutzer zu einem untergeordneten Team hinzufügen. Um neue Benutzer hinzuzufügen, die noch nicht ins Prüfungstool aufgenommen wurden, laden Sie sie ein, indem Sie auf der Seite mit den Teameinstellungen auf „Einladung“ klicken.

![Mitglieder zu untergeordneten Teams hinzufügen](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Zuweisen von Überprüfungen zu Ihren untergeordneten Teams

Sobald Sie Ihre untergeordneten Teams erstellt und Teammitglieder zugewiesen haben, können Sie mit der Zuordnung von Bild- und Textüberprüfungen zu diesen untergeordneten Teams beginnen. Dies erfolgt im Fenster „Überprüfung“.
Wenn Sie ein einzelnes Bild einem untergeordneten Team zuweisen möchten, klicken Sie auf die Auslassungspunkte in der oberen rechten Ecke des Bildes, wählen Sie „Verschieben nach“ und wählen Sie ein untergeordnetes Team aus.

![Bildüberprüfung einem untergeordneten Team zuweisen](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Wechseln zwischen untergeordneten Teams zur Überprüfung zugewiesener Inhalte

Wenn Sie Mitglied in einem oder mehreren untergeordneten Teams sind, können Sie im Dashboard des Prüfungstools zwischen diesen untergeordneten Teams wechseln. Um alle aktuell ausstehenden Überprüfungen eines untergeordneten Teams anzuzeigen, wählen Sie auf der Registerkarte „Bild“ die Option „Untergeordnetes Team auswählen“.

![Zwischen untergeordneten Teams wechseln](images/3-review-image-subteam-2.png)

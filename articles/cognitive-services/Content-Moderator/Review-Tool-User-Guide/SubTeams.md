---
title: Teams und untergeordnete Teams in der Content Moderator-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Teams und untergeordnete Teams in der Content Moderator API für Cognitive Services verwenden.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 6e1fc08af1062ae8962ba33c6df980182175264b
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852145"
---
# <a name="team-and-subteams"></a>Teams und untergeordnete Teams #

Vor der Verwendung von Content Moderator müssen Sie ein Team erstellen. Wenn Sie Ihr Team registrieren und benennen, ist dieses Team Ihr Standardteam. Das Prüfungstool kann nur ein Team enthalten. Sie können jedoch mehrere untergeordnete Teams erstellen. Untergeordnete Teams sind nützlich für die Erstellung von Eskalationsteams oder Teams, die sich mit der Überprüfung bestimmter Inhaltskategorien befassen. Beispielsweise können Sie Inhalte für Erwachsene zur zusätzlichen Überprüfung an ein anderes Team senden.

In diesem Thema wird erklärt, wie Sie untergeordnete Teams erstellen und Überprüfungen schnell und einfach zuweisen können. Sie können jedoch [Workflows](workflows.md) verwenden, um Überprüfungen anhand bestimmter Kriterien zuzuweisen.

## <a name="go-to-the-teams-setting"></a>Aufrufen der Teameinstellungen ##

Um mit der Erstellung eines untergeordneten Teams zu beginnen, wählen Sie unter „Einstellungen“ die Option **Teams**.

![Teameinstellungen](images/0-teams-1.png)

## <a name="create-subteams"></a>Erstellen von untergeordneten Teams ##

Das Standardteam enthält alle potenziellen Prüfer; untergeordnete Teams sind Teilmengen des Standardteams. Sie können keine Person einem untergeordneten Team zuweisen, wenn diese Person nicht bereits Mitglied des Standardteams ist, daher müssen Sie jetzt dem Standardteam weitere Prüfer hinzufügen. Klicken Sie auf der Teamseite auf die Schaltfläche „Einladung“.

![Benutzer einladen](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Erstellen Sie ein untergeordnetes Team.
Scrollen Sie auf der Teamseite nach unten zum Abschnitt „Untergeordnetes Team“. Klicken Sie auf die Schaltfläche „Untergeordnetes Team hinzufügen“. 

![Untergeordnetes Team hinzufügen](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Benennen Sie Ihr untergeordnetes Team.
Geben Sie den Namen Ihres untergeordneten Teams im Dialogfeld ein, und klicken Sie auf „Speichern“.

![Name des untergeordneten Teams](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Fügen Sie Mitglieder zu Ihrem Standardteam hinzu.
Klicken Sie auf „Mitglied hinzufügen“, um Mitglieder Ihres Standardteam zu einem oder mehreren untergeordneten Teams hinzuzufügen. Sie können nur vorhandene Benutzer zu einem untergeordneten Team hinzufügen. Um neue Benutzer hinzuzufügen, die noch nicht ins Prüfungstool aufgenommen wurden, laden Sie sie ein, indem Sie auf der Seite mit den Teameinstellungen auf „Einladung“ klicken.

![Mitglieder zu untergeordneten Teams hinzufügen](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Zuweisen von Überprüfungen zu Ihren untergeordneten Teams ##

Sobald Sie Ihre untergeordneten Teams erstellt und Teammitglieder zugewiesen haben, können Sie mit der Zuordnung von Bild- und Textüberprüfungen zu diesen untergeordneten Teams beginnen. Dies erfolgt im Fenster „Überprüfung“.
Wenn Sie ein einzelnes Bild einem untergeordneten Team zuweisen möchten, klicken Sie auf die Auslassungspunkte in der oberen rechten Ecke des Bildes, wählen Sie „Verschieben nach“ und wählen Sie ein untergeordnetes Team aus.

![Bildüberprüfung einem untergeordneten Team zuweisen](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Wechseln zwischen untergeordneten Teams zur Überprüfung zugewiesener Inhalte ##

Wenn Sie Mitglied in einem oder mehreren untergeordneten Teams sind, können Sie im Dashboard des Prüfungstools zwischen diesen untergeordneten Teams wechseln. Um alle aktuell ausstehenden Überprüfungen eines untergeordneten Teams anzuzeigen, wählen Sie auf der Registerkarte „Bild“ die Option „Untergeordnetes Team auswählen“.

![Zwischen untergeordneten Teams wechseln](images/3-review-image-subteam-2.png)

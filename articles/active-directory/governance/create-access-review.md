---
title: Erstellen einer Zugriffsüberprüfung für Gruppen oder Anwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Gruppenmitglieder oder den Anwendungszugriff betreffende Zugriffsüberprüfung in Azure Active Directory-Zugriffsüberprüfungen erstellen.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/01/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 804efa6e0a39e009e18bbb9dec5ad1638a163597
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495053"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Erstellen einer Zugriffsüberprüfung für Gruppen oder Anwendungen in Azure AD-Zugriffsüberprüfungen

Der Zugriff auf Gruppen und Anwendungen für Mitarbeiter und Gäste ändert sich im Laufe der Zeit. Zur Senkung der Risiken im Zusammenhang mit veralteten Zugriffszuweisungen können Administratoren mithilfe von Azure Active Directory (Azure AD) Zugriffsüberprüfungen für Gruppenmitglieder oder Anwendungszugriff erstellen. Für eine routinemäßige Überprüfung können bei Bedarf auch wiederkehrende Zugriffsüberprüfungen erstellt werden. Weitere Informationen zu diesen Szenarien finden Sie unter [Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen](manage-user-access-with-access-reviews.md) sowie unter [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](manage-guest-access-with-access-reviews.md).

In diesem Artikel wird die Erstellung einer oder mehrerer Zugriffsüberprüfungen für Gruppenmitglieder oder Anwendungszugriff beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- [Aktivierte Zugriffsüberprüfungen](access-reviews-overview.md)
- Globaler Administrator oder Benutzeradministrator

## <a name="create-one-or-more-access-reviews"></a>Erstellen einer oder mehrerer Zugriffsüberprüfungen

1. Melden Sie sich beim Azure-Portal an, und öffnen Sie die [Seite „Zugriffsüberprüfungen“](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Klicken Sie im linken Menü auf **Zugriffsüberprüfungen**.

1. Klicken Sie auf **Neue Zugriffsüberprüfung**, um eine neue Zugriffsüberprüfung zu erstellen.

    ![Zugriffsüberprüfung – Steuerelemente](./media/create-access-review/access-reviews.png)

1. Benennen Sie die Zugriffsüberprüfung. Wahlweise können Sie jeder Überprüfung eine Beschreibung hinzufügen. Den Prüfern werden Name und Beschreibung angezeigt.

    ![Erstellen einer Zugriffsüberprüfung – Name und Beschreibung der Überprüfung](./media/create-access-review/name-description.png)

1. Legen Sie das **Startdatum** fest. Standardmäßig findet eine Zugriffsüberprüfung einmalig statt. Sie beginnt an dem Tag, an dem sie erstellt wird, und endet nach einem Monat. Sie können Start- und Enddatum so ändern, dass der Start der Zugriffsüberprüfung in der Zukunft liegt und sie so lange dauert, wie Sie wünschen.

    ![Erstellen einer Zugriffsüberprüfung – Start- und Enddatum](./media/create-access-review/start-end-dates.png)

1. Wenn Sie eine Zugriffsüberprüfungsserie erstellen möchten, ändern Sie die Einstellung **Häufigkeit** von **Einmal** in **Wöchentlich**, **Monatlich**,  **Vierteljährlich** oder **Jährlich**. Verwenden Sie den Schieberegler oder das Textfeld **Dauer**, um festzulegen, wie viele Tage Prüfer Eingaben für jede Überprüfung der Serie vornehmen können. Für eine monatliche Überprüfung kann beispielsweise eine maximale Dauer von 27 Tagen angegeben werden, um Überschneidungen zu vermeiden.

1. Geben Sie mithilfe der Einstellung **Ende** an, wie die wiederkehrende Zugriffsüberprüfungsreihe beendet werden soll. Die Reihe kann auf drei Arten enden: Die Serie wird unendlich ausgeführt, um Überprüfungen ohne zeitliche Beschränkung zu starten, die Serie wird bis zu einem bestimmten Datum ausgeführt, oder sie wird nach einer bestimmten Anzahl von Vorkommen beendet. Sie (oder ein anderer Benutzeradministrator oder globaler Administrator) können die Serie nach der Erstellung beenden, indem Sie unter **Einstellungen** das Datum ändern, sodass die Serie an diesem Datum endet.

1. Geben Sie im Abschnitt **Benutzer** die Benutzer an, für die die Zugriffsüberprüfung gelten soll. Zugriffsüberprüfungen können für die Mitglieder einer Gruppe oder für Benutzer erfolgen, die einer Anwendung zugewiesen wurden. Sie können die Zugriffsüberprüfung weiter anpassen, um nur die Gastbenutzer zu überprüfen, die Mitglieder (oder der Anwendung zugewiesen) sind, anstatt alle Benutzer zu überprüfen, die Mitglieder sind oder Zugriff auf die Anwendung haben.

    ![Erstellen einer Zugriffsüberprüfung – Benutzer](./media/create-access-review/users.png)

1. Wählen Sie im Abschnitt **Gruppe** eine oder mehrere Gruppen aus, deren Mitgliedschaft überprüft werden soll.

    > [!NOTE]
    > Bei der Auswahl mehrerer Gruppen werden mehrere Zugriffsüberprüfungen erstellt. Bei der Auswahl von fünf Gruppen werden z. B. fünf separate Zugriffsüberprüfungen erstellt.
    
    ![Erstellen einer Zugriffsüberprüfung – Gruppe auswählen](./media/create-access-review/select-group.png)

1. Wählen Sie im Abschnitt **Anwendungen** (wenn Sie in Schritt 8 **Zugewiesen zu einer Anwendung** ausgewählt haben) die Anwendungen aus, für die Sie den Zugriff überprüfen möchten.

    > [!NOTE]
    > Bei der Auswahl mehrerer Anwendungen werden mehrere Zugriffsüberprüfungen erstellt. Bei der Auswahl von fünf Anwendungen werden z. B. fünf separate Zugriffsüberprüfungen erstellt.
    
    ![Erstellen einer Zugriffsüberprüfung – Anwendung auswählen](./media/create-access-review/select-application.png)

1. Wählen Sie im Abschnitt **Prüfer** mindestens eine Person für die Überprüfung aller Benutzer des Bereichs aus. Alternativ können Sie auswählen, dass die Mitglieder ihren eigenen Zugriff überprüfen. Wenn es sich bei der Ressource um eine Gruppe handelt, können Sie die Gruppenbesitzer um die Überprüfung bitten. Sie können auch festlegen, dass die Prüfer einen Grund angeben müssen, wenn sie den Zugriff genehmigen.

    ![Erstellen einer Zugriffsüberprüfung – Prüfer](./media/create-access-review/reviewers.png)

1. Wählen Sie im Abschnitt **Programme** das gewünschte Programm aus. Sie können die Nachverfolgung und das Erfassen von Zugriffsüberprüfungen für verschiedene Zwecke vereinfachen, indem Sie diese Aufgaben in Programmen organisieren. Das **Standardprogramm** ist immer vorhanden. Sie können aber auch ein anderes Programm erstellen. Sie können z.B. ein Programm für jede Konformitätsinitiative oder für jedes Geschäftsziel verwenden.

    ![Erstellen einer Zugriffsüberprüfung – Programme](./media/create-access-review/programs.png)

### <a name="upon-completion-settings"></a>Einstellungen nach Abschluss

1. Erweitern Sie den Abschnitt **Einstellungen nach Abschluss**, um anzugeben, was nach Abschluss der Überprüfung geschehen soll.

    ![Einstellungen nach Abschluss](./media/create-access-review/upon-completion-settings.png)

1. Soll abgelehnten Benutzern automatisch der Zugriff entzogen werden, legen Sie **Ergebnisse automatisch auf Ressource anwenden** auf **Aktivieren** fest. Falls Sie die Ergebnisse nach Abschluss der Überprüfung manuell anwenden möchten, legen Sie die Einstellung auf **Deaktivieren** fest.

1. Geben Sie mithilfe der Liste **Falls der Prüfer nicht reagiert** an, was bei Benutzern geschehen soll, die vom Prüfer nicht innerhalb des vorgesehenen Zeitraums überprüft werden. Diese Einstellung hat keine Auswirkungen auf Benutzer, die von den Prüfern manuell überprüft wurden. Lautet die Entscheidung des Prüfers letztlich „Verweigern“, wird dem Benutzer der Zugriff entzogen.

    - **Keine Änderung**: Der Zugriff des Benutzers bleibt unverändert.
    - **Zugriff entfernen**: Dem Benutzer wird der Zugriff entzogen.
    - **Zugriff genehmigen**: Der Zugriff des Benutzers wird genehmigt.
    - **Empfehlungen annehmen**: Die Systemempfehlungen hinsichtlich der Ablehnung oder Gewährung des weiteren Benutzerzugriffs werden verwendet.

### <a name="advanced-settings"></a>Erweiterte Einstellungen

1. Erweitern Sie den Abschnitt **Erweiterte Einstellungen**, um weitere Einstellungen anzugeben.

    ![Erweiterte Einstellungen](./media/create-access-review/advanced-settings.png)

1. Legen Sie **Empfehlungen anzeigen** auf **Aktivieren** fest, damit den Prüfern die Systemempfehlungen auf der Grundlage der Zugriffsinformationen des Benutzers angezeigt werden.

1. Legen Sie **Bei Genehmigung Grund anfordern** auf **Aktivieren** fest, damit der Prüfer einen Grund für die Genehmigung angeben muss.

1. Legen Sie **E-Mail-Benachrichtigungen** auf **Aktivieren** fest, damit Azure AD beim Start einer Zugriffsüberprüfung E-Mail-Benachrichtigungen an die Prüfer und beim Abschluss einer Überprüfung Benachrichtigungen an Administratoren sendet.

1. Legen Sie **Erinnerungen** auf **Aktivieren** fest, damit Azure AD Erinnerungen zu laufenden Zugriffsüberprüfungen an Prüfer sendet, die ihre Überprüfung noch nicht abgeschlossen haben.

## <a name="start-the-access-review"></a>Starten der Zugriffsüberprüfung

Klicken Sie nach dem Festlegen der Einstellungen für eine Zugriffsüberprüfung auf **Starten**. Die Zugriffsüberprüfung wird in der Liste mit einer Angabe des Status angezeigt.

![Liste mit den Zugriffsüberprüfungen](./media/create-access-review/access-reviews-list.png)

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Überprüfen des Zugriffs auf Gruppen oder Anwendungen](perform-access-review.md) anzeigen. Wenn Ihre Überprüfung für Gäste gedacht ist, die ihren eigenen Zugriff überprüfen sollen, können Sie ihnen die Anweisungen zum [Überprüfen des eigenen Zugriffs auf Gruppen oder Anwendungen ](review-your-access.md) anzeigen.

Wenn es sich bei einigen Prüfern um Gäste handelt: Gäste werden nur dann per E-Mail benachrichtigt, wenn sie die Einladung bereits angenommen haben.

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Sie können den Fortschritt der Überprüfungen durch die Prüfer auf der Seite **Übersicht** der Zugriffsüberprüfung nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](complete-access-review.md) ist.

![Fortschritt der Zugriffsüberprüfungen](./media/create-access-review/overview-progress.png)

Einmalige Überprüfung: Führen Sie nach Ablauf des Zugriffsüberprüfungszeitraums oder nach Beenden der Zugriffsüberprüfung durch den Administrator die Schritte unter [Durchführen einer Gruppen oder Anwendungen betreffenden Zugriffsüberprüfung](complete-access-review.md) aus, um die Ergebnisse anzuzeigen und anzuwenden.  

Um eine Serie von Zugriffsüberprüfungen zu verwalten, navigieren Sie zur Zugriffsüberprüfung. Dort finden Sie unter den geplanten Überprüfungen die anstehenden Überprüfungen, und Sie können das Enddatum bearbeiten oder Prüfer entsprechend hinzufügen/entfernen.

Basierend auf Ihrer Auswahl unter **Einstellungen nach Abschluss** wird nach dem Enddatum der Überprüfung oder bei manueller Beendigung der Überprüfung die automatische Anwendung ausgeführt. Der Status der Überprüfung ändert sich von **Abgeschlossen** über Zwischenzustände wie **Wird angewandt** schließlich in den Status **Angewandt**. Erwartungsgemäß sollten abgelehnte Benutzer (sofern vorhanden) innerhalb weniger Minuten aus der Gruppenmitgliedschaft oder Anwendungszuweisung entfernt werden.

## <a name="create-reviews-via-apis"></a>Erstellen von Überprüfungen über APIs

Zugriffsüberprüfungen können auch unter Verwendung von APIs erstellt werden. Die Aktionen, die Sie zur Verwaltung von Zugriffsüberprüfungen für Gruppen und Anwendungsbenutzer im Azure-Portal ausführen, können auch über Microsoft Graph-APIs ausgeführt werden. Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Ein Codebeispiel finden Sie unter [Beispiel für das Abrufen von Azure AD-Zugriffsüberprüfungen über Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen des Zugriffs auf Gruppen oder Anwendungen](perform-access-review.md)
- [Überprüfen des eigenen Zugriffs auf Gruppen oder Anwendungen](review-your-access.md)
- [Abschließen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](complete-access-review.md)

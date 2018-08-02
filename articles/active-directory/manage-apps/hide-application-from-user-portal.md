---
title: Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory | Microsoft-Dokumentation
description: Ausblenden einer Anwendung auf der Benutzeroberfläche in Zugriffsbereichen von Azure Active Directory oder Startfeldern von Office 365
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 55f80396df4cbfe7d0a16a6a5066b68aadc0bdd3
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39368918"
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory

Wenn Sie eine Anwendung besitzen, die nicht im Zugriffsbereich des Benutzers oder in Office 365-Startfeldern angezeigt werden soll, stehen Ihnen Optionen zur Verfügung, um diese App-Kachel auszublenden.  Die folgenden zwei Optionen stehen zum Ausblenden von Anwendungen in App-Startfeldern von Benutzern zur Verfügung.

- Ausblenden einer Drittanbieteranwendung in Zugriffsbereichen von Benutzern und Office 365-App-Startfeldern
- Ausblenden aller Office 365-Anwendungen in Zugriffsbereichen von Benutzern

Benutzer sind weiterhin berechtigt, diese App zu nutzen, wenn Sie diese ausblenden, sie wird jedoch nicht mehr in deren App-Startfeldern angezeigt. Sie benötigen die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App, und Sie müssen ein globaler Administrator für das Verzeichnis sein.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Ausblenden einer Anwendung aus der Benutzeroberfläche von Endbenutzern
Je nach Ihrer Situation können Sie die Schritte unten verwenden, um Anwendungen in Zugriffsbereichen auszublenden.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Wie blende ich eine Drittanbieter-App aus dem Zugriffsbereich des Benutzers und den App-Startfeldern von Office 365 aus?
Befolgen Sie folgende Schritte, um eine Anwendung im Zugriffsbereich eines Benutzers und in den App-Startfeldern von Office 365 auszublenden.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2.  Wählen Sie **Alle Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3.  Klicken Sie auf dem Bildschirm **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Bildschirm für das Verzeichnis, das Sie verwalten) auf **Unternehmensanwendungen**.
![Unternehmens-Apps](./media/hide-application-from-user-portal/app1.png)
4.  Wählen Sie **Alle Anwendungen** auf dem Bildschirm **Unternehmensanwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5.  Wählen Sie auf dem Bildschirm **Unternehmensanwendungen – Alle Anwendungen** eine App aus.</br>
![Unternehmens-Apps](./media/hide-application-from-user-portal/app2.png)
6.  Wählen Sie „Eigenschaften“ auf dem Bildschirm ***App-Name*** (dem Bildschirm mit dem Namen der ausgewählten App im Titel) aus.
7.  Wählen Sie auf dem Bildschirm ***App-Name* – Eigenschaften** **Ja** für **Für Benutzer sichtbar?** aus.
![Unternehmens-Apps](./media/hide-application-from-user-portal/app3.png)
8.  Klicken Sie auf **Speichern** .

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Gewusst wie: Ausblenden von Office 365-Anwendungen in Zugriffsbereichen von Benutzern

Mithilfe der folgenden Schritte können Sie alle Office 365-Anwendungen im Zugriffsbereich ausblenden. Diese Apps werden im Office 365-Portal weiterhin angezeigt.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2.  Wählen Sie **Alle Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3.  Wählen Sie auf dem Bildschirm **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Bildschirm für das Verzeichnis, das Sie verwalten) **Benutzereinstellungen** aus.
4.  Wählen Sie auf dem Bildschirm **Benutzereinstellungen** unter **Unternehmensanwendungen** für **Benutzer können Office 365-Apps nur im Office 365-Portal anzeigen** den Wert **Ja** aus.

![Unternehmens-Apps](./media/hide-application-from-user-portal/apps4.png)

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](assign-user-or-group-access-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](remove-user-or-group-access-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](change-name-or-logo-portal.md)


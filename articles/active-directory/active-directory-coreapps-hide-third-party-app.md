---
title: "Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory | Microsoft-Dokumentation"
description: "Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 28b6885e7e74a44f4d00f4a804a93e74f6eec685
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="hide-a-third-party-application-from-users-experience-in-azure-active-directory"></a>Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory

Wenn Sie eine Drittanbieteranwendung (eine App, die nicht von Microsoft veröffentlicht wurde) besitzen, die nicht im Zugriffsbereich des Benutzers oder im Office 365-Startfeldern angezeigt werden soll, steht Ihnen eine Option zur Verfügung, um diese App-Kachel auszublenden. Benutzer sind weiterhin berechtigt, diese App zu nutzen, wenn Sie diese ausblenden, sie wird jedoch nicht mehr in deren App-Startfeldern angezeigt. Sie benötigen die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App, und Sie müssen globaler Administrator für das Verzeichnis sein.

## <a name="hiding-a-users-access-panel"></a>Ausblenden des Zugriffsbereichs eines Benutzers
Befolgen Sie folgende Schritte, um den Zugriffsbereich eines Benutzers und die App-Startfelder von Office 365 auszublenden.

### <a name="how-do-i-hide-an-app-from-users-access-panel-and-o365-app-launchers"></a>Wie blende ich eine App aus dem Zugriffsbereich des Benutzers und den App-Startfeldern von Office 365 aus?

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2.  Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3.  Klicken Sie auf dem Bildschirm **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Bildschirm für das Verzeichnis, das Sie verwalten) auf **Unternehmensanwendungen**.
![Unternehmens-Apps](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Wählen Sie **Alle Anwendungen** auf dem Bildschirm **Unternehmensanwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5.  Wählen Sie auf dem Bildschirm **Unternehmensanwendungen – Alle Anwendungen** eine App aus.</br>
![Unternehmens-Apps](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Wählen Sie „Eigenschaften“ auf dem Bildschirm ***App-Name*** (dem Bildschirm mit dem Namen der ausgewählten App im Titel) aus.
7.  Wählen Sie auf dem Bildschirm ***App-Name* – Eigenschaften** **Ja** für **Für Benutzer sichtbar?** aus.
![Unternehmens-Apps](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-user-azure-portal.md)

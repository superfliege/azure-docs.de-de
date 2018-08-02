---
title: Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory | Microsoft-Dokumentation
description: Deaktivieren einer Unternehmens-App in Azure Active Directory, damit sich Benutzer nicht mehr bei der App anmelden können
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
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 39e926a392cbd87eff23e25d9708792ec7c40a2c
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39368708"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie auf einfache Weise eine Unternehmens-App deaktivieren, damit sich Benutzer nicht mehr bei der App anmelden können. Sie benötigen die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App, und Sie müssen globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-disable-user-sign-ins"></a>Wie deaktiviere ich Benutzeranmeldungen?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Alle Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie im Bereich **Azure Active Directory** -  ***Verzeichnisname*** (also dem Azure AD-Bereich für das Verzeichnis, das Sie verwalten) den Eintrag **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. Wählen Sie im Bereich **Unternehmensanwendungen** die Option **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie im Bereich **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie im Bereich Blatt ***App-Name*** (dem Bereich mit dem Namen der ausgewählten App im Titel) die Option **Eigenschaften** aus.

    ![Auswählen des Befehls „Alle Anwendungen“](./media/disable-user-sign-in-portal/select-app.png)
7. Wählen Sie im Bereich ***App-Name*** - **Eigenschaften** für die Option **Aktiviert für die Benutzeranmeldung?** die Einstellung **Nein** aus.
8. Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](assign-user-or-group-access-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](remove-user-or-group-access-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](change-name-or-logo-portal.md)

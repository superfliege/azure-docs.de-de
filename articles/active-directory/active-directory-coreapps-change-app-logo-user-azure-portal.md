---
title: "Ändern des Namens oder Logos einer Unternehmens-App in Azure Active Directory | Microsoft-Dokumentation"
description: "Ändern des Namens oder des Logos einer Unternehmens-App in der Azure Active Directory-Vorschau"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: fe03d2a8dcb39cb06679386959ac17354a543089
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Ändern des Namens oder Logos einer Unternehmens-App in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie auf einfache Weise den Namen oder das Logo für eine benutzerdefinierte Unternehmensanwendung ändern. Sie benötigen die erforderlichen Berechtigungen, um diese Änderungen vorzunehmen, und Sie müssen der Ersteller der benutzerdefinierten App sein.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Wie ändere ich den Namen oder das Logo einer Unternehmens-App?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Alle Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie im Bereich **Azure Active Directory –  *Verzeichnisname*** (also dem Azure AD-Bereich für das Verzeichnis, das Sie verwalten) die Option **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. Wählen Sie im Bereich **Unternehmensanwendungen** die Option **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie im Bereich **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie im Bereich Blatt ***App-Name*** (dem Bereich mit dem Namen der ausgewählten App im Titel) die Option **Eigenschaften** aus.

    ![Auswählen des Befehls „Eigenschaften“](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. Suchen Sie im Bereich ***App-Name*** – **Eigenschaften** nach einer Datei, die Sie als neues Logo verwenden möchten, oder bearbeiten Sie den App-Namen (oder beides).

    ![Befehl zum Ändern des App-Logos oder des Namens](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Klicken Sie auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](active-directory-coreapps-disable-app-azure-portal.md)

---
title: Zusammenarbeit mit anderen Mitwirkenden an LUIS-Apps in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit anderen Mitwirkenden an LUIS-Anwendungen (Language Understanding) zusammenarbeiten können.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/31/2018
ms.author: diberry
ms.openlocfilehash: 99f37cb6dc5e05fc5eb4bde09685435ee57fecc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397787"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Gewusst wie: Verwalten von Erstellern und Mitwirkenden 

Sie können mit anderen zusammen an LUIS-Apps arbeiten. 

## <a name="owner-and-collaborators"></a>Besitzer und Mitarbeiter

Eine App hat einen einzelnen Autor, den Besitzer, sie kann aber viele Mitarbeiter aufweisen. 

## <a name="add-collaborator"></a>Mitarbeiter hinzufügen

Um Mitarbeitern das Bearbeiten Ihrer LUIS-App zu erlauben, geben Sie auf der Seite **Einstellungen** Ihrer LUIS-App die E-Mail-Adresse des Mitarbeiters ein, und klicken Sie auf **Add collaborator** (Mitarbeiter hinzufügen). Mitarbeiter können sich bei der LUIS-App anmelden und sie bearbeiten, während Sie an der App arbeiten.

![Mitarbeiter hinzufügen](./media/luis-how-to-collaborate/add-collaborator.png)

## <a name="transfer-of-ownership"></a>Übertragen des Besitzes

Zwar unterstützt LUIS derzeit die Übertragung des Besitzes nicht, jedoch können Sie Ihre App exportieren, und ein anderer LUIS-Benutzer kann sie importieren. Zwischen den beiden Anwendungen kann es zu kleinen Unterschieden bei den LUIS-Bewertungen kommen. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-Mandantenbenutzer

LUIS verwendet den Standard-Zustimmungsfluss von Azure Active Directory (Azure AD). 

Der Administrator des Mandanten sollte direkt mit dem Benutzer zusammenarbeiten, der den Zugriff auf LUIS in der Azure AD benötigt. 

Im ersten Schritt meldet sich der Benutzer bei LUIS an und sieht das Popupdialogfeld, aus dem er erfährt, dass er eine Administratorgenehmigung benötigt. Der Benutzer setzt sich mit dem Administrator des Mandanten in Verbindung, bevor er fortfährt. 

Im zweiten Schritt meldet sich der Mandantenadministrator bei LUIS und sieht ein Popupdialogfeld zum Zustimmungsfluss. Dies ist das Dialogfeld, das der Administrator benötigt, um dem Benutzer die Genehmigung zu erteilen. Nachdem der Administrator die Berechtigung akzeptiert hat, kann der Benutzer mit LUIS fortfahren.

Wenn der Mandantenadministrator sich nicht bei LUIS anmelden möchte, kann er auf die [Zustimmung](https://account.activedirectory.windowsazure.com/r#/applications) für LUIS zugreifen. 

![Azure Active Directory-Website mit Berechtigungen nach Anwendung](./media/luis-how-to-account-settings/tenant-permissions.png)

Wenn der Mandantenadministrator möchte, dass nur bestimmte Benutzer LUIS verwenden, stehen ihm in diesem [Identitätsblog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/) entsprechende Informationen zur Verfügung.

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Benutzerkonten mit mehreren E-Mail-Adressen für Projektmitarbeiter

Wenn Sie einer LUIS-App Projektmitarbeiter hinzufügen, geben Sie genau die E-Mail-Adressen an, die ein Projektmitarbeiter benötigt, um LUIS als Projektmitarbeiter zu verwenden. Azure Active Directory (Azure AD) erlaubt einem Einzelbenutzer zwar den Besitz mehrerer abwechselnd verwendeter E-Mail-Konten, für LUIS ist jedoch die Anmeldung des Benutzers mit der in der Liste der Projektmitarbeiter angegebenen E-Mail-Adresse erforderlich.
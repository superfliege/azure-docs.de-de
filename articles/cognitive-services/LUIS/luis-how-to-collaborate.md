---
title: Zusammenarbeit mit anderen Mitwirkenden an LUIS-Apps
titleSuffix: Azure Cognitive Services
description: Ein App-Besitzer kann einer App Projektmitarbeiter hinzufügen. Diese Projektmitarbeiter können das Modell ändern und die App trainieren und veröffentlichen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: b7d108bfc0c4283e7856b93daba3f4f92af4cc5b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042192"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Gewusst wie: Verwalten von Erstellern und Mitwirkenden 

Ein App-Besitzer kann einer App Projektmitarbeiter hinzufügen. Diese Projektmitarbeiter können das Modell ändern und die App trainieren und veröffentlichen. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Mitarbeiter hinzufügen

Eine App hat einen einzelnen Autor, den Besitzer, sie kann aber viele Mitarbeiter aufweisen. Um Projektmitarbeitern das Bearbeiten Ihrer LUIS-App zu ermöglichen, müssen Sie ihre E-Mail-Adresse, die sie für den Zugriff auf das LUIS-Portal verwenden, zur Liste der Mitwirkenden hinzufügen. Nach dem Hinzufügen wird die App in ihrem LUIS-Portal angezeigt.

1. Wählen Sie im Menü rechts oben **Verwalten** und dann im linken Menü **Projektmitarbeiter** aus.

2. Wählen Sie auf der Symbolleiste **Projektmitarbeiter hinzufügen** aus.

    [![](./media/luis-how-to-collaborate/add-collaborator.png "Projektmitarbeiter hinzufügen")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Geben Sie die E-Mail-Adresse ein, die der Projektmitarbeiter für die Anmeldung beim LUIS-Portal verwendet.

    ![Hinzufügen der E-Mail-Adresse des Projektmitarbeiters](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Übertragen des Besitzes

Zwar unterstützt LUIS derzeit die Übertragung des Besitzes nicht, jedoch können Sie Ihre App exportieren, und ein anderer LUIS-Benutzer kann sie importieren. Zwischen den beiden Anwendungen kann es zu kleinen Unterschieden bei den LUIS-Bewertungen kommen. 

## <a name="azure-active-directory-resources"></a>Azure Active Directory-Ressourcen

Wenn Sie in Ihrer Organisation Azure Active Directory (Azure AD) verwenden, benötigt LUIS eine Berechtigung zum Zugriff auf Informationen über Ihre Benutzer, wenn diese LUIS verwenden möchten. Die von LUIS benötigten Ressourcen sind minimal. 

Die detaillierte Beschreibung wird angezeigt, wenn Sie sich mit einem Konto zu registrieren versuchen, für das eine Administratoreinwilligung vorliegt oder keine Administratoreinwilligung erforderlich ist:

* Ermöglicht Ihnen die Anmeldung bei der App mit Ihrem Organisationskonto und lässt die App Ihr Profil lesen. Es erlaubt der App darüber hinaus, grundlegende Firmeninformationen zu lesen.
* Es ermöglicht der App, Ihre Daten zu sehen und zu aktualisieren, auch wenn Sie die App aktuell nicht verwenden.

Die erste Berechtigung gibt LUIS die Berechtigung, grundlegende Profildaten wie die Benutzer-ID, die E-Mail-Adresse und den Namen zu lesen. Die zweite Berechtigung ist erforderlich, um das Zugriffstoken des Benutzers zu aktualisieren.

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-Mandantenbenutzer

LUIS verwendet den Standard-Zustimmungsfluss von Azure Active Directory (Azure AD). 

Der Administrator des Mandanten sollte direkt mit dem Benutzer zusammenarbeiten, der den Zugriff auf LUIS in der Azure AD benötigt. 

Im ersten Schritt meldet sich der Benutzer bei LUIS an und sieht das Popupdialogfeld, aus dem er erfährt, dass er eine Administratorgenehmigung benötigt. Der Benutzer setzt sich mit dem Administrator des Mandanten in Verbindung, bevor er fortfährt. 

Im zweiten Schritt meldet sich der Mandantenadministrator bei LUIS und sieht ein Popupdialogfeld zum Zustimmungsfluss. Dies ist das Dialogfeld, das der Administrator benötigt, um dem Benutzer die Genehmigung zu erteilen. Nachdem der Administrator die Berechtigung akzeptiert hat, kann der Benutzer mit LUIS fortfahren.

Wenn der Mandantenadministrator sich nicht bei LUIS anmelden möchte, kann er auf die [Zustimmung](https://account.activedirectory.windowsazure.com/r#/applications) für LUIS zugreifen. 

![Azure Active Directory-Website mit Berechtigungen nach Anwendung](./media/luis-how-to-collaborate/tenant-permissions.png)

Wenn der Mandantenadministrator möchte, dass nur bestimmte Benutzer LUIS verwenden, stehen ihm in diesem [Identitätsblog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/) entsprechende Informationen zur Verfügung.

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Benutzerkonten mit mehreren E-Mail-Adressen für Projektmitarbeiter

Wenn Sie einer LUIS-App Projektmitarbeiter hinzufügen, geben Sie genau die E-Mail-Adressen an, die ein Projektmitarbeiter benötigt, um LUIS als Projektmitarbeiter zu verwenden. Azure Active Directory (Azure AD) erlaubt einem Einzelbenutzer zwar den Besitz mehrerer abwechselnd verwendeter E-Mail-Konten, für LUIS ist jedoch die Anmeldung des Benutzers mit der in der Liste der Projektmitarbeiter angegebenen E-Mail-Adresse erforderlich.


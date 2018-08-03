---
title: Verwalten Ihrer Kontoeinstellungen in LUIS | Microsoft Docs
description: Verwenden Sie die LUIS-Website, um Ihre Kontoeinstellungen zu verwalten.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 963a7f8c196702ea899ddfe31e6187a15eb5f683
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223207"
---
# <a name="manage-account-and-authoring-key"></a>Verwalten von Konto und Authoringschlüssel
Die zwei entscheidenden Informationen für ein LUIS-Konto sind das Benutzerkonto und der Authoringschlüssel. Ihre Anmeldeinformationen werden unter [account.microsoft.com](https://account.microsoft.com) verwaltet. Ihr Authoringschlüssel wird auf der Seite **Einstellungen** der [LUIS](luis-reference-regions.md)-Website verwaltet. 

## <a name="authoring-key"></a>Erstellungsschlüssel

Dieser eindeutige, regionsspezifische Authoringschlüssel auf der Seite **Einstellungen** ermöglicht Ihnen das Authoring Ihrer sämtlichen Apps von der [LUIS](luis-reference-regions.md)-Website sowie der [Authoring-APIs](https://aka.ms/luis-authoring-api). Zu Ihrer Bequemlichkeit ist es dem Authoringschlüssel gestattet, eine [beschränkte](luis-boundaries.md) Anzahl Endpunktabfragen im Monat auszuführen. 

![LUIS-Seite „Einstellungen“](./media/luis-how-to-account-settings/account-settings.png)

Der Authoringschlüssel wird für alle Apps verwendet, die Sie besitzen, sowie für alle Apps, für die Sie als Mitwirkender gelistet sind.

## <a name="authoring-key-regions"></a>Regionen von Authoringschlüsseln
Der Authoringschlüssel ist für die [Authoringregion](luis-reference-regions.md#publishing-regions) spezifisch. Der Schlüssel funktioniert in anderen Regionen nicht. 

## <a name="reset-authoring-key"></a>Zurücksetzen des Authoringschlüssels
Wenn Ihr Authoringschlüssel beschädigt wurde, setzen Sie den Schlüssel zurück. Der Schlüssel wird für alle Ihre Apps auf der [LUIS](luis-reference-regions.md)-Website zurückgesetzt. Wenn Sie Ihre Apps mithilfe der Authoring-APIs verfassen, müssen Sie den Wert von `Ocp-Apim-Subscription-Key` auf den neuen Schlüssel ändern. 

## <a name="delete-account"></a>Konto löschen
Informationen zu den Daten, die beim Löschen Ihres Kontos gelöscht werden, finden Sie unter [Datenspeicherung und -löschung](luis-concept-data-storage.md#accounts). 

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


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Ihren [Authoringschlüssel](luis-concept-keys.md#authoring-key). 


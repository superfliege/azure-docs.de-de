---
title: Authentifizierung mit Azure Maps | Microsoft-Dokumentation
description: Enthält Informationen zur Authentifizierung für die Verwendung von Azure Maps-Diensten.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b97c303433eb8fadcda51257d37447f052ce4a3b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118814"
---
# <a name="authentication-with-azure-maps"></a>Authentifizierung mit Azure Maps

Azure Maps unterstützt zwei Möglichkeiten für die Authentifizierung von Anforderungen. Bei der Nutzung eines gemeinsam verwendeten Schlüssels oder von Azure Active Directory (Azure AD) gibt es jeweils eine Methode, mit der jede an Azure Maps gesendete Anforderung autorisiert werden kann. In diesem Artikel werden beide Authentifizierungsmethoden beschrieben, um Sie bei der Implementierung der Authentifizierung zu unterstützen.

## <a name="shared-key-authentication"></a>Authentifizierung mit gemeinsam verwendetem Schlüssel

Bei der Authentifizierung mit einem gemeinsam verwendeten Schlüssel werden bei jeder Anforderung an Azure Maps Schlüssel übergeben, die über das Azure Maps-Konto generiert werden.  Bei der Erstellung Ihres Azure Maps-Kontos werden zwei Schlüssel generiert.  Für jede Anforderung, die an Azure Maps-Dienste gesendet wird, muss der Abonnementschlüssel der URL als Parameter hinzugefügt werden.

> [!Tip]
> Wir empfehlen Ihnen, Ihre Schlüssel regelmäßig neu zu generieren. Es werden zwei Schlüssel bereitgestellt, damit Sie die Verbindungen mit einem Schlüssel aufrechterhalten können, während Sie den anderen neu generieren. Bei der Neugenerierung Ihrer Schlüssel müssen Sie alle Anwendungen, die auf dieses Konto zugreifen, so aktualisieren, dass die neuen Schlüssel verwendet werden.

Informationen zum Anzeigen Ihrer Schlüssel finden Sie unter [View authentication details](https://aka.ms/amauthdetails) (Anzeigen von Authentifizierungsdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Authentifizierung mit Azure Active Directory (Vorschauversion)

Azure Maps verfügt jetzt über eine Integration von [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis), mit der Anforderungen für Azure Maps-Dienste authentifiziert werden können.  Azure AD ermöglicht eine identitätsbasierte Authentifizierung mit [rollenbasierter Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/overview) (Role-Based Access Control, RBAC), um den Zugriff auf Azure Maps-Ressourcen auf der Benutzer- oder Anwendungsebene zu gewähren. In diesem Artikel sollen die Konzepte und Komponenten der Azure AD-Integration von Azure Maps vermittelt werden.

## <a name="authentication-with-oauth-access-tokens"></a>Authentifizierung mit OAuth-Zugriffstoken

Azure Maps akzeptiert **OAuth 2.0**-Zugriffstoken für Azure AD-Mandanten, die dem Azure-Abonnement zugeordnet sind, in dem ein Azure Maps-Konto enthalten ist.  Azure Maps akzeptiert Token für:

* Azure AD-Benutzer 
* Drittanbieteranwendungen, für die von Benutzern delegierte Berechtigungen verwendet werden
* Verwaltete Identitäten für Azure-Ressourcen

Azure Maps generiert für jedes Azure Maps-Konto das Element `unique identifier (client ID)`.  Wenn die Client-ID mit zusätzlichen Parametern kombiniert wird, können Token von Azure AD angefordert werden, indem dieser Wert angegeben wird:

```
https://login.microsoftonline.com
```
Weitere Informationen zum Konfigurieren von Azure AD und Anforderungstoken für Azure Maps finden Sie im Artikel zum Thema [Verwalten der Authentifizierung](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Allgemeine Informationen zur Anforderung von Token von Azure AD finden Sie unter [Was ist Authentifizierung?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="requesting-azure-map-resources-with-oauth-tokens"></a>Anfordern von Azure Map-Ressourcen mit OAuth-Token

Nachdem ein Token von Azure AD abgerufen wurde, kann eine Anforderung an Azure Maps gesendet werden, für die die beiden folgenden Anforderungsheader festgelegt sind:

| Anforderungsheader    |    Wert    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorisierung     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` ist die auf dem Azure Maps-Konto basierende GUID, die auf der Azure Maps-Authentifizierungsseite angezeigt wird.

Hier ist ein Beispiel für eine Azure Maps-Routenanforderung mit einem OAuth-Token angegeben:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Informationen zum Anzeigen Ihrer Client-ID finden Sie unter [View authentication details](https://aka.ms/amauthdetails) (Anzeigen von Authentifizierungsdetails).

## <a name="control-access-with-role-based-access-control-rbac"></a>Steuern des Zugriffs mit rollenbasierter Zugriffssteuerung

Ein wichtiges Feature von Azure AD ist das Steuern des Zugriffs auf geschützte Ressourcen per rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC). Nachdem Ihr Azure Maps-Konto erstellt wurde und Sie Ihre Azure Maps-Azure AD-Anwendung unter Ihrem Azure AD-Mandanten registriert haben, können Sie RBAC für einen Benutzer, eine Anwendung oder eine Azure-Ressource auf der Portalseite des Azure Maps-Kontos konfigurieren. 

Azure Maps unterstützt derzeit die Steuerung des Lesezugriffs für einzelne Azure AD-Benutzer, Anwendungen oder Azure-Dienste über verwaltete Identitäten für Azure-Ressourcen.

![Konzept](./media/azure-maps-authentication/concept.png)

Informationen zum Anzeigen Ihrer RBAC-Einstellungen finden Sie unter [View Azure Maps role-based access control (RBAC)](https://aka.ms/amrbac) (Anzeigen der rollenbasierten Zugriffssteuerung (RBAC) für Azure Maps).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Verwaltete Identitäten für Azure-Ressourcen und Azure Maps

[Verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) stellen Azure-Dienste (Azure App Service, Azure Functions, Virtual Machines usw.) mit einer automatisch verwalteten Identität bereit, die für den Zugriff auf Azure Maps-Dienste autorisiert werden kann.  

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Authentifizierung einer Anwendung mit Azure AD und Azure Maps finden Sie im Artikel zum Thema [Verwalten der Authentifizierung](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Weitere Informationen zur Authentifizierung von Azure Maps, des Kartensteuerelements und von Azure AD finden Sie unter [Verwenden des Azure Maps-Kartensteuerelements](https://aka.ms/amaadmc).
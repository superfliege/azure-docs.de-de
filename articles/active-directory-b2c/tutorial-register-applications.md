---
title: 'Tutorial: Registrieren einer Anwendung zum Aktivieren der Registrierung und Anmeldung mit Azure Active Directory B2C | Microsoft Docs'
description: Verwenden Sie das Azure-Portal, um einen Azure AD B2C-Mandanten zu erstellen und eine Anwendung dafür zu registrieren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/08/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ebfe4a2d8c6a5b1d5334034e406131fac81f0c19
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713897"
---
# <a name="tutorial-register-an-application-to-enable-sign-up-and-sign-in-using-azure-active-directory-b2c"></a>Tutorial: Registrieren einer Anwendung zum Aktivieren der Registrierung und Anmeldung mit Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie in wenigen Minuten einen Microsoft Azure Active Directory B2C-Mandanten (Azure AD) erstellen und eine Anwendung dafür registrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement
> * Anwendung registrieren

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten

In Ihren vorhandenen Mandanten können B2C-Features nicht aktiviert werden. Sie müssen einen Azure AD B2C-Mandanten erstellen.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Glückwunsch, Sie haben einen Azure Active Directory B2C-Mandanten erstellt. Sie sind ein globaler Administrator des Mandanten. Sie können bei Bedarf weitere globale Administratoren hinzufügen. Um zu Ihrem neuen Mandanten zu wechseln, klicken Sie auf den *Link zur Verwaltung Ihres neuen Mandanten*.

![Link zur Verwaltung Ihres neuen Mandanten](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Wenn Sie einen B2C-Mandanten für eine Produktions-App verwenden möchten, lesen Sie den Artikel zum [Vergleich zwischen Produktionsmandanten und B2C-Vorschaumandanten](active-directory-b2c-reference-tenant-type.md). Beim Löschen eines vorhandenen B2C-Mandanten und erneuten Erstellen mit dem gleichen Domänennamen treten bekannte Probleme auf. Sie müssen einen B2C-Mandanten mit einem anderen Domänennamen erstellen.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Verknüpfen Ihres Mandanten mit Ihrem Abonnement

Sie müssen Ihren Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement verknüpfen, um die gesamte B2C-Funktionalität zu aktivieren und die Nutzungsgebühren zahlen zu können. Weitere Informationen finden Sie in [diesem Artikel](active-directory-b2c-how-to-enable-billing.md). Wenn Sie Ihren Azure AD B2C-Mandanten nicht mit Ihrem Azure-Abonnement verknüpfen, wird ein Teil der Funktionalität blockiert und eine Warnmeldung in den B2C-Einstellungen angezeigt („Mit diesem B2C-Mandanten ist kein Abonnement verknüpft, oder das Abonnement benötigt Ihre Aufmerksamkeit.“). Es ist wichtig, dass Sie diesen Schritt ausführen, bevor Sie Ihre Apps in die Produktion übertragen.


[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Sie können auf das Blatt auch zugreifen, indem Sie am oberen Rand des Portals `Azure AD B2C` in **Ressourcen suchen** eingeben. Wählen Sie in der Ergebnisliste **Azure AD B2C**, um auf das B2C-Einstellungenblatt zuzugreifen.

## <a name="register-your-application"></a>Anwendung registrieren

Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)beschriebenen Schritte.

Anwendungen, die im Azure-Portal erstellt wurden, müssen am selben Speicherort verwaltet werden. Mit PowerShell oder über ein anderes Portal bearbeitete Azure AD B2C-Anwendungen werden nicht mehr unterstützt und können nicht mehr mit Azure AD B2C verwendet werden. Ausführliche Informationen finden Sie im Abschnitt [Fehlerhafte Apps](#faulted-apps). 

In diesem Artikel werden Beispiele verwendet, die Ihnen als Hilfe bei den ersten Schritten mit unseren Beispieldaten dienen. Die nachfolgenden Artikel enthalten weitere Informationen zu diesen Beispielen.

### <a name="navigate-to-b2c-settings"></a>Aufrufen der B2C-Einstellungen

Melden Sie sich im [Azure-Portal](https://portal.azure.com/) als globaler Administrator des B2C-Mandanten an. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

### <a name="choose-next-steps-based-on-your-application-type"></a>Auswählen der nächsten Schritte basierend auf Ihrem Anwendungstyp

* [Registrieren einer Webanwendung](#register-a-web-app)
* [Registrieren einer Web-API](#register-a-web-api)
* [Registrieren einer mobilen oder nativen Anwendung](#register-a-mobile-or-native-app)
 
### <a name="register-a-web-app"></a>Registrieren einer Web-App

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

### <a name="create-a-web-app-client-secret"></a>Erstellen eines Web-App-Clientgeheimnisses

Wenn Ihre Webanwendung eine durch Azure AD B2C geschützte Web-API aufruft, gehen Sie wie folgt vor:
   1. Erstellen Sie ein Anwendungsgeheimnis. Klicken Sie hierzu auf dem Blatt **Schlüssel** auf die Schaltfläche **Schlüssel generieren**. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.
   2. Klicken Sie auf **API-Zugriff** und dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

> [!NOTE]
> **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden müssen.
> 

[Zu **Nächste Schritte** springen](#next-steps)

### <a name="register-a-web-api"></a>Registrieren einer Web-API

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Klicken Sie auf **Veröffentlichte Bereiche**, um bei Bedarf weitere Bereiche hinzuzufügen. Standardmäßig wird der Bereich „user_impersonation“ definiert. Mithilfe des Bereichs „user_impersonation“ können andere Anwendungen im Namen des angemeldeten Benutzers auf diese API zugreifen. Der Bereich „user_impersonation“ kann auch entfernt werden.

[Zu **Nächste Schritte** springen](#next-steps)

### <a name="register-a-mobile-or-native-app"></a>Registrieren einer mobilen oder nativen Anwendung

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Zu **Nächste Schritte** springen](#next-steps)

### <a name="choosing-a-web-app-or-api-reply-url"></a>Auswählen einer Antwort-URL für eine Web-App oder -API

Aktuell sind in Azure AD B2C registrierte Apps auf eine begrenzte Anzahl von Antwort-URL-Werten beschränkt. Die Antwort-URL für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Antwort-URL-Werte müssen dieselbe DNS-Domäne verwenden. Es ist beispielsweise nicht möglich, eine Web-App zu registrieren, die eine der folgenden Antwort-URLs aufweist:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen der vorhandenen Antwort-URL mit dem DNS-Namen der von Ihnen hinzugefügten Antwort-URL. Die Anforderung zum Hinzufügen des DNS-Namens schlägt fehl, wenn eine der folgenden Bedingungen erfüllt ist:

* Der gesamte DNS-Name der neuen Antwort-URL entspricht nicht dem DNS-Namen der vorhandenen Antwort-URL.
* Der gesamte DNS-Name der neuen Antwort-URL ist keine Unterdomäne der vorhandenen Antwort-URL.

Beispiel: Die App besitzt die folgende Antwort-URL:

`https://login.contoso.com`

Eine Hinzufügung ist wie folgt möglich:

`https://login.contoso.com/new`

In diesem Fall stimmt der DNS-Name exakt überein. Alternativ haben Sie folgende Möglichkeit:

`https://new.login.contoso.com`

In diesem Fall verweisen Sie auf eine DNS-Unterdomäne von login.contoso.com. Wenn Sie eine App mit den Antwort-URLs „login-east.contoso.com“ und „login-west.contoso.com“ benötigen, müssen Sie die folgenden Antwort-URLs in der angegebenen Reihenfolge hinzufügen:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Die beiden letztgenannten URLs können hinzugefügt werden, da es sich hierbei um Unterdomänen der ersten Antwort-URL „contoso.com“ handelt.

### <a name="choosing-a-native-app-redirect-uri"></a>Auswählen eines Umleitungs-URI für eine native App

Bei der Auswahl eines Umleitungs-URIs für mobile/systemeigene Anwendungen sind zwei Aspekte zu berücksichtigen:

* **Eindeutigkeit**: Das Schema des Umleitungs-URIS muss für jede Anwendung eindeutig sein. Im Beispiel (com.onmicrosoft.contoso.appname://redirect/path) ist „com.onmicrosoft.contoso.appname“ das Schema. Es wird empfohlen, diesem Muster zu folgen. Wenn zwei Anwendungen dasselbe Schema verwenden, wird den Benutzer das Dialogfeld „App auswählen“ angezeigt. Wenn der Benutzer die falsche Auswahl trifft, schlägt die Anmeldung fehl.
* **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss nach der Domäne mindestens einen Schrägstrich enthalten. (Beispiel: „//contoso/“ funktioniert, aber „//contoso“ schlägt fehl.)

Stellen Sie sicher, dass der Umleitungs-URI keine Sonderzeichen wie Unterstriche enthält.

### <a name="faulted-apps"></a>Fehlerhafte Apps

B2C-Anwendungen sollten NICHT wie folgt bearbeitet werden:

* In anderen Portalen für die Anwendungsverwaltung, z.B. im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/)
* Mithilfe der Graph-API oder PowerShell

Wenn Sie die Azure AD B2C-Anwendung wie oben beschrieben bearbeiten und sie dann noch einmal in den Azure AD B2C-Features im Azure-Portal bearbeiten möchten, wird sie eine fehlerhafte App, und Ihre Anwendung kann nicht mehr mit Azure AD B2C verwendet werden. Sie müssen die Anwendung löschen und neu erstellen.

Um die App zu löschen, rufen Sie das [App-Registrierungsportal](https://apps.dev.microsoft.com/) auf und löschen dort die Anwendung. Damit die Anwendung angezeigt wird, müssen Sie der Besitzer der Anwendung sein (und nicht nur Administrator des Mandanten).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure AD B2C-Mandanten
> * Verknüpfen Ihres Mandanten mit Ihrem Abonnement
> * Anwendung registrieren

> [!div class="nextstepaction"]
> [Aktivieren einer Webanwendung für die Authentifizierung mit Konten](active-directory-b2c-tutorials-web-app.md)
---
title: 'Tutorial: Registrieren Ihrer Anwendungen in Azure Active Directory B2C | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Ihre Anwendungen in Azure Active Directory B2C mithilfe des Azure-Portals registrieren.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/11/2019
ms.author: davidmu
ms.openlocfilehash: 99ad1bbaa732b1207ead9da8da36f345d4978241
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856025"
---
# <a name="tutorial-register-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Registrieren Ihrer Anwendungen in Azure Active Directory B2C

Bevor Ihre [Anwendungen](active-directory-b2c-apps.md) mit Azure Active Directory (Azure AD) B2C interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden. In diesem Tutorial erfahren Sie, wie Sie Anwendungen über das Azure-Portal registrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Registrieren einer Web-API
> * Registrieren einer mobilen oder nativen Anwendung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihren eigenen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) noch nicht erstellt haben, erstellen Sie jetzt einen. Sie können einen vorhandenen Mandanten verwenden.

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.

    ![Wechseln zum Abonnementverzeichnis](./media/tutorial-register-applications/switch-directories.png)

2. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
3. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.

    ![Anwendung hinzufügen](./media/tutorial-register-applications/add-application.png)

4. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapp1*.
5. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
6. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können auch das lokale Lauschen an `https://localhost:44316` einrichten. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und später ändern. Zu Testzwecken können Sie `https://jwt.ms` festlegen. Damit zeigen Sie den Inhalt eines Tokens für die Überprüfung an. Legen Sie für dieses Tutorial `https://jwt.ms` fest. 

    Die Antwort-URL muss mit dem Schema `https` beginnen, und alle Antwort-URL-Werte müssen dieselbe DNS-Domäne verwenden. Wenn die Anwendung z.B. über die Antwort-URL `https://login.contoso.com` verfügt, können Sie die URL `https://login.contoso.com/new` hinzufügen. Sie können auch auf die DNS-Unterdomäne `login.contoso.com` verweisen, z.B. mit `https://new.login.contoso.com`. Wenn Sie eine Anwendung verwenden möchten, für die `login-east.contoso.com` und `login-west.contoso.com` als Antwort-URLs angegeben sind, müssen Sie diese Antwort-URLs in dieser Reihenfolge hinzufügen: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. Die beiden letztgenannten URLs können hinzugefügt werden, da es sich hierbei um Unterdomänen der ersten Antwort-URL `contoso.com` handelt.

7. Klicken Sie auf **Create**.

    ![Festlegen von Anwendungseigenschaften](./media/tutorial-register-applications/application-properties.png)

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung einen Code für ein Token austauscht, müssen Sie ein Anwendungsgeheimnis erstellen.

1. Wählen Sie **Schlüssel**, und klicken Sie dann auf **Schlüssel generieren**.

    ![Generieren von Schlüsseln](./media/tutorial-register-applications/generate-keys.png)

2. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.

    ![Speichern des Schlüssels](./media/tutorial-register-applications/save-key.png)
    
3. Wählen Sie **API-Zugriff**, und klicken Sie dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

    ![Konfigurieren des API-Zugriffs](./media/tutorial-register-applications/api-access.png)

## <a name="register-a-web-api"></a>Registrieren einer Web-API

1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
3. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapi1*.
4. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
5. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.
6. Geben Sie für **App-ID-URI** den für Ihre Web-API verwendeten Bezeichner ein. Der vollständige Bezeichner-URI (einschließlich der Domäne) wird für Sie generiert. Beispiel: `https://contosotenant.onmicrosoft.com/api`.
7. Klicken Sie auf **Create**.
8. Wählen Sie die von Ihnen erstellte Anwendung *webapi1* und dann **Veröffentlichte Bereiche** aus, um bei Bedarf weitere Bereiche hinzuzufügen. In der Standardeinstellung der Bereich `user_impersonation` definiert. Mit dem Bereich `user_impersonation` können andere Anwendungen im Namen des angemeldeten Benutzers auf diese API zugreifen. Bei Bedarf können Sie den Bereich `user_impersonation` entfernen.

    ![Festlegen veröffentlichter Bereiche](./media/tutorial-register-applications/published-scopes.png)


## <a name="register-a-mobile-or-native-application"></a>Registrieren einer mobilen oder nativen Anwendung

1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
2. Geben Sie einen Namen für die Anwendung ein. Beispiel: *nativeapp1*.
3. Wählen Sie für **Web-App/Web-API einschließen** die Option **Nein**.
4. Wählen Sie für **Nativen Client einschließen** die Option **Ja**.
5. Geben Sie für **Umleitungs-URI** einen gültigen URI mit einem benutzerdefinierten Schema ein. Bei der Auswahl eines Umleitungs-URIs sind zwei Aspekte zu berücksichtigen:

    - **Eindeutigkeit:** Das Schema des Umleitungs-URIS muss für jede Anwendung eindeutig sein. Im Beispiel `com.onmicrosoft.contoso.appname://redirect/path` ist `com.onmicrosoft.contoso.appname` das Schema. Dieses Muster sollte befolgt werden. Wenn zwei Anwendungen dasselbe Schema verwenden, erhält der Benutzer die Möglichkeit, eine Anwendung auszuwählen. Wenn der Benutzer die falsche Auswahl trifft, kann die Anmeldung nicht ausgeführt werden.
    - **Vollständigkeit:** Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss mindestens einen Schrägstrich nach der Domäne enthalten. Zum Beispiel funktioniert `//contoso/`, bei `//contoso` tritt ein Fehler auf. Stellen Sie sicher, dass der Umleitungs-URI keine Sonderzeichen wie Unterstriche enthält.

6. Klicken Sie auf **Create**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Registrieren einer Web-API
> * Registrieren einer mobilen oder nativen Anwendung

> [!div class="nextstepaction"]
> [Erstellen von Benutzerflows in Azure Active Directory B2C](tutorial-create-user-flows.md)
---
title: Registrieren einer Anwendung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Anwendung bei Azure Active Directory B2C registrieren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8068c4a8f38cd33a1a0547f5db5079bc75c76ec1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013410"
---
# <a name="register-an-application-in-azure-active-directory-b2c"></a>Registrieren Sie eine Anwendung in Azure Active Directory B2C

Zum Erstellen einer [Anwendung](active-directory-b2c-apps.md), die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure AD B2C-Mandanten registrieren. In diesem Artikel erfahren Sie, wie Sie in wenigen Minuten eine Anwendung in einem Azure Active Directory B2C-Mandanten (Azure AD) registrieren. Wenn Sie fertig sind, ist Ihre Anwendung für die Verwendung im Azure AD-B2C-Mandanten registriert.

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie einen eigenen Mandanten mithilfe der in [Erstellen eines Azure Active Directory B2C-Mandanten](tutorial-create-tenant.md)beschriebenen Schritte.

Wählen Sie die nächsten Schritte basierend auf Ihrem Anwendungstyp:

- [Registrieren einer Webanwendung](#register-a-web-application)
- [Registrieren einer Web-API](#register-a-web-api)
- [Registrieren einer mobilen oder nativen Anwendung](#register-a-mobile-or-native-application)

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
2. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
3. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
4. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *testapp1*.
5. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
6. Geben Sie für **Antwort-URLs** Endpunkte ein, an denen Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.
7. Klicken Sie auf **Create**.

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung eine durch Azure AD B2C geschützte Web-API aufruft, müssen Sie ein Anwendungsgeheimnis erstellen.

1. Wählen Sie **Schlüssel**, und klicken Sie dann auf **Schlüssel generieren**. 
2. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.
3. Wählen Sie **API-Zugriff**, und klicken Sie dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

## <a name="register-a-web-api"></a>Registrieren einer Web-API

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
2. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
3. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
4. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *testapp2*.
5. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
6. Geben Sie für **Antwort-URLs** Endpunkte ein, an denen Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.
7. Geben Sie für **App-ID-URI** den für Ihre Web-API verwendeten Bezeichner ein. Der vollständige Bezeichner-URI (einschließlich der Domäne) wird für Sie generiert. Beispiel: `https://contosotenant.onmicrosoft.com/api`.
8. Klicken Sie auf **Create**.
9. Wählen Sie **Veröffentlichte Bereiche**, um bei Bedarf weitere Bereiche hinzuzufügen. In der Standardeinstellung der Bereich `user_impersonation` definiert. Mit dem Bereich `user_impersonation` können andere Anwendungen im Namen des angemeldeten Benutzers auf diese API zugreifen. Bei Bedarf können Sie den Bereich `user_impersonation` entfernen.

## <a name="register-a-mobile-or-native-application"></a>Registrieren einer mobilen oder nativen Anwendung

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
2. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
3. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
4. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *testapp3*.
5. Wählen Sie für **Web-App/Web-API einschließen** die Option **Nein**.
6. Wählen Sie für **Nativen Client einschließen** die Option **Ja**.
7. Geben Sie für **Umleitungs-URI** eine [Umleitungs-URI mit einem benutzerdefinierten Schema](active-directory-b2c-apps.md) ein. Stellen Sie sicher, dass Sie einen gültigen Umleitungs-URI auswählen und keine Sonderzeichen wie Unterstriche eingeben.
8. Klicken Sie auf **Create**.

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung eine durch Azure AD B2C geschützte Web-API aufruft, müssen Sie ein Anwendungsgeheimnis erstellen.

1. Wählen Sie **Schlüssel**, und klicken Sie dann auf **Schlüssel generieren**. 
2. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.
3. Wählen Sie **API-Zugriff**, und klicken Sie dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über eine bei Azure AD B2C registrierte Anwendung verfügen, können Sie zum Einstieg eines der [Schnellstarttutorials](active-directory-b2c-overview.md) ausführen.

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET-Web-App mit Registrierung, Anmeldung und Kennwortzurücksetzung](active-directory-b2c-devquickstarts-web-dotnet-susi.md)

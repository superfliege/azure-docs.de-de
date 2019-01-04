---
title: 'Konfigurieren der Facebook-Authentifizierung: Azure App Service'
description: Erfahren Sie, wie Sie die Facebook-Authentifizierung für Ihre App Services-Anwendung konfigurieren.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: cc10c9be5bab3b84c8773d8a930473267db353ab
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411309"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema erfahren Sie, wie Sie Azure App Services für die Verwendung von Facebook als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Facebook-Konto mit verifizierter E-Mail-Adresse und einer Mobiltelefonnummer, um den in diesem Thema beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [facebook.com]auf, um ein neues Facebook-Konto zu erstellen.

## <a name="register"></a>Registrieren Ihrer Anwendung für Facebook
1. Melden Sie sich beim [Azure-Portal]an, und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**. Diese verwenden Sie zur Konfiguration der Facebook-App.
2. Navigieren Sie in einem anderen Browserfenster zur Website für [Facebook-Entwickler] , und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.
3. (Optional) Falls Sie sich noch nicht registriert haben, klicken Sie auf **Apps** > **Register as a Developer** (Als Entwickler registrieren). Akzeptieren Sie dann die Richtlinie, und führen Sie die Registrierungsschritte durch.
4. Klicken Sie auf **My Apps**(Meine Apps) > **Add a New App** (Neue App hinzufügen).
5. Geben Sie unter **Display Name** (Anzeigenamen) einen eindeutigen Namen für Ihre App ein. Geben Sie auch Ihre **Contact Email** an, klicken Sie dann auf **Create App ID** (App-ID erstellen), und schließen Sie die Sicherheitsüberprüfung ab. Dadurch gelangen Sie zum Entwickler-Dashboard für Ihre neue Facebook-App.
6. Klicken Sie unter **Facebook Login** (Facebook-Anmeldung) auf **Set up** (Einrichten), und wählen Sie dann unter **Facebook Login** im linken Navigationsbereich **Settings** (Einstellungen) aus.
7. Fügen Sie den **Umleitungs-URI** Ihrer Anwendung zu **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) hinzu, und klicken Sie dann auf **Save Changes** (Änderungen speichern).
   
   > [!NOTE]
   > Der Umleitungs-URI ist die URL Ihrer Anwendung, an die der Pfad */.auth/login/facebook/callback* angefügt wurde. Beispiel: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.
   > 
   > 
8. Klicken Sie im linken Navigationsbereich auf **Settings** > **Basic**. Klicken Sie im Feld **App-Geheimnis** auf **Anzeigen**, geben Sie bei entsprechender Aufforderung Ihr Kennwort ein, und notieren Sie die Werte für **App-ID** und **App-Geheimnis**. Ihre Anwendung wird später in Azure mit diesen Angaben konfiguriert.
   
   > [!IMPORTANT]
   > Der App-Schlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.
   > 
   > 
9. Das zum Registrieren der Anwendung verwendete Facebook-Konto fungiert als Administrator der App. Zu diesem Zeitpunkt können sich nur Administratoren bei der Anwendung anmelden. Um andere Facebook-Konten zu authentifizieren, klicken Sie auf **App Review** (App-Überprüfung), und aktivieren Sie **Make <your-app-name> public** (<Name Ihrer App> öffentlich machen), um den allgemeinen öffentlichen Zugriff mithilfe der Facebook-Authentifizierung zu aktivieren.

## <a name="secrets"></a>Hinzufügen von Facebook-Informationen zu Ihrer Anwendung
1. Navigieren Sie im [Azure-Portal]wieder zu Ihrer Anwendung. Klicken Sie auf **Einstellungen** > **Authentifizierung/Autorisierung,** und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
2. Klicken Sie auf **Facebook**, fügen Sie die App-ID und das App-Geheimnis ein, die Sie zuvor erhalten haben, und aktivieren Sie optional alle Bereiche, die Ihre Anwendung benötigt. Klicken Sie anschließend auf **OK**.
   
    ![][0]
   
    Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
3. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Facebook authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Facebook** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Facebook umgeleitet.
4. Klicken Sie nach dem Konfigurieren der Authentifizierung auf **Speichern**.

Sie können nun Facebook für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"></a>Verwandte Inhalte
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-Entwickler]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-Portal]: https://portal.azure.com/

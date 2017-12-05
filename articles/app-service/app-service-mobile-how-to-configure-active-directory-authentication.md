---
title: "Konfigurieren der Azure Active Directory-Authentifizierung für die App Services-Anwendung"
description: "Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung für die App Services-Anwendung konfigurieren."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 990fab9aeea71b8cf344b9a49a5ed438db6663c0
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-login"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird veranschaulicht, wie Sie Azure App Services zur Verwendung von Azure Active Directory als Authentifizierungsanbieter konfigurieren.

## <a name="express"></a>Konfigurieren von Azure Active Directory mit Express-Einstellungen
1. Navigieren Sie im [Azure-Portal] zu Ihrer App Service-App. Wählen Sie im linken Navigationsbereich **Authentifizierung/Autorisierung**.
2. Wenn **Authentifizierung/Autorisierung** nicht aktiviert ist, wählen Sie **Ein**.
3. Wählen Sie **Azure Active Directory** und dann unter **Verwaltungsmodus** die Option **Express**.
4. Klicken Sie auf **OK**, um die App Service-App in Azure Active Directory zu registrieren. Dadurch wird eine neue App-Registrierung erstellt. Wenn Sie stattdessen eine vorhandene App-Registrierung auswählen möchten, klicken Sie auf **Vorhandene App auswählen**, und suchen Sie nach dem Namen einer zuvor erstellten Registrierung des Mandanten.
   Klicken Sie auf die App-Registrierung, um sie auszuwählen, und klicken Sie auf **OK**. Klicken Sie dann auf der Seite „Azure Active Directory-Einstellungen“ auf **OK**.
   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
5. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Azure Active Directory umgeleitet.
6. Klicken Sie auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="advanced"></a>(Alternative Methode:) Manuelles Konfigurieren von Azure Active Directory mit erweiterten Einstellungen
Sie können Konfigurationseinstellungen auch manuell bereitstellen Dies ist die bevorzugte Lösung, falls der zu verwendende AAD-Mandant sich von dem Mandanten unterscheidet, mit dem die Anmeldung in Azure erfolgt. Um die Konfiguration abzuschließen, müssen Sie zunächst eine Registrierung in Azure Active Directory erstellen, und App Service dann einige Registrierungsdetails liefern.

### <a name="register"> </a>Registrieren Ihrer App Service-App beim Azure Active Directory
1. Melden Sie sich beim [Azure-Portal] an, und navigieren Sie zu Ihrer App Service-App. Kopieren Sie die **URL** Ihrer App. Diese dient zum Konfigurieren Ihrer Azure Active Directory-App-Registrierung.
2. Navigieren Sie zu **Active Directory**, und wählen Sie dann die **App-Registrierungen** aus. Klicken Sie anschließend oben auf **Neue Anwendungsregistrierung**, um eine neue App-Registrierung zu starten. 
3. Geben Sie auf der Seite **Erstellen** einen **Namen** für Ihre App-Registrierung ein, wählen Sie den **Web-App/API**-Typ aus, und fügen Sie im Feld **Anmelde-URL** die Anwendungs-URL (aus Schritt 1) ein. Klicken Sie dann auf **Erstellen**.
4. Innerhalb weniger Sekunden sollte die von Ihnen erstellte neue App-Registrierung angezeigt werden.
5. Nachdem die App hinzugefügt wurde, klicken Sie auf den Namen der App-Registrierung, anschließend oben auf **Einstellungen** und dann auf **Eigenschaften**. 
6. Fügen Sie die Anwendungs-URL (aus Schritt 1) in das Feld **App-ID-URI** und in **URL der Startseite** ein, und klicken Sie dann auf **Speichern**.
7. Klicken Sie nun auf die **Antwort-URLs**, bearbeiten Sie die **Antwort-URL**, fügen Sie die Anwendungs-URL (aus Schritt 1) ein, ändern Sie das Protokoll, damit das Protokoll **https://** (nicht „http://“) verwendet wird, und fügen Sie dann */.auth/login/aad/callback* an das Ende der URL an (z.B. `https://contoso.azurewebsites.net/.auth/login/aad/callback`). Klicken Sie auf **Speichern**.   
8.  Kopieren Sie nun die **Anwendungs-ID** für die App. Bewahren Sie sie für die spätere Verwendung auf. Sie benötigen sie zum Konfigurieren Ihrer App Service-App.
9. Schließen Sie die Seite **Registrierte App**. Klicken Sie auf der Seite **App-Registrierungen** oben auf die Schaltfläche **Endpunkte**, und kopieren Sie die **Verbundmetadatendokument**-URL. 
10. Öffnen Sie ein neues Browserfenster, und navigieren Sie zu der URL, indem Sie sie einfügen und zur XML-Seite browsen. Oben im Dokument ist ein **EntityDescriptor**-Element. Dort muss ein **entityID**-Attribut gemäß `https://sts.windows.net/` gefolgt von einer GUID, die speziell für Ihren Mandanten gültig ist (Mandanten-ID), vorhanden sein. Kopieren Sie diesen Wert – er dient als Ihre **Aussteller-URL**. Später konfigurieren Sie Ihre Anwendung für seine Verwendung.

### <a name="secrets"> </a>Hinzufügen von Azure Active Directory-Informationen zu Ihrer App Service-App
1. Kehren Sie zurück zum [Azure-Portal], und navigieren Sie zu Ihrer App Service-App. Klicken Sie auf **Authentifizierung/Autorisierung**. Falls das Authentifizierungs-/Autorisierungsfeature nicht aktiviert ist, aktivieren Sie es über die Option **Ein**. Klicken Sie bei den Authentifizierungsanbietern auf **Azure Active Directory**, um Ihre App zu konfigurieren. (Optional:) Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren. Legen Sie für **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist.** die Option **Mit Azure Active Directory anmelden** fest. Diese Option erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Azure Active Directory umgeleitet.
2. Klicken Sie in der Konfiguration der Active Directory-Authentifizierung unter **Verwaltungsmodus** auf **Erweitert**. Fügen Sie die Anwendungs-ID (aus Schritt 8) in das Feld für die Client-ID und die entityId (aus Schritt 10) als Wert für die Aussteller-URL ein. Klicken Sie dann auf **OK**.
3. Klicken Sie auf der Konfigurationsseite für die Active Directory-Authentifizierung auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="optional-configure-a-native-client-application"></a>(Optional) Konfigurieren Sie eine systemeigene Clientanwendung
Mit Azure Active Directory können Sie auch systemeigene Clients registrieren. So haben Sie noch mehr Kontrolle bei der Zuordnung von Berechtigungen. Dies ist erforderlich, wenn Sie mit einer Bibliothek, etwa der **Active Directory-Authentifizierungsbibliothek**, Anmeldungen durchführen möchten.

1. Navigieren Sie im [Azure-Portal] zu **Azure Active Directory**.
2. Wählen Sie im linken Navigationsbereich **App-Registrierungen**. Klicken Sie oben auf **Neue App-Registrierung**.
4. Geben Sie auf der Seite **Erstellen** einen **Namen** für Ihre App-Registrierung ein. Wählen Sie **Nativ** in **Anwendungstyp**.
5. Geben Sie im Feld **Umleitungs-URI** den */.auth/login/done*-Endpunkt Ihrer Website unter Verwendung des HTTPS-Schemas ein. Dieser Wert sollte so ähnlich lauten wie *https://contoso.azurewebsites.net/.auth/login/done*. Wenn Sie eine Windows-Anwendung erstellen, verwenden Sie stattdessen die [Paket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.
5. Klicken Sie auf **Erstellen**.
6. Sobald die App-Registrierung hinzugefügt wurde, wählen Sie sie aus, um sie zu öffnen. Notieren Sie sich die **Anwendungs-ID**.
7. Klicken Sie auf **Alle Einstellungen** > **Erforderliche Berechtigungen** > **Hinzufügen** > **API-Typ auswählen**.
8. Geben Sie den Namen der zuvor registrierten App Service-App ein, um sie zu suchen, wählen Sie diese aus, und klicken Sie auf **Wählen**. 
9. Wählen Sie **Auf \<App-Name> zugreifen**. Klicken Sie dann auf **Auswählen**. Klicken Sie anschließend auf **Fertig**.

Sie haben nun eine native Clientanwendung konfiguriert, die auf Ihre App Service-App zugreifen kann.

## <a name="related-content"></a>Verwandte Inhalte
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure-Portal]: https://portal.azure.com/
[alternative method]:#advanced

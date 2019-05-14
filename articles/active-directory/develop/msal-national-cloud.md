---
title: Verwenden der Microsoft Authentication Library (MSAL) in nationalen Clouds – Microsoft Identity Platform
description: Die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) ermöglicht Anwendungsentwicklern das Abrufen von Token, um geschützte Web-APIs aufzurufen. Bei diesen Web-APIs kann es sich um Microsoft Graph, andere Microsoft APIs, Web-APIs von Drittanbietern oder Ihre eigene Web-API handeln. Die MSAL unterstützt mehrere Anwendungsarchitekturen und -plattformen.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18eccd6b6d29a43cc3fa13d133d449bf0b9be657
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080222"
---
# <a name="use-msal-in-national-cloud-environment"></a>Verwenden der MSAL in nationalen Cloudumgebungen

[Nationale Clouds](authentication-national-cloud.md) sind physisch isolierte Azure-Instanzen. Diese Regionen von Azure sollen dafür sorgen, dass Anforderungen an Datenresidenz, Datenhoheit und Compliance innerhalb geografischer Grenzen erfüllt werden.

Neben der weltweiten Microsoft-Cloud ermöglicht die Microsoft Authentication Library (MSAL) den Anwendungsentwicklern in nationalen Clouds auch das Abrufen von Token für die Authentifizierung und das Aufrufen gesicherter Web-APIs. Bei diesen Web-APIs kann es sich um Microsoft Graph oder andere Microsoft-APIs handeln.

Azure Active Directory (Azure AD) wird in den folgenden nationalen Clouds bereitgestellt (globale Cloud eingeschlossen):  

- Azure US Government
- Azure China 21Vianet
- Azure Deutschland

Diese Anleitung veranschaulicht die Anmeldung von Geschäfts-, Schul- und Unikonten, das Abrufen eines Zugriffstokens und den Aufruf der Microsoft Graph-API in der [Microsoft Cloud for US Government](https://azure.microsoft.com/global-infrastructure/government/)-Umgebung.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zu Beginn sicher, dass die folgenden Voraussetzungen erfüllt sind.

### <a name="choose-the-appropriate-identities"></a>Auswählen der geeigneten Identitäten

[Azure Government](https://docs.microsoft.com/azure/azure-government/)-Anwendungen können sowohl Azure AD Government-Identitäten als auch öffentliche Azure AD-Identitäten zum Authentifizieren von Benutzern verwenden. Da Sie eine beliebige dieser Identitäten verwenden können, müssen Sie fundiert entscheiden, welchen Autoritätsendpunkt Sie für Ihr Szenario auswählen:

- Öffentliche Azure AD-Identität: Wird häufig verwendet, wenn Ihre Organisation bereits über einen öffentlichen Azure AD-Mandanten verfügt, um Office 365 (öffentlich oder GCC) oder eine andere Anwendung zu unterstützen.
- Azure AD Government-Identität: Wird häufig verwendet, wenn Ihre Organisation bereits über einen Azure AD Government-Mandanten verfügt, um Office 365 (GCC High oder DoD) zu unterstützen, oder wenn ein neuer Mandant in Azure AD Government erstellt werden soll.

Nach dieser Entscheidung müssen Sie sich überlegen, wo die App-Registrierung durchgeführt werden soll. Wenn Sie öffentliche Azure AD-Identitäten für Ihre Azure Government-Anwendung auswählen, müssen Sie die Anwendung in Ihrem öffentlichen Azure AD-Mandanten registrieren.

### <a name="get-an-azure-government-subscription"></a>Erhalten eines Azure Government-Abonnements

- Informationen zum Erhalt eines Azure Government-Abonnement finden Sie unter [Verwalten und Herstellen einer Verbindung mit Ihrem Abonnement in Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).
- Wenn Sie kein Azure Government-Abonnement besitzen, können Sie zu Beginn ein [kostenloses Konto](https://azure.microsoft.com/global-infrastructure/government/request/) erstellen.

## <a name="javascript"></a>JavaScript

### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.us/) an, um eine Anwendung zu registrieren.
    1. Azure-Portalendpunkte für andere nationale Clouds finden Sie unter [App-Registrierungsendpunkte](authentication-national-cloud.md#app-registration-endpoints).

1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Navigieren Sie zur Seite [App-Registrierungen](https://aka.ms/ra/ff) von Microsoft Identity Platform für Entwickler.
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
1. Wählen Sie im Abschnitt **Umleitungs-URI** die Plattform **Web** aus, und setzen Sie den Wert auf die URL der Anwendung basierend auf Ihrem Webserver. Informationen über Anweisungen zum Festlegen und Abrufen der Umleitungs-URL in Visual Studio und Node finden Sie in den folgenden Abschnitten.
1. Wenn Sie so weit sind, klicken Sie auf **Registrieren**.
1. Notieren Sie sich auf der Seite **Übersicht** den Wert von **Anwendungsclient-ID**.
1. Für dieses Tutorial muss der [Flow zur impliziten Genehmigung](v2-oauth2-implicit-grant-flow.md) aktiviert werden. Wählen Sie im linken Navigationsbereich der registrierten Anwendung **Authentifizierung** aus.
1. Aktivieren Sie unter **Erweiterte Einstellungen** und **Implizite Gewährung** die Kontrollkästchen **ID-Token** und **Zugriffstoken**. ID-Token und Zugriffstoken sind erforderlich, da diese App Benutzer anmelden und eine API aufrufen muss.
1. Wählen Sie **Speichern** aus.

### <a name="step-2--set-up-your-web-server-or-project"></a>Schritt 2:  Einrichten Ihres Webservers oder Projekts

- [Laden Sie die Projektdateien für einen lokalen Webserver herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip), z.B. Node.

  oder

- [Laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

Fahren Sie dann mit dem Schritt [Konfigurieren Ihrer JavaScript-SPA](#step-4-configure-your-javascript-spa) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

### <a name="step-3-use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Schritt 3: Verwenden der MSAL (Microsoft Authentication Library) für die Benutzeranmeldung

Befolgen Sie zur Benutzeranmeldung die Schritte im [Javascript-Tutorial](tutorial-v2-javascript-spa.md#create-your-project), um Ihr Projekt zu erstellen und in die Microsoft Authentication Library (MSAL) zu integrieren.

### <a name="step-4-configure-your-javascript-spa"></a>Schritt 4: Konfigurieren Ihrer JavaScript-SPA

Fügen Sie in der Datei `index.html`, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie in den Tags `<script></script>` oben im Text der Datei `index.html` den folgenden Code hinzu:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Hinweis:

- `Enter_the_Application_Id_here` ist die **Anwendungs-ID (Client)** für die von Ihnen registrierte Anwendung.
- `Enter_the_Tenant_Info_Here` wird auf eine der folgenden Optionen festgelegt:
    - Unterstützt Ihre Anwendung **nur Konten in diesem Organisationsverzeichnis**, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (z.B. contoso.microsoft.com).
    - Unterstützt Ihre Anwendung **Konten in einem beliebigen Organisationsverzeichnis**, ersetzen Sie diesen Wert durch `organizations`.
    -  Informationen zum Finden von Authentifizierungsendpunkten für alle nationalen Clouds finden Sie unter [Azure AD-Authentifizierungsendpunkte](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).

     > [!NOTE]
     > Szenarien für persönliche Microsoft-Konten (MSA) werden in nationalen Clouds nicht unterstützt.
  
-   `graphEndpoint`: Der Microsoft Graph-Endpunkt für Microsoft Cloud for US Government.
    -  Informationen zum Finden von Microsoft Graph-Endpunkten für alle nationalen Clouds finden Sie unter [Microsoft Graph-Endpunkte in nationalen Clouds](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints).

## <a name="net"></a>.NET

Über MSAL-.NET können Sie Benutzer anmelden, Token anfordern und die Microsoft Graph-API in nationalen Clouds aufrufen.

Das folgende Tutorial veranschaulicht die Erstellung einer MVC-Web-App in .NET Core 2.2, die OpenID Connect verwendet, um Benutzer über deren Geschäfts-, Schul- oder Unikonto bei ihrer Organisation anzumelden, welche zu den nationalen Clouds gehört.

- Befolgen Sie zum Anmelden von Benutzern und zum Abrufen von Token [dieses Tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Um die Microsoft Graph-API aufzurufen, befolgen Sie [dieses Tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Deutschland](https://docs.microsoft.com/azure/germany/)
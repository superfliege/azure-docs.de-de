---
title: Azure AD Android – Erste Schritte | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie eine Android-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth 2.0 durch Azure AD geschützte APIs aufruft.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: c548f9287ce1326de3322950f297176b67ae61c6
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600243"
---
# <a name="azure-ad-android-getting-started"></a>Azure AD Android – Erste Schritte
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Wenn Sie eine Android-Anwendung entwickeln, sorgt Microsoft für eine einfache und unkomplizierte Anmeldung von Azure Active Directory (Azure AD)-Benutzern. Azure AD ermöglicht es Ihrer Anwendung, über Microsoft Graph oder Ihre eigene geschützte Web-API auf die Benutzerdaten zuzugreifen. 

Die Azure AD-Authentifizierungsbibliothek (ADAL) für Android bietet Ihrer App die Möglichkeit, durch Unterstützung der [Microsoft Azure Active Directory-Konten](https://azure.microsoft.com/services/active-directory/) mithilfe der Industriestandards OAuth2 und OpenID Connect mit der Nutzung der [Microsoft Azure-Cloud](https://cloud.microsoft.com) & [Microsoft Graph-API](https://graph.microsoft.io) zu beginnen. Dieses Beispiel zeigt die üblichen normalen Lebenszyklen für Ihre Anwendung, einschließlich:

* Abrufen eines Tokens für Microsoft Graph
* Aktualisieren eines Tokens
* Aufrufen von Microsoft Graph
* Abmelden des Benutzers

Zu Beginn benötigen Sie einen Azure AD-Mandanten zum Erstellen von Benutzern und zum Registrieren der Anwendung. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Szenario: Anmelden von Benutzern und Aufrufen von Microsoft Graph

![Topologie](./media/quickstart-v1-android/active-directory-android-topology.png)

Diese App kann für alle Azure AD-Konten verwendet werden. Sie unterstützt sowohl Szenarien mit einer als auch mehreren Organisationseinheiten (dies wird Schritt für Schritt erläutert). Sie veranschaulicht, wie ein Entwickler Apps erstellt, die über Microsoft Graph eine Verbindung zu Unternehmensbenutzern herstellen und auf deren Azure- und Office 365-Daten zugreifen können. Während des Authentifizierungsvorgangs muss sich der Endbenutzer anmelden und den Berechtigungen der Anwendung zustimmen. In manchen Fällen ist für die App möglicherweise die Zustimmung eines Administrators erforderlich. Der Großteil der Logik in diesem Beispiel zeigt, wie die Authentifizierung eines Endbenutzers erfolgt und ein Grundaufruf an Microsoft Graph ausgegeben wird.

## <a name="example-code"></a>Beispielcode

Den vollständigen Beispielcode finden Sie [auf GitHub](https://github.com/Azure-Samples/active-directory-android). 

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this, 
        AUTHORITY, 
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(), 
    RESOURCE_ID, 
    CLIENT_ID, 
    REDIRECT_URI,  
    PromptBehavior.Auto, 
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="steps-to-run"></a>Auszuführende Schritte

### <a name="register-and-configure-your-app"></a>Registrieren und Konfigurieren der App 
Sie müssen eine systemeigene Clientanwendung über das [Azure-Portal](https://portal.azure.com) bei Microsoft registrieren lassen. 

1. Aufrufen der App-Registrierung
    - Navigieren Sie zum [Azure-Portal](https://aad.portal.azure.com). 
    - Klicken Sie auf ***Azure Active Directory*** > ***App-Registrierungen***. 

2. Erstellen der App
    - Wählen Sie **Registrierung einer neuen Anwendung** aus. 
    - Geben Sie im Feld **Name** einen Namen für die App ein. 
    - Wählen Sie **Systemeigen** als **Anwendungstyp** aus. 
    - Geben Sie **als**Umleitungs-URI`http://localhost` ein. 

3. Konfigurieren von Microsoft Graph
    - Wählen Sie **Einstellungen > Erforderliche Berechtigungen** aus.
    - Wählen Sie **Hinzufügen** aus, und wählen Sie dann unter **API auswählen** die Option ***Microsoft Graph*** aus. 
    - Wählen Sie die Berechtigung **Anmelden und Benutzerprofil lesen** aus, und klicken Sie dann auf **Auswählen**, um Ihre Auswahl zu speichern. 
        - Durch diese Berechtigung erfolgt eine Zuordnung zum `User.Read`-Bereich. 
    - Optional: Entfernen Sie unter **Erforderliche Berechtigungen > Windows Azure Active Directory** die ausgewählte Berechtigung **Anmelden und Benutzerprofil lesen**. Dadurch wird vermieden, dass die Berechtigung auf der Seite der Benutzerzustimmungen doppelt aufgeführt wird. 

4. Glückwunsch! Ihre App wurde erfolgreich konfiguriert. Im nächsten Abschnitt benötigen Sie:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>Laden Sie den Beispielcode herunter

1. Klonen Sie den Code:
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Öffnen Sie das Beispiel in Android Studio.
    - Wählen Sie die Option zum **Öffnen eines vorhandenen Android Studio-Projekts** aus.

### <a name="configure-your-code"></a>Konfigurieren des Codes

Die gesamte Konfiguration für dieses Codebeispiel finden Sie in der Datei ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java***. 

1. Ersetzen Sie die Konstante `CLIENT_ID` durch `ApplicationID`.
2. Ersetzen Sie die Konstante `REDIRECT URI` durch den zuvor konfigurierten `Redirect URI` (`http://localhost`). 

### <a name="run-the-sample"></a>Ausführen des Beispiels

1. Wählen Sie **Erstellen > Projekt bereinigen** aus. 
2. Wählen Sie **Ausführen > App ausführen** aus. 
3. Die App sollte erstellt werden und einige grundlegende Benutzerfunktionen zeigen. Beim Klicken auf die Schaltfläche `Call Graph API` wird eine Anmeldeaufforderung angezeigt, und anschließend wird im Hintergrund die Microsoft Graph-API mit dem neuen Token aufgerufen. 

## <a name="important-info"></a>Wichtige Informationen

1. Weitere Informationen zu den Bibliotheksmechanismen und zum Konfigurieren neuer Szenarien und Funktionen finden Sie im [ADAL-Android-Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki). 
2. Bei Szenarien mit dem Anwendungstyp „Systemeigen“ verwendet die App eine eingebettete Webansicht und verlässt die App nicht. Der `Redirect URI` kann beliebig sein. 
3. Haben Sie Probleme oder Wünsche? Sie können ein Problem erstellen oder mit dem Tag `azure-active-directory` einen Beitrag in Stackoverflow veröffentlichen. 

### <a name="cross-app-sso"></a>App-übergreifendes SSO
Informationen zum Aktivieren von App-übergreifendem SSO unter Android mit ADAL finden Sie [hier](howto-v1-enable-sso-android.md). 

### <a name="auth-telemetry"></a>Telemetriedaten zur Authentifizierung
Die ADAL-Bibliothek macht Telemetriedaten zur Authentifizierung verfügbar, damit App-Entwickler besser verstehen, wie sich ihre Apps verhalten und sie die Benutzererfahrung optimieren können. So erhalten sie erfolgreiche Anmeldungen und aktive Benutzern und gewinnen eine Reihe anderer interessanter Einblicke. Bei Nutzung der Telemetriedaten zur Authentifizierung brauchen App-Entwickler keinen Telemetriedienst zum Aggregieren und Speichern von Ereignissen einzurichten.

Weitere Informationen zu den bei der Authentifizierung erfassten Telemetriedaten finden Sie unter [ADAL Android-Authentifizierung – Telemetrie](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

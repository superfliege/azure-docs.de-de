---
title: 'Schnellstart: Microsoft Identity Platform – Android | Azure'
description: Hier erfahren Sie, wie Android-Anwendungen eine API aufrufen können, für die Zugriffstoken vom Microsoft Identity Platform-Endpunkt erforderlich sind.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1f174229da565627c0e5791f53031b338880cb3
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495310"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Schnellstart: Benutzeranmeldung und Aufrufen der Microsoft Graph-API aus einer Android-App

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Dieser Schnellstart enthält ein Codebeispiel, das zeigt, wie eine Android-Anwendung persönliche, Geschäfts-, Schul- oder Unikonten anmelden, ein Zugriffstoken abrufen und die Microsoft Graph-API aufrufen kann.

![Zeigt, wie die in diesem Schnellstart generierte Beispiel-App funktioniert](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Voraussetzungen**
> * Android Studio 3+
> * Android 21+ erforderlich 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registrieren und Herunterladen Ihrer Schnellstart-App
> Die Schnellstartanwendung kann auf zwei Arten gestartet werden:
> * [Express] [Option 1: Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manuell] [Option 2: Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Option 1: Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
> So registrieren Sie Ihre App:
> 1. Navigieren Sie zum neuen Bereich [Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs).
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **Registrieren** aus.
> 1. Befolgen Sie die Anweisungen, um Ihre neue Anwendung mit nur einem Klick herunterzuladen und automatisch zu konfigurieren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Option 2: Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
> Führen Sie die folgenden Schritte aus, um Ihre Anwendung zu registrieren und Ihrer Projektmappe manuell die Registrierungsinformationen Ihrer App hinzuzufügen:
>
> 1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
> 1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
> 1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
> 1. Wählen Sie **Neue Registrierung** aus.
> 1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen für Ihre Anwendung ein:
>      - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `Android-Quickstart`).
>      - Klicken Sie auf die Schaltfläche `Register`.
> 1. Navigieren Sie zu `Authentication` > `Redirect URIs` > `Suggested Redirect URIs for public clients`, und wählen Sie den Umleitungs-URI mit dem Format **msal{App-ID}://auth** aus. Speichern Sie die Änderungen.


> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Schritt 1: Konfigurieren der Anwendung
> Damit das Codebeispiel für diesen Schnellstart funktioniert, müssen Sie eine Antwort-URL als **msal{AppId}://auth** hinzufügen. (wobei {AppId} die Anwendungs-ID Ihrer App ist).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-android/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-project"></a>Schritt 2: Herunterladen des Projekts

* [Laden Sie das Android Studio-Projekt herunter.](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Schritt 3: Konfigurieren des Projekts

> [!div renderon="docs"]
> Wenn Sie weiter oben die erste Option ausgewählt haben, können Sie diese Schritte überspringen. Öffnen Sie das Projekt in Android Studio, und führen Sie die App aus. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Extrahieren und öffnen Sie das Projekt in Android Studio.
> 1. Öffnen Sie unter **app** > **res** > **raw** die Datei **auth_config.json**.
> 1. Bearbeiten Sie **auth_config.json**, und ersetzen Sie `client_id` und `tenant_id`:
>    ```javascript
>    "client_id" : "Enter_the_Application_Id_Here",
>    "type": "Enter_the_Audience_Info_Here",
>    "tenant_id" : "Enter_the_Tenant_Info_Here"
>    ```
> 1. Öffnen Sie unter **app** > **manifests** die Datei **AndroidManifest.xml**.
> 1. Fügen Sie dem Knoten **manifest\application** die folgende Aktivität hinzu. Dieser Code ermöglicht es Microsoft, Rückrufe für Ihre App auszuführen:   
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msalEnter_The_Application_Id_Here"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```

> [!div renderon="docs"]
> 1. Extrahieren und öffnen Sie das Projekt in Android Studio.
> 1. Öffnen Sie unter **app** > **res** > **raw** die Datei **auth_config.json**.
> 1. Bearbeiten Sie **auth_config.json**, und ersetzen Sie `client_id` und `redirect_uri`:
>    ```javascript
>    "client_id" : "ENTER_YOUR_APPLICATION_ID",
>    "redirect_uri": "ENTER_YOUR_REDIRECT_URI", 
>     ```
> 1. Öffnen Sie unter **app** > **manifests** die Datei **AndroidManifest.xml**.
> 1. Fügen Sie dem Knoten **manifest\application** die folgende Aktivität hinzu. Dieser Codeausschnitt registriert eine **BrowserTabActivity**, damit das Betriebssystem Ihre Anwendung nach Abschluss der Authentifizierung fortsetzen kann:
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msal<ENTER_YOUR_APPLICATION_ID>"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Ersetzen Sie `<ENTER_THE_APPLICATION_ID_HERE>` durch die *Anwendungs-ID* für Ihre Anwendung. Wenn Sie die *Anwendungs-ID* ermitteln müssen, navigieren Sie zur Seite *Übersicht*.

## <a name="more-information"></a>Weitere Informationen

Lesen Sie die folgenden Abschnitte, um mehr über diesen Schnellstart zu erfahren.

### <a name="msal"></a>MSAL

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) ist die Bibliothek zum Anmelden von Benutzern und Anfordern von Token, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet wird. Sie können Gradle verwenden, um sie zu installieren, indem Sie Folgendes in **Gradle Scripts** > **build.gradle (Modul: app)** unter **Dependencies** hinzufügen:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

### <a name="msal-initialization"></a>MSAL-Initialisierung

Sie können den Verweis auf MSAL hinzufügen, indem Sie den folgenden Code hinzufügen:

```java
import com.microsoft.identity.client.*;
```

Initialisieren Sie MSAL anschließend mit dem folgenden Code:

```java
    sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);
```

> |Hinweis: ||
> |---------|---------|
> |`R.raw.auth_config` | Diese Datei enthält die Konfigurationen für Ihre Anwendung. Dazu zählen unter anderem die App-/Client-ID, die Zielgruppe für die Anmeldung sowie verschiedene andere Anpassungsoptionen. |

### <a name="requesting-tokens"></a>Anfordern von Token

MSAL verwendet zwei Methoden zum Abrufen von Token: `acquireToken` und `acquireTokenSilentAsync`

#### <a name="getting-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

In einigen Situationen müssen Benutzer gezwungen werden, mit dem Microsoft Identity Platform-Endpunkt zu interagieren. Dies führt zu einem Kontextwechsel zum Systembrowser, um entweder die Anmeldeinformationen der Benutzer zu überprüfen oder die Zustimmung zu erhalten. Beispiele hierfür sind:

* Erstmaliges Anmelden von Benutzern bei der Anwendung.
* Benutzer müssen ihre Anmeldeinformationen erneut eingeben, weil das Kennwort abgelaufen ist.
* Ihre Anwendung fordert Zugriff auf eine Ressource an, dem der Benutzer zustimmen muss.
* Zweistufige Authentifizierung ist erforderlich.

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Hinweis:||
> |---------|---------|
> | `SCOPES` | Enthält die angeforderten Bereiche (d.h. `{ "user.read" }` für Microsoft Graph oder `{ "<Application ID URL>/scope" }` für benutzerdefinierte Web-APIs (z.B. `api://<Application ID>/access_as_user`)). |
> | `getAuthInteractiveCallback` | Der Rückruf wird ausgeführt, wenn die Steuerung nach der Authentifizierung an die Anwendung zurückgegeben wird. |

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Sie möchten nicht, dass Benutzer ihre Anmeldeinformationen jedes Mal überprüfen müssen, wenn sie auf eine Ressource zugreifen müssen. Meistens wünschen Sie sich den Abruf und die Erneuerung von Token ohne jegliche Benutzerinteraktion. Sie können die `AcquireTokenSilentAsync`-Methode verwenden, um Token für den Zugriff auf geschützte Ressourcen nach der anfänglichen `acquireToken`-Methode abzurufen:

```java
List<IAccount> accounts = sampleApp.getAccounts();
if (sample.size() == 1) {
    sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
} else {
    // No or multiple accounts
}
```

> |Hinweis:||
> |---------|---------|
> | `SCOPES` | Enthält die angeforderten Bereiche (d.h. `{ "user.read" }` für Microsoft Graph oder `{ "<Application ID URL>/scope" }` für benutzerdefinierte Web-APIs (z.B. `api://<Application ID>/access_as_user`)). |
> | `accounts.get(0)` | Enthält das Konto, für das Sie automatisch Token abrufen möchten. |
> | `getAuthInteractiveCallback` | Der Rückruf wird ausgeführt, wenn die Steuerung nach der Authentifizierung an die Anwendung zurückgegeben wird. |

## <a name="next-steps"></a>Nächste Schritte

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Informieren Sie sich über die Schritte zum Erstellen der in diesem Schnellstart verwendeten Anwendung.

Probieren Sie das Android-Tutorial aus, um eine vollständige Schritt-für-Schritt-Anleitung zum Erstellen von Anwendungen und neuen Features zu erhalten, einschließlich einer vollständigen Erläuterung dieses Schnellstarts.

> [!div class="nextstepaction"]
> [Anmelden von Benutzern und Aufrufen von Microsoft Graph aus einer Android-App](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Wiki: MSAL für Android-Bibliothek

Erfahren Sie mehr über die MSAL-Bibliothek für Android:

> [!div class="nextstepaction"]
> [Wiki: MSAL für Android-Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

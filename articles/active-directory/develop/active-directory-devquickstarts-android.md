---
title: Azure AD Android – Erste Schritte | Microsoft Docs
description: In diesem Thema erfahren Sie, wie eine Android-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth 2.0 durch Azure AD geschützte APIs aufruft.
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 8ea69fbc95b9d9b2452f041e99c46d223581bdac
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD Android – Erste Schritte
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Bei der Entwicklung einer Desktopanwendung ist es für Sie mit Azure Active Directory (Azure AD) einfach und problemlos möglich, Ihre Benutzer über deren lokale Active Directory-Konten zu authentifizieren. Außerdem kann Ihre Anwendung damit auf sichere Weise alle Web-APIs nutzen, die per Azure AD geschützt sind, z. B. die Office 365-APIs oder die Azure-API.

Für Android-Clients, die auf geschützte Ressourcen zugreifen müssen, ist unter Azure AD die Active Directory Authentication Library (ADAL) verfügbar. Die einzige Aufgabe der ADAL besteht darin, Ihrer App das Abrufen von Zugriffstoken zu erleichtern. Um Ihnen zu zeigen, wie einfach es geht, erstellen wir eine Android-Anwendung für Aufgabenlisten, mit der folgende Aktionen ausgeführt werden können:

* Abrufen von Zugriffstoken zum Aufrufen einer Aufgabenlisten-API mit dem [OAuth 2.0-Authentifizierungsprotokoll](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code)
* Abrufen der Aufgabenliste eines Benutzers
* Abmelden von Benutzern

Um zu beginnen, benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und eine Anwendung registrieren können. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Schritt 1: Herunterladen und Ausführen des „Node.js REST API TODO Sample Server“
Das Beispiel „Node.js REST API TODO“ ist speziell für die Arbeit mit dem vorhandenen Beispiel zum Erstellen einer Aufgabenlisten-REST-API für Azure AD für einen einzelnen Mandanten gedacht. Dies ist eine Voraussetzung für den Schnellstart.

Informationen zur Einrichtung finden Sie in unseren vorhandenen Beispielen unter [Azure Active Directory-Beispiel – REST-API-Dienst für „Node.js“](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Schritt 2: Registrieren Ihrer Web-API beim Azure AD-Mandanten
Active Directory unterstützt das Hinzufügen von zwei Arten von Anwendungen:

- Web-APIs, die Dienste für Benutzer bieten
- Anwendungen (die im Web oder auf einem Gerät ausgeführt werden), die auf Web-APIs zugreifen

In diesem Schritt registrieren Sie die Web-API, die Sie lokal zum Testen dieses Beispiels ausführen. Normalerweise handelt es sich bei dieser Web-API um einen REST-Dienst, mit dem die Funktionen bereitgestellt werden, auf die eine App zugreifen soll. Mit Azure AD können Sie einen beliebigen Endpunkt schützen.

Es wird davon ausgegangen, dass Sie die zuvor erwähnte TODO-REST-API registrieren. Dies funktioniert allerdings für alle Web-APIs, die Sie mit Azure Active Directory schützen möchten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnis** den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Bereich auf **Alle Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, z.B. **TodoListService**, wählen Sie **Webanwendung und/oder Web-API** aus, und klicken Sie auf **Weiter**.
6. Geben Sie als Anmelde-URL die Basis-URL für das Beispiel ein. Standardmäßig ist dies `https://localhost:8080`.
7. Klicken Sie auf **OK**, um die Registrierung abzuschließen.
8. Wechseln Sie jetzt im Azure-Portal zur Anwendungsseite, suchen Sie den Wert der Anwendungs-ID, und notieren Sie ihn. Sie benötigen ihn später für die Konfiguration Ihrer Anwendung.
9. Aktualisieren Sie auf der Seite **Einstellungen** -> **Eigenschaften** den App-ID-URI: Geben Sie `https://<your_tenant_name>/TodoListService` ein. Ersetzen Sie `<your_tenant_name>` durch den Namen des Azure AD-Mandanten.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Schritt 3: Registrieren der systemeigenen Android-Beispiel-Clientanwendung
Sie müssen Ihre Webanwendung in diesem Beispiel registrieren. Nur so kann die Anwendung mit der eben registrierten Web-API kommunizieren. Azure AD verweigert Ihrer Anwendung sogar die Anforderung einer Anmeldung, wenn sie nicht registriert ist. Dies ist Teil der Sicherheit dieses Modells.

Es wird davon ausgegangen, dass Sie die zuvor erwähnte Beispielanwendung registrieren. Aber dieses Verfahren funktioniert für jede App, die Sie entwickeln.

> [!NOTE]
> Sie könnten sich fragen, warum Sie eine Anwendung und eine Web-API auf einem Mandanten bereitstellen. Wie Sie vielleicht schon vermuten, können Sie eine App erstellen, von der auf eine externe API zugegriffen wird, die von einem anderen Mandanten aus in Azure AD registriert wurde. Wenn Sie dies tun, werden Ihre Kunden aufgefordert, der Verwendung der API in der Anwendung zuzustimmen. Die Active Directory Authentication Library für iOS führt diese Zustimmung für Sie durch. Wenn wir uns mit den erweiterten Funktionen beschäftigen, werden Sie feststellen, dass dies ein wichtiger Teil der Aufgaben ist, die für den Zugriff auf die Suite der Microsoft-APIs von Azure und Office sowie anderer Dienstanbieter erforderlich sind. Da Sie aber nun sowohl Ihre Web-API als auch Ihre Anwendung unter dem gleichen Mandanten registriert haben, wird Ihnen keine Aufforderung zur Zustimmung angezeigt. Dies ist eher die Regel, wenn Sie eine Anwendung nur für Ihr eigenes Unternehmen entwickeln.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnis** den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Bereich auf **Alle Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, z.B. **TodoListClient-Android**, wählen Sie **Systemeigene Clientanwendung** aus, und klicken Sie auf **Weiter**.
6. Geben Sie als Umleitungs-URI `http://TodoListClient` ein. Klicken Sie auf **Fertig stellen**.
7. Suchen Sie auf der Anwendungsseite den Wert der Anwendungs-ID, und notieren Sie ihn. Sie benötigen ihn später für die Konfiguration Ihrer Anwendung.
8. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.  Suchen Sie TodoListService, wählen Sie ihn aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Auf TodoListService zugreifen** hinzu. Klicken Sie anschließend auf **Fertig**.

Für die Erstellung mit Maven können Sie „pom.xml“ auf der obersten Ebene verwenden:

1. Klonen Sie dieses Repository in einem Verzeichnis Ihrer Wahl:

  `$ git clone https://github.com/Azure-Samples/active-directory-android.git`  
2. Führen Sie die Schritte im Abschnitt mit den [Voraussetzungen zum Einrichten der Maven-Umgebung für Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Maven) aus.
3. Richten Sie den Emulator mit SDK 19 ein.
4. Wechseln Sie zu dem Stammordner, in dem Sie das Repository geklont haben.
5. Führen Sie den folgenden Befehl aus: `mvn clean install`
6. Wechseln Sie in das Verzeichnis mit dem Schnellstartbeispiel: `cd samples\hello`
7. Führen Sie den folgenden Befehl aus: `mvn android:deploy android:run`

   Die App wird gestartet.
8. Geben Sie die Anmeldeinformationen des Testbenutzers ein, um sie auszuprobieren.

Zusätzlich zum AAR-Paket werden JAR-Pakete übermittelt.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Schritt 4: Herunterladen der Android-ADAL und Hinzufügen zum Eclipse-Arbeitsbereich
Wir haben die Verwendung für Sie vereinfacht. Sie haben mehrere Möglichkeiten zur Verwendung der ADAL in Ihrem Android-Projekt:

* Sie können den Quellcode nutzen, um diese Bibliothek in Eclipse zu importieren und mit Ihrer Anwendung zu verknüpfen.
* Bei Verwendung von Android Studio können Sie das Paketformat „AAR“ nutzen und auf die Binärdateien verweisen.

### <a name="option-1-source-zip"></a>Option 1: ZIP-Datei mit Quellcode
Klicken Sie zum Herunterladen einer Kopie des Quellcodes rechts auf der Seite auf **Download ZIP**. Sie können die ZIP-Datei auch [von GitHub herunterladen](https://github.com/AzureAD/azure-activedirectory-library-for-android/releases).

### <a name="option-2-source-via-git"></a>Option 2: Quellcode per Git
Geben Sie Folgendes ein, um den Quellcode des SDK über Git abzurufen:

    git clone https://github.com/AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Option 3: Binärdateien per Gradle
Sie können die Binärdateien aus dem zentralen Repository von Maven abrufen. Das AAR-Paket kann wie folgt in Ihr Projekt unter Android Studio eingefügt werden:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Option 4: AAR über Maven
Wenn Sie das M2Eclipse-Plug-In verwenden, können Sie die Abhängigkeit in Ihrer Datei „pom.xml“ angeben:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a>Option 5: JAR-Paket im Ordner „libs“
Sie können die JAR-Datei aus dem Repository von Maven abrufen und im Ordner **libs** Ihres Projekts ablegen. Außerdem müssen Sie die erforderlichen Ressourcen in Ihr Projekt kopieren, da sie in den JAR-Paketen nicht enthalten sind.

## <a name="step-5-add-references-to-android-adal-to-your-project"></a>Schritt 5: Hinzufügen von Verweisen auf die ADAL für Android zu Ihrem Projekt
1. Fügen Sie einen Verweis auf Ihr Projekt hinzu, und geben Sie ihn als Android-Bibliothek an. Wenn Sie damit nicht vertraut sind, finden Sie weitere Informationen auf der [Android Studio-Website](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Fügen Sie die Projektabhängigkeit für das Debuggen in Ihren Projekteinstellungen hinzu.
3. Aktualisieren Sie die Datei „AndroidManifest.xml“ Ihres Projekts, damit sie Folgendes enthält:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Erstellen Sie eine Instanz von AuthenticationContext als Hauptaktivität. Die Details dieses Aufrufs liegen außerhalb des Umfangs dieses Themas, aber das [Android-Beispiel für systemeigene Clients](https://github.com/AzureAD/azure-activedirectory-library-for-android) stellt eine gute Einstiegshilfe dar. Im folgenden Beispiel ist SharedPreferences der Standardcache, und die Autorität hat das Format `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Kopieren Sie diesen Codeblock, um die Beendigung der AuthenticationActivity zu behandeln, nachdem der Benutzer Anmeldeinformationen eingegeben und einen Autorisierungscode empfangen hat:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. Zum Anfordern eines Tokens definieren Sie einen Rückruf:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Abschließend fordern Sie mit diesem Rückruf ein Token an:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Im Folgenden finden Sie eine Erklärung der Parameter:

* *resource* ist erforderlich, und es handelt sich um die Ressource, auf die Sie zugreifen möchten.
* *clientid* ist erforderlich und stammt von Azure AD.
* *RedirectUri* muss für den Aufruf „acquireToken“ nicht angegeben werden. Sie können dafür Ihren Paketnamen verwenden.
* *PromptBehavior* ist eine Hilfe zum Abfragen von Anmeldeinformationen, um Cache und Cookie zu überspringen.
* *callback* wird aufgerufen, nachdem der Autorisierungscode für ein Token ausgetauscht wurde. Dazu gehört ein Objekt „AuthenticationResult“, mit dem Informationen zum Zugriffstoken, Ablaufdatum und ID-Token bereitgestellt werden.
* *acquireTokenSilent* ist optional. Sie können diesen Parameter aufrufen, um das Zwischenspeichern und Aktualisieren des Tokens durchzuführen. Er stellt auch die Synchronisierungsversion bereit. *userId* wird als Parameter akzeptiert.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Mit dieser exemplarischen Vorgehensweise haben Sie alles, was Sie für die erfolgreiche Integration in Azure Active Directory benötigen. Weitere Beispiele hierzu finden Sie im Repository „AzureADSamples/“ bei GitHub.

## <a name="important-information"></a>Wichtige Informationen

### <a name="broker"></a>Broker
Die Brokerkomponente wird im Intune-Unternehmensportal oder über die Microsoft Authenticator-App bereitgestellt. Das Konto wird in AccountManager erstellt. Der Kontotyp lautet „com.microsoft.workaccount“. AccountManager lässt nur ein SSO-Konto zu. Für den Benutzer wird ein SSO-Cookie erstellt, nachdem die Geräteabfrage (Challenge) für eine der Apps abgeschlossen wurde.

Weitere Informationen zum Konfigurieren mithilfe eines Brokers finden Sie im [Wiki-Artikel „Broker“](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Broker). 

### <a name="authority-url-and-ad-fs"></a>Autoritäts-URL und AD FS
Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) werden nicht als STS für die Produktion erkannt. Daher müssen Sie die Instanzenermittlung deaktivieren und für den AuthenticationContext-Konstruktor „false“ übergeben.

Für die Autoritäts-URL sind die STS-Instanz und ein [Mandantenname](https://login.microsoftonline.com/yourtenant.onmicrosoft.com) erforderlich.

### <a name="querying-cache-items"></a>Abfragen von Cacheelementen
Die ADAL stellt einen Standardcache unter SharedPreferences mit einigen einfachen Funktionen für die Cacheabfrage bereit. Sie können den aktuellen Cache wie folgt aus AuthenticationContext abrufen:

    ITokenCacheStore cache = mContext.getCache();

Sie können auch eine eigene Cacheimplementierung bereitstellen, falls Sie sie anpassen möchten.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Verhalten der Eingabeaufforderung
Die ADAL verfügt über eine Option, mit der Sie das Verhalten der Eingabeaufforderung angeben können. PromptBehavior.Auto zeigt die Benutzeroberfläche an, wenn das Aktualisierungstoken ungültig ist und Benutzeranmeldeinformationen erforderlich sind. Mit PromptBehavior.Always wird die Cachenutzung übersprungen und immer die Benutzeroberfläche angezeigt.

### <a name="silent-token-request-from-cache-and-refresh"></a>Automatische Tokenanforderung aus dem Cache und Aktualisierung
Für eine automatische Tokenanforderung wird die Popup-Benutzeroberfläche nicht verwendet, und eine Aktivität ist nicht erforderlich. Ein Token wird aus dem Cache zurückgegeben, sofern verfügbar. Wenn das Token abgelaufen ist, versucht diese Methode, es zu aktualisieren. Falls das Aktualisierungstoken abgelaufen ist oder einen Fehler verursacht, wird „AuthenticationException“ zurückgegeben.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Mit dieser Methode können Sie auch einen Synchronisierungsaufruf durchführen. Sie können „null“ auf „callback“ (Rückruf) festlegen oder acquireTokenSilentSync verwenden.

### <a name="diagnostics"></a>Diagnose
Dies sind die wichtigsten Informationsquellen zum Diagnostizieren von Problemen:

* Ausnahmen
* Protokolle
* Netzwerkablaufverfolgung

Beachten Sie, dass die Korrelations-IDs zentraler Bestandteil der Diagnose in der Bibliothek sind. Sie können für Ihre Korrelations-IDs festlegen, dass sie bei Bedarf angefordert werden können, wenn Sie eine ADAL-Anforderung mit anderen Vorgängen in Ihrem Code korrelieren möchten. Wenn Sie keine Korrelations-ID festlegen, generiert die ADAL eine nach dem Zufallsprinzip. Alle Protokollmeldungen und Netzwerkaufrufe werden dann mit der Korrelations-ID versehen. Die selbst generierte ID ändert sich mit jeder Anforderung.

#### <a name="errors--exceptions"></a>Fehler und Ausnahmen
Ausnahmen sind die erste Diagnosemaßnahme. Wir versuchen, hilfreiche Fehlermeldungen für Sie bereitzustellen. Sie können uns ein Problem melden, falls Sie eine Fehlermeldung finden, die nicht hilfreich ist. Geben Sie dabei auch die Geräteinformationen an, z.B. Modell und SDK-Nummer.

Wenn Sie mehr darüber erfahren möchten, welche Fehler Ihre App beheben sollte, lesen Sie die [Bewährten Methoden bei der Fehlerbehandlung](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-adal-error-handling). 

#### <a name="logs"></a>Protokolle
Sie können die Bibliothek so konfigurieren, dass Protokollmeldungen generiert werden, die Sie zum Diagnostizieren von Problemen verwenden können. Sie können die Protokollierung konfigurieren, indem Sie den folgenden Aufruf verwenden. Hiermit können Sie einen Rückruf konfigurieren, der von der ADAL zum Weitergeben der Protokollmeldungen genutzt wird, wenn diese erzeugt werden.

Informationen zum Aktivieren der Protokollierung finden Sie im [Wiki-Artikel „Protokollierung“](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Logging).

### <a name="session-cookies-in-webview"></a>Sitzungscookies in WebView
Android WebView löscht Sitzungscookies nach dem Schließen der App nicht. Dafür können Sie diesen Beispielcode verwenden:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Einzelheiten zu Cookies finden Sie in den [Informationen zu CookieSyncManager auf der Android-Website](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="ntlm-dialog-box"></a>NTLM-Dialogfeld
ADAL, Version 1.1.0, unterstützt ein NTLM-Dialogfeld, das über das onReceivedHttpAuthRequest-Ereignis des WebViewClient-Elements verarbeitet wird. Sie können das Layout und die Zeichenfolgen für das Dialogfeld anpassen.

### <a name="cross-app-sso"></a>App-übergreifendes SSO
Informationen zum Aktivieren von App-übergreifendem SSO unter Android mit ADAL finden Sie [hier](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

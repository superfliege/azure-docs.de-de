---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 06da33b91ef9846204b33ba2cb3dea40c75d425d
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988274"
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Verwenden der Microsoft Authentication Library (MSAL) zum Abrufen eines Tokens für die Microsoft Graph-API

Öffnen Sie `ViewController.swift`, und ersetzen Sie den Code mit:

```swift
import UIKit
import MSAL

// A View Controller that will respond to the events of the Storyboard.
class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {

    // Replace Your_Application_Id_Here with the client ID you received in the portal. The below is for running the demo only.
    let kClientID = "Your_Application_Id_Here"

    // These settings you don't need to edit unless you wish to attempt deeper scenarios with the app.
    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    let kAuthority = "https://login.microsoftonline.com/common"

    var accessToken = String()
    var applicationContext : MSALPublicClientApplication?

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

    /**
        Setup public client application in viewDidLoad
    */

    override func viewDidLoad() {

        super.viewDidLoad()

        do {

            /**
             Initialize a MSALPublicClientApplication with a given clientID and authority
             - clientId:            The clientID of your application, you should get this from the app portal.
             - authority:           A URL indicating a directory that MSAL can use to obtain tokens. In Azure AD
                                    it is of the form https://<instance/<tenant>, where <instance> is the
                                    directory host (e.g. https://login.microsoftonline.com) and <tenant> is a
                                    identifier within the directory itself (e.g. a domain associated to the
                                    tenant, such as contoso.onmicrosoft.com, or the GUID representing the
                                    TenantID property of the directory)
             - error                The error that occurred creating the application object, if any, if you're
                                    not interested in the specific error pass in nil.
             */

            guard let authorityURL = URL(string: kAuthority) else {
                self.loggingText.text = "Unable to create authority URL"
                return
            }

            let authority = try MSALAuthority(url: authorityURL)
            self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)

        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }

    override func viewWillAppear(_ animated: Bool) {

        super.viewWillAppear(animated)
        signoutButton.isEnabled = !self.accessToken.isEmpty
    }

    /**
     This button will invoke the authorization flow.
    */

    @IBAction func callGraphButton(_ sender: UIButton) {

        if self.currentAccount() == nil {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            self.acquireTokenInteractively()
        } else {
            self.acquireTokenSilently()
        }
    }

    func acquireTokenInteractively() {

        guard let applicationContext = self.applicationContext else { return }

        applicationContext.acquireToken(forScopes: kScopes) { (result, error) in

            if let error = error {

                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func acquireTokenSilently() {

        guard let applicationContext = self.applicationContext else { return }

        /**
         Acquire a token for an existing account silently
         - forScopes:           Permissions you want included in the access token received
                                in the result in the completionBlock. Not all scopes are
                                guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
                                authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
                                flow completes, or encounters an error.
         */

        applicationContext.acquireTokenSilent(forScopes: kScopes, account: self.currentAccount()) { (result, error) in

            if let error = error {

                let nsError = error as NSError

                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                if (nsError.domain == MSALErrorDomain
                    && nsError.code == MSALErrorCode.interactionRequired.rawValue) {

                    DispatchQueue.main.async {
                        self.acquireTokenInteractively()
                    }

                } else {
                    self.updateLogging(text: "Could not acquire token silently: \(error)")
                }

                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func currentAccount() -> MSALAccount? {

        guard let applicationContext = self.applicationContext else { return nil }

        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        do {

            let cachedAccounts = try applicationContext.allAccounts()

            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }

        } catch let error as NSError {

            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }

        return nil
    }
```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

#### <a name="getting-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

Das Aufrufen der `acquireToken`-Methode führt zum Öffnen eines Browserfensters, in dem der Benutzer aufgefordert wird, sich anzumelden. Anwendungen erfordern in der Regel, dass sich ein Benutzer interaktiv anmeldet, wenn er erstmals auf eine geschützte Ressource zugreifen muss oder ein automatischer Vorgang zum Beziehen eines Tokens misslingt (wenn z.B. das Kennwort des Benutzers abgelaufen ist).

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die Methode `acquireTokenSilent` verwaltet das Abrufen und Erneuern von Token ohne Eingreifen des Benutzers. Nachdem `acquireToken` zum ersten Mal ausgeführt wurde, ist `acquireTokenSilent` die übliche Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen bei nachfolgenden Aufrufen verwendet werden, da Aufrufe zum Anfordern oder Verlängern von Token automatisch erfolgen.

Irgendwann schlägt `acquireTokenSilent` fehl, z.B. wenn sich der Benutzer abgemeldet oder sein Kennwort auf einem anderen Gerät geändert hat. Wenn die MSAL feststellt, dass das Problem durch Anfordern einer interaktiven Aktion gelöst werden kann, löst sie eine `MSALErrorCode.interactionRequired`-Ausnahme aus. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1. Sofortiges Aufrufen von `acquireToken`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein Offline-Inhalt in der Anwendung für den Benutzer verfügbar ist. Die Beispielanwendung, die durch dieses Schritt-für-Schritt-Setup erstellt wird, verwendet dieses Muster: Sie sehen es beim ersten Ausführen der Anwendung in Aktion. Da noch kein Benutzer die Anwendung verwendet hat, enthält `applicationContext.allAccounts().first` einen Nullwert, und eine ` MSALErrorCode.interactionRequired `-Ausnahme wird ausgelöst. Der Code im Beispiel behandelt dann die Ausnahme durch Aufrufen von `acquireToken`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird.

2. Anwendungen können dem Benutzer auch visuell zu verstehen geben, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden wählen oder die Anwendung `acquireTokenSilent` zu einem späteren Zeitpunkt wiederholen kann. Dies wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden, z.B. wenn Offline-Inhalt in der Anwendung verfügbar ist. In diesem Fall kann der Benutzer entscheiden, wann er sich für den Zugriff auf die geschützte Ressource anmelden oder die veralteten Informationen aktualisieren möchte. Ihre Anwendung kann sich auch für einen Wiederholungsversuch von `acquireTokenSilent` entscheiden, wenn das Netzwerk wiederhergestellt wird, nachdem es vorübergehend nicht verfügbar war.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

Fügen Sie die nachstehende neue Methode zu `ViewController.swift` hinzu. Die Methode dient zum Stellen einer `GET`-Anforderung an die Microsoft Graph-API mithilfe eines *HTTP-Autorisierungsheaders*:

```swift
 func getContentWithToken() {

        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
    
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in

            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }

            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {

                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            self.updateLogging(text: "Result from Graph: \(result))")

        }.resume()
    }
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Ausführen eines REST-Aufrufs einer geschützten API

In dieser Beispielanwendung dient die `getContentWithToken()`-Methode zum Richten einer HTTP `GET`-Anforderung an eine geschützte Ressource, die ein Token erfordert, und anschließenden Zurückgeben des Inhalts an den Aufrufer. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In diesem Beispiel ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Einrichten der Abmeldung

Fügen Sie die folgende Methode zu `ViewController.swift` zum Abmelden des Benutzers hinzu:

```swift 
 @IBAction func signoutButton(_ sender: UIButton) {

        guard let applicationContext = self.applicationContext else { return }

        guard let account = self.currentAccount() else { return }

        do {

            /**
             Removes all tokens from the cache for this application for the provided account
             - account:    The account to remove from the cache
             */

            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signoutButton.isEnabled = false

        } catch let error as NSError {

            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }

}
```

<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Weitere Informationen zum Abmelden

Die `signoutButton`-Methode entfernt den Benutzer aus dem MSAL-Benutzercache. Dadurch wird die MSAL effektiv angewiesen, den aktuellen Benutzer zu vergessen, sodass eine künftige Anforderung zum Abrufen eines Tokens nur erfolgreich ist, wenn diese interaktiv gestaltet wird.

Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, werden von der MSAL Szenarios unterstützt, in denen mehrere Konten gleichzeitig angemeldet werden können. Ein Beispiel ist eine E-Mail-Anwendung, in der ein Benutzer mehrere Konten hat.
<!--end-collapse-->

## <a name="register-the-callback"></a>Registrieren des Rückrufs

Sobald der Benutzer die Authentifizierung durchgeführt hat, leitet der Browser den Benutzer wieder zurück zur Anwendung. Befolgen Sie den unten aufgeführten Schritte, um diesen Rückruf registrieren:

1. Öffnen Sie `AppDelegate.swift`, und importieren Sie MSAL:

```swift
import MSAL
```

<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Fügen Sie die folgende Methode zu Ihrer <code>AppDelegate</code>-Klasse hinzu, um Rückrufe zu verarbeiten:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

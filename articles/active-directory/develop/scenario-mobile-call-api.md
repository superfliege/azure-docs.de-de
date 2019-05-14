---
title: Mobile App, die Web-APIs aufruft (Aufrufen einer Web-API) | Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Aufrufen einer Web-API)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fd65b9f97c373c55a3486e06e83fca7cf824cad
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080132"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Mobile App, die Web-APIs aufruft – Aufrufen einer Web-API

Sobald Ihre App einen Benutzer angemeldet und Token erhalten hat, macht MSAL mehrere Informationen über den Benutzer, dessen Umgebung und die ausgestellten Token verfügbar. Ihre App kann diese Werte verwenden, um eine Web-API aufzurufen oder einem Benutzer eine Willkommensnachricht anzuzeigen.

Zunächst werden wir das MSAL-Ergebnis untersuchen. Anschließend wird erklärt, wie ein Zugriffstoken von `AuthenticationResult` oder `result` zum Aufrufen einer geschützten Web-API verwendet wird.

## <a name="msal-result"></a>MSAL-Ergebnis

- `AccessToken`: Wird zum Aufrufen geschützter Web-APIs in einer HTTP-Beareranforderung verwendet.
- `IdToken`: Enthält nützliche Ansprüche zum angemeldeten Benutzer, wie dessen Namen, Basismandanten und eindeutigen Bezeichner zum Speichern.
- `ExpiresOn`: Die Ablaufzeit des Tokens. MSAL übernimmt die automatische Aktualisierung für Apps.
- `TenantId`: Der für die Anmeldung verwendete Bezeichner des Benutzermandanten. Bei Gastbenutzern (Azure AD B2B) handelt es sich hierbei um den Mandanten, mit dem sich der Benutzer angemeldet hat, nicht um seinen Basismandanten.  
- `Scopes`: Die Bereiche, die mit Ihrem Token gewährt wurden. Dabei handelt es sich möglicherweise um eine Teilmenge Ihrer Anforderung.

Darüber hinaus stellt MSAL auch eine Abstraktion für einen `Account` bereit. Ein Konto stellt das Konto dar, mit dem der aktuelle Benutzer angemeldet ist.

- `HomeAccountIdentifier`: Der Bezeichner des Basismandanten des Benutzers.
- `UserName`: Der bevorzugt Benutzername des Benutzers. Bei Azure AD B2C-Benutzern kann diese Angabe fehlen.
- `AccountIdentifier`: Der Bezeichner des angemeldeten Benutzers. In den meisten Fällen stimmt dieser mit `HomeAccountIdentifier` überein, sofern der Benutzer nicht als Gast in einem anderen Mandanten fungiert.

## <a name="calling-an-api"></a>Aufrufen einer API

Wenn das Zugriffstoken bereit steht, ist es einfach, eine Web-API aufzurufen. Ihre App verwendet dieses Token, erstellt eine HTTP-Anforderung und führt diese aus.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called graph, process data and send to UI 
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put Access Token in HTTP request 
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put Access token in HTTP Request
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

            // Successfully got data from Graph
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put Access token in HTTP request 
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Erstellen mehrerer API-Anforderungen

Wenn Sie die gleiche API mehrmals oder mehrere APIs aufrufen müssen, müssen Sie beim Erstellen Ihrer App zusätzliche Aspekte berücksichtigen:

- ***Inkrementelle Zustimmung***: Microsoft Identity Platform ermöglicht es Apps, Benutzereinwilligungen anstatt vorab erst bei Bedarf einzuholen. Jedes Mal, wenn Ihre App für den Aufruf einer API bereit ist, sollte sie nur die Bereiche anfordern, die sie verwenden möchte.
- ***Bedingter Zugriff***: In bestimmten Szenarios erhalten Sie möglicherweise weitere Anforderungen zum bedingten Zugriff, wenn Sie mehrere APIs anfordern. Stellen Sie für den Umgang mit diesem Szenario sicher, dass Sie Fehler bei automatischen Anforderungen erkennen, und bereiten Sie sich auf eine interaktive Anforderung vor. Dieser Fall kann auftreten, wenn für die erste Anforderung keine Richtlinien für den bedingten Zugriff angewendet wurden und Ihre App versucht, automatisch auf eine neue API zuzugreifen, die bedingten Zugriff erfordert. Weitere Informationen finden Sie unter [Anleitung für Entwickler zum bedingten Zugriff mit Azure Active Directory](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-mobile-production.md)

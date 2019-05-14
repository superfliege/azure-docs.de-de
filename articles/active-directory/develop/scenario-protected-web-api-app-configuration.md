---
title: Geschützte Web-API – App-Codekonfiguration | Azure
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen und den Code Ihrer Anwendung konfigurieren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e206cb29338445e30a7462bcbaf0079236e75510
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080112"
---
# <a name="protected-web-api---code-configuration"></a>Geschützte Web-API – Codekonfiguration

Um den Code für Ihre geschützte Web-API erfolgreich zu konfigurieren, müssen Sie wissen, wodurch die APIs geschützt werden, was Sie zum Konfigurieren des Bearertokens benötigen und wie das Token überprüft wird.

## <a name="what-makes-aspnetaspnet-core-apis-protected"></a>Wodurch werden ASP.NET-/ASP.NET Core-APIs geschützt?

Wie bei Web-Apps gelten ASP.NET-/ASP.NET Core-Web-APIs als „geschützt“, wenn ihren Controlleraktionen das `[Authorize]`-Attribut als Präfix vorangestellt wird. Aus diesem Grund können die Controlleraktionen nur aufgerufen werden, wenn die API mit einer Identität aufgerufen wird, die autorisiert ist.

Stellen Sie sich die folgenden Fragen:

- Woher kennt die Web-API die Identität der Anwendung, von der sie aufgerufen wird (nur eine Anwendung kann eine Web-API aufrufen)?
- Wenn die Anwendung die Web-API im Auftrag eines Benutzers aufgerufen hat, wie lautet die Identität des Benutzers?

## <a name="bearer-token"></a>Bearertoken

Die Informationen zur Identität der Anwendung und zum Benutzer (es sei denn, die Web-App akzeptiert Aufrufe zwischen Diensten aus einer Daemonanwendung) wird im Bearertoken gespeichert, das bei Aufruf der Anwendung im Header festgelegt wird.

Das folgende C#-Codebeispiel zeigt einen Client, der die API nach Anforderung eines Tokens bei MSAL.NET aufruft.

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Das Bearertoken wurde von einer Clientanwendung beim Microsoft Identity Platform-Endpunkt **für die Web-API** angefordert. Die Web-API ist die einzige Anwendung, die das Token und die darin enthaltenen Ansprüche überprüfen sollte. Die Clientanwendungen sollten niemals die Ansprüche in den Token prüfen. (Die Web-API könnte zu einem späteren Zeitpunkt verlangen, dass das Token verschlüsselt wird. Dadurch würde die Clientanwendung unterbrochen, was zu einer Kompromittierung der Zugriffstoken führen kann.)

## <a name="jwtbearer-configuration"></a>JwtBearer-Konfiguration

Dieser Abschnitt behandelt die Voraussetzungen für die Konfiguration des Bearertokens.

### <a name="config-file"></a>Konfigurationsdatei

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line of business app)
      Otherwise you can leave them set to common
      This can be:
      - A guid (Tenant ID = Directory ID)
      - 'common' (Any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Codeinitialisierung

Wenn die Anwendung für die Controlleraktion aufgerufen wird, die ein `[Authorize]`-Attribut aufweist, prüft ASP.NET/ASP.NET Core das Bearertoken im Autorisierungsheader der aufrufenden Anforderung und extrahiert das Zugriffstoken. Dieses wird anschließend an die JwtBearer-Middleware weitergeleitet, die die Microsoft-Identitätsmodellerweiterungen für .NET aufruft.

In ASP.NET Core wird diese Middleware in der Datei `Startup.cs` initialisiert.

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Die Middleware wird der Web-API durch die folgende Anweisung hinzugefügt:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Zurzeit werden durch die ASP.NET Core-Vorlagen Web-APIs für Azure AD v1.0 erstellt. Sie können sie jedoch einfach so ändern, dass sie den Microsoft Identity Platform-Endpunkt verwenden, indem Sie in der Datei `Startup.cs` den folgenden Code hinzufügen.

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API
    options.Authority += "/v2.0";

    // The web API accepts as audiences are both the Client ID (options.Audience) and api://{ClientID}
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line of business apps),
    // we inject our own multi-tenant validation logic (which even accepts both V1 and V2 tokens)
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Tokenüberprüfung

Die JwtBearer-Middleware wird, ebenso wie die OpenID Connect-Middleware in Web-Apps, von `TokenValidationParameters` zum Überprüfen des Tokens angeleitet. Das Token wird ggf. entschlüsselt, die Ansprüche werden extrahiert, und die Signatur wird überprüft. Anschließend wird das Token anhand folgender Daten überprüft:

- Es ist für die Web-API vorgesehen. (Zielgruppe)
- Es wurde für eine Anwendung ausgegeben, die die Web-API aufrufen darf. (Sub)
- Es wurde von einem vertrauenswürdigen STS (Sicherheitstokenserver) ausgegeben. (Aussteller)
- Die Tokenlebensdauer befindet sich im zulässigen Bereich. (Ablauf)
- Es wurde nicht manipuliert. (Signatur)

Es können auch spezielle Validierungen vorliegen. Beispielsweise können Sie sicherstellen, dass Signaturschlüssel (bei Einbettung in einem Token) vertrauenswürdig sind und dass das Token nicht wiedergegeben wird. Für einige Protokolle sind bestimmte Validierungen erforderlich.

### <a name="validators"></a>Validierungssteuerelemente

Die Validierungsschritte werden in Validierungssteuerelementen erfasst, die alle in der Open Source-Bibliothek der [Microsoft-Identitätsmodellerweiterung für .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) in einer Quelldatei befinden: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)

Die Validierungssteuerelemente werden in der folgenden Tabelle beschrieben:

| Validierungssteuerelement | BESCHREIBUNG |
|---------|---------|
| `ValidateAudience` | Stellt sicher, dass das Token für die Anwendung bestimmt ist, die das Token (für mich) überprüft. |
| `ValidateIssuer` | Stellt sicher, dass das Token von einem vertrauenswürdigen STS (von einer Person, der ich vertraue) ausgestellt wurde. |
| `ValidateIssuerSigningKey` | Stellt sicher, dass die Anwendung, die das Token überprüft, dem Schlüssel vertraut, der zum Signieren des Tokens verwendet wurde (Sonderfall, bei dem der Schlüssel im Token eingebettet ist, in der Regel nicht erforderlich). |
| `ValidateLifetime` | Stellt sicher, dass das Token noch immer (oder bereits) gültig ist. Hierzu wird überprüft, ob die Lebensdauer des Tokens (`notbefore`, `expires` Ansprüche) im gültigen Bereich liegt. |
| `ValidateSignature` | Stellt sicher, dass das Token nicht manipuliert wurde. |
| `ValidateTokenReplay` | Stellt sicher, dass das Token nicht wiedergegeben wird (Sonderfall für einige Protokolle zur einmaligen Verwendung). |

Allen Validierungssteuerelementen werden Eigenschaften der `TokenValidationParameters`-Klasse zugeordnet, die selbst über die ASP.NET-/ASP.NET Core-Konfiguration initialisiert werden. In den meisten Fällen müssen Sie die Parameter nicht ändern. Es gibt eine Ausnahme für Anwendungen, die keine einzelnen Mandanten sind (also Web-Apps, die Benutzer aus einer beliebigen Organisation oder persönliche Microsoft-Konten akzeptieren) – in diesem Fall muss der Aussteller überprüft werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-protected-web-api-production.md)

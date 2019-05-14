---
title: Single-Page-Webanwendung (Aufrufen einer Web-API) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Single-Page-Webanwendung (Aufrufen einer Web-API) erstellen
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01f141a5374c0e794b264f6e0135ca3e15ff8359
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080096"
---
# <a name="single-page-application---call-a-web-api"></a>Single-Page-Webanwendung – Aufrufen einer Web-API

Es wird empfohlen, dass Sie die `acquireTokenSilent`-Methode aufrufen, um ein Zugriffstoken zu erhalten oder zu verlängern, bevor Sie eine Web-API aufrufen. Wenn Sie über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="call-a-web-api"></a>Aufrufen einer Web-API

### <a name="javascript"></a>JavaScript

Verwenden Sie das erhaltene Zugriffstoken als Bearer in einer HTTP-Anforderung, um Web-APIs aufzurufen, z. B. die Microsoft Graph-API. Beispiel: 

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Wie im Abschnitt zum [Abrufen von Token](scenario-spa-acquire-token.md) bereits erwähnt nutzt der MSAL Angular-Wrapper den HTTP-Interceptor, um Zugriffstoken automatisch im Hintergrund abzurufen und sie an die HTTP-Anforderungen von APIs anzufügen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-spa-production.md)

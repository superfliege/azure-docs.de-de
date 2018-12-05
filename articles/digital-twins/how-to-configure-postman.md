---
title: 'Vorgehensweise: Konfigurieren von Postman für Azure Digital Twins | Microsoft-Dokumentation'
description: 'Vorgehensweise: Konfigurieren von Postman für Azure Digital Twins'
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: b22bf34a06966f917cdcdd07c28ead2d042061c1
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163995"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Vorgehensweise: Konfigurieren von Postman für Azure Digital Twins

Dieser Artikel beschreibt, wie Sie eine Azure Active Directory-Anwendung (Azure AD) für die Verwendung der impliziten OAuth 2.0-Genehmigung konfigurieren. Anschließend wird erläutert, wie Sie den Postman REST-Client so konfigurieren, dass er HTTP-Anforderungen die Token enthalten, an Ihre Verwaltungs-APIs ausgibt.

## <a name="postman-summary"></a>Postman: Zusammenfassung

Führen Sie erste Schritte mit Azure Digital Twins aus, indem Sie ein REST-Clienttool wie [Postman](https://www.getpostman.com/) verwenden, um Ihre lokale Testumgebung vorzubereiten. Der Postman-Client unterstützt die schnelle Erstellung komplexer HTTP-Anforderungen. Laden Sie die Desktopversion des Postman-Clients herunter, indem Sie zu [www.getpostman.com/apps](https://www.getpostman.com/apps) navigieren.

[Postman](https://www.getpostman.com/) ist ein REST-Testtool, das wichtige HTTP-Anforderungsfunktionen in einer nützlichen desktop- und plugin-basierten grafischen Benutzeroberfläche ermittelt. Über den Postman-Client können Lösungsentwickler die Art der HTTP-Anforderung (POST, GET, UPDATE, PATCH und DELETE), den aufzurufenden API-Endpunkt und die Verwendung von SSL angeben. Postman unterstützt außerdem das Hinzufügen von HTTP-Anforderungsheadern, Parametern, Formulardaten und Textkörpern.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Konfigurieren von Azure Active Directory für die Verwendung der impliziten OAuth 2.0-Gewährung

Konfigurieren Sie Ihre Azure AD-App für die Verwendung der impliziten OAuth 2.0-Gewährung.

1. Führen Sie die Schritte in [diesem Schnellstart](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) aus, um eine Azure AD-Anwendung vom Typ „Nativ“ zu erstellen. Alternativ können Sie eine vorhandene App-Registrierung vom Typ „Nativ“ wiederverwenden.

1. Geben Sie unter **Erforderliche Berechtigungen** `Azure Digital Twins` ein, und wählen Sie **Delegierte Berechtigungen** aus. Wählen Sie dann **Berechtigungen erteilen** aus.

    ![Azure AD-App-Registrierungen, API hinzufügen](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Klicken Sie auf **Manifest**, um das Anwendungsmanifest für Ihre App zu öffnen. Legen Sie *oauth2AllowImplicitFlow* auf `true` fest.

      ![Impliziter Azure AD-Fluss][1]

1. Konfigurieren Sie eine **Antwort-URL** namens [`https://www.getpostman.com/oauth2/callback`](https://www.getpostman.com/oauth2/callback).

      ![Azure AD-Antwort-URL][2]

1. Kopieren Sie die **Anwendungs-ID** Ihrer Azure AD-App, und bewahren Sie sie auf. Sie wird weiter unten verwendet.

## <a name="configure-the-postman-client"></a>Konfigurieren des Postman-Clients

Nun richten Sie Postman ein und konfigurieren das Tool so, dass ein Azure AD-Token abgerufen wird. Geben Sie anschließend unter Verwendung des abgerufenen Tokens eine authentifizierte HTTP-Anforderung an Azure Digital Twins aus:

1. Navigieren Sie zu [www.getpostman.com]([https://www.getpostman.com/), um die App herunterzuladen.
1. Stellen Sie sicher, dass Ihre **Autorisierungs-URL** richtig ist. Sie sollte das folgende Format aufweisen:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | NAME  | Ersetzen durch | Beispiel |
    |---------|---------|---------|
    | IHR_AZURE_MANDANT | Der Name Ihres Mandanten oder Ihrer Organisation | `microsoft` |

1. Wählen Sie die Registerkarte **Authorization** (Autorisierung) und anschließend **OAuth 2.0** und **Get New Access Token** (Neues Zugriffstoken abrufen) aus.

    | Feld  | Wert |
    |---------|---------|
    | Gewährungstyp | `Implicit` |
    | Rückruf-URL | [`https://www.getpostman.com/oauth2/callback`](https://www.getpostman.com/oauth2/callback) |
    | Authentifizierungs-URL | Verwenden Sie die **Autorisierungs-URL** aus Schritt 2 oben. |
    | Client-ID | Verwenden Sie die **Anwendungs-ID** für die Azure AD-App aus dem vorherigen Abschnitt, die erstellt bzw. anderweitig eingesetzt wurde. |
    | Bereich | Nicht ausfüllen |
    | Zustand | Nicht ausfüllen |
    | Clientauthentifizierung | `Send as Basic Auth header` |

1. Der Client sollte jetzt folgendermaßen aussehen:

   ![Postman-Clientbeispiel][3]

1. Wählen Sie **Request Token** (Token anfordern) aus.

    >[!NOTE]
    >Versuchen Sie Folgendes, wenn Sie die Fehlermeldung „OAuth2 couldn’t be completed“ (OAuth2 konnte nicht abgeschlossen werden) erhalten:
    > * Schließen Sie Postman, und öffnen Sie das Tool dann erneut, um den Vorgang zu wiederholen.
  
1. Scrollen Sie nach unten, und wählen Sie **Use Token** (Token verwenden) aus.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie die Authentifizierung mit den Verwaltungs-APIs erfolgt, finden Sie unter [Authentifizieren mit APIs](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png

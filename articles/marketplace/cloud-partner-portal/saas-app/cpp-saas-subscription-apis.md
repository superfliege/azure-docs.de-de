---
title: SaaS – Verkaufen über Azure-APIs | Azure Marketplace
description: Erläutert das Erstellen eines SaaS-Angebots über Marketplace-APIs.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/17/2018
ms.author: pabutler
ms.openlocfilehash: a76fb2989320c64ad85b0f41f17798e2d9c743e1
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941944"
---
# <a name="saas-sell-through-azure---apis"></a>SaaS – Verkaufen über Azure: APIs

In diesem Artikel wird erklärt, wie Sie ein SaaS-Angebot mit APIs erstellen. Die APIs sind für das Zulassen von Abonnements Ihres SaaS-Angebots erforderlich, wenn Sie den Verkauf über Azure ausgewählt haben.  Dieser Artikel ist in zwei Abschnitte unterteilt:

-   Dienst-zu-Dienst-Authentifizierung zwischen SaaS-Dienst und Azure Marketplace
-   API-Methoden und -Endpunkte

Die folgenden APIs zur Integration Ihres SaaS-Diensts in Azure werden bereitgestellt:

-   Beheben
-   Abonnieren
-   Convert
-   Abbestellen

Das folgende Diagramm zeigt den Ablauf des Abonnements eines neuen Kunden und die Verwendung dieser APIs:

![SaaS-Angebot: API-Ablauf](./media/saas-offer-publish-api-flow.png)


## <a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Dienst-zu-Dienst-Authentifizierung zwischen SaaS-Dienst und Azure Marketplace

Azure legt keine Einschränkungen für die Authentifizierung fest, die die SaaS-Lösung für ihre Endbenutzer verfügbar macht. Wenn der SaaS-Dienst jedoch mit Azure Marketplace-APIs kommuniziert, erfolgt die Authentifizierung im Kontext einer Azure Active Directory-Anwendung (Azure AD).

Im folgenden Abschnitt wird das Erstellen einer Azure AD-Anwendung beschrieben.


### <a name="register-an-azure-ad-application"></a>Registrieren einer Azure AD-Anwendung

Jede Anwendung muss zunächst in einem Azure AD-Mandanten registriert werden, um die Funktionen von Azure AD nutzen zu können. Dieser Registrierungsvorgang umfasst das Angeben von Details zu Ihrer Anwendung in Azure AD. Beispielsweise muss die URL für den Speicherort angegeben werden, die URL, an die nach der Authentifizierung eines Benutzers Antworten gesendet werden sollen, der URI zum Identifizieren der App usw.

Führen Sie die folgenden Schritte aus, um mit dem Azure-Portal eine neue Anwendung zu registrieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wenn Sie unter Ihrem Konto mehrere Zugriffsmöglichkeiten haben, können Sie oben rechts auf Ihr Konto klicken und Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten festlegen.
3. Klicken Sie im linken Navigationsbereich auf den **Azure Active Directory**-Dienst und dann auf **App-Registrierungen** und **Registrierung einer neuen Anwendung**.

   ![SaaS: AD-App-Registrierungen](./media/saas-offer-app-registration.png)

4. Geben Sie auf der Seite „Erstellen“ die Registrierungsinformationen\' für Ihre Anwendung ein:
   - **Name**: Geben Sie einen aussagekräftigen Anwendungsnamen ein.
   - **Anwendungstyp**: 
     - Wählen Sie für [Clientanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application), die lokal auf dem Gerät installiert sind, die Option **Nativ** aus. Diese Einstellung wird für öffentliche [native OAuth-Clients](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) verwendet.
     - Wählen Sie die Option **Web-App/API** für [Clientanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) und [Ressourcen-/API-Anwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) aus, die auf einem sicheren Server installiert sind. Diese Einstellung wird für vertrauliche OAuth-[Webclients](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) und für öffentliche [Clients auf Basis von Benutzer-Agents](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client) verwendet.
     Außerdem kann dieselbe Anwendung sowohl einen Client als auch eine Ressource/API verfügbar machen.
   - **Anmelde-URL**: Geben Sie für Web-Apps/API-Anwendungen die Basis-URL Ihrer App an. **http:\//localhost:31544** könnte beispielsweise die URL für eine Web-App sein, die auf einem lokalen Computer ausgeführt wird. Benutzer können sich mit dieser URL dann bei einer Webclientanwendung anmelden.
   - **Umleitungs-URI**: Geben Sie für native Anwendungen den URI an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. **http:\//MeineErsteAADApp**.

     ![SaaS: AD-App-Registrierungen](./media/saas-offer-app-registration-2.png) Spezifische Beispiele für Webanwendungen oder native Anwendungen finden Sie in den Schnellstart-Einrichtungsanleitungen, die im Abschnitt „Erste Schritte“ des [Azure AD-Entwicklerhandbuchs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) verfügbar sind.

5. Klicken Sie auf **Erstellen**, wenn Sie fertig sind. Azure AD weist Ihrer Anwendung eine eindeutige Anwendungs-ID zu, und Sie gelangen auf die Hauptseite für die Registrierung Ihrer Anwendung. Je nachdem, ob es sich bei Ihrer Anwendung um eine Web- oder eine systemeigene Anwendung handelt, werden jeweils andere Optionen zum Hinzufügen weiterer Funktionen zu Ihrer Anwendung bereitgestellt.

   **Hinweis:** Die neu registrierte Anwendung wird standardmäßig so konfiguriert, dass sich nur Benutzer desselben Mandanten an Ihrer Anwendung anmelden können.

<a name="api-methods-and-endpoints"></a>API-Methoden und -Endpunkte
-------------------------

In den folgenden Abschnitten werden die API-Methoden und -Endpunkte beschrieben, die zum Aktivieren von Abonnements für ein SaaS-Angebot verfügbar sind.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Abrufen eines Tokens basierend auf der Azure AD-App

HTTP-Methode

`GET`

*Anforderungs-URL*

**https://login.microsoftonline.com/ *{tenantId}* /oauth2/token**

*URI-Parameter*

|  **Parametername**  | **Erforderlich**  | **Beschreibung**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | True          | Mandanten-ID der registrierten AAD-Anwendung   |
|  |  |  |


*Anforderungsheader*

|  **Headername**  | **Erforderlich** |  **Beschreibung**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Typ     | True         | Der Anforderung zugeordneter Inhaltstyp. Standardwert: `application/x-www-form-urlencoded`.  |
|  |  |  |


*Anforderungstext*

| **Eigenschaftenname**   | **Erforderlich** |  **Beschreibung**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | Gewährungstyp. Standardwert: `client_credentials`.                    |
|  Client_id          | True         |  Der Azure AD-App zugeordneter Client-/App-Bezeichner.                  |
|  client_secret      | True         |  Der Azure AD-App zugeordnetes Kennwort.                               |
|  Resource           | True         |  Zielressource, für die das Token angefordert wird. Standardwert: `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Antwort*

|  **Name**  | **Typ**       |  **Beschreibung**    |
| ---------- | -------------  | ------------------- |
| 200 – OK    | TokenResponse  | Anforderung erfolgreich   |
|  |  |  |

*TokenResponse*

Beispiel für Antworttoken:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "62d94f6c-d599-489b-a797-3e10e42fbe22",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Marketplace-API-Endpunkt und API-Version

Der Endpunkt für die Azure Marketplace-API ist `https://marketplaceapi.microsoft.com`.

Die aktuelle API-Version ist `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Abonnement auflösen

Eine POST-Aktion beim Auflösen des Endpunkts ermöglicht Benutzern das Auflösen eines Marketplace-Tokens in eine permanente Ressourcen-ID.  Die Ressourcen-ID ist der eindeutige Bezeichner des SAAS-Abonnements. 

Wenn ein Benutzer zur Website eines ISV weitergeleitet wird, enthält die URL in den Abfrageparametern ein Token. Es wird erwartet, dass der ISV dieses Token verwendet und eine Anforderung zu dessen Auflösung stellt. Die Antwort enthält die eindeutige ID des SAAS-Abonnements, den Namen, die Angebots-ID und den Plan für die Ressource. Dieses Token ist nur eine Stunde gültig.

*Anforderung*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parametername** |     **Beschreibung**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.   |
|  |  |


*Header*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
| x-ms-correlationid | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser korreliert alle Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| Inhaltstyp       | Ja          | `application/json`                                        |
| authorization      | Ja          | JWT-Bearertoken (JSON Web Token)                    |
| x-ms-marketplace-token| Ja| Der Tokenabfrageparameter in der URL, wenn der Benutzer von Azure zur Website des SaaS-ISV umgeleitet wird. **Hinweis:** Dieses Token ist nur eine Stunde lang gültig. Führen Sie zusätzlich eine URL-Decodierung des Tokenwerts im Browser durch, bevor Sie ihn verwenden.|
|  |  |  |
  

*Antworttext*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parametername** | **Datentyp** | **Beschreibung**                       |
|--------------------|---------------|---------------------------------------|
| id                 | Zeichenfolge        | ID des SaaS-Abonnements.          |
| subscriptionName| Zeichenfolge| Name des SaaS-Abonnements, das vom Benutzer beim Abonnieren des SaaS-Diensts in Azure festgelegt wurde.|
| OfferId            | Zeichenfolge        | Angebots-ID, die der Benutzer abonniert hat. |
| planId             | Zeichenfolge        | Tarif-ID, die der Benutzer abonniert hat.  |
|  |  |  |


*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token erfolgreich aufgelöst.                                                            |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder eine ungültige API-Version wurde angegeben. Fehler beim Auflösen des Tokens, da das Token entweder falsch formatiert oder abgelaufen ist (nach dem Generieren ist das Token nur eine Stunde lang gültig). |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                                 |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                                |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen, versuchen Sie es später erneut.                                        |
|  |  |  |


*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben; andernfalls ist dieser Wert die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Nein            | Dieser Wert wird nur für die Antwort 429 festgelegt.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abonnieren

Der Abonnementendpunkt ermöglicht Benutzern das Starten des Abonnements eines SaaS-Diensts für einen bestimmten Tarif und das Aktivieren der Abrechnung im Commerce-System.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Eindeutige ID des SaaS-Abonnements, die nach dem Auflösen des Tokens über die API-Auflösung abgerufen wird.                              |
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

|  **Headerschlüssel**        | **Erforderlich** |  **Beschreibung**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Nein          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| x-ms-correlationid     |   Nein          | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| If-Match/If-None-Match |   Nein          |   Starker ETag-Wert des Validierungssteuerelements.                                                          |
| Inhaltstyp           |   Ja        |    `application/json`                                                                   |
|  authorization         |   Ja        |    JWT-Bearertoken (JSON Web Token)                                               |
| x-ms-marketplace-session-mode| Nein  | Flag zum Aktivieren des Probelaufmodus beim Abonnieren eines SaaS-Angebots. Wenn es festgelegt ist, wird das Abonnement nicht belastet. Nützlich für ISV-Testszenarien. Legen Sie es auf **„dryrun“** fest.|
|  |  |  |

*Text*

``` json
{
    "lanId": "",
}
```

| **Elementname** | **Datentyp** | **Beschreibung**                      |
|------------------|---------------|--------------------------------------|
| planId           | (Erforderliche) Zeichenfolge        | Tarif-ID des SaaS-Diensts, den der Benutzer abonniert.  |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktivierung des SaaS-Abonnements für einen bestimmten Tarif erhalten.                   |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder der Text der JSON-Datei ist falsch formatiert. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                   |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                  |
| 409                  | `Conflict`           | Für das Abonnement wird derzeit ein anderer Vorgang ausgeführt.                     |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                  |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen, versuchen Sie es später erneut.                          |
|  |  |  |

Verfolgen Sie für die Antwort 202 den Status des Anforderungsvorgangs am Header „Operation-Location“ nach. Die Authentifizierung ist identisch mit anderen Marketplace-APIs.

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben; andernfalls ist dieser Wert die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Ja          | Intervall, in dem der Client den Status überprüfen kann.                                                       |
| Operation-Location | Ja          | Link zu einer Ressource, um den Vorgangsstatus abzurufen.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Endpunkt der Tarifänderung

Der Änderungsendpunkt ermöglicht dem Benutzer das Konvertieren seines derzeit abonnierten Tarifs in einen neuen Tarif.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID des SaaS-Abonnements.                              |
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

| **Headerschlüssel**          | **Erforderlich** | **Beschreibung**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client. Es wird eine GUID empfohlen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
| x-ms-correlationid      | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| If-Match /If-None-Match | Nein            | Starker ETag-Wert des Validierungssteuerelements.                              |
| Inhaltstyp            | Ja          | `application/json`                                        |
| authorization           | Ja          | JWT-Bearertoken (JSON Web Token)                    |
|  |  |  |

*Text*

```json
{
    "planId": ""
}
```

|  **Elementname** |  **Datentyp**  | **Beschreibung**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (Erforderliche) Zeichenfolge         | Tarif-ID des SaaS-Diensts, den der Benutzer abonniert.          |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktivierung des SaaS-Abonnements für einen bestimmten Tarif erhalten.                   |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder der Text der JSON-Datei ist falsch formatiert. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                   |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                  |
| 409                  | `Conflict`           | Für das Abonnement wird derzeit ein anderer Vorgang ausgeführt.                     |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                  |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen, versuchen Sie es später erneut.                          |
|  |  |  |

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben; andernfalls ist dieser Wert die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Ja          | Intervall, in dem der Client den Status überprüfen kann.                                                       |
| Operation-Location | Ja          | Link zu einer Ressource, um den Vorgangsstatus abzurufen.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Löschen eines Abonnements

Die DELETE-Aktion am Abonnementendpunkt ermöglicht einem Benutzer das Löschen eines Abonnements mit einer bestimmten ID.

*Anforderung*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID des SaaS-Abonnements.                              |
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client. Es wird eine GUID empfohlen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.                                                           |
| x-ms-correlationid | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| authorization      | Ja          | JWT-Bearertoken (JSON Web Token)                    |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Aktivierung des SaaS-Abonnements für einen bestimmten Tarif erhalten.                   |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder der Text der JSON-Datei ist falsch formatiert. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                   |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                  |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                  |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen. Versuchen Sie es später noch mal.                          |
|  |  |  |

Verfolgen Sie für die Antwort 202 den Status des Anforderungsvorgangs am Header „Operation-Location“ nach. Die Authentifizierung ist identisch mit anderen Marketplace-APIs.

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben, andernfalls ist dies die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Ja          | Intervall, in dem der Client den Status überprüfen kann.                                                       |
| Operation-Location | Ja          | Link zu einer Ressource, um den Vorgangsstatus abzurufen.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Vorgangsstatus abrufen

Dieser Endpunkt ermöglicht dem Benutzer das Nachverfolgen des Status eines ausgelösten asynchronen Vorgangs (Abonnieren/Abbestellen/Tarif ändern).

*Anforderung*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/ *{operationId}* ?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Eindeutige ID für den ausgelösten Vorgang.                |
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client. Es wird eine GUID empfohlen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
| x-ms-correlationid | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
| authorization      | Ja          | JWT-Bearertoken (JSON Web Token)                    |
|  |  |  | 

*Antworttext*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametername** | **Datentyp** | **Beschreibung**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | Zeichenfolge        | ID des Vorgangs.                                                                      |
| status             | Enum          | Vorgangsstatus, einer der folgenden Werte: `In Progress`, `Succeeded`, oder `Failed`.          |
| resourceLocation   | Zeichenfolge        | Link zum erstellten oder geänderten Abonnement. Hilft dem Client beim Abrufen des aktualisierten Status nach dem Vorgang. Dieser Wert wird für `Unsubscribe`-Vorgänge nicht festgelegt. |
| created            | Datetime      | Erstellungszeitpunkt (UTC) des Vorgangs.                                                           |
| lastModified       | Datetime      | Letzte Aktualisierung des Vorgangs in UTC.                                                      |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Die GET-Anforderung wurde erfolgreich aufgelöst, und der Text enthält die Antwort.    |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder eine ungültige API-Version wurde angegeben. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                      |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                     |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                     |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen, versuchen Sie es später erneut.                             |
|  |  |  |

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben, andernfalls ist dies die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Ja          | Intervall, in dem der Client den Status überprüfen kann.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Abonnement abrufen

Die GET-Aktion am Abonnementendpunkt ermöglicht einem Benutzer das Abrufen eines Abonnements mit einem bestimmten Ressourcenbezeichner.

*Anforderung*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{subscriptionId}* ?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID des SaaS-Abonnements.                              |
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.                                                           |
| x-ms-correlationid | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| authorization      | Ja          | JWT-Bearertoken (JSON Web Token)                                                                    |
|  |  |  |

*Antworttext*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametername**     | **Datentyp** | **Beschreibung**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | Zeichenfolge        | ID der SaaS-Abonnementsressource in Azure.    |
| offerId                | Zeichenfolge        | Angebots-ID, die der Benutzer abonniert hat.         |
| planId                 | Zeichenfolge        | Tarif-ID, die der Benutzer abonniert hat.          |
| saasSubscriptionName   | Zeichenfolge        | Name des SaaS-Abonnements.                |
| saasSubscriptionStatus | Enum          | Vorgangsstatus  Einer der folgenden:  <br/> - `Subscribed`: Das Abonnement ist aktiv.  <br/> - `Pending`: Die Ressource wurde vom Benutzer erstellt, vom ISV jedoch nicht aktiviert.   <br/> - `Unsubscribed`: Der Benutzer hat das Abonnement gekündigt.   <br/> - `Suspended`: Der Benutzer hat das Abonnement angehalten.   <br/> - `Deactivated`:  Das Azure-Abonnement wurde angehalten.  |
| created                | Datetime      | Zeitstempelwert der Abonnementerstellung in UTC. |
| lastModified           | Datetime      | Zeitstempelwert der Abonnementänderung in UTC. |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Die GET-Anforderung wurde erfolgreich aufgelöst, und der Text enthält die Antwort.    |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder eine ungültige API-Version wurde angegeben. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                      |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                     |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                     |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen, versuchen Sie es später erneut.                             |
|  |  |  |

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben, andernfalls ist dies die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Nein            | Intervall, in dem der Client den Status überprüfen kann.                                                       |
| eTag               | Ja          | Link zu einer Ressource, um den Vorgangsstatus abzurufen.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Abonnements abrufen

Die GET-Aktion am Abonnementendpunkt ermöglicht einem Benutzer das Abrufen aller Abonnements für alle Angebote des ISV.

*Anforderung*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Parametername**  | **Beschreibung**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | Die Version des für diese Anforderung zu verwendenden Vorgangs. |
|  |  |

*Header*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nein            | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client. Es wird eine GUID empfohlen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.             |
| x-ms-correlationid | Nein            | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Wert dient zum Korrelieren aller Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| authorization      | Ja          | JWT-Bearertoken (JSON Web Token)                    |
|  |  |  |

*Antworttext*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametername**     | **Datentyp** | **Beschreibung**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | Zeichenfolge        | ID der SaaS-Abonnementsressource in Azure.    |
| offerId                | Zeichenfolge        | Angebots-ID, die der Benutzer abonniert hat.         |
| planId                 | Zeichenfolge        | Tarif-ID, die der Benutzer abonniert hat.          |
| saasSubscriptionName   | Zeichenfolge        | Name des SaaS-Abonnements.                |
| saasSubscriptionStatus | Enum          | Vorgangsstatus  Einer der folgenden:  <br/> - `Subscribed`: Das Abonnement ist aktiv.  <br/> - `Pending`: Die Ressource wurde vom Benutzer erstellt, vom ISV jedoch nicht aktiviert.   <br/> - `Unsubscribed`: Der Benutzer hat das Abonnement gekündigt.   <br/> - `Suspended`: Der Benutzer hat das Abonnement angehalten.   <br/> - `Deactivated`:  Das Azure-Abonnement wurde angehalten.  |
| created                | Datetime      | Zeitstempelwert der Abonnementerstellung in UTC. |
| lastModified           | Datetime      | Zeitstempelwert der Abonnementänderung in UTC. |
|  |  |  |

*Antwortcodes*

| **HTTP-Statuscode** | **Fehlercode**     | **Beschreibung**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Die GET-Anforderung wurde erfolgreich aufgelöst, und der Text enthält die Antwort.    |
| 400                  | `BadRequest`         | Entweder fehlen erforderliche Header, oder eine ungültige API-Version wurde angegeben. |
| 403                  | `Forbidden`          | Der Aufrufer ist zum Durchführen dieses Vorgangs nicht autorisiert.                      |
| 404                  | `NotFound`           | Abonnement mit der angegebenen ID nicht gefunden                                     |
| 429                  | `RequestThrottleId`  | Der Dienst ist mit der Verarbeitung von Anforderungen ausgelastet, versuchen Sie es später erneut.                     |
| 503                  | `ServiceUnavailable` | Der Dienst ist vorübergehend ausgefallen. Versuchen Sie es später noch mal.                             |
|  |  |  |

*Antwortheader*

| **Headerschlüssel**     | **Erforderlich** | **Beschreibung**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Vom Client empfangene Anforderungs-ID.                                                                   |
| x-ms-correlationid | Ja          | Korrelations-ID, wenn vom Client übergeben, andernfalls ist dies die Serverkorrelations-ID.                   |
| x-ms-activityid    | Ja          | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet. |
| Retry-After        | Nein            | Intervall, in dem der Client den Status überprüfen kann.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS-Webhook

Ein SaaS-Webhook wird verwendet, um Änderungen proaktiv an den SaaS-Dienst zu melden. Diese POST-API muss nicht authentifiziert werden und wird vom Microsoft-Dienst aufgerufen. Der SaaS-Dienst muss die API-Vorgänge zur Überprüfung und Autorisierung aufrufen, bevor er auf die Webhookbenachrichtigung reagieren kann. 

*Text*

``` json
  { 
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "sampleSaaSOffer", // Provided with "Update" action
    "publisherId": "contoso", 
    "planId": "silver",     // Provided with "Update" action
    "action": "Activate", // Activate/Delete/Suspend/Reinstate/Update
    "timeStamp": "2018-12-01T00:00:00"
  }
```

| **Parametername**     | **Datentyp** | **Beschreibung**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | Zeichenfolge       | Eindeutige ID für den ausgelösten Vorgang.                |
| activityId   | Zeichenfolge        | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Dienst. Dieser Wert wird für alle Abstimmungen verwendet.               |
| subscriptionId                     | Zeichenfolge        | ID der SaaS-Abonnementsressource in Azure.    |
| offerId                | Zeichenfolge        | Angebots-ID, die der Benutzer abonniert hat. Nur mit der Aktion „Update“ bereitgestellt.        |
| publisherId                | Zeichenfolge        | Herausgeber-ID des SaaS-Angebots         |
| planId                 | Zeichenfolge        | Tarif-ID, die der Benutzer abonniert hat. Nur mit der Aktion „Update“ bereitgestellt.          |
| action                 | Zeichenfolge        | Die Aktion, die diese Benachrichtigung auslöst. Mögliche Werte: Activate, Delete, Suspend, Reinstate, Update          |
| timeStamp                 | Zeichenfolge        | Zeitstempelwert in UTC, als diese Benachrichtigung ausgelöst wurde.          |
|  |  |  |

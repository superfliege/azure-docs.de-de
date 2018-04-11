---
title: Schützen einer API über OAuth 2.0 mit Azure Active Directory und API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Web-API-Back-End mit Azure Active Directory und API Management schützen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2018
ms.author: apimpm
ms.openlocfilehash: 2c05407d761a8848f9e032aa219960cd7ea6fa93
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-protect-an-api-using-oauth-20-with-azure-active-directory-and-api-management"></a>So schützen Sie eine API über OAuth 2.0 mit Azure Active Directory und API Management

In diesem Handbuch wird gezeigt, wie Sie Ihre APIM-Instanz (API Management) so konfigurieren, dass eine API über das OAuth 2.0-Protokoll mit Azure Active Directory (AAD) geschützt ist. 

## <a name="prerequisite"></a>Voraussetzung
Damit Sie den Schritten in diesem Artikel folgen können, benötigen Sie folgende Komponenten:
* Eine APIM-Instanz
* Eine API, die mit der APIM-Instanz veröffentlicht wird
* Einen Azure AD-Mandanten

## <a name="overview"></a>Übersicht

In diesem Handbuch wird gezeigt, wie Sie eine API über OAuth 2.0 in APIM schützen können. In diesem Artikel wird Azure AD als Autorisierungsserver (OAuth-Server) verwendet. Es folgt eine kurze Übersicht über die Schritte:

1. Registrieren einer Anwendung (Back-End-App) in Azure AD, die die API darstellt
2. Registrieren einer anderen Anwendung (Client-App) in Azure AD, die eine Clientanwendung darstellt, aus der die API aufgerufen werden muss
3. Gewähren von Berechtigungen in Azure AD, damit die Client-App die Back-End-App aufrufen kann
4. Konfigurieren der Entwicklerkonsole, damit in ihr die OAuth 2.0-Benutzerautorisierung verwendet wird
5. Hinzufügen der „validate-jwt“-Richtlinie, um das OAuth-Token für jede eingehende Anforderung zu überprüfen

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Registrieren einer Anwendung in Azure AD, die die API darstellt

Um eine API mit Azure AD zu schützen, besteht der erste Schritt darin, eine Anwendung in Azure AD zu registrieren, die die API darstellt. 

Navigieren Sie zu Ihrem Azure AD-Mandanten, und navigieren Sie dann zu **App-Registrierungen**.

Wählen Sie **Registrierung einer neuen Anwendung** aus. 

Geben Sie den Namen der Anwendung an. In diesem Beispiel wird `backend-app` verwendet.  

Wählen Sie **Web-App/API** als **Anwendungstyp** aus. 

Für **Anmelde-URL** können Sie `https://localhost` als Platzhalter verwenden.

Klicken Sie auf **Erstellen**.

Sobald die Anwendung erstellt ist, notieren Sie sich die **Anwendungs-ID**, um sie in einem späteren Schritt verwenden zu können. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Registrieren einer anderen Anwendung in Azure AD, die eine Clientanwendung darstellt

Jede Clientanwendung, aus der die API aufgerufen werden muss, muss in Azure AD als eine Anwendung registriert werden. In diesem Handbuch wird die Entwicklerkonsole im APIM-Entwicklerportal als Beispielclientanwendung verwendet. 

Es muss eine weitere Anwendung in Azure AD registriert werden, die die Entwicklerkonsole darstellt.

Klicken Sie erneut auf **Registrierung einer neuen Anwendung**. 

Geben Sie den Namen der Anwendung an, und wählen Sie **Web-App/API** als **Anwendungstyp** aus. In diesem Beispiel wird `client-app` verwendet.  

Für **Anmelde-URL** können Sie `https://localhost` als Platzhalter oder die Anmelde-URL Ihrer APIM-Instanz verwenden. In diesem Beispiel wird `https://contoso5.portal.azure-api.net/signin` verwendet.

Klicken Sie auf **Erstellen**.

Sobald die Anwendung erstellt ist, notieren Sie sich die **Anwendungs-ID**, um sie in einem späteren Schritt verwenden zu können. 

Jetzt muss ein geheimer Clientschlüssel für diese Anwendung erstellt werden, der in einem späteren Schritt verwendet wird.

Klicken Sie erneut auf **Einstellungen**, und wechseln Sie zu **Schlüssel**.

Geben Sie unter **Kennwörter** eine **Schlüsselbeschreibung** an, wählen Sie aus, wann der Schlüssel ablaufen soll, und klicken Sie auf **Speichern**.

Notieren Sie sich den Schlüsselwert. 

## <a name="grant-permissions-in-aad"></a>Erteilen von Berechtigungen in AAD

Nachdem nun zwei Anwendungen registriert sind, die die API (d. h. die Back-End-App) und die Entwicklerkonsole (d. h. die Client-App) darstellen, müssen Berechtigungen gewährt werden, damit die Client-App die Back-End-App aufrufen kann.  

Navigieren Sie erneut zu **Anwendungsregistrierung**. 

Klicken Sie auf `client-app`, und wechseln Sie zu **Einstellungen**.

Klicken Sie auf **Erforderliche Berechtigungen**, und klicken Sie dann auf **Hinzufügen**.

Klicken Sie auf **Hiermit wählen Sie eine API aus**, und suchen Sie nach `backend-app`.

Aktivieren Sie `Access backend-app` unter **Delegierte Berechtigungen**. 

Klicken Sie auf **Auswählen**, und klicken Sie dann auf **Fertig**. 

> [!NOTE]
> Wenn **Windows** **Azure Active Directory** unter den Berechtigungen für andere Programme nicht aufgeführt ist, klicken Sie auf **Hinzufügen**, und fügen Sie die Option aus der Liste hinzu.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Aktivieren der OAuth 2.0-Benutzerautorisierung in der Entwicklerkonsole

An diesem Punkt sind die Anwendungen in Azure AD erstellt und die erforderlichen Berechtigungen erteilt, damit die Client-App die Back-End-App aufrufen kann. 

In diesem Handbuch wird die Entwicklerkonsole als Client-App verwendet. In den folgenden Schritten ist beschrieben, wie OAuth 2.0-Benutzerautorisierung in der Entwicklerkonsole aktiviert wird. 

Navigieren zu Ihrer APIM-Instanz.

Klicken Sie auf **OAuth 2.0**, und klicken Sie dann auf **Hinzufügen**.

Geben Sie den **Anzeigenamen** und eine **Beschreibung** an.

Geben Sie als Seiten-URL für Clientregistrierung (**) einen Platzhalterwert ein, z. B. `http://localhost`.  **Seiten-URL für Clientregistrierung** verweist auf die Seite, die Benutzer zum Erstellen und Konfigurieren eigener Konten für OAuth 2.0-Anbieter nutzen können, die eine Benutzerverwaltung für Konten unterstützen. In diesem Beispiel werden von Benutzern keine eigenen Konten erstellt und konfiguriert, daher wird ein Platzhalter verwendet.

Aktivieren Sie **Autorisierungscode** als **Autorisierungszuweisungstypen**.

Geben Sie anschließend die **Autorisierungsendpunkt-URL** und die **Token-Endpunkt-URL** ein.

Diese Werte können Sie von der Seite **Endpunkte** in Ihrem Azure AD-Mandanten abrufen. Um auf die Endpunkte zuzugreifen, navigieren Sie zur Seite **App-Registrierungen**, und klicken Sie erneut auf **Endpunkte**.

Kopieren Sie den **OAuth 2.0-Autorisierungsendpunkt**, und fügen Sie ihn in das Textfeld **URL für Autorisierungsendpunkt** ein.

Kopieren Sie den **OAuth 2.0-Token-Endpunkt**, und fügen Sie ihn in das Textfeld **URL für Tokenendpunkt** ein.

Zusätzlich zum Einfügen des Tokenendpunkts fügen Sie einen Textkörperparameter namens **resource** hinzu, und verwenden Sie als Wert die **Anwendungs-ID** für die Back-End-App.

Geben Sie anschließend Clientanmeldeinformationen an. Diese sind die Anmeldeinformationen für die Client-App.

Verwenden Sie für **Client-ID** die **Anwendungs-ID** für die Client-App.

Verwenden Sie für **Clientgeheimnis** den Schlüssel, den Sie zuvor für die Client-App erstellt haben. 

Unmittelbar hinter dem Clientgeheimnis folgt die Umleitungs-URL (**redirect_url**) für den Typ der Gewährung für Autorisierungscode.

Notieren Sie sich diese URL.

Klicken Sie auf **Erstellen**.

Navigieren Sie zurück zur **Einstellungen**-Seite Ihrer Client-App.

Klicken Sie auf **Antwort-URLs**, und fügen Sie die **redirect_url** in der ersten Zeile hinzu. In diesem Beispiel wurde `https://localhost` durch die URL in der ersten Zeile ersetzt.  

Nachdem nun ein OAuth 2.0-Autorisierungsserver konfiguriert ist, sollte es möglich sein, dass die Entwicklerkonsole Zugriffstoken von Azure AD abruft. 

Der nächste Schritt besteht darin, die OAuth 2.0-Benutzerautorisierung für die API zu aktivieren, damit die Entwicklerkonsole weiß, dass sie im Namen des Benutzers ein Zugriffstoken abrufen muss, bevor Aufrufe an die API vorgenommen werden.

Navigieren Sie zu Ihrer APIM-Instanz, und wechseln Sie zu **APIs**.

Klicken Sie auf die API, die Sie schützen möchten. In diesem Beispiel wird die `Echo API` verwendet.

Wechseln Sie zu **Einstellungen**.

Wählen Sie unter **Sicherheit** die Option **OAuth 2.0** aus, und wählen Sie den OAuth 2.0-Server aus, den Sie zuvor konfiguriert haben. 

Klicken Sie auf **Speichern**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Erfolgreiches Aufrufen der API aus dem Entwicklerportal

Nachdem nun die OAuth 2.0-Benutzerautorisierung für die `Echo API` aktiviert ist, ruft die Entwicklerkonsole im Namen des Benutzers ein Zugriffstoken ab, bevor die API aufgerufen wird.

Navigieren Sie zu irgendeinem Vorgang unter der `Echo API` im Entwicklerportal, und klicken Sie auf **Ausprobieren**, damit Sie zur Entwicklerkonsole gelangen.

Beachten Sie ein neues Element im Abschnitt **Autorisierung**, das dem Autorisierungsserver entspricht, den Sie soeben hinzugefügt haben.

Wählen Sie **Autorisierungscode** in der Dropdownliste „Autorisierung“ aus. Sie werden dann aufgefordert, sich bei dem Azure AD-Mandanten anzumelden. Wenn Sie bereits mit dem Konto angemeldet sind, werden Sie möglicherweise nicht dazu aufgefordert.

Nach der erfolgreichen Anmeldung wird der Anforderung ein `Authorization`-Header mit einem Zugriffstoken von Azure AD hinzugefügt. 

Ein Beispieltoken sieht wie das folgende aus, das Base64-codiert ist.

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
```

Klicken Sie auf **Senden**. Danach sollten Sie die API erfolgreich aufrufen können.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfigurieren einer JWT-Überprüfungsrichtlinie zur Vorautorisierung von Anforderungen

Wenn ein Benutzer an diesem Punkt versucht, aus der Entwicklerkonsole einen Aufruf auszuführen, wird er aufgefordert, sich anzumelden, und die Entwicklerkonsole erhält im Namen des Benutzers ein Zugriffstoken. Alles funktioniert erwartungsgemäß. Was aber passiert, wenn jemand die API ohne Token oder mit einem ungültigen Token aufruft? Sie können beispielsweise den `Authorization`-Header löschen und werden feststellen, dass Sie die API weiterhin aufrufen können. Dies liegt daran, dass APIM das Zugriffstoken an diesem Punkt nicht überprüft. APIM übergibt den `Auhtorization`-Header einfach an die Back-End-API.

Sie können die Richtlinie [JWT überprüfen](api-management-access-restriction-policies.md#ValidateJWT) verwenden, um Anforderungen in APIM vorab zu autorisieren, indem die Zugriffstoken für jede eingehende Anforderung überprüft werden. Wenn eine Anforderung kein gültiges Token hat, wird sie von API Management blockiert, und sie wird nicht an das Back-End übergeben. Wir können z.B. die folgende Richtlinie zum Richtlinienabschnitt `<inbound>` von `Echo API` hinzufügen. Sie überprüft den „Audience“-Anspruch in einem Zugriffstoken und gibt eine Fehlermeldung zurück, wenn das Token nicht gültig ist. Informationen zum Konfigurieren von Richtlinien finden Sie unter [How to set or edit Azure API Management policies](set-edit-policies.md) (Festlegen oder Bearbeiten von Azure API Management-Richtlinien).

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## <a name="build-an-application-to-call-the-api"></a>Erstellen einer Anwendung zum Aufrufen der API

In diesem Handbuch wird die Entwicklerkonsole in APIM als Beispielclientanwendung verwendet, um die durch OAuth 2.0 geschützte `Echo API` aufzurufen. Weitere Informationen zum Erstellen einer Anwendung und zum Implementieren des OAuth 2.0-Datenflusses finden Sie unter [Azure Active Directory-Codebeispiele](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Azure Active Directory und OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md)
* Hier finden Sie weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) zu API Management.
* Weitere Methoden zum Sichern Ihres Back-End-Diensts finden Sie unter [Gegenseitige Zertifikatauthentifizierung](api-management-howto-mutual-certificates.md).

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md

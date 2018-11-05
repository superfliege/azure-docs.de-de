---
title: Grundlegendes zur Azure Digital Twins-API-Authentifizierung | Microsoft-Dokumentation
description: Verwenden von Azure Digital Twins, um die Verbindungsherstellung und Authentifizierung für APIs durchzuführen
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: 414a52141aac05aac51bbb4fedf6ab67d98f2637
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092269"
---
# <a name="connect-and-authenticate-to-apis"></a>Durchführen der Verbindungsherstellung und Authentifizierung für APIs

Für Azure Digital Twins wird Azure Active Directory (Azure AD) verwendet, um Benutzer zu authentifizieren und Anwendungen zu schützen. Azure AD unterstützt die Authentifizierung für eine Vielzahl moderner Architekturen, die alle auf den branchenüblichen Standardprotokollen OAuth 2.0 oder OpenID Connect basieren. Zusätzlich ermöglicht Azure AD es Entwicklern, sowohl Branchenanwendungen mit einem einzelnen Mandanten zu erstellen als auch mehrinstanzenfähige Anwendungen zu entwickeln.

Eine Übersicht über Azure AD finden Sie auf der Seite mit der [Dokumentation mit grundlegenden Informationen zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/index), z.B. Schritt-für-Schritt-Anleitungen, Konzepte und Schnellstartanleitungen.

Um eine Anwendung oder einen Dienst in Azure AD zu integrieren, muss ein Entwickler die Anwendung zuerst bei Azure AD registrieren. Eine ausführliche Anleitung und Screenshots finden Sie in [dieser Anleitung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Azure AD unterstützt die folgenden [fünf Hauptanwendungsszenarien](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types):

* Single-Page-Webanwendung (SPA): Ein Benutzer muss sich bei einer durch Azure AD geschützten Single-Page-Webanwendung anmelden.
* Webbrowser zu Webanwendung: Ein Benutzer muss sich bei einer durch Azure AD geschützten Webanwendung anmelden.
* Native Anwendung zu Web-API: Eine native Anwendung auf einem Smartphone, Tablet oder PC muss einen Benutzer authentifizieren, um Ressourcen von einer durch Azure AD geschützten Web-API abzurufen.
* Webanwendung zu Web-API: Eine Webanwendung muss Ressourcen von einer durch Azure AD geschützten Web-API abrufen.
* Daemon- oder Serveranwendung zu Web-API: Eine Daemon- oder Serveranwendung ohne Webbenutzeroberfläche muss Ressourcen von einer durch Azure AD geschützten Web-API abrufen.

Die Microsoft Azure-Authentifizierungsbibliothek bietet viele Möglichkeiten zum Beziehen von Active Directory-Token. Ausführliche Informationen zu dieser Bibliothek und Codebeispiele finden Sie [hier](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="calling-digital-twins-from-a-middle-tier-web-api"></a>Aufrufen von Digital Twins über eine Web-API der mittleren Ebene

Beim Entwickeln der Architektur von Digital Twins-Lösungen entscheiden sich Entwickler häufig für die Erstellung einer Anwendung oder API der mittleren Ebene, die im weiteren Verlauf dann die Digital Twins-API aufruft. Benutzer führen hierbei zuerst die Authentifizierung für die Anwendung der mittleren Ebene durch, und anschließend wird beim Aufrufen dann ein „Im Auftrag von“-Tokenablauf genutzt. Eine ausführliche Anleitung zur Orchestrierung des „Im Auftrag von“-Ablaufs finden Sie auf [dieser Seite](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Sie können auch [hier](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/) Codebeispiele anzeigen.


## <a name="test-with-the-postman-client"></a>Durchführen von Tests mit dem Postman-Client

Für den Einstieg in Digital Twins-APIs können Sie beispielsweise einen Client wie Postman als API-Umgebung nutzen. Mit Postman können Sie schnell komplexe HTTP-Anforderungen erstellen. In der Anleitung unten geht es um das Beziehen eines Azure AD-Tokens, das zum Aufrufen von Digital Twins über die Postman-Benutzeroberfläche benötigt wird.


1. Navigieren Sie zu https://www.getpostman.com/, um die App herunterzuladen.
1. Führen Sie die [hier angegebenen](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) Schritte aus, um eine Azure Active Directory-Anwendung zu erstellen. (Sie können auch eine vorhandene Registrierung wiederverwenden.) 
1. Fügen Sie unter „Erforderliche Berechtigungen“ den Eintrag „Azure Digital Twins“ hinzu, und wählen Sie „Delegierte Berechtigungen“. Vergessen Sie nicht, abschließend auf „Berechtigungen erteilen“ zu klicken.
1. Öffnen Sie das Anwendungsmanifest, und legen Sie „oauth2AllowImplicitFlow“ auf „true“ fest.
1. Konfigurieren Sie eine Antwort-URL für [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback).
1. Wählen Sie die Registerkarte **Authorization** (Autorisierung), klicken Sie auf **OAuth 2.0**, und wählen Sie **Get New Access Token** (Neues Zugriffstoken abrufen).

    |**Feld**  |**Wert** |
    |---------|---------|
    | Gewährungstyp | Implicit (Implizit) |
    | Rückruf-URL | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Authentifizierungs-URL | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0 |
    | Client-ID | Verwenden Sie die Anwendungs-ID für die Azure AD-App aus Schritt 1, die erstellt bzw. anderweitig eingesetzt wurde. |
    | Bereich | Lassen Sie dieses Feld leer. |
    | Zustand | Lassen Sie dieses Feld leer. |
    | Clientauthentifizierung | Als Standardauthentifizierungsheader senden |

1. Klicken Sie auf **Request Token** (Token anfordern).

    >[!NOTE]
    >Versuchen Sie Folgendes, wenn Sie eine Fehlermeldung wie „OAuth2 couldn’t be completed“ (OAuth2 konnte nicht abgeschlossen werden) erhalten:
    > * Schließen Sie Postman, und öffnen Sie das Tool dann erneut, um den Vorgang zu wiederholen.
   
1. Scrollen Sie nach unten, und klicken Sie auf **Use Token** (Token verwenden).

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Sicherheit von Azure Digital Twins finden unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).

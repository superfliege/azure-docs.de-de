---
title: Grundlegendes zur Azure Digital Twins-API-Authentifizierung | Microsoft-Dokumentation
description: Durchführen der Verbindungsherstellung und Authentifizierung für APIs mithilfe von Azure Digital Twins
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: f85ab05e785ea559962490b43e75b196d1602159
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016215"
---
# <a name="connect-and-authenticate-to-apis"></a>Durchführen der Verbindungsherstellung und Authentifizierung für APIs

Für Azure Digital Twins wird Azure Active Directory (Azure AD) verwendet, um Benutzer zu authentifizieren und Anwendungen zu schützen. Azure AD unterstützt die Authentifizierung für eine Vielzahl moderner Architekturen. Diese basieren alle auf den branchenüblichen Protokollen OAuth 2.0 oder OpenID Connect. Darüber hinaus können Entwickler Azure AD dazu verwenden, Anwendungen mit einem Mandanten sowie Branchenanwendungen zu erstellen. Entwickler können Azure AD auch für die Entwicklung mehrinstanzenfähiger Anwendungen nutzen.

Die Seite mit der [Dokumentation mit grundlegenden Informationen zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/index) enthält eine Übersicht über Azure AD. Dort finden Sie beispielsweise Schritt-für-Schritt-Anleitungen, Informationen zu Konzepten sowie Schnellstartanleitungen.

Um eine Anwendung oder einen Dienst in Azure AD zu integrieren, muss ein Entwickler die Anwendung zuerst bei Azure AD registrieren. Ausführliche Anweisungen und Screenshots finden Sie in [dieser Schnellstartanleitung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Azure AD unterstützt die folgenden [fünf Hauptanwendungsszenarien](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types):

* Single-Page-Webanwendung (SPA): Ein Benutzer muss sich bei einer durch Azure AD geschützten Single-Page-Webanwendung anmelden.
* Webbrowser zu Webanwendung: Ein Benutzer muss sich bei einer durch Azure AD geschützten Webanwendung anmelden.
* Native Anwendung zu Web-API: Eine native Anwendung auf einem Smartphone, Tablet oder PC muss einen Benutzer authentifizieren, um Ressourcen von einer durch Azure AD geschützten Web-API abzurufen.
* Webanwendung zu Web-API: Eine Webanwendung muss Ressourcen von einer durch Azure AD geschützten Web-API abrufen.
* Daemon- oder Serveranwendung zu Web-API: Eine Daemon- oder Serveranwendung ohne Webbenutzeroberfläche muss Ressourcen von einer durch Azure AD geschützten Web-API abrufen.

Die Microsoft Azure-Authentifizierungsbibliothek bietet viele Möglichkeiten zum Beziehen von Active Directory-Token. Ausführliche Informationen zur Bibliothek und zu den Codebeispielen finden Sie in [diesem Artikel](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Aufrufen von Digital Twins über eine Web-API der mittleren Ebene

Wenn Entwickler Digital Twins-Lösungen entwerfen, erstellen Sie in der Regel eine Anwendung oder API der mittleren Ebene. Die App oder API ruft dann im weiteren Verlauf die Digital Twins-API auf. Benutzer authentifizieren sich hierbei zuerst bei der Anwendung der mittleren Ebene, und anschließend wird beim Aufrufen dann ein Tokenfluss vom Typ „Im Auftrag von“ genutzt. Eine Anleitung zur Orchestrierung des Flusses vom Typ „Im Auftrag von“ finden Sie auf [dieser Seite](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Sie können sich auch die Codebeispiele auf [dieser Seite](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/) ansehen.


## <a name="test-with-the-postman-client"></a>Durchführen von Tests mit dem Postman-Client

Für den Einstieg in Digital Twins-APIs können Sie beispielsweise einen Client wie Postman als API-Umgebung nutzen. Mit Postman können Sie schnell komplexe HTTP-Anforderungen erstellen. In den folgenden Schritten wird gezeigt, wie Sie ein Azure AD-Token abrufen, das zum Aufrufen von Digital Twins über die Postman-Benutzeroberfläche benötigt wird.


1. Navigieren Sie zu https://www.getpostman.com/, um die App herunterzuladen.
1. Führen Sie die Schritte [dieser Schnellstartanleitung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) aus, um eine Azure AD-Anwendung zu erstellen. Alternativ können Sie eine vorhandene Registrierung wiederverwenden. 
1. Geben Sie unter **Erforderliche Berechtigungen** den Namen „Azure Digital Twins“ ein, und wählen Sie **Delegierte Berechtigungen** aus. Wählen Sie dann **Berechtigungen erteilen** aus.
1. Öffnen Sie das Anwendungsmanifest, und legen Sie **oauth2AllowImplicitFlow** auf „true“ fest.
1. Konfigurieren Sie eine Antwort-URL für [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback).
1. Wählen Sie die Registerkarte **Authorization** (Autorisierung) und anschließend **OAuth 2.0** und **Get New Access Token** (Neues Zugriffstoken abrufen) aus.

    |**Feld**  |**Wert** |
    |---------|---------|
    | Gewährungstyp | Implicit (Implizit) |
    | Rückruf-URL | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Authentifizierungs-URL | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0 |
    | Client-ID | Verwenden Sie die Anwendungs-ID für die Azure AD-App aus Schritt 2, die erstellt bzw. anderweitig eingesetzt wurde. |
    | Bereich | Lassen Sie dieses Feld leer. |
    | Zustand | Lassen Sie dieses Feld leer. |
    | Clientauthentifizierung | Als Standardauthentifizierungsheader senden |

1. Wählen Sie **Request Token** (Token anfordern) aus.

    >[!NOTE]
    >Versuchen Sie Folgendes, wenn Sie die Fehlermeldung „OAuth2 couldn’t be completed“ (OAuth2 konnte nicht abgeschlossen werden) erhalten:
    > * Schließen Sie Postman, und öffnen Sie das Tool dann erneut, um den Vorgang zu wiederholen.
   
1. Scrollen Sie nach unten, und wählen Sie **Use Token** (Token verwenden) aus.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Sicherheit von Azure Digital Twins finden unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).

---
title: "Azure AD-v2-Android-erste Schritte – Einführung | Microsoft Docs"
description: "Wie eine Android-app ein Zugriffstoken abrufen und Aufrufen von Microsoft Graph-API oder APIs, Zugriffstoken aus Azure Active Directory-v2-Endpunkt erfordern, können"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Rufen Sie die Microsoft Graph-API aus einer Android-app

Dieses Handbuch veranschaulicht, wie eine systemeigene Android-Anwendung ein Zugriffstoken abrufen und Microsoft Graph-API oder andere APIs, Zugriffstoken aus Azure Active Directory-v2-Endpunkt erfordern, aufrufen kann.

Am Ende dieses Handbuchs wird Ihre Anwendung Lage rufen Sie eine geschützte API, die Persönliche Konten (z. B. outlook.com, live.com usw.) sowie Arbeiten mit und schulkonten von Unternehmen oder Organisation, die über Azure Active Directory verfügt.  

### <a name="how-this-sample-works"></a>Wie funktioniert dieses Beispiel
![Wie funktioniert dieses Beispiel](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Das Beispiel erstellt haben, wird in diesem Handbuch basiert auf einem Szenario, an eine Android-Anwendung verwendet wird, um eine Web-API abzufragen, die Token von Azure Active Directory v2-Endpunkt – in diesem Fall Microsoft Graph-API akzeptiert. In diesem Szenario wird der HTTP-Anforderungen über den Autorisierungsheader ein Token hinzugefügt. Tokenabruf und-Erneuerung wird von Microsoft Authentication Library (MSAL) behandelt.

### <a name="pre-requisites"></a>Voraussetzungen
* Diese schrittweise Anleitung konzentriert sich auf Android Studio andere Android-Anwendung-Entwicklungsumgebung ist jedoch ebenfalls akzeptabel. 
* Android SDK 21 oder höher ist erforderlich (25 SDK wird empfohlen).
* Google Chrome oder eines Webbrowsers unter Verwendung von benutzerdefinierten Registerkarten ist für diese Version von Microsoft Authentication Library (MSAL) für Android erforderlich.

> Hinweis: Google Chrome ist nicht für Visual Studio-Emulator für Android enthalten. Wir empfehlen Ihnen, diesen Code in einem Emulator mit API-25 oder einem Bild mit API 21 oder höher zu testen, die mit Google Chrome installiert wurde.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Behandeln von tokenabruf für den Zugriff auf eine geschützte Web-API

Nachdem der Benutzer authentifiziert hat, empfängt die beispielanwendung ein Token, das zum Abfragen von Microsoft Graph-API oder eine Web-API von Microsoft Azure Active Directory v2 gesichert verwendet werden kann.

APIs, wie z. B. Microsoft Graph erfordern ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen – z. B. zum Lesen eines Benutzerprofils Kalender des Zugriffsbenutzers oder senden Sie eine e-Mail zu ermöglichen. Ihre Anwendung kann ein Zugriffstoken mithilfe von MSAL auf diese Ressourcen zugreifen, durch Angeben von API-Bereichen anfordern. Dieses Zugriffstoken wird dann der HTTP-Authorization-Header für jeden Aufruf für die geschützte Ressource hinzugefügt. 

MSAL verwaltet Zwischenspeichern und Zugriffstoken, aktualisieren, damit Ihre Anwendung nicht um benötigt.

### <a name="libraries"></a>Bibliotheken

Dieses Handbuch verwendet die folgenden Bibliotheken:

|-Bibliothek|Beschreibung|
|---|---|
|[com.Microsoft.Identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft-Authentifizierungsbibliothek (MSAL)|

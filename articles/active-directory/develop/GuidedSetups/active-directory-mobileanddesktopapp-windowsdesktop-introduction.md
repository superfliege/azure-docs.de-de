---
title: "Azure AD v2 Windows Desktop erste Schritte – Einführung | Microsoft Docs"
description: "Wie Windows Desktop .NET (XAML)-Anwendungen eine API, die erfordern Zugriffstoken von Azure Active Directory-v2-Endpunkt aufrufen können"
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
ms.openlocfilehash: 4a695c00fce4deb02261ba58ec95469746bb1486
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Rufen Sie die Microsoft Graph-API aus einer Windows-Desktop-app

Dieses Handbuch veranschaulicht, wie eine systemeigene Windows-Desktop-.NET (XAML)-Anwendung ein Zugriffstoken abrufen und Microsoft Graph-API oder andere APIs, Zugriffstoken aus Azure Active Directory-v2-Endpunkt erfordern, aufrufen kann.

Am Ende dieses Handbuchs wird Ihre Anwendung Lage rufen Sie eine geschützte API, die Persönliche Konten (z. B. outlook.com, live.com usw.) sowie Arbeiten mit und schulkonten von Unternehmen oder Organisation, die über Azure Active Directory verfügt.  

> Dieses Handbuch ist Visual Studio 2015 Update 3 oder Visual Studio 2017 erforderlich.  Haben sie kein? [Visual Studio-2017 kostenlos herunterladen](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Funktionsweise dieses Handbuchs

![Funktionsweise dieses Handbuchs](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

Die beispielanwendung, die erstellt wird, wird in diesem Handbuch kann einer Windows-Desktop-Anwendung zum Abfragen von Microsoft Graph-API oder eine Web-API, die Token von Azure Active Directory-v2-Endpunkt akzeptiert. In diesem Szenario wird der HTTP-Anforderungen über den Autorisierungsheader ein Token hinzugefügt. Tokenabruf und-Erneuerung wird von Microsoft Authentication Library (MSAL) behandelt.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Behandlung von tokenabruf für den Zugriff auf Web-APIs geschützt

Nachdem der Benutzer authentifiziert hat, empfängt die beispielanwendung ein Token, das zum Abfragen von Microsoft Graph-API oder eine Web-API von Microsoft Azure Active Directory v2 gesichert verwendet werden kann.

APIs, wie z. B. Microsoft Graph erfordern ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen – z. B. zum Lesen eines Benutzerprofils Kalender des Zugriffsbenutzers oder senden Sie eine e-Mail zu ermöglichen. Ihre Anwendung kann ein Zugriffstoken mithilfe von MSAL auf diese Ressourcen zugreifen, durch Angeben von API-Bereichen anfordern. Dieses Zugriffstoken wird dann der HTTP-Authorization-Header für jeden Aufruf für die geschützte Ressource hinzugefügt. 

MSAL verwaltet Zwischenspeichern und Zugriffstoken, aktualisieren, damit Ihre Anwendung nicht um benötigt.


### <a name="nuget-packages"></a>NuGet-Pakete

Dieses Handbuch verwendet die folgenden NuGet-Pakete:

|-Bibliothek|Beschreibung|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft-Authentifizierungsbibliothek (MSAL)|


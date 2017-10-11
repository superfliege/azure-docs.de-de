---
title: "Azure AD-v2-ASP.NET Web-Server-Erste Schritte – Einführung | Microsoft Docs"
description: "Implementieren von Microsoft anmelden in einer ASP.NET-Projektmappe mit einer herkömmlichen Web browserbasierte Anwendung, die mit standardmäßigen OpenID Connect"
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
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Melden Sie sich mit Microsoft in einer ASP.NET Web-app hinzufügen

Dieses Handbuch veranschaulicht das Implementieren-in für Microsoft ASP.NET MVC-Projektmappe mit einer herkömmlichen browserbasierten Webanwendung mithilfe der OpenID Connect verwenden. 

Am Ende dieses Handbuchs werden Ihre Anwendung können akzeptieren Anmeldung ins Persönliche Konten (z. B. outlook.com, live.com usw.) als auch funktionieren und schulkonten von Unternehmen oder Organisation, die in Azure Active Directory integriert ist. 

> Dieses Handbuch ist Visual Studio 2015 Update 3 oder Visual Studio 2017 erforderlich.  Haben sie kein?  [Visual Studio-2017 kostenlos herunterladen](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Funktionsweise dieses Handbuchs

![Funktionsweise dieses Handbuchs](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Dieses Handbuch basiert auf dem Szenario, in ein Browser eine ASP.NET-Website greift auf, einen Benutzer über eine Schaltfläche "Anmelden" Authentifizierung anfordern. In diesem Szenario tritt Großteil der Arbeit auf der Webseite Rendern auf dem Server.

## <a name="libraries"></a>Bibliotheken

Dieses Handbuch verwendet die folgenden Bibliotheken:

|-Bibliothek|Beschreibung|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware, die Anwendung für die Verwendung von OpenIdConnect für die Authentifizierung ermöglicht.|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware, die eine Anwendung benutzersitzung mithilfe von Cookies beibehalten werden kann|
|["Microsoft.owin.Host.systemweb"](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|OWIN-basierte Anwendungen zur Ausführung unter IIS mithilfe der Anforderungspipeline ASP.NET ermöglicht|


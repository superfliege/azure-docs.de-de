---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 79b6b8e5d81d3885f9c125f971f3e32e695cf2b1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32200610"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Aufrufen der Microsoft Graph-API aus einer Windows Desktop-App

Diese Anleitung veranschaulicht, wie eine native Windows Desktop .NET-Anwendung (XAML) ein Zugriffstoken abrufen und die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt erfordern.

Am Ende dieses Leitfadens kann Ihre Anwendung eine geschützte API aufrufen, die persönliche Konten (outlook.com, live.com und andere) verwendet. Die Anwendung kann auch Geschäfts-, Schul- und Unikonten aus Unternehmen oder Organisationen nutzen, die Azure Active Directory verwenden.  

> [!NOTE] 
> Für diesen Leitfaden wird Visual Studio 2015 Update 3 oder Visual Studio 2017 vorausgesetzt.  Sie verfügen über keine dieser Versionen? Visual Studio 2017 können Sie [hier](https://www.visualstudio.com/downloads/) kostenlos herunterladen.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funktionsweise der über diesen Leitfaden generierten Beispiel-App

![Funktionsweise dieser Anleitung](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Die in dieser Anleitung erstellte Beispielanwendung ermöglicht einer Windows Desktop-Anwendung das Abfragen der Microsoft Graph-API oder einer Web-API, die Token von einem Azure Active Directory-v2-Endpunkt akzeptiert. In diesem Szenario fügen Sie HTTP-Anforderungen ein Token über den Autorisierungsheader hinzu. Tokenabruf und -erneuerung werden von der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) gehandhabt.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Verarbeiten des Beziehens von Token für den Zugriff auf geschützte Web-APIs

Nach der Authentifizierung des Benutzers empfängt die Beispielanwendung ein Token, mit dem eine durch Azure Active Directory v2 geschützte Microsoft Graph-API oder Web-API abgefragt werden kann.

APIs wie Microsoft Graph erfordern ein Token, um den Zugriff auf bestimmte Ressourcen zu ermöglichen. Ein Token ist beispielsweise erforderlich, um das Profil eines Benutzers zu lesen, auf den Kalender eines Benutzers zuzugreifen oder eine E-Mail zu senden. Ihre Anwendung kann unter Verwendung von MSAL ein Zugriffstoken anfordern, um auf diese Ressourcen durch Angeben von API-Bereichen zuzugreifen. Dieses Zugriffstoken wird dann dem HTTP-Autorisierungsheader für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt. 

MSAL nimmt Ihrer Anwendung die Verwaltung der Zwischenspeicherung und Aktualisierung von Zugriffstoken ab.

## <a name="nuget-packages"></a>NuGet-Pakete

In dieser Anleitung werden die folgenden NuGet-Pakete verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|


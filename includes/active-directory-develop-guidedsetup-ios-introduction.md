---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 8c7f11d74d0a0b81f9f0c40871b2eaa3eb25f51f
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988261"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Aufrufen der Microsoft Graph-API aus einer iOS-Anwendung

Dieser Leitfaden zeigt, wie eine native iOS-Anwendung (Swift) APIs aufrufen kann, die Zugriffstoken vom Azure AD v2.0-Endpunkt (Microsoft Azure Active Directory) benötigen. Der Leitfaden erläutert, wie Sie Zugriffstoken abrufen und in Aufrufen der Microsoft Graph-API und anderer APIs verwenden können.

Nachdem Sie die Übungen in diesem Leitfaden abgeschlossen haben, kann Ihre Anwendung eine geschützte API von jedem Unternehmen oder jeder Organisation aufrufen, die über Azure AD verfügt. Ihre Anwendung kann Aufrufe geschützter APIs ausführen, indem sie persönliche Konten wie outlook.com, live.com oder andere sowie Geschäfts-, Schul- oder Unikonten verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Für das Beispiel, das in diesem Leitfaden erstellt wird, ist XCode Version 10.x erforderlich. Können Sie XCode von der [iTunes-Website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode-Download-URL") herunterladen.
- Der [Carthage](https://github.com/Carthage/Carthage)-Abhängigkeits-Manager ist für die Paketverwaltung erforderlich.

## <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

Die Beispielanwendung in diesem Leitfaden ermöglicht einer iOS-Anwendung das Abfragen der Microsoft Graph-API oder Web-API, die Token von einem Azure AD v2.0-Endpunkt akzeptiert. In diesem Szenario wird ein Token über den **Autorisierungsheader** HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Verarbeiten des Abrufs von Token für den Zugriff auf geschützte Web-APIs

Nachdem sich der Benutzer authentifiziert hat, empfängt die Beispielanwendung ein Token. Das Token wird zum Abfragen der Microsoft Graph-API oder einer Web-API verwendet, die durch den Azure AD v2. 0-Endpunkt gesichert ist.

APIs wie Microsoft Graph benötigen ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen. Token sind erforderlich, um das Profil eines Benutzers zu lesen, auf den Kalender eines Benutzers zuzugreifen, eine E-Mail zu senden usw. Ihre Anwendung kann ein Zugriffstoken anfordern, indem sie MSAL verwendet und API-Bereiche angibt. Dieses Zugriffstoken wird dem HTTP-**Autorisierungsheader** für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt.

Die MSAL verwaltet das Zwischenspeichern und Aktualisieren von Zugriffstoken, damit Ihre Anwendung dies nicht übernehmen muss.

## <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft Authentifizierungsbibliotheksvorschau für iOS|

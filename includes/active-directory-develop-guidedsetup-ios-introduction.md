---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 03/20/2019
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 971ae8cd44f1b345d3a71b8fa4f256c8f25ef961
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59503073"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Aufrufen der Microsoft Graph-API aus einer iOS-Anwendung

In diesem Leitfaden wird gezeigt, wie eine native iOS-Anwendung (Swift) APIs aufrufen kann, für die Zugriffstoken vom Microsoft Identity Platform-Endpunkt benötigt werden. Der Leitfaden erläutert, wie Sie Zugriffstoken abrufen und in Aufrufen der Microsoft Graph-API und anderer APIs verwenden können.

Nachdem Sie die Übungen in diesem Leitfaden abgeschlossen haben, kann Ihre Anwendung eine geschützte API von jedem Unternehmen oder jeder Organisation aufrufen, die über Azure AD verfügt. Ihre Anwendung kann Aufrufe geschützter APIs ausführen, indem sie persönliche Konten wie outlook.com, live.com oder andere sowie Geschäfts-, Schul- oder Unikonten verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Für das Beispiel, das in diesem Leitfaden erstellt wird, ist XCode Version 10.x erforderlich. Können Sie XCode von der [iTunes-Website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode-Download-URL") herunterladen.
- Der [Carthage](https://github.com/Carthage/Carthage)-Abhängigkeits-Manager ist für die Paketverwaltung erforderlich.

## <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Zeigt, wie die in diesen Tutorials generierte Beispiel-App funktioniert](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Die Beispielanwendung in diesem Leitfaden ermöglicht einer iOS-Anwendung das Abfragen der Microsoft Graph-API oder Web-API, die Token von einem Microsoft Identity Platform-Endpunkt akzeptiert. In diesem Szenario wird ein Token über den **Autorisierungsheader** HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Verarbeiten des Abrufs von Token für den Zugriff auf geschützte Web-APIs

Nachdem sich der Benutzer authentifiziert hat, empfängt die Beispielanwendung ein Token. Das Token wird zum Abfragen der Microsoft Graph-API oder einer Web-API verwendet, die durch den Microsoft Identity Platform-Endpunkt geschützt ist.

APIs wie Microsoft Graph benötigen ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen. Token sind erforderlich, um das Profil eines Benutzers zu lesen, auf den Kalender eines Benutzers zuzugreifen, eine E-Mail zu senden usw. Ihre Anwendung kann ein Zugriffstoken anfordern, indem sie MSAL verwendet und API-Bereiche angibt. Dieses Zugriffstoken wird dem HTTP-**Autorisierungsheader** für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt.

Die MSAL verwaltet das Zwischenspeichern und Aktualisieren von Zugriffstoken, damit Ihre Anwendung dies nicht übernehmen muss.

## <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft Authentifizierungsbibliotheksvorschau für iOS|

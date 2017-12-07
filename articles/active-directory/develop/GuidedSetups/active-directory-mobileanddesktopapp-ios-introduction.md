---
title: "Erste Schritte in Azure AD v2 iOS – Einführung | Microsoft-Dokumentation"
description: "Informationen, wie iOS (Swift)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 5fcd78eaa0c5b09b70aa973466a34556fff56071
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="call-the-microsoft-graph-api-from-an-ios-app"></a>Aufrufen der Microsoft Graph-API aus einer iOS-App

Diese Anleitung veranschaulicht, wie eine native iOS-Anwendung (Swift) ein Zugriffstoken abrufen, und die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt anfordern.

Am Ende dieser Anleitung kann Ihre Anwendung eine geschützte API sowohl mit persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen aufrufen, die mit Azure Active Directory arbeiten.

> ### <a name="pre-requisites"></a>Voraussetzungen
> - Für dieses Handbuch ist XCode 8.x erforderlich. Sie können XCode [hier](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode-Download-URL") herunterladen.
> - [Carthage](https://github.com/Carthage/Carthage) für Paketverwaltung

### <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

Die in dieser Anleitung erstellte Beispielanwendung ermöglicht einer iOS-Anwendung das Abfragen der Microsoft Graph-API oder Web-API, die Token von einem Azure Active Directory-v2-Endpunkt akzeptiert. In diesem Szenario wird ein Token über den Autorisierungsheader HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Verarbeiten des Beziehens von Token für den Zugriff auf geschützte Web-APIs

Nach der Authentifizierung des Benutzers empfängt die Beispielanwendung ein Token, mit dem die Microsoft Graph-API oder Web-API abgefragt werden kann, die von Microsoft Azure Active Directory v2 geschützt wird.

APIs wie Microsoft Graph erfordern ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen, z.B. zum Lesen eines Benutzerprofils, Zugreifen auf den Kalender eines Benutzers oder Senden einer E-Mail. Ihre Anwendung kann durch Angeben von API-Bereichen ein Zugriffstoken mithilfe der MSAL anfordern. Dieses Zugriffstoken wird dann dem HTTP-Autorisierungsheader für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt.

Die MSAL verwaltet das Zwischenspeichern und Aktualisieren von Zugriffstoken, damit Ihre Anwendung dies nicht übernehmen muss.


### <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|Beschreibung|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft Authentifizierungsbibliotheksvorschau für iOS|


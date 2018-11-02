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
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 9bc8f30d2bbf6a084ad680306da9b1e330d488e3
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988333"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Anmelden von Benutzern und Aufrufen von Microsoft Graph aus einer Android-App

In diesem Tutorial wird beschrieben, wie Sie eine Android-Anwendung erstellen und diese in die Microsoft Identity Platform integrieren. Mit dieser App wird ein Benutzer angemeldet, ein Zugriffstoken zum Aufrufen der Microsoft Graph-API abgerufen und eine einfache Anforderung an die Microsoft Graph-API gesendet.  

Am Ende dieses Leitfadens akzeptiert Ihre Anwendung Anmeldungen von persönlichen Microsoft-Konten (z.B. outlook.com, live.com u.a.) sowie von Geschäfts,- Schul- oder Unikonten von allen Unternehmen oder Organisationen, die Azure Active Directory nutzen.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funktionsweise der über diesen Leitfaden generierten Beispiel-App

![Funktionsweise dieses Beispiels](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

Mit der App in diesem Beispiel werden Benutzer angemeldet und Daten in deren Namen abgerufen.  Auf diese Daten wird über eine Remote-API (hier: Microsoft Graph-API) zugegriffen, für die eine Autorisierung erforderlich ist und die über die Microsoft Identity Platform geschützt ist.

Dies gilt insbesondere in folgenden Fällen:

* Ihre App startet eine Webseite für die Anmeldung des Benutzers.
* Ihre App stellt ein Zugriffstoken für die Microsoft Graph-API aus.
* Das Zugriffstoken ist in der HTTP-Anforderung an die Web-API enthalten.
* Die Microsoft Graph-Antwort wird verarbeitet.

In diesem Beispiel wird die Microsoft Authentication Library (MSAL) für Android genutzt, um die Authentifizierung zu koordinieren und als Hilfe dafür zu dienen. Mit MSAL werden Token automatisch verlängert, das einmalige Anmelden zwischen anderen Apps auf dem Gerät wird bereitgestellt, Hilfe bei der Kontoverwaltung wird geleistet, und die meisten Fälle mit bedingtem Zugriff können verarbeitet werden.

## <a name="prerequisites"></a>Voraussetzungen

* In diesem angeleiteten Setup wird Android Studio 3.0 verwendet.
* Android 21 oder höher ist erforderlich (25+ empfohlen).
* Google Chrome oder ein Webbrowser mit benutzerdefinierten Registerkarten ist für diese Version der MSAL für Android erforderlich.

## <a name="library"></a>Bibliothek

In dieser Anleitung wird die folgende Authentifizierungsbibliothek verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|

---
title: Übersicht über das Azure AD .NET-Protokoll | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mit Azure AD verwenden.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: d97aa34f8e76a1ea49841f26534b6a5403e6b5d7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125344"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registrieren der Anwendung beim AD-Mandanten
Zuerst müssen Sie Ihre Anwendung beim Azure Active Directory-Mandanten (Azure AD) registrieren. Hierbei erhalten Sie eine Anwendungs-ID für die Anwendung, und die Aktivierung für den Empfang von Token wird durchgeführt.

* Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
* Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie auf der Seite oben rechts auf Ihr Konto klicken. Klicken Sie anschließend auf das Navigationselement **Verzeichnis wechseln**, und wählen Sie dann den entsprechenden Mandanten aus. 
  * Überspringen Sie diesen Schritt, wenn Ihr Konto nur einen Azure AD-Mandanten enthält oder wenn Sie bereits den entsprechenden Azure AD-Mandanten ausgewählt haben.
* Klicken Sie im linken Navigationsbereich auf **Azure Active Directory**.
* Klicken Sie auf **App-Registrierungen** und dann auf **Neue Awendungsregistrierung**.
* Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Es spielt bei diesem Tutorial keine Rolle, ob es sich um eine Webanwendung oder eine native Anwendung handelt. Falls Sie spezielle Beispiele für Webanwendungen oder native Anwendungen wünschen, können Sie sich unsere [Schnellstarts](../articles/active-directory/develop/v1-overview.md) ansehen.
  * Geben Sie für Webanwendungen die **Anmelde-URL** an. Dies ist die Basis-URL Ihrer App, unter der sich Benutzer anmelden können, z.B. `http://localhost:12345`.
    <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Geben Sie für native Anwendungen einen **Umleitungs-URI** an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.
* Nach Abschluss der Registrierung weist Azure AD Ihrer Anwendung eine eindeutige Client-ID zu, die **Anwendungs-ID**. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Anwendungsseite kopieren.
* Um die Anwendung im Azure-Portal zu suchen, klicken Sie auf **App Registrierungen** und dann auf **Alle Anwendungen anzeigen**.

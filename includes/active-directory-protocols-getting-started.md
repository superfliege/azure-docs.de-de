---
title: "Übersicht über das Azure AD .NET Protokoll | Microsoft Docs"
description: Verwendung von HTTP-Nachrichten beim Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten, die mithilfe von Azure AD.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 42ce8dfd30cda7d4085778954350550fd9fdf13d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## Registrieren Sie Ihre Anwendung mit dem AD-Mandanten
Zunächst müssen Sie Ihre Anwendung mit Azure Active Directory (Azure AD)-Mandanten zu registrieren. Dies wird bieten Ihnen eine Anwendungs-ID für Ihre Anwendung sowie zum Empfangen von Token zu aktivieren.

* Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
* Wählen Sie die Azure AD-Mandanten durch Klicken auf Ihr Konto in der oberen rechten Ecke der Seite.
* Klicken Sie im linken Navigationsbereich auf **Azure Active Directory**.
* Klicken Sie auf **App Registrierungen** , und klicken Sie auf **hinzufügen**.
* Befolgen Sie die Anweisungen aus, und erstellen Sie eine neue Anwendung. Keine Rolle spielt, ob es sich um eine Webanwendung oder eine systemeigene Anwendung für dieses Lernprogramm ist, aber wenn Sie spezifische Beispiele für Webanwendungen oder systemeigenen Anwendungen möchten sehen Sie sich unsere [Schnellstarts](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Geben Sie für Webanwendungen, die **Anmelde-URL** ist die Basis-URL Ihrer App, wo können Benutzer z. B. Anmeldung `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Geben Sie für systemeigene Anwendungen eine **Umleitungs-URI**, die von Azure AD verwendet werden, token Antworten zurückgeben. Geben Sie einen bestimmten Wert für Ihre Anwendung. z. B.`http://MyFirstAADApp`
* Sobald Sie die Registrierung abgeschlossen haben, wird Azure AD Ihrer Anwendung zuweisen eine eindeutige Client-ID, der die Anwendung-ID Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie es aus der Seite "Anwendung".

---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5940195207f85d8011f61336c0318e456c2a8a4c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214278"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Konfigurieren Ihrer ASP.NET-Web-App mit den-Registrierungsinformationen der Anwendung

In diesem Schritt konfigurieren Sie Ihr Projekt für das Verwenden von SSL. Anschließend verwenden Sie die SSL-URL zum Konfigurieren der Registrierungsinformationen Ihrer Anwendung. Anschließend fügen Sie die Registrierungsinformationen Ihrer Anwendung der Projektmappe über *web.config* hinzu.

1. Wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie F4).
2. Ändern Sie `SSL Enabled` in `True`.
3. Kopieren Sie den obigen Wert von `SSL URL`, und fügen Sie ihn oben auf dieser Seite in das Feld `Redirect URL` ein. Klicken Sie dann auf *Aktualisieren*:<br/><br/>![Projekteigenschaften](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4. Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```

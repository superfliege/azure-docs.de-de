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
ms.date: 09/17/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: b18b6e2fd5bb2071f57033dbe74ef87da028720b
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843137"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Konfigurieren Ihrer ASP.NET-Web-App mit den-Registrierungsinformationen der Anwendung

In diesem Schritt konfigurieren Sie Ihr Projekt für das Verwenden von SSL. Anschließend verwenden Sie die SSL-URL zum Konfigurieren der Registrierungsinformationen Ihrer Anwendung. Anschließend fügen Sie die Registrierungsinformationen Ihrer Anwendung der Projektmappe über *web.config* hinzu.

1.  Wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie F4).
2.  Ändern Sie `SSL Enabled` in `True`.
3.  Kopieren Sie den obigen Wert von `SSL URL`, und fügen Sie ihn oben auf dieser Seite in das Feld `Redirect URL` ein. Klicken Sie dann auf *Aktualisieren*:<br/><br/>![Projekteigenschaften](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4.  Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```

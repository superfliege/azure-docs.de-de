---
title: Azure AD-v2-ASP.NET Web-Server-Erste Schritte - Konfiguration | Microsoft Docs
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
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun Sie zum Registrieren Ihrer Anwendung in müssen der *Microsoft App-Registrierungsportal*:
1. Registrieren Sie Ihre Anwendung über die [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Geben Sie einen Namen für die Anwendung und Ihre e-Mail-Adresse
3.  Stellen Sie sicher, dass die Option für das Setup geführtes aktiviert ist
4.  Führen Sie die Anweisungen, um eine Umleitungs-URL zu einer Anwendung hinzufügen

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Fügen Sie Ihre Anwendung Registrierungsinformationen zur Projektmappe (Erweitert)
Nun Sie zum Registrieren Ihrer Anwendung in müssen der *Microsoft App-Registrierungsportal*:
1. Wechseln Sie zu der [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app) zum Registrieren einer Anwendung
2. Geben Sie einen Namen für die Anwendung und Ihre e-Mail-Adresse 
3.  Stellen Sie sicher, dass die Option für das Setup geführtes deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, und wählen Sie dann`Web`
5.  Wechseln Sie zurück zu Visual Studio und im Projektmappen-Explorer, wählen Sie das Projekt, und betrachten Sie das Eigenschaftenfenster (Wenn Sie ein Eigenschaftenfenster drücken Sie F4 sehen)
6.  Änderung SSL aktiviert`True`
7.  Kopieren Sie die SSL-URL ein, und fügen Sie diese URL zur Liste der Umleitungs-URLs in das Registrierungsportal Liste der Umleitungs-URLs hinzu:<br/><br/>![Projekteigenschaften](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Fügen Sie die folgenden in `web.config` im Ordner "Root" unter dem Abschnitt `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Ersetzen Sie `ClientId` mit Anwendungs-Id, die Sie gerade registriert haben.
</li>
<li>
Ersetzen Sie `redirectUri` mit dem SSL-URL des Projekts
</li>
</ol>
<!-- End Docs -->
